# Descrição do sistema

## O problema

Em um time de desenvolvimento, o trabalho do dev deixa rastro sozinho. Cada mudança vira um commit, cada entrega vira merge request a ser avaliado, cada erro aparece em teste ou em produção. 

O trabalho de quem define o que o sistema deve fazer muitas vezes não deixa esse rastro algum. 

O pedido nasce em reunião, muda em conversa, é anotado em um card e refinado em um documento. Essas fontes nem sempre dizem a mesma coisa e ninguém está efetivamente deixando tudo sincronizado e coerente.

O resultado é conhecido. O card diz uma coisa e a especificação diz outra. O que foi combinado em reunião não foi anotado porque era detalhe demais ou esqueceu. 

Uma decisão muda no meio do caminho e o dev descobre tarde demais ou precisa refazer o seu trabalho. 

O testador valida um documento que já não é verdade, está defasado. 

Quando alguém pergunta quem decidiu o quê e quando, a resposta depende da memória das pessoas. Quando existe essa preocupação.

Não é falta de método. Métodos como o Spec Kit ajudam a manter a especificação consistente por dentro. O que falta é um elo entre a especificação e o lugar onde as pessoas realmente combinam as coisas: o board, o chat o versionamento, o figma e a reunião.

## A proposta

O Combinado.ai é um colaborador de IA que trata o requisito como se fosse código. 

Ele acompanha o board, o repositório, o figma e as conversas que o time decide compartilhar com ele. 

Reunições precisam ser transcritas e entrar como insumo. Uma reunião sem uma transcrição é tempo jogado no lixo.

Quando alguém define ou muda um requisito, essa mudança precisa acontecer também no arquivo Markdown versionado. O sistema abre uma proposta de alteração e pede aprovação para quem tem autoridade sobre aquele tipo de decisão. 

A aprovação acontece no canal da pessoa, por exemplo um comentário no card. O sistema registra quem aprovou, quando e com base em quê, e só então aplica a mudança.

O PO e o stakeholder continuam trabalhando onde já trabalham. O Git fica atrás deles, não na frente. O dev passa a consumir requisitos com histórico verificável, versão e aprovação, do mesmo jeito que acontece no código.

## Escopo

Dentro do escopo:

- Capturar definições e mudanças de requisito vindas do board, de comentários e de atas de reunião transcrita.
- Transformar cada mudança em uma alteração versionada em um arquivo de requisito, com a fonte de cada afirmação.
- Pedir e registrar a aprovação de quem tem autoridade sobre a decisão.
- Manter o card apontando para a versão aprovada do requisito.
- Avisar o dev quando um requisito muda depois que a implementação começou.
- Tornar visível o que está pendente: aprovações sem resposta, decisões sem fonte, mais de uma fonte de verdade, requisitos alterados após o início do desenvolvimento.

Fora do escopo:

- Não é um board nem um gerenciador de tarefas. Ele se integra ao que a empresa já usa. "Jira", "Azure Board", etc
- Não gera código nem refina a solução técnica.
- Não decide conflitos. Quando duas fontes divergem, ele pergunta a quem tem autoridade e registra a resposta.
- Não substitui a reunião. Ele consome o que saiu textualmente dela e registra.

## Nível da visão

Diagrama de containers, no estilo do C4 Model, e um diagrama de sequência da jornada crítica. Não entra em componentes internos nem em código.

## Limites e responsabilidades

- Observador de fontes: fica de olho no board, no repositório de requisitos, no figma correspondente e nas conversas/textos compartilhadas. Detecta que algo mudou e entrega para o orquestrador. Não interpreta conteúdo.
- Orquestrador: guarda o estado de cada requisito (proposto, aguardando aprovação, aprovado, aplicado) e decide o próximo passo. Não fala com modelo de linguagem diretamente.
- Agente redator: recebe o conteúdo capturado e escreve a proposta de mudança no arquivo de requisito, citando a fonte. É o único que conversa com o modelo de linguagem.
- Gateway de modelos: ponto único por onde toda chamada a modelo de linguagem passa. Controla qual modelo é usado, quanto custa e quem pode chamar.
- Registro de decisões: guarda quem aprovou o quê, quando, por qual canal e com base em qual fonte. É o histórico que responde à pergunta "quem combinou isso".
- Adaptadores: traduzem o board, o Git e o chat da empresa para uma interface comum. Trocar de ferramenta significa trocar o adaptador, não o sistema.
- Repositório de requisitos: pasta `requisitos/` dentro do repositório de código do projeto, com um arquivo Markdown por card, versionado em Git. É a fonte de verdade do requisito e fica onde o agente do dev já lê. Quando a permissão exigir (por exemplo, cliente que aprova requisito mas não pode ver código), a configuração do projeto pode apontar para um repositório separado.

## Integrações externas

- Board de tarefas (Azure Boards, Jira, GitLab Issues ou similar): leitura de cards e comentários, escrita de comentários e de links.
- Repositório Git do projeto (GitLab, GitHub ou similar): leitura e escrita dos arquivos em `requisitos/`, abertura e aplicação de merge requests de requisito, identificados por prefixo ou label.
- Chat da empresa (Teams, Slack ou similar): leitura de mensagens em canais compartilhados dedicados a esta integração.
- Provedores de modelo de linguagem (via gateway): geração dos textos de proposta.
- Transcrição de reunião: recebida como arquivo ou texto anexado ao card. O sistema não grava reunião.

## Restrições

- Toda chamada a modelo de linguagem passa pelo gateway. Nenhum componente tem chave de provedor.
- Nenhuma mudança de requisito é aplicada sem aprovação registrada de uma pessoa com autoridade.
- A aprovação é dada no canal que a pessoa já usa. O sistema nunca exige que PO ou stakeholder abram o Git.
- O conteúdo de cards, comentários e atas é tratado como dado, não como instrução para o agente.
- O sistema só lê as conversas que o time compartilhou de forma explícita. Não tem acesso a mensagens privadas.
- Dados de clientes e informações sensíveis que apareçam nas fontes não podem ser copiados para os arquivos de requisito nem para logs.
- Toda ação irreversível (aplicar mudança, fechar proposta, alterar card) fica registrada com autor, data e fonte, rastreabilidade total.

## Lacunas

Pontos que ainda não foram decididos. Um agente que for implementar este sistema não deve decidir sozinho nenhum deles.

- Como o sistema sabe quem tem autoridade sobre cada tipo de decisão (regra de negócio, comportamento de tela, prioridade). Tabela fixa, campo no card ou definido por projeto.
- O que fazer quando a pessoa com autoridade não responde. Prazo, lembrete, escalonamento.
- O que fazer quando uma decisão foi dada verbalmente e ninguém confirma por escrito. Aplicar como não confirmada ou segurar.
- Formato do arquivo de requisito. Livre, ou um modelo fixo com seções.
- Como o sistema descobre mudanças no board: webhook ou consulta periódica.
- Como ligar um card ao arquivo de requisito correspondente. Identificador no título, campo no card ou convenção de nome.
- Quanto de contexto o agente redator recebe. Só a mudança, ou o arquivo inteiro e o histórico.
- Quais métricas serão expostas e para quem.
- Projeto com front e back em repositórios separados: em qual deles fica a pasta `requisitos/`.
