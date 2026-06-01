# Interfaces vs. Classes Abstratas em Java

## O Problema que Queremos Resolver

Imagine que você escreveu um sistema que baixa e processa dados de URLs. SE o código que *usa* o processador DEPENDE DIRETAMENTE da classe que *faz* o processamento, qualquer mudança nessa classe pode quebrar quem a usa. Pior: trocar a implementação exige mexer em todo o código que a chama.

A solução clássica em Java é separar o **contrato** (o que o componente promete fazer) da **implementação** (como ele faz). É exatamente para isso que existem interfaces e classes abstratas — dois mecanismos distintos, com propósitos complementares.

---

## Parte 1 — Interface: Definindo o Contrato

### O que é uma Interface?

Uma interface declara *o que* um componente deve fazer, sem dizer *como*. Pense nela como um contrato formal: qualquer classe que o assinar é **obrigada** a fornecer uma implementação para cada método declarado. A interface em si não executa nada — ela apenas estabelece a forma que as implementações devem ter.

Isso traz uma vantagem enorme: quem usa o processador depende apenas do contrato, não da classe concreta. Se amanhã você trocar a implementação, o código que chama `process()` não precisa mudar uma linha.

### Definindo a Interface

```java
// A interface estabelece o contrato público do componente.
// Ela não sabe — nem quer saber — como o processamento será feito.
public interface URLProcessor {

    // Método abstrato: toda classe que implementar URLProcessor
    // é obrigada a fornecer seu próprio código para process().
    // A assinatura (nome, parâmetros e exceção) é imutável — é o contrato.
    void process(URL url) throws IOException;
}
```

### Implementando o Contrato Diretamente

Aqui temos duas implementações independentes da mesma interface. Cada uma honra o contrato (assina o método `process`) e decide livremente como executá-lo.

**Implementação 1 — exibe os dados no console:**

```java
// Implementação concreta que cumpre o contrato da interface URLProcessor.
// Essa classe representa uma estratégia de processamento: imprimir os dados brutos.
public class PrintURLProcessor implements URLProcessor {

    // A anotação @Override confirma que estamos implementando um método da interface,
    // e faz o compilador verificar se a assinatura está correta.
    @Override
    public void process(URL url) throws IOException {
        URLConnection connection = url.openConnection();
        InputStream input = connection.getInputStream();

        try {
            int data = input.read();
            // Lê byte a byte até o fim do stream (read() retorna -1 ao terminar)
            while (data != -1) {
                System.out.print((char) data); // Converte o byte para caractere e imprime
                data = input.read();
            }
        } finally {
            input.close(); // Sempre fecha o stream, mesmo que ocorra uma exceção
        }
    }
}
```

**Implementação 2 — conta os bytes recebidos:**

```java
// Segunda implementação do mesmo contrato, com comportamento completamente diferente.
// A interface não impõe nada sobre o "como" — apenas sobre o "o quê".
public class CountURLProcessor implements URLProcessor {

    @Override
    public void process(URL url) throws IOException {
        URLConnection connection = url.openConnection();
        InputStream input = connection.getInputStream();
        int count = 0; // Contador de bytes lidos

        try {
            // Lê o stream inteiro apenas para contar; não exibe o conteúdo
            while (input.read() != -1) {
                count++;
            }
        } finally {
            input.close();
        }

        // Exibe o resultado após fechar o stream com segurança
        System.out.println("Total de bytes recebidos: " + count);
    }
}
```

### Demonstração: Usando a Interface como Tipo

O ponto central do polimorfismo está aqui. Note que a variável `processor` é declarada com o tipo `URLProcessor` (a interface), não com o tipo da classe concreta. Isso significa que o código abaixo **não precisa mudar** se você trocar `PrintURLProcessor` por qualquer outra implementação.

```java
import java.io.*;
import java.net.*;

public class MainInterface {

    public static void main(String[] args) throws IOException {

        // ── Exemplo 1: usando a implementação que imprime os dados ──────────
        // O tipo da variável é a interface — não a classe concreta.
        // Isso é polimorfismo: o mesmo contrato, comportamentos diferentes.
        URLProcessor processor = new PrintURLProcessor();
        System.out.println("=== Saída de PrintURLProcessor ===");
        processor.process(new URL("http://jenkov.com"));

        System.out.println("\n");

        // ── Exemplo 2: trocando a implementação sem mudar mais nada ─────────
        // Apenas esta linha muda. Tudo que chama processor.process() permanece igual.
        // Esse é o benefício direto de programar para a interface, não para a classe.
        processor = new CountURLProcessor();
        System.out.println("=== Saída de CountURLProcessor ===");
        processor.process(new URL("http://jenkov.com"));

        // ── Exemplo 3: passando implementações como argumento ───────────────
        // Métodos que recebem URLProcessor funcionam com qualquer implementação,
        // presente ou futura — sem precisar ser alterados.
        System.out.println("\n=== Processamento via método auxiliar ===");
        executarProcessamento(new PrintURLProcessor(), new URL("http://jenkov.com"));
    }

    // Recebe qualquer objeto que cumpra o contrato URLProcessor.
    // Não importa se é PrintURLProcessor, CountURLProcessor ou algo criado no futuro.
    private static void executarProcessamento(URLProcessor p, URL url) throws IOException {
        p.process(url);
    }
}
```

