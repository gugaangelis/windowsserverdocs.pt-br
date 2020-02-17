---
title: Usar o Robocopy para propagar previamente arquivos para Replicação do DFS
description: Como usar o Robocopy.exe para propagar previamente arquivos para Replicação do DFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea5cd954dde6d4fa8fcaa7874f75cb9588115ab1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402130"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usar o Robocopy para propagar previamente arquivos para Replicação do DFS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Este tópico explica como usar a ferramenta de linha de comando, **Robocopy.exe**, para propagar arquivos ao configurar a replicação para a Replicação DFS (Sistema de Arquivos Distribuído) (também conhecida como DFSR ou DFS-R) no Windows Server. Ao pré-propagar arquivos antes de configurar Replicação do DFS, adicionar um novo parceiro de replicação ou substituir um servidor, você pode acelerar a sincronização inicial e habilitar a clonagem do banco de dados Replicação do DFS no Windows Server 2012 R2. O método Robocopy é um dos vários métodos de pré-propagação. Para obter uma visão geral, confira a [Etapa 1: arquivos de pré-propagação para Replicação do DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

O utilitário de linha de comando Robocopy (Cópia de Arquivo Robusto) está incluído com o Windows Server. O utilitário fornece amplas opções que incluem a cópia de segurança, suporte à API de backup, recursos de repetição e registro em log. As versões posteriores incluem suporte a E/S com vários threads e sem buffer.

>[!IMPORTANT]
>O Robocopy não copia arquivos exclusivamente bloqueados. Se os usuários tendem a bloquear muitos arquivos por longos períodos em seus servidores de arquivos, considere usar um método de pré-propagação diferente. A pré-propagação não exige uma correspondência perfeita entre as listas de arquivos nos servidores de origem e de destino, porém, quanto mais houver arquivos que não existam quando a sincronização inicial é executada para Replicação do DFS, menos eficaz será a pré-propagação. Para minimizar conflitos de bloqueio, use o Robocopy fora do horário de pico para sua organização. Sempre examine os logs do Robocopy após a pré-propagação para garantir que você compreenda quais arquivos foram ignorados devido a bloqueios exclusivos.

Para usar o Robocopy para pré-propagar arquivos para Replicação do DFS, siga estas etapas:

