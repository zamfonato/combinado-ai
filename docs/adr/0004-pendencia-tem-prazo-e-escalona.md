# ADR 0004: Pendência tem prazo e escalona, mas nada é aplicado sem aprovação

## Status

Proposto

## Contexto

Uma proposta de requisito fica parada se a pessoa com autoridade não responde. Se o sistema simplesmente esperar, o problema fica invisível, que é o que acontece hoje. Se o sistema aplicar a mudança depois de um prazo, quebra a regra de que requisito só muda com aprovação registrada.

## Decisão

Toda pendência (aprovação, confirmação de trecho de reunião, transcrição que não chegou, divergência entre spec e requisito) tem um prazo definido na configuração do projeto. Ao vencer, o sistema lembra a pessoa uma vez, no mesmo canal. Se vencer de novo, escala para a gerência com o histórico: o que está pendente, com quem e há quanto tempo. A mudança continua sem aplicar. O card mostra que existe mudança pendente. Fica registrado quem estava com a pendência e por quanto tempo.

## Alternativas consideradas

- Aplicar a mudança como "não aprovada" depois do prazo. Descartado porque coloca no requisito algo que ninguém com autoridade validou. O dev implementaria em cima de uma suposição.
- Só lembrar, sem escalar. Descartado porque não muda o comportamento de quem não responde. A visibilidade para a gerência é o que dá peso à pendência.
- Escalar direto, sem lembrete. Descartado por ser agressivo demais para o caso comum, que é esquecimento.

## Consequências

- A responsabilidade por requisito parado passa a ter nome e tempo, do mesmo jeito que um card parado tem.
- A gerência recebe mensagens do sistema. O volume precisa ser controlado; por isso o resumo semanal de pendências, e não uma mensagem por evento.
- O prazo é decisão de cada projeto. O sistema não tem valor padrão embutido.

## Como verificar

- Criar uma proposta em um projeto de teste com prazo de um dia e não responder. Conferir que o lembrete chega no dia seguinte, que a escalada chega no outro, e que o requisito não mudou.
- Conferir no registro de decisões que a pendência tem responsável, data de início e data de escalada.
