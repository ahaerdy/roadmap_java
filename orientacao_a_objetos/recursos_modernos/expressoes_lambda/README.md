# Expressões Lambda em Java

As expressões lambda do Java são novas no Java 8. As expressões lambda do Java são o primeiro passo do Java em direção à programação funcional. Uma expressão lambda do Java é, portanto, uma função que pode ser criada sem pertencer a nenhuma classe. Uma expressão lambda do Java pode ser passada adiante como se fosse um objeto e executada sob demanda.

As expressões lambda do Java são comumente usadas para implementar ouvintes de eventos (event listeners) / callbacks simples, ou na programação funcional com a API Java Streams. As Expressões Lambda do Java também são frequentemente usadas em programação funcional no Java.

Se você prefere vídeo, eu tenho uma versão em vídeo deste tutorial nesta Playlist do YouTube de Expressões Lambda em Java. Aqui está o primeiro vídeo desta playlist:

Tutorial de Expressões Lambda em Java

## Lambdas do Java e a Interface de Método Único

A programação funcional é frequentemente utilizada para implementar ouvintes de eventos. Os ouvintes de eventos em Java são muitas vezes definidos como interfaces Java com um único método. Aqui está um exemplo de uma interface fictícia de método único:

```java
public interface StateChangeListener {
    // Método abstrato único que define a assinatura do evento de mudança de estado
    public void onStateChange(State oldState, State newState);
}

```

Esta interface Java define um único método que é chamado sempre que o estado muda (em tudo o que estiver sendo observado).

No Java 7, você teria que implementar essa interface para escutar as mudanças de estado. Imagine que você tem uma classe chamada `StateOwner` que pode registrar ouvintes de eventos de estado. Aqui está um exemplo:

```java
public class StateOwner {
    // Método que aceita uma implementação da interface StateChangeListener
    public void addStateListener(StateChangeListener listener) { ... }
}

```

No Java 7, você poderia adicionar um ouvinte de evento usando uma implementação de interface anônima, assim:

```java
StateOwner stateOwner = new StateOwner(); // Cria uma instância da classe proprietária do estado
stateOwner.addStateListener(new StateChangeListener() { // Instancia uma classe anônima que implementa a interface
    public void onStateChange(State oldState, State newState) {
        // faça algo com o estado antigo e novo.
    }
});

```

Primeiro, uma instância de `StateOwner` é criada. Em seguida, uma implementação anônima da interface `StateChangeListener` é adicionada como ouvinte na instância de `StateOwner`.

No Java 8, você pode adicionar um ouvinte de evento usando uma expressão lambda do Java, assim:

```java
StateOwner stateOwner = new StateOwner(); // Cria a instância normalmente
stateOwner.addStateListener(
    // Expressão lambda substituindo a classe anônima: recebe dois parâmetros e executa uma ação
    (oldState, newState) -> System.out.println("State changed")
);

```

A expressão lambda é esta parte:

```java
// Parâmetros à esquerda do operador '->' e o corpo da função à direita
(oldState, newState) -> System.out.println("State changed")

```

A expressão lambda é correspondida em relação ao tipo de parâmetro do método `addStateListener()`. Se a expressão lambda corresponder ao tipo de parâmetro (neste caso, a interface `StateChangeListener`), then a expressão lambda é transformada em uma função que implementa a mesma interface daquele parâmetro.

As expressões lambda do Java só podem ser usadas onde o tipo com o qual são correspondidas for uma interface de método único. No exemplo acima, uma expressão lambda é usada como parâmetro onde o tipo do parâmetro era a interface `StateChangeListener`. Esta interface possui apenas um único método. Assim, a expressão lambda é correspondida com sucesso em relação a essa interface.

## Correspondendo Lambdas a Interfaces

Uma interface de método único também é às vezes chamada de interface funcional. A correspondência de uma expressão lambda do Java com uma interface funcional é dividida nestas etapas:

1. A interface possui apenas um método abstrato (não implementado)?
2. Os parâmetros da expressão lambda correspondem aos parâmetros do método único?
3. O tipo de retorno da expressão lambda corresponde ao tipo de retorno do método único?

Se a resposta for sim para essas três perguntas, then a expressão lambda fornecida é correspondida com sucesso com a interface.

## Interfaces com Métodos Default e Static

