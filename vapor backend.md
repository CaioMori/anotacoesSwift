## Construindo um Serviço Web com Vapor

O código-fonte deste guia pode ser encontrado no GitHub.

### Instalando o Swift

Para iniciar sua jornada, instale o Swift para utilizá-lo em macOS, Linux ou Windows.

**Dica:** Para verificar se o Swift está instalado, execute `swift --version` no seu terminal.

O Swift vem acompanhado do Swift Package Manager (SwiftPM), que gerencia a distribuição de código Swift. Ele permite a importação fácil de outros pacotes Swift em suas aplicações e bibliotecas, tornando-se uma ferramenta valiosa para qualquer desenvolvedor Swift.

O Swift é coberto pela Licença Apache, Versão 2.0.

### Escolhendo um Framework Web

Ao longo dos anos, a comunidade Swift criou diversos frameworks web projetados para facilitar a construção de serviços web. Este guia foca no framework Vapor, uma escolha popular dentro da comunidade.

### Instalando o Vapor

Primeiro, você precisa instalar a ferramenta Vapor. Se você já tem o Homebrew instalado no macOS, execute:

```bash
brew install vapor
```

Se você estiver utilizando outro sistema operacional ou quiser instalar a ferramenta a partir do código-fonte, consulte a documentação do Vapor para instruções.

### Criando um Projeto

Em seguida, no seu terminal, em um diretório onde deseja criar o novo projeto, execute:

```bash
vapor new HelloVapor
```

Isso baixará um template e fará uma série de perguntas para criar um projeto simples com tudo que você precisa para começar. Este guia criará uma API REST simples que permite enviar e receber JSON. Portanto, responda "não" a todas as outras perguntas. Você verá que o projeto foi criado com sucesso.

#### Um Novo Projeto Vapor

Navegue até o diretório criado e abra o projeto em seu IDE de escolha. Por exemplo, para usar o VSCode, execute:

```bash
cd HelloVapor
code .
```

Para o Xcode, execute:

```bash
cd HelloVapor
open Package.swift
```

O template do Vapor contém vários arquivos e funções já configurados para você. O arquivo `configure.swift` contém o código para configurar sua aplicação e `routes.swift` contém o código dos manipuladores de rotas.

### Criando Rotas

Primeiro, abra `routes.swift` e crie uma nova rota para cumprimentar quem acessar seu site, declarando uma nova rota abaixo de `app.get("hello") { ... }`:

```swift
// 1
app.get("hello", ":name") { req async throws -> String in
    // 2
    let name = try req.parameters.require("name")
    // 3
    return "Hello, \(name.capitalized)!"
}
```

Aqui está o que o código faz:

- Declara um novo manipulador de rota registrado como uma requisição GET para `/hello/<NAME>`. O `:` denota um parâmetro de caminho dinâmico no Vapor e corresponderá a qualquer valor, permitindo que você o recupere no manipulador da rota. `app.get(...)` aceita uma closure como último parâmetro que pode ser assíncrona e deve retornar um `Response` ou algo que conforme com `ResponseEncodable`, como uma `String`.
  
- Obtém o nome dos parâmetros. Por padrão, isso retorna uma `String`. Se você quiser extrair outro tipo, como `Int` ou `UUID`, pode escrever `req.parameters.require("id", as: UUID.self)` e o Vapor tentará convertê-lo para o tipo desejado, lançando automaticamente um erro se não conseguir. Isso lança um erro se a rota não tiver sido registrada com o nome de parâmetro correto.
  
- Retorna a resposta; neste caso, uma `String`. Note que você não precisa definir um código de status, corpo da resposta ou cabeçalhos. O Vapor cuida disso tudo para você, permitindo que você controle a resposta retornada se necessário.

Salve o arquivo e construa e execute a aplicação:

```bash
$ swift run
```

Você verá:

```
Building for debugging...
...
Build complete! (59.87s)
[ NOTICE ] Server starting on http://127.0.0.1:8080
```

Envie uma requisição GET para `http://localhost:8080/hello/tim`. Você receberá a resposta:

```bash
$ curl http://localhost:8080/hello/tim
Hello, Tim!
```

Experimente com diferentes nomes para ver a resposta mudar automaticamente!

### Retornando JSON

O Vapor utiliza *Codable* internamente para facilitar o envio e recebimento de JSON, usando um protocolo wrapper chamado *Content* para adicionar alguns recursos extras. A seguir, você retornará um corpo JSON com a mensagem da rota "Hello!". Primeiro, crie um novo tipo na parte inferior de `routes.swift`:

```swift
struct UserResponse: Content {
    let message: String
}
```

Isso define um novo tipo que conforma ao *Content*, correspondendo ao JSON que você deseja retornar.

Crie uma nova rota abaixo de `app.get("hello", ":name") { ... }` para retornar este JSON:

```swift
// 1
app.get("json", ":name") { req async throws -> UserResponse in
    // 2
    let name = try req.parameters.require("name")
    let message = "Hello, \(name.capitalized)!"
    // 3
    return UserResponse(message: message)
}
```

Aqui está o que este código faz:

- Define um novo manipulador de rota que lida com uma requisição GET para `/json`. Importante notar que o tipo de retorno da closure é `UserResponse`.
  
- Obtém o nome como antes e constrói a mensagem.
  
- Retorna o `UserResponse`.

Salve e construa novamente sua aplicação e envie uma requisição GET para `http://localhost:8080/json/tim`:

```bash
$ curl http://localhost:8080/json/tim
{"message":"Hello, Tim!"}
```

Desta vez, você recebe JSON!

### Manipulando JSON

Por fim, abordaremos como receber JSON. Na parte inferior de `routes.swift`, crie um novo tipo para modelar o JSON que você enviará ao servidor:

```swift
struct UserInfo: Content {
    let name: String
    let age: Int
}
```

Isso contém duas propriedades: um nome e uma idade. Em seguida, abaixo da rota JSON, crie uma nova rota para lidar com uma requisição POST com este corpo:

```swift
// 1
app.post("user-info") { req async throws -> UserResponse in
    // 2
    let userInfo = try req.content.decode(UserInfo.self)
    // 3
    let message = "Hello, \(userInfo.name.capitalized)! You are \(userInfo.age) years old."
    return UserResponse(message: message)
}
```

As diferenças importantes neste novo manipulador de rota são:

- Use `app.post(...)` em vez de `app.get(...)`, pois este manipulador de rota é uma requisição POST.
  
- Decodifique o JSON do corpo da requisição.
  
- Use os dados do corpo JSON para criar uma nova mensagem.

Envie uma requisição POST com um corpo JSON válido e veja sua resposta:

```bash
$ curl http://localhost:8080/user-info -X POST -d '{"name": "Tim", "age": 99}' -H "Content-Type: application/json"
{"message":"Hello, Tim! You are 99 years old."}
```

**Parabéns! Você construiu seu primeiro servidor web em Swift!**

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/41735214/51b77616-9c85-41c6-b76c-75bbbca08d67/paste.txt
