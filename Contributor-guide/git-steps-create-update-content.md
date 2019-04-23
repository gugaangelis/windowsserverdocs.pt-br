<properties pageTitle="Comandos do Git para criar um novo artigo ou atualizar um artigo existente" description="Etapas para criar e atualizar um artigo em WindowsServerDocs-pr." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>Comandos do Git para criar ou atualizar um artigo

>! OBSERVAÇÃO: Esses comandos pressupõem que você configurou o Github para especificar o repositório padrão em que você extrair arquivos de. No Github é terminologia, onde você pode extrair arquivos do seu *upstream*. É onde você pode enviar arquivos para seus *origem*. Com base em como nosso repositório e o fluxo de trabalho são criados, seu upstream deve ser definido como nosso repositório, que está sob a organização da Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr e sua origem deve ser seu fork deste repositório, em sua conta do Github. Por exemplo, é de Kathy https://github.com/KBDAzure/WindowsServerDocs-pr 

>Para verificar suas configurações, digite ```git config -l```. Examine as URLs para certificar-se de que eles se referir ao local que você pretendia.

## <a name="add-or-update-an-article"></a>Adicionar ou atualizar um artigo

Aqui está como criar uma ramificação local, salve suas alterações e, em seguida, enviá-las para sua bifurcação remota.

1. Inicie o Git Bash (ou a ferramenta de linha de comando usada para Git).

2. Altere para WindowsServerDocs-pr:

        cd WindowsServerDocs-pr

3. É melhor manter o mestre local branch e o mestre remoto ramificar na sua bifurcação em sincronia com o mestre do repositório ramificação. Isso pode ajudar a economizar muita frustração e perda de tempo, você é mais provável capturar problemas antecipadamente e manter as coisas em um estado de funcionamento boa, conhecido. Executar:

        git checkout master
        git pull upstream master
        git push origin master

4. Agora você está pronto para criar uma ramificação para fazer o diário ou a do produto com base em funcionar. A melhor maneira de criar um branch de trabalho local de um branch de lançamento é usada para executar:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   Isso cria o branch local diretamente do branch upstream e ajuda a evitar a mesclar os arquivos incorretos em seu novo branch local. Por exemplo, para criar um branch de trabalho com base na ramificação de limite de ga, você pode executar um comando como este:
      
        git checkout upstream/master -b working-11-18

   Se você estiver trabalhando em conteúdo que deve ser mesclado na ramificação que não é         

5. Adicione o branch de trabalho local para seu fork:

        git push origin <working branch>

6. Crie seu novo artigo ou faça alterações em um artigo existente. Use o Windows Explorer para abrir arquivos de markdown e seu editor de markdown para criar e editar arquivos. Para obter ajuda de formatação básica, consulte este [artigo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) no Github.

7. Adicionar os metadados necessários e informações de versão, de acordo com a [controle de versão do produto e metadados](metadata-OSversioning-and-trademarks.md).

8. Verificar o status, adicionar e confirmar suas alterações:

        git status

   Examine os resultados para garantir que os arquivos listados são aqueles que você alterou. Se todos os arquivos parecerem precisos, execute este comando para adicionar todos os arquivos:

        git add .
        git commit –m "<comment>"

   Para adicionar somente os arquivos específicos (por exemplo, se ```git status``` lista os arquivos que você não deseja enviar), em vez disso, você deve executar:

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >O comando ```git add .``` adiciona todas as alterações pendentes relatadas pelo ```git status```. Isso significa que, se ```git status``` mostra as atualizações não controladas que você não deseja adicionar, use ```git add <file path>``` em vez disso.  

9. Atualize seu branch de trabalho local com as alterações por push do upstream:

        git pull upstream master

10. Envie por push as alterações à bifurcação no GitHub:

        git push origin <working branch>

## <a name="submit-your-changes"></a>Enviar suas alterações

Quando você estiver pronto para enviar seu conteúdo para preparação, validação e/ou a publicação, use a UI GitHub para criar uma solicitação de pull. 

Quando você abrir uma solicitação de pull (PR), isso dispara uma passagem de teste, compila o projeto e publica ao nosso site de preparo interno. É okey abrir uma solicitação de pull que você não está pronto para foram mesclados porque se trata de como você pode obter uma aprovação de teste e verificar as atualizações no site de preparo. Detalhes da compilação e links de preparo é lançado como comentários para a PR. 

É sua responsabilidade para fazer o seguinte **antes de pedir para que as alterações sejam mescladas**:
  - Revisão de compilação detalhes para garantir que ele não contenha erros. 
  - Examine suas atualizações no site de preparo.

Depois de fazer isso, indicá-lo por qualquer um:
- Adicionando o rótulo "ready-to-merge" para a PR. \(Clique em **rótulos** ou o ícone de engrenagem à direita do fluxo de comentários na PR.)
- Adicionando ready-to-merge como um comentário e enviar email para o alias do WSSC Pull revisores: wssc pra

O revisor de PR verifica as alterações e aceita a solicitação de pull, a menos que haja problemas ou dúvidas. Comentários ou solicitações para corrigir problemas são adicionadas como comentários. Examine [critérios de qualidade para pull solicitar revisão](contributor-guide-pr-criteria.md) para saber o que é esperado.

## <a name="publishing"></a>Publicação

- Artigos são publicados em aproximadamente às 10h e às 15:00 horário do Pacífico, de segunda a sexta-feira. Tenha em mente que um revisor de solicitação de pull precisa de tempo para examinar e aceitar suas alterações antes de poderem mescladas. As alterações devem ser mescladas para ser capturado no próximo ciclo de publicação agendado. Se você precisar de um artigo publicado de um ciclo de publicação específica, informe um revisor de solicitação de pull antecipadamente. Quando sua PR é aceito, suas alterações são mescladas para o repositório e serão incluídas na próxima publicação tempo executar. Ele pode levar até 30 minutos para artigos apareçam online após a publicação. 
