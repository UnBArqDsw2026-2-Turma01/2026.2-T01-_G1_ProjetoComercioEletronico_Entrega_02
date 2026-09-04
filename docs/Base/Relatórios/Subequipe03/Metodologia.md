# Scrum Adaptado para Modelagem UML Estática e Dinâmica

## Sumário

1. [Introdução](#1-introdução)
2. [Por que Scrum para este projeto](#2-por-que-scrum-para-este-projeto)
3. [Visão geral do processo](#3-visão-geral-do-processo)
4. [Papéis (Roles)](#4-papéis-roles)
5. [Artefatos (Artifacts)](#5-artefatos-artifacts)
6. [Eventos (Ceremonies)](#6-eventos-ceremonies)
7. [Adaptações feitas ao Scrum tradicional](#7-adaptações-feitas-ao-scrum-tradicional)
8. [Definição de Pronto (Definition of Done)](#8-definição-de-pronto-definition-of-done)
9. [Estrutura das Sprints do projeto](#9-estrutura-das-sprints-do-projeto)
10. [Ferramentas utilizadas](#10-ferramentas-utilizadas)

---

## 1. Introdução

Este documento descreve a metodologia de gerenciamento de projeto adotada para o desenvolvimento do trabalho de **modelagem UML estática e dinâmica**, com prazo total de **duas semanas**. A metodologia escolhida foi o **Scrum**, adaptado à escala e à natureza acadêmica do projeto.

O objetivo desta documentação é registrar não apenas o cronograma, mas o **raciocínio metodológico** por trás das decisões de processo tomadas pelo grupo, servindo como referência para trabalhos futuros e para avaliação do processo empregado.

---

## 2. Por que Scrum para este projeto

Scrum é um framework ágil originalmente concebido para desenvolvimento de software, mas seus princípios de entregas incrementais, ciclos curtos de feedback, e adaptação contínua, se aplicam bem a qualquer projeto que possa ser dividido em etapas com valor entregável próprio.

No caso deste projeto, identificamos uma divisão natural do trabalho em dois grandes blocos:

- **Modelagem estática**: descreve a estrutura do sistema (classes, atributos, relacionamentos, componentes).
- **Modelagem dinâmica**: descreve o comportamento do sistema (casos de uso, fluxos, interações, estados).

Essa divisão se encaixa naturalmente em **duas Sprints sequenciais**, já que a modelagem dinâmica depende conceitualmente da estrutura estática estar bem definida (por exemplo, um diagrama de sequência referencia as classes já modeladas).

Vantagens do Scrum para este contexto específico:

- **Entregas parciais verificáveis**: ao final de cada semana, há um conjunto de diagramas completo e revisável, reduzindo o risco de acumular todo o trabalho para o fim do prazo.
- **Visibilidade do progresso**: reuniões curtas e recorrentes tornam claro o que já foi feito e o que falta, o que é especialmente útil em grupos com integrantes cursando múltiplas disciplinas em paralelo.
- **Espaço para correção de rota**: erros de modelagem (ex: relação mal definida) tendem a aparecer nas revisões cedo, antes de se propagarem para os diagramas dependentes.

---

## 3. Visão geral do processo

```
Product Backlog
      │
      ▼
Sprint 1 Planning ──► Sprint 1 (Modelagem Estática) ──► Sprint 1 Review ──► Sprint 1 Retrospective
                                                                                      │
                                                                                      ▼
Sprint 2 Planning ──► Sprint 2 (Modelagem Dinâmica) ──► Sprint 2 Review ──► Sprint 2 Retrospective
                                                                                      │
                                                                                      ▼
                                                                          Entrega final consolidada
```

O processo segue dois ciclos completos de Scrum, cada um com duração de uma semana, culminando em uma entrega final que integra os artefatos estáticos e dinâmicos em um único conjunto coerente de documentação UML.

---

## 4. Papéis (Roles)

Como se trata de um grupo pequeno e acadêmico, os papéis do Scrum tradicional foram adaptados para refletir a realidade da equipe, sem hierarquia rígida:

| Papel | Adaptação para o projeto |
|---|---|
| **Product Owner** | Responsável por interpretar os requisitos/especificação fornecidos pela disciplina e priorizar quais diagramas têm mais valor/urgência. Pode ser um integrante fixo ou rotativo. |
| **Scrum Master** | Responsável por conduzir as reuniões (Planning, Review, Retrospective), garantir que os prazos das tarefas estão sendo cumpridos e remover bloqueios (ex: dúvidas técnicas, acesso a ferramentas). |
| **Development Team** | Todos os integrantes do grupo, atuando de forma multidisciplinar — cada um pode contribuir tanto na modelagem estática quanto na dinâmica, ainda que haja divisão de responsabilidades por sprint. |


---

## 5. Artefatos (Artifacts)

### 5.1 Product Backlog

Lista priorizada de todos os diagramas e entregáveis do projeto. Serve como fonte única de verdade sobre o escopo total do trabalho.

| # | Item | Categoria | Prioridade |
|---|---|---|---|
| 1 | Levantamento de entidades e atores do domínio | Preparação | Alta |
| 2 | Diagrama de Classes | Estático | Alta |
| 3 | Diagrama de Objetos | Estático | Média |
| 4 | Diagrama de Componentes | Estático | Média |
| 5 | Diagrama de Pacotes | Estático | Baixa |
| 6 | Diagrama de Casos de Uso | Dinâmico | Alta |
| 7 | Diagrama de Sequência | Dinâmico | Alta |
| 8 | Diagrama de Atividades | Dinâmico | Média |
| 9 | Diagrama de Máquina de Estados | Dinâmico | Média |
| 10 | Revisão geral e consolidação | Transversal | Alta |

A priorização segue o critério de **dependência estrutural**: itens que servem de base para outros (como o Diagrama de Classes, referenciado pelo Diagrama de Sequência) recebem prioridade mais alta.

### 5.2 Sprint Backlog

Subconjunto do Product Backlog selecionado para cada Sprint, acompanhado de estimativas de tempo e responsáveis. É revisado e ajustado a cada Sprint Planning.

### 5.3 Incremento

Ao final de cada Sprint, o incremento é o conjunto de diagramas concluídos e revisados, que juntos formam uma parte coesa e utilizável da documentação UML final.

---

## 6. Eventos (Ceremonies)

### 6.1 Sprint Planning

Realizada no início de cada Sprint. Objetivo: transformar itens do Product Backlog em um Sprint Backlog acionável.

Estrutura adotada:
1. Alinhamento do objetivo da Sprint
2. Revisão coletiva dos requisitos/especificação
3. Refinamento do Sprint Backlog (ajuste de tarefas e estimativas)
4. Distribuição de responsabilidades
5. Definição dos canais de comunicação e ferramentas
6. Compromisso final de cada integrante

### 6.2 Daily Scrum (adaptado)

Como o grupo não trabalha em ambiente corporativo com disponibilidade full-time, o Daily Scrum foi adaptado para um **check-in assíncrono diário**, feito por mensagem em grupo (WhatsApp/Discord), respondendo três perguntas:

- O que modelei ontem?
- O que vou modelar hoje?
- Há algum bloqueio ou dúvida?

### 6.3 Sprint Review

Ao final de cada Sprint, o grupo apresenta os diagramas concluídos entre si (e, quando aplicável, ao professor/orientador), validando se a modelagem reflete corretamente o domínio do problema.

### 6.4 Sprint Retrospective

Reflexão sobre o processo de trabalho da Sprint, não sobre o conteúdo técnico dos diagramas. Perguntas-guia:

- O que funcionou bem no processo?
- Onde o grupo perdeu tempo ou teve dificuldade?
- O que ajustar para a próxima Sprint?

---

## 7. Adaptações feitas ao Scrum tradicional

O Scrum, em sua forma originalmente descrita no *Scrum Guide*, é pensado para equipes de desenvolvimento de software trabalhando em ciclos contínuos, frequentemente com Sprints de 2 a 4 semanas. Para este projeto acadêmico de curta duração, foram feitas as seguintes adaptações:

| Elemento tradicional | Adaptação |
|---|---|
| Sprint de 2-4 semanas | Sprints de 1 semana, dado o prazo total de 2 semanas |
| Daily Scrum presencial de 15 min | Check-in assíncrono por mensagem de texto |
| Product Owner como papel fixo | Papel pode ser rotativo ou compartilhado, dado o tamanho pequeno do grupo |
| Backlog gerido continuamente | Backlog definido majoritariamente no início, com pouco espaço para novos itens dado o prazo curto |
| Reuniões de Review com stakeholders externos | Review interno ao grupo, com validação ocasional do professor/orientador |

Essas adaptações preservam o espírito do framework — iteração, transparência e inspeção/adaptação contínua — sem impor overhead desnecessário a um projeto de escopo e prazo reduzidos.

---

## 8. Definição de Pronto (Definition of Done)

Um item do backlog é considerado concluído (Done) quando atende a todos os critérios abaixo:

- O diagrama foi revisado por, no mínimo, um integrante além do autor original
- Está consistente com os demais diagramas já produzidos (nomenclatura de classes, atores e fluxos coincide entre os diagramas)
- Foi exportado em formato adequado (imagem/PDF) e versionado no repositório do grupo (Git) ou pasta compartilhada
- Segue a notação UML padrão, sem elementos ambíguos ou não documentados

---

## 9. Estrutura das Sprints do projeto

### Sprint 1 — Modelagem Estática (Semana 1)

**Objetivo:** Ter a estrutura estática do sistema (classes, atributos, relações, componentes) completa e validada.

**Entregáveis:**
- Diagrama de Classes
- Diagrama de Objetos
- Diagrama de Componentes
- Diagrama de Pacotes (quando aplicável)

### Sprint 2 — Modelagem Dinâmica (Semana 2)

**Objetivo:** Representar o comportamento do sistema de forma coerente com a estrutura estática já definida na Sprint 1.

**Entregáveis:**
- Diagrama de Casos de Uso
- Diagrama de Sequência
- Diagrama de Atividades
- Diagrama de Máquina de Estados
- Consolidação final de toda a documentação UML

---

## 10. Ferramentas utilizadas

| Ferramenta | Finalidade |
|---|---|
| **Git / GitHub** | Versionamento dos artefatos e da documentação |
| **GitHub Pages** | Publicação da documentação final do projeto (este documento) |

---

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 04/09/2026 | Definição da metodologia | José Joaquim da Silva Neto | João Paulo Barbosa Pereira Nunes, Júlia Santana Campos e Pedro Henrique Gomes |
