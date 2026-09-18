# Metodologia

## Sumário

1. [Introdução](#1-introdução)
2. [Metodologia adotada](#2-metodologia-adotada)
3. [Papéis](#3-papéis)
4. [Artefatos](#4-artefatos)
5. [Eventos](#5-eventos)
6. [Definição de Pronto (Definition of Done)](#6-definição-de-pronto-definition-of-done)
7. [Cronograma](#7-cronograma)
8. [Ferramentas utilizadas](#8-ferramentas-utilizadas)

---

## 1. Introdução

Este documento detalha o processo de trabalho em equipe adotado pela SubEquipe 02 para a elaboração do Módulo de Desenho de Software (Modelagem). O objetivo é registar a metodologia ágil adaptada, as práticas de comunicação e a divisão de tarefas que culminaram na entrega dos modelos estáticos e dinâmicos (Notação UML), garantindo a rastreabilidade e a transparência do processo de desenvolvimento.

---

## 2. Metodologia adotada

Adotou-se uma metodologia híbrida inspirada em práticas ágeis (Scrum), otimizada para o prazo de entrega.  A equipe optou por uma abordagem fortemente assíncrona. A coordenação e o acompanhamento das tarefas deram-se de forma contínua através de um grupo no WhatsApp após uma reunião inicial para repartição de funções e planejamento de datas. A distribuição de responsabilidades para a modelagem estática e dinâmica foi definida de forma imparcial através de um sorteio, garantindo uma divisão equitativa de carga de trabalho entre os membros.

---

## 3. Papéis

Apesar da colaboração integral de todos os membros nas discussões arquiteturais, os papéis foram distribuídos para otimizar as entregas individuais e a consolidação do repositório.

| Papel | Integrante(s) responsável(is) |
|---|---|
| **Coordenação / Integração Git** | Julia Oliveira Patricio, Maria Clara Sena, Guilherme Davila |
| **Garantia de Qualidade UML** | Julia Oliveira Patricio, Maria Clara Sena, Guilherme Davila |
| **Equipe de Modelagem Estática** | Julia Oliveira Patricio, Maria Clara Sena, Guilherme Davila |
| **Equipe de Modelagem Dinâmica** | Julia Oliveira Patricio, Maria Clara Sena, Guilherme Davila |

---

## 4. Artefatos

### 4.1 Backlog do Grupo

Lista priorizada de todos os diagramas e entregáveis do projeto.

<a id="Backlog"></a>

| # | Item | Categoria | Prioridade |
|---|---|---|---|
| 1 | Diagrama de Classes | Estático | Alta |
| 2 | Diagrama de Componentes | Estático | Média |
| 3 | Diagrama de Casos de Uso | Estático | Alta |
| 4 | Diagrama de Sequência | Dinâmico | Alta |
| 5 | Diagrama de Atividades | Dinâmico | Média |
| 6 | Diagrama de Máquina de Estados | Dinâmico | Média |
| 7 | Revisão geral e consolidação | Transversal | Alta |

### 4.2 Incremento

Um "incremento" neste ciclo de trabalho foi definido como um diagrama UML (estático ou dinâmico) completamente modelado, exportado em formato de imagem, acompanhado do seu respetivo documento em Markdown (contendo introdução, rastreabilidade, decisões arquiteturais embasadas e referências bibliográficas), e revisto para submissão no GitHub Pages.

---

## 5. Eventos

A comunicação assíncrona substituiu os ritos tradicionais longos para conferir celeridade à entrega:
- **Planejamento:** Reunião online via meet para o sorteio das atribuições, alinhamento de escopo (seção de busca).
- **Check-ins Contínuos:** Troca de mensagens no WhatsApp reportando impedimentos, validando decisões de design (ex: aplicação do padrão MVC) e partilhando versões de rascunho dos diagramas.
- **Review e Integração:** Revisão técnica mútua dos diagramas finalizados, validação da rastreabilidade entre modelos estáticos e dinâmicos, e submissão (commits) na *branch* correspondente para publicação.

---

## 6. Definição de Pronto (Definition of Done)

Um diagrama do backlog é considerado concluído (Pronto) quando atende a todos os seguintes critérios:
1. **Rigor Notacional:** Cumpre estritamente com as regras canónicas da notação UML 2.0 (associações, visibilidade, estereótipos, fluxos e criação de instâncias).
2. **Consistência:** Demonstra coerência direta com os outros artefatos (ex: Diagrama de Sequência utilizando os mesmos métodos definidos no Diagrama de Classes).
3. **Senso Crítico:** Possui documentação redigida no padrão exigido, explicitando justificações arquiteturais embasadas na literatura da Engenharia de Software.
4. **Submissão:** Está exportado em imagem (`.png`/`.jpg`), referenciado corretamente no Markdown sem quebrar a *sidebar*, e comitado no GitHub garantindo a coautoria.

---

## 7. Cronograma

A execução das tarefas foi concentrada num ciclo intensivo de três dias:
- **Terça-feira (15/09/2026):** Definição da divisão do trabalho (sorteio). Início do desenvolvimento da Modelagem Estática (Classes, Casos de Uso, Componentes).
- **Quarta-feira (16/09/2026):** Fecho da Modelagem Estática e transição para o desenvolvimento da Modelagem Dinâmica (Sequência, Atividades, Estados), assegurando que o dinâmico refletisse a estrutura concebida no dia anterior.
- **Quinta-feira (17/09/2026):** Refinamentos finais, correções de acoplamento (arquitetura MVC), elaboração das documentações Markdown de suporte, preenchimento da matriz de rastreabilidade e commits finais no repositório.

---

## 8. Ferramentas utilizadas

| Ferramenta | Finalidade |
|---|---|
| **WhatsApp** | Coordenação da equipe, planejamento ágil e check-ins diários assíncronos. |
| **Google Meet** | Coordenação da equipe, planejamento ágil, sorteio de tarefas. |
| **Draw.io / PlantUML** | Prototipagem, modelagem rigorosa e renderização dos diagramas UML estáticos e dinâmicos. |
| **Visual Studio Code (VS Code)** | Edição local dos ficheiros Markdown, visualização da árvore de diretórios e pre-visualização local. |
| **Git / GitHub** | Versionamento dos artefatos, registo de coautorias e gestão de configuração do projeto. |
| **GitHub Pages / Docsify** | Renderização da documentação final (este documento) para visualização pública e avaliação. |

---

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 17/09/2026 | Criação da página | Maria Clara | -- |
| 1.1 | 17/09/2026 | Preenchimento dos tópicos de metodologia, papéis, eventos e DoD | Julia Oliveira Patricio | -- |