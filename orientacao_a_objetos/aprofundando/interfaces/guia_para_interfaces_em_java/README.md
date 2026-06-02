# Guia para Interfaces em Java

## 1. Visão Geral

Uma interface em Java é um tipo abstrato que define um conjunto de métodos e constantes que outras classes se comprometem a implementar. É um dos conceitos centrais da linguagem, e serve como base para três recursos fundamentais: **abstração**, **polimorfismo** e **herança múltipla**.

---

## 2. O que são Interfaces em Java?

Uma interface declara *o que* um tipo deve ser capaz de fazer — sem ditar *como* fazê-lo. Veja um exemplo com os quatro tipos de membros que uma interface pode conter:

```java
public interface Electronic {

    // CONSTANTE: implicitamente 'public static final'.
    // Pode ser acessada diretamente como Electronic.LED, sem instanciar nada.
    String LED = "LED";

    // MÉTODO ABSTRATO: não tem corpo — apenas a assinatura.
    // Toda classe que implementar Electronic DEVE fornecer uma implementação
    // para este método, ou o compilador acusará erro.
    int getElectricityUse();

    // MÉTODO ESTÁTICO: tem implementação, pertence à interface (não às instâncias)
    // e não pode ser sobrescrito pelas classes que implementam a interface.
    // Útil para funções utilitárias relacionadas ao contrato da interface.
    static boolean isEnergyEfficient(String electronicType) {
        if (electronicType.equals(LED)) {
            return true;  // LED é considerado eficiente
        }
        return false;
    }

    // MÉTODO DEFAULT: tem implementação, mas as classes que implementam a interface
    // podem sobrescrevê-lo se precisarem de um comportamento diferente.
    // Introduzido no Java 8 para permitir a evolução de interfaces sem quebrar
    // código existente (veja a seção 4 para mais detalhes).
    default void printDescription() {
        System.out.println("Electronic Device");
    }
}
```

Para usar a interface, criamos uma classe que a implementa com a palavra-chave `implements`:

```java
// 'implements Electronic' é o "contrato assinado":
// o compilador garante que Computer implementa todos os métodos abstratos
// declarados em Electronic. Se algum estiver faltando, o código não compila.
public class Computer implements Electronic {

    // Implementação obrigatória do único método abstrato de Electronic.
    // A assinatura (nome, tipo de retorno e parâmetros) deve ser idêntica
    // à declarada na interface. Só o corpo do método é responsabilidade nossa.
    @Override
    public int getElectricityUse() {
        return 1000; // consumo fixo de 1000 watts para este modelo
    }

    // Nota: getElectricityUse() foi implementado porque é abstrato (obrigatório).
    // printDescription() NÃO foi implementado porque é 'default' (opcional):
    // Computer herdará automaticamente a implementação da interface.
}
```

### 2.1. Regras para Criação de Interfaces

- ✅ Permitido: constantes (`public static final`), métodos abstratos, métodos `static` e métodos `default`.
- ❌ Interfaces não podem ser instanciadas diretamente (`new Electronic()` causa erro).
- ❌ A palavra-chave `final` não pode ser usada na definição da interface — uma interface `final` nunca poderia ser implementada, o que a tornaria inútil.
- ❌ Métodos de interface não podem ser `protected` ou `final`.
- ✅ Desde o Java 9, métodos `private` são permitidos (úteis para fatorar lógica compartilhada entre métodos `default`).
- O compilador adiciona automaticamente `abstract` nos métodos e `public static final` nas variáveis, mesmo que você os omita.

---

## 3. O que Podemos Alcançar Usando Interfaces?

### 3.1. Abstração de Comportamento

Interfaces permitem definir capacidades que classes não relacionadas podem compartilhar — sem forçar uma hierarquia de herança artificial. `Comparable`, `Comparator` e `Cloneable` são exemplos da biblioteca padrão do Java: uma `String` e um `Integer` não têm nada em comum, mas ambas podem ser `Comparable`.

### 3.2. Herança Múltipla

