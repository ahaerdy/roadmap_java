# Encapsulamento

Encapsulamento é um dos quatro conceitos fundamentais de POO (Programação Orientada a Objetos). Os outros três são herança, polimorfismo e abstração.

Encapsulamento em Java é um mecanismo de envolver os **dados** (variáveis) e o **código que age sobre esses dados** (métodos) juntos como uma única unidade. As variáveis de uma classe ficam ocultas de outras classes e só podem ser acessadas por meio dos métodos da própria classe — por isso o conceito também é chamado de **ocultação de dados** (*data hiding*).

Para implementar encapsulamento em Java:

* Declare as variáveis de uma classe como `private`.
* Forneça métodos `public` do tipo *getter* e *setter* para ler e modificar o valor dessas variáveis.

---

## Exemplo

A classe abaixo representa os dados de uma pessoa. Observe como os atributos são declarados como `private` e só podem ser manipulados pelos métodos públicos:

```java
/* File name : EncapTest.java */
public class EncapTest {

    // Atributos privados: ficam ocultos de outras classes.
    // Nenhuma outra classe pode lê-los ou alterá-los diretamente.
    private String name;    // nome da pessoa
    private String idNum;   // número de identificação
    private int age;        // idade

    // ------------------------------------------------------------------
    // Getters: métodos de LEITURA — retornam o valor de cada atributo.
    // O prefixo "get" seguido do nome do atributo (em CamelCase)
    // é a convenção JavaBeans adotada em todo o ecossistema Java.
    // ------------------------------------------------------------------

    // Retorna a idade atual armazenada no atributo 'age'
    public int getAge() {
        return age;
    }

    // Retorna o nome armazenado no atributo 'name'
    public String getName() {
        return name;
    }

    // Retorna o número de identificação armazenado em 'idNum'
    public String getIdNum() {
        return idNum;
    }

    // ------------------------------------------------------------------
    // Setters: métodos de ESCRITA — atribuem um novo valor a cada atributo.
    // Recebem o novo valor como parâmetro e o transferem para o atributo.
    // O prefixo "set" + nome do atributo também é convenção JavaBeans.
    // ------------------------------------------------------------------

    // Define uma nova idade. O parâmetro 'newAge' recebe o valor informado
    // pelo código externo e o salva no atributo privado 'age'.
    public void setAge(int newAge) {
        age = newAge;
    }

    // Define um novo nome. Mesma lógica do setter anterior.
    public void setName(String newName) {
        name = newName;
    }

    // Define um novo número de identificação.
    public void setIdNum(String newId) {
        idNum = newId;
    }
}
```

Os métodos `setXXX()` e `getXXX()` são os únicos **pontos de acesso** às variáveis de instância da classe `EncapTest`. Qualquer outra classe que precise ler ou alterar esses dados deve passar, obrigatoriamente, por esses métodos.

---

A classe abaixo demonstra como usar `EncapTest` a partir de outra classe:

```java
/* File name : RunEncap.java */
public class RunEncap {

    public static void main(String args[]) {

        // Cria um objeto do tipo EncapTest.
        // Neste momento, os atributos 'name', 'idNum' e 'age' existem
        // na memória, mas ainda não têm valores úteis (null / 0).
        EncapTest encap = new EncapTest();

        // Usa os setters para definir os valores dos atributos privados.
        // Tente acessar 'encap.name = "James"' diretamente — o compilador
        // bloqueará com erro, pois 'name' é private. Só o setter funciona.
        encap.setName("James");
        encap.setAge(20);
        encap.setIdNum("12343ms");

        // Usa os getters para LER os valores e exibi-los.
        // Novamente: 'encap.name' estaria inacessível aqui; o getter é
        // o único caminho permitido para obter o valor.
        System.out.print("Name : " + encap.getName() + " Age : " + encap.getAge());
    }
}
```

A execução deste código produzirá o seguinte resultado:

```text
Name : James Age : 20
```

---

## Benefícios do Encapsulamento

* **Controle de acesso:** os campos de uma classe podem ser configurados como somente leitura (*read-only*) — expondo apenas o getter — ou somente escrita (*write-only*) — expondo apenas o setter.
* **Validação centralizada:** a classe pode verificar e restringir o que é armazenado em seus campos. Por exemplo, um setter `setAge` pode rejeitar valores negativos antes de atribuí-los.
* **Independência interna:** os usuários de uma classe não precisam saber como ela armazena seus dados internamente. Se o tipo de um atributo mudar (de `int` para `long`, por exemplo), o código externo que usa os getters e setters não precisa ser alterado.