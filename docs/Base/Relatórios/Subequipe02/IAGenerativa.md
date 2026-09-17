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

## Nome do Membro 3

### Lições aprendidas

### Uso de IA Generativa

---

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Inclusão das lições aprendidas e do uso crítico de IA | Guilherme Davila|  Maria Clara  |
| 1.2 | 17/09/2026 | Inclusão das lições aprendidas e do uso crítico de IA de Maria Clara | Maria Clara | -- |
