# Diagrama de Classes

<div style="text-align:center;">

![Diagrama de Classes](../../../../Assets/Subequipe02/DiagramaDeClasses.png)

<p><strong>Diagrama de Classes</strong> — estrutura de entidades, atributos, métodos e relacionamentos do domínio de busca de produtos. <em>Autor: Julia Oliveira Patricio</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe02/DiagramaDeClasses.png ':ignore')

</div>

## 1. Introdução

Este diagrama modela a estrutura estática do domínio de **busca de produtos**, dando forma às regras de negócio identificadas na fase de [Engenharia Reversa e BPMN](/Base/Relatórios/Subequipe02/ArtefatoGeneralista.md). Diferentemente do [Diagrama de Sequência](/Base/Relatórios/Subequipe02/DiagramaDeSequencia.md), que descreve a troca de mensagens ao longo do tempo, este artefato define a espinha dorsal do sistema: o que cada entidade conhece (atributos) e o que cada uma faz (métodos). As classes e métodos aqui definidos são a base estrutural sobre a qual o diagrama de sequência opera.

## 2. Entidades Principais

| Classe | Descrição |
|---|---|
| **Busca** | Centraliza o contexto da requisição feita pelo usuário, armazenando o termo digitado, a ordenação selecionada e gerenciando a aplicação de filtros. |
| **ResultadoBusca** | Representa o pacote de dados devolvido pelo servidor, isolando o controle de paginação, o tempo de resposta e a coleção de itens encontrados. |
| **Produto** | Entidade central do catálogo, reunindo as características do anúncio (preço, estoque, tipo de frete) exibidas na interface de resultados. |
| **Filtro (Abstrata)** | Superclasse que define o contrato base para o afunilamento de resultados, garantindo polimorfismo na aplicação das regras de negócio. |
| **Usuario** | Ator que interage com o sistema, mantendo suas preferências e disparando o fluxo principal. |

## 3. Relacionamentos e Senso Crítico (Decisões de Design)

- **Separação de contexto e payload:** a entidade `Busca` foi deliberadamente separada de `ResultadoBusca`. No sistema real, a requisição (o que o usuário quer) tem ciclo de vida e responsabilidades diferentes da resposta (o que o servidor paginou e devolveu) — separação que garante alta coesão estrutural.
- **Associação em vez de atributos redundantes:** em observância estrita à notação UML canônica, atributos tipados como coleções (por exemplo, `List<Produto>`) foram removidos de dentro das caixas das classes e substituídos pelas linhas de associação correspondentes, com multiplicidade `*`.
- **Agregação (`o--`):** usada entre `Busca` e `Filtro`, e também entre `ResultadoBusca` e `Produto`. O losango vazado indica uma relação "todo-parte" sem dependência existencial obrigatória — um `Produto`, por exemplo, continua existindo no banco de dados independentemente de a busca que o retornou ter sido encerrada ou destruída.
- **Generalização (`<|--`):** as derivações de `Filtro` (`FiltroPreco`, `FiltroFrete`, `FiltroMarca`) usam herança, permitindo que a classe `Busca` aplique diferentes restrições reutilizando a mesma assinatura abstrata do método `aplicar()`.

## 4. Notação Utilizada

As classes são representadas como retângulos divididos em três compartimentos (nome, atributos e métodos), com visibilidade demarcada formalmente (`+` público, `-` privado, `#` protegido). Enumerações agrupam domínios de valores primitivos, como `Ordenacao` e `TipoFrete`, mantendo o núcleo do diagrama limpo. Os relacionamentos são expressos por associações direcionadas, agregações e generalizações da UML clássica.

## Embasamento na literatura

A escolha de omitir coleções da listagem de atributos e representá-las exclusivamente por associações diretas com multiplicidade segue as diretrizes de Larman (2004) para a transição de um Modelo de Domínio a um Diagrama de Classes de Projeto. Segundo o autor, visibilidade e navegabilidade em projetos orientados a objetos devem ser documentadas primariamente pelas conexões entre as classes, evitando redundâncias que poluem a abstração estrutural do sistema.

### Referências

LARMAN, Craig. **Utilizando UML e Padrões: Uma introdução à análise e ao projeto orientados a objetos e ao desenvolvimento iterativo**. 3. ed. Porto Alegre: Bookman, 2004.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Adiciona Diagrama de Classes, entidades, justificativas arquiteturais e referencial teórico | Julia Oliveira Patricio | -- |