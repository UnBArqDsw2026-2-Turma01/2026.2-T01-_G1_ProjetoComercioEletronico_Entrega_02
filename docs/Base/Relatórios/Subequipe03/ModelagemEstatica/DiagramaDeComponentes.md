# Diagrama de Componentes

<iframe width="768" height="432" src="https://miro.com/app/live-embed/uXjVHoHZ5Wc=/?embedMode=view_only_without_ui&moveToViewport=-2,297,646,2289&embedId=260757789637" frameborder="0" scrolling="no" allow="fullscreen; clipboard-read; clipboard-write" allowfullscreen></iframe>

## 1. Introdução

Este documento apresenta a fundamentação técnica das decisões de modelagem adotadas no diagrama de componentes do processo de Login e Cadastro do Mercado Livre. O objetivo não é apenas descrever a estrutura montada, mas justificar por que cada escolha foi feita, discutir alternativas descartadas e ancorar as decisões em literatura de arquitetura de software.

O diagrama é composto por três categorias de elementos: componentes (unidades substituíveis de funcionalidade, estereotipadas `<<component>>`), interfaces (contratos fornecidos e requeridos, estereotipadas `<<interface>>`) e as relações de dependência entre eles. Cada categoria resolve um problema de modelagem específico, detalhado a seguir.

## 2. Decomposição em Componentes

### 2.1 A decisão

O sistema foi dividido em seis componentes: UI de login e cadastro, Serviço de autenticação, Serviço de cadastro, Banco de dados de usuários, Verificação e notificação e Provedor externo. Cada um encapsula uma responsabilidade que não se repete em nenhum outro.

### 2.2 Justificativa crítica

A alternativa mais simples seria tratar login e cadastro como um único bloco monolítico, afinal, do ponto de vista do usuário, ambos fazem parte de uma mesma tela e de um mesmo fluxo de entrada no sistema. O problema dessa simplificação é que autenticação e cadastro têm ciclos de mudança completamente diferentes na prática: o Mercado Livre pode alterar seu fluxo de OAuth 2.0 (autenticação) sem tocar em uma linha do processo de criação de conta, e vice versa. Um componente que muda por dois motivos distintos viola o princípio de que cada peça deveria ter um único motivo para mudar, e um único componente monolítico obrigaria qualquer alteração em um dos fluxos a arriscar regressão no outro.

A separação entre Verificação e notificação e Provedor externo segue o mesmo raciocínio, mas por um motivo adicional: o envio efetivo de e-mail e SMS depende de um serviço terceiro (o próprio provedor de e-mail/SMS), que é, por natureza, o elemento mais sujeito a troca (por custo, disponibilidade ou contrato comercial). Isolá-lo em um componente à parte significa que trocar de provedor não exige tocar na lógica de geração e validação de código de confirmação.

### 2.3 Trade-off reconhecido

Seis componentes para um fluxo de login e cadastro é, comparado a implementações reais mais enxutas, uma granularidade relativamente fina. Para um sistema de escopo pequeno, um projeto poderia razoavelmente unificar Verificação e notificação dentro do próprio Serviço de cadastro, já que hoje só o cadastro depende dela. A decisão de mantê-la separada se justifica pela mesma lógica de isolamento de motivo de mudança descrita acima, mas é uma escolha discutível, não a única tecnicamente válida.

## 3. Interfaces Fornecidas e Requeridas

### 3.1 A decisão

Nenhum componente depende diretamente da implementação de outro. Toda dependência passa por uma interface: o componente que oferece a funcionalidade a fornece (`()--`, notação lollipop) e o componente que precisa dela a requer (`..>`, dependência tracejada).

### 3.2 Justificativa crítica

A alternativa mais direta seria simplesmente desenhar uma seta entre os componentes que trocam informação (ex: UI aponta direto para Serviço de autenticação). Isso funcionaria para descrever o fluxo, mas esconderia uma informação que o diagrama de componentes existe justamente para explicitar: o contrato. Ao modelar a dependência como "UI requer a interface Autenticação" em vez de "UI depende do Serviço de autenticação", o diagrama comunica que a UI não precisa saber como a autenticação é implementada, só precisa que a interface exista e responda daquele jeito. Isso é o que a literatura de arquitetura de software chama de baixo acoplamento por contrato: se o Serviço de autenticação for reescrito internamente (por exemplo, trocando o fluxo de PKCE por outro mecanismo), nenhum componente que o consome precisa mudar, desde que a interface Autenticação continue sendo cumprida.

A cadeia de dependências resultante (UI requer Autenticação e Cadastro; ambos requerem Dados de usuários; Cadastro também requer Verificação e notificação, que requer Provedor externo) é acíclica: nenhum componente depende, direta ou indiretamente, de algo que dependa dele de volta. Essa propriedade não foi acidental, foi verificada durante a montagem do diagrama, exatamente porque um ciclo de dependência entre componentes torna impossível substituir ou testar um deles isoladamente.

### 3.3 Trade-off reconhecido

Modelar cada dependência por meio de uma interface explícita, em vez de uma associação direta entre componentes, dobra o número de elementos no diagrama (seis componentes viram onze elementos, com as cinco interfaces). Para uma apresentação rápida do sistema a um público não técnico, essa explicitação pode ser ruído desnecessário. A escolha de mantê-la se justifica pelo próprio objetivo da disciplina: o diagrama de componentes existe para mostrar fornecimento e requisição de interface, não apenas fluxo de dados.

