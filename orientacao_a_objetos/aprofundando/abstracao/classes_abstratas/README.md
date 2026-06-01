# Classes Abstratas em Java

Uma *classe abstrata em Java* é uma classe que não pode ser instanciada, o que significa que você não pode criar novas instâncias de uma classe abstrata. O propósito de uma classe abstrata é funcionar como uma base para subclasses. Este tutorial explica como as classes abstratas são criadas em Java e quais regras se aplicam a elas. 

## Declarando uma Classe Abstrata em Java

Em Java, você declara que uma classe é abstrata adicionando a palavra-chave `abstract` à declaração da classe. Aqui está um exemplo de classe abstrata em Java:

```java
// A palavra-chave 'abstract' define que esta classe é abstrata e não pode ser instanciada diretamente
public abstract class MyAbstractClass {

}

```

Isso é tudo o que há para declarar uma classe abstrata em Java. Agora você não pode criar instâncias de `MyAbstractClass`. Portanto, o seguinte código Java não é mais válido:

```java
// Tentativa de criar um objeto a partir da classe abstrata
MyAbstractClass myClassInstance = 
    new MyAbstractClass();  // NÃO É VÁLIDO: Erro de compilação ocorre aqui

```

Se você tentar compilar o código acima, o compilador Java gerará um erro, dizendo que você não pode instanciar `MyAbstractClass` porque ela é uma classe abstrata.

## Métodos Abstratos

Uma classe abstrata pode ter métodos abstratos. Você declara um método como abstrato adicionando a palavra-chave `abstract` antes da declaração do método. Aqui está um exemplo de método abstrato em Java:

```java
public abstract class MyAbstractClass {

    // Método abstrato: possui modificador, tipo de retorno e assinatura, mas termina com ponto e vírgula
    // Não há corpo do método (sem chaves {}), delegando a responsabilidade de implementação para as subclasses
    public abstract void abstractMethod();
}

```

Um método abstrato não possui implementação. Ele possui apenas uma assinatura de método. Assim como os métodos em uma interface Java.

Se uma classe possui um método abstrato, a classe inteira deve ser declarada como abstrata. Nem todos os métodos em uma classe abstrata precisam ser métodos abstratos. Uma classe abstrata pode ter uma mistura de métodos abstratos e não abstratos.

As subclasses de uma classe abstrata devem implementar (sobrescrever) todos os métodos abstratos de sua superclasse abstrata. Os métodos não abstratos da superclasse são simplesmente herdados como são. Eles também podem ser sobrescritos, se necessário.

Aqui está um exemplo de subclasse da classe abstrata `MyAbstractClass`:

```java
// A palavra-chave 'extends' estabelece a relação de herança com a classe abstrata
public class MySubClass extends MyAbstractClass {

    // Sobrescrita obrigatória do método abstrato herdado da superclasse
    // Fornece o corpo concreto do método com as chaves {}
    public void abstractMethod() {
        // Exibe uma mensagem de texto simples no console de saída
        System.out.println("My method implementation");
    }
}

```

Observe como `MySubClass` tem que implementar o método abstrato `abstractMethod()` de sua superclasse abstrata `MyAbstractClass`.

A única situação em que uma subclasse de uma classe abstrata não é forçada a implementar todos os métodos abstratos de sua superclasse é se a subclasse também for uma classe abstrata.

## O Propósito de Classes Abstratas

O propósito das classes abstratas é funcionar como classes base que podem ser estendidas por subclasses para criar uma implementação completa. Por exemplo, imagine que um determinado processo requer 3 etapas:

1. A etapa antes da ação.
2. A ação.
3. A etapa após da ação.

Se as etapas antes e após a ação forem sempre as mesmas, o processo de 3 etapas poderia ser implementado em uma superclasse abstrata com este código Java:

```java
public abstract class MyAbstractProcess {

    // Método concreto principal que dita o fluxo e a ordem de execução do algoritmo
    public void process() {
        stepBefore(); // Executa o primeiro passo fixo
        action();     // Executa o passo variável/abstrato (definido pelas subclasses)
        stepAfter();  // Executa o último passo fixo
    }

    // Método concreto com comportamento padrão compartilhado por todas as subclasses
    public void stepBefore() {
        // implementação diretamente na superclasse abstrata
    }

    // Método abstrato que força cada subclasse a fornecer sua própria lógica customizada para a ação
    public abstract void action(); // implementado pelas subclasses

    // Outro método concreto com comportamento fixo compartilhado
    public void stepAfter() {
        // implementação diretamente na superclasse abstrata
    }
}

```

Observe como o método `action()` é abstrato. As subclasses de `MyAbstractProcess` podem agora estender `MyAbstractProcess` e apenas sobrescrever o método `action()`.

Quando o método `process()` da subclasse é chamado, o processo completo é executado, incluindo o `stepBefore()` e `stepAfter()` da superclasse abstrata, e o método `action()` da subclasse.

É claro que a `MyAbstractProcess` não precisava ser uma classe abstrata para funcionar como uma classe base. Nem o método `action()` precisava ser abstrato. Você poderia ter usado apenas uma classe comum. No entanto, ao tornar o método a ser implementado abstrato, e consequentemente a classe também, você sinaliza claramente aos usuários desta classe que esta classe não deve ser usada como ela está. Em vez disso, ela deve ser usada como uma classe base para uma subclasse, e que o método abstrato deve ser implementado na subclasse.

O exemplo acima não tinha uma implementação padrão para o método `action()`. Em alguns casos, sua superclasse pode realmente ter uma implementação padrão para o método que as subclasses devem sobrescrever. Nesse caso, você não pode tornar o método abstrato. Você ainda pode tornar a superclasse abstrata, mesmo que ela não contenha métodos abstratos.

Aqui está um exemplo mais concreto que abre uma URL, processa-a e fecha a conexão com a URL depois.

```java
public abstract class URLProcessorBase {

    // Método concreto que gerencia a infraestrutura e o ciclo de vida dos recursos de rede
    public void process(URL url) throws IOException {
        // Abre a conexão de rede a partir do objeto URL recebido
        URLConnection urlConnection = url.openConnection();
        // Obtém o fluxo de dados de entrada da conexão aberta
        InputStream input = urlConnection.getInputStream();

        try {
            // Invoca o método abstrato passando os dados; a lógica de negócio exata depende da subclasse
            processURLData(input);
        } finally {
            // Garante o fechamento seguro do recurso de rede, independentemente de falhas no processamento
            input.close();
        }
    }

    // Método abstrato protegido para isolar o processamento dos dados brutos
    // Deve ser implementado por quem estender esta classe
    protected abstract void processURLData(InputStream input)
        throws IOException;

}

```

Observe como o `processURLData()` é um método abstrato, e que `URLProcessorBase` é uma classe abstrata. As subclasses de `URLProcessorBase` têm que implementar o método `processURLData()` porque ele é um método abstrato.

As subclasses da classe abstrata `URLProcessorBase` podem processar dados baixados de URLs sem se preocupar em abrir e fechar a conexão de rede com a URL. Isso é feito pela `URLProcessorBase`. As subclasses só precisam se preocupar em processar os dados do `InputStream` passado para o método `processURLData()`. Isso torna mais fácil implementar classes que processam dados de URLs.

Aqui está um exemplo de subclasse:

```java
public class URLProcessorImpl extends URLProcessorBase {

    // A anotação @Override indica explicitamente que este método substitui o método abstrato da superclasse
    @Override
    protected void processURLData(InputStream input) throws IOException {
        // Lê o primeiro byte de dados do fluxo de entrada
        int data = input.read();
        // Loop de repetição continua até encontrar o fim do fluxo (representado por -1)
        while(data != -1){
            // Converte o byte numérico lido para seu caractere correspondente e imprime na tela
            System.out.println((char) data);
            // Lê o próximo byte de dados para continuar a iteração
            data = input.read();
        }
    }
}

```

Observe como a subclasse apenas implementa o método `processURLData()`, e nada mais. O restante do código é herdado da superclasse `URLProcessorBase`.

Aqui está um exemplo de como usar a classe `URLProcessorImpl`:

```java
// Instanciação da classe filha concreta que possui a implementação de processamento
URLProcessorImpl urlProcessor = new URLProcessorImpl();

// Executa o fluxo genérico herdado, passando a URL do site como argumento
urlProcessor.process(new URL("[http://jenkov.com](http://jenkov.com)"));

```

O método `process()` é chamado, o qual está implementado na superclasse `URLProcessorBase`. Este método, por sua vez, chama o `processURLData()` na classe `URLProcessorImpl`.

### Classes Abstratas e o Padrão de Projeto Template Method

O exemplo que mostrei acima com a classe `URLProcessorBase` é na verdade um exemplo do padrão de projeto Template Method. O padrão de projeto Template Method fornece uma implementação parcial de algum processo, que as subclasses podem completar ao estender a classe base do Template Method.
