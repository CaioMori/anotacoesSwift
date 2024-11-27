## Construindo uma Aplicação Embarcada para Microcontrolador

O código-fonte deste guia pode ser encontrado no GitHub.

Neste tutorial, vamos desenvolver para um Raspberry Pi Pico como o dispositivo embarcado que executará nossa aplicação Swift. Se você não possui um fisicamente, não se preocupe! Você ainda poderá executar a aplicação em um emulador online.

### Instalando o Swift

Se você ainda não tem o Swift instalado, instale-o primeiro. Como o Swift Embarcado é experimental e disponível apenas em toolchains de preview, certifique-se de instalar o toolchain "Development Snapshot" (main) em vez de um toolchain de lançamento (6.0).

Se você estiver usando uma máquina macOS, precisará garantir que o toolchain instalado esteja selecionado como ativo, por exemplo, exportando a variável de ambiente TOOLCHAINS:

```bash
export TOOLCHAINS=org.swift.59202405011a
```

Para testar se o Swift está instalado, execute `swift --version` no seu terminal. Deve mostrar "6.0-dev", indicando que você tem um toolchain "Development Snapshot".

### Instalando Dependências para Desenvolvimento Embarcado

Instale o SDK do Raspberry Pi Pico e o Arm Embedded Toolchain seguindo o guia "Getting Started With Pico".

Exporte três variáveis de ambiente para corresponder à sua configuração e hardware:

```bash
export PICO_BOARD=pico
export PICO_SDK_PATH=... # caminho para o seu Pico SDK
export PICO_TOOLCHAIN_PATH=... # caminho para o Arm Embedded Toolchain
```

Se você tem a placa Pico W habilitada para Wi-Fi em vez do Pico regular, observe que precisará de uma configuração ligeiramente diferente descrita no projeto de exemplo Pico W, e apenas especificar `PICO_BOARD=pico_w` não funcionará.

Instale o CMake 3.29 ou mais recente.

Para testar se você tem todas as partes necessárias instaladas, execute os seguintes comandos em um terminal:

```bash
swift --version
cmake --version
echo $PICO_BOARD
ls $PICO_SDK_PATH
ls $PICO_TOOLCHAIN_PATH
```

### Construindo uma Aplicação Embarcada "Blinky"

O "Hello, World" padrão no desenvolvimento embarcado é um programa que pisca repetidamente um LED. Vamos construir um.

Crie um novo diretório vazio e prepare uma estrutura simples para um projeto baseado em CMake que pode ser usado sobre o SDK do Pico:

```
embedded-swift-tutorial
├── BridgingHeader.h
├── CMakeLists.txt
└── Main.swift
```

Os arquivos `Main.swift` e `BridgingHeader.h` podem inicialmente ter o seguinte conteúdo básico:

```swift
// Main.swift
let led = UInt32(PICO_DEFAULT_LED_PIN)
gpio_init(led)
gpio_set_dir(led, /*out*/true)

while true {
    gpio_put(led, true)
    sleep_ms(250)
    gpio_put(led, false)
    sleep_ms(250)
}
```

```c
// BridgingHeader.h
#include "pico/stdlib.h"
```

Para construir sobre o suporte CMake do SDK do Pico, precisamos de um pouco mais de lógica CMake no arquivo `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.29)
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

set(CMAKE_Swift_COMPILATION_MODE wholemodule)
set(CMAKE_Swift_COMPILER_WORKS YES)

project(blinky)
pico_sdk_init()
enable_language(Swift)

add_executable(blinky Main.swift)
set_target_properties(blinky PROPERTIES LINKER_LANGUAGE CXX)

set_target_properties(pico_standard_link PROPERTIES INTERFACE_COMPILE_OPTIONS "")
target_compile_options(pico_standard_link INTERFACE "$<$<COMPILE_LANGUAGE:C>:SHELL: -ffunction-sections -fdata-sections>")

set(SWIFT_INCLUDES)
foreach(dir ${CMAKE_C_IMPLICIT_INCLUDE_DIRECTORIES})
    string(CONCAT SWIFT_INCLUDES ${SWIFT_INCLUDES} "-Xcc ")
    string(CONCAT SWIFT_INCLUDES ${SWIFT_INCLUDES} "-I${dir} ")
endforeach()

target_compile_options(blinky PUBLIC "$<$<COMPILE_LANGUAGE:Swift>:SHELL: -enable-experimental-feature Embedded -target armv6m-none-none-eabi -Xcc -mfloat-abi=soft -Xcc -fshort-enums -Xfrontend -function-sections -import-bridging-header ${CMAKE_CURRENT_LIST_DIR}/BridgingHeader.h ${SWIFT_INCLUDES} >")

target_link_libraries(blinky pico_stdlib hardware_uart hardware_gpio)
pico_add_extra_outputs(blinky)
```

Agora estamos prontos para configurar e construir este firmware para o Pico. Execute os seguintes comandos:

```bash
cmake -B build -G Ninja .  # etapa de configuração
cmake --build build  # etapa de construção
```

A construção deve ser bem-sucedida e produzir o firmware em vários formatos (ELF, HEX, UF2), incluindo alguns arquivos de informações de despejo (DIS, ELF.MAP).

### Executando o Firmware em um Dispositivo

Se você tem um Raspberry Pi Pico, agora vamos fazer o upload do firmware construído e executá-lo. Se você não tiver um, pule para a próxima seção e execute o mesmo arquivo de firmware em um emulador.

Conecte a placa Raspberry Pi Pico via cabo USB ao seu Mac e certifique-se de que ela esteja no modo de upload de firmware USB Mass Storage. Copie o arquivo UF2 para o volume montado:

```bash
cp build/blinky.uf2 /Volumes/RPI-RP2
```

Isso fará com que o Pico instale automaticamente o firmware, reinicie e execute o firmware. O LED verde deve agora estar piscando repetidamente.

### Executando o Firmware em um Emulador

Se você não tem um Pico físico, ou se quiser iterar rapidamente, o Wokwi é um emulador online gratuito de vários microcontroladores, incluindo o Raspberry Pi Pico.

Abra um novo projeto Pico no Wokwi. Em vez de usar o editor de código para escrever código C, pressione F1 e escolha "Upload Firmware and Start Simulation". Em seguida, selecione o arquivo UF2 que nosso processo de construção produziu.

Uma vez que você faz o upload do arquivo UF2 para o Wokwi, a simulação começará, e o LED deve começar a piscar repetidamente.

Parabéns! Seu primeiro programa Swift Embarcado está funcionando em um emulador!

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/41735214/444d2334-9500-44ce-81d3-7f04105f41dd/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/41735214/51b77616-9c85-41c6-b76c-75bbbca08d67/paste.txt