A partir do Java 8, uma interface Java pode conter tanto métodos default quanto métodos static. Ambos os métodos default e static possuem uma implementação definida diretamente na declaração da interface. Isso significa que uma expressão lambda do Java pode implementar interfaces com mais de um método — desde que a interface possua apenas um único método não implementado (ou seja, abstrato).

Em outras palavras, uma interface ainda é uma interface funcional mesmo que contenha métodos default e static, desde que a interface contenha apenas um único método não implementado (abstrato). Aqui está uma versão em vídeo desta pequena seção:

Tutorial de Interfaces Funcionais e Expressões Lambda em Java

A seguinte interface pode ser implementada com uma expressão lambda:

```java
import java.io.IOException;
import java.io.OutputStream;

public interface MyInterface {

    // Único método abstrato (não implementado). Essencial para ser uma interface funcional.
    void printIt(String text);

    // Método default: possui uma implementação padrão e não impede a interface de ser funcional
    default public void printUtf8To(String text, OutputStream outputStream){
        try {
            outputStream.write(text.getBytes("UTF-8"));
        } catch (IOException e) {
            throw new RuntimeException("Error writing String as UTF-8 to OutputStream", e);
        }
    }

    // Método static: método utilitário da própria interface, também não afeta o status de interface funcional
    static void printItToSystemOut(String text){
        System.out.println(text);
    }
}

```

Apesar de esta interface conter 3 métodos, ela pode ser implementada por uma expressão lambda, porque apenas um dos métodos não está implementado. Aqui está como fica a implementação:

```java
// O compilador associa este lambda ao único método abstrato da MyInterface (printIt)
MyInterface myInterface = (String text) -> {
    System.out.print(text); // Corpo do método implementado
};

```

## Expressões Lambda vs. Implementações de Interface Anônimas

Embora as expressões lambda sejam próximas das implementações de interface anônimas, existem algumas diferenças que vale a pena notar.

A principal diferença é que uma implementação de interface anônima pode ter estado (variáveis de membro), enquanto uma expressão lambda não pode. Veja esta interface:

```java
public interface MyEventConsumer {
    // Método abstrato que consome um objeto de evento
    public void consume(Object event);
}

```

Esta interface pode ser implementada usando uma implementação de interface anônima, assim:

```java
MyEventConsumer consumer = new MyEventConsumer() {
    // Implementação padrão através de classe anônima
    public void consume(Object event){
        System.out.println(event.toString() + " consumed");
    }
};

```

Esta implementação anônima de `MyEventConsumer` pode ter seu próprio estado interno. Veja esta reformulação:

```java
MyEventConsumer myEventConsumer = new MyEventConsumer() {
    private int eventCount = 0; // Estado interno (variável de membro) permitido em classes anônimas

    public void consume(Object event) {
        // Modifica e utiliza o estado interno a cada chamada
        System.out.println(event.toString() + " consumed " + this.eventCount++ + " times.");
    }
};

```

Note como a implementação anônima de `MyEventConsumer` agora possui um campo chamado `eventCount`.

Uma expressão lambda não pode ter tais campos. Diz-se, portanto, que uma expressão lambda é stateless (sem estado).

## Inferência de Tipo em Lambdas

Antes do Java 8, você teria que especificar qual interface implementar ao criar implementações de interface anônimas. Aqui está o exemplo de implementação de interface anônima do início deste texto:

```java
// É necessário declarar explicitamente o tipo 'new StateChangeListener()'
stateOwner.addStateListener(new StateChangeListener() {
    public void onStateChange(State oldState, State newState) {
        // faça algo com o estado antigo e novo.
    }
});

```

Com expressões lambda, o tipo geralmente pode ser inferido a partir do código ao redor. Por exemplo, o tipo de interface do parâmetro pode ser inferido a partir da declaração do método `addStateListener()` (o único método na interface `StateChangeListener`). Isso é chamado de inferência de tipo. O compilador infere o tipo de um parâmetro procurando pelo tipo em outro lugar — neste caso, na definição do método. Aqui está o exemplo do início deste texto, mostrando que a interface `StateChangeListener` não é mencionada na expressão lambda:

```java
stateOwner.addStateListener(
    // O Java infere automaticamente que este lambda implementa StateChangeListener e quais são os tipos de oldState e newState
    (oldState, newState) -> System.out.println("State changed")
);

```