Em Java, uma classe só pode estender uma única classe-pai. Mas pode implementar quantas interfaces quiser — é assim que o Java oferece herança múltipla de comportamento:

```java
// Interface que define a capacidade de voar.
public interface Fly {
    void fly();
}
```

```java
// Interface que define a capacidade de se transformar.
public interface Transform {
    void transform();
}
```

```java
// Car implementa DUAS interfaces ao mesmo tempo.
// Isso significa que Car assume o compromisso de implementar
// os métodos de ambas: fly() E transform().
// Uma classe só pode 'extends' uma única classe, mas pode
// 'implements' múltiplas interfaces — essa é a herança múltipla em Java.
public class Car implements Fly, Transform {

    // Implementação obrigatória exigida pela interface Fly.
    @Override
    public void fly() {
        System.out.println("Eu posso Voar!!");
    }

    // Implementação obrigatória exigida pela interface Transform.
    @Override
    public void transform() {
        System.out.println("Eu posso me transformar!!");
    }
}
```

### 3.3. Polimorfismo

Polimorfismo significa tratar objetos de tipos concretos diferentes de forma uniforme, desde que compartilhem a mesma interface. O código que opera sobre a interface não precisa saber qual classe concreta está usando:

```java
// Interface Shape: qualquer forma geométrica deve saber retornar seu nome.
public interface Shape {
    String name();
}
```

```java
// Circle é uma Shape. Ela define o que "nome" significa para um círculo.
public class Circle implements Shape {

    @Override
    public String name() {
        return "Circle";
    }
}
```

```java
// Square também é uma Shape, com sua própria definição de nome.
public class Square implements Shape {

    @Override
    public String name() {
        return "Square";
    }
}
```

```java
// A lista é do tipo Shape — ela não sabe (e não precisa saber)
// se guarda Circles, Squares, ou qualquer outra forma futura.
List<Shape> shapes = new ArrayList<>();

// As variáveis são declaradas como Shape (tipo da interface),
// mas os objetos criados são das classes concretas.
// Isso é possível porque Circle e Square "são" Shapes.
Shape circleShape = new Circle();
Shape squareShape = new Square();

shapes.add(circleShape);
shapes.add(squareShape);

// O loop chama shape.name() sem saber o tipo concreto de cada objeto.
// O Java decide em tempo de execução qual implementação de name() invocar:
// para circleShape → Circle.name() → "Circle"
// para squareShape → Square.name() → "Square"
for (Shape shape : shapes) {
    System.out.println(shape.name());
}
```

---

## 4. Métodos Default em Interfaces

Antes do Java 8, adicionar um novo método a uma interface pública quebrava todo código existente que a implementava — todas as classes eram obrigadas a implementar o método novo ou deixavam de compilar.

O Java 8 introduziu os **métodos `default`** para resolver esse problema: um método com implementação embutida na própria interface, que as classes herdam automaticamente se não o sobrescreverem.

```java
public interface Transform {

    // Método abstrato: obrigatório para todas as classes que implementarem Transform.
    void transform();

    // Método default: OPCIONAL para as classes implementadoras.
    // Se uma classe não sobrescrever printSpecs(), ela herdará esta implementação.
    // Se sobrescrever, sua implementação tem precedência sobre esta.
    // Isso permite adicionar novos comportamentos a uma interface sem quebrar
    // nenhuma classe existente que já a implementava.
    default void printSpecs() {
        System.out.println("Transform Specification");
    }
}
```

```java
// Vehicle implementa Transform mas NÃO implementa printSpecs().
// Isso é válido porque printSpecs() é 'default' — não obrigatório.
// Vehicle herda automaticamente a implementação de printSpecs() da interface.
// Ela DEVE implementar transform(), pois este é abstrato.
// Por ser uma classe abstrata, Vehicle pode deixar transform() sem implementação
// e delegar essa responsabilidade para suas subclasses concretas.
public abstract class Vehicle implements Transform {}
```

---

## 5. Regras de Herança de Interfaces

### 5.1. Interface Estendendo Outra Interface

