## Sobrecarga vs. Sobrescrita de Métodos

No universo da Programação Orientada a Objetos (POO), esses dois conceitos são pilares do **Polimorfismo**, mas eles funcionam de maneiras bem diferentes. Vamos decifrar cada um deles:

### 1. Sobrecarga de Métodos (*Method Overloading*)

A sobrecarga acontece **dentro da mesma classe**. Ela permite que você crie múltiplos métodos com o **mesmo nome**, desde que as listas de parâmetros sejam diferentes.

O compilador diferencia os métodos pela sua "assinatura" (os argumentos que ele recebe). Você pode mudar:

* A **quantidade** de parâmetros.
* O **tipo** dos parâmetros.
* A **ordem** dos parâmetros.

> 💡 **Exemplo prático:** Pense em um botão de "Enviar". Você pode ter um método `enviar(texto)` e outro `enviar(texto, anexo)`. O nome é o mesmo, mas a ação se adapta ao que você passa para ela.

### 2. Sobrescrita de Métodos (*Method Overriding*)

A sobrescrita acontece no contexto de **Herança** (entre uma classe mãe/superclasse e uma classe filha/subclasse). Ela ocorre quando a classe filha decide dar uma **assinatura e comportamento específicos** a um método que ela já herdou da classe mãe.

Para que a sobrescrita aconteça, a assinatura do método (nome e parâmetros) deve ser **idêntica** na classe mãe e na classe filha.

> 💡 **Exemplo prático:** Uma classe mãe chamada `Animal` tem o método `emitirSom()`. A classe filha `Cachorro` herda esse método, mas o *sobrescreve* para que o som específico seja "Au Au".

---

### Resumo Comparativo: Não confunda mais!

Para fixar de vez, veja a tabela comparativa abaixo:

| Característica | Sobrecarga (*Overloading*) | Sobrescrita (*Overriding*) |
| --- | --- | --- |
| **Onde ocorre?** | Na **mesma** classe. | Em classes **diferentes** (Herança: Mãe e Filha). |
| **Nome do método** | Igual. | Igual. |
| **Argumentos/Parâmetros** | **Obrigatoriamente diferentes** (em número, tipo ou ordem). | **Obrigatoriamente iguais**. |
| **Comportamento** | Cria um método novo (com outro propósito). | Modifica o comportamento de um método herdado. |
| **Momento da definição** | Em tempo de compilação (*Static Binding*). | Em tempo de execução (*Dynamic Binding*). |