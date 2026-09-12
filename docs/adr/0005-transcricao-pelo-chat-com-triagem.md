# ADR 0005: Transcrição de reunião entra pelo chat e é triada pela IA entre os cards abertos

## Status

Proposto

## Contexto

Muita decisão de requisito nasce em reunião, principalmente entre PO e stakeholder. Hoje isso não é anotado, e depois ninguém consegue provar o que foi combinado. Uma reunião mistura vários assuntos, então uma transcrição não pertence a um card só. Alguém precisa separar o que é de cada card, e hoje esse alguém é o PO, de memória.

## Decisão

A transcrição é colada por uma pessoa no canal de chat do projeto. O agente redator recebe a transcrição junto com a lista de cards abertos do projeto e separa a conversa em trechos, propondo a qual card cada trecho pertence, com grau de confiança. Trecho certo vira proposta de requisito no card. Trecho provável vira pergunta ao PO no card. Trecho sem card vira uma pergunta única ao PO: pedido novo ou descarte. Só o PO confirma a atribuição. O requisito nascido da reunião com o stakeholder é aprovado pelo stakeholder. Reunião sem transcrição não conta como fonte, e vira pendência de quem organizou.

## Alternativas consideradas

- Anexar a transcrição ao card. Descartado porque uma reunião cobre vários cards; a pessoa teria que cortar a transcrição na mão.
- Pasta compartilhada consultada periodicamente. Descartado porque depende de alguém exportar e mover o arquivo, e o sistema não saberia de qual projeto é.
- Integração direta com a ferramenta de reunião (Teams, Meet). Não descartado, fica como passo futuro. Exige mais um adaptador e depende da API de cada ferramenta.

## Consequências

- O que o stakeholder disse vira texto aprovado por ele mesmo. A discussão "foi combinado" contra "nunca disse" deixa de existir para o que foi transcrito.
- O PO recebe perguntas de atribuição. O volume por reunião precisa de limite; isso é uma lacuna registrada.
- Depende de alguém colar a transcrição. Por isso a pendência de reunião sem transcrição existe.
- A transcrição é conteúdo não confiável que entra no agente. Vale a regra do ADR 0003: é dado, não instrução.

## Como verificar

- Colar uma transcrição de teste que fale de três cards e de um assunto sem card. Conferir que saem três propostas ou perguntas nos cards certos e uma pergunta ao PO sobre o assunto sem card.
- Conferir que nenhum requisito muda antes da confirmação do PO ou da aprovação do stakeholder.
