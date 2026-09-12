# Esclarecimento antes dos diagramas

Este arquivo foi gerado com apoio de IA a partir de `descricao-sistema.md`, seguindo o roteiro da Unidade III: antes de desenhar, listar o que está ambíguo e perguntar. As respostas são do autor. O que ficar sem resposta vira lacuna oficial.

## Lacunas encontradas na descrição

1. A descrição fala em "quem tem autoridade sobre aquele tipo de decisão", mas não diz como o sistema descobre isso.
2. Não está definido o que acontece quando ninguém responde a um pedido de aprovação.
3. O texto diz que reunião sem transcrição "é tempo jogado no lixo", mas não diz o que o sistema faz quando a reunião acontece e a transcrição não chega.
4. O Figma entrou como fonte, mas não está claro o que o sistema lê dele: comentários, versões de tela, ou os dois.
5. Não está definido quem pode propor mudança de requisito. Qualquer pessoa que comente no card, ou só PO e stakeholder.
6. O texto diz que o card aponta para o requisito, mas não diz como a ligação é criada.
7. Não está claro se o dev pode contestar um requisito aprovado e por onde.
8. A descrição menciona métricas e pendências visíveis, mas não diz onde elas aparecem, já que o sistema não tem interface própria.
9. Não está definido se o sistema atende um projeto ou vários ao mesmo tempo, com pessoas e regras diferentes.
10. Não está claro o que o agente redator faz quando o texto capturado é ambíguo demais para virar proposta.

## Perguntas, em ordem de impacto na arquitetura

Responda abaixo de cada pergunta. Se não souber ou se depender de cada empresa, escreva "fica como lacuna".

**1. Como o sistema sabe quem aprova o quê?**
Exemplo de opções: uma tabela por projeto (regra de negócio = PO, tela = design, prioridade = gerência); um campo no card; ou o próprio PO indica ao criar o card.

Resposta: Uma tabela de autoridade por projeto, definida na configuração do sistema e não no card. Regra de negócio é aprovada pelo PO. Comportamento de tela é aprovado pelo UX. Prioridade é aprovada pelo PO. 

O stakeholder aprova o requisito final quando o projeto exige isso. Se o tipo de decisão não estiver na tabela, o sistema pergunta ao PO.

**2. O que acontece quando a pessoa com autoridade não responde?**
Existe prazo? Depois do prazo, o sistema lembra, escala para a gerência, ou aplica a mudança marcada como "não aprovada"?

Resposta: Existe prazo, definido por projeto (por exemplo, dois dias úteis). Ao vencer, o sistema lembra a pessoa uma vez no mesmo canal. Se vencer de novo, escala para a gerência com o histórico da pendência. A mudança nunca é aplicada sem aprovação. Fica registrado quem estava com a pendência e por quanto tempo ficou com ela.

**3. Reunião sem transcrição: o sistema faz alguma coisa?**
Por exemplo, registra que a reunião aconteceu sem ata e deixa isso visível como pendência de quem organizou.

Resposta: Sim. Se um card ou uma decisão fizer referência a uma reunião e a transcrição não entrar no sistema, isso vira uma pendência de quem organizou a reunião, com o mesmo prazo e escalonamento das aprovações. Decisão de reunião sem transcrição não é aceita como fonte.

**4. O que o sistema lê do Figma?**
Só comentários, só mudanças de tela, ou os dois. E o Figma pode ser fonte de requisito ou só de divergência (a tela mostra algo que o requisito não diz)?

Resposta: Os dois. Comentários no Figma podem virar proposta de requisito como qualquer outro comentário. Mudanças de tela são usadas para apontar divergência. O agente lê a tela pelo MCP do Figma, entende os elementos e o fluxo, e compara com o card e com o requisito aprovado. 

Se a tela mostra algo que o requisito não diz, ou o contrário, o sistema abre uma pergunta para quem responde pelo design. O Figma não é fonte de verdade do requisito, o arquivo Markdown é. 