## 4. Notação: Lollipop e Dependência

### 4.1 A decisão

Interface fornecida é representada por uma linha sólida entre o componente e um círculo/caixa `<<interface>>` (equivalente à notação de lollipop). Interface requerida é representada por uma seta tracejada com o rótulo "requer" apontando para essa mesma interface.

### 4.2 Justificativa crítica

Essa escolha segue diretamente a equivalência apresentada em aula: "quem requer versus quem oferece" corresponde a "quem depende versus quem realiza" no diagrama de classes. A alternativa seria usar a notação de ball and socket combinada (um símbolo único de bola encaixada em meia lua, como no exemplo de arquitetura cliente servidor do material da disciplina), mas essa forma exige uma ferramenta com suporte nativo a esse desenho específico. Como o Mermaid (usado para montar o diagrama) não reproduz fielmente esse símbolo combinado, foi adotada a forma equivalente de interface fornecida e requerida representadas separadamente, que é uma variação igualmente válida da mesma notação UML, e não uma simplificação do conteúdo, apenas da forma gráfica.

## 5. Operações: Base Real Versus Invenção

### 5.1 A decisão

As operações e atributos de cada componente não foram escolhidas arbitrariamente. O fluxo de cadastro (`solicitarEmail()`, `solicitarDadosPessoais()`, `validarCPF()`) segue o processo público real de criação de conta do Mercado Livre. O fluxo de autenticação (`autorizar()`, `trocarCodigoPorToken()`, `renovarToken()`) segue a documentação pública da API deles, que usa OAuth 2.0 com PKCE.

### 5.2 Justificativa crítica

A alternativa mais rápida seria preencher os componentes com operações genéricas plausíveis (`login()`, `cadastrar()`), suficientes para o diagrama parecer completo sem exigir pesquisa adicional. O problema dessa abordagem é que ela não distingue um diagrama de domínio de um diagrama decorativo: qualquer sistema de login poderia ter essas mesmas operações genéricas, o que não demonstra entendimento do sistema real modelado. Basear as operações em documentação pública do próprio Mercado Livre ancora o diagrama em uma fonte verificável, e não em suposição.

### 5.3 Limitação reconhecida

O fluxo de OAuth 2.0 documentado publicamente pelo Mercado Livre é o de integração para desenvolvedores e vendedores terceiros, não necessariamente idêntico ao login interno de um comprador comum no site, que provavelmente é mais simples (e mail e senha) e não tem documentação pública detalhada disponível. Essa é uma simplificação assumida conscientemente, e não um erro de pesquisa: na ausência de documentação pública do fluxo interno do comprador, o fluxo de desenvolvedor foi usado como a melhor aproximação real disponível.

## 6. Estereótipos e Compartimentos

### 6.1 A decisão

Cada componente traz o estereótipo `<<component>>` e cada interface o estereótipo `<<interface>>` acima do nome. Componentes exibem atributos e operações; interfaces exibem apenas operações (sem atributos).

### 6.2 Justificativa crítica

Sem o estereótipo explícito, uma caixa com nome, atributos e operações é visualmente indistinguível de uma classe comum de diagrama de classes, os dois usam a mesma estrutura de compartimentos. O estereótipo existe justamente para desambiguar isso.

A ausência de atributos nas interfaces não é uma omissão, é uma regra: uma interface define comportamento (o que deve ser cumprido), não estado (o que é armazenado internamente). Dar atributos a uma interface seria expor detalhe de implementação exatamente no elemento cuja função é escondê-lo.

## 7. Considerações finais

As decisões apresentadas não são as únicas tecnicamente válidas, modelagem de componentes raramente tem resposta única. Um sistema com prazo mais apertado poderia razoavelmente unificar Verificação e notificação dentro do Serviço de cadastro, usar associação direta entre componentes em vez de interfaces explícitas, ou preencher operações genéricas sem base documental real. A escolha feita aqui prioriza isolamento de responsabilidade por motivo de mudança, baixo acoplamento por contrato de interface e fidelidade a um sistema real e verificável, características que a literatura de arquitetura de software associa a sistemas mais fáceis de manter, testar e evoluir isoladamente, ao custo de mais elementos no diagrama e maior esforço de pesquisa na etapa de modelagem.

### Referências

- Szyperski, C. (2002). *Component Software: Beyond Object-Oriented Programming*. Addison-Wesley.
- Clements, P., Bass, L., Kazman, R. *Software Architecture in Practice*. Addison-Wesley.
- Martin, R. C. *Agile Software Development: Principles, Patterns, and Practices* (Princípio da Dependência Acíclica).
- OMG (Object Management Group). *Unified Modeling Language Specification*, seção de Component Diagrams.
- Material de aula: Arquitetura e Desenho de Software, Aula Modelagem UML Estática, Profa. Milene Serrano.

## Embasamento na literatura

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 10/09/2026 | Criação da página | José Joaquim da Silva Neto | -- |
| 1.1 | 11/09/2026 | Primeiro relatorio sobre a modelagem | João Paulo Barbosa Pereira Nunes | -- |
| 1.2 | 17/09/2026 | atualização do relatorio | João Paulo Barbosa Pereira Nunes | -- |
