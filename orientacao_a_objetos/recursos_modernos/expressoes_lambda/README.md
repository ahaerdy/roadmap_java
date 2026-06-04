# Expressões Lambda em Java

*Referência teórica e prática para engenharia de software e transição para o paradigma funcional.*

---

As expressões lambda, introduzidas a partir do Java 8, representam um marco fundamental na evolução da linguagem, permitindo a transição do modelo estritamente imperativo/orientado a objetos para o paradigma funcional. Em termos estruturais, uma expressão lambda é uma função anônima — uma função que não possui nome, lista de modificadores ou tipo de retorno explícito — projetada para implementar o método de uma Interface Funcional de forma concisa.

---

## 1. Fundamentos Teóricos e Sintaxe

O principal objetivo das expressões lambda é tratar o comportamento como dados. Isso possibilita passar blocos de lógica diretamente como argumentos para métodos, eliminando a verbosidade associada às implementações tradicionais da plataforma Java.

### Anatomia de uma Expressão Lambda

A sintaxe de uma lambda é composta por três componentes estruturais distintos:

1. **Parâmetros de Entrada:** Os argumentos que a função recebe para processamento.
2. **Operador Arrow (`->`):** O token que separa formalmente a assinatura dos parâmetros do corpo da função.
3. **Corpo da Expressão:** O escopo de execução contendo a lógica que processa os parâmetros de entrada.

```text
(parâmetros) -> { corpo_da_execucao }

```

---

## 2. O Conceito de Interfaces Funcionais (SAM)

Uma expressão lambda não existe de forma isolada no ecossistema Java; ela requer um tipo de destino (*target type*). Esse tipo deve ser obrigatoriamente uma **Interface Funcional**, também conhecida pela sigla **SAM** (*Single Abstract Method*). Trata-se de uma interface que possui estritamente **um único método abstrato**.

Para assegurar a integridade do design em tempo de compilação, utiliza-se a anotação `@FunctionalInterface`. Caso um desenvolvedor tente adicionar um segundo método abstrato a esta interface, o compilador emitirá um erro.

```java
@FunctionalInterface
public interface OperacaoMatematica {
    // Único método abstrato que define o contrato da interface
    int executar(int a, int b); 
}

```

---

## 3. Análise Comparativa: Abordagem Imperativa vs. Funcional

Para compreender o ganho em legibilidade e a redução da verbosidade arquitetural, analise o cenário onde é necessário implementar a interface `OperacaoMatematica` descrita anteriormente.

### Abordagem Tradicional (Classe Anônima)

No modelo anterior ao Java 8, o isolamento de um comportamento exigia a instanciação explícita de uma classe anônima em tempo de execução:

```java
OperacaoMatematica somaTradicional = new OperacaoMatematica() {
    @Override
    public int executar(int a, int b) {
        return a + b; // O núcleo lógico está encapsulado sob uma estrutura redundante
    }
};

```

### Abordagem Funcional (Expressão Lambda)

Aproveitando o mecanismo de **inferência de tipos** do compilador Java, a sintaxe pode ser reduzida ao seu núcleo lógico fundamental:

```java
OperacaoMatematica somaFuncional = (a, b) -> a + b; // Tipos de dados e palavra-chave 'return' inferidos automaticamente

```

---

## 4. Implementações Práticas e Regras de Sintaxe

O código abaixo demonstra diferentes cenários de aplicação de expressões lambda, evidenciando as regras de simplificação sintática conforme a assinatura do método alvo.

