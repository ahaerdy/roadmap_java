# Interfaces em Java

Uma interface define **o que** uma classe deve ser capaz de fazer, sem ditar **como** ela deve fazer. Tecnicamente, uma interface pode conter assinaturas de métodos, constantes e — a partir do Java 8 — implementações padrão (*default*).

A assinatura de um método é composta pelo seu nome, seus parâmetros e as exceções que ele pode lançar. A interface declara essa "promessa de comportamento", e cada classe que a implementa é responsável por escrever o corpo desses métodos.

Interfaces são o principal mecanismo de Java para alcançar o **polimorfismo**: ao referenciar um objeto pelo tipo da interface (em vez do tipo concreto da classe), o código fica desacoplado de implementações específicas.

---

## Exemplo de Interface Java

```java
public interface MyInterface {

    // Constante pública: implicitamente 'public static final'.
    // Pode ser acessada diretamente como MyInterface.hello,
    // sem precisar criar nenhum objeto.
    public String hello = "Hello";

    // Assinatura do método: define o "contrato".
    // Qualquer classe que implementar MyInterface
    // DEVE fornecer um corpo para este método.
    public void sayHello();
}
```

A palavra-chave `interface` substitui `class` na declaração. Assim como classes, interfaces podem ter escopo `public` ou de pacote (sem modificador).

Para acessar a constante diretamente pela interface, sem instanciar nada:

```java
// Acesso à constante da interface — igual ao acesso a um campo 'static' de classe.
// Nenhum objeto precisa ser criado para isso.
System.out.println(MyInterface.hello);
```

O método `sayHello()`, por outro lado, só pode ser chamado por meio de um objeto de uma classe que o implemente. A seção seguinte mostra como fazer isso.

---

## Implementando uma Interface

Para usar uma interface, você precisa de uma classe que a implemente. A palavra-chave `implements` faz essa ligação:

```java
// 'implements MyInterface' avisa o compilador que esta classe
// assume o compromisso de implementar todos os métodos de MyInterface.
public class MyInterfaceImpl implements MyInterface {

    // Implementação obrigatória de 'sayHello()'.
    // A assinatura (nome + parâmetros) deve ser idêntica à da interface.
    // O que vai DENTRO do método é responsabilidade desta classe.
    public void sayHello() {
        // Acessa a constante 'hello' diretamente pela interface.
        System.out.println(MyInterface.hello);
    }
}
```

Regras que o compilador verifica automaticamente:

- Todo método declarado na interface **deve** ser implementado na classe.
- A assinatura (nome + parâmetros) deve ser idêntica.
- As **variáveis/constantes** da interface não precisam ser reimplementadas — elas são herdadas automaticamente.

---

## Instâncias de Interfaces

Uma vez que uma classe implementa uma interface, objetos dessa classe podem ser referenciados pelo tipo da interface:

```java
// A variável é do tipo da INTERFACE (MyInterface),
// mas o objeto criado é da CLASSE concreta (MyInterfaceImpl).
// Isso é possível porque MyInterfaceImpl "é um" MyInterface.
MyInterface myInterface = new MyInterfaceImpl();

// Chama o método pela referência da interface.
// O Java sabe, em tempo de execução, qual implementação usar.
myInterface.sayHello();
```

> **Importante:** você não pode instanciar uma interface diretamente.
> `new MyInterface()` causa erro de compilação. Sempre instancie
> uma classe concreta e, se quiser, referencie-a pelo tipo da interface.

---

## Implementando Múltiplas Interfaces

Ao contrário das classes (que só herdam de uma), uma classe pode implementar quantas interfaces quiser. Basta listá-las após `implements`, separadas por vírgula:

```java
// Esta classe assina dois contratos ao mesmo tempo:
// MyInterface (exige sayHello) e MyOtherInterface (exige sayGoodbye).
public class MyInterfaceImpl implements MyInterface, MyOtherInterface {

    // Implementação exigida por MyInterface.
    public void sayHello() {
        System.out.println("Hello");
    }

    // Implementação exigida por MyOtherInterface.
    public void sayGoodbye() {
        System.out.println("Goodbye");
    }
}
```

Se as interfaces estiverem em pacotes diferentes da classe, importe-as normalmente com `import`, como faria com qualquer classe:

```java
// Importações necessárias quando as interfaces estão em pacotes diferentes.
import com.jenkov.package1.MyInterface;
import com.jenkov.package2.MyOtherInterface;

public class MyInterfaceImpl implements MyInterface, MyOtherInterface {
    // ... implementações dos métodos de ambas as interfaces
}
```

