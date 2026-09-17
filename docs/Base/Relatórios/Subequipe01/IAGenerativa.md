# Uso de IA Generativa e Lições Aprendidas

O Foco 03 da entrega pede o ponto de vista **individual** de cada integrante sobre as lições aprendidas e sobre o uso de IA generativa, com senso crítico. Conforme a [ata de 12/09/2026](/ReunioesAtas/Subequipe1/Ata12_09.md), cada integrante da Subequipe 01 redige e commita a própria seção. O resumo de cada ponto de vista está no [relatório da subequipe](/Base/Relatórios/1.1.1.SubEquipe_01.md); o relato completo está aqui.

---

## Guilherme Costa Zanella

--

---

## Patrick Anderson

### Lições aprendidas

**O diagrama de componentes já estava dentro do BPMN, e eu não tinha percebido.** Na Entrega 1 desenhei os dois modelos BPMN e dividi a *pool* da plataforma em raias — Catálogo e Carrinho, Entrega, Pagamentos, Anúncios — porque a notação pede que se separe quem faz o quê. Só agora entendi que raia é, por definição, uma partição de responsabilidade dentro de um participante, que é exatamente o que um componente é. A lição não é sobre BPMN nem sobre UML: é que notações diferentes aplicadas à mesma evidência não são trabalho repetido. O BPMN responde *em que ordem*; o de componentes responde *quem precisa de quem*; a máquina de estados responde *em que situação o objeto está*. Foi preciso desenhar as três para ver que a terceira pergunta era a que faltava.

**Um detalhe de URL virou a decisão arquitetural mais forte do meu diagrama.** T-B04 é uma linha na tabela de transições da Entrega 1: clicar em "Adicionar novo endereço" leva a um formulário **em outro subdomínio**. Na hora, era só uma curiosidade anotada na etapa de observação da URL. Na modelagem, virou a única evidência observável de fronteira de implantação que a engenharia reversa de caixa-preta produziu, e é ela que sustenta `Serviço de Contas e Endereços` como componente próprio. Aprendi que o valor de um achado não se mede quando ele é registrado, e que isso é um argumento a favor de registrar mais do que parece necessário.

**Modelar estado obriga a preencher o que não foi observado.** O meu recorte parou antes da tela de pagamento, de propósito: avançar exigiria confirmar uma compra real. O BPMN conviveu bem com isso, porque um processo pode terminar em um evento de fim. Uma máquina de estados, não: se o `Pedido` fica `Pago`, alguma coisa acontece depois, e o diagrama tem de dizer o quê. Foi a primeira vez que a escolha da notação me forçou a assumir uma dívida com a evidência. A saída foi manter os estados e marcá-los como inferidos, na nota do diagrama e na seção de limites — mas a lição é anterior a isso: a notação não é neutra em relação ao que você pode deixar em branco.

**`entry`, `exit` e efeito de transição não são questão de estilo.** Escrevi `exit / liberar estoque` em `Aguardando pagamento` e só percebi o erro ao conferir o caminho feliz: uma ação de saída vale para *toda* saída, inclusive a que leva a `Autorizando`. O pedido teria liberado o estoque no exato momento em que o pagamento estava sendo autorizado. A correção foi mover a liberação para as três transições específicas que a exigem. Foi um erro de semântica, não de desenho, e nenhuma revisão visual o pegaria.

**Saber qual recurso da notação não usar vale tanto quanto usar.** RN-B01 diz que o pedido é logicamente múltiplo — um envio por vendedor. A UML tem o recurso certo para isso: regiões ortogonais. Não usei, porque o número de regiões dependeria de quantos vendedores o pedido tem, e regiões são estruturais e fixas; o correto seria uma submáquina por `Envio`, o que muda o sujeito do diagrama. Registrei a limitação em vez de inflar o desenho. As Diretrizes pedem para usar os vários recursos da notação, e a leitura fácil dessa frase é "use todos". A leitura que eu adotei é que usar um recurso sem que o modelo o sustente é pior do que não usar e explicar por quê.

### Uso da IA generativa (senso crítico)

**O que eu pedi a ela.** Nada de conteúdo. A evidência já era nossa: sessenta e um identificadores de regras, requisitos e transições levantados na Entrega 1, mais os dois BPMNs e o Rich Picture. Usei a IA como tradutora de notação — transcrever um achado codificado para a gramática de componentes ou de estados — e como conferidora de consistência entre seis diagramas feitos por três pessoas, onde o risco real é o mesmo conceito aparecer com três nomes.

**Onde ela foi boa.** Na gramática. Foi ela que apontou o problema do `exit / liberar estoque` descrito acima, e foi ela que insistiu em conferir cada estado do `Pedido` contra a enumeração `StatusPedido` do diagrama de classes do Pedro, o que evitou que dois diagramas nossos usassem nomes diferentes para a mesma coisa. IA é forte em regra formal — e notação é regra formal.

**Onde ela falhou, e é sempre o mesmo lugar.** Preencher lacuna com o plausível, sem sinalizar que está preenchendo. Ao pedir o ciclo de vida do pedido depois do pagamento, ela produziu uma sequência inteira — preparação, envio, entrega, disputa, reembolso — com a mesma segurança com que tinha citado RN-B07. Não havia nada no texto distinguindo o que vinha do nosso levantamento do que vinha do formato "marketplace" que ela aprendeu em outro lugar. Se eu não estivesse com a Engenharia Reversa aberta ao lado, aquilo teria entrado no diagrama como observação. É a mesma falha que eu já tinha registrado na Entrega 1, com uma diferença: lá ela inventou um detalhe; aqui ela inventou metade de um ciclo de vida, porque a pergunta era maior.

**Um caso concreto de verificação.** A legenda que eu herdei do rascunho desta página afirmava que a máquina do Pedido tinha "três estados finais distintos". Abrindo o SVG e lendo as transições uma a uma, há **um** estado final, alcançado por quatro caminhos — venda concluída, expiração, cancelamento e reembolso. A frase soava certa, era quase certa, e estava errada. Aprendi com isso que verificar texto contra texto não é verificar: a conferência tem de ser contra o artefato. Fiz o mesmo com cada `RN` citado nas minhas duas páginas, um a um, contra a tabela da Entrega 1.

**A conclusão que eu tiro.** A IA foi útil na proporção direta da quantidade de evidência que eu tinha para dar a ela. Sobre os meus recortes B e C, onde havia sessenta e um achados codificados, ela acertou quase tudo e me corrigiu duas vezes. Sobre o pós-pagamento, onde não havia nada, ela produziu texto igualmente fluente e igualmente convincente — e inteiramente inventado. O problema não é que ela erre; é que a fluência é a mesma nos dois casos, e nada no resultado distingue um do outro. Quem sustenta a distinção é o autor, e só consegue sustentá-la se tiver feito o levantamento antes.

---

## Pedro Luciano de Azevedo

--

---

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página e redação do ponto de vista de Patrick Anderson sobre lições aprendidas e uso de IA generativa | Patrick Anderson | -- |
