# Records

Um Record é um tipo especial de classe Java que possui uma sintaxe concisa para definir classes imutáveis compostas apenas por dados. Instâncias Record podem ser úteis para armazenar registros retornados de uma consulta ao banco de dados, registros retornados de uma chamada de serviço remoto, registros lidos de um arquivo CSV ou semelhantes de casos de uso.

Um Record consiste em um ou mais campos de dados que correspondem a variáveis de membro em uma classe Java comum. O compilador Java gera automaticamente os métodos getter, toString(), hashCode() e equals() para esses campos de dados, para que você não precise escrever esse código clichê por conta própria. Como um Java Record é imutável, nenhum método setter é gerado.

## Sintaxe

A sintaxe do Record é bastante simples. Aqui está um exemplo modelando um Vehicle:

```java
public record Vehicle(String brand, String licensePlate) {}

```

Observe como o exemplo usa record em vez de class. A palavra-chave record é o que diz ao compilador que essa definição de tipo é um record.

Observe também como o record definido no exemplo não possui definições explícitas de campos Java. O record é definido unicamente pelo que se parece com um construtor Java comum. Esse construtor é, na verdade, suficiente para definir um Record. Os dois parâmetros definidos no construtor do Record dizem ao compilador que o tipo de record possui dois campos - um campo por parâmetro no construtor. O compilador Java então gera os campos correspondentes, métodos getter e um método hashCode() e equals().

## Usando um Record

Você usa um Record da mesma forma que usa outras classes Java - criando instâncias do tipo record usando a palavra-chave new do Java. Aqui está um exemplo de uso do tipo Record Vehicle definido na seção anterior:

```java
public class RecordsExample {
    public static void main(String[] args) {
        Vehicle vehicle = new Vehicle("Mercedes", "UX 1238 A95");

        System.out.println( vehicle.brand() );
        System.out.println( vehicle.licensePlate() );

        System.out.println( vehicle.toString() );
    }
}

```

Observe como o compilador gerou um método brand(), um método licensePlate() e um método toString() para nós. A saída gerada a partir do exemplo acima seria:

```text
Mercedes
UX 1238 A95
Vehicle[brand=Mercedes, licensePlate=UX 1238 A95]

```

## Um Record é Final

A definição de um tipo Record é final, significando que você não pode criar subclasses (subrecords) de um tipo Java Record.

## Múltiplos Construtores

É possível que uma definição de tipo Java Record contenha múltiplos construtores. Aqui está um exemplo de Java Record que define um construtor extra para o tipo de record Vehicle mostrado anteriormente neste tutorial de Java Record:

```java
public record Vehicle(String brand, String licensePlate) {

    public Vehicle(String brand) {
        this(brand, null);
    }
}

```

O construtor extra é declarado dentro do corpo da declaração do Record Vehicle. Observe como o construtor extra chama o construtor padrão do Record Vehicle. Isso é exigido pelo compilador Java, para que o compilador saiba quais parâmetros de construtor no construtor extra correspondem a quais parâmetros no construtor padrão.

Você pode adicionar quantos construtores extras fizerem sentido para a sua definição concreta de Java Record.

## Métodos de Instância

Você pode adicionar métodos de instância a uma definição de Java Record - exatamente como faria com uma classe Java comum. Aqui está um exemplo da definição do Java Record Vehicle de seções anteriores com um método de instância chamado brandAsLowerCase() adicionado:

```java
public record Vehicle(String brand, String licensePlate) {

    public String brandAsLowerCase() {
        return brand().toLowerCase();
    }
}

```

Observe como o método brandAsLowerCase() chama o método brand() gerado automaticamente internamente.

## Métodos Estáticos

Também é possível adicionar métodos estáticos a uma definição de Java Record. Aqui está um exemplo da definição anterior do Java Record Vehicle com um método estático adicionado:

```java
public record Vehicle(String brand, String licensePlate) {

    public static String brandAsUpperCase(Vehicle vehicle) {
        return vehicle.brand.toUpperCase();
    }
}

```
