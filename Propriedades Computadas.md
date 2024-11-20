## Propriedades Computadas

Propriedades computadas não armazenam um valor diretamente. Em vez disso, elas fornecem um getter e, opcionalmente, um setter para recuperar e definir outros valores indiretamente.

### Sintaxe Básica

```swift
struct Circle {
    var radius: Double
    
    var area: Double {
        get {
            return Double.pi * radius * radius
        }
        set(newArea) {
            radius = sqrt(newArea / Double.pi)
        }
    }
}
```

Neste exemplo, `area` é uma propriedade computada que calcula a área do círculo baseada no raio.

### Getters e Setters

#### Getter

O getter é obrigatório em uma propriedade computada. Ele é chamado quando o valor da propriedade é lido.

```swift
var perimeter: Double {
    get {
        return 2 * Double.pi * radius
    }
}
```

#### Setter

O setter é opcional. Ele é chamado quando um novo valor é atribuído à propriedade.

```swift
var diameter: Double {
    get {
        return 2 * radius
    }
    set {
        radius = newValue / 2
    }
}
```

Observe que `newValue` é um nome padrão para o valor passado ao setter. Você pode especificar um nome personalizado entre parênteses após `set`, como visto no exemplo da `area`.

### Propriedades Computadas Somente Leitura

Se uma propriedade computada possui apenas um getter, ela é considerada somente leitura. Nesse caso, a palavra-chave `get` pode ser omitida:

```swift
var volume: Double {
    4/3 * Double.pi * pow(radius, 3)
}
```

## Propriedades Estáticas

Propriedades estáticas pertencem ao tipo em si, não a instâncias desse tipo. Elas são compartilhadas por todas as instâncias.

### Sintaxe

```swift
struct MathConstants {
    static let pi = 3.14159
    static var e = 2.71828
}
```

Para acessar propriedades estáticas, use o nome do tipo seguido pela propriedade:

```swift
let piValue = MathConstants.pi
```

### Propriedades Estáticas Computadas

Propriedades estáticas também podem ser computadas:

```swift
struct TemperatureConverter {
    static var celsiusToFahrenheitFactor: Double {
        return 9.0 / 5.0
    }
    
    static func celsiusToFahrenheit(_ celsius: Double) -> Double {
        return celsius * celsiusToFahrenheitFactor + 32
    }
}
```

## Possíveis Erros e Cuidados

1. **Recursão Infinita**: Cuidado ao acessar a propriedade computada dentro de seu próprio getter ou setter.

   ```swift
   var badProperty: Int {
       get { return self.badProperty } // Erro: recursão infinita
   }
   ```

2. **Efeitos Colaterais**: Evite efeitos colaterais em getters, pois podem levar a comportamentos inesperados.

3. **Desempenho**: Propriedades computadas são recalculadas cada vez que são acessadas. Para cálculos intensivos, considere usar lazy properties ou caching.

4. **Mutabilidade**: Propriedades computadas em structs não podem modificar outras propriedades da struct a menos que a função seja marcada como `mutating`.

## Melhores Práticas

1. **Use Propriedades Computadas para Lógica Simples**: Elas são ideais para cálculos diretos ou transformações de dados.

2. **Prefira Propriedades Armazenadas para Dados Simples**: Se não há lógica envolvida, use uma propriedade armazenada.

3. **Considere o Uso de Lazy Properties**: Para cálculos pesados que não mudam, use `lazy var` para calcular apenas uma vez.

4. **Mantenha Getters Puros**: Evite efeitos colaterais em getters para manter o código previsível.

5. **Use Propriedades Estáticas para Constantes Globais**: Elas oferecem um namespace limpo e evitam poluição do escopo global.

6. **Documente Propriedades Computadas Complexas**: Use comentários para explicar a lógica por trás de cálculos não triviais.

## Exemplo Avançado

Vamos combinar vários conceitos em um exemplo mais complexo:

```swift
struct TemperatureSensor {
    private var _celsius: Double = 0
    
    var celsius: Double {
        get { _celsius }
        set { _celsius = newValue }
    }
    
    var fahrenheit: Double {
        get { celsius * 9/5 + 32 }
        set { celsius = (newValue - 32) * 5/9 }
    }
    
    static var defaultCalibration: Double {
        get { UserDefaults.standard.double(forKey: "TempCalibration") }
        set { UserDefaults.standard.set(newValue, forKey: "TempCalibration") }
    }
    
    static func calibrate(sensor: inout TemperatureSensor) {
        sensor.celsius += defaultCalibration
    }
}
```

Neste exemplo:
- Usamos uma propriedade privada `_celsius` para armazenamento interno.
- Implementamos propriedades computadas `celsius` e `fahrenheit` com getters e setters.
- Criamos uma propriedade estática computada `defaultCalibration` que interage com `UserDefaults`.
- Definimos uma função estática `calibrate` que modifica uma instância do sensor.

## Conclusão

Propriedades computadas e estáticas são ferramentas poderosas no arsenal de um desenvolvedor Swift. Quando usadas corretamente, elas podem tornar o código mais limpo, mais eficiente e mais expressivo. Ao seguir as melhores práticas e estar ciente dos possíveis pitfalls, você pode aproveitar ao máximo esses recursos para criar código Swift robusto e elegante.
