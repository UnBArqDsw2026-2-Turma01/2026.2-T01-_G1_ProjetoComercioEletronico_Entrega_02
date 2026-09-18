# Diagrama de Máquina de Estados

<iframe width="768" height="432" src="https://miro.com/app/live-embed/uXjVHluKsqg=/?embedMode=view_only_without_ui&moveToViewport=12,61,1492,815&embedId=648250601957" frameborder="0" scrolling="no" allow="fullscreen; clipboard-read; clipboard-write" allowfullscreen></iframe>

## 1. Introdução

Este documento apresenta a fundamentação técnica das decisões de modelagem adotadas no diagrama de estados do processo de Autenticação (login) do Mercado Livre. O objetivo não é apenas descrever os estados montados, mas justificar por que cada escolha foi feita, discutir alternativas descartadas e ancorar as decisões em literatura de modelagem comportamental.

O diagrama é composto por quatro categorias de elementos: estados (situações estáveis do processo, incluindo estados compostos), pseudo-estados de escolha (pontos de decisão dentro de um estado composto), transições (mudanças de estado disparadas por um evento ou condição) e os estados inicial/final que delimitam o processo. Cada categoria resolve um problema de modelagem específico, detalhado a seguir.

## 2. Decomposição em Estados

### 2.1 A decisão

O processo foi dividido em sete estados: Validando Credenciais (composto), Gerando Token de Acesso, Sessão Ativa, Finalizando Autenticação (composto), Notificando Usuário, Cancelando Autenticação e Tratando Erro.

### 2.2 Justificativa crítica

A alternativa mais simples seria representar o login como dois estados apenas, algo como Autenticando e Autenticado, afinal do ponto de vista do usuário o processo parece binário: ou ele está logado, ou não está. O problema dessa simplificação é que ela esconde exatamente os pontos em que o sistema se comporta de forma diferente dependendo do que aconteceu antes, que é o critério formal para justificar um novo estado: segundo Harel (1987), um estado deve existir quando ele representa uma configuração em que o sistema responde de maneira distinta a eventos futuros. Um "Autenticando" único não distingue entre estar validando a senha, esperando o token ser gerado ou gravando a sessão no banco, situações em que um erro ou um cancelamento levam a caminhos completamente diferentes.

A separação entre Gerando Token de Acesso e Sessão Ativa segue o mesmo raciocínio: são dois momentos em que uma falha tem origem e tratamento potencialmente diferentes (uma falha de token é um problema de autorização OAuth, uma falha em sessão ativa pode ser um problema de infraestrutura), e a literatura de máquinas de estado recomenda não colapsar dois momentos assim só porque, no caminho feliz, um sempre leva ao outro.

### 2.3 Trade-off reconhecido

Sete estados para um fluxo de login é, comparado a diagramas didáticos mais simples, uma granularidade relativamente fina. Um projeto com prazo mais apertado poderia razoavelmente unificar Gerando Token de Acesso e Sessão Ativa em um único estado "Sessão Estabelecida", já que a diferença entre os dois só importa para fins de depuração, não para o usuário final. A decisão de mantê-los separados se justifica pela mesma lógica de distinguibilidade comportamental descrita acima, mas é uma escolha discutível, não a única tecnicamente válida.

## 3. Estados Compostos e Pseudo-estados de Escolha

### 3.1 A decisão

Validando Credenciais e Finalizando Autenticação foram modelados como estados compostos, cada um com um sub-estado interno (Verificando Email e Senha, Registrando Sessão no Banco de Dados) e um pseudo-estado de escolha (losango) que decide entre repetir o sub-estado ou sair do estado composto.

### 3.2 Justificativa crítica

A alternativa mais direta seria uma transição de auto-laço, o próprio sub-estado apontando para ele mesmo em caso de falha (por exemplo, Verificando Email e Senha transitando para Verificando Email e Senha quando as credenciais são inválidas). Essa forma é tecnicamente válida em UML, mas ela conflita duas coisas que Harel trata como conceitualmente distintas em seu formalismo original de statecharts: a ação de verificar e a decisão sobre o resultado dessa verificação. O pseudo-estado de escolha separa essas duas responsabilidades, o sub-estado só executa a verificação, e o losango é quem decide o próximo passo, o que deixa o diagrama mais fiel à semântica de decisão da UML (equivalente ao nó de decisão em diagrama de atividades).