Na expressão lambda, os tipos dos parâmetros também podem frequentemente ser inferidos. No exemplo acima, o compilador pode inferir o tipo deles a partir da declaração do método `onStateChange()`. Assim, os tipos dos parâmetros `oldState` e `newState` são inferidos a partir da declaração do método `onStateChange()`.

## Parâmetros de Lambdas

Como as expressões lambda do Java são efetivamente apenas métodos, las expressões lambda podem receber parâmetros assim como os métodos. A parte `(oldState, newState)` da expressão lambda mostrada anteriormente especifica os parâmetros que a expressão lambda recebe. Esses parâmetros devem corresponder aos parâmetros do método na interface de método único. Neste caso, esses parâmetros devem corresponder aos parâmetros do método `onStateChange()` da interface `StateChangeListener`:

```java
// Assinatura original do método que dita as regras para a expressão lambda correspondente
public void onStateChange(State oldState, State newState);

```

No mínimo, o número de parâmetros na expressão lambda e no método deve corresponder.

Em segundo lugar, se você tiver especificado quaisquer tipos de parâmetros na expressão lambda, esses tipos também devem corresponder. Eu ainda não mostrei como colocar tipos nos parâmetros de uma expressão lambda (isso é mostrado mais adiante neste texto), mas em muitos casos você não precisa deles.

### Zero Parâmetros

Se o método com o qual você está correspondendo sua expressão lambda não receber parâmetros, você poderá escrever sua expressão lambda assim:

```java
// Parênteses vazios indicam explicitamente a ausência de parâmetros de entrada
() -> System.out.println("Zero parameter lambda");

```

Note que os parênteses não têm conteúdo entre eles. Isso serve para sinalizar que o lambda não recebe parâmetros.

### Um Parâmetro

Se o método com o qual você está correspondendo sua expressão lambda do Java receber um parâmetro, você poderá escrever la expressão lambda assim:

```java
// Um parâmetro declarado entre parênteses
(param) -> System.out.println("One parameter: " + param);

```

Note que o parâmetro é listado dentro dos parênteses.

Quando uma expressão lambda recebe um único parâmetro, você também pode omitir os parênteses, assim:

```java
// Parênteses omitidos, o que é permitido exclusivamente para lambdas de um único parâmetro
param -> System.out.println("One parameter: " + param);

```

### Múltiplos Parâmetros

Se o método com o qual você corresponde sua expressão lambda do Java receber múltiplos parâmetros, os parâmetros precisam ser listados dentro de parênteses. Aqui está como isso se parece no código Java:

```java
// Múltiplos parâmetros separados por vírgula; os parênteses são estritamente obrigatórios aqui
(p1, p2) -> System.out.println("Multiple parameters: " + p1 + ", " + p2);

```

Os parênteses só podem ser omitidos quando o método recebe um único parâmetro.

### Tipos de Parâmetros

Especificar tipos de parâmetros para uma expressão lambda pode às vezes ser necessário se o compilador não conseguir inferir os tipos de parâmetros a partir do método da interface funcional com o qual o lambda está correspondendo. Não se preocupe, o compilador avisará quando esse for o caso. Aqui está um exemplo de tipo de parâmetro em um lambda Java:

```java
// Tipo explicitamente declarado ('Car'); útil se o compilador precisar de ajuda com a inferência
(Car car) -> System.out.println("The car is: " + car.getName());

```

Como você pode ver, o tipo (`Car`) do parâmetro `car` é escrito na frente do próprio nome do parâmetro, exatamente como você faria ao declarar um parâmetro em um método em outro lugar, ou ao criar uma implementação anônima de uma interface.

### Tipos de Parâmetros var a partir do Java 11

A partir do Java 11, você pode usar a palavra-chave `var` como tipo de parâmetro. A palavra-chave `var` foi introduzida no Java 10 como inferência de tipo de variável local. A partir do Java 11, `var` também pode ser usado para tipos de parâmetros de lambdas. Aqui está um exemplo do uso da palavra-chave `var` do Java como tipos de parâmetros em uma expressão lambda:

```java
// Uso de 'var' a partir do Java 11 para manter a inferência local de tipos dentro do lambda
Function<String, String> toLowerCase = (var input) -> input.toLowerCase();

```

O tipo do parâmetro declarado com a palavra-chave `var` acima será inferido para o tipo `String`, porque a declaração de tipo da variável tem seu tipo genérico definido como `Function<String, String>`, o que significa que o tipo do parâmetro e o tipo de retorno da `Function` é `String`.

