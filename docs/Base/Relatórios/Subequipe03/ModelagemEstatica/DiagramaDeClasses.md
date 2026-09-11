# Diagrama de Classes


<div style="text-align:center;">


![Diagrama de Classes](../../../../Assets/Subequipe3/DiagramaDeClasses3.png)

<p><strong>Diagrama de Classes</strong> — Diagrama de classes de um comércio eletrônico baseado no Mercado Livre. <em>Autor: José Joaquim da Silva Neto</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe3/DiagramaDeClasses3.png ':ignore')

</div>

## 1. Introdução

Este documento apresenta a fundamentação técnica e teórica das decisões de modelagem adotadas no diagrama de classes do sistema de comércio eletrônico. Mais do que descrever a estrutura das classes, o objetivo é justificar por que cada escolha de design foi feita, discutindo alternativas descartadas e ancorando as decisões em literatura.

O diagrama é composto por quatro categorias de elementos: Value Objects (tipos de valor autovalidados), enumerações, interfaces (aplicando o padrão Strategy) e classes de entidade (com identidade e ciclo de vida próprios). Cada uma dessas categorias resolve um problema de modelagem específico, detalhado a seguir.

---

## 2. Value Objects

### 2.1 A decisão

Atributos como `cpf`, `email`, `telefone`, `cep` e `preco` não foram modelados como `String` ou `double` soltos. Cada um recebeu uma classe própria, um Value Object, responsável por validar e encapsular seu próprio formato.

### 2.2 Justificativa crítica

Do ponto de vista puramente funcional, um atributo `String` "funciona": ele armazena o dado e o sistema roda. O problema não é funcional, é de garantia de invariante. Sem encapsulamento, nada impede que um CPF malformado, um e-mail sem `@`, ou um preço negativo por erro de cálculo exista em qualquer parte do sistema, a validação, se existir, fica espalhada (e frequentemente duplicada ou esquecida) em múltiplos pontos do código: no formulário do frontend, no controller da API, talvez no banco de dados.

Ao transformar esses dados em Value Objects cuja validação ocorre obrigatoriamente no construtor, o sistema passa a garantir, por construção, que é impossível representar um estado inválido, se um objeto `CPF` existe em memória, ele necessariamente é válido, porque não haveria como instanciá-lo de outra forma.

O caso de `Preco` merece destaque à parte: internamente ele armazena `centavos: long` em vez de `double`. Essa não é uma escolha estética é uma correção de um problema conhecido de representação numérica: valores de ponto flutuante não representam frações decimais com exatidão binária, o que pode gerar erros de arredondamento acumulados em operações financeiras sucessivas (somas, descontos, multiplicações por quantidade). Representar dinheiro como unidade inteira mínima (centavos) elimina essa classe de erro.

### 2.3 Trade-off reconhecido

A introdução de Value Objects aumenta o número de classes e exige disciplina de implementação (cada Value Object precisa reimplementar `equals()`, e a imutabilidade deve ser respeitada consistentemente). Para um sistema de escopo muito reduzido, isso pode ser desproporcional ao benefício. A decisão de adotá-los aqui se justifica pelo domínio ser transacional e financeiro, exatamente o cenário em que a literatura recomenda esse padrão com mais ênfase.

### 2.4 Embasamento na literatura

Evans (2003), em *Domain-Driven Design*, formaliza a distinção entre **Entity** (objeto com identidade contínua ao longo do tempo, como `Cliente` ou `Pedido`) e **Value Object** (objeto definido inteiramente por seus atributos, sem identidade própria, e que deveria ser imutável). Segundo essa perspectiva, atributos que representam conceitos do domínio com regras de validade — e não meros containers de dado — devem ser modelados como tipos próprios, e não como primitivos da linguagem de programação.

Fowler (2002), em *Patterns of Enterprise Application Architecture*, descreve explicitamente o padrão **Money**, recomendando que valores monetários nunca sejam representados como tipos de ponto flutuante nativos, justamente pelo problema de arredondamento citado acima — o que fundamenta diretamente a escolha de `Preco.centavos: long`.

---

## 3. Composição vs. Associação

### 3.1 A decisão

O diagrama distingue rigorosamente dois tipos de relacionamento estrutural: composição (losango preenchido) para partes cujo ciclo de vida depende inteiramente do todo (ex.: `Pedido ◆── ItemPedido`), e associação simples (seta aberta) para entidades que existem independentemente (ex.: `Produto` associado a `Categoria`).

### 3.2 Justificativa crítica

A tentação comum em modelagem é usar associação simples para tudo, por ser visualmente mais simples. Isso, porém, esconde uma informação semântica importante: quem é responsável por criar e destruir o quê. Se `ItemPedido` fosse modelado apenas como associação, o diagrama não comunicaria que um item de pedido nunca deveria existir sem um pedido, abrindo margem para implementações que permitem "itens órfãos" no banco de dados, uma inconsistência que a composição, ao menos na intenção do modelo, evita.

O critério aplicado consistentemente foi: **a parte tem sentido de existir sozinha no domínio?** Um `Produto` sim, ele existe no catálogo independentemente de estar em algum pedido. Um `ItemPedido`, não, ele é, por definição, um registro de "este produto, nesta quantidade, dentro deste pedido específico".

---

