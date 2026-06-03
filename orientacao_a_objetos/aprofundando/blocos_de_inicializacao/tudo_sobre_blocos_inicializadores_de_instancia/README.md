# Tudo sobre os blocos inicializadores de instância do Java

**27 de abril de 2023**

O Java pode inicializar campos durante a criação de objetos usando blocos inicializadores de instância.

*[Este artigo foi adaptado do livro OCP Oracle Certified Professional Java SE 17 Developer (Exam 1Z0-829) Programmer's Guide, publicado pela Oracle Press. —Ed.]*

Os inicializadores podem ser usados para definir valores iniciais para campos em objetos e classes. Existem três tipos de inicializadores:

* Expressões inicializadoras de campo
* Blocos inicializadores estáticos
* Blocos inicializadores de instância

A inicialização de campos pode ser especificada em instruções de declaração de campos usando expressões inicializadoras. O valor da expressão inicializadora deve ser compatível com a atribuição do campo declarado.

O Java permite que blocos inicializadores estáticos sejam definidos em uma classe. Embora tais blocos possam incluir código arbitrário, eles são usados principalmente para inicializar campos estáticos. O código em um bloco inicializador estático é executado apenas uma vez quando a classe é carregada e inicializada.

Assim como os blocos inicializadores estáticos podem ser usados para inicializar campos estáticos em uma classe nomeada, o Java oferece a capacidade de inicializar campos durante a criação de objetos usando blocos inicializadores de instância, e esse é o assunto deste artigo.

A esse respeito, tais blocos servem ao mesmo propósito que os construtores durante a criação de objetos. A sintaxe de um bloco inicializador de instância é a mesma de um bloco local, como mostrado na linha (2) no código a seguir. O código no bloco local é executado toda vez que uma instância da classe é criada.

```java
class InstanceInitializers {
  long[] squares = new long[10];      // (1) Declaração e instanciação do array squares com 10 elementos
  // ...
  {                                   // (2) Inicializador de Instância: executado a cada nova criação de objeto
    for (int i = 0; i < squares.length; i++) // Laço para iterar por todos os índices do array
      squares[i] = i*i;               // Atribui o quadrado do índice atual a cada posição do array
  }                                   // Fim do bloco inicializador de instância
  // ...
}

```

O array `squares` com o comprimento especificado é criado primeiro na linha (1); sua criação é seguida pela execução do bloco inicializador de instância na linha (2) toda vez que uma instância da classe `InstanceInitializers` é criada. Observe que o bloco inicializador de instância não está contido em nenhum método. Uma classe pode ter mais de um bloco inicializador de instância, e estes (e quaisquer expressões inicializadoras de instância em declarações de campos de instância) são executados na ordem em que são especificados na classe.

## Ordem de declaração dos inicializadores de instância

De forma análoga aos outros inicializadores discutidos anteriormente, um bloco inicializador de instância não pode fazer uma referência antecipada (forward reference) a um campo por seu nome simples em uma operação de leitura, pois isso violaria a regra de declarar antes de ler (declare-before-reading). No entanto, o uso da palavra-chave `this` para acessar um campo não é um problema.

A classe na Listagem 1 possui um bloco inicializador de instância na linha (1) com referências antecipadas aos campos `i`, `j` e `k`, que são declarados nas linhas (7), (8) e (9), respectivamente. Esses campos são acessados usando a referência `this` em operações de leitura nas linhas (3), (4), (5) e (6). O uso do nome simples desses campos nas linhas (3), (4), (5) e (6) para acessar seus valores violará a regra de declarar antes de usar (declare-before-use), resultando em erros de compilação — independentemente de os campos serem declarados com expressões inicializadoras ou se são `final`.

Os campos `i` e `j` são acessados na linha (2) em operações de escrita, as quais são permitidas usando o nome simples. No entanto, deve-se ter cuidado para garantir que os campos sejam inicializados corretamente. Nas linhas (3), (4) e (5), os campos `i` e `j` possuem o valor 10. Contudo, quando as expressões inicializadoras são avaliadas nas declarações de campos de instância, o valor de `j` será definido como 100.

**Listagem 1. Acessando campos usando a palavra-chave this**

```java
public class InstanceInitializersII {
  { // Inicializador de instância com referências antecipadas. (1)
    i = j = 10;                         // (2) Permitido. Operação de escrita usando nomes simples antes da declaração.
    int result = this.i * this.j;       // (3) i é 10, j é 10. Leitura permitida através do prefixo 'this'.
    System.out.println(this.i);         // (4) Imprime 10 usando a referência explícita 'this'
    System.out.println(this.j);         // (5) Imprime 10 usando a referência explícita 'this'
    System.out.println(this.k);         // (6) Imprime 50 acessando o campo final via 'this'
  }
  // Declarações de campos de instância.
  int i;                                // (7) Declaração de campo sem expressão inicializadora (assume padrão 0 antes do bloco)
  int j = 100;                          // (8) Declaração de campo com expressão inicializadora (sobrescreve o valor do bloco depois)
  final int k = 50;                     // (9) Campo de instância final com expressão constante.
}

```

A Listagem 2 ilustra alguns pontos sutis adicionais em relação aos blocos inicializadores de instância. Ocorre uma referência antecipada ilegal no código na linha (4), que tenta ler o valor do campo `nsf1` antes de ele ser declarado. A operação de leitura na linha (11) ocorre após a declaração; portanto, é permitida. Referências antecipadas feitas no lado esquerdo da atribuição são sempre permitidas, como mostrado nas linhas (2), (3), (5) e (7). Enquanto isso, a declaração de variáveis locais usando a palavra reservada `var` em blocos inicializadores de instância é exibida nas linhas (5) e (12).

**Listagem 2. Inicializadores de instância e referências antecipadas**

```java
public class NonStaticForwardReferences {
  {                                     // (1) Bloco inicializador de instância.
    nsf1 = 10;                          // (2) OK. Atribuição para nsf1 permitida (escrita).
    nsf1 = sf1;                         // (3) OK. Acesso a campo estático em contexto não-estático permitido.
    // int a = 2 * nsf1;                // (4) Não OK. Operação de leitura antes da declaração (causa erro de compilação).
    var b = nsf1 = 20;                  // (5) OK. Atribuição para nsf1 permitida (uso de var local).
    int c = this.nsf1;                  // (6) OK. Não é acessado pelo nome simples, mas via 'this'.
  }
  int nsf1 = nsf2 = 30;                 // (7) Campo não-estático. Atribuição para nsf2 permitida (escrita).
  int nsf2;                             // (8) Campo não-estático declarado.
  static int sf1 = 5;                   // (9) Campo estático declarado com inicializador.
  {                                     // (10) Segundo bloco inicializador de instância.
    int d = 2 * nsf1;                   // (11) OK. Operação de leitura realizada após a declaração de nsf1.
    var e = nsf1 = 50;                  // (12) OK. Atribuição para nsf1 permitida.
  }
  public static void main(String[] args) { // Método principal para execução do programa
    NonStaticForwardReferences objRef = new NonStaticForwardReferences () ; // Cria instância da classe, acionando os blocos
    System.out.println("nsf1: " + objRef.nsf1) ; // Imprime o valor final de nsf1 no objeto criado
    System.out.println("nsf2: " + objRef.nsf2);  // Imprime o valor final de nsf2 no objeto criado
  }
}

```

A seguir está a saída do código da Listagem 2:

```text
nsf1: 50
nsf2: 30

```

Assim como em uma expressão inicializadora de instância, as palavras-chave `this` e `super` podem ser usadas para se referir ao objeto atual em um bloco inicializador de instância. (Uma instrução `return` não é permitida em blocos inicializadores de instância.)

Um bloco inicializador de instância pode ser usado para extrair códigos comuns de inicialização que serão executados independentemente de qual construtor seja invocado. Um uso típico de um bloco inicializador de instância ocorre em classes anônimas, que não podem declarar construtores, mas podem usar blocos inicializadores de instância para initialize campos. Na Listagem 3, a classe anônima definida na linha (1) usa um bloco inicializador de instância na linha (2) para inicializar seus campos.

**Listagem 3. Bloco inicializador de instância em uma classe anônima**

```java
// Arquivo: InstanceInitBlock.java
class Base {
  protected int a;                      // Campo protegido 'a' acessível por subclasses
  protected int b;                      // Campo protegido 'b' acessível por subclasses
  void print() {                        // Método para exibir o valor de 'a'
    System.out.println("a: " + a);      // Imprime o valor atual de 'a'
  }
}
class AnonymousClassMaker {
  Base createAnonymous() {              // Método que cria e retorna uma classe anônima derivada de Base
    return new Base() {                 // (1) Início da declaração da classe anônima estendendo Base
      {                                 // (2) Inicializador de Instância na classe anônima
        a = 5;                          // Inicializa o campo herdado 'a' com o valor 5
        b = 10;                         // Inicializa o campo herdado 'b' com o valor 10
      }                                 // Fim do bloco inicializador de instância
      @Override
      void print() {                    // Sobrescrita do método print na classe anônima
        super.print();                  // Invoca o método print original da classe pai (Base)
        System.out.println("b: " + b);  // Adiciona a impressão do valor do campo 'b'
      }
    };                                  // Fim da definição da classe anônima
  }
}
public class InstanceInitBlock {
  public static void main(String[] args) { // Método de entrada do programa
    new AnonymousClassMaker().createAnonymous().print(); // Instancia a fábrica, gera a classe anônima e chama print()
  }
}

```

A seguir está a saída do código da Listagem 3:

```text
a: 5
b: 10

```

## Tratamento de exceções em blocos inicializadores de instância

O tratamento de exceções em blocos inicializadores de instância é semelhante ao de blocos inicializadores estáticos. No entanto, o tratamento de exceções em blocos inicializadores de instância difere daquele em blocos inicializadores estáticos no seguinte aspecto: A execução de um bloco inicializador de instância pode resultar em uma exceção verificada (checked exception) não capturada, desde que a exceção seja declarada na cláusula `throws` de cada construtor da classe. Os blocos inicializadores estáticos não podem permitir isso, pois nenhum construtor está envolvido na inicialização da classe. Os blocos inicializadores de instância em classes anônimas têm uma liberdade ainda maior: eles podem lançar qualquer exceção.

## Autores

**Vasily Strelnikov**
Vasily Strelnikov é especialista principal sênior em soluções OCI na Oracle; anteriormente, ele foi consultor de treinamento principal sênior. As especialidades de Strelnikov são design e integração de sistemas usando arquitetura orientada a serviços (SOA), Service Component Architecture (SCA) e Java; ele criou cursos de treinamento para Java e Java EE. Ele reside em Londres.

**Khalid Mughal**
Khalid A. Mughal é professor associado (emérito) no Departamento de Informática da Universidade de Bergen, Noruega. Durante sua extensa carreira, ele projetou e implementou muitos cursos sobre Java, desenvolvimento de sistemas orientados a objetos, desenvolvimento de aplicações web, segurança de software e técnicas de compiladores. Mughal ministrou seminários para a indústria de TI e é o autor principal de vários livros sobre Java.