```java
import java.util.ArrayList;
import java.util.List;

public class ExecucaoLambda {

    public static void main(String[] args) {
        
        // ====================================================================
        // Cenário 1: Ausência de parâmetros de entrada (Interface Runnable)
        // ====================================================================
        
        // Sintaxe exige parênteses vazios () para denotar a ausência de argumentos
        Runnable tarefaBackground = () -> System.out.println("Executando processo assíncrono.");
        tarefaBackground.run();

        // ====================================================================
        // Cenário 2: Parâmetro único (Omissão de delimitadores)
        // ====================================================================
        List<String> engenheiros = new ArrayList<>();
        engenheiros.add("Arthur");
        engenheiros.add("Diógenes");
        engenheiros.add("Paloma");

        // Quando há exatamente um parâmetro, os parênteses delimitadores são opcionais.
        // O método forEach consome a interface funcional 'Consumer<T>'
        engenheiros.forEach(engenheiro -> System.out.println("Registro processado: " + engenheiro));

        // ====================================================================
        // Cenário 3: Escopo de execução com múltiplas linhas
        // ====================================================================
        
        // Se a lógica interna demandar múltiplas instruções, o uso de chaves {}
        // e a declaração explícita da instrução 'return' tornam-se obrigatórios.
        OperacaoMatematica algoritmoComplexo = (x, y) -> {
            System.out.println("Iniciando processamento matemático complexo...");
            int fatorMultiplicacao = x * 2;
            return fatorMultiplicacao + y; // Retorno explícito exigido pelo bloco de código {}
        };

        System.out.println("Resultado do algoritmo: " + algoritmoComplexo.executar(5, 10));
    }
}

```

---

## 5. Interfaces Funcionais Nativas (`java.util.function`)

Para mitigar a necessidade de criação constante de interfaces customizadas, a JDK disponibiliza o pacote padrão `java.util.function`, mapeando os padrões matemáticos e lógicos mais comuns da computação:

| Interface | Método Abstrato | Propósito Lógico | Exemplo de Expressão |
| --- | --- | --- | --- |
| **`Predicate<T>`** | `boolean test(T t)` | Avaliação condicional (Filtros). Retorna um booleano. | `string -> string.isEmpty()` |
| **`Consumer<T>`** | `void accept(T t)` | Processamento de efeitos colaterais. Sem retorno. | `valor -> System.out.println(valor)` |
| **`Function<T, R>`** | `R apply(T t)` | Transformação ou mapeamento de um tipo `T` para `R`. | `objeto -> objeto.toString()` |
| **`Supplier<T>`** | `T get()` | Fábrica ou provedor de dados. Não requer entradas. | `() -> new StringBuilder()` |

### Demonstração Prática com `Predicate`

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class FiltragemDados {
    public static void main(String[] args) {
        List<Integer> dataset = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Instanciação de um predicado para validação de valores pares
        Predicate<Integer> condicaoParidade = numero -> numero % 2 == 0;

        System.out.println("Filtrando elementos do conjunto:");
        for (Integer registro : dataset) {
            // Execução formal do método de teste definido via lambda
            if (condicaoParidade.test(registro)) {
                System.out.print(registro + " ");
            }
        }
    }
}

```

---

## 6. Restrições de Escopo e Otimizações de Código

Ao implementar expressões lambda no pipeline de engenharia, duas regras de arquitetura devem ser observadas rigorosamente:

### Restrição de Escopo: Variáveis Efetivamente Finais (*Effectively Final*)

Uma expressão lambda pode acessar variáveis definidas em seu escopo externo (captura de escopo ou *closures*), sob a condição restrita de que tais variáveis sejam semanticamente imutáveis. Ou seja, devem ser declaradas como `final` ou comportar-se como tais.

```java
int fatorEscalar = 5;
OperacaoMatematica multiplicador = (a, b) -> a * b * fatorEscalar; // Compilação bem-sucedida.

// fatorEscalar = 10; 
// Se a linha acima for descomentada, o compilador gerará um erro, pois a variável perde o status de "effectively final".

```

### Otimização Sintática: Referências a Métodos (*Method References*)

Sempre que o corpo de uma expressão lambda se limitar a repassar integralmente os argumentos recebidos para um método já existente, a sintaxe pode ser otimizada utilizando o operador de resolução de escopo `::`.

```java
// Implementação lambda padrão:
engenheiros.forEach(nome -> System.out.println(nome));

// Otimização via Method Reference:
engenheiros.forEach(System.out::println);

```

---

## Conclusão

A adoção de expressões lambda eleva a manutenibilidade do código fonte Java e atua como pré-requisito técnico indispensável para a utilização avançada da **Streams API**, viabilizando operações complexas de processamento de dados paralelas, assíncronas e declarativas.