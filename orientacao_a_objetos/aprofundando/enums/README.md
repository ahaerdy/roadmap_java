# Enums

Um Enum é um tipo especial do Java usado para definir coleções de constantes. Mais precisamente, um tipo enum é um tipo especial de classe. Um enum pode conter constantes, métodos, etc. Os enums foram adicionados no Java 5.

### Exemplo de Enum

```java
// Definição de um Enum chamado 'Level'
public enum Level {
    // Coleção de constantes disponíveis (por convenção, escritas em letras maiúsculas)
    HIGH,
    MEDIUM,
    LOW
}

```

Observe a palavra-chave `enum`, que é usada no lugar de `class` ou `interface`. A palavra-chave `enum` do Java sinaliza para o compilador que essa declaração de tipo é um enum.

Podemos referenciar constantes no enum acima desta forma:

```java
// Declaração de uma variável do tipo 'Level' recebendo a constante 'HIGH'
Level level = Level.HIGH;

```

Observe como a variável `level` é do tipo `Level`, que é o tipo enum do Java definido no exemplo acima. A variável `level` pode assumir uma das constantes do enum `Level` como valor (`HIGH`, `MEDIUM` ou `LOW`). Neste caso, `level` é definida como `HIGH`.

### Enums em Instruções if

Como os enums do Java são constantes, você frequentemente terá que comparar uma variável que aponta para uma constante de enum com as constantes possíveis no tipo enum. Aqui está um exemplo de uso de um enum do Java em uma instrução `if`:

```java
// Simulação: Variável 'level' inicializada com alguma das constantes existentes
Level level = Level.MEDIUM; 

// Comparação direta de enums utilizando o operador de igualdade referencial (==)
if (level == Level.HIGH) {
    // Código executado se a variável for igual à constante HIGH
} else if (level == Level.MEDIUM) {
    // Código executado se a variável for igual à constante MEDIUM
} else if (level == Level.LOW) {
    // Código executado se a variável for igual à constante LOW
}

```

Este código compara a variável `level` com cada uma das constantes de enum possíveis no enum `Level`.

Se um dos valores do enum ocorrer com mais frequência do que os outros, verificar esse valor na primeira instrução `if` resultará em um melhor desempenho, pois menos comparações serão executadas em média. No entanto, essa não é uma grande diferença, a menos que as comparações sejam executadas em grande quantidade.

### Enums em Instruções switch

Se os seus tipos enum do Java contiverem muitas constantes e você precisar verificar uma variável em relação aos valores, conforme mostrado na seção anterior, usar uma instrução `switch` do Java pode ser uma boa ideia.

Você pode usar enums em instruções switch desta forma:

```java
// Simulação: Variável 'level' inicializada com alguma das constantes existentes
Level level = Level.HIGH; 

// O bloco switch avalia o valor contido na variável do tipo Enum
switch (level) {
    // Nos 'cases', não é necessário (e nem permitido) utilizar o prefixo 'Level.'
    case HIGH: 
        // ... substitua por código a ser executado para o caso HIGH
        break; // Interrompe a execução para não entrar nos blocos abaixo
    case MEDIUM: 
        // ... substitua por código a ser executado para o caso MEDIUM
        break; 
    case LOW: 
        // ... substitua por código a ser executado para o caso LOW
        break; 
}

```

Substitua o `...` pelo código a ser executado se a variável `level` corresponder ao valor da constante `Level` fornecido. O código pode ser uma operação simples em Java, uma chamada de método, etc.

### Iteração de Enum

Você pode obter um array de todos os valores possíveis de um tipo enum do Java chamando seu método estático `values()`. Todos os tipos enum recebem um método estático `values()` automaticamente pelo compilador Java. Aqui está um exemplo de iteração de todos os valores de um enum:

```java
// O método .values() retorna um array com todas as constantes do Enum na ordem em que foram declaradas
for (Level level : Level.values()) {
    // Imprime o nome de cada constante encontrada no Enum (ex: HIGH, MEDIUM, LOW)
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
// O método .toString() converte a instância do Enum em sua representação textual exata ("HIGH")
String levelText = Level.HIGH.toString();

```