### O que Observar neste Exemplo

Ao rodar `MainInterface`, o mesmo código (`processor.process(url)`) produz comportamentos completamente diferentes dependendo de qual objeto foi atribuído à variável. Isso é polimorfismo em ação. A interface garante que qualquer implementação respeite a mesma forma, enquanto cada classe decide livremente o que fazer com os dados.

A limitação que aparece aqui é que **cada implementação repete a lógica de abrir a conexão e fechar o stream**. É exatamente esse problema que a classe abstrata resolve.

---

## Parte 2 — Classe Abstrata: Fornecendo uma Base Reutilizável

### O que é uma Classe Abstrata?

Uma classe abstrata é uma classe que **não pode ser instanciada diretamente** — ela existe para ser estendida. Seu papel é fornecer uma estrutura comum (código já pronto, campos compartilhados, fluxo de execução) enquanto delega para as subclasses apenas as partes que variam.

Quando usada em conjunto com uma interface, ela oferece o melhor dos dois mundos: o desacoplamento do contrato e o reaproveitamento de código da herança.

### A Interface (mantida como contrato central)

```java
// A interface continua sendo o contrato público do sistema.
// Nada muda para quem usa o processador — ele continua dependendo apenas de URLProcessor.
public interface URLProcessor {
    void process(URL url) throws IOException;
}
```

### A Classe Abstrata (implementação parcial)

A classe abstrata implementa a interface, mas resolve apenas o que é comum a todas as implementações. O passo específico — o que fazer com os dados — é declarado como `abstract` e delegado às subclasses.

```java
// Classe abstrata que implementa URLProcessor parcialmente.
// Ela resolve o problema identificado no Exemplo 1: a repetição de
// código de infraestrutura (abrir conexão, fechar stream) em cada classe.
public abstract class URLProcessorBase implements URLProcessor {

    // Implementação concreta do método da interface.
    // Define o esqueleto do algoritmo: conectar → processar → fechar.
    // Esse fluxo é idêntico para todas as implementações, por isso vive aqui.
    @Override
    public void process(URL url) throws IOException {
        URLConnection connection = url.openConnection();
        InputStream input = connection.getInputStream();

        try {
            // O "como processar" é desconhecido aqui — será definido pela subclasse.
            // Isso é o padrão Template Method: o esqueleto está fixo, o passo variável não.
            processURLData(input);
        } finally {
            // O fechamento do stream é garantido aqui, uma única vez para todas as subclasses.
            // Nenhuma subclasse precisa se preocupar com isso.
            input.close();
        }
    }

    // Método abstrato: o único ponto que muda entre as implementações.
    // A subclasse recebe o stream já aberto e pronto — só precisa usá-lo.
    // O modificador "protected" garante que só subclasses e o próprio pacote o acessem.
    protected abstract void processURLData(InputStream input) throws IOException;
}
```

Este é o **padrão Template Method**: a classe abstrata define o esqueleto do algoritmo (conectar, processar, fechar) e delega apenas o passo variável (`processURLData`) para as subclasses. O fluxo de controle pertence à classe base; a lógica específica pertence a quem a estende.

### Subclasse 1 — Exibindo os dados no console

```java
// Subclasse concreta que estende URLProcessorBase.
// Ela herda automaticamente toda a lógica de conexão e fechamento de stream.
// Sua única responsabilidade é dizer o que fazer com os bytes recebidos.
public class PrintURLProcessor extends URLProcessorBase {

    // Sobrescreve o método abstrato para fornecer o comportamento específico.
    // Repare: não há URLConnection, não há try/finally, não há input.close().
    // Tudo isso já está resolvido na classe base.
    @Override
    protected void processURLData(InputStream input) throws IOException {
        int data = input.read();
        while (data != -1) {          // -1 sinaliza fim do stream
            System.out.print((char) data);
            data = input.read();
        }
    }
}
```

### Subclasse 2 — Contando os bytes recebidos

