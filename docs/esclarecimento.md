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
Resposta:

**2. O que acontece quando a pessoa com autoridade não responde?**
Existe prazo? Depois do prazo, o sistema lembra, escala para a gerência, ou aplica a mudança marcada como "não aprovada"?
Resposta:

**3. Reunião sem transcrição: o sistema faz alguma coisa?**
Por exemplo, registra que a reunião aconteceu sem ata e deixa isso visível como pendência de quem organizou.
Resposta:

**4. O que o sistema lê do Figma?**
Só comentários, só mudanças de tela, ou os dois. E o Figma pode ser fonte de requisito ou só de divergência (a tela mostra algo que o requisito não diz)?
Resposta:

**5. Quem pode iniciar uma mudança de requisito?**
Qualquer comentário no card vira proposta, ou só de pessoas com papel definido.
Resposta:

**6. Como um card fica ligado ao arquivo de requisito?**
Identificador no título do card, campo no card, link, ou o sistema cria o arquivo quando o card é criado.
Resposta:

**7. O dev pode contestar um requisito aprovado?**
Se sim, por onde (comentário no card, comentário na proposta) e quem resolve.
Resposta:

**8. Onde as pendências e métricas aparecem?**
Comentário no card, mensagem no chat, um relatório periódico, um painel simples do próprio sistema.
Resposta:

**9. Um projeto ou vários?**
O sistema roda para um time só ou para vários projetos com pessoas e regras diferentes.
Resposta:

**10. Webhook ou consulta periódica para saber que algo mudou?**
Resposta:

## Roteiro revisado (o que vai guiar os diagramas)

Escrito com base na descrição atual. Será ajustado depois das respostas.

- Escopo: capturar definições e mudanças de requisito vindas de board, comentários, chat compartilhado, Figma e transcrições de reunião; transformar em proposta versionada; coletar aprovação de quem tem autoridade; aplicar; manter card apontando para a versão aprovada; expor pendências.
- Nível: containers (C4) e sequência de uma jornada crítica.
- Limites: adaptadores por ferramenta; observador que só detecta mudança; orquestrador que guarda estado e decide fluxo; agente redator, único que usa modelo de linguagem; gateway de modelos; registro de decisões; repositório de requisitos em Git.
- Integrações externas: board, Git, chat, Figma, transcrição de reunião, provedores de modelo via gateway.
- Restrições: nenhuma mudança aplicada sem aprovação registrada; aprovação no canal da pessoa; conteúdo das fontes é dado, não instrução; só conversas compartilhadas; dados sensíveis não entram em requisito nem em log; toda ação irreversível registrada; pendência tem prazo e escalonamento.
- Lacunas: as que ficarem sem resposta acima.
