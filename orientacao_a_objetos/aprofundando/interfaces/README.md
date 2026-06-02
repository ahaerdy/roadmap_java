# Interfaces

Uma interface em Java define **o que** uma classe deve ser capaz de fazer, sem ditar **como** ela deve fazer. Na prática, é uma lista de métodos que qualquer classe interessada em "assinar" aquela interface fica obrigada a implementar.

Pense em uma interface como um **contrato**: ao implementá-la, uma classe se compromete a fornecer todos os comportamentos listados nela. Se uma classe assina o contrato mas não cumpre algum método, o compilador acusa erro — o contrato é rígido.

Além de métodos, interfaces também podem conter **constantes** (variáveis `static final`).

Em Java, interfaces são o mecanismo que viabiliza dois recursos importantes:

- **Abstração** — quem usa a interface não precisa conhecer os detalhes de implementação da classe, apenas o que ela é capaz de fazer.
- **Herança múltipla** — ao contrário das classes, que só podem herdar de uma única classe-pai, uma classe pode implementar várias interfaces simultaneamente.

Veja também:

- [Interfaces em Java](./interfaces_em_java/)
- [Guia para Inerfaces em Java](./guia_para_interfaces_em_java/)