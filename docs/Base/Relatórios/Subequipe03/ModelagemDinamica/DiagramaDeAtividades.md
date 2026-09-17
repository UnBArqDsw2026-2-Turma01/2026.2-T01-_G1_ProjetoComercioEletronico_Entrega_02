# Diagrama de Atividades

Esse documento contém os diagramas desenvolvidos pelos membros José Joaquim da Silva Neto e Pedro Henrique gomes.

## José Joaquim da Silva Neto

### Introdução
Os diagramas apresentados a seguir foram construídos com base no que fiz no diagrama de classes.

### Diagrama do Fluxo de Compra
<div style="text-align:center;">


![Diagrama de Classes](../../../../Assets/Subequipe3/DiagramaDeAtividades/FluxoCompraJoaquim.png)

<p><strong>Diagrama de Atividades</strong> — Diagrama de classes do fluxo de compra. <br> <em>Autor: José Joaquim da Silva Neto</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe3/DiagramaDeAtividades/FluxoCompraJoaquim.png ':ignore')

</div>

O diagrama central do sistema: cobre desde a navegação no catálogo até a avaliação pós-entrega. O processamento de pagamento está dentro de um laço `repeat`: se o pagamento for recusado, o cliente pode corrigir os dados e tentar novamente, sem sair do fluxo, só avançando para o cancelamento se desistir explicitamente. O caminho de sucesso atravessa todas as transições de status do pedido, com uma notificação disparada a cada etapa relevante.

### Diagrama de autenticação e gestão de endereços
<div style="text-align:center;">

![Diagrama de Classes](../../../../Assets/Subequipe3/DiagramaDeAtividades/EnderecosJoaquim.png)

<p><strong>Diagrama de Atividades</strong> — Diagrama de autenticação e gestão de endereços. <br> <em>Autor: José Joaquim da Silva Neto</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe3/DiagramaDeAtividades/EnderecosJoaquim.png ':ignore')

</div>

Modela o login do cliente. A validação de credenciais está dentro de um `repeat`: uma tentativa malsucedida permite nova tentativa sem reiniciar o diagrama; só ao desistir o cliente é direcionado ao fluxo de recuperação de senha.

Modela adicionar e remover endereços como uma decisão binária simples, com a validação do CEP atribuída ao construtor do Value Object correspondente.

### Diagrama de gestão do carrinho de compras
<div style="text-align:center;">

![Diagrama de Classes](../../../../Assets/Subequipe3/DiagramaDeAtividades/GestaoCarrinhoJoaquim.png)

<p><strong>Diagrama de Atividades</strong> — Diagrama de gestão do carrinho de compras. <br> <em>Autor: José Joaquim da Silva Neto</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe3/DiagramaDeAtividades/GestaoCarrinhoJoaquim.png ':ignore')

</div>


### Diagrama de gestão de produto e estoque 
<div style="text-align:center;">

![Diagrama de Classes](../../../../Assets/Subequipe3/DiagramaDeAtividades/GestaoProdutoEstoqueJoaquim.png)

<p><strong>Diagrama de Atividades</strong> — Diagrama de gestão do carrinho de compras. <br> <em>Autor: José Joaquim da Silva Neto</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe3/DiagramaDeAtividades/GestaoProdutoEstoqueJoaquim.png ':ignore')

</div>

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 14/09/2026 | Criação da página | José Joaquim da Silva Neto | -- |
| 1.1 | 17/09/2026 | Adiciona as imagens dos diagramas | José Joaquim da Silva Neto | -- |

