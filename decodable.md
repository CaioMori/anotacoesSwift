# Entendendo o Protocolo `Decodable` no Swift

O protocolo `Decodable` no Swift desempenha um papel crucial na decodificação de dados estruturados em objetos de uma aplicação. Ele faz parte do framework **Codable**, introduzido na linguagem Swift a partir da versão 4, que combina os protocolos `Encodable` e `Decodable` para permitir que objetos sejam facilmente codificados e decodificados.

## O que é `Decodable`?

`Decodable` é um protocolo que permite transformar dados em um formato como JSON, Property List (plist) ou outro tipo de representação serializada em instâncias de tipos Swift. Quando um tipo é declarado como conforme ao protocolo `Decodable`, ele pode ser convertido diretamente de dados codificados.

A ideia principal é que, ao utilizar o `Decodable`, você pode:

- Receber um JSON de uma API.
- Transformar os dados JSON em tipos fortemente tipados.
- Validar e lidar com dados de forma segura.

## Implementação do Protocolo

### Exemplo prático de `Decodable`
Considere o seguinte JSON:

```json
{
  "id": 101,
  "name": "John Doe",
  "email": "johndoe@example.com"
}
```

Para decodificar este JSON em um objeto Swift, começamos criando uma estrutura que corresponde à sua estrutura:

```swift
struct User: Decodable {
    let id: Int
    let name: String
    let email: String
}
```

### Decodificando JSON
Agora, vamos utilizar o protocolo `Decodable` para transformar o JSON em uma instância de `User`.

```swift
import Foundation

let json = """
{
    "id": 101,
    "name": "John Doe",
    "email": "johndoe@example.com"
}
""".data(using: .utf8)!

// Decodificador JSON
let decoder = JSONDecoder()

do {
    let user = try decoder.decode(User.self, from: json)
    print("Usuário: \(user.name), Email: \(user.email)")
} catch {
    print("Erro ao decodificar JSON: \(error)")
}
```

### Resultado Esperado
O exemplo acima produz a seguinte saída:

```
Usuário: John Doe, Email: johndoe@example.com
```

## Personalizando a Decodificação com `CodingKeys`
Em casos em que as chaves no JSON não correspondem aos nomes das propriedades em seu tipo Swift, você pode utilizar o enum `CodingKeys` para mapear as chaves do JSON para suas propriedades.

### Exemplo com `CodingKeys`
Considere o seguinte JSON:

```json
{
  "user_id": 101,
  "full_name": "John Doe",
  "user_email": "johndoe@example.com"
}
```

Para mapear as chaves do JSON para as propriedades de um tipo Swift, utilize o `CodingKeys`:

```swift
struct User: Decodable {
    let id: Int
    let name: String
    let email: String

    private enum CodingKeys: String, CodingKey {
        case id = "user_id"
        case name = "full_name"
        case email = "user_email"
    }
}
```

### Decodificação com `CodingKeys`

```swift
let json = """
{
    "user_id": 101,
    "full_name": "John Doe",
    "user_email": "johndoe@example.com"
}
""".data(using: .utf8)!

do {
    let user = try decoder.decode(User.self, from: json)
    print("Usuário: \(user.name), Email: \(user.email)")
} catch {
    print("Erro ao decodificar JSON: \(error)")
}
```

A saída será a mesma:

```
Usuário: John Doe, Email: johndoe@example.com
```

## Trabalhando com Tipos Aninhados
O `Decodable` também suporta estruturas JSON aninhadas. Considere o seguinte exemplo:

```json
{
  "id": 101,
  "name": "John Doe",
  "contact": {
    "email": "johndoe@example.com",
    "phone": "123-456-7890"
  }
}
```

Aqui, o JSON inclui um objeto aninhado dentro da chave `contact`. Você pode modelar isso com structs aninhadas:

```swift
struct User: Decodable {
    let id: Int
    let name: String
    let contact: Contact

    struct Contact: Decodable {
        let email: String
        let phone: String
    }
}
```

A decodificação segue o mesmo processo:

```swift
do {
    let user = try decoder.decode(User.self, from: json)
    print("Email: \(user.contact.email), Telefone: \(user.contact.phone)")
} catch {
    print("Erro ao decodificar JSON: \(error)")
}
```

## Conclusão
O protocolo `Decodable` simplifica drasticamente o trabalho com dados serializados no Swift. Com suporte para mapeamento de chaves personalizadas, estruturas aninhadas e validação de tipos, ele se torna uma ferramenta poderosa para trabalhar com APIs e outras fontes de dados estruturados. Ao entender como utilizá-lo, você ganha um controle maior sobre a manipulação de dados em sua aplicação, garantindo segurança e desempenho.

