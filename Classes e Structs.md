## O que são Classes e Structs

### Classes

Classes são tipos de referência em Swift, o que significa que múltiplas variáveis podem apontar para a mesma instância na memória. Elas suportam herança, permitem o uso de deinicializadores e possibilitam o type casting em tempo de execução.

Exemplo de uma classe em Swift:

```swift
class Veiculo {
    var marca: String
    var modelo: String
    
    init(marca: String, modelo: String) {
        self.marca = marca
        self.modelo = modelo
    }
    
    func descricao() -> String {
        return "\(marca) \(modelo)"
    }
}
```

### Structs

Structs são tipos de valor em Swift, o que significa que cada instância mantém uma cópia única dos dados. Elas são mais leves e eficientes em termos de performance, especialmente para pequenas estruturas de dados.

Exemplo de uma struct em Swift:

```swift
struct Ponto {
    var x: Double
    var y: Double
    
    func distancia(para outro: Ponto) -> Double {
        return sqrt(pow(x - outro.x, 2) + pow(y - outro.y, 2))
    }
}
```

## Diferenças Principais

1. **Semântica de Valor vs. Referência**: Structs são tipos de valor, enquanto classes são tipos de referência[1][2].

2. **Herança**: Classes suportam herança, structs não[1].

3. **Deinicialização**: Classes podem implementar deinicializadores (deinit), structs não[1].

4. **Identidade**: Classes têm identidade única, permitindo comparação com ===, structs não[1].

5. **Mutabilidade**: Structs promovem imutabilidade por padrão, classes são mutáveis por natureza[2].

## Quando Usar Classes

1. **Modelagem de Objetos Complexos**: Para representar entidades complexas com comportamentos e estados mutáveis.

```swift
class ViewController: UIViewController {
    var dataSource: [String] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Configuração adicional
    }
    
    func updateData(_ newData: [String]) {
        dataSource = newData
        // Atualizar UI
    }
}
```

2. **Compartilhamento de Estado**: Quando múltiplas partes do código precisam acessar e modificar o mesmo objeto.

3. **Interoperabilidade com Objective-C**: Para usar recursos como @objc e dynamic[1].

4. **Gerenciamento de Ciclo de Vida**: Quando é necessário controlar a inicialização e deinicialização de objetos.

## Quando Usar Structs

1. **Modelagem de Dados Simples**: Para representar tipos de dados simples e imutáveis.

```swift
struct Usuario {
    let id: Int
    let nome: String
    let email: String
}
```

2. **Performance**: Em cenários onde a criação e destruição frequente de objetos é necessária[1].

3. **Thread Safety**: Em ambientes multi-thread, structs são mais seguras por serem copiadas entre threads[1].

4. **Valor Semântico**: Quando a cópia de valores faz sentido para o modelo de dados.

## Dicas de Uso

1. **Prefira Structs**: Use structs por padrão e migre para classes quando necessário[1].

2. **Composição sobre Herança**: Utilize composição com structs para criar estruturas complexas sem recorrer à herança de classes.

3. **Imutabilidade**: Aproveite a imutabilidade natural das structs para criar código mais previsível e seguro.

4. **Performance**: Para tipos de dados pequenos e frequentemente utilizados, prefira structs para melhor performance[1].

5. **Modelagem de Domínio**: Use structs para modelar entidades de domínio simples e classes para objetos com comportamentos complexos ou estado compartilhado.

## Conclusão

A escolha entre classes e structs em Swift depende das necessidades específicas do seu projeto. Structs são ideais para tipos de dados simples, imutáveis e com semântica de valor, enquanto classes são mais adequadas para objetos complexos, com estado compartilhado e que requerem herança ou gerenciamento de ciclo de vida. Ao compreender as diferenças e aplicar corretamente cada tipo, você pode criar código mais eficiente, seguro e fácil de manter em seus projetos iOS.

Citations:
[1] https://www.appypie.com/struct-vs-class-swift-how-to
[2] https://www.dice.com/career-advice/swift-tutorial-structs-classes-beginners
[3] https://www.avanderlee.com/swift/struct-class-differences/
[4] https://www.linkedin.com/pulse/structs-vs-classes-swift-explained-hermine-militonyan-lxupf
[5] https://www.linkedin.com/pulse/when-use-structs-classes-actors-swift-5-practical-scenarios-haviv-tk1de
[6] https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes
