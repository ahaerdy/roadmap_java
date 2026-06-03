# Passagem por Valor

**(Como um Mecanismo de Passagem de Parâmetros em Java)**

## 1. Introdução

Os dois modos mais prevalentes de passar argumentos para métodos são “passagem por valor” (*passing-by-value*) e “passagem por referência” (*passing-by-reference*). Diferentes linguagens de programação usam esses conceitos de maneiras diferentes. No que diz respeito ao Java, tudo é estritamente Passagem por Valor (*Pass-by-Value*).

Neste tutorial, vamos ilustrar como o Java passa argumentos para vários tipos.

## 2. Passagem por Valor vs Passagem por Referência

Vamos começar com alguns dos diferentes mecanismos para passar parâmetros para funções:

* valor (*value*)
* referência (*reference*)
* resultado (*result*)
* valor-resultado (*value-result*)
* nome (*name*)

Os dois mecanismos mais comuns em linguagens de programação modernas são “Passagem por Valor” e “Passagem por Referência”. Antes de prosseguirmos, vamos discutir estes primeiro:

### 2.1. Passagem por Valor

Quando um parâmetro é por valor (*pass-by-value*), o método chamador (*caller*) e o método chamado (*callee*) operam em duas variáveis diferentes que são cópias uma da outra. Quaisquer alterações em uma variável não modificam a outra.

Isso significa que, ao chamar um método, os parâmetros passados para o método chamado serão clones dos parâmetros originais. Qualquer modificação feita no método chamado não terá efeito sobre os parâmetros originais no método chamador.

### 2.2. Passagem por Referência

Quando um parâmetro é por referência (*pass-by-reference*), o chamador e o chamado operam no mesmo objeto.

Isso significa que, quando uma variável é por referência, o identificador exclusivo do objeto é enviado para o método. Quaisquer alterações nos membros de instância do parâmetro resultarão em essa alteração sendo feita no valor original.

## 3. Passagem de Parâmetros em Java

Os conceitos fundamentais em qualquer linguagem de programação são “valores” e “referências”. Em Java, as variáveis Primitivas armazenam os valores reais, enquanto as Não Primitivas armazenam as variáveis de referência que apontam para os endereços dos objetos aos quais estão se referindo. Tanto os valores os quais as referências representam são armazenados na memória stack.

```java
import org.junit.jupiter.api.Test; // Importa a anotação Test do JUnit 5
import static org.junit.jupiter.api.Assertions.assertEquals; // Importa o método de asserção estática assertEquals do JUnit 5

public class PrimitivesUnitTest { // Declaração da classe de teste para tipos primitivos
    
    @Test // Anotação que indica que este método é um caso de teste isolado do JUnit
    public void whenModifyingPrimitives_thenOriginalValuesNotModified() {
        int x = 1; // Inicializa a variável primitiva x com o valor 1 na memória stack
        int y = 2; // Inicializa a variável primitiva y com o valor 2 na memória stack
        
        // Antes da Modificação
        assertEquals(x, 1); // Verifica e garante que o valor inicial de x é igual a 1
        assertEquals(y, 2); // Verifica e garante que o valor inicial de y é igual a 2
        
        modify(x, y); // Chama o método estático passando apenas cópias dos valores contidos em x e y
        
        // Após a Modificação
        assertEquals(x, 1); // Verifica se o x original do escopo atual permaneceu inalterado (igual a 1)
        assertEquals(y, 2); // Verifica se o y original do escopo atual permaneceu inalterado (igual a 2)
    }
    
    public static void modify(int x1, int y1) { // Recebe cópias dos valores em novas variáveis locais (x1 e y1) na stack
        x1 = 5; // Altera o valor da variável local x1 para 5; o x original fora deste método não é afetado
        y1 = 10; // Altera o valor da variável local y1 para 10; o y original fora deste método não é afetado
    }
}

```

---

### ✍️ Passo a Passo da Execução

#### 1. Inicialização na Memória Stack

```java
int x = 1; // Cria espaço na stack para x
int y = 2; // Cria espaço na stack para y

```

