# Diagrama de Componentes

<iframe width="768" height="432" src="https://miro.com/app/live-embed/uXjVHoHZ5Wc=/?embedMode=view_only_without_ui&moveToViewport=-2,297,646,2289&embedId=260757789637" frameborder="0" scrolling="no" allow="fullscreen; clipboard-read; clipboard-write" allowfullscreen></iframe>

## Diagrama de Componentes UML
Como uma das entregas minimas, aqui será falado como o *Diagrama de Componentes* foi construido, aplicado ao mesmo domínio já modelado em BPMN: o processo de Login e Cadastro do Mercado Livre.

## Notação utilizada 
Estereótipo <<component>> para cada componente e <<interface>> para cada interface; interface fornecida representada por linha sólida (lollipop) e interface requerida por seta tracejada com o rótulo "requer", seguindo a equivalência apresentada em aula ("quem requer versus quem oferece" = "quem depende versus quem realiza").

## Componentes modelados 
UI de login e cadastro, Serviço de autenticação, Serviço de cadastro, Banco de dados de usuários, Verificação e notificação e Provedor externo, cada um com suas respectivas interfaces fornecidas e requeridas.

## Base real utilizada
As operações de cadastro seguem o fluxo oficial de criação de conta do Mercado Livre (e-mail, código de confirmação, CPF, validação de telefone, senha); as operações de autenticação seguem a documentação pública da API deles (OAuth 2.0 com PKCE).

## Embasamento na literatura

### Referências

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| -- | -- | -- | -- | -- |
| 1.0 | 10/09/2026 | Criação da página | José Joaquim da Silva Neto | -- |
