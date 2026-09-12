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

Revisado em 12/09/2026. Itens conferidos contra os dois diagramas; correções aplicadas no mesmo commit.

1. [x] Cada diagrama cobre um caminho de sucesso e pelo menos um caminho de falha. No diagrama 1 as falhas são de ambiguidade (trecho provável, trecho sem card); a falha de prazo está só no diagrama 2, para não repetir.
2. [x] Em cada falha está claro que nada é aplicado sem aprovação.
3. [x] Toda chamada a modelo passa pelo gateway; nenhum outro participante fala com modelo.
4. [x] Toda escrita para fora (card, MR, chat) sai do orquestrador pelos adaptadores.
5. [x] Toda aprovação e toda pendência são gravadas no registro de decisões, com quem, quando e canal. Correção: no diagrama 1 faltava gravar a decisão do PO sobre o trecho sem card; incluído.
6. [x] A mudança tardia fica marcada como não planejada e aparece no card com autor e origem.
7. [x] O dev é avisado quando o requisito muda depois do início da implementação.
8. [x] A spec e os subcards ficam ligados ao requisito e são marcados quando ele muda.
9. [x] O escalonamento para a gerência acontece só depois de prazo vencido e lembrete.
10. [x] Os participantes usam os mesmos nomes do diagrama de containers. Correção: o Observador de fontes não aparecia; foi omitido de propósito para simplificar e agora há uma nota dizendo isso nos dois diagramas.
11. [x] Nenhum número de prazo foi inventado; o prazo é definido por projeto.
12. [x] As suposições acima estão de acordo com o esclarecimento.