O MCP do Figma é uma integração do agente redator, passando pelas mesmas regras de acesso do gateway.

**5. Quem pode iniciar uma mudança de requisito?**
Qualquer comentário no card vira proposta, ou só de pessoas com papel definido.

Resposta: Qualquer pessoa com papel definido no projeto pode iniciar uma proposta. Comentário de quem não tem papel vira apenas uma pergunta registrada, sem virar proposta. Quem pode aprovar é sempre quem está na tabela de autoridade.

**6. Como um card fica ligado ao arquivo de requisito?**
Identificador no título do card, campo no card, link, ou o sistema cria o arquivo quando o card é criado.

Resposta: Pelo identificador do card. Todo board dá um identificador único para cada tarefa, e ele é fácil de filtrar em qualquer ferramenta. O arquivo de requisito leva esse identificador no nome (por exemplo, `1234-horas-extras.md`) e no cabeçalho. 

O sistema cria o arquivo quando o card entra em refinamento e escreve no card o link para o arquivo no repositório Git (GitLab, GitHub). 

Esse link aponta sempre para a versão aprovada, na branch principal, e o histórico do Git mostra todas as mudanças anteriores. Enquanto existe uma proposta de mudança em aberto, o sistema deixa no card um segundo link, para o merge request pendente. 

Se o card já existe sem arquivo, o PO aciona o sistema por uma tag no card. A ligação não depende de campo extra nem de convenção de título.

**7. O dev pode contestar um requisito aprovado?**
Se sim, por onde (comentário no card, comentário na proposta) e quem resolve.

Resposta: Sim. O dev comenta no card ou na proposta de mudança. O sistema abre uma nova proposta com a contestação, e quem resolve é a mesma autoridade do tipo de decisão. Se a contestação for técnica, a decisão é do tech lead. Se de negócio, do PO.

**8. Onde as pendências e métricas aparecem?**
Comentário no card, mensagem no chat, um relatório periódico, um painel simples do próprio sistema.

Resposta: No canal da pessoa. Pendência aparece como comentário no card ou mensagem no chat para o responsável. Um resumo semanal vai para a gerência com o que está vencido e com quem. O sistema não tem painel próprio nesta versão. Fica como lacuna se um painel será necessário depois.

**9. Um projeto ou vários?**
O sistema roda para um time só ou para vários projetos com pessoas e regras diferentes.

Resposta: Vários projetos, cada um com sua tabela de autoridade, prazos e ferramentas. Os adaptadores são configurados por projeto.

**10. Webhook ou consulta periódica para saber que algo mudou?**

Resposta: Webhook quando a ferramenta oferece (board, Git, chat). Consulta periódica só para o que não tem webhook (por exemplo, transcrições depositadas em uma pasta). Fica como lacuna o intervalo da consulta.

## Roteiro revisado (o que vai guiar os diagramas)

Escrito com base na descrição atual. Será ajustado depois das respostas.

- Escopo: capturar definições e mudanças de requisito vindas de board, comentários, chat compartilhado, Figma e transcrições de reunião; transformar em proposta versionada; coletar aprovação de quem tem autoridade; aplicar; manter card apontando para a versão aprovada; expor pendências.
- Nível: containers (C4) e sequência de uma jornada crítica.
- Limites: adaptadores por ferramenta; observador que só detecta mudança; orquestrador que guarda estado e decide fluxo; agente redator, único que usa modelo de linguagem; gateway de modelos; registro de decisões; repositório de requisitos em Git.
- Integrações externas: board, Git, chat, Figma, transcrição de reunião, provedores de modelo via gateway.
- Restrições: nenhuma mudança aplicada sem aprovação registrada; aprovação no canal da pessoa; conteúdo das fontes é dado, não instrução; só conversas compartilhadas; dados sensíveis não entram em requisito nem em log; toda ação irreversível registrada; pendência tem prazo e escalonamento.
- Lacunas: as que ficarem sem resposta acima.
