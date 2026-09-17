# Diagrama de Classes

<div style="text-align:center;">

![Diagrama de Classes](../../../../Assets/Subequipe02/DiagramaDeClasses.png)

<p><strong>Diagrama de Classes</strong> — estrutura de entidades, atributos, métodos e relacionamentos do domínio de busca de produtos. <em>Autor: Julia Oliveira Patricio</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe02/DiagramaDeClasses.png ':ignore')

</div>

## 1. Introdução

Este diagrama modela a estrutura estática do domínio de **busca de produtos**, operacionalizando as regras de negócio identificadas durante a fase de [Engenharia Reversa e BPMN](/Base/Relatórios/Subequipe02/ArtefatoGeneralista.md). Diferentemente do diagrama de sequência (que ilustrará a troca de mensagens no tempo), este artefato define a espinha dorsal do sistema, estabelecendo o que cada entidade conhece (atributos) e o que cada uma faz (métodos).

## 2. Entidades Principais

| Classe | Descrição |
|---|---|
| **Busca** | Centraliza o contexto da requisição feita pelo usuário, armazenando o termo digitado, a ordenação selecionada e gerenciando a aplicação de filtros. |
| **ResultadoBusca** | Representa o pacote de dados devolvido pelo servidor, isolando o controle de paginação, tempo de resposta e a coleção de itens encontrados. |
| **Produto** | Entidade central do catálogo, contendo as características do anúncio (preço, estoque, tipo de frete) exibidas na interface de resultados. |
| **Filtro (Abstrata)** | Superclasse que define o contrato base para afunilamento de resultados, garantindo polimorfismo na aplicação de regras de negócio. |
| **Usuario** | Ator que interage com o sistema, mantendo estado sobre suas preferências e disparando o fluxo principal. |

## 3. Relacionamentos e Senso Crítico (Decisões de Design)

- **Separação de Contexto e Payload:** A entidade `Busca` foi deliberadamente separada de `ResultadoBusca`. No sistema real, a requisição (o que o usuário quer) possui um ciclo de vida e responsabilidades diferentes da resposta (o que o servidor paginou e devolveu). Isso garante alta coesão estrutural.
- **Associação vs. Atributos Redundantes:** Em estrita observância à notação UML canônica, atributos tipados como coleções (ex: `List<Produto>`) foram suprimidos de dentro das caixas das classes, sendo substituídos pelas linhas de associação correspondentes com multiplicidade `*`.
- **Agregação (`o--`):** Utilizada entre `Busca` e `Filtro`, bem como entre `ResultadoBusca` e `Produto`. O losango vazado indica uma relação "todo-parte" sem dependência existencial obrigatória (ou seja, um `Produto` continua existindo no banco de dados independentemente de a busca ter sido encerrada ou destruída).
- **Generalização (`<|--`):** As derivações de `Filtro` (`FiltroPreco`, `FiltroFrete`, `FiltroMarca`) utilizam herança, permitindo que a classe `Busca` aplique múltiplas restrições diferentes utilizando a mesma assinatura abstrata do método `aplicar()`.

## 4. Notação utilizada

Classes representadas como retângulos divididos em três compartimentos (nome, atributos e métodos). Visibilidade demarcada formalmente (`+` público, `-` privado, `#` protegido). Enumerações utilizadas para agrupar domínios de valores primitivos (como `Ordenacao` e `TipoFrete`), limpando o núcleo do diagrama. Relacionamentos expressos através de associações direcionadas, agregações e generalizações da UML clássica.

## Embasamento na literatura

A escolha por omitir coleções de dentro da listagem de atributos e representá-las exclusivamente através de associações diretas e multiplicidade é fundamentada nas diretrizes de Larman (2004) para a transição de um Modelo de Domínio para um Diagrama de Classes de Projeto. Segundo o autor, a visibilidade e a navegabilidade em projetos orientados a objetos devem ser documentadas primariamente pelas conexões entre as classes, evitando redundâncias que poluem a abstração estrutural do sistema.

### Referências

LARMAN, Craig. **Utilizando UML e Padrões: Uma introdução à análise e ao projeto orientados a objetos e ao desenvolvimento iterativo**. 3. ed. Porto Alegre: Bookman, 2004.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Adiciona Diagrama de Classes, entidades, justificativas arquiteturais e referencial teórico | Julia Oliveira Patricio | -- |