```java
// Segunda subclasse: mesma herança, comportamento diferente.
// O código de infraestrutura (connection, stream, finally) não se repete —
// está centralizado na classe abstrata e é herdado automaticamente.
public class CountURLProcessor extends URLProcessorBase {

    @Override
    protected void processURLData(InputStream input) throws IOException {
        int count = 0;

        // Recebemos o stream já aberto e pronto para leitura.
        // Não precisamos abri-lo, nem nos preocupar em fechá-lo.
        while (input.read() != -1) {
            count++;
        }

        System.out.println("Total de bytes recebidos: " + count);
    }
}
```

### Demonstração: Usando a Hierarquia Completa

```java
import java.io.*;
import java.net.*;

public class MainAbstrata {

    public static void main(String[] args) throws IOException {

        // ── Exemplo 1: subclasse que imprime os dados ───────────────────────
        // A variável continua sendo do tipo da interface — o código consumidor
        // não precisa saber que existe uma classe abstrata no meio da hierarquia.
        URLProcessor processor = new PrintURLProcessor();
        System.out.println("=== Saída de PrintURLProcessor ===");
        processor.process(new URL("http://jenkov.com"));

        System.out.println("\n");

        // ── Exemplo 2: trocando por outra subclasse ─────────────────────────
        // Trocar de implementação continua sendo uma mudança de uma única linha.
        // A classe abstrata não altera esse benefício — ela opera de forma transparente.
        processor = new CountURLProcessor();
        System.out.println("=== Saída de CountURLProcessor ===");
        processor.process(new URL("http://jenkov.com"));

        System.out.println("\n");

        // ── Exemplo 3: implementação direta da interface (sem a classe base) ─
        // Este é o poder da flexibilidade: se uma implementação futura precisar
        // de um fluxo completamente diferente, ela pode implementar URLProcessor
        // diretamente, ignorando URLProcessorBase — sem quebrar nada.
        URLProcessor processadorCustom = new URLProcessor() {
            @Override
            public void process(URL url) throws IOException {
                // Implementação inline: apenas registra que a URL foi chamada.
                // Útil para mocks em testes, por exemplo.
                System.out.println("Processamento customizado para: " + url.getHost());
            }
        };

        System.out.println("=== Saída do processador customizado ===");
        processadorCustom.process(new URL("http://jenkov.com"));
    }
}
```

### O que Observar neste Exemplo

A diferença em relação ao `MainInterface` está no que as subclasses **não precisam mais fazer**: abrir a conexão, lidar com o `try/finally`, fechar o stream. Toda essa infraestrutura está centralizada em `URLProcessorBase` e herdada automaticamente.

O Exemplo 3 demonstra a flexibilidade preservada: mesmo com a classe abstrata no meio da hierarquia, ainda é possível implementar a interface diretamente quando necessário. A arquitetura não força ninguém a usar `URLProcessorBase` — ela apenas oferece essa base como conveniência.

---

## Comparativo: Interface Pura vs. Interface + Classe Abstrata

| | Interface pura | Interface + Classe Abstrata |
|---|---|---|
| **Código de infraestrutura** | Repetido em cada implementação | Centralizado na classe base, herdado |
| **Flexibilidade de troca** | Total | Total (mesma interface como tipo) |
| **Esforço para nova implementação** | Maior (reimplementar tudo) | Menor (apenas o que varia) |
| **Quando usar** | Implementações muito diferentes entre si | Implementações que compartilham estrutura comum |
| **Herança múltipla** | Sim (Java permite múltiplas interfaces) | Não (só uma superclasse) |

> **Regra de ouro:** uma classe pode implementar **múltiplas interfaces**, mas só pode herdar de **uma** superclasse. Se uma classe já possui uma superclasse, ela não pode estender a classe abstrata — mas ainda pode implementar a interface diretamente. Isso reforça o valor de manter a interface como contrato público central.

---

## Visão Geral da Arquitetura

```
«interface»
URLProcessor
    └── process(URL)                   ← contrato público
          │
          ├─── implementação direta ───► PrintURLProcessor (Parte 1)
          │                             CountURLProcessor (Parte 1)
          │                             Qualquer classe futura
          │
          └─── via classe abstrata ────► «abstract» URLProcessorBase
                                              ├── process(URL)      ← implementado (infraestrutura)
                                              └── processURLData()  ← abstrato (ponto de extensão)
                                                    │
                                                    ├── PrintURLProcessor (Parte 2)
                                                    └── CountURLProcessor (Parte 2)
```

A interface é o ponto de entrada para todo o sistema. A classe abstrata é um atalho opcional para implementações que compartilham estrutura. Quem consome o processador só conhece a interface — as camadas internas são invisíveis e intercambiáveis.