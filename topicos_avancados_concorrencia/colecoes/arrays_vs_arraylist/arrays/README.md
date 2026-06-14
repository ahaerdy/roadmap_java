# Arrays

Um Array em Java é uma coleção de variáveis do mesmo tipo. Por exemplo, um array de `int` é uma coleção de variáveis do tipo `int`. As variáveis no array são ordenadas e cada uma possui um índice. Você verá como indexar em um array mais adiante neste texto. Aqui está uma ilustração de arrays em Java:

<p align="center">
  <img src="000-Midia_e_Anexos/2026-06-14-15-27-26.png" alt="" width="360">
</p>

Arrays em Java são coleções de variáveis do mesmo tipo, ordenadas com um índice.

## Declarando uma Variável de Array em Java

Uma variável de array em Java é declarada exatamente como você declararia uma variável do tipo desejado, exceto pelo fato de você adicionar `[]` após o tipo. Aqui está um exemplo simples de declaração de array em Java:

```java
int[] intArray;

```

Você pode usar un array em Java como um campo (field), campo estático (static field), uma variável local ou parâmetro, assim como qualquer outra variável. Um array é simplesmente uma variação do tipo de dados. Em vez de ser uma única variável daquele tipo, é uma coleção de variáveis daquele tipo.

Aqui estão mais alguns exemplos de declaração de arrays em Java:

```java
String[] stringArray;
MyClass[] myClassArray;

```

A primeira linha declara um array de referências a `String`. A segunda linha declara um array de referências a objetos da classe `MyClass`, que simboliza uma classe que você mesmo criou.

Na verdade, você tem a opção de escolher onde colocar os colchetes `[]` quando declara um array em Java. O primeiro local você já viu: atrás do nome do tipo de dados (ex: `String[]`). O segundo local é após o nome da variável. As seguintes declarações de array em Java são, na verdade, todas válidas:

```java
int[] intArray;
int intArray[];

String[] stringArray;
String stringArray[];

MyClass[] myClassArray;
MyClass myClassArray[];

```

Pessoalmente, prefiro posicionar os colchetes `[]` após o tipo de dados (ex: `String[]`) e não após o nome da variável. Afinal, um array é um tipo especial de dado, então sinto que é mais fácil ler o código quando os colchetes são colocados logo após o tipo de dados na declaração do array.

## Instanciando um Array em Java

Quando você declara uma variável de array em Java, você declara apenas a variável (referência) para o próprio array. A declaração não cria, de fato, um array. Você cria um array assim:

```java
int[] intArray;
intArray = new int[10];

```

Este exemplo cria um array do tipo `int` com espaço para 10 variáveis `int` dentro dele.

O exemplo anterior de array em Java criou um array de `int`, que é um tipo de dado primitivo. Você também pode criar um array de referências a objetos. Por exemplo:

```java
String[] stringArray = new String[10];

```

O Java permite que você crie um array de referências para qualquer tipo de objeto (para instâncias de qualquer classe).

## Literais de Array em Java

A linguagem de programação Java contém um atalho para instanciar arrays de tipos primitivos e strings. Se você já sabe quais valores deseja inserir no array, pode usar um literal de array. Aqui está como um literal de array se parece no código Java:

```java
int[] ints2 = new int[]{ 1,2,3,4,5,6,7,8,9,10 };

```

Note como os valores a serem inseridos no array são listados dentro do bloco `{ ... }`. O tamanho desta lista também determina o tamanho do array criado.

Na verdade, você não precisa escrever a parte `new int[]` nas versões mais recentes do Java. Você pode apenas escrever:

```java
int[] ints2 = { 1,2,3,4,5,6,7,8,9,10 };

```

É a parte dentro das chaves que é chamada de literal de array.

Este estilo funciona para arrays de todos os tipos primitivos, bem como para arrays de strings. Aqui está um exemplo com array de strings:

```java
String[] strings = {"one", "two", "three"};

```

## O Tamanho do Array em Java Não Pode Ser Alterado

