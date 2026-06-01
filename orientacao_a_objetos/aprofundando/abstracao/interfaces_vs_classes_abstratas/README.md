# Interfaces vs. Classes Abstratas em Java

## O Problema que se Busca Resolver

Imagine que você escreveu um sistema que baixa e processa dados de URLs. Se o código que *usa* o processador conhece diretamente a classe que *faz* o processamento, qualquer mudança nessa classe pode quebrar quem a usa. Pior: trocar a implementação exige mexer em todo o código que a chama.

A solução clássica em Java é separar o **contrato** (o que o componente promete fazer) da **implementação** (como ele faz). É exatamente para isso que existem interfaces e classes abstratas.

---

## Interface: Definindo o Contrato

Uma interface declara *o que* um componente deve fazer, sem dizer *como*. Qualquer classe que assinar esse contrato é obrigada a fornecer uma implementação para cada método declarado.

```java
public interface URLProcessor {
    // Contrato: toda implementação deve saber processar uma URL.
    // O "como" fica totalmente a cargo de quem implementar.
    void process(URL url) throws IOException;
}
```

Quem depende de `URLProcessor` não precisa saber se os dados vêm de um servidor HTTP, de um arquivo local ou de um mock de testes. O contrato é o mesmo para todos os casos.

---

## Classe Abstrata: Fornecendo uma Base Reutilizável

Às vezes, várias implementações compartilham uma mesma lógica estrutural. Em vez de repetir esse código em cada classe, você pode centralizá-lo em uma **classe abstrata** — que implementa a interface mas deixa as partes específicas para as subclasses preencherem.

```java
public abstract class URLProcessorBase implements URLProcessor {

    // Implementa o fluxo comum: abre a conexão, garante o fechamento do stream.
    // Esse código não precisa ser repetido em cada implementação.
    @Override
    public void process(URL url) throws IOException {
        URLConnection urlConnection = url.openConnection();
        InputStream input = urlConnection.getInputStream();

        try {
            processURLData(input); // Delega o comportamento específico à subclasse
        } finally {
            input.close(); // Garante o fechamento mesmo em caso de erro
        }
    }

    // Ponto de extensão: cada subclasse define o que fazer com os dados recebidos.
    protected abstract void processURLData(InputStream input) throws IOException;
}
```

Perceba o padrão: a classe abstrata cuida da infraestrutura (conexão, tratamento de erros, liberação de recursos) e deixa apenas a lógica de negócio para as subclasses.

---

## Subclasse Concreta: A Implementação Real

A subclasse herda toda a estrutura da classe abstrata e só precisa implementar o que é específico dela — neste caso, ler e exibir os bytes recebidos.

```java
public class URLProcessorImpl extends URLProcessorBase {

    @Override
    protected void processURLData(InputStream input) throws IOException {
        int data = input.read();
        while (data != -1) {          // -1 indica fim do stream
            System.out.print((char) data);
            data = input.read();
        }
    }
}
```

Toda a complexidade de abrir a conexão e fechar o stream já foi resolvida na classe abstrata. `URLProcessorImpl` foca exclusivamente na sua responsabilidade.

---

## Polimorfismo: O Poder do Contrato

Com essa estrutura, você pode usar a **interface como tipo da variável**, sem que o restante do código precise saber qual implementação está sendo usada:

```java
// O tipo declarado é a interface — o código que usa o processador
// não sabe (nem precisa saber) que é um URLProcessorImpl.
URLProcessor urlProcessor = new URLProcessorImpl();

urlProcessor.process(new URL("http://jenkov.com"));
```

Amanhã, se você criar um `CachedURLProcessor` ou um `MockURLProcessor` para testes, basta trocar a linha de instanciação. O resto do sistema não muda.

---

## Quando Usar Cada Um

| Situação | Solução recomendada |
|---|---|
| Quero apenas definir um contrato e garantir desacoplamento | Interface |
| Quero um contrato **e** fornecer código base reutilizável | Interface + Classe Abstrata |
| A classe já herda de outra classe e precisa de comportamento adicional | Interface (Java só permite herança simples) |

> **Regra de ouro:** uma classe pode implementar **múltiplas interfaces**, mas só pode herdar de **uma** superclasse. Interfaces são, portanto, o mecanismo mais flexível para compartilhar comportamentos entre classes de hierarquias distintas.

---

## Visão Geral da Arquitetura

```
«interface»
URLProcessor
    └── process(URL)
         │
         ▼
«abstract»
URLProcessorBase
    ├── process(URL)       ← implementado (fluxo comum)
    └── processURLData()   ← abstrato (delegado à subclasse)
         │
         ▼
URLProcessorImpl
    └── processURLData()   ← implementado (lógica específica)
```

Essa separação em três camadas oferece o melhor dos dois mundos: o desacoplamento da interface e o reaproveitamento de código da classe abstrata. Se você precisar de uma implementação muito diferente, pode ignorar a classe abstrata e implementar a interface diretamente — a flexibilidade fica preservada.