## 4. Imutabilidade do Pedido

### 4.1 A decisão

Apesar de terem estrutura idêntica, `ItemCarrinho` e `ItemPedido` foram modeladas como classes distintas, em vez de uma única classe reutilizada em ambos os contextos.

### 4.2 Justificativa crítica

Essa é talvez a decisão mais sutil do diagrama, e a mais fácil de questionar como redundante. A separação existe para resolver um problema real de consistência temporal: um carrinho de compras é, por natureza, **mutável** , o cliente pode alterar quantidades e remover itens livremente. Um pedido já confirmado, por outro lado, deveria ser **imutável** no que diz respeito ao que foi efetivamente comprado, por razões contábeis, fiscais e de auditoria.

Se `Pedido` apenas referenciasse os mesmos objetos `ItemCarrinho` do carrinho de origem (em vez de copiar os dados para novas instâncias de `ItemPedido` no momento de `finalizarCompra()`), uma alteração posterior no carrinho, reaproveitado pelo cliente para nova compra, por exemplo, propagaria indevidamente para o histórico de um pedido já fechado. A separação de classes, combinada com a cópia de dados na fronteira entre as duas, é o mecanismo que garante essa imutabilidade.

### 4.3 Trade-off reconhecido

O custo é duplicação estrutural: duas classes quase idênticas, com o risco de divergirem inadvertidamente se uma for alterada e a outra não (por exemplo, se um novo atributo for adicionado a `ItemCarrinho` e o desenvolvedor esquecer de replicá-lo em `ItemPedido`). Em um cenário de restrição rígida de número de classes, a unificação em uma única classe reutilizada é uma simplificação aceitável, desde que se documente explicitamente a garantia de que instâncias nunca são compartilhadas entre carrinho e pedido.

---

## 5. Interfaces

### 5.1 A decisão

Em vez de `Pagamento` conter lógica condicional para cada método de pagamento (`if metodo == PIX ... else if metodo == CARTAO ...`), o sistema depende de uma interface `ProcessadorPagamento`, implementada por classes concretas (`ProcessadorCartao`, `ProcessadorPix`). O mesmo padrão se aplica a `Notificador`, consumido por `Pedido` para avisar o cliente sobre mudanças de status.

### 5.2 Justificativa crítica

A alternativa mais simples, condicionais dentro de `Pagamento.processar()`, cresce em complexidade ciclomática a cada novo método de pagamento adicionado, e viola o princípio de que uma classe deveria ter um único motivo para mudar: `Pagamento` passaria a precisar ser modificada toda vez que uma nova operadora fosse integrada, mesmo que sua responsabilidade central (registrar e rastrear o pagamento de um pedido) não tivesse mudado.

Ao delegar o "como processar" para implementações da interface, `Pagamento` permanece estável mesmo quando novos métodos de pagamento são adicionados, a mudança fica isolada em uma nova classe (`ProcessadorBoleto`, por exemplo), sem tocar no código já existente e testado.

Um ponto de atenção que emergiu durante a própria elaboração deste diagrama: inicialmente, `Notificador` foi modelada sem nenhuma classe cliente de fato dependendo dela, um erro de modelagem identificado e corrigido ao se perceber que uma interface sem consumidor não deveria constar no diagrama de domínio, pois não representa uma dependência real do sistema, apenas uma possibilidade abstrata não conectada a nenhum fluxo.

---

## 6. Enumeração vs. Interface 

### 6.1 A decisão

`StatusPedido`, `MetodoPagamento`, `StatusPagamento` e `StatusEntrega` foram modeladas como enumerações (`«enumeration»`), não como interfaces.

### 6.2 Justificativa crítica

A confusão entre os dois é compreensível porque visualmente ambos aparecem como um retângulo com um estereótipo, mas representam conceitos ortogonais. Uma enumeração modela **dado**: um conjunto fechado e finito de valores possíveis que um atributo pode assumir. Uma interface modela **comportamento**: um contrato de métodos que classes concretas se comprometem a implementar, sem prescrever um conjunto fixo de variações.

Se `StatusPedido` fosse modelada como interface, cada status (`PAGO`, `CANCELADO`, etc.) exigiria uma classe concreta própria implementando-a uma sobre-engenharia desnecessária para um conceito que é.

---

## 7. Considerações finais

As decisões apresentadas não são as únicas tecnicamente válidas modelagem de software raramente tem resposta única. Um sistema com requisitos de simplicidade e prazo mais apertados poderia razoavelmente optar por atributos primitivos em vez de Value Objects, por uma única classe de item reutilizada em vez de `ItemCarrinho`/`ItemPedido` separadas, ou por condicionais simples em vez de Strategy Pattern para pagamento. A escolha feita aqui prioriza **explicitação de invariantes de domínio** e **baixo acoplamento entre responsabilidades**, características que a literatura de Domain-Driven Design e de padrões de projeto associa a sistemas mais fáceis de manter e evoluir — ao custo de maior número de classes e maior curva de entendimento inicial do modelo.


---


## Embasamento na literatura

### Referências

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 10/09/2026 | Criação da página | José Joaquim da Silva Neto | -- |
| 1.1 | 11/09/2026 | Adicionando Diagrama de Classes e documentação referente | José Joaquim da Silva Neto | -- |