Para referência, veja como cada interface é definida:

```java
// Interface 1: exige apenas o método sayHello().
public interface MyInterface {
    public void sayHello();
}
```

```java
// Interface 2: exige apenas o método sayGoodbye().
public interface MyOtherInterface {
    public void sayGoodbye();
}
```

---

## Assinaturas de Métodos Sobrepostas

Se duas interfaces implementadas por uma mesma classe declararem métodos com a **mesma assinatura** (mesmo nome e mesmos parâmetros), o compilador aceita — afinal, a classe só precisa fornecer uma única implementação para aquela assinatura, que satisfaz as duas interfaces ao mesmo tempo.

O problema surge quando as interfaces declaram o mesmo método como `default` com **implementações diferentes**. Nesse caso, o compilador exige que a classe resolva o conflito explicitamente (veja a seção *Herança e Métodos Padrão*).

---

## Quais Tipos Java Podem Implementar Interfaces?

Não são apenas classes concretas que podem implementar interfaces. Os seguintes tipos também podem:

- Classe Java comum (`class`)
- Classe abstrata (`abstract class`)
- Classe aninhada (*nested class*)
- Enum
- Proxy Dinâmico (*Dynamic Proxy*)

---

## Constantes em Interfaces

Uma interface pode declarar constantes. Todo campo declarado em uma interface é implicitamente `public`, `static` e `final` — mesmo que você omita essas palavras-chave:

```java
public interface MyInterface {

    // Equivalente a: public static final int FALSE = 0;
    // As palavras-chave public, static e final são adicionadas
    // automaticamente pelo compilador, mesmo sem você escrevê-las.
    int FALSE = 0;

    // Equivalente a: public static final int TRUE = 1;
    int TRUE = 1;
}
```

> Use constantes em interfaces com moderação. Em geral, é preferível usar
> `enum` para representar conjuntos de valores relacionados.

---

## Métodos de Interface

Todos os métodos declarados em uma interface são implicitamente `public`, mesmo que você omita o modificador. Uma interface pode ter três tipos de métodos:

| Tipo | Disponível desde | Tem implementação? |
|---|---|---|
| Abstrato (padrão) | Java 1.0 | Não — a classe implementadora é obrigada a fornecer |
| `default` | Java 8 | Sim — implementação opcional na classe |
| `static` | Java 8 | Sim — não pode ser sobrescrito |

---

## Métodos Padrão em Interfaces (`default`)

Antes do Java 8, adicionar um novo método a uma interface pública quebrava todas as classes que a implementavam — elas eram obrigadas a implementar o novo método ou deixavam de compilar.

O Java 8 resolveu isso com os **métodos `default`**: um método com implementação embutida na própria interface. Classes que não sobrescrevem o método herdam automaticamente essa implementação padrão.

**O problema sem `default`:**

```java
// Interface original, usada por muitas classes no mundo.
public interface ResourceLoader {
    Resource load(String resourcePath);
}
```

```java
// Classe de um cliente que usa a API.
// Se 'ResourceLoader' ganhar um novo método, esta classe
// vai parar de compilar — ela precisa implementar o método novo.
public class FileLoader implements ResourceLoader {
    public Resource load(String resourcePath) {
        // implementação que lê o recurso a partir de um caminho em String
    }
}
```

**A solução com `default`:**

```java
public interface ResourceLoader {

    // Método original — continua exigindo implementação nas classes.
    Resource load(String resourcePath);

    // Novo método adicionado como 'default'.
    // Classes que já existem NÃO precisam ser alteradas:
    // se não implementarem este método, usarão esta implementação automaticamente.
    // Classes que precisarem de comportamento diferente podem sobrescrever normalmente.
    default Resource load(Path resourcePath) {
        // implementação padrão: carrega o recurso a partir de um Path
        // e retorna o conteúdo em um objeto Resource
    }
}
```

> **Prioridade:** se uma classe fornecer sua própria implementação de um método
> `default`, essa implementação tem precedência sobre a da interface.

---

## Métodos Estáticos em Interfaces

Interfaces também podem conter métodos `static`. Diferente dos métodos abstratos, métodos estáticos **devem** ter implementação na própria interface e **não podem** ser sobrescritos pelas classes:

```java
public interface MyInterface {

    // Método utilitário estático: pertence à interface, não às instâncias.
    // Não pode ser sobrescrito por classes que implementam a interface.
    public static void print(String text) {
        System.out.print(text);
    }
}
```

