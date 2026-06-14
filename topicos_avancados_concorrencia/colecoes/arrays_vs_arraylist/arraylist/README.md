## Java List (e ArrayList)

A interface Java List (`java.util.List`) representa uma **sequência ordenada de objetos**. Pense nela como uma fila de banco ou uma lista de compras: a ordem em que você coloca os elementos importa, e você pode acessar qualquer item sabendo a sua posição exata.

Cada elemento em uma `List` possui um **índice** (uma posição numérica).

* O primeiro elemento está no índice **0**.
* O segundo elemento está no índice **1**, e assim por diante.

> 💡 **Analogia Didática:** O índice indica a quantos passos um elemento está do início da lista. O primeiro elemento está a exatamente `0` passos do início, por isso seu índice é 0!

Você pode adicionar qualquer objeto Java a uma `List`. Se não especificar o tipo de dado que ela deve guardar (usando Generics), você poderá misturar textos, números e objetos customizados na mesma lista — embora isso quase nunca seja feito na prática para evitar confusões.

A `List` é uma interface padrão do Java e herda todas as características de `Collection`.

---

## List (Lista) vs. Set (Conjunto)

Embora ambas guardem coleções de elementos, elas possuem regras de comportamento bem diferentes:

| Característica | Java List (Lista) | Java Set (Conjunto) |
| --- | --- | --- |
| **Duplicatas** | Permite elementos repetidos. | Cada elemento é único; não permite duplicatas. |
| **Ordem** | Mantém estritamente a ordem de inserção. | Não garante nenhuma ordem interna dos elementos. |

---

## Implementações de List

Como `List` é uma interface, ela serve como um "contrato". Você não pode criá-la diretamente (ex: `new List()`). É preciso escolher uma classe concreta que implemente esse contrato:

* **`ArrayList`**: A mais utilizada. Usa um array interno que cresce automaticamente. Excelente para buscar elementos rapidamente pelo índice.
* **`LinkedList`**: Interliga os elementos como correntes (nós). Muito rápida para inserir ou remover itens nas extremidades, mas lenta para buscar itens no meio.
* **`Vector` e `Stack**`: Versões antigas e sincronizadas (seguras para múltiplas threads), mas raramente usadas hoje em dia devido ao impacto na performance.

---

## Como Criar uma List

Para criar sua lista, você escolhe uma das classes mencionadas acima:

```java
// Criando uma lista baseada em Array (A mais comum)
List listA = new ArrayList();

// Criando uma lista duplamente encadeada
List listB = new LinkedList();

// Criando um Vetor (legado)
List listC = new Vector();

// Criando uma Pilha (legado)
List listD = new Stack();

```

---

## Listas Genéricas (Generics)

Por padrão, as listas aceitam qualquer tipo de `Object`. Contudo, desde o Java 5, usamos os **Generics** (`<Tipo>`) para blindar nosso código contra erros de digitação e tipos inválidos.

```java
// Esta lista aceita estritamente objetos do tipo 'MyObject'
List<MyObject> list = new ArrayList<MyObject>();

// Adiciona um novo objeto à lista
list.add(new MyObject("First MyObject"));

// Recupera o objeto do índice 0 diretamente, sem necessidade de conversão (cast)
MyObject myObject = list.get(0);

// Itera facilmente sabendo que todos os itens são do tipo 'MyObject'
for(MyObject anObject : list){
   // Executa alguma lógica com o objeto
}

```

### O Perigo de Não Usar Generics

Veja como o código fica confuso e propenso a erros sem o uso de Generics, pois o Java assume que tudo ali dentro é um `Object` genérico:

```java
// Lista sem tipo definido (aceita qualquer coisa)
List list = new ArrayList(); 

// Adiciona o objeto normalmente
list.add(new MyObject("First MyObject"));

// Erro se esquecer o cast! É obrigatório converter de Object para MyObject manualmente
MyObject myObject = (MyObject) list.get(0); 

// O loop precisa extrair como Object genérico
for(Object anObject : list){
    // É necessário fazer outro cast manual para poder acessar os métodos da sua classe
    MyObject theMyObject = (MyObject) anObject;
}

```

> ✔️ **Boa Prática:** Sempre defina o tipo da sua lista (`List<String>`, `List<Integer>`). Isso evita que um número seja inserido por engano em uma lista de textos e elimina a necessidade de conversão manual de tipos.

---

## Manipulando Elementos na Lista

### 1. Inserindo Elementos

O método `add()` envia o elemento diretamente para o final da lista.

```java
// Cria uma lista específica para textos (String)
List<String> listA = new ArrayList<>();

// Adiciona elementos sequencialmente no fim da lista
listA.add("element 1"); // Fica no índice 0
listA.add("element 2"); // Fica no índice 1
listA.add("element 3"); // Fica no índice 2

```