Vale registrar honestamente que essa troca também teve uma motivação prática: a primeira versão, com auto-laço, apresentou sobreposição visual entre os rótulos das transições na ferramenta usada para desenhar o diagrama. A mudança para pseudo-estado de escolha resolveu tanto o problema conceitual quanto o de legibilidade, mas o gatilho imediato para a mudança foi a renderização, não apenas a semântica.

O uso de estados compostos em si (em vez de expandir tudo em um único nível) segue diretamente o formalismo de Harel: estados compostos existem para esconder complexidade interna do fluxo externo, permitindo que quem lê o diagrama entenda a transição de alto nível (Validando Credenciais para Gerando Token de Acesso) sem precisar processar o detalhe de repetição interna.

### 3.3 Trade-off reconhecido

O pseudo-estado de escolha aumenta o número de elementos no diagrama em relação ao auto-laço direto (um losango extra por decisão). Para uma leitura rápida do fluxo, esse elemento adicional pode ser considerado ruído. A escolha de mantê-lo se justifica pela separação conceitual entre ação e decisão descrita acima, mas reconhecidamente também resolveu um problema de ferramenta, não só de modelagem.

## 4. Notação: Estados Inicial, Final e Transições Rotuladas

### 4.1 A decisão

O processo começa em um estado inicial (círculo preenchido) e termina em um estado final (círculo com anel), com toda transição rotulada com o evento ou condição que a dispara (por exemplo, "credenciais válidas", "token gerado", "erro").

### 4.2 Justificativa crítica

Essa notação segue diretamente o formalismo original de Harel (1987), posteriormente incorporado pela UML como a base dos diagramas de máquina de estados: cada transição é etiquetada pelo evento (ou condição de guarda) que causa a mudança de estado, nunca deixada implícita. A alternativa de setas sem rótulo até funcionaria para mostrar a ordem geral do processo, mas esconderia justamente a informação que diferencia um diagrama de estados de um simples fluxograma, o que precisa acontecer, e não apenas o que vem depois do quê.

### 4.3 Trade-off reconhecido

A notação UML completa permite rótulos no formato evento[guarda]/ação (por exemplo, erro[tentativas maior que 3]/registrarFalha()). O diagrama usou apenas o evento ou condição, sem formalizar guardas complexas nem ações explícitas de saída. Isso foi uma escolha deliberada de legibilidade para uma apresentação em sala de aula, mas é uma simplificação da notação completa, não a forma mais rigorosa que a UML permite.

## 5. Base Real: Fluxo OAuth 2.0 Versus Estados Genéricos

### 5.1 A decisão

Os estados e as transições do fluxo principal (Gerando Token de Acesso, com os eventos correspondentes a autorizar e trocar código por token) seguem o mesmo fluxo real de autenticação do Mercado Livre já usado no diagrama de componentes, documentado publicamente pela API deles (OAuth 2.0 com PKCE).

### 5.2 Justificativa crítica

A alternativa mais rápida seria criar estados genéricos plausíveis, como "Login iniciado" e "Login concluído", sem nenhuma correspondência com o mecanismo real de autenticação. O problema dessa abordagem é o mesmo já identificado na modelagem estática: estados genéricos não demonstram entendimento do sistema real, qualquer processo de login teria esses mesmos nomes. Ancorar o diagrama de estados no mesmo fluxo real já usado no diagrama de componentes também tem uma vantagem adicional específica da modelagem dinâmica: mantém consistência entre o artefato estático e o dinâmico, o Serviço de autenticação que "fornece" a operação autorizar() no diagrama de componentes é o mesmo autorizar() que aparece como transição aqui.

### 5.3 Limitação reconhecida

Vale a mesma ressalva já registrada no diagrama de componentes: o fluxo de OAuth 2.0 documentado publicamente pelo Mercado Livre é o de integração para desenvolvedores e vendedores terceiros, não necessariamente idêntico ao login interno de um comprador comum no site. Essa é uma simplificação assumida conscientemente, não um erro de pesquisa.

## 6. Convergência de Erros num Único Estado

### 6.1 A decisão

O estado Tratando Erro recebe transições vindas de três pontos diferentes do fluxo principal (Gerando Token de Acesso, Sessão Ativa e Finalizando Autenticação), em vez de existir um estado de erro separado para cada um.

### 6.2 Justificativa crítica