A chamada é feita diretamente pelo nome da interface, igual a um método estático de classe:

```java
// Não é necessário criar nenhum objeto para chamar um método estático de interface.
MyInterface.print("Hello static method!");
```

Métodos estáticos em interfaces são úteis para agrupar **funções utilitárias** relacionadas ao contrato da interface. Por exemplo, uma interface `Vehicle` poderia ter um método `printVehicle(Vehicle v)` que formata e exibe os dados de qualquer veículo.

---

## Interfaces e Herança

Assim como classes, interfaces podem herdar de outras interfaces, usando `extends`:

```java
// Superinterface: define o comportamento base.
public interface MySuperInterface {
    public void sayHello();
}
```

```java
// Subinterface: herda sayHello() de MySuperInterface
// e acrescenta sayGoodbye().
// Uma classe que implementar MySubInterface deverá implementar
// os dois métodos: sayHello() E sayGoodbye().
public interface MySubInterface extends MySuperInterface {
    public void sayGoodbye();
}
```

**Diferença importante entre herança de classes e de interfaces:** enquanto uma classe só pode estender uma única classe, uma interface pode estender **múltiplas interfaces** ao mesmo tempo:

```java
// MySubInterface herda de duas superinterfaces.
// Uma classe que a implementar deverá implementar:
// - todos os métodos de SuperInterface1
// - todos os métodos de SuperInterface2
// - sayItAll(), declarado aqui
public interface MySubInterface extends SuperInterface1, SuperInterface2 {
    public void sayItAll();
}
```

---

## Herança e Métodos Padrão

Quando duas interfaces possuem um método `default` com a **mesma assinatura**, o compilador não consegue decidir qual implementação usar — e exige que a classe resolva o conflito explicitamente:

```java
public interface A {
    // Método default em A
    default void hello() { System.out.println("Hello from A"); }
}

public interface B {
    // Mesmo nome e parâmetros que A.hello() — conflito!
    default void hello() { System.out.println("Hello from B"); }
}

// O compilador rejeita esta classe sem uma implementação explícita de hello(),
// pois não sabe se deve usar A.hello() ou B.hello().
public class C implements A, B {

    // Solução obrigatória: a classe sobrescreve hello() e decide o comportamento.
    // Opcionalmente, pode delegar para uma das interfaces usando super:
    //   A.super.hello();  — chama a implementação de A
    //   B.super.hello();  — chama a implementação de B
    @Override
    public void hello() {
        A.super.hello(); // escolhe explicitamente a implementação de A
    }
}
```

A mesma regra vale quando uma **subinterface** herda de múltiplas superinterfaces com métodos `default` em conflito.

---

## Interfaces e Polimorfismo

Interfaces são o principal recurso de Java para implementar **polimorfismo**: a capacidade de tratar objetos de tipos diferentes de forma uniforme, desde que compartilhem a mesma interface.

Imagine um modelo com classes de veículos e motoristas. Você quer que todos esses objetos possam ser salvos em banco de dados e serializados para XML/JSON. Em vez de colocar esses métodos em uma superclasse (o que misturaria responsabilidades), você define interfaces:

```java
// Contrato de persistência: qualquer classe que implemente Storable
// promete saber como se salvar.
public interface Storable {
    public void store();
}
```

```java
// Contrato de serialização: qualquer classe que implemente Serializable
// promete saber como gerar XML e JSON de si mesma.
public interface Serializable {
    public void serializeToXML(Writer writer);
    public void serializeToJSON(Writer writer);
}
```

Com isso, qualquer classe (`Car`, `Truck`, `Driver`…) que implementar essas interfaces pode ser tratada polimorficamente:

```java
Car car = new Car(); // Car implementa Storable e Serializable

// Faz o cast para Storable: não importa que 'car' seja um Car específico —
// o que importa é que ele cumpre o contrato de Storable.
Storable storable = (Storable) car;
storable.store();

// Faz o cast para Serializable: mesma lógica.
// O código a seguir funcionaria igual para qualquer objeto
// que implemente Serializable, seja Car, Truck ou Driver.
Serializable serializable = (Serializable) car;
serializable.serializeToXML(new FileWriter("car.xml"));
serializable.serializeToJSON(new FileWriter("car.json"));
```

Essa abordagem mantém as classes coesas (cada uma cuida do seu próprio domínio) e o código de infraestrutura desacoplado dos tipos concretos.