* O Java aloca um espaço na memória *Stack* dedicado a este método de teste.
* Ele cria duas variáveis locais: `x` (armazenando o valor `1`) e `y` (armazenando o valor `2`).
* Os métodos `assertEquals(x, 1);` e `assertEquals(y, 2);` servem apenas para checar e garantir que, neste momento, os valores são exatamente esses.

#### 2. A Invocação do Método (O Ponto Crítico)

```java
modify(x, y); // Invocação com passagem estrita por valor

```

* Aqui ocorre a mágica do *Pass-by-Value*. O Java **não** passa as variáveis `x` e `y` reais para o método `modify`.
* Em vez disso, ele lê os valores dentro delas (`1` e `2`), duplica esses valores e os entrega para o método `modify`.
* Na memória *Stack*, em um escopo totalmente novo e isolado para o método `modify`, são criadas as variáveis `x1` (recebendo a cópia `1`) e `y1` (recebendo a cópia `2`).

#### 3. A Alteração Isolada

```java
x1 = 5;  // Escopo local do método modify
y1 = 10; // Escopo local do método modify

```

* Dentro do método `modify`, os valores de `x1` e `y1` são alterados para `5` e `10`.
* Essa alteração ocorre exclusivamente nas cópias locais. As variáveis originais `x` e `y`, que estão no escopo do método de teste, permanecem intocadas e seguras em seu próprio canto da memória.
* Assim que o método `modify` chega ao fim, o escopo dele é destruído, e as variáveis `x1` e `y1` deixam de existir.

#### 4. A Verificação Final

```java
assertEquals(x, 1); // Confirmação de isolamento
assertEquals(y, 2); // Confirmação de isolamento

```

* De volta ao método principal, o JUnit verifica se `x` continua sendo `1` e se `y` continua sendo `2`.
* Como a resposta é sim, o teste passa com sucesso, provando empiricamente que o método externo não conseguiu afetar os escopos originais.

---

Os argumentos em Java são sempre passados por valor (*passed-by-value*). Durante a invocação do método, uma cópia de cada argumento, seja ele um valor ou uma referência, é criada na memória stack, a qual é então passada para o método.

No caso de primitivos, o valor é simplesmente copiado dentro da memória stack e depois passado para o método chamado; no caso de não primitivos, uma referência na memória stack aponta para os dados reais que residem na heap. Quando passamos um objeto, a referência na memória stack é copiada e a nova referência é passada para o método.

Vamos agora ver isso em ação com a ajuda de alguns exemplos de código.

### 3.1. Passando Tipos Primitivos

A Linguagem de Programação Java apresenta oito tipos de dados primitivos. As variáveis primitivas são armazenadas diretamente na memória stack. Sempre que qualquer variável de tipo de dados primitivo é passada como um argumento, os parâmetros reais são copiados para os argumentos formais e esses argumentos formais acumulam seu próprio espaço na memória stack.

O tempo de vida desses parâmetros formais dura apenas enquanto esse método estiver em execução e, ao retornar, esses argumentos formais são eliminados da stack e descartados.

Vamos tentar entender as asserções no programa acima analisando como esses valores são armazenados na memória:

* As variáveis “x” e “y” no método principal são tipos primitivos e seus valores são armazenados diretamente na memória stack.
* Quando chamamos o método `modify()`, uma cópia exata de cada uma dessas variáveis é criada e armazenada em um local diferente na memória stack.
* Qualquer modificação nessas cópias afeta apenas a elas e deixa as variáveis originais inalteradas.

### 3.2. Passando Referências de Objeto

Em Java, todos os objetos são armazenados dinamicamente no espaço Heap nos bastidores. Esses objetos são acessados a partir de variáveis de referência.

Um objeto Java, em contraste com os Primitivos, é gerenciado em duas etapas. As variáveis de referência são armazenadas na memória stack e o objeto ao qual elas estão se referindo é armazenado na memória Heap.

Sempre que um objeto é passado como argumento, uma cópia exata da variável de referência é criada, a qual aponta para o mesmo local do objeto na memória heap que a variável de referência original.