O valor da variável `levelText` após a execução da instrução acima será o texto `HIGH`.

### Impressão de Enum

Se você imprimir um enum, assim:

```java
// Passar o Enum diretamente para o println invoca implicitamente o método .toString() por baixo dos panos
System.out.println(Level.HIGH);

```

Então o método `toString()` será chamado por baixo dos panos, de modo que o valor que será impresso é o nome textual da instância do enum. Em outras palavras, no exemplo acima, the texto `HIGH` teria sido impresso.

### Enum valueOf()

Uma classe enum obtém automaticamente um método estático `valueOf()` na classe quando compilada. O método `valueOf()` pode ser usado para obter uma instância da classe enum para um determinado valor de String. Aqui está um exemplo:

```java
// O método estático .valueOf() faz o inverso: converte um texto String exato de volta na constante do Enum correspondente.
// Nota: Se o texto passado não corresponder exatamente a uma constante (ex: "high"), lançará um IllegalArgumentException.
Level level = Level.valueOf("HIGH");

```

A variável `level` apontará para `Level.HIGH` após a execução desta linha.

### Campos de Enum

Você pode adicionar campos a um enum do Java. Assim, cada valor constante do enum recebe esses campos. Os valores dos campos devem ser fornecidos ao construtor do enum ao definir as constantes. Aqui está um exemplo:

```java
public enum Level {
    // Declaração das constantes do Enum invocando o construtor personalizado passando o inteiro correspondente
    HIGH  (3),  // Chama o construtor passando o valor 3
    MEDIUM(2),  // Chama o construtor passando o valor 2
    LOW   (1)   // Chama o construtor passando o valor 1
    ;           // O ponto e vírgula é OBRIGATÓRIO aqui porque há campos e construtores declarados abaixo das constantes

    // Atributo interno privado e imutável para armazenar o código numérico de cada nível
    private final int levelCode;

    // Construtor do Enum. Obrigatoriamente privado (private). Não é permitido usar construtores public ou protected.
    private Level(int levelCode) {
        this.levelCode = levelCode; // Atribui o código recebido ao atributo da constante correspondente
    }
}

```

Observe como o enum do Java no exemplo acima possui um construtor que aceita um `int`. O construtor do enum define o campo `int`. Quando os valores constantes do enum são definidos, um valor `int` é passado para o construtor do enum.

O construtor do enum deve ser `private`. Você não pode usar construtores `public` ou `protected` em um enum do Java. Se você não especificar um modificador de acesso, o construtor do enum será implicitamente `private`.

### Métodos de Enum

Você também pode adicionar métodos a um enum do Java. Aqui está um exemplo:

```java
public enum Level {
    HIGH  (3),  // Chama o construtor passando o valor 3
    MEDIUM(2),  // Chama o construtor passando o valor 2
    LOW   (1)   // Chama o construtor passando o valor 1
    ;           // Ponto e vírgula separando as constantes dos membros da classe

    private final int levelCode;

    // Construtor privado interno
    Level(int levelCode) {
        this.levelCode = levelCode;
    }

    // Método público comum (Getter) para expor e retornar o valor de 'levelCode' externamente
    public int getLevelCode() {
        return this.levelCode;
    }
}

```

Você chama um método de enum do Java por meio de uma referência a um dos valores constantes. Aqui está um exemplo de chamada de método de enum do Java:

```java
// Atribui a constante HIGH para a variável 'level'
Level level = Level.HIGH;

// Executa o método público .getLevelCode() a partir da instância obtida e exibe no console (imprimirá 3)
System.out.println(level.getLevelCode());

```

Este código imprimiria o valor `3`, que é o valor do campo `levelCode` para a constante de enum `HIGH`.

Você não está restrito a métodos getter e setter simples. Você também pode criar métodos que realizam cálculos com base nos valores dos campos da constante do enum. Se os seus campos não forem declarados como `final`, você poderá até modificar os valores dos campos (embora isso possa não ser uma ideia tão boa, considerando que os enums devem ser constantes).

### Métodos Abstratos de Enum

Também é possível que uma classe enum do Java possua métodos abstratos. Se uma classe enum tiver um método abstrato, cada instância da classe enum deverá implementá-lo. Aqui está um exemplo de método abstrato em um enum do Java:

```java
public enum Level {
    // Cada constante passa a funcionar como uma espécie de classe anônima interna,
    // sendo obrigada a abrir chaves e implementar o método abstrato declarado na classe pai do Enum.
    HIGH {
        @Override
        public String asLowerCase() {
            // Implementação específica para a constante HIGH: converte seu nome para letras minúsculas ("high")
            return HIGH.toString().toLowerCase();
        }
    },
    MEDIUM {
        @Override
        // Sobrescrita obrigatória do método abstrato
        public String asLowerCase() {
            // Retorna "medium"
            return MEDIUM.toString().toLowerCase();
        }
    },
    LOW {
        @Override
        // Sobrescrita obrigatória do método abstrato
        public String asLowerCase() {
            // Retorna "low"
            return LOW.toString().toLowerCase();
        }
    }; // Ponto e vírgula que sinaliza o final das declarações das constantes

    // Assinatura do método abstrato. Todas as constantes acima são forçadas a fornecer sua própria implementação.
    public abstract String asLowerCase();
}

```

Observe a declaração do método abstrato na parte inferior da classe enum. Observe também como cada instância do enum (cada constante) define sua própria implementação desse método abstrato. O uso de um método abstrato é útil quando você precisa de uma implementação diferente de um método para cada instância de um enum do Java.

### Enum Implementando Interface

Um Enum do Java pode implementar uma Interface Java, caso você ache que isso faça sentido na sua situação. Aqui está um exemplo de um Enum do Java implementando uma interface:

```java
// O Enum usa a cláusula padrão 'implements' para assinar o contrato com uma Interface Java
public enum EnumImplementingInterface implements MyInterface {
    // Definição das constantes enviando uma String descritiva para o construtor personalizado
    FIRST("First Value"),
    SECOND("Second Value");

    // Atributo interno para armazenar o dado
    private String description = null;

    // Construtor privado para inicialização do campo descritivo
    private EnumImplementingInterface(String desc){
        this.description = desc;
    }

    // Sobrescrita obrigatória do método determinado pelo contrato da interface 'MyInterface'
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
// O EnumSet usa internamente vetores de bits (bit vectors), o que o torna incrivelmente rápido e leve em memória.
// O método estático .of() cria um conjunto contendo apenas as duas constantes fornecidas nos argumentos.
EnumSet<Level> enumSet = EnumSet.of(Level.HIGH, Level.MEDIUM);

```

Uma vez criado, você pode usar o `EnumSet` como qualquer outro `Set`.

### EnumMap

O Java também contém uma implementação especial de `Map` que pode usar instâncias de enum do Java como chaves. Aqui está um exemplo de `EnumMap` do Java:

```java
// O EnumMap armazena chaves de um tipo Enum específico. É altamente otimizado e requer a passagem do tipo da classe (.class) em seu construtor.
EnumMap<Level, String> enumMap = new EnumMap<Level, String>(Level.class);

// Inserindo chaves do tipo Enum e associando a valores do tipo String comuns
enumMap.put(Level.HIGH  , "High level");
enumMap.put(Level.MEDIUM, "Medium level");
enumMap.put(Level.LOW   , "Low level");

// Recuperação de dados de forma extremamente performática passando a constante do Enum (retornará "High level")
String levelValue = enumMap.get(Level.HIGH);

```

### Detalhes Diversos de Enum

Os enums do Java estendem a classe `java.lang.Enum` implicitamente, portanto, seus tipos enum não podem estender outra classe.

Se um enum do Java contiver campos e métodos, a definição dos campos e métodos deve sempre vir após a lista de constantes no enum. Além disso, a lista de constantes do enum deve ser encerrada por um ponto e vírgula `;`.