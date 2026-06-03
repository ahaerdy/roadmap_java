# Records

Um **Record** é um tipo especial de classe do Java que possui uma sintaxe concisa para definir classes imutáveis compostas apenas por dados.

Instâncias de um Record são ideais para:

* Armazenar registros retornados de uma consulta ao banco de dados.
* Capturar dados de uma chamada de serviço remoto (APIs).
* Representar linhas lidas de um arquivo CSV.
* Qualquer cenário onde o foco seja puramente o transporte de dados, e não o comportamento.

Um Record consiste em um ou mais campos de dados que correspondem a variáveis de membro em uma classe Java comum. O compilador Java gera automaticamente os métodos getter, `toString()`, `hashCode()` e `equals()` para esses campos de dados, poupando você de escrever todo aquele código clichê (*boilerplate*). Como um Java Record é intrinsecamente **imutável**, nenhum método setter é gerado.

---

## Sintaxe

A sintaxe do Record é bastante direta. Veja abaixo o exemplo de modelagem de um `Vehicle`:

```java
// A palavra-chave 'record' substitui 'class' e define os componentes imutáveis diretamente no cabeçalho
public record Vehicle(String brand, String licensePlate) {
    // O corpo pode ficar vazio {} porque o compilador cria os campos privados,
    // o construtor padrão, os métodos de acesso (getters), equals(), hashCode() e toString() sozinhos!
}

```

Observe como o exemplo usa `record` em vez de `class`. A palavra-chave `record` é o que diz ao compilador que essa definição de tipo é um record.

Note também que o record definido não possui definições explícitas de campos Java. Ele é definido unicamente pelo que se parece com um construtor Java comum. Esse construtor é, na verdade, suficiente para definir um Record. Os dois parâmetros definidos no construtor do Record dizem ao compilador que o tipo possui dois campos — um campo por parâmetro. O compilador Java então cuida do resto do trabalho pesado.

---

## Usando um Record

Você usa um Record da mesma forma que usa outras classes Java: criando instâncias do tipo record usando a palavra-chave `new`.

```java
public class RecordsExample {
    public static void main(String[] args) {
        // Criando uma instância do Record usando o construtor gerado automaticamente
        Vehicle vehicle = new Vehicle("Mercedes", "UX 1238 A95");

        // Nos Records, os métodos de acesso NÃO usam o prefixo 'get'. 
        // Eles têm o mesmo nome do próprio campo: brand() e licensePlate()
        System.out.println( vehicle.brand() );        // Imprime a marca
        System.out.println( vehicle.licensePlate() ); // Imprime a placa

        // O método toString() já vem formatado de forma limpa e legível por padrão
        System.out.println( vehicle.toString() );
    }
}

```

Observe como o compilador gerou um método `brand()`, um método `licensePlate()` e um método `toString()` para nós. A saída gerada a partir do exemplo acima seria:

```text
Mercedes
UX 1238 A95
Vehicle[brand=Mercedes, licensePlate=UX 1238 A95]

```

---

## Um Record é Final

> **Regra Importante:** A definição de um tipo Record é implicitamente `final`. Isso significa que você não pode criar subclasses (subrecords) a partir de um tipo Java Record, garantindo a imutabilidade e a segurança do seu modelo de dados.

---

## Múltiplos Construtores

É perfeitamente possível que uma definição de tipo Java Record contenha múltiplos construtores (sobrecarga), desde que todos eles acabem chamando o construtor principal.

```java
public record Vehicle(String brand, String licensePlate) {

    // Construtor secundário (adicional) que aceita apenas a marca
    public Vehicle(String brand) {
        // Obrigatório: Qualquer construtor alternativo DEVE delegar a inicialização
        // para o construtor canônico (o principal) usando a palavra-chave 'this'
        this(brand, null); 
    }
}

```

O construtor extra é declarado dentro do corpo da declaração do Record `Vehicle`. Note como ele obrigatoriamente chama o construtor padrão do Record. Isso é exigido pelo compilador Java para garantir que todos os campos do Record sejam inicializados corretamente, sabendo exatamente quais parâmetros adicionais mapeiam para os originais.

Você pode adicionar quantos construtores extras fizerem sentido para a sua lógica de negócio.

---

## Métodos de Instância

Você pode adicionar métodos de instância a uma definição de Java Record para adicionar comportamentos — exatamente como faria com uma classe Java comum.

```java
public record Vehicle(String brand, String licensePlate) {

    // Adicionando um método de instância customizado para manipular os dados internos
    public String brandAsLowerCase() {
        // Dentro do Record, você pode acessar o dado chamando o método de acesso brand()
        return brand().toLowerCase();
    }
}

```

Observe como o método `brandAsLowerCase()` chama o método `brand()` gerado automaticamente internamente para transformar o texto em letras minúsculas.

---

## Métodos Estáticos

Também é possível adicionar métodos estáticos (métodos de classe) a uma definição de Java Record, muito úteis para utilitários ou fábricas de objetos.

```java
public record Vehicle(String brand, String licensePlate) {

    // Um método estático que recebe um Record como argumento e executa uma lógica
    public static String brandAsUpperCase(Vehicle vehicle) {
        // Como estamos dentro do próprio escopo do Record, podemos acessar o campo privado diretamente (.brand)
        return vehicle.brand.toUpperCase();
    }
}
```