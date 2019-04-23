# <a name="rename-or-delete-an-article"></a>Renomear ou excluir um artigo

Antes de renomear ou excluir um artigo, você precisará seguir esse processo para evitar ou reduzir o número de links desfeitos. Os clientes odeiam links desfeitos e não são receio de soar desativado quando atingido um. Observe que renomear ou excluir um artigo é a última etapa neste processo, não o primeiro.


## <a name="step-1-manage-inbound-links"></a>Etapa 1: Gerenciar links de entrada

Determine se há quaisquer links de entrada não são da Microsoft ao seu conteúdo, como blogs, fóruns e outros conteúdos na web. Entre em contato com os proprietários de blog para alterar esses links e remover ou atualizar links em postagens de fóruns. Ferramentas de análise da Web podem ajudar a identificar links de entrada de tráfego intenso que talvez você precise gerenciar dessa maneira.

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>Etapa 2: Remover todos os links cruzados no artigo do repositório WindowsServerDocs pr

1. Atualize seu local branch – executar `git pull upstream <branch>`, ie para atualizar a partir do mestre, execute `git pull upstream master`

2.  Examinará a pasta WindowsServerDocs pr para encontrar artigos que têm um link para o artigo que você deseja renomear ou remover e, em seguida, atualize os artigos para remover os links ou substituí-los com links para outro artigo. Você pode usar uma pesquisa e substitua o utilitário para localizar as referências cruzadas, se você tiver um instalado. Se você não fizer isso, você pode usar o Windows PowerShell:

 a. Inicie o Windows PowerShell.

 b. No prompt do PowerShell, altere para a pasta de pr WindowsServerDocs:

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c. Execute um comando para listar todos os arquivos que contêm um link para o artigo para renomear ou excluir:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Para enviar a lista de nomes de arquivo para um arquivo de texto (nesse caso, denominado psoutput. txt), execute:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Adicionar e confirmar todas as suas alterações, enviá-las ao seu fork e abrir uma solicitação de pull. Para obter instruções, consulte [comandos do Git para criar ou atualizar um artigo](git-steps-create-update-content.md).

## <a name="step-3-update-fwlinks"></a>Etapa 3: FWLinks de atualização

Verifique a ferramenta de FWLink qualquer FWLinks que apontam para o artigo. Ponto de qualquer FWLinks no conteúdo de substituição; Se você não estiver no alias que possui o link, associá-la. Se os proprietários não atualizar o link, arquive um tíquete com MSCOM para que o link alterado. Mais informações - [wiki interno](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>Etapa 4: Remover links cruzados no artigo do sumário

Trabalhar com a pessoa que mantém o arquivo ToC.md. Esse arquivo preenche o sumário à esquerda na biblioteca técnica do. Se você não souber quem contatar, envie um email para wssc-pra@microsoft.com.

## <a name="step-5-add-redirects"></a>Etapa 5: Adicionar redirecionamentos
Se você está renomeando ou excluindo um arquivo, adicione um redirecionamento para que os links existentes não interrompam:

1. Deixe o arquivo antigo no local existente com o nome de arquivo existente.
2. Substitua o conteúdo do arquivo com esse item de metadados:
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<redirecionamento de URL ou arquivo > é a URL completa para um local diferente ou é o caminho + nome de arquivo para um tópico diferente no mesmo repositório OPS.

   Por exemplo, isso seria todo o arquivo:

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. Depois de criar uma solicitação de pull para o redirecionamento, clique nos links nos comentários de PR - se o redirecionamento funciona, você deve obter o tópico de destino.

## <a name="step-6-rename-or-delete-the-article"></a>Etapa 6: Renomear ou excluir o artigo

Se você não estiver usando um redirecionamento, siga esta etapa depois de concluir as etapas anteriores e todos os artigos afetados são publicados. Se você estiver usando um redirecionamento, renomear ou excluir o artigo seria desfazer isso e fazer com que um link desfeito. Para renomear um arquivo, basta renomeá-lo no sistema de arquivos, em seguida, adicionar, confirmar e enviar por push a alteração e, em seguida, abra uma solicitação de pull.
Para excluir um arquivo, primeiro você precisa saber que ele não funciona para excluir apenas um arquivo do seu sistema de arquivos, porque este é um sistema de controle do código-fonte e precisa saber que você pretende fazer isso. Caso contrário, seus arquivos excluídos provavelmente reaparecerá.
Há duas maneiras de fazer isso:

- Sistema de arquivos e o git: Exclua o arquivo do sistema de arquivos. Em seguida, da sua ferramenta git, execute um destes procedimentos  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- Apenas git:   Execute ```git rm foo.md```

    Obter mais informações [ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo) e [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>Etapa 7: Localizar e corrigir straggler de links desfeitos

Use a ferramenta de controle de qualidade de conteúdo para localizar links desfeitos que as etapas anteriores não capturar, em seguida, remova ou corrigir os links.

## <a name="step-8-remove-cached-pages-from-search-engines"></a>Etapa 8: Remover páginas armazenadas em cache dos mecanismos de pesquisa

Acesse estas páginas da web para remover páginas da web em cache dos mecanismos de pesquisa: [Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Links do guia dos colaboradores

- [Índice de artigos com diretrizes](./contributor-guide-index.md)