### 2. Inserindo Valores Nulos (`null`)

A maioria das listas aceita referências vazias:

```java
// Define uma variável nula
Object element = null;
List<Object> list = new ArrayList<>();

// Guarda o valor nulo na lista com sucesso
list.add(element);

```

### 3. Inserindo em um Índice Específico

Você pode "furar a fila" especificando o índice desejado. Os elementos que estavam ali e nas posições seguintes serão empurrados para a direita.

```java
// Força a inserção do texto exatamente no índice 0 (o início de tudo)
list.add(0, "element 4");
// O antigo elemento do índice 0 agora passa a ser o índice 1, o 1 vira 2, etc.

```

### 4. Unindo Duas Listas (`addAll`)

Você pode juntar o conteúdo de uma lista dentro de outra em uma única operação.

```java
// Lista de origem com alguns dados
List<String> listSource = new ArrayList<>();
listSource.add("123");
listSource.add("456");

// Lista de destino vazia
List<String> listDest = new ArrayList<>();

// Copia todos os elementos da lista de origem para dentro da lista de destino
listDest.addAll(listSource);

```

---

## Recuperando e Buscando Elementos

### 1. Pegando um Item pelo Índice

Use o método `get(int index)` para capturar o valor de uma posição específica.

```java
List<String> listA = new ArrayList<>();
listA.add("element 0");
listA.add("element 1");
listA.add("element 2");

// Resgata os elementos apontando diretamente seus respectivos índices
String element0 = listA.get(0); // Retorna "element 0"
String element1 = listA.get(1); // Retorna "element 1"
String element2 = listA.get(2); // Retorna "element 2"

```

### 2. Encontrando o Índice de um Elemento

Se você tem o objeto mas não sabe em qual posição ele está, use `indexOf()` (retorna a primeira ocorrência) ou `lastIndexOf()` (retorna a última ocorrência).

```java
List<String> list = new ArrayList<>();
String element1 = "element 1";
String element2 = "element 2";

list.add(element1); // Índice 0
list.add(element2); // Índice 1
list.add(element1); // Índice 2 (Duplicata de propósito)

// Descobre a primeira posição onde "element 1" aparece
int index1 = list.indexOf(element1); // Retorna 0

// Descobre a última posição onde "element 1" aparece
int lastIndex = list.lastIndexOf(element1); // Retorna 2

```

### 3. Verificando se o Item Existe (`contains`)

Retorna um valor booleano (`true` ou `false`). Internamente, o Java passa por todos os itens usando o método `.equals()` para comparar.

```java
List<String> list = new ArrayList<>();
String element1 = "element 1";
list.add(element1);

// Pergunta para a lista se o texto existe ali dentro
boolean containsElement = list.contains("element 1"); // Retorna true

// Se pesquisar por 'null', o Java usa o operador '==' em vez do '.equals()'
list.add(null);
boolean containsNull = list.contains(null); // Retorna true

```

---

## Removendo Elementos

Você pode remover itens informando o próprio objeto ou a posição dele.

```java
List<String> list = new ArrayList<>();
list.add("element 0");
list.add("element 1");
list.add("element 2");

// Método A: Remove passando o objeto alvo diretamente
list.remove("element 1"); 

// Método B: Remove passando o índice do elemento (aqui remove o "element 0")
list.remove(0); 

// IMPORTANTE: Após as remoções, os elementos restantes se reorganizam.
// O "element 2", que estava no índice 2, agora passa a ocupar o índice 0!

```

### Limpando Toda a Lista

```java
List<String> list = new ArrayList<>();
list.add("object 1");
list.add("object 2");

// Apaga absolutamente tudo da lista de uma vez só
list.clear(); 
// Agora a lista está totalmente vazia (size = 0)

```

### Interseção de Listas (`retainAll`)

Este método remove tudo, **exceto** os elementos que estiverem presentes na lista informada como parâmetro (operação de interseção).

```java
List<String> list = new ArrayList<>();
List<String> otherList = new ArrayList<>();

list.add("A"); list.add("B"); list.add("C");
otherList.add("A"); otherList.add("C"); otherList.add("D");

// Mantém em 'list' apenas o que também existir em 'otherList'
list.retainAll(otherList); 

// Resultado: 'list' agora só contém ["A", "C"]

```

---

## Operações Avançadas

### Tamanho da Lista

```java
List<String> list = new ArrayList<>();
list.add("Item 1");

// Retorna a quantidade atual de elementos na lista
int tamanho = list.size(); // Retorna 1

```

### Recortando a Lista (`subList`)

Cria um recorte (fatia) da lista original. O índice inicial está incluso, mas o índice final fica de fora.