Interfaces podem herdar de outras interfaces com `extends`, acumulando todos os seus métodos:

```java
// HasColor define que qualquer coisa colorida deve saber retornar sua cor.
public interface HasColor {
    String getColor();
}
```

```java
// Box estende HasColor: além de getHeight() (próprio),
// Box também herda getColor() de HasColor.
// Uma classe que implementar Box deverá implementar AMBOS os métodos:
// getColor() (herdado de HasColor) e getHeight() (declarado aqui).
public interface Box extends HasColor {
    int getHeight();
}
```

### 5.2. Classe Abstrata Implementando uma Interface

Quando uma **classe abstrata** implementa uma interface, ela herda todos os seus métodos abstratos e `default`, mas **não é obrigada a implementar os métodos abstratos**. Ela pode deixar essa responsabilidade para as primeiras subclasses concretas que a estenderem.

Isso é útil para criar implementações parciais: a classe abstrata implementa o que for comum a todas as subclasses e deixa em aberto o que for específico de cada uma.

---

## 6. Interfaces Funcionais

Uma **interface funcional** é qualquer interface com exatamente **um único método abstrato**. Ela pode ter múltiplos métodos `static` ou `default`, mas apenas um abstrato.

Essa restrição é o que permite que a interface seja implementada por uma **expressão lambda** — uma função anônima concisa introduzida no Java 8.

```java
// @FunctionalInterface é opcional, mas recomendado:
// ela instrui o compilador a verificar que a interface tem
// exatamente um método abstrato. Se você adicionar um segundo por engano,
// o compilador acusará erro imediatamente — em vez de falhar silenciosamente.
@FunctionalInterface
public interface Validator<T> {

    // Único método abstrato: a "assinatura do contrato funcional".
    // É este método que uma expressão lambda irá implementar.
    boolean validate(T value);

    // Métodos default e static NÃO contam para o limite de "um método abstrato",
    // portanto a interface continua sendo funcional mesmo com eles presentes.
    default Validator<T> and(Validator<T> other) {
        return value -> this.validate(value) && other.validate(value);
    }
}
```

```java
// Sem lambda (modo clássico): implementação anônima explícita.
// Verbosa, mas equivalente ao que a lambda faz internamente.
Validator<String> classicValidator = new Validator<String>() {
    @Override
    public boolean validate(String value) {
        return value != null && !value.isEmpty();
    }
};

// Com lambda (modo funcional): o compilador sabe que a lambda implementa
// o único método abstrato da interface — validate().
// A sintaxe "value -> expressão" substitui todo o bloco anônimo acima.
Validator<String> lambdaValidator = value -> value != null && !value.isEmpty();

System.out.println(lambdaValidator.validate("Java")); // true
System.out.println(lambdaValidator.validate(""));     // false
```

Interfaces funcionais nativas mais usadas da biblioteca padrão do Java:

| Interface | Método abstrato | Uso típico |
|---|---|---|
| `Runnable` | `void run()` | tarefas em threads |
| `Comparable<T>` | `int compareTo(T o)` | ordenação natural |
| `Predicate<T>` | `boolean test(T t)` | filtragem em Streams |
| `Function<T,R>` | `R apply(T t)` | transformação de valores |
| `Consumer<T>` | `void accept(T t)` | ação sem retorno |
| `Supplier<T>` | `T get()` | fornecimento de valores |

---

## 7. Conclusão

| Recurso | Para que serve |
|---|---|
| Métodos abstratos | Definem o contrato obrigatório que as classes devem cumprir |
| Constantes | Agrupam valores imutáveis relacionados ao contrato |
| Métodos `default` | Permitem evoluir a interface sem quebrar implementações existentes |
| Métodos `static` | Fornecem utilitários relacionados ao contrato da interface |
| Herança múltipla | Permitem que uma classe assine vários contratos ao mesmo tempo |
| Polimorfismo | Permitem tratar objetos diferentes de forma uniforme pelo tipo da interface |
| Interfaces funcionais | Habilitam o uso de lambdas e a programação funcional no Java 8+ |