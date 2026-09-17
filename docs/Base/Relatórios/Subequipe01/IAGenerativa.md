# IA Generativa — lições aprendidas e senso crítico

Conforme o Foco 03 de [1.1.1. SubEquipe_01](/Base/Relatórios/1.1.1.SubEquipe_01.md), cada integrante registra aqui, individualmente, o que aprendeu na modelagem UML desta entrega e como utilizou — e onde desconfiou de — IA generativa. Esta página segue o formato da [página equivalente da Entrega 1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G1_ProjetoComercioEletronico_Entrega_01/#/Base/Relat%C3%B3rios/SubEquipe01/IAGenerativa).

---

## Pedro Luciano de Azevedo

### Lições aprendidas

**A decisão mais importante do modelo de domínio já estava escrita.** `Produto` separado de `Anuncio` — item de catálogo *versus* oferta de um vendedor — é o que dá coerência a tudo: é por isso que o carrinho agrupa por vendedor, que o pedido tem um envio por vendedor, que a avaliação é sobre a oferta. Eu não inventei essa decisão nesta entrega. Ela estava na seção 7 da Engenharia Reversa da Entrega 1, *o que só aparece juntando os três recortes*, esperando alguém ler o recorte C do Patrick ao lado do meu recorte A. Modelagem, aprendi, é menos sobre criar abstrações e mais sobre reconhecer as que a observação já produziu.

**O diagrama de sequência explica falhas; não só as registra.** Na Entrega 1, RNF-A02 — preservar filtros *e posição* ao voltar à listagem — ficou registrado como requisito não satisfeito, e o SIG culpou a rolagem infinita com um `−−`. Era uma observação empírica: "a listagem reinicia do topo". O diagrama de sequência transformou isso numa explicação: a URL recebe a consulta com `pagina` na mensagem 14; a rolagem carrega páginas sem atualizar esse campo, porque não há transição de página do ponto de vista do comprador; o "voltar" reconstrói a consulta da URL e obtém a página 1. Filtro certo, posição errada, por construção. Entendi que a ordem temporal é um instrumento de diagnóstico, e que um modelo dinâmico bem feito é uma hipótese causal, não uma ilustração.

**Sequência e atividades respondem perguntas diferentes.** Escolhi sequência para o meu fluxo porque a pergunta era *que participante é responsável por cada resposta*, e o Guilherme ficou com atividades porque a pergunta dele era *sob que condição cada desfecho acontece*. São os dois diagramas dinâmicos mais parecidos da UML, e a escolha errada produziria um diagrama que não erra nada e não diz nada.

### Uso da IA Generativa

Durante o desenvolvimento desta entrega, utilizei ferramentas de IA generativa como apoio para consultas, esclarecimento de dúvidas sobre a notação UML, revisão de decisões de modelagem e obtenção de feedback sobre possíveis inconsistências nos diagramas. A elaboração do modelo, a análise dos requisitos e a validação das decisões foram conduzidas por mim, com base nos registros de engenharia reversa e nas evidências levantadas pela subequipe na Entrega 1.

A IA foi utilizada principalmente como uma ferramenta de consulta e apoio à revisão, ajudando a identificar pontos que poderiam estar incorretos, pouco claros ou que mereciam uma análise mais cuidadosa. Seu uso não substituiu a interpretação das evidências nem a avaliação crítica necessária para a construção dos modelos.

**Onde ajudou.** As consultas foram úteis para esclarecer dúvidas sobre elementos da notação UML, como composição, generalização, interfaces, relacionamentos `«include»` e `«extend»`, estados compostos, bifurcações e junções. Também utilizei a IA para obter feedback sobre decisões de modelagem e verificar se determinadas representações faziam sentido diante das informações documentadas. Um exemplo foi a discussão sobre a distinção entre `Produto` e `Anuncio`, na qual a consulta ajudou a retomar os argumentos registrados na seção 7 da nossa Engenharia Reversa. Nesse caso, o valor da ferramenta esteve em auxiliar a análise de um material que já havia sido produzido pela equipe, e não em estabelecer a decisão por conta própria.

**Onde exigiu revisão.** As respostas da IA nem sempre consideraram adequadamente o contexto específico do projeto. Em algumas consultas, foram apontadas representações que precisavam ser revistas, como a mistura de elementos de casos de uso em um diagrama de classes. Além disso, o feedback sobre a estrutura visual dos diagramas não substituiu a necessidade de analisar as imagens reais feitas por nós. O primeiro Diagrama de Classes, por exemplo, apresentava problemas de legibilidade, com largura excessiva e notas de rastreabilidade distantes das classes correspondentes. A identificação desses problemas dependeu da inspeção visual do resultado e da avaliação das necessidades de comunicação do modelo.

**Onde exigiu senso crítico.** Um dos principais cuidados foi distinguir informações efetivamente observadas durante a engenharia reversa de hipóteses e inferências sobre o funcionamento do sistema. A IA pode apresentar explicações plausíveis sobre comportamentos que não foram observados, mas isso não significa que essas explicações correspondam ao sistema real. Por exemplo, os acontecimentos posteriores ao estado `Pago`, como preparação, envio, entrega e disputa, não foram observados pela subequipe na Entrega 1. Da mesma forma, a possibilidade de reembolso automático por ausência de resposta do vendedor e a existência de novas tentativas após o estado `Recusado` são hipóteses que precisam ser diferenciadas dos achados documentados.

Esses exemplos reforçaram a importância de não tratar uma resposta convincente como evidência. As informações levantadas na engenharia reversa foram utilizadas como referência para avaliar as sugestões e identificar quais elementos tinham sustentação documental e quais precisavam ser tratados como inferências.

**O que passei a fazer.** Passei a utilizar a IA como uma ferramenta de apoio à análise, consultando-a quando surgiam dúvidas e solicitando feedback sobre possíveis erros, inconsistências ou pontos que poderiam ser melhorados. Entretanto, mantive a responsabilidade pela avaliação das respostas e de sua veracidade.

A principal lição que levo é que a IA pode contribuir para a revisão e o aprofundamento da análise, mas suas respostas precisam ser confrontadas com as evidências e com o conhecimento do domínio. Utilizá-la de forma crítica significa reconhecer que ela pode apontar problemas relevantes, mas também apresentar sugestões inadequadas ou explicações que não correspondem ao sistema analisado.

---

## Guilherme Costa Zanella

### Lições aprendidas

**O Rich Picture já continha dois diagramas UML.** A estrutura que Monk e Howard (1998) pedem — sete *stakeholders* e duas fronteiras — é a lista de atores do meu Diagrama de Casos de Uso; o processo de reclamação é o meu Diagrama de Atividades inteiro. Modelar, aqui, foi reler com outra gramática um desenho que já existia. E o registro sobreviveu à intenção: grafei a tensão entre loja oficial e vendedor autônomo para mostrar conflito, e ela acabou virando generalização de atores, que não tem nada a ver com conflito.

**`«include»` e `«extend»` são afirmações, não estilo.** A pergunta "acontece sempre ou só às vezes?" torna cada um dos doze relacionamentos do Diagrama de Casos de Uso verificável contra uma regra de negócio. No primeiro rascunho, `Descrever produto do zero` estava pendurado como `«include»`, e com isso o diagrama afirmava que toda publicação passa por descrição livre — o contrário de RN-C04, que é observada. Um estereótipo errado não deixa o diagrama feio; deixa-o falso.

**O mesmo fato admite duas modelagens certas, e a diferença é a pergunta.** Loja oficial × vendedor autônomo virou generalização de atores no meu diagrama e enumeração `TipoVendedor` no Diagrama de Classes do Pedro; `Atendimento` é ator fora do sistema no meu Diagrama de Casos de Uso e raia da plataforma no meu Diagrama de Atividades. O que a subequipe precisa não é convergência entre os seis diagramas — é o critério de cada divergência escrito onde ela aparece.

**Formalizar obriga a inventar.** Um Rich Picture pode parar em "resposta ou reembolso"; um diagrama de atividades não deixa o vendedor calado sem consequência. A regra do reembolso por prazo esgotado é minha, não da observação, e está marcada como inferida em três lugares — na nota dentro do SVG, na decisão 4 e na tabela de rastreabilidade. Declarar a dívida custou menos do que escondê-la.

**Diagrama de casos de uso não é checklist de requisitos.** Conferindo um a um, RF-A06 — sugerir termos alternativos quando a busca não retorna nada — ficou sem elipse, porque o estado vazio é resposta dentro de `Buscar produto`. A decisão é defensável, mas o requisito some do índice; anotar isso como limite não resolve o problema, só o deixa visível para quem vier depois.

### Uso da IA generativa

**O que pedi a ela.** Apoio de notação e conferência de consistência sobre os dois diagramas desta entrega, nunca conteúdo: aplicar o critério de estereótipo aos doze relacionamentos do Diagrama de Casos de Uso, redigir as guardas das três decisões do Diagrama de Atividades e cruzar os seis diagramas da subequipe, feitos por três pessoas em paralelo. O que cada um modelaria foi decidido antes, na [reunião de 12/09/2026](/ReunioesAtas/Subequipe1/Ata12_09.md), e não foi pauta de nenhuma consulta.

**Onde ajudou.** Em três frentes. Na regra formal da notação: a semântica de `fork` e `join` — a junção só prossegue quando todos os fluxos de entrada chegaram — é o que sustenta a decisão 2 do Diagrama de Atividades, e não era algo que eu soubesse antes de perguntar. Na consistência entre autores: conferir os cinco rótulos `[status = ...]` do meu diagrama contra a enumeração `StatusReclamacao` do Diagrama de Classes é trabalho mecânico, e é exatamente onde dois diagramas nossos poderiam divergir sem ninguém notar. E como contraditora: quando escrevi que a decisão `respondeu dentro do prazo?` ficava na raia do Vendedor, ela montou o argumento oposto — quem dispara o temporizador é a plataforma — com a mesma força. Não mudei a decisão; o *trade-off* escrito nela existe por causa do contra-argumento.

**Onde exigiu revisão.** O mapeamento de requisito para caso de uso saiu como se fosse um para um, e não é. A primeira versão da minha tabela de montagem dizia "RF-A01 a RF-A06" e "RF-C01 a RF-C04 viraram o pacote Venda"; conferindo contra o desenho, RF-A06 não tem caso de uso e os quatro RF-C sustentam três. As duas linhas foram corrigidas, e a lacuna virou limite declarado na página. O erro não era de notação: era leitura apressada de uma correspondência que soava óbvia.

**Onde exigiu senso crítico.** Ao pedir o fluxo completo de reclamação e mediação, ela devolveu prazo em dias, instância de recurso e etapa de arbitragem. Nenhum dos três tem origem em coisa alguma que a subequipe tenha levantado, e nenhum veio sinalizado como suposição. Só um sobreviveu — o do prazo —, e sobreviveu marcado como inferido; recurso e arbitragem ficaram de fora, porque acrescentar um segundo ciclo inteiro sobre a mesma ausência de evidência é pior do que deixar o diagrama incompleto e dizer que está. O Pedro aponta essa mesma hipótese na seção dele, o que é um bom sinal: a marcação sobreviveu à leitura de outra pessoa.

**O que passei a fazer.** Conferir contra o artefato, e não contra o texto. Cada número das legendas das figuras 6 e 7 foi contado no próprio SVG — sete atores, vinte e um casos de uso, dezesseis associações, doze relacionamentos, dezessete ações, três decisões, dois pares de bifurcação e junção, quatro nós finais —, e um deles estava errado no rascunho, perto o bastante do certo para atravessar uma leitura sem chamar atenção. É a regra que eu levo desta entrega: conferir texto contra texto não é conferir. O SVG, a tabela de regras da Engenharia Reversa e a enumeração do diagrama do colega respondem sim ou não; enquanto a verificação for feita em cima da própria resposta da IA, ela vai concordar consigo mesma.

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


## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0    | 16/09/2026 | Criação da página; lições aprendidas e senso crítico sobre o uso de IA generativa na modelagem UML | Pedro Luciano de Azevedo | --          |
| 1.1 | 17/09/2026 | Criação da página e redação do ponto de vista de Patrick Anderson sobre lições aprendidas e uso de IA generativa | Patrick Anderson | -- |
| 1.2 | 17/09/2026 | Redação do ponto de vista de Guilherme Costa Zanella sobre lições aprendidas e uso de IA generativa na modelagem do Diagrama de Casos de Uso e do Diagrama de Atividades | Guilherme Costa Zanella | -- |