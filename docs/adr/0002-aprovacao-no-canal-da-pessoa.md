# ADR 0002: Aprovação acontece no canal da pessoa, nunca no Git

## Status

Proposto

## Contexto

O requisito é versionado em Git e muda por merge request. Quem aprova requisito é PO, stakeholder e UX, e essas pessoas não usam Git. Exigir que aprovem no GitLab ou GitHub seria trazer a fricção que o sistema existe para evitar.

## Decisão

O sistema abre e aplica o merge request sozinho. A pessoa aprova onde já trabalha: comentário no card ou resposta no chat. O adaptador captura a resposta, o orquestrador grava no registro de decisões (quem, quando, canal, fonte) e só então aplica o MR. O card recebe o link para a versão aprovada e, enquanto houver proposta em aberto, um segundo link para o MR pendente.

## Alternativas consideradas

- PO aprova o MR diretamente no GitLab/GitHub. Descartado porque exige conta, permissão de escrita e aprendizado de ferramenta para quem não é dev. Na prática ninguém aprovaria.
- Uma interface própria do sistema para aprovação. Descartado porque cria mais uma ferramenta para aprender e mais um lugar para olhar. A tese do sistema é não ter interface própria.

## Consequências

- O Git registra o merge com o sistema como autor. Quem aprovou de verdade está no registro de decisões, não no Git. Os dois precisam ser lidos juntos.
- O sistema precisa de uma conta de serviço com permissão de escrita no repositório. É uma credencial sensível e fica só no adaptador do Git.
- Uma aprovação dada por engano no card vale como aprovação. O sistema registra, e a correção é uma nova proposta.

## Como verificar

- Simular uma aprovação por comentário no card e conferir que o MR foi aplicado e que o registro tem autor, data e canal.
- Conferir que ninguém além da conta de serviço tem permissão de escrita na pasta `requisitos/`.
