# Herança em Java com Exemplos

A herança em Java é o método para criar uma hierarquia entre classes ao herdar de outras classes.

A herança em Java é transitiva - portanto, se `Sedan` estende `Car` (Carro) e `Car` estende `Vehicle` (Veículo), então `Sedan` também herda da classe `Vehicle`. O `Vehicle` se torna a superclasse tanto de `Car` quanto de `Sedan`.

A herança é amplamente utilizada em aplicações Java, por exemplo, estendendo a classe Exception para criar uma classe de exceção específica da aplicação que contenha mais informações, como códigos de erro. Por exemplo, `NullPointerException`.

## Exemplo de Herança em Java

Cada classe em Java estende implicitamente a classe `java.lang.Object`. Portanto, a classe Object está no nível mais alto da hierarquia de herança em Java.

Vamos ver como implementar a herança em Java com um exemplo simples.

**Superclasse: Animal**

```java
// Define o pacote ao qual a classe pertence, organizando o código-fonte
package com.journaldev.inheritance;

/**
 * Classe base (Superclasse) Animal.
 * Serve como molde genérico para classes mais específicas (como Cat ou Dog).
 */
public class Animal {

	// Atributos privados: encapsulam os dados para que não sejam acessados diretamente de fora da classe
	private boolean vegetarian; // Indica se o animal é vegetariano (verdadeiro/falso)
	private String eats;        // Armazena o que o animal come
	private int noOfLegs;       // Armazena a quantidade de pernas do animal

	/**
	 * Construtor padrão (sem argumentos).
	 * Permite criar um objeto Animal sem definir os atributos imediatamente.
	 * Essencial para subclasses que não chamam explicitamente um construtor com parâmetros.
	 */
	public Animal(){}

	/**
	 * Construtor sobrecarregado (com argumentos).
	 * Permite inicializar um objeto Animal já definindo todos os seus atributos.
	 * @param veg  Define se é vegetariano
	 * @param food Define o tipo de alimentação
	 * @param legs Define o número de pernas
	 */
	public Animal(boolean veg, String food, int legs){
		// A palavra-chave 'this' diferencia o atributo da classe do parâmetro recebido
		this.vegetarian = veg;
		this.eats = food;
		this.noOfLegs = legs;
	}

	// ===================================================---======================
	// Métodos Getters e Setters (Interface pública para acessar os dados privados)
	// ======================================================---===================

	// Retorna o estado booleano do atributo vegetarian
	public boolean isVegetarian() {
		return vegetarian;
	}

	// Permite alterar o estado do atributo vegetarian com validação/controle se necessário
	public void setVegetarian(boolean vegetarian) {
		this.vegetarian = vegetarian;
	}

	// Retorna o que o animal come
	public String getEats() {
		return eats;
	}

	// Define ou altera o alimento do animal
	public void setEats(String eats) {
		this.eats = eats;
	}

	// Retorna a quantidade de pernas
	public int getNoOfLegs() {
		return noOfLegs;
	}

	// Define ou altera a quantidade de pernas
	public void setNoOfLegs(int noOfLegs) {
		this.noOfLegs = noOfLegs;
	}

}

```

O `Animal` é a classe base aqui. Vamos criar uma classe `Cat` (Gato) que herda da classe Animal.

**Subclasse: `Cat`**

```java
// Define o pacote ao qual a classe pertence, mantendo-a no mesmo diretório de Animal
package com.journaldev.inheritance;

/**
 * Subclasse Cat (Gato) que estende (extends) a Superclasse Animal.
 * Isso significa que Cat herda automaticamente todos os métodos e atributos não privados de Animal.
 */
public class Cat extends Animal {

	// Atributo específico da subclasse (Animal não possui 'color')
	private String color;

	/**
	 * Primeiro construtor sobrecarregado.
	 * Recebe os dados de herança e define uma cor padrão.
	 */
	public Cat(boolean veg, String food, int legs) {
		// A instrução super() invoca obrigatoriamente o construtor correspondente da Superclasse (Animal)
		// Ela repassa os parâmetros para inicializar os atributos privados vegetarian, eats e noOfLegs
		super(veg, food, legs);
		
		// Inicializa o atributo específico desta subclasse com um valor padrão
		this.color = "White";
	}

	/**
	 * Segundo construtor sobrecarregado.
	 * Recebe tanto os dados da Superclasse quanto a cor customizada para o gato.
	 */
	public Cat(boolean veg, String food, int legs, String color){
		// Invoca o construtor da Superclasse para tratar as propriedades genéricas de Animal
		super(veg, food, legs);
		
		// Inicializa o atributo de cor com o valor customizado recebido no parâmetro
		this.color = color;
	}

	// =========================================================================
	// Métodos Getters e Setters específicos da subclasse Cat
	// =========================================================================

	// Retorna a cor específica do gato
	public String getColor() {
		return color;
	}

	// Permite alterar a cor do gato
	public void setColor(String color) {
		this.color = color;
	}

}
```

Observe que estamos usando a palavra-chave `extends` para implementar a herança em Java.

## Programa de Teste de Herança em Java

Vamos escrever uma classe de teste simples para criar um objeto `Cat` e usar alguns de seus métodos.

```java
// Define o pacote ao qual a classe pertence
package com.journaldev.inheritance;

/**
 * Classe de teste para demonstrar a execução da herança na prática.
 */
public class AnimalInheritanceTest {

	// Ponto de entrada principal (main method) que inicia a execução do programa Java
	public static void main(String[] args) {
		
		// Instanciação do objeto: cria um objeto do tipo 'Cat' na memória Heap.
		// O construtor de Cat é invocado, o qual por sua vez aciona o construtor de Animal via super().
		// Parâmetros passados: vegetarian = false, eats = "milk", noOfLegs = 4, color = "black"
		Cat cat = new Cat(false, "milk", 4, "black");

		// Imprime se o gato é vegetariano.
		// O método isVegetarian() NÃO está declarado em Cat, mas é acessado aqui porque foi HERDADO de Animal.
		System.out.println("Cat is Vegetarian?" + cat.isVegetarian());
		
		// Imprime o que o gato come.
		// O método getEats() também é invocado diretamente a partir da superclasse Animal.
		System.out.println("Cat eats " + cat.getEats());
		
		// Imprime a quantidade de pernas.
		// Demonstra o reuso de código: a lógica de armazenamento e retorno do inteiro pertence a Animal.
		System.out.println("Cat has " + cat.getNoOfLegs() + " legs.");
		
		// Imprime a cor do gato.
		// O método getColor() é específico da subclasse Cat; a superclasse Animal não tem conhecimento dele.
		System.out.println("Cat color is " + cat.getColor());
	}

}
```

**Saída:**

A classe Cat não possui o método `getEats()`, mas, ainda assim, o programa funciona porque ele é herdado da classe Animal.

## Pontos Importantes

1. O reuso de código é o benefício mais importante da herança, porque as subclasses herdam as variáveis e os métodos da superclasse.
2. Membros `private` da superclasse não são diretamente acessíveis pela subclasse. Como neste exemplo, a variável `noOfLegs` de Animal não é acessível pela classe Cat, mas pode ser acessada indiretamente por meio dos métodos getter e setter.
3. Membros da superclasse com acesso padrão (default) são acessíveis pela subclasse APENAS se estiverem no mesmo pacote.
4. Construtores da superclasse não são herdados pela subclasse.
5. Se a superclasse não tiver um construtor padrão, então a subclasse também precisa ter um construtor explícito definido. Caso contrário, ela lançará uma exceção em tempo de compilação. No construtor da subclasse, a chamada para o construtor da superclasse é obrigatória nesse caso e deve ser a primeira instrução no construtor da subclasse.
6. **Java não suporta herança múltipla**, uma subclasse pode estender apenas uma classe. A classe Animal está estendendo implicitamente a classe Object e Cat está estendendo a classe Animal, mas devido à natureza transitiva da herança em Java, a classe Cat também estende a classe Object.
7. Podemos criar uma instância de uma subclasse e então atribuí-la a uma variável da superclasse, isso é chamado de **upcasting**. Abaixo está um exemplo simples de upcasting:

```java
Cat c = new Cat(); //instância da subclasse
Animal a = c; //upcasting, está correto já que Cat também é um Animal

```

8. Quando uma instância de uma Superclasse é atribuída a uma variável de uma Subclasse, isso é chamado de **downcasting**. Precisamos fazer o cast explicitamente para a Subclasse. Por exemplo:

```java
Cat c = new Cat();
Animal a = c;
Cat c1 = (Cat) a; //cast explícito, funciona bem porque "c" é na verdade do tipo Cat

```

Note que o Compilador não vai reclamar mesmo se estivermos fazendo isso de forma errada, por causa do cast explícito. Abaixo estão alguns dos casos em que será lançado `ClassCastException` em tempo de execução.

```java
Dog d = new Dog();
Animal a = d;
Cat c1 = (Cat) a; //ClassCastException em tempo de execução

Animal a1 = new Animal();
Cat c2 = (Cat) a1; //ClassCastException porque a1 é na verdade do tipo Animal em tempo de execução

```

9. Podemos sobrescrever (override) o método da Superclasse na Subclasse. No entanto, devemos sempre anotar o método sobrescrito com a anotação `@Override`. O compilador saberá que estamos sobrescrevendo um método e, se algo mudar no método da superclasse, receberemos um erro em tempo de compilação em vez de obter resultados indesejados em tempo de execução.
10. Podemos chamar os métodos da superclasse e acessar as variáveis da superclasse usando a palavra-chave **super**. Ela é útil quando temos uma variável/método com o mesmo nome na subclasse, mas queremos acessar a variável/método da superclasse. Isso também é usado quando Construtores são definidos na superclasse e na subclasse e temos que chamar explicitamente o construtor da superclasse.
11. Podemos usar a instrução `instanceof` para verificar a herança entre objetos, vamos ver isso com o exemplo abaixo.

```java
Cat c = new Cat();
Dog d = new Dog();
Animal an = c;

boolean flag = c instanceof Cat; //caso normal, retorna true

flag = c instanceof Animal; // retorna true já que c também é-um Animal

flag = an instanceof Cat; //retorna true porque an é do tipo Cat em tempo de execução

flag = an instanceof Dog; //retorna false por razões óbvias.

```

12. Não podemos estender classes `final` em java.
13. Se você não vai usar a Superclasse no código, ou seja, se a sua Superclasse é apenas uma base para manter código reutilizável, então você pode mantê-las como uma classe Abstrata (`Abstract class`) para evitar instanciação desnecessária por classes clientes. Isso também restringirá a criação de instâncias da classe base.
