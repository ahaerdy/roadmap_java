# Encapsulamento

Encapsulamento é um dos quatro conceitos fundamentais de POO (Programação Orientada a Objetos). Os outros três são herança, polimorfismo e abstração.

Encapsulação em Java é um mecanismo de envolver os dados (variáveis) e o código que age sobre os dados (métodos) juntos como uma única unidade. No encapsulamento, as variáveis de uma classe serão ocultadas de outras classes e só poderão ser acessadas por meio dos métodos de sua classe atual. Por isso, também é conhecido como ocultação de dados (data hiding).

Para implementar um encapsulamento em Java −

* Declare as variáveis de uma classe como privadas (`private`).
* Forneça métodos setter e getter públicos (public) para modificar e visualizar os valores das variáveis.

## Exemplo

Segue um exemplo de como implementar o encapsulamento em Java:

```java
/* File name : EncapTest.java */
public class EncapTest {
   private String name;
   private String idNum;
   private int age;

   public int getAge() {
      return age;
   }

   public String getName() {
      return name;
   }

   public String getIdNum() {
      return idNum;
   }

   public void setAge( int newAge) {
      age = newAge;
   }

   public void setName(String newName) {
      name = newName;
   }

   public void setIdNum( String newId) {
      idNum = newId;
   }
}

```

Os métodos públicos `setXXX()` e `getXXX()` são os pontos de acesso para as variáveis de instância da classe `EncapTest`. Normalmente, esses métodos são referidos como getters e setters. Portanto, qualquer classe que queira acessar as variáveis deve acessá-las por meio desses getters e setters.

As variáveis da classe `EncapTest` podem ser acessadas usando o seguinte programa −

```java
/* File name : RunEncap.java */
public class RunEncap {

   public static void main(String args[]) {
      EncapTest encap = new EncapTest();
      encap.setName("James");
      encap.setAge(20);
      encap.setIdNum("12343ms");

      System.out.print("Name : " + encap.getName() + " Age : " + encap.getAge());
   }
}

```

A execução deste código produzirá o seguinte resultado:

```text
Name : James Age : 20

```

## Benefícios do Encapsulamento

* Os campos de uma classe podem ser configurados como somente leitura (read-only) ou somente escrita (write-only).
* A classe pode ter controle total sobre o que é armazenado em seus campos.
* Os usuários de uma classe não sabem como a classe armazena seus dados. Uma classe pode alterar o tipo de dado de um campo e os usuários da classe não precisam alterar nada em seu código.
