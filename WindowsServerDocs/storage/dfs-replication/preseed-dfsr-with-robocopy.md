---
title: Usar o Robocopy para preseed arquivos para replicação DFS
description: Como usar Robocopy.exe para preseed arquivos para replicação DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081807"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usar o Robocopy para preseed arquivos para replicação DFS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Este tópico explica como usar a ferramenta de linha de comando, **Robocopy.exe**, para preseed arquivos ao configurar a replicação para replicação do sistema de arquivos distribuídos (DFS) (também conhecido como DFSR ou DFS-R) no Windows Server. Por preseeding arquivos antes de configurar a replicação DFS, adicionar um novo parceiro de replicação, ou substituir um servidor, você pode acelerar a sincronização inicial e habilitar a clonagem do banco de dados de replicação DFS no Windows Server 2012 R2. O método Robocopy é um dos vários métodos preseeding; Para obter uma visão geral, consulte [etapa 1: preseed arquivos para replicação DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

O utilitário de linha de comando Robocopy (cópia de arquivo robusto) está incluído no Windows Server. O utilitário fornece extensiva opções que incluem a cópia segurança, backup API suporte, recursos de repetição e registro em log. Versões posteriores incluem o suporte de e/s multithreading e sem buffer.

>[!IMPORTANT]
>O Robocopy não copia exclusivamente arquivos bloqueados. Se os usuários tendem a bloquear muitos arquivos por longos períodos em seus servidores de arquivo, considere o uso de um método preseeding diferente. Preseeding não requer uma correspondência perfeita entre as listas de arquivos nos servidores de origem e destino, mas é o número de arquivos que não existe quando a sincronização inicial é executada para replicação do DFS, o preseeding menos eficaz. Para minimizar os conflitos de bloqueio, use o Robocopy durante o horário não sejam de pico para sua organização. Sempre examine os logs de Robocopy após preseeding para garantir que você entenda quais arquivos foram ignorados por causa de bloqueios exclusivos.

Para usar o Robocopy para preseed arquivos para replicação DFS, siga estas etapas:

1. [Baixe e instale a versão mais recente do Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Estabilização arquivos que serão replicados.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copie os arquivos replicados para o servidor de destino.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Pré-requisitos

Porque preseeding não envolve diretamente a replicação DFS, você precisa atender aos requisitos para a realização de uma cópia de arquivo com o Robocopy.

- Você precisa de uma conta que seja membro do grupo Administradores local em servidores de origem e destino.

- Instalar a versão mais recente do Robocopy no servidor que você usará para copiar os arquivos — o servidor de origem ou o servidor de destino; Você precisará instalar a versão mais recente para a versão do sistema operacional. Para obter instruções, consulte [etapa 2: estabilização arquivos que serão replicados](#step-2:-stabilize-files-that-will-be-replicated). A menos que você está preseeding arquivos a partir de um servidor executando o Windows Server 2003 R2, você pode executar o Robocopy no servidor de origem ou de destino. O servidor de destino, que geralmente tem a versão mais recente do sistema operacional, dá acesso à versão mais recente do Robocopy.

- Certifique-se de que o espaço de armazenamento suficiente está disponível na unidade de destino. Não crie uma pasta no caminho que você planeja copiar: Robocopy deve criar a pasta raiz.
    
    >[!NOTE]
    >Quando você decide quanto espaço alocar para os arquivos preseeded, considere o crescimento esperado de dados sobre os requisitos de armazenamento e tempo para replicação DFS. Para o planejamento de Ajuda, consulte [Editar o tamanho da cota da pasta de preparo e conflito e pasta excluídos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) da replicação do [Gerenciamento DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- No servidor de origem, se desejar instale Process Monitor ou Explorer de processo, o que você pode usar para verificar para aplicativos que o bloqueio de arquivos. Para obter informações de download, consulte [Monitor de processo](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Etapa 1: Baixar e instalar a versão mais recente do Robocopy

Antes de usar o Robocopy para preseed arquivos, você deve baixar e instalar a versão mais recente do **Robocopy.exe**. Isso garante que a replicação DFS não ignorar arquivos devido a problemas em versões de envio do Robocopy.

A fonte para a última versão compatível do Robocopy depende da versão do Windows Server que está sendo executado no servidor. Para obter informações sobre como baixar o hotfix com a versão mais recente do Robocopy para Windows Server 2008 R2 ou Windows Server 2008, consulte [lista de hotfixes atualmente disponíveis para as tecnologias no Windows Server 2008 e no sistema de arquivos distribuídos (DFS) Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, você pode localizar e instalar o hotfix mais recente para um sistema operacional de acordo com as seguintes etapas.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Localizar e instalar a versão mais recente do Robocopy para uma versão específica do Windows Server

1. Em um navegador da web, abra [https://support.microsoft.com](https://support.microsoft.com/).

2. Em **Suporte de pesquisa**, digite o seguinte cadeia de caracteres, substituindo `<operating system version>` com o sistema operacional apropriado, pressione a tecla Enter:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por exemplo, insira **robocopy.exe kbqfe "Windows Server 2008 R2"**.

3. Localize e baixe o hotfix com o maior número de identificação (ou seja, a versão mais recente).

4. Instale o hotfix no servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Etapa 2: Estabilização arquivos que serão replicados

Após instalar a versão mais recente do Robocopy no servidor, você deve impedir que arquivos bloqueados copiem bloqueio usando os métodos descritos na tabela a seguir. A maioria dos aplicativos não bloqueiam arquivos exclusivamente. No entanto, durante operações normais, uma pequena porcentagem dos arquivos pode ser bloqueada em servidores de arquivos.

|Origem do bloqueio|Explicação|Mitigação|
|---|---|---|
|Os usuários remotamente abrir arquivos em compartilhamentos.|Funcionários conectem a um servidor de arquivo padrão e editar documentos, conteúdo multimídia ou outros arquivos. Em alguns casos, conhecido como a pasta base tradicional ou cargas de trabalho de dados compartilhados.|Realizar operações de Robocopy durante apenas pico, fora do horário comercial. Isso minimiza o número de arquivos que o Robocopy deve ignorar durante preseeding.<br><br>Considere definir temporariamente o acesso somente leitura nos compartilhamentos de arquivos que serão replicados usando os cmdlets do Windows PowerShell **Grant-SmbShareAccess** e **SmbSession de fechar** . Se você definir permissões para um grupo comuns, como todos ou usuários autenticados ler, os usuários padrão podem ser menos suscetíveis a abrir arquivos com bloqueios exclusivos (se seus aplicativos detectam o acesso somente leitura quando os arquivos são abertos).<br><br>Você também pode considerar a definição de uma regra de firewall temporário para porta SMB 445 entrada a esse servidor para bloquear o acesso aos arquivos ou use o cmdlet **SmbShareAccess de bloco** . No entanto, esses dois métodos são muito destrutivos para operações de usuário.|
|Aplicativos de abrem arquivos locais.|Cargas de trabalho de aplicativos em execução em um servidor de arquivos em alguns casos, bloquear arquivos.|Temporariamente desativar ou desinstalar os aplicativos que o bloqueio de arquivos. Você pode usar o Monitor de processo ou Process Explorer para determinar quais aplicativos estão bloqueio de arquivos. Para baixar o Monitor de processo ou Process Explorer, visite as páginas [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Etapa 3: Copiar arquivos replicados para o servidor de destino

Depois que você minimizar bloqueios nos arquivos que serão replicados, você pode preseed os arquivos do servidor de origem para o servidor de destino.

>[!NOTE]
>Você pode executar o Robocopy no computador de origem ou no computador de destino. O procedimento a seguir descreve o Robocopy em execução no servidor de destino, que geralmente está executando um sistema operacional mais recente, para se beneficiar dos quaisquer recursos adicionais de Robocopy que o sistema operacional mais recente possível fornecer.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed arquivos duplicados logon no servidor de destino com o Robocopy

1. Inscreva-se para o servidor de destino com uma conta que seja membro do grupo local Administradores nos servidores de origem e destino.

2. Abra um prompt de comando com privilégios elevados.

3. Para preseed os arquivos da origem para o servidor de destino, execute o seguinte comando, substituindo o seu próprio código-fonte, no destino e caminhos de arquivo de log para os valores entre colchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Este comando copia todo o conteúdo da pasta de origem para a pasta de destino, com os seguintes parâmetros:
    
    |Parâmetro|Descrição|
    |---|---|
    |"\ < pasta replicada caminho de origem >"|Especifica a pasta de origem para preseed no servidor de destino.|
    |"\ < destino replicado caminho da pasta >"|Especifica o caminho para a pasta que irá armazenar os arquivos preseeded.<br><br>A pasta de destino não deve existir no servidor de destino. Para obter os hashes de arquivo correspondentes, o Robocopy deve criar a pasta raiz quando ele preseeds os arquivos.|
    |. exe /e|Copia os subdiretórios e seus arquivos, bem como os subdiretórios vazios.|
    |/b|Copia os arquivos no modo de Backup.|
    |/ copyal|Copia todas as informações do arquivo, incluindo dados, atributos, carimbos de data / hora, a lista de controle de acesso (ACL) do NTFS, informações sobre o proprietário e informações de auditoria.|
    |/r:6|Repete a operação seis vezes quando ocorre um erro.|
    |/w:5|5 segundos entre as tentativas de espera.|
    |MT:64|Copia 64 arquivos simultaneamente.|
    |/XD DfsrPrivate|Exclui a pasta DfsrPrivate.|
    |/TEE|Grava a saída de status na janela do console, bem como o arquivo de log.|
    |/log \ < caminho do arquivo de log >|Especifica o arquivo de log para gravar. Substitui o conteúdo do arquivo existente. (Para acrescentar as entradas do arquivo de log existente, use `/log+ <log file path>`.)|
    |/v|Produz a saída detalhada que inclui os arquivos ignorados.|
    
    Por exemplo, o comando a seguir replica arquivos da pasta de origem replicada, E:\\RF01, D de unidade de dados no servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Recomendamos que você use os parâmetros descritos acima, quando você usa o Robocopy para preseed arquivos para replicação DFS. No entanto, você pode alterar alguns dos seus valores ou adicionar outros parâmetros. Por exemplo, você pode descobrir por meio de testes que você tenha a capacidade para definir um valor mais alto (contagem de threads) para o parâmetro */MT* . Além disso, se você vai principalmente replicar arquivos maiores, você poderá aumentar o desempenho de cópia, adicionando a opção **/j** para e/s sem buffer. Para obter mais informações sobre o Robocopy parâmetros, consulte a referência de linha de comando [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Para evitar a perda de dados quando você usa o Robocopy para preseed arquivos para replicação DFS, não faça as seguintes alterações para os parâmetros recomendados:
    >- Não use o parâmetro */mir* (que espelha a uma árvore de diretório) ou o parâmetro */mov* (que move os arquivos e, em seguida, exclui-los da origem).
    >-  Não remova as opções **. exe /e**, **/b**e **/copyall** .

4. Após a conclusão da cópia, examine o log por quaisquer erros ou arquivos ignorados. Use o Robocopy para copiar quaisquer arquivos ignorados individualmente, em vez de copiar novamente todo o conjunto de arquivos. Se os arquivos foram ignorados por causa de bloqueios exclusivos, tente copiar os arquivos individuais com o Robocopy posteriormente ou aceitar o que esses arquivos exigirá a replicação de over-durante a transmissão pela replicação DFS durante a sincronização inicial.

## <a name="next-step"></a>Próxima etapa

Depois de concluir a cópia inicial e utilize o Robocopy para resolver problemas com arquivos ignorados quantos possível, você usará o cmdlet **Get-DfsrFileHash** no Windows PowerShell ou o comando **Dfsrdiag** para validar os arquivos preseeded comparando hashes de arquivo nos servidores de origem e destino. Para obter instruções detalhadas, consulte [etapa 2: Validar arquivos Preseeded para replicação DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).