A alternativa seria um estado de erro específico por etapa (Erro ao Gerar Token, Erro de Sessão, Erro ao Registrar), o que preservaria a informação de origem da falha. A escolha por um único estado compartilhado segue um critério de minimização comum em modelagem de máquinas de estado: dois estados devem ser diferenciados apenas quando o comportamento subsequente do sistema também é diferente entre eles. Do ponto de vista do que acontece depois de qualquer uma dessas três falhas (informar o erro e encerrar o processo), o comportamento é idêntico, então mantê-los como um único estado reduz a complexidade do diagrama sem perder informação relevante ao nível de abstração escolhido.

### 6.3 Trade-off reconhecido

Essa convergência tem um custo real: ao chegar em Tratando Erro, o diagrama por si só não distingue qual das três falhas ocorreu, essa informação, se necessária, teria que vir de um parâmetro do evento ou de uma região adicional, fora do escopo deste modelo simplificado. Para um sistema em produção, essa diferenciação provavelmente seria necessária para fins de log e depuração.

## 7. Escopo Não Coberto: Regiões Concorrentes e Estado de Histórico

### 7.1 A decisão

O diagrama não usa regiões concorrentes (ortogonais) nem pseudo-estado de histórico, dois elementos que a UML de máquina de estados oferece e que o próprio material da disciplina exemplifica (o estado "Ligado" de um rádio relógio, com duas regiões simultâneas separadas por linha tracejada, e um pseudo-estado H marcando de qual sub-estado retomar ao ligar de novo).

### 7.2 Justificativa crítica

Região concorrente existe para representar comportamentos genuinamente independentes e simultâneos dentro do mesmo estado, como mostrar hora e tocar rádio ao mesmo tempo, sem que um dependa da conclusão do outro. O processo de autenticação modelado aqui não tem essa característica: validar credenciais, gerar token, manter sessão ativa e finalizar autenticação acontecem em sequência estrita, um depende do resultado do anterior para começar. Introduzir uma região concorrente aqui seria formalmente incorreto, não uma simplificação, já que o processo real não é paralelo nesse sentido.

O pseudo-estado de histórico existe para que, ao sair de um estado composto e depois voltar a entrar nele, o sistema retome o sub-estado exato em que estava, em vez de sempre reiniciar do sub-estado inicial padrão, como o rádio relógio que volta tocando rádio (não CD) se foi isso que estava tocando antes de desligar. No fluxo de autenticação modelado, essa necessidade não existe: se o usuário cancela ou o processo falha, o próximo login começa do zero, em Validando Credenciais, não há um ponto intermediário para retomar. Adicionar um estado de histórico aqui representaria um comportamento que o sistema não tem.

### 7.3 Trade-off reconhecido

Essa decisão foi verificada contra o processo específico modelado, não é uma omissão por desconhecimento da notação, ambos os elementos foram estudados a partir do próprio material de aula. Ainda assim, é uma decisão sensível a mudança de requisito: se o sistema real permitisse retomar uma autenticação interrompida exatamente de onde parou, ou se validação de credenciais e alguma verificação independente (por exemplo, checagem de fraude) pudessem rodar em paralelo, os dois elementos aqui descartados passariam a ser necessários.

## 8. Considerações finais

As decisões apresentadas não são as únicas tecnicamente válidas, modelagem de máquina de estados raramente tem resposta única, especialmente na escolha do nível de granularidade. Um projeto com prazo mais apertado poderia razoavelmente colapsar estados adjacentes do caminho feliz, usar auto-laços em vez de pseudo-estados de escolha, ou criar estados de erro genéricos sem base documental real. A escolha feita aqui prioriza distinguibilidade comportamental entre estados, separação conceitual entre ação e decisão, e fidelidade a um sistema real e verificável, consistente com o mesmo critério já aplicado na modelagem estática, ao custo de mais elementos no diagrama e maior esforço de pesquisa na etapa de modelagem.

### Referências

- Harel, D. (1987). *Statecharts: A Visual Formalism for Complex Systems*. Science of Computer Programming, 8(3), 231-274.
- OMG (Object Management Group). *Unified Modeling Language Specification*, seção de State Machine Diagrams.
- Booch, G., Rumbaugh, J., Jacobson, I. *The Unified Modeling Language User Guide*. Addison-Wesley.
- Material de aula: Arquitetura e Desenho de Software, Aula Modelagem UML Dinâmica, Profa. Milene Serrano.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 14/09/2026 | Criação da página | José Joaquim da Silva Neto | -- |
| 1.1 | 17/09/2026 | Criaçao de relatorio sobre Diagrama de estados da autenticação(Login) | João Paulo Barbosa Pereira Nunes | -- |