Como resultado disso, sempre que fazemos qualquer alteração nos atributos de um objeto através dessa referência cópia dentro do método, essa alteração é refletida no objeto original. No entanto, se alocarmos um novo objeto para a variável de referência passada, isso alterará apenas a cópia local, não modificando o objeto original no chamador.

Vamos tentar compreender isso com a ajuda do exemplo de código abaixo:

```java
import org.junit.jupiter.api.Test; // Importa a anotação Test do JUnit 5
import static org.junit.jupiter.api.Assertions.assertEquals; // Importa o método de asserção estática assertEquals do JUnit 5

public class NonPrimitivesUnitTest { // Declaração da classe de teste para tipos não primitivos (objetos)
    
    @Test // Anotação que indica um método de teste JUnit isolado
    public void whenModifyingObjects_thenOriginalObjectChanged() {
        Foo a = new Foo(1); // Cria um objeto Foo na heap e armazena o endereço de referência na variável 'a' da stack
        Foo b = new Foo(1); // Cria outro objeto Foo na heap e armazena o endereço de referência na variável 'b' da stack
        
        // Antes da Modificação
        assertEquals(a.num, 1); // Verifica se o atributo interno 'num' do objeto apontado por 'a' vale 1
        assertEquals(b.num, 1); // Verifica se o atributo interno 'num' do objeto apontado por 'b' vale 1
        
        modify(a, b); // Chama o método passando cópias das referências 'a' e 'b' (apontam para os mesmos objetos na heap)
        
        // Após a Modificação
        assertEquals(a.num, 2); // Passa! O valor mudou para 2 porque o método modify alterou a propriedade do objeto que 'a' aponta
        assertEquals(b.num, 1); // Passa! Continua 1 porque a variável original 'b' ainda aponta para o mesmo objeto original intacto
    }
    
    public static void modify(Foo a1, Foo b1) { // Recebe cópias das referências de memória locais em 'a1' e 'b1'
        a1.num++; // Modifica a propriedade do objeto real na heap compartilhado com a variável original 'a'
        b1 = new Foo(1); // Modifica a referência local 'b1': ela passa a apontar para um novo objeto recém-criado na heap
        b1.num++; // Incrementa a propriedade 'num' deste novo objeto; a variável original 'b' continua alheia a isso
    }
}

class Foo { // Definição da classe auxiliar de suporte para os testes controlados
    public int num; // Atributo primitivo inteiro encapsulado na classe
    
    public Foo(int num) { // Construtor padrão da classe Foo
        this.num = num; // Associa o parâmetro de entrada diretamente ao atributo de instância do objeto
    }
}

```

Vamos analisar as asserções no programa acima. Passamos os objetos `a` e `b` no método `modify()` que têm o mesmo valor inicial 1. Inicialmente, essas referências de objeto estão apontando para dois locais de objetos distintos em um espaço heap.

Quando essas referências `a` e `b` são passadas no método `modify()`, ele cria cópias espelhadas daquelas referências para `a1` e `b1` que apontam, inicialmente, para os mesmos objetos antigos.

No método `modify()`, quando modificamos um atributo interno através da referência `a1`, isso altera o objeto que reside na heap. No entanto, para a referência `b1`, atribuímos um novo objeto com a palavra-chave `new`. Portanto, a variável local de escopo `b1` passa a apontar para um local inédito na memória heap.

Qualquer alteração feita em `b1` a partir desse ponto não refletirá absolutamente nada no objeto associado à referência original `b`.

## 4. Conclusão

Neste artigo, analisamos como a passagem de parâmetros é tratada no caso de Primitivos e Não Primitivos.

Aprendemos que a passagem de parâmetros em Java é sempre *Pass-by-Value*. No entanto, o comportamento visual muda dependendo se estamos lidando com Primitivos ou com as referências dos Objetos:

* Para tipos Primitivos, os valores reais são copiados diretamente na stack.
* Para tipos Objeto, o valor copiado e passado também por valor é a própria **referência** (endereço de memória) do objeto.