## Corpo da Função Lambda

O corpo de uma expressão lambda e, portanto, o corpo da função/método que ela representa, é especificado à direita do `->` na declaração do lambda. Aqui está um exemplo:

```java
// Sem chaves à direita: o corpo executa uma única instrução/expressão direta
(oldState, newState) -> System.out.println("State changed")

```

Se a sua expressão lambda precisar consistir em múltiplas linhas, você pode envolver o corpo da função lambda dentro das chaves `{ }`, que o Java também exige ao declarar métodos em outros lugares. Aqui está um exemplo:

```java
// Uso de chaves '{ }' obrigatório para agrupar múltiplas instruções no corpo do lambda
(oldState, newState) -> {
    System.out.println("Old state: " + oldState);
    System.out.println("New state: " + newState);
};

```

## Retornando um Valor de uma Expressão Lambda

Você pode retornar valores de expressões lambda do Java, assim como faria em um método. Basta adicionar uma instrução `return` ao corpo da função lambda, assim:

```java
(param) -> {
    System.out.println("param: " + param);
    return "return value"; // Uso explícito da palavra-chave 'return' dentro de um bloco com chaves
};

```

Caso tudo o que sua expressão lambda esteja fazendo seja calcular um valor de retorno e retorná-lo, você pode especificar o valor de retorno de uma maneira mais curta. Em vez disso:

```java
// Sintaxe mais longa com bloco de chaves e retorno explícito
(a1, a2) -> { return a1 > a2; }

```

Você pode escrever:

```java
// Sintaxe curta: a própria expressão avaliada é o valor de retorno implícito (sem chaves, sem 'return')
(a1, a2) -> a1 > a2;

```

O compilador então descobre que a expressão `a1 > a2` é o valor de retorno da expressão lambda (daí o nome expressões lambda — já que expressões retornam um valor de algum tipo).

## Lambdas como Objetos

Uma expressão lambda do Java é essencialmente um objeto. Você pode atribuir uma expressão lambda a uma variável e passá-la adiante, como faz com qualquer outro objeto. Aqui está um exemplo:

```java
public interface MyComparator {
    // Método que recebe dois inteiros e retorna um booleano para comparação
    public boolean compare(int a1, int a2);
}

```

```java
// Atribuindo a expressão lambda a uma variável do tipo da interface funcional
MyComparator myComparator = (a1, a2) -> a1 > a2;
// Invocando o lambda exatamente como se fosse um método de um objeto convencional
boolean result = myComparator.compare(2, 5);

```

O primeiro bloco de código mostra a interface que a expressão lambda implementa. O segundo bloco de código mostra a definição da expressão lambda, como ela é atribuída a uma variável e, finalmente, como a expressão lambda é invocada ao chamar o método da interface que ela implementa.

## Captura de Variáveis (Variable Capture)

Uma expressão lambda do Java é capaz de acessar variáveis declaradas fora do corpo da função lambda sob certas circunstâncias. Eu tenho uma versão em vídeo desta seção aqui:

Tutorial de Captura de Variáveis em Expressões Lambda em Java

Os lambdas do Java podem capturar os seguintes tipos de variáveis:

* Variáveis locais
* Variáveis de instância
* Variáveis estáticas

Cada uma dessas capturas de variáveis será descrita nas seções seguintes.

### Captura de Variável Local

Um lambda do Java pode capturar o valor de uma variável local declarada fora do corpo do lambda. Para ilustrar isso, primeiro veja esta interface de método único:

```java
public interface MyFactory {
    // Método abstrato para fabricação de Strings a partir de um array de chars
    public String create(char[] chars);
}

```

Agora, veja esta expressão lambda que implementa a interface `MyFactory`:

```java
MyFactory myFactory = (chars) -> {
    return new String(chars); // Apenas transforma e retorna o parâmetro recebido
};

```

No momento, esta expressão lambda está apenas referenciando o valor do parâmetro passado para ela (`chars`). Mas podemos mudar isso. Aqui está uma versão atualizada que referencia uma variável `String` declarada fora do corpo da função lambda:

```java
String myString = "Test"; // Variável local que deve ser "efetivamente final" (não alterada após isso)
MyFactory myFactory = (chars) -> {
    // Captura e utiliza a variável local 'myString' de dentro do escopo do lambda
    return myString + ":" + new String(chars);
};

```

