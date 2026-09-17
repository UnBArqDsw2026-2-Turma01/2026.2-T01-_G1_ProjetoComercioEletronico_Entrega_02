# Diagrama de Componentes

<div style="text-align:center;">

![Diagrama de Componentes UML](../../../../Assets/Subequipe2/Diagrama%20de%20Componentes.drawio.svg)

<p><strong>Diagrama de Componentes UML</strong> — arquitetura modular do mecanismo de busca do comércio eletrônico.</p>

</div>

## Objetivo e escopo
O diagrama representa a organização estrutural do mecanismo de busca do catálogo de produtos. Seu escopo começa na interação do comprador com a interface e termina nos serviços responsáveis pela recuperação, ordenação e apresentação dos resultados, incluindo a oferta de produtos patrocinados.

## Leitura do diagrama

O componente de interface encaminha a consulta para o **Backend for Frontend (BFF)**, que atua como orquestrador e evita que a camada de apresentação conheça diretamente os serviços internos. O BFF utiliza o **cache Redis** para consultas recorrentes e consulta o **índice Elasticsearch** para recuperar os produtos correspondentes. Em seguida, o **Motor de Ranqueamento** ordena os resultados por relevância.

O fluxo de anúncios é representado separadamente pelo componente **Mercado Ads**. Essa separação evidencia que a busca orgânica e a publicidade possuem responsabilidades distintas, permitindo tratar a indisponibilidade do serviço de anúncios sem interromper a busca principal. As portas e interfaces do diagrama tornam explícitos os contratos entre os componentes e reduzem o acoplamento entre as implementações.

## Justificativas e senso crítico

O BFF foi escolhido como ponto de orquestração para concentrar a composição da resposta adequada ao cliente. O Redis reduz a latência de consultas repetidas, enquanto o Elasticsearch é apropriado para busca textual e recuperação por índice. A decomposição em componentes facilita a evolução independente dos serviços e a substituição de tecnologias, mas também introduz custos de operação, observabilidade e consistência de dados.


## Ferramenta
O diagrama foi elaborado no Draw.io.

![Print da edição do Diagrama de Componentes no Draw.io](../../../../Assets/Subequipe2/tela%20do%20drawio.png)

<p style="text-align:center;"><strong>Print da edição do Diagrama de Componentes no Draw.io</strong></p>

## Rastreabilidade e elos com outros artefatos

- O comportamento de busca e a interação entre a busca orgânica e os anúncios são detalhados no [Diagrama de Atividades](/Base/Relatórios/Subequipe02/ModelagemDinamica/DiagramaDeAtividades.md).
- O diagrama de componentes complementa o BPMN da Entrega 1: enquanto o BPMN apresenta o processo de negócio de forma ponta a ponta, este artefato mostra os componentes de software que colaboram para executar as atividades de busca, filtragem e apresentação dos resultados.


### Referências

- OBJECT MANAGEMENT GROUP. *OMG Unified Modeling Language (UML), Version 2.5.1*. 2017. Referência para componentes, interfaces, portas e dependências da notação UML.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Inclusão do diagrama, análise técnica,  e referências | Guilherme D'Avila |  Maria Clara  |
