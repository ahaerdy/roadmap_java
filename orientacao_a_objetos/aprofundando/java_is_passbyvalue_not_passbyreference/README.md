# Passagem por Valor
 **(Como um Mecanismo de Passagem de Parâmetros em Java)**

## 1. Introdução
Os dois modos mais prevalentes de passar argumentos para métodos são “passagem por valor” (*passing-by-value*) e “passagem por referência” (*passing-by-reference*). Diferentes linguagens de programação usam esses conceitos de maneiras diferentes. No que diz respeito ao Java, tudo é estritamente Passagem por Valor (*Pass-by-Value*).

Neste tutorial, vamos ilustrar como o Java passa argumentos para vários tipos.

## 2. Passagem por Valor vs Passagem por Referência
Vamos começar com alguns dos diferentes mecanismos para passar parâmetros para funções:
- valor (*value*)
- referência (*reference*)
- resultado (*result*)
- valor-resultado (*value-result*)
- nome (*name*)

Os dois mecanismos mais comuns em linguagens de programação modernas são “Passagem por Valor” e “Passagem por Referência”. Antes de prosseguirmos, vamos discutir estes primeiro:

### 2.1. Passagem por Valor
Quando um parâmetro é por valor (*pass-by-value*), o método chamador (*caller*) e o método chamado (*callee*) operam em duas variáveis diferentes que são cópias uma da outra. Quaisquer alterações em uma variável não modificam a outra.

Isso significa que, ao chamar um método, os parâmetros passados para o método chamado serão clones dos parâmetros originais. Qualquer modificação feita no método chamado não terá efeito sobre os parâmetros originais no método chamador.

### 2.2. Passagem por Referência
Quando um parâmetro é por referência (*pass-by-reference*), o chamador e o chamado operam no mesmo objeto.

Isso significa que, quando uma variável é por referência, o identificador exclusivo do objeto é enviado para o método. Quaisquer alterações nos membros de instância do parâmetro resultarão em essa alteração sendo feita no valor original.

## 3. Passagem de Parâmetros em Java
Os conceitos fundamentais em qualquer linguagem de programação são “valores” e “referências”. Em Java, as variáveis Primitivas armazenam os valores reais, enquanto as Não Primitivas armazenam as variáveis de referência que apontam para os endereços dos objetos aos quais estão se referindo. Tanto os valores quanto as referências são armazenados na memória stack.

```java
public class PrimitivesUnitTest { // Declaração da classe de teste para tipos primitivos
    @Test // Anotação que indica que este método é um caso de teste do JUnit
    public void whenModifyingPrimitives_thenOriginalValuesNotModified() {
        int x = 1; // Inicializa a variável primitiva x com o valor 1 na memória stack
        int y = 2; // Inicializa a variável primitiva y com o valor 2 na memória stack
        
        // Antes da Modificação
        assertEquals(x, 1); // Verifica se o valor de x é igual a 1
        assertEquals(y, 2); // Verifica se o valor de y é igual a 2
        
        modify(x, y); // Chama o método passando cópias dos valores de x e y
        
        // Após a Modificação
        assertEquals(x, 1); // Verifica se o x original permaneceu inalterado (igual a 1)
        assertEquals(y, 2); // Verifica se o y original permaneceu inalterado (igual a 2)
    }
    
    public static void modify(int x1, int y1) { // Recebe as cópias dos valores em novas variáveis locais da stack
        x1 = 5; // Modifica apenas a variável local x1 para 5; não afeta o x original
        y1 = 10; // Modifica apenas a variável local y1 para 10; não afeta o y original
    }
}
```

Os argumentos em Java são sempre passados por valor (*passed-by-value*). Durante a invocação do método, uma cópia de cada argumento, seja ele um valor ou uma referência, é criada na memória stack, a qual é então passada para o método.

No caso de primitivos, o valor é simplesmente copiado dentro da memória stack e depois passado para o método chamado; no caso de não primitivos, uma referência na memória stack aponta para os dados reais que residem na heap. Quando passamos um objeto, a referência na memória stack é copiada e a nova referência é passada para o método.

Vamos agora ver isso em ação com a ajuda de alguns exemplos de código.

### 3.1. Passando Tipos Primitivos
A Linguagem de Programação Java apresenta oito tipos de dados primitivos. As variáveis primitivas são armazenadas diretamente na memória stack. Sempre que qualquer variável de tipo de dados primitivo é passada como um argumento, os parâmetros reais são copiados para os argumentos formais e esses argumentos formais acumulam seu próprio espaço na memória stack.

O tempo de vida desses parâmetros formais dura apenas enquanto esse método estiver em execução e, ao retornar, esses argumentos formais são eliminados da stack e descartados.

Vamos tentar entender isso com a ajuda de um exemplo de código:

```java
public class PrimitivesUnitTest { // Declaração da classe de teste para tipos primitivos
    @Test // Anotação que indica que este método é um caso de teste do JUnit
    public void whenModifyingPrimitives_thenOriginalValuesNotModified() {
        int x = 1; // Inicializa a variável primitiva x com o valor 1 na memória stack
        int y = 2; // Inicializa a variável primitiva y com o valor 2 na memória stack
        
        // Antes da Modificação
        assertEquals(x, 1); // Verifica se o valor de x é igual a 1
        assertEquals(y, 2); // Verifica se o valor de y é igual a 2
        
        modify(x, y); // Chama o método passando cópias dos valores de x e y
        
        // Após a Modificação
        assertEquals(x, 1); // Verifica se o x original permaneceu inalterado (igual a 1)
        assertEquals(y, 2); // Verifica se o y original permaneceu inalterado (igual a 2)
    }
    
    public static void modify(int x1, int y1) { // Recebe as cópias dos valores em novas variáveis locais da stack
        x1 = 5; // Modifica apenas a variável local x1 para 5; não afeta o x original
        y1 = 10; // Modifica apenas a variável local y1 para 10; não afeta o y original
    }
}

```

Vamos tentar entender as asserções no programa acima analisando como esses valores são armazenados na memória:

* As variáveis “x” e “y” no método principal são tipos primitivos e seus valores são armazenados diretamente na memória stack.
* Quando chamamos o método `modify()`, uma cópia exata de cada uma dessas variáveis é criada e armazenada em um local diferente na memória stack.
* Qualquer modificação nessas cópias afeta apenas a elas e deixa as variáveis originais inalteradas.

### 3.2. Passando Referências de Objeto

Em Java, todos os objetos são armazenados dinamicamente no espaço Heap nos bastidores. Esses objetos são referenciados a partir de referências chamadas variáveis de referência.

Um objeto Java, em contraste com os Primitivos, é armazenado em duas etapas. As variáveis de referência são armazenadas na memória stack e o objeto ao qual elas estão se referindo é armazenado em uma memória Heap.

Sempre que um objeto é passado como argumento, uma cópia exata da variável de referência é criada, a qual aponta para o mesmo local do objeto na memória heap que a variável de referência original.

Como resultado disso, sempre que fazemos qualquer alteração no mesmo objeto no método, essa alteração é refletida no objeto original. No entanto, se alocarmos um novo objeto para a variável de referência passada, isso não será refletido no objeto original.

Vamos tentar compreender isso com a ajuda de um exemplo de código:

```java
public class NonPrimitivesUnitTest { // Declaração da classe de teste para tipos não primitivos
    @Test // Anotação que indica um método de teste JUnit
    public void whenModifyingObjects_thenOriginalObjectChanged() {
        Foo a = new Foo(1); // Cria um novo objeto Foo na heap e armazena sua referência na variável 'a' da stack
        Foo b = new Foo(1); // Cria outro objeto Foo na heap e armazena sua referência na variável 'b' da stack
        
        // Antes da Modificação
        assertEquals(a.num, 1); // Verifica se o atributo 'num' do objeto apontado por 'a' é igual a 1
        assertEquals(b.num, 1); // Verifica se o atributo 'num' do objeto apontado por 'b' é igual a 1
        
        modify(a, b); // Chama o método passando cópias das variáveis de referência 'a' e 'b'
        
        // Após a Modificação
        assertEquals(a.num, 2); // Verifica se 'a.num' mudou para 2, pois o objeto na heap foi alterado através de sua referência
        assertEquals(b.num, 1); // Verifica se 'b.num' continua 1, pois a variável original 'b' ainda aponta para o mesmo objeto
    }
    
    public static void modify(Foo a1, Foo b1) { // Recebe cópias das variáveis de referência em 'a1' e 'b1'
        a1.num++; // Incrementa o atributo 'num' do objeto compartilhado; afeta o objeto original referenciado por 'a'
        b1 = new Foo(1); // Cria um novo objeto na heap e faz a referência local 'b1' apontar para ele
        b1.num++; // Incrementa o 'num' deste novo objeto; não causa impacto no objeto original referenciado por 'b'
    }
}

class Foo { // Definição da classe auxiliar Foo usado nos testes
    public int num; // Atributo inteiro público que armazena um número
    
    public Foo(int num) { // Construtor da classe Foo
        this.num = num; // Atribui o parâmetro num ao atributo de instância correspondente
    }
}

```

Vamos analisar as asserções no programa acima. Passamos os objetos `a` e `b` no método `modify()` que têm o mesmo valor 1. Inicialmente, essas referências de objeto estão apontando para dois locais de objetos donuts em um espaço heap:

Quando essas referências `a` e `b` são passadas no método `modify()`, ele cria cópias espelhadas daquelas referências `a1` e `b1` que apontam para os mesmos objetos antigos:

No método `modify()`, quando modificamos a referência `a1`, isso altera o objeto original. No entanto, para uma referência `b1`, atribuímos um novo objeto. Portanto, ela agora está apontando para um novo objeto na memória heap.

Qualquer alteração feita em `b1` não refletirá nada no objeto original:

## 4. Conclusão

Neste artigo, analisamos como a passagem de parâmetros é tratada no caso de Primitivos e Não Primitivos.

Aprendemos que a passagem de parâmetros em Java é sempre *Pass-by-Value*. No entanto, o contexto muda dependendo se estamos lidando com Primitivos ou Objetos:

* Para tipos Primitivos, os parâmetros são *pass-by-value*
* Para tipos Objeto, a referência do objeto é *pass-by-reference*
