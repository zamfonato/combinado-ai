# Diagrama de containers: suposições e checklist

Gerado com apoio de IA a partir de `descricao-sistema.md` e `esclarecimento.md`. Fonte do diagrama: `containers.mmd`.

## Suposições adotadas antes do código

1. O Figma não passa pelos adaptadores. Ele é acessado pelo agente redator via MCP, através do gateway, porque quem lê a tela é o modelo, não um serviço comum. Comentários do Figma que viram proposta entram por esse mesmo caminho.
2. As transcrições de reunião entram pelo chat, em canal compartilhado, e são triadas pelo agente redator entre os cards abertos do projeto. Não existe integração direta com ferramenta de reunião nem pasta de arquivos.
3. A configuração por projeto (tabela de autoridade, prazos, ferramentas) vive em arquivos versionados, não em banco. Assim a mudança de regra também tem histórico.
4. O registro de decisões é um banco relacional separado do Git. O Git guarda o conteúdo do requisito; o banco guarda o ato de aprovar, as pendências e os prazos.
5. O orquestrador é quem controla prazos e escalonamento, porque ele já guarda o estado de cada requisito. Não existe um serviço separado de "cobrança".
6. O gateway de modelos concentra tanto as chamadas a modelo quanto as chamadas a MCP. É o único ponto com credencial externa de IA.
7. Não há interface própria. Toda saída para pessoas passa por board, chat ou Git.
8. O requisito vive na pasta `requisitos/` do repositório de código do projeto, não em repositório separado, para ficar no contexto que o agente do dev já lê. Repositório separado só quando a permissão exigir (decisão a registrar em ADR).

## Checklist de revisão

Revisado em 12/09/2026. Itens conferidos contra o diagrama; correções aplicadas no mesmo commit.

1. [x] O diagrama está no nível de containers, sem componentes internos nem detalhes de código.
2. [x] Todas as integrações externas estão marcadas como externas (board, Git, chat, Figma, provedores).
3. [x] Só o agente redator chega ao gateway, e só o gateway chega aos provedores e ao Figma.
4. [x] Nenhum container interno tem seta direta para provedor de modelo.
5. [x] O orquestrador não fala com modelo de linguagem.
6. [x] O observador só detecta, não interpreta, e não escreve em lugar nenhum.
7. [x] Toda escrita para fora (card, MR, mensagem) sai do orquestrador via adaptadores.
8. [x] O dev aparece consumindo requisito no Git e contestando pelo card, não em interface própria.
9. [x] A gerência aparece só como destino de escalonamento.
10. [x] Os três papéis de aprovação (PO, UX, stakeholder) estão representados como atores. Estão agrupados em um único ator para não poluir o desenho; a tabela de autoridade os distingue.
11. [x] Os nomes dos containers batem com os nomes usados em `descricao-sistema.md`. Correção: "Configuração por projeto" faltava na descrição e foi incluída; a descrição dos adaptadores ainda citava pasta de transcrições e foi corrigida.
12. [x] As suposições acima estão de acordo com o que foi respondido no esclarecimento.