Uma vez que um array foi criado, seu tamanho não pode ser redimensionado. Em algumas linguagens de programação (ex: JavaScript), os arrays podem mudar de tamanho após a criação, mas em Java um array não pode mudar de tamanho depois de criado. Se você precisa de uma estrutura de dados semelhante a um array que possa mudar de tamanho, deve usar uma `List`, ou pode criar um array redimensionável em Java. Em alguns casos, você também pode usar um `RingBuffer` em Java que, a propósito, é implementado internamente utilizando um array Java.

## Acessando Elementos de um Array em Java

Cada variável em um array Java também é chamada de "elemento". Portanto, o exemplo mostrado anteriormente criou um array com espaço para 10 elementos, e cada elemento é uma variável do tipo `int`.

Cada elemento no array possui um índice (um número). Você pode acessar cada elemento no array através do seu índice. Aqui está um exemplo:

```java
intArray[0] = 0;
int firstInt = intArray[0];

```

Este exemplo primeiro define o valor do elemento (`int`) com índice 0 e, em segundo lugar, lê o valor do elemento com índice 0 para uma variável `int`.

Você pode usar os elementos em um array Java exatamente como se fossem variáveis comuns. Você pode ler o valor deles, atribuir valores a eles, usar os elementos em cálculos e passar elementos específicos como parâmetros em chamadas de métodos.

Os índices dos elementos em um array Java sempre começam em 0 e continuam até o número correspondente ao tamanho do array menos 1. Portanto, no exemplo acima de um array com 10 elementos, os índices vão de 0 a 9.

## Tamanho do Array

Você pode acessar o tamanho de um array através do seu campo `length`. Aqui está um exemplo:

```java
int[] intArray = new int[10];
int arrayLength = intArray.length;

```

Neste exemplo, a variável chamada `arrayLength` conterá o valor 10 após a execução da segunda linha de código.

## Iterando Arrays

Você pode percorrer todos os elementos de um array usando o laço `for` do Java. Aqui está um exemplo de iteração de um array com um laço `for` em Java:

```java
String[] stringArray = new String[10];

for(int i=0; i < stringArray.length; i++) {
    stringArray[i] = "String no " + i;
}

for(int i=0; i < stringArray.length; i++) {
    System.out.println( stringArray[i] );
}

```

Este exemplo primeiro cria um array de referências a `String`. Quando você cria um array de referências a objetos pela primeira vez, cada uma das posições no array aponta para `null` - nenhum objeto.

O primeiro dos dois laços `for` itera pelo array de `String`, cria uma `String` e faz com que a posição referencie essa `String`.

O segundo dos dois laços `for` itera pelo array de `String` e imprime todas as strings que as posições referenciam.

Se este tivesse sido um array de `int` (valores primitivos), poderia ser assim:

```java
int[] intArray = new int[10];

for(int i=0; i < intArray.length; i++) {
    intArray[i] = i;
}

for(int i=0; i < intArray.length; i++) {
    System.out.println( intArray[i] );
}

```

A variável `i` é inicializada com 0 e roda até o tamanho do array menos 1. Neste caso, `i` assume os valores de 0 a 9, repetindo o código dentro do laço `for` uma vez para cada iteração, e a cada iteração `i` possui um valor diferente.

Você também pode iterar um array usando o laço "for-each" em Java. Aqui está como se parece:

```java
int[] intArray = new int[10];

for(int theInt : intArray) {
    System.out.println(theInt);
}

```

O laço for-each lhe dá acesso a cada elemento no array, um de cada vez, mas não fornece informações sobre o índice de cada elemento. Além disso, você só tem acesso ao valor. Você não pode alterar o valor do elemento naquela posição. Se precisar disso, utilize um laço `for` normal, conforme mostrado anteriormente.

O laço for-each também funciona com arrays de objetos. Aqui está um exemplo que mostra como iterar um array de objetos `String`:

```java
String[] stringArray = {"one", "two", "three"};

for(String theString : stringArray) {
    System.out.println(theString);
}

```

## Arrays Multidimensionais em Java