1. [Baixar e instalar a versão mais recente do Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Estabilizar os arquivos que serão replicados.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copiar os arquivos replicados para o servidor de destino.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Pré-requisitos

Como a pré-propagação não envolve diretamente Replicação do DFS, você só precisa cumprir aos requisitos para executar uma cópia de arquivo com o Robocopy.

- Você precisa de uma conta que seja membro do grupo local de Administradores nos servidores de origem e de destino.

- Instale a versão mais recente do Robocopy no servidor que você usará para copiar os arquivos, ou seja, o servidor de origem ou o servidor de destino. Será necessário instalar a versão mais recente para a versão do sistema operacional. Confira as instruções na [Etapa 2: Estabilizar os arquivos que serão replicados](#step-2-stabilize-files-that-will-be-replicated). A menos que você esteja pré-propagando arquivos de um servidor que executa o Windows Server 2003 R2, poderá executar o Robocopy no servidor de origem ou de destino. O servidor de destino, que normalmente tem a versão mais recente do sistema operacional, dá acesso à versão mais recente do Robocopy.

- Verifique se há espaço de armazenamento suficiente disponível na unidade de destino. Não crie uma pasta no caminho para o qual você planeja copiar: O Robocopy deve criar a pasta raiz.
    
    >[!NOTE]
    >Quando você decide quanto espaço deve ser alocado aos arquivos propagados, considere o crescimento de dados esperado com o tempo e os requisitos de armazenamento para a Replicação do DFS. Para obter ajuda sobre o planejamento, confira [Editar o Tamanho da cota da pasta de preparo e da pasta de conflito e excluída](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) em [Gerenciar Replicação do DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- No servidor de origem, opcionalmente, instale o Monitor do Processo ou o Explorador de Processos, que você pode usar para verificar se há aplicativos que estão bloqueando arquivos. Para obter informações de download, confira [Monitor do Processo](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Explorador de Processos](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Etapa 1: Baixar e instalar a versão mais recente do Robocopy

Antes de usar o Robocopy para pré-propagar arquivos, você deve baixar e instalar a versão mais recente do **Robocopy.exe**. Isso garante que a Replicação do DFS não ignore arquivos devido a problemas nas versões de envio do Robocopy.

A origem para a versão mais recente compatível do Robocopy depende da versão do Windows Server que está sendo executada no servidor. Para obter informações sobre como baixar o hotfix com a versão mais recente do Robocopy para Windows Server 2008 R2 ou Windows Server 2008, confira [Lista de hotfixes disponíveis atualmente para tecnologias de DFS (Sistema de Arquivos Distribuído) no Windows Server 2008 e no Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, você pode localizar e instalar o hotfix mais recente para um sistema operacional executando as etapas a seguir.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Localize e instale a versão mais recente do Robocopy para uma versão específica do Windows Server

1. Em um navegador da Web, abra [https://support.microsoft.com](https://support.microsoft.com/).

2. No **Suporte de Pesquisa**, insira a seguinte cadeia de caracteres, substituindo `<operating system version>` pelo sistema operacional apropriado e, em seguida, pressione a tecla Enter:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por exemplo, insira **robocopy.exe kbqfe "Windows Server 2008 R2"** .

3. Localize e baixe o hotfix com o número de ID mais alto (ou seja, a versão mais recente).

4. Instale o hotfix no servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Etapa 2: Estabilizar os arquivos que serão replicados

Depois de instalar a versão mais recente do Robocopy no servidor, você deve impedir que arquivos bloqueados bloqueiem a cópia usando os métodos descritos na tabela a seguir. A maioria dos aplicativos não bloqueia exclusivamente arquivos. No entanto, durante operações normais, um pequeno percentual de arquivos pode ser bloqueada em servidores de arquivos.

|Origem do bloqueio|Explicação|Atenuação|
|---|---|---|
|Os usuários abrem arquivos remotamente em compartilhamentos.|Os funcionários conectam-se a um servidor de arquivos padrão e editam documentos, conteúdo multimídia ou outros arquivos. Às vezes conhecido como a pasta base tradicional ou cargas de trabalho de dados compartilhados.|Execute apenas operações de Robocopy fora do horário de pico e do horário comercial. Isso minimiza o número de arquivos que o Robocopy deve ignorar durante a pré-propagação.<br><br>Considere a configuração temporária de acesso somente leitura nos compartilhamentos de arquivos que serão replicados usando o Windows PowerShell dos cmdlets **Grant-SmbShareAccess** e **Close-SmbSession**. Se você definir permissões para um grupo comum, como todos ou usuários autenticados para leitura, talvez os usuários padrão tenham menos probabilidade de abrir arquivos com bloqueios exclusivos (se os aplicativos detectarem o acesso somente leitura quando os arquivos forem abertos).<br><br>Você também pode considerar a configuração de uma regra de firewall temporária para a porta SMB 445 de entrada para o servidor bloquear o acesso a arquivos ou usar o cmdlet **Block-SmbShareAccess**. No entanto, esses dois métodos perturbam muito as operações do usuário.|
|Aplicativos abrem arquivos locais.|As cargas de trabalho de aplicativo em execução em um servidor de arquivos às vezes bloqueiam arquivos.|Desabilite ou desinstale temporariamente os aplicativos que estão bloqueando arquivos. Você pode usar o Monitor do Processo ou o Explorador de Processos para determinar quais aplicativos estão bloqueando arquivos. Para baixar o Monitor do Processo ou o Explorador de Processos, acesse as páginas [Monitor do Processo](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Explorador de Processos](https://docs.microsoft.com/sysinternals/downloads/process-explorer).|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Etapa 3: Copiar os arquivos replicados para o servidor de destino

Depois de minimizar os bloqueios nos arquivos que serão replicados, você pode propagar os arquivos do servidor de origem para o servidor de destino.

>[!NOTE]
>Você pode executar o Robocopy no computador de origem ou no computador de destino. O procedimento a seguir descreve a execução do Robocopy no servidor de destino, que normalmente está executando um sistema operacional mais recente, para aproveitar os recursos adicionais do Robocopy que o sistema operacional mais recente possa fornecer.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Propagar os arquivos replicados para o servidor de destino com Robocopy

1. Entre no servidor de destino com uma conta que seja membro do grupo local de Administradores nos servidores de origem e de destino.

2. Abra um prompt de comando com privilégios elevados.

3. Para propagar os arquivos do servidor de origem para o de destino, execute o seguinte comando, substituindo seus caminhos de origem, destino e arquivo de log pelos valores entre colchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Esse comando copia todo o conteúdo da pasta de origem para a pasta de destino com os seguintes parâmetros:
    
    |Parâmetro|Descrição|
    |---|---|
    |"\<caminho da pasta replicada de origem\>"|Especifica a pasta de origem para a pré-propagação no servidor de destino.|
    |"\<caminho da pasta replicada de destino\>"|Especifica o caminho para a pasta que armazenará os arquivos pré-propagados.<br><br>A pasta de destino ainda não deve existir no servidor de destino. Para obter os hashes de arquivo correspondentes, o Robocopy deve criar a pasta raiz ao propagar os arquivos.|
    |/e|Copia subdiretórios e seus arquivos, bem como subdiretórios vazios.|
    |/b|Copia arquivos no modo de Backup.|
    |/copyall|Copia todas as informações do arquivo, incluindo dados, atributos, carimbos de data/hora, a ACL (lista de controle de acesso) do NTFS, informações de proprietário e informações de auditoria.|
    |/r:6|Repete a operação seis vezes quando ocorre um erro.|
    |/w:5|Aguarda cinco segundos entre as repetições.|
    |MT:64|Copia 64 arquivos simultaneamente.|
    |/xd DfsrPrivate|Exclui a pasta DfsrPrivate.|
    |/tee|Grava a saída de status na janela do console, bem como no arquivo de log.|
    |/log \<caminho do arquivo de log>|Especifica o arquivo de log a ser gravado. Substitui o conteúdo existente do arquivo. (Para acrescentar as entradas ao arquivo de log existente, use `/log+ <log file path>`.)|
    |/v|Produz uma saída detalhada que inclui arquivos ignorados.|
    
    Por exemplo, o seguinte comando replica arquivos da pasta replicada de origem, E:\\RF01, para a unidade de dados D no servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Recomendamos usar os parâmetros descritos acima ao usar o Robocopy para propagar arquivos para Replicação do DFS. No entanto, você pode alterar alguns de seus valores ou adicionar outros parâmetros. Por exemplo, você pode descobrir como testar se tem a capacidade de definir um valor mais alto (contagem de thread) para o parâmetro */MT*. Além disso, se você replicar principalmente arquivos maiores, poderá conseguir aumentar o desempenho da cópia adicionando a opção **/j** para E/S não armazenada em buffer. Para obter mais informações sobre o Robocopy, confira a página de referência de linha de comando [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy).

    >[!WARNING]
    >Para evitar a possível perda de dados ao usar o Robocopy para pré-propagar os arquivos para Replicação do DFS, não faça as seguintes alterações aos parâmetros recomendados:
    >- Não use o parâmetro */mir* (que espelha uma árvore de diretórios) nem o parâmetro */mov* (que move os arquivos e, em seguida, os exclui da origem).
    >-  Não remova as opções **/e**, **/b** e **/copyall**.

4. Após a conclusão da cópia, examine se há erros ou arquivos ignorados no log. Use o Robocopy para copiar todos os arquivos ignorados individualmente, em vez de copiar todo o conjunto de arquivos. Se os arquivos foram ignorados devido a bloqueios exclusivos, tente copiar arquivos individuais com o Robocopy mais tarde ou aceite que esses arquivos exigirão replicação por transmissão pela Replicação do DFS durante a sincronização inicial.

## <a name="next-step"></a>Próxima etapa

Depois de concluir a cópia inicial e usar o Robocopy para resolver problemas com o máximo possível de arquivos ignorados, você usará o cmdlet **Get-DfsrFileHash** no Windows PowerShell ou o comando **Dfsrdiag** para validar os arquivos pré-propagados comparando os hashes de arquivo nos servidores de origem e de destino. Para obter instruções detalhadas, confira a [Etapa 2: Validar arquivos de pré-propagação para Replicação do DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).
