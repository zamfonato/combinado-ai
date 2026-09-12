# ADR 0001: Requisitos ficam na pasta requisitos/ do repositório de código

## Status

Proposto

## Contexto

O requisito precisa viver em algum lugar versionado. As opções eram um repositório documental único para todos os projetos, um repositório de requisitos por projeto, ou uma pasta dentro do repositório de código de cada projeto. O dev vai trabalhar com um agente de IA dentro do repositório de código, e esse agente só enxerga o que está lá.

## Decisão

Cada projeto tem uma pasta `requisitos/` dentro do seu repositório de código, com um arquivo Markdown por card, nomeado pelo identificador do card (por exemplo, `1234-horas-extras.md`). Merge requests de requisito recebem o prefixo ou label `requisito` para não se misturar com os de código. A configuração do projeto pode apontar para um repositório separado quando a permissão exigir, por exemplo um cliente que aprova requisito mas não pode ver código.

## Alternativas consideradas

- Repositório documental único (monorepo só de documentação). Descartado porque permissão no Git é por repositório, não por pasta. Time A veria os documentos do time B.
- Um repositório de requisitos por projeto, separado do código. Descartado porque o requisito ficaria fora do contexto que o agente do dev lê. Alguém teria que lembrar de trazer. Fica como exceção configurável.

## Consequências

- O agente do dev lê requisito e código no mesmo lugar, sem passo extra.
- PO e stakeholder precisam de permissão de leitura no repositório de código para abrir o link do card. É um papel só de leitura, sem risco.
- Projeto com front e back em repositórios separados precisa definir em qual deles fica a pasta. Isso é uma lacuna registrada na descrição do sistema.
- A fila de merge requests do repositório passa a ter MRs de requisito. O label resolve o filtro.

## Como verificar

- Em um projeto de teste, abrir o repositório com um agente de codificação e pedir para ele implementar um card. Ele deve encontrar o requisito sem instrução adicional.
- Confirmar que um usuário com papel de leitura consegue abrir o link do card e não consegue alterar nada.