```java
List<String> list = new ArrayList<>();
list.add("índice 0"); // 0
list.add("índice 1"); // 1
list.add("índice 2"); // 2
list.add("índice 3"); // 3

// Recorta do índice 1 ao 3 (o 3 não entra!)
List<String> sublist = list.subList(1, 3); 
// Resultado: sublist conterá ["índice 1", "índice 2"]

```

---

## Conversões (Array e Set)

### Converter List para Set (Removendo Duplicatas)

Uma forma inteligente de eliminar dados repetidos de uma lista é jogá-la dentro de um `Set`.

```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("A"); // Duplicado!

// Cria um conjunto passando a lista no construtor ou via addAll
Set<String> set = new HashSet<>(list);
// Resultado: O 'set' conterá apenas ["A"]

```

### Converter List para Array

```java
List<String> list = new ArrayList<>();
list.add("A");

// Abordagem antiga: Retorna um array de Object genérico
Object[] objects = list.toArray();

// Abordagem moderna: Retorna um array do tipo exato (String)
String[] strings = list.toArray(new String[0]);

```

### Converter Array para List

```java
String[] valoresArray = new String[]{ "um", "dois", "três" };

// Transforma o array fixo em uma estrutura de Lista
List<String> list = Arrays.asList(valoresArray);

```

---

## Ordenando Listas

### 1. Ordem Natural (`Comparable`)

Se a classe dos objetos implementa `Comparable` (como as classes nativas `String`, `Integer`), a ordenação ocorre de forma natural (alfabética ou numérica).

```java
List<String> list = new ArrayList<>();
list.add("C"); list.add("B"); list.add("A");

// Organiza os dados da lista em ordem alfabética ("A", "B", "C")
Collections.sort(list);

```

### 2. Ordem Customizada (`Comparator`)

Quando seus objetos não possuem ordem óbvia (ex: uma classe `Carro`), criamos um critério de comparação personalizado.

Dado o modelo de `Car`:

```java
public class Car {
    public String brand;
    public String numberPlate;
    public int noOfDoors;

    public Car(String brand, String numberPlate, int noOfDoors) {
        this.brand = brand;
        this.numberPlate = numberPlate;
        this.noOfDoors = noOfDoors;
    }
}

```

Podemos ordená-lo de forma clássica ou com Lambdas:

```java
List<Car> list = new ArrayList<>();
list.add(new Car("Volvo V40", "XYZ 201845", 5));
list.add(new Car("Citroen C1", "ABC 164521", 4));

// Abordagem 1: Usando classe anônima tradicional
Comparator<Car> carBrandComparator = new Comparator<Car>() {
    @Override
    public int compare(Car car1, Car car2) {
        return car1.brand.compareTo(car2.brand); // Compara o texto da marca do carro
    }
};
Collections.sort(list, carBrandComparator);

// Abordagem 2: Usando Expressões Lambda (Muito mais limpo)
Comparator<Car> brandLambda = (c1, c2) -> c1.brand.compareTo(c2.brand);
Comparator<Car> doorsLambda = (c1, c2) -> c1.noOfDoors - c2.noOfDoors; // Ordenação numérica

// Executa a ordenação com o critério escolhido
Collections.sort(list, doorsLambda);

```

---

## Formas de Iterar (Percorrer) uma Lista

### Método 1: Usando um Iterator (Clássico)

```java
List<String> list = new ArrayList<>();
list.add("A"); list.add("B");

// Solicita o objeto controlador de iteração da lista
Iterator<String> iterator = list.iterator();

// Enquanto houver um próximo item na lista...
while(iterator.hasNext()) {
    // Avança e captura o elemento atual
    String elemento = iterator.next();
}

```

### Método 2: Loop For-Each (Recomendado para o dia a dia)

A forma mais limpa e legível para percorrer todos os itens.

```java
List<String> list = new ArrayList<>();
list.add("A"); list.add("B");

// Para cada 'elemento' do tipo String contido dentro de 'list'
for(String elemento : list) {
    System.out.println(elemento); // Imprime o item atual
}

```

### Método 3: Loop For Tradicional (Indexado)

Útil quando você precisa obrigatoriamente controlar o número do índice durante o processamento.

```java
List<String> list = new ArrayList<>();
list.add("A"); list.add("B");

// Varre a lista incrementando a variável 'i' do zero até o tamanho limite
for(int i = 0; i < list.size(); i++) {
    // Busca o elemento correspondente à posição atual do contador
    String elemento = list.get(i);
}

```

### Método 4: Usando a Java Stream API (Moderno)

Ideal para operações funcionais e manipulações complexas de dados.

```java
List<String> stringList = new ArrayList<>();
stringList.add("abc");
stringList.add("def");

// Transforma a lista em um fluxo de dados (Stream) e aplica uma ação para cada item
stringList.stream().forEach(elemento -> {
    System.out.println(elemento); // Imprime o elemento processado pelo fluxo
});

```