Como você pode ver, o corpo do lambda agora referencia a variável local `myString` que está declarada fora do corpo do lambda. Isso é possível se, e somente se, a variável referenciada for "efetivamente final" (effectively final), significando que ela não muda seu valor após ser atribuída. Se a variável `myString` tivesse seu valor alterado mais tarde, o compilador reclamaria da referência a ela de dentro do corpo do lambda.

### Captura de Variável de Instância

Uma expressão lambda também pode capturar uma variável de instância no objeto que cria o lambda. Aqui está um exemplo que mostra isso:

```java
public class EventConsumerImpl {
    private String name = "MyConsumer"; // Variável de instância da classe envolvente

    public void attach(MyEventProducer eventProducer){
        eventProducer.listen(e -> {
            // 'this' refere-se ao objeto EventConsumerImpl, capturando sua variável de instância 'name'
            System.out.println(this.name);
        });
    }
}

```

Note a referência a `this.name` dentro do corpo do lambda. Isso captura a variável de instância `name` do objeto `EventConsumerImpl` envolvente. É até possível alterar o valor da variável de instância após sua captura — e o valor será refletido dentro do lambda.

A semântica disso é, na verdade, uma das áreas onde os lambdas do Java diferem das implementações anônimas de interfaces. Uma implementação de interface anônima pode ter suas próprias variáveis de instância, que são referenciadas através da referência `this`. No entanto, um lambda não pode ter suas próprias variáveis de instância, então `this` sempre aponta para o objeto envolvente.

Nota: O design acima de um consumidor de eventos não é particularmente elegante. Eu apenas o fez assim para ser capaz de ilustrar a captura de variáveis de instância.

### Captura de Variável Estática

Uma expressão lambda do Java também pode capturar variáveis estáticas. Isso não é surpreendente, já que variáveis estáticas são acessíveis de qualquer lugar em uma aplicação Java, desde que a variável estática esteja acessível (escopo de pacote ou pública).

Aqui está uma classe de exemplo que cria um lambda que referencia uma variável estática de dentro do corpo do lambda:

```java
public class EventConsumerImpl {
    private static String someStaticVar = "Some text"; // Variável estática (escopo da classe)

    public void attach(MyEventProducer eventProducer){
        eventProducer.listen(e -> {
            // Captura e acessa a variável estática diretamente
            System.out.println(someStaticVar);
        });
    }
}

```

O valor de uma variável estática também tem permissão para mudar depois que o lambda a capturou.

Novamente, o design da classe acima é um pouco sem sentido. Não pense muito sobre isso. A classe serve principalmente para mostrar que um lambda pode acessar variáveis estáticas.

## Referências de Método como Lambdas (Method References)

No caso em que tudo o que sua expressão lambda faz é chamar outro método com os parâmetros passados para o lambda, a implementação de lambda do Java fornece uma maneira mais curta de expressar a chamada do método. Primeiro, aqui está um exemplo de interface de função única:

```java
public interface MyPrinter{
    // Interface funcional que recebe uma String e não retorna nada
    public void print(String s);
}

```

E aqui está um exemplo de criação de uma instância de lambda Java implementando a interface `MyPrinter`:

```java
MyPrinter myPrinter = (s) -> {
    System.out.println(s); // Implementação padrão com chaves
};

```

Como o corpo do lambda consiste em apenas uma única instrução, nós podemos omitir as chaves `{ }` envolventes. Além disso, como há apenas um parâmetro para o método lambda, podemos omitir os parênteses `( )` envolventes ao redor do parâmetro. Aqui está como fica a declaração do lambda resultante:

```java
// Sintaxe otimizada sem parênteses no parâmetro único e sem chaves na instrução única
MyPrinter myPrinter = s -> System.out.println(s);

```

Como tudo o que o corpo do lambda faz é encaminhar o parâmetro de string para o método `System.out.println()`, nós podemos substituir a declaração de lambda acima por uma referência de método. Aqui está como se parece uma referência de método lambda:

```java
// Referência de método: substitui completamente o lambda 's -> System.out.println(s)'
MyPrinter myPrinter = System.out::println;

```

Note os dois pontos duplos `::`. Eles sinalizam ao compilador Java que se trata de uma referência de método. O método referenciado é o que vem após os dois pontos duplos. Qualquer classe ou objeto proprietário do método referenciado vem antes dos dois pontos duplos.

Você pode referenciar os seguintes tipos de métodos:

* Método estático
* Método de instância em objetos de parâmetros
* Método de instância
* Construtor

Cada um desses tipos de referências de método é abordado nas seções a seguir.

### Referências de Método Estático

Os métodos mais fáceis de referenciar são os métodos estáticos. Aqui está primeiro um exemplo de uma interface de função única:

```java
public interface Finder {
    // Método abstrato que procura uma substring em outra string e retorna a posição
    public int find(String s1, String s2);
}

```

E aqui está uma classe com um método estático para o qual queremos criar uma referência de método:

```java
public class MyClass{
    // Método estático com assinatura compatível com o método find da interface Finder
    public static int doFind(String s1, String s2){
        return s1.lastIndexOf(s2);
    }
}

```

E finalmente aqui está uma expressão lambda do Java referenciando o método estático:

```java
// Referenciando um método estático através do nome da classe :: nome do método
Finder finder = MyClass::doFind;

```

Como os parâmetros dos métodos `Finder.find()` e `MyClass.doFind()` correspondem, é possível criar uma expressão lambda que implementa `Finder.find()` e referencia o método `MyClass.doFind()`.

### Referência de Método de Parâmetro

Você também pode referenciar um método de um dos parâmetros para o lambda. Imagine uma interface de função única parecida com esta:

```java
public interface Finder {
    public int find(String s1, String s2);
}

```

A interface destina-se a representar um componente capaz de pesquisar em `s1` por ocorrências de `s2`. Aqui está um exemplo de uma expressão lambda do Java que chama `String.indexOf()` para pesquisar:

```java
// O primeiro parâmetro (s1) vira o alvo do método, e o segundo (s2) vira o argumento: s1.indexOf(s2)
Finder finder = String::indexOf;

```

Isso é equivalente a esta definição de lambda:

```java
// Equivalente explícito da referência de método por parâmetro acima
Finder finder = (s1, s2) -> s1.indexOf(s2);

```

Note como a versão de atalho referencia um único método. O compilador Java tentará corresponder o método referenciado com o tipo do primeiro parâmetro, usando o tipo do segundo parâmetro como parâmetro para o método referenciado.

### Referências de Método de Instância

Em terceiro lugar, também é possível referenciar um método de instância a partir de uma definição de lambda. Primeiro, olhe para uma definição de interface de método único:

```java
public interface Deserializer {
    // Método abstrato para converter String para tipo numérico inteiro
    public int deserialize(String v1);
}

```

Esta interface representa a component que é capaz de "desserializar" uma `String` em um `int`.

Agora olhe para esta classe `StringConverter`:

```java
public class StringConverter {
    // Método de instância que realiza uma conversão similar à esperada pela interface
    public int convertToInt(String v1){
        return Integer.valueOf(v1);
    }
}

```

O método `convertToInt()` tem a mesma assinatura que o método `deserialize()` da interface `Deserializer`. Por causa disso, podemos criar uma instância de `StringConverter` e referenciar seu método `convertToInt()` a partir de uma expressão lambda do Java, assim:

```java
StringConverter stringConverter = new StringConverter(); // Instancia o objeto que contém o método
// Referencia o método de instância usando o objeto criado :: nome do método
Deserializer des = stringConverter::convertToInt;

```

A expressão lambda criada pela segunda das duas linhas referencia o método `convertToInt` da instância de `StringConverter` criada na primeira linha.

### Referências de Construtor

Finalmente, é possível referenciar um construtor de uma classe. Você faz isso escrevendo o nome da classe seguido por `::new`, assim:

```java
// Sintaxe conceitual para referenciar o construtor padrão ou parametrizado de MyClass
MyClass::new

```

Para ver como usar um construtor como uma expressão lambda, olhe para esta definição de interface:

```java
public interface Factory {
    // Método abstrato que espera a criação de uma String recebendo um array de char
    public String create(char[] val);
}

```

O método `create()` desta interface corresponde à assinatura de um dos construtores da classe `String`. Portanto, este construtor pode ser usado como um lambda. Aqui está um exemplo de como isso se parece:

```java
// Referência de construtor: aponta para 'new String(char[])' baseado na assinatura do método create
Factory factory = String::new;

```

Isso é equivalente a esta expressão lambda do Java:

```java
// Equivalente explícito chamando diretamente o operador 'new' com o parâmetro
Factory factory = chars -> new String(chars);

```