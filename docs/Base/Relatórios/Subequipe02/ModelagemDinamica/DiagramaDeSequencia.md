# Diagrama de Sequência

<div style="text-align:center;">

![Diagrama de Sequência](../../../../Assets/Subequipe02/DiagramaDeSequencia.png)

<p><strong>Diagrama de Sequência</strong> — troca de mensagens e fluxo temporal do processamento de busca, destacando o padrão Boundary-Control-Entity e o ciclo de vida dos objetos. <em>Autor: Julia Oliveira Patricio</em></p>

[Clique aqui para baixar a imagem!](../../../../Assets/Subequipe02/DiagramaDeSequencia.png ':ignore')

</div>

## 1. Introdução

Este diagrama modela a visão dinâmica da funcionalidade de **busca de produtos**, descrevendo como o sistema se comporta ao longo do tempo. Ele dá corpo, na prática, às regras de negócio já mapeadas na entrega base de [Engenharia Reversa e BPMN](/Base/Relatórios/Subequipe02/ArtefatoGeneralista.md), detalhando a interação entre o Usuário, o Frontend (Interface) e o Backend (serviços e banco de dados).

Este artefato é diretamente rastreável ao [Diagrama de Classes](/Base/Relatórios/Subequipe02/DiagramaDeClasses.md): cada lifeline corresponde a uma classe ou instância lá definida, e cada mensagem trocada aciona um método já especificado na estrutura estática do domínio.

## 2. Padrão Arquitetural e Linhas de Vida (Lifelines)

Para manter o baixo acoplamento entre as camadas, as linhas de vida foram modeladas segundo o padrão arquitetural **BCE (Boundary-Control-Entity)**, expresso nos estereótipos visuais do diagrama:

| Instância / Estereótipo | Descrição |
|---|---|
| **:Interface** `«boundary»` | Fronteira do sistema (Frontend): é a única lifeline autorizada a interagir com o `Usuario` e a renderizar dados na tela. |
| **busca : Busca** `«control»` | Classe controladora que orquestra a lógica de negócio — aciona consultas, aplica filtros e instancia o resultado. |
| **: IndiceCatalogo** `(Database)` | Infraestrutura de persistência otimizada para buscas textuais. |
| **filtro, produto, resultado** `«entity»` | Classes de domínio puro (Model): guardam estado e respondem apenas a operações internas de cálculo e validação. |

## 3. Fluxo de Mensagens Modelado

O fluxo cronológico foi segmentado nos seguintes fragmentos lógicos:

1. **Digitar (autocomplete):** interação em que o controlador gera objetos `SugestaoBusca` sem bloquear a interface.
2. **Configurar busca:** fragmentos `opt` mostram que as preferências do usuário (filtros e ordenação) alteram o estado do controlador *antes* de a busca ser executada.
3. **Processar e ranquear:** o controlador `Busca` delega a recuperação de dados ao `IndiceCatalogo` e itera sobre os produtos (`loop`), calculando descontos e aplicando as regras de negócio internamente.
4. **Exibir (decisão MVC):** o objeto `ResultadoBusca` é criado tardiamente (`<<create>>`). A `Interface` consulta esse objeto (`estaVazio()`) e decide entre renderizar a tela de produtos ou a tela de aviso de resultado vazio.
5. **Paginação:** evento opcional, gerenciado pelo próprio objeto de resultado retornado anteriormente.

## 4. Senso Crítico e Decisões de Design

- **Instanciação tardia (`<<create>>`):** o objeto `ResultadoBusca` não existe durante a digitação nem o processamento. Sua linha de vida só começa na etapa de exibição, refletindo um uso mais eficiente de memória.
- **Isolamento de responsabilidades (MVC):** em versões preliminares do modelo, os objetos de domínio injetavam dados diretamente na View — um anti-padrão arquitetural corrigido na versão final. Agora o Model (`ResultadoBusca`) apenas informa seu estado (`boolean`), e é o Boundary (`Interface`) quem assume as operações `exibirMensagemSemResultados()` e `exibirProdutos()`.

## Embasamento na literatura

A aplicação rigorosa dos estereótipos `«boundary»`, `«control»` e `«entity»` segue a metodologia de Jacobson et al. (1992) para a Engenharia de Software Orientada a Objetos (OOSE). Jacobson defende que separar os objetos responsáveis pela interface (boundary) daqueles que coordenam os fluxos (control) e daqueles que armazenam informação (entity) torna o sistema mais resiliente a mudanças — resiliência evidenciada aqui pelo isolamento das entidades em relação aos métodos de renderização visual.

### Referências

JACOBSON, Ivar et al. **Object-Oriented Software Engineering: A Use Case Driven Approach**. Boston: Addison-Wesley, 1992.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Adiciona Diagrama de Sequência final, documentação do padrão BCE, separação MVC e senso crítico | Julia Oliveira Patricio | -- |