---

## Interfaces Genéricas

Interfaces genéricas permitem que você parametrize o tipo com o qual a interface trabalha, eliminando a necessidade de *casts* manuais e tornando o código mais seguro em tempo de compilação.

**Problema: sem generics, o tipo de retorno é `Object`**

```java
// Interface sem generics: produce() só pode retornar Object.
// Quem chamar este método precisará fazer cast manualmente.
public interface MyProducer {
    public Object produce();
}
```

```java
// Implementação concreta que produz Cars.
public class CarProducer implements MyProducer {
    public Object produce() {
        return new Car(); // retorna Car, mas declarado como Object
    }
}
```

```java
MyProducer carProducer = new CarProducer();

// Cast manual obrigatório: o compilador não sabe que produce() retorna Car.
// Se a implementação mudar e retornar outro tipo, o erro só aparece em tempo de execução.
Car car = (Car) carProducer.produce();
```

**Solução: interface genérica com parâmetro de tipo `<T>`**

```java
// 'T' é um parâmetro de tipo: um "curinga" que será substituído
// pelo tipo real quando a interface for usada.
public interface MyProducer<T> {

    // Agora produce() retorna T — o tipo que for especificado na hora de usar.
    public T produce();
}
```

**Opção 1: deixar o tipo aberto na classe implementadora**

A classe também declara `<T>` e repassa o parâmetro para a interface. O tipo concreto só é fixado na hora de instanciar:

```java
// CarProducer<T> mantém o tipo genérico aberto.
// Quem criar um CarProducer poderá especificar qualquer T —
// mas atenção: a implementação sempre retorna Car (veja o problema abaixo).
public class CarProducer<T> implements MyProducer<T> {

    @Override
    public T produce() {
        return (T) new Car(); // cast inseguro: T pode não ser Car em tempo de execução
    }
}
```

```java
// Uso correto: T especificado como Car — funciona como esperado.
MyProducer<Car> myCarProducer = new CarProducer<Car>();
Car car = myCarProducer.produce(); // sem cast manual necessário
```

```java
// Uso INCORRETO: T especificado como String, mas produce() retorna Car.
// O compilador aceita, mas lança ClassCastException em tempo de execução.
MyProducer<String> myStringProducer = new CarProducer<String>();
String produce1 = myStringProducer.produce(); // ERRO em execução!
```

**Opção 2 (recomendada): fixar o tipo já na implementação**

Ao implementar a interface, a classe já especifica o tipo concreto. Isso elimina a possibilidade de uso incorreto:

```java
// CarProducer implementa MyProducer<Car> — o tipo está travado em Car.
// Não é possível criar um CarProducer<String> acidentalmente.
public class CarProducer implements MyProducer<Car> {

    @Override
    public Car produce() {
        // Retorna Car diretamente — sem cast, sem risco de ClassCastException.
        return new Car();
    }
}
```

```java
// Uso limpo e seguro: o compilador sabe que produce() retorna Car.
MyProducer<Car> myCarProducer = new CarProducer();
Car car = myCarProducer.produce(); // sem cast, sem surpresas em execução
```

---

## Interfaces Funcionais

A partir do Java 8, uma **interface funcional** é qualquer interface que declare exatamente **um único método abstrato** (não-`default`, não-`static`). Essa restrição permite que a interface seja implementada por uma **expressão lambda**, tornando o código mais conciso:

```java
// @FunctionalInterface é opcional, mas recomendado:
// faz o compilador verificar que a interface realmente tem
// apenas um método abstrato. Se você adicionar um segundo por engano, vira erro.
@FunctionalInterface
public interface MyProducer<T> {
    public T produce(); // único método abstrato — o requisito para ser funcional
}
```

```java
// Sem lambda (modo tradicional): implementação anônima explícita.
MyProducer<String> classicProducer = new MyProducer<String>() {
    @Override
    public String produce() {
        return "Hello!";
    }
};

// Com lambda (modo funcional): o compilador sabe que a lambda
// implementa o único método abstrato da interface — produce().
// O código fica significativamente mais curto e legível.
MyProducer<String> lambdaProducer = () -> "Hello!";

System.out.println(lambdaProducer.produce()); // imprime: Hello!
```

Interfaces funcionais são a base das **Streams**, do `Optional` e de toda a API funcional do Java 8+. As mais usadas da biblioteca padrão são `Runnable`, `Callable`, `Comparator`, `Function<T,R>`, `Predicate<T>` e `Supplier<T>`.