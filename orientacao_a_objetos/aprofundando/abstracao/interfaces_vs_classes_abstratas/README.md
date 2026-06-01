# Interfaces vs. Classes Abstratas em Java

Duas perguntas que se faz frequentemente é: Qual a diferença entre interfaces e classes abstratas em Java, e quando usar cada uma. Tendo respondido a essa pergunta por e-mail várias vezes, decidi escrever este tutorial sobre interfaces Java vs. classes abstratas.

As interfaces Java são usadas para desacoplar a interface de algum componente de sua implementação. Em outras palavras, para tornar as classes que utilizam a interface independentes das classes que a implementam. Assim, você pode trocar a implementação da interface sem precisar alterar a classe que a utiliza.

As classes abstratas são tipicamente usadas como classes base para extensão por subclasses. Algumas linguagens de programação usam classes abstratas para alcançar o polimorfismo e separar a interface da implementação, mas em Java você usa interfaces para isso. Lembre-se de que uma classe Java pode ter apenas 1 superclasse, mas pode implementar múltiplas interfaces. Sendo assim, se uma classe já possui uma superclasse diferente, ela pode implementar uma interface, mas não pode estender outra classe abstrata. Portanto, as interfaces são um mecanismo mais flexível para expor uma interface comum.

Se você precisa separar uma interface de sua implementação, use uma interface. Se você também precisa fornecer uma classe base ou uma implementação padrão da interface, adicione uma classe abstrata (comum ou normal) que implemente a interface.

Aqui está um exemplo que mostra uma classe referenciando uma interface, uma classe abstrata que implementa essa interface e uma subclasse que estende a classe abstrata.

A classe azul conhece apenas a interface. A classe abstrata implementa a interface, e a subclasse herda da classe abstrata.

Abaixo estão os exemplos de código do texto sobre Classes Abstratas em Java, mas com a adição de uma interface que é implementada pela classe base abstrata. Dessa forma, assemelha-se ao diagrama acima.

Primeiro, a interface:

```java
// Definição da interface que estabelece o contrato para processamento de URLs
public interface URLProcessor {
    // Declaração do método abstrato. Qualquer classe que implementar esta interface
    // será obrigada a fornecer uma implementação para o método process.
    public void process(URL url) throws IOException;
}

```

Segundo, a classe base abstrata:

```java
// Classe abstrata que implementa a interface URLProcessor.
// Ela fornece a estrutura principal (esqueleto) para o processamento.
public abstract class URLProcessorBase implements URLProcessor {

    // Implementação concreta do método da interface. 
    // Define o fluxo comum de abrir a conexão e garantir o fechamento do fluxo.
    public void process(URL url) throws IOException {
        URLConnection urlConnection = url.openConnection();
        InputStream input = urlConnection.getInputStream();

        try {
            // Delegação do processamento específico dos dados para o método abstrato
            processURLData(input);
        } finally {
            // Garante que o recurso InputStream será fechado independentemente de erros
            input.close();
        }
    }

    // Método abstrato que as subclasses devem obrigatoriamente implementar
    // para definir o comportamento específico de processamento dos dados.
    protected abstract void processURLData(InputStream input)
        throws IOException;
}

```

Terceiro, a subclasse da classe base abstrata:

```java
// Subclasse concreta que estende a classe abstrata URLProcessorBase
public class URLProcessorImpl extends URLProcessorBase {

    // Sobrescrita do método abstrato da classe base para fornecer a lógica
    // específica de leitura e exibição dos dados da URL no console.
    @Override
    protected void processURLData(InputStream input) throws IOException {
        int data = input.read();
        // Loop para ler todos os bytes do InputStream até atingir o fim do fluxo (-1)
        while(data != -1){
            System.out.println((char) data);
            data = input.read();
        }
    }
}

```

Quarto, como usar a interface URLProcessor como tipo de variável, mesmo que seja a subclasse URLProcessorImpl que seja instanciada.

```java
// Polimorfismo: O tipo da variável é a interface (URLProcessor),
// mas o objeto real instanciado é a implementação específica (URLProcessorImpl).
URLProcessor urlProcessor = new URLProcessorImpl();

// Execução do processamento através da referência da interface,
// mantendo o código desacoplado da implementação concreta.
urlProcessor.process(new URL("[http://jenkov.com](http://jenkov.com)"));

```

O uso combinado de uma interface e de uma classe base abstrata torna seu código mais flexível. É possível implementar processadores de URL simples apenas criando uma subclasse a partir da classe base abstrata. Se precisar de algo mais avançado, seu processador de URL pode simplesmente implementar a interface URLProcessor diretamente, sem herdar de URLProcessorBase.
