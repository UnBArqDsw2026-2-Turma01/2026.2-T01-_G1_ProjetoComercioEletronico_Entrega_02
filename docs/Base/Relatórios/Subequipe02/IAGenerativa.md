# Uso de IA Generativa e lições aprendidas

Este documento apresenta as **lições aprendidas** e o **uso de Inteligência Artificial Generativa** por cada membro da **Subequipe 2** ao longo do desenvolvimento do projeto.

---

## Maria Clara

### Lições aprendidas

Estruturar o relatório da subequipe antes de ter qualquer diagrama pronto deixou claro que modelagem estática e dinâmica exigem raciocínios diferentes mesmo quando descrevem o mesmo recorte do sistema (o fluxo de busca): o diagrama de casos de uso precisa expressar *o quê* o usuário consegue fazer e quais ações são obrigatórias (`include`) ou opcionais (`extend`), enquanto o diagrama de máquina de estados precisa expressar *como* o sistema se comporta internamente entre um evento e outro, incluindo estados de espera que não têm equivalente direto no diagrama de casos de uso.

Também aprendi, na prática, que um diagrama tecnicamente correto ainda pode ser malfeito visualmente: duas transições opostas entre os mesmos dois estados (`Buscando` ⇄ `ErroDeBusca`) geravam sobreposição de rótulos mesmo depois de várias tentativas de só aumentar o espaçamento do layout. A solução não foi ajustar espaçamento, e sim repensar a própria transição (o retorno do erro passou a levar a `Digitando`, e não de volta a `Buscando`), o que reforçou que problema de legibilidade de diagrama às vezes é sintoma de uma modelagem que pode ser simplificada.

### Uso de IA Generativa

A IA Generativa foi utilizada como apoio na elaboração dos diagramas de casos de uso e de máquina de estados, principalmente para conferência e para tirar algumas dúvidas menores, como sintaxe de Git.

---

## Guilherme Davila Rodrigues Carneiro Sampaio

### Lições aprendidas

O desenvolvimento dos diagramas de componentes e atividades mostrou a diferença entre representar a estrutura modular do sistema e representar o comportamento ao longo do tempo. No modelo estático, foi necessário explicitar interfaces, portas, dependências e responsabilidades; no modelo dinâmico, foi necessário representar partições, decisões e a concorrência entre a busca orgânica e os anúncios.

Também foi possível compreender que decisões como paralelismo, cache e isolamento do serviço de anúncios precisam ser acompanhadas de uma análise crítica. O uso de Fork/Join não garante desempenho por si só, e a separação de componentes não elimina preocupações com falhas, timeouts, observabilidade e consistência.

### Uso de IA Generativa

A IA Generativa foi utilizada para apoio da redação do relatorio que foi revisado e consulta sobre a sintaxe da UML para ver se eu tinha entendido e algumas vezes para ver se liguei corratamente os componentes.

---

## Julia Oliveira Patricio

### Lições aprendidas

Modelar o diagrama de classes antes do de sequência deixou claro que a visão estática só é realmente útil se antecipar as perguntas que a visão dinâmica vai fazer depois. Decisões que pareciam de estilo na hora de desenhar as classes — como não deixar `List<Produto>` como atributo e representá-la só por associação com multiplicidade `*` — se mostraram decisões estruturais quando cheguei no diagrama de sequência: uma coleção não é um objeto ativo, então ela nunca poderia ter sido uma lifeline capaz de "receber" uma mensagem como `buscarPorTermo()`. Precisei voltar e substituí-la por um repositório (`:IndiceCatalogo`) e por uma instância singular de `Produto` para o cálculo de desconto item a item, o que só ficou evidente porque os dois diagramas foram confrontados entre si.

O maior aprendizado, porém, foi sobre ordem cronológica em diagramas de sequência: eu tinha desenhado `aplicarFiltro()` e `ordenarPor()` *dentro* da execução de `executar()`, mas na prática o usuário configura filtro e ordenação na interface antes de clicar em buscar. Um diagrama pode estar sintaticamente correto e mesmo assim descrever uma sequência de eventos que não corresponde ao fluxo real do sistema — a UML não impede isso, quem precisa perceber é quem modela.

Por fim, comparar meu diagrama com um exemplo de referência (um fluxo de autenticação OAuth consagrado em uml-diagrams.org) foi útil para perceber convenções que eu tinha deixado de lado sem perceber, como a moldura `sd` com o rótulo do diagrama e a notação de guarda entre colchetes (`[condição]`) nos fragmentos `alt`/`opt` — detalhes que não mudam a semântica do diagrama, mas que fazem diferença na legibilidade e na aderência ao padrão OMG.

### Uso de IA Generativa

A IA Generativa foi utilizada principalmente como revisora crítica dos diagramas de classes e de sequência, não como geradora inicial do conteúdo. Descrevi o modelo já desenhado (em PlantUML) e pedi para que fossem apontados erros de notação UML, o que revelou problemas que eu não tinha percebido sozinha: uma lifeline de coleção (`List<Produto>`) sem capacidade de receber mensagens, uma violação do padrão MVC em que o Model (`ResultadoBusca`) enviava mensagens diretamente para o ator, e uma mensagem de criação (`<<create>>`) posicionada incorretamente no topo do diagrama em vez de no ponto exato em que o objeto nasce. Também usei a IA para comparar meu diagrama com um exemplo de referência de mercado e para revisar a redação final dos relatórios de Diagrama de Classes e Diagrama de Sequência, garantindo consistência de terminologia e rastreabilidade entre os dois artefatos. Todas as correções sugeridas foram validadas manualmente por mim antes de serem incorporadas, conferindo cada uma contra o diagrama de classes original e contra a literatura de referência (Larman, Jacobson) citada nos relatórios.

---

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Inclusão das lições aprendidas e do uso crítico de IA | Guilherme Davila|  Maria Clara  |
| 1.2 | 17/09/2026 | Inclusão das lições aprendidas e do uso crítico de IA de Maria Clara | Maria Clara | -- |
| 1.3 | 17/09/2026 | Inclusão das lições aprendidas e do uso crítico de IA de Julia Oliveira Patricio | Julia Oliveira Patricio | -- |