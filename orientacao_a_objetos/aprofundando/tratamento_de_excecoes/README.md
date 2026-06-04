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
/**
 * Não há necessidade de 'import java.lang.ArithmeticException' ou 'java.lang.System' 
 * porque o pacote java.lang é importado de forma implícita e automática pelo compilador.
 */
public class ExemploBasico {
    public static void main(String[] args) {
        
        // Execução linear padrão do fluxo do programa
        System.out.println("Iniciando o programa...");

        /**
         * Bloco TRY (Tentar): 
         * Delimita uma "zona de monitoramento de risco". A JVM fica atenta a qualquer 
         * comportamento anômalo gerado pelas instruções contidas aqui dentro.
         */
        try {
            int numerador = 10;   // Aloca espaço na memória Stack para a variável primitiva 'numerador'
            int denominador = 0;  // Aloca espaço para a variável primitiva 'denominador'
            
            /**
             * PASSO CRÍTICO:
             * 1. A JVM tenta avaliar a expressão matemática da divisão.
             * 2. O processador/JVM detecta que o denominador é zero (operação indefinida).
             * 3. A execução normal DESTA LINHA É INTERROMPIDA IMEDIATAMENTE.
             * 4. Nos bastidores, a JVM faz algo como: throw new ArithmeticException("/ by zero");
             * 5. Um objeto de erro é instanciado na memória Heap contendo o Stack Trace (rastro do erro).
             */
            int resultado = numerador / denominador; 
            
            /**
             * LINHA IGNORADA/PULADA:
             * Como a linha de cima "lançou" uma exceção, o fluxo abandona o bloco 'try' na hora.
             * Esta instrução nunca será executada, pois o Java entrou em modo de busca por tratamento.
             */
            System.out.println("Resultado: " + resultado); 

        } 
        /**
         * Bloco CATCH (Capturar):
         * Aqui ocorre a DECLARAÇÃO da variável 'e'. É idêntico à assinatura de um método.
         * - 'ArithmeticException' define o TIPO de objeto que este bloco aceita tratar.
         * - 'e' é o NOME da referência local que apontará para o objeto de erro criado pela JVM.
         * O escopo de 'e' é restrito estritamente ao espaço entre as chaves deste catch.
         */
        catch (ArithmeticException e) {
            
            /**
             * TRATAMENTO DO ERRO:
             * O fluxo do programa é desviado para cá. O "crash" foi evitado.
             * Usamos 'System.err' em vez de 'System.out' para direcionar a mensagem ao fluxo de 
             * erro padrão (geralmente renderizado em vermelho em consoles/IDEs).
             */
            System.err.println("Erro capturado: Não é possível dividir um número por zero.");
            
            /**
             * Usando a variável 'e' declarada no catch:
             * e.getMessage() -> Extrai o texto interno da exceção (retornará "/ by zero").
             * e.printStackTrace() -> Se descomentado, imprime no console a linha exata onde o erro nasceu.
             */
            // System.err.println("Detalhe interno do erro: " + e.getMessage());
            // e.printStackTrace(); 
            
        } 
        /**
         * Bloco FINALLY (Finalmente):
         * Bloco de execução garantida. Ele funciona como uma cláusula de segurança.
         * Ele SERÁ executado se o 'try' rodar com sucesso, se o 'catch' capturar um erro, 
         * ou mesmo se ocorrer um erro não capturado ou um comando 'return'.
         * É o local ideal para fechar arquivos, conexões de banco ou desalocar recursos.
         */
        finally {
            System.out.println("Bloco finally executado: Limpando os recursos do sistema.");
        }

        /**
         * RETORNO AO FLUXO NORMAL:
         * Como a exceção foi devidamente capturada e tratada pelo bloco 'catch',
         * a JVM entende que a estabilidade foi restabelecida e continua executando
         * as linhas subsequentes do programa normalmente.
         */
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