Os exemplos mostrados acima criaram arrays com uma única dimensão, o que significa elementos com índices partindo de 0 em diante. No entanto, é possível criar arrays onde cada elemento possui dois ou mais índices que o identificam (localizam) no array.

Você cria um array multidimensional em Java anexando um conjunto de colchetes (`[]`) por dimensão que deseja adicionar. Aqui está um exemplo que cria um array bidimensional:

```java
int[][] intArray = new int[10][20];

```

Este exemplo cria um array bidimensional de elementos `int`. O array contém 10 elementos na primeira dimensão e 20 elementos na segunda dimensão. Em outras palavras, este exemplo cria um array de arrays de elementos `int`. O array de arrays tem espaço para 10 arrays de `int`, e cada array de `int` tem espaço para 20 elementos `int`.

Você acessa os elementos em um array multidimensional com um índice por dimensão. No exemplo acima, você teria que usar dois índices. Aqui está um exemplo:

```java
int[][] intArray = new int[10][20];

intArray[0][2] = 129;

int oneInt = intArray[0][2];

```

A variável chamada `oneInt` conterá o valor 129 após a execução da última linha de código Java.

### Iterando Arrays Multidimensionais

Quando você itera um array multidimensional em Java, você precisa iterar cada dimensão do array separadamente. Aqui está como se parece a iteração de um array multidimensional em Java:

```java
int[][] intArray = new int[10][20];

for(int i=0; i < intArray.length; i++){
    for(int j=0; j < intArray[i].length; j++){
        System.out.println("i: " + i + ", j: " + j);
    }
}

```

## Inserindo Elementos em um Array

Às vezes, você precisa inserir elementos em algum lugar de um array Java. Aqui está como você insere un novo valor em um array em Java:

```java
int[] ints = new int[20];

int insertIndex = 10;
int newValue    = 123;

// move os elementos abaixo do ponto de inserção.
for(int i=ints.length-1; i > insertIndex; i--){
    ints[i] = ints[i-1];
}

// insere o novo valor
ints[insertIndex] = newValue;

System.out.println(Arrays.toString(ints));

```

O exemplo primeiro cria um array. Depois, define um índice de inserção e um novo valor para inserir. Em seguida, todos os elementos a partir do índice de inserção até o final do array são deslocados um índice para baixo no array. Note que isso deslocará o último valor do array para fora dele (ele será simplesmente apagado).

O código de inserção de array acima poderia ser incorporado em um método Java. Aqui está como isso poderia parecer:

```java
public void insertIntoArray(int[] array, int insertIndex, int newValue){
    // move os elementos abaixo do ponto de inserção.
    for(int i=array.length-1; i > insertIndex; i--){
        array[i] = array[i-1];
    }
    
    // insere o novo valor
    array[insertIndex] = newValue;
}

```

Este método recebe um array `int[]` como parâmetro, bem como o índice para inserir o novo valor e o próprio novo valor. Você pode inserir elementos em um array chamando este método assim:

```java
int[] ints = new int[20];

insertIntoArray(ints, 0, 10);
insertIntoArray(ints, 1, 23);
insertIntoArray(ints, 9, 67);

```

Claro, se o método `insertIntoArray()` estiver localizado em uma classe diferente do código acima, você precisará de um objeto daquela classe para poder chamar o método. Ou, se o método `insertIntoArray()` for `static`, você precisará colocar o nome da classe e um ponto antes do nome do método.

## Removendo Elementos de um Array

Às vezes, você deseja remover um elemento de um array Java. Aqui está o código para remover um elemento de um array em Java:

```java
int[] ints = new int[20];

ints[10] = 123;

int removeIndex = 10;

for(int i = removeIndex; i < ints.length -1; i++){
    ints[i] = ints[i + 1];
}

```

Este exemplo primeiro cria um array de `int`. Depois, define o valor do elemento com índice 10 como 123. Em seguida, o exemplo remove o elemento com índice 10. Ele remove o elemento deslocando todos os elementos abaixo do índice 10 uma posição para cima no array. Após a remoção, o último elemento no array existirá duas vezes. Tanto no último quanto no penúltimo elemento.
