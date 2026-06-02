# Enums

Um Enum é um tipo especial do Java usado para definir coleções de constantes. Mais precisamente, um tipo enum do é um tipo especial de classe. Um enum pode conter constantes, métodos, etc. Os enums foram adicionados no Java 5.

### Exemplo de Enum

```java
public enum Level {
    HIGH,
    MEDIUM,
    LOW
}
```

Observe a palavra-chave `enum`, que é usada no lugar de `class` ou `interface`. A palavra-chave `enum` do Java sinaliza para o compilador que essa definição de tipo é um enum.

Você pode se referir às constantes no enum acima desta forma:

```java
Level level = Level.HIGH;
```

Observe como a variável `level` é do tipo `Level`, que é o tipo enum do Java definido no exemplo acima. A variável `level` pode assumir uma das constantes do enum `Level` como valor (`HIGH`, `MEDIUM` ou `LOW`). Neste caso, `level` é definida como `HIGH`.

### Enums em Instruções if

Como os enums do Java são constantes, você frequentemente terá que comparar uma variável que aponta para uma constante de enum com as constantes possíveis no tipo enum. Aqui está um exemplo de uso de um enum do Java em uma instrução `if`:

```java
Level level = ... //assign some Level constant to it

if( level == Level.HIGH) {

} else if( level == Level.MEDIUM) {

} else if( level == Level.LOW) {

}
```

Este código compara a variável `level` com cada uma das constantes de enum possíveis no enum `Level`.

Se um dos valores do enum ocorrer com mais frequência do que os outros, verificar esse valor na primeira instrução `if` resultará em um melhor desempenho, pois menos comparações serão executadas em média. No entanto, essa não é uma grande diferença, a menos que as comparações sejam executadas em grande quantidade.

### Enums em Instruções switch

Se os seus tipos enum do Java contiverem muitas constantes e você precisar verificar uma variável em relação aos valores, conforme mostrado na seção anterior, usar uma instrução `switch` do Java pode ser uma boa ideia.

Você pode usar enums em instruções switch desta forma:

```java
Level level = ... //assign some Level constant to it

switch (level) {
    case HIGH   : ...; break;
    case MEDIUM : ...; break;
    case LOW    : ...; break;
}
```

Substitua o `...` pelo código a ser executado se a variável `level` corresponder ao valor da constante `Level` fornecido. O código pode ser uma operação simples em Java, uma chamada de método, etc.

### Iteração de Enum

Você pode obter um array de todos os valores possíveis de um tipo enum do Java chamando seu método estático `values()`. Todos os tipos enum recebem um método estático `values()` automaticamente pelo compilador Java. Aqui está um exemplo de iteração de todos os valores de um enum:

```java
for (Level level : Level.values()) {
    System.out.println(level);
}
```

A execução deste código Java imprimiria todos os valores do enum. Aqui está a saída:

```
HIGH
MEDIUM
LOW
```

Observe como os próprios nomes das constantes são impressos. Esta é uma área em que os enums do Java são diferentes das constantes `static final`.

### Enum toString()

Uma classe enum obtém automaticamente um método `toString()` na classe quando compilada. O método `toString()` retorna um valor de string com o nome da instância de enum fornecida. Aqui está um exemplo:

```java
String levelText = Level.HIGH.toString();
```

O valor da variável `levelText` após a execução da instrução acima será o texto `HIGH`.

### Impressão de Enum

Se você imprimir um enum, assim:

```java
System.out.println(Level.HIGH);
```

Então o método `toString()` será chamado por baixo dos panos, de modo que o valor que será impresso é o nome textual da instância do enum. Em outras palavras, no exemplo acima, o texto `HIGH` teria sido impresso.

### Enum valueOf()

Uma classe enum obtém automaticamente um método estático `valueOf()` na classe quando compilada. O método `valueOf()` pode ser usado para obter uma instância da classe enum para um determinado valor de String. Aqui está um exemplo:

```java
Level level = Level.valueOf("HIGH");
```

A variável `level` apontará para `Level.HIGH` após a execução desta linha.

### Campos de Enum

Você pode adicionar campos a um enum do Java. Assim, cada valor constante do enum recebe esses campos. Os valores dos campos devem ser fornecidos ao construtor do enum ao definir as constantes. Aqui está um exemplo:

```java
public enum Level {
    HIGH  (3),  //calls constructor with value 3
    MEDIUM(2),  //calls constructor with value 2
    LOW   (1)   //calls constructor with value 1
    ;           // semicolon needed when fields / methods follow

    private final int levelCode;

    private Level(int levelCode) {
        this.levelCode = levelCode;
    }
}
```

Observe como o enum do Java no exemplo acima possui um construtor que aceita um `int`. O construtor do enum define o campo `int`. Quando os valores constantes do enum são definidos, um valor `int` é passado para o construtor do enum.

O construtor do enum deve ser `private`. Você não pode usar construtores `public` ou `protected` em um enum do Java. Se você não especificar um modificador de acesso, o construtor do enum será implicitamente `private`.

### Métodos de Enum

Você também pode adicionar métodos a um enum do Java. Aqui está um exemplo:

```java
public enum Level {
    HIGH  (3),  //calls constructor with value 3
    MEDIUM(2),  //calls constructor with value 2
    LOW   (1)   //calls constructor with value 1
    ;           // semicolon needed when fields / methods follow

    private final int levelCode;

    Level(int levelCode) {
        this.levelCode = levelCode;
    }

    public int getLevelCode() {
        return this.levelCode;
    }
}
```

Você chama um método de enum do Java por meio de uma referência a um dos valores constantes. Aqui está um exemplo de chamada de método de enum do Java:

```java
Level level = Level.HIGH;

System.out.println(level.getLevelCode());
```

Este código imprimiria o valor `3`, que é o valor do campo `levelCode` para a constante de enum `HIGH`.

Você não está restrito a métodos getter e setter simples. Você também pode criar métodos que realizam cálculos com base nos valores dos campos da constante do enum. Se os seus campos não forem declarados como `final`, você poderá até modificar os valores dos campos (embora isso possa não ser uma ideia tão boa, considerando que os enums devem ser constantes).

### Métodos Abstratos de Enum

Também é possível que uma classe enum do Java possua métodos abstratos. Se uma classe enum tiver um método abstrato, cada instância da classe enum deverá implementá-lo. Aqui está um exemplo de método abstrato em um enum do Java:

```java
public enum Level {
    HIGH {
        @Override
        public String asLowerCase() {
            return HIGH.toString().toLowerCase();
        }
    },
    MEDIUM {
        @Override
        public String asLowerCase() {
            return MEDIUM.toString().toLowerCase();
        }
    },
    LOW {
        @Override
        public String asLowerCase() {
            return LOW.toString().toLowerCase();
        }
    };

    public abstract String asLowerCase();
}
```

Observe a declaração do método abstrato na parte inferior da classe enum. Observe também como cada instância do enum (cada constante) define sua própria implementação desse método abstrato. O uso de um método abstrato é útil quando você precisa de uma implementação diferente de um método para cada instância de um enum do Java.

### Enum Implementando Interface

Um Enum do Java pode implementar uma Interface Java, caso você ache que isso faça sentido na sua situação. Aqui está um exemplo de um Enum do Java implementando uma interface:

```java
public enum EnumImplementingInterface implements MyInterface {
    FIRST("First Value"),
    SECOND("Second Value");

    private String description = null;

    private EnumImplementingInterface(String desc){
        this.description = desc;
    }

    @Override
    public String getDescription() {
        return this.description;
    }
}
```

É o método `getDescription()` que vem da interface `MyInterface`.

A implementação de uma interface com um Enum pode ser usada para implementar um conjunto de constantes `Comparator` diferentes que podem ser usadas para ordenar coleções de objetos. A ordenação de objetos no Java é explicada em mais detalhes no Tutorial de Ordenação de Coleções do Java (Java Collection Sorting Tutorial).

### EnumSet

O Java contém uma implementação especial de `Set` chamada `EnumSet`, que pode armazenar enums de forma mais eficiente do que as implementações padrão de `Set` do Java. Aqui está como você cria uma instância de um `EnumSet`:

```java
EnumSet<Level> enumSet = EnumSet.of(Level.HIGH, Level.MEDIUM);
```

Uma vez criado, você pode usar o `EnumSet` como qualquer outro `Set`.

### EnumMap

O Java também contém uma implementação especial de `Map` que pode usar instâncias de enum do Java como chaves. Aqui está um exemplo de `EnumMap` do Java:

```java
EnumMap<Level, String> enumMap = new EnumMap<Level, String>(Level.class);

enumMap.put(Level.HIGH  , "High level");
enumMap.put(Level.MEDIUM, "Medium level");
enumMap.put(Level.LOW   , "Low level");

String levelValue = enumMap.get(Level.HIGH);
```

### Detalhes Diversos de Enum

Os enums do Java estendem a classe `java.lang.Enum` implicitamente, portanto, seus tipos enum não podem estender outra classe.

Se um enum do Java contiver campos e métodos, a definição dos campos e métodos deve sempre vir após a lista de constantes no enum. Além disso, a lista de constantes do enum deve ser encerrada por um ponto e vírgula `;`.
