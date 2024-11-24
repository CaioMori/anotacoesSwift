## UIViewController

UIViewController é uma classe fundamental do framework UIKit da Apple. Ela gerencia uma hierarquia de views e coordena o fluxo de dados entre o modelo de dados de um aplicativo e as views que exibem esses dados[2]. No seu código, a classe QuestaoViewController herda de UIViewController, permitindo que ela controle uma view específica da interface do usuário.

## IBOutlet

IBOutlet é um decorador usado para criar uma conexão entre elementos da interface do usuário (definidos no Interface Builder) e propriedades no código Swift[1][2]. No seu código, temos dois exemplos:

```swift
@IBOutlet weak var questionTitleLabel: UILabel!
@IBOutlet var botoesResposta: [UIButton]!
```

Estes outlets permitem que você acesse e manipule a label de título da questão e os botões de resposta diretamente no código.

## IBAction

IBAction é um decorador usado para conectar eventos de interface do usuário (como toques em botões) a métodos no código Swift[1][2]. No seu código, temos:

```swift
@IBAction func respostaBotaoPressionado(_ sender: UIButton) {
    // ...
}
```

Este método é chamado quando um botão de resposta é pressionado.

## Timer

Timer é uma classe do Foundation framework que permite agendar a execução de código após um determinado intervalo de tempo[6][8]. No seu código:

```swift
Timer.scheduledTimer(timeInterval: 0.5, target: self, selector: #selector(generateQuestion), userInfo: nil, repeats: false)
```

Este timer agenda a execução do método generateQuestion após 0.5 segundos.

## viewDidLoad()

viewDidLoad() é um método do ciclo de vida de UIViewController que é chamado após a view do controlador ser carregada na memória[4]. É comumente usado para realizar configurações iniciais:

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    setLayout()
    generateQuestion()
}
```

## Métodos personalizados

### setLayout()

Este método configura a aparência da interface do usuário, ajustando propriedades como cornerRadius e numberOfLines.

### generateQuestion()

Este método é responsável por gerar e exibir uma nova questão, atualizando o texto da label de título e os títulos dos botões de resposta.

## Propriedades

- score: Armazena a pontuação do usuário.
- questionNumber: Mantém o controle do número da questão atual.

Este código implementa a lógica de um quiz, gerenciando a exibição de questões, a verificação de respostas e a atualização da pontuação. Ele utiliza conceitos fundamentais de desenvolvimento iOS com Swift e UIKit, demonstrando o uso eficaz de IBOutlets, IBActions, e manipulação de interface do usuário programaticamente.

Citations:
[1] https://reintech.io/blog/mastering-swifts-ibaction
[2] https://reintech.io/blog/how-to-use-swift-iboutlet
[3] https://www.geeksforgeeks.org/repeating-timers-in-swift/
[4] https://sarunw.com/posts/uiviewcontroller-in-swiftui/
[5] https://forums.swift.org/t/ib-outlet-and-action/27046
[6] https://www.swiftanytime.com/blog/ultimate-guide-on-timer-in-swift
[7] https://www.hackingwithswift.com/books/ios-swiftui/wrapping-a-uiviewcontroller-in-a-swiftui-view
[8] https://www.hackingwithswift.com/articles/117/the-ultimate-guide-to-timer
[9] https://stackoverflow.com/questions/75504381/what-is-the-difference-between-viewcontroller-and-uiviewcontroller
