# Diagramas de sequência: suposições e checklist

Gerados com apoio de IA a partir de `descricao-sistema.md`, `esclarecimento.md` e do diagrama de containers. Fontes: `sequencia-1-nascimento-do-requisito.mmd` e `sequencia-2-mudanca-com-o-carro-andando.mmd`.

## Por que dois diagramas

Um cobre o requisito nascendo (reunião com o stakeholder até o refinamento técnico). O outro cobre a mudança depois que a implementação começou. Juntar os dois daria um desenho grande demais para revisar.

## Suposições adotadas antes do código

1. A transcrição é colada no canal do projeto por uma pessoa (normalmente o PO). A ferramenta de reunião não faz isso.
2. Existe um canal de chat por projeto. É assim que o sistema sabe a qual projeto a transcrição pertence.
3. Quem aprova o requisito nascido da reunião com o stakeholder é o stakeholder. Quem aprova mudança de regra de negócio depois é o PO, com confirmação do stakeholder quando o pedido veio dele.
4. O sistema detecta mudança na spec por webhook do Git (commit em `specs/`) e compara com o requisito aprovado. A divergência vira pendência do tech lead, não bloqueio.
5. Mudança no Figma chega pelo MCP, lida pelo agente redator. Não existe webhook do Figma para o adaptador.
6. Mudança aprovada depois do início da implementação marca a spec como desatualizada e cria pendência para o tech lead. O sistema não altera a spec sozinho.
7. O dev é avisado por chat e pelo card. Não existe outro canal.
8. O exemplo usa "horas em HH:MM" e "coluna de saldo" só para dar concretude. Não são requisitos reais de nenhum sistema.

## Checklist de revisão

1. [ ] Cada diagrama cobre um caminho de sucesso e pelo menos um caminho de falha.
2. [ ] Em cada falha está claro que nada é aplicado sem aprovação.
3. [ ] Toda chamada a modelo passa pelo gateway; nenhum outro participante fala com modelo.
4. [ ] Toda escrita para fora (card, MR, chat) sai do orquestrador pelos adaptadores.
5. [ ] Toda aprovação e toda pendência são gravadas no registro de decisões, com quem, quando e canal.
6. [ ] A mudança tardia fica marcada como não planejada e aparece no card com autor e origem.
7. [ ] O dev é avisado quando o requisito muda depois do início da implementação.
8. [ ] A spec e os subcards ficam ligados ao requisito e são marcados quando ele muda.
9. [ ] O escalonamento para a gerência acontece só depois de prazo vencido e lembrete.
10. [ ] Os participantes usam os mesmos nomes do diagrama de containers.
11. [ ] Nenhum número de prazo foi inventado; o prazo é definido por projeto.
12. [ ] As suposições acima estão de acordo com o esclarecimento.
