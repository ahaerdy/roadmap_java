# Tratamento de Exceções em Java

O tratamento de exceções é um mecanismo poderoso que permite gerenciar erros em tempo de execução, impedindo que a aplicação quebre abruptamente. Em Java, uma exceção é um objeto que encapsula um evento anormal ocorrido durante a execução do código.

---

## 1. A Hierarquia das Exceções

Todas as exceções em Java herdam da classe `Throwable`. A estrutura divide-se principalmente em duas grandes ramificações:

- **Error:** Problemas graves que a aplicação não deve tentar capturar (ex: `OutOfMemoryError`).
- **Exception:** Condições que a aplicação pode e deve capturar. Divide-se em:
    - **Checked Exceptions (Exceções Verificadas):** São checadas em tempo de compilação. O compilador obriga você a tratá-las ou declará-las (ex: `IOException`, `SQLException`).
    - **Unchecked Exceptions (Exceções Não Verificadas):** Herdam de `RuntimeException`. Ocorrem por falhas de lógica do programador e não são checadas na compilação (ex: `NullPointerException`, `ArithmeticException`).

---

## 2. Palavras-Chave Universais

Para manipular esse fluxo, Java utiliza cinco palavras-chave essenciais:

| Palavra-chave | Função |
| --- | --- |
| **`try`** | Delimita o bloco de código onde uma exceção pode ocorrer. |
| **`catch`** | Captura e trata a exceção gerada no bloco `try`. |
| **`finally`** | Bloco opcional que **sempre** é executado, ideal para fechar recursos (como conexões de banco de dados). |
| **`throw`** | Lança explicitamente uma exceção no código. |
| **`throws`** | Declara na assinatura de um método que ele pode lançar determinadas exceções. |

---

## 3. Exemplos Práticos Comentados

### Exemplo 1: Estrutura Básica (`try-catch-finally`)

Este exemplo demonstra a captura de uma exceção aritmética comum (divisão por zero) e o comportamento do bloco `finally`.

```java
public class ExemploBasico {
    public static void main(String[] args) {
        System.out.println("Iniciando o programa...");

        try {
            int numerador = 10;
            int denominador = 0;
            // A linha abaixo causará uma ArithmeticException porque divisão por zero é indefinida
            int resultado = numerador / denominador; 
            
            // Esta linha NUNCA será executada se a linha anterior falhar
            System.out.println("Resultado: " + resultado); 

        } catch (ArithmeticException e) {
            // Este bloco captura exclusivamente erros aritméticos
            System.err.println("Erro capturado: Não é possível dividir um número por zero.");
            // Imprime a pilha de chamadas para ajudar no debug (opcional, mas recomendado)
            // e.printStackTrace(); 
            
        } finally {
            // O bloco finally SEMPRE roda, independentemente de ter ocorrido erro ou não
            System.out.println("Bloco finally executado: Limpando os recursos do sistema.");
        }

        System.out.println("Programa finalizado com sucesso (sem crash).");
    }
}

```

### Exemplo 2: Múltiplos Blocos `catch` e Try-with-Resources

Quando um bloco `try` pode lançar mais de um tipo de exceção, você pode empilhar blocos `catch`. **Regra de ouro:** As exceções mais específicas devem vir antes das mais genéricas.

Também usamos aqui o **Try-with-Resources** (introduzido no Java 7), que fecha os recursos automaticamente sem precisar do `finally`.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.FileNotFoundException;
import java.io.IOException;

public class MultiplosCatches {
    public static void main(String[] args) {
        // O recurso dentro dos parênteses do try será fechado automaticamente ao fim do bloco
        try (BufferedReader br = new BufferedReader(new FileReader("arquivo_inexistente.txt"))) {
            
            String linha = br.readLine(); // Pode lançar IOException
            int numero = Integer.parseInt(linha); // Pode lançar NumberFormatException
            System.out.println("Número lido: " + numero);

        } catch (FileNotFoundException e) {
            // Captura o erro específico de arquivo não encontrado (subclasse de IOException)
            System.err.println("Erro: O arquivo especificado não foi localizado.");
            
        } catch (IOException e) {
            // Captura erros genéricos de leitura/escrita (superclasse de FileNotFoundException)
            System.err.println("Erro de Entrada/Saída ao ler o arquivo.");
            
        } catch (NumberFormatException | NullPointerException e) {
            // Exemplo de Multi-Catch (Java 7+): Captura múltiplos tipos na mesma linha usando o operador '|'
            System.err.println("Erro nos dados: O conteúdo do arquivo não é um número válido ou está vazio.");
            
        } catch (Exception e) {
            // Catch genérico: Captura qualquer outra exceção não prevista acima. Sempre por último!
            System.err.println("Ocorreu um erro inesperado: " + e.getMessage());
        }
    }
}

```

### Exemplo 3: Propagando Exceções (`throws`) e Criando Exceções Customizadas

Criar suas próprias exceções torna o domínio do negócio muito mais claro. Para exceções de negócio, geralmente herdamos de `Exception` (Checked) ou `RuntimeException` (Unchecked).

```java
// 1. Criando uma Exceção de Negócio Customizada (Checked)
class SaldoInsuficienteException extends Exception {
    // Construtor que aceita uma mensagem customizada
    public SaldoInsuficienteException(String mensagem) {
        super(mensagem);
    }
}

// 2. Classe de Serviço que utiliza throw e throws
class ContaBancaria {
    private double saldo = 500.00;

    // O uso de 'throws' avisa quem chamar este método que ele é obrigado a tratar a exceção
    public void sacar(double valor) throws SaldoInsuficienteException {
        if (valor > saldo) {
            // Lança explicitamente a exceção se a condição de erro for atendida
            throw new SaldoInsuficienteException("Tentativa de saque de R$" + valor + ", mas o saldo é de apenas R$" + saldo);
        }
        saldo -= valor;
        System.out.println("Saque realizado! Novo saldo: R$" + saldo);
    }
}

// 3. Classe Executável manipulando o fluxo
public class ExemploCustomizado {
    public static void main(String[] args) {
        ContaBancaria conta = new ContaBancaria();

        try {
            conta.sacar(100.00); // Executa normalmente
            conta.sacar(600.00); // Vai disparar a nossa exceção customizada
            
        } catch (SaldoInsuficienteException e) {
            // Tratamento direcionado para a regra de negócio que quebrou
            System.err.println("Operação Negada! Motivo: " + e.getMessage());
        }
    }
}

```

---

## 4. Boas Práticas Rápidas

> ⚠️ **Evite blocos catch vazios:** Capturar exceções (`catch (Exception e) {}`) esconde bugs e torna a manutenção impossível. No mínimo, registre o erro em um log.
> 🎯 **Seja específico:** Evite capturar apenas a classe genérica `Exception` se você sabe exatamente qual erro pode acontecer (como `SQLException`). Isso evita mascarar outros problemas adjacentes.
> 🛡️ **Prefira Try-with-Resources:** Sempre que manipular arquivos, streams ou conexões de rede, utilize a sintaxe de inicialização dentro do `try (...)` para mitigar vazamentos de memória (*memory leaks*).