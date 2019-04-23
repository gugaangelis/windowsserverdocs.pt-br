---
title: Usar o Robocopy para pré-propagar arquivos para a replicação DFS
description: Como usar Robocopy.exe para pré-propagar arquivos para a replicação DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865077"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usar o Robocopy para pré-propagar arquivos para a replicação DFS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Este tópico explica como usar a ferramenta de linha de comando **Robocopy.exe**para pré-propagar arquivos durante a configuração de replicação para replicação de sistema de arquivos distribuído (DFS) (também conhecido como DFSR ou DFS-R) no Windows Server. Por pré-propagar arquivos antes de configurar a replicação do DFS, adicionar um novo parceiro de replicação, ou substituir um servidor, você pode agilizar a sincronização inicial e habilitar a clonagem de banco de dados de replicação do DFS no Windows Server 2012 R2. O método de Robocopy é um dos vários métodos preseeding; Para obter uma visão geral, consulte [etapa 1: pré-propagar arquivos para a replicação DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

O utilitário de linha de comando Robocopy (cópia de arquivo robusto) está incluído com o Windows Server. O utilitário oferece amplas opções que incluem a cópia segurança, backup API suporte, recursos de repetição e registro em log. Em versões posteriores incluem o suporte de e/s de vários thread e sem buffer.

>[!IMPORTANT]
>Robocopy não copia exclusivamente os arquivos bloqueados. Se os usuários tendem a bloquear muitos arquivos por longos períodos em seus servidores de arquivos, considere usar um método preseeding diferente. Pré-propagar não requer uma correspondência perfeita entre as listas de arquivo nos servidores de origem e destino, mas é o número de arquivos que não existe quando a sincronização inicial é executada para a replicação DFS, o pré-propagar menos eficaz. Para minimizar conflitos de bloqueio, use o Robocopy durante o horário de pico para sua organização. Sempre examine os logs de Robocopy depois de pré-propagar para garantir que você entenda quais arquivos foram ignorados devido a bloqueios exclusivos.

Para usar o Robocopy para pré-propagar arquivos para a replicação DFS, siga estas etapas:

1. [Baixe e instale a última versão do Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Estabilize arquivos que serão replicados.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copie os arquivos replicados para o servidor de destino.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Pré-requisitos

Porque pré-propagar não envolve a replicação do DFS diretamente, você só precisará atender aos requisitos para executar uma cópia de arquivo com o Robocopy.

- Você precisa de uma conta que seja membro do grupo Administradores local nos servidores de origem e de destino.

- Instalar a versão mais recente do Robocopy no servidor que você usará para copiar os arquivos — o servidor de origem ou o servidor de destino. Você precisará instalar a versão mais recente para a versão do sistema operacional. Para obter instruções, consulte [etapa 2: Estabilizar arquivos que serão replicados](#step-2:-stabilize-files-that-will-be-replicated). A menos que você está pré-propagar arquivos de um servidor executando o Windows Server 2003 R2, você pode executar o Robocopy no servidor de origem ou destino. O servidor de destino, que geralmente tem a versão mais recente do sistema operacional, fornece acesso para a versão mais recente do Robocopy.

- Certifique-se de que o espaço de armazenamento suficiente está disponível na unidade de destino. Não crie uma pasta no caminho que você pretende copiar para: Robocopy deve criar a pasta raiz.
    
    >[!NOTE]
    >Quando você decidir quanto espaço alocar para os arquivos preseeded, considere o crescimento esperado dos dados ao longo do tempo e o armazenamento de requisitos para replicação DFS. Para obter ajuda de planejamento, consulte [editar o tamanho da cota da pasta de preparo e conflito e pasta excluída](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) na [gerenciamento de replicação do DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- No servidor de origem, opcionalmente, instale o Process Monitor ou o Process Explorer, que você pode usar para verificar para aplicativos que estão bloqueando arquivos. Para baixar informações, consulte [Monitor de processos](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Etapa 1: Baixe e instale a última versão do Robocopy

Antes de usar o Robocopy para pré-propagar arquivos, você deve baixar e instalar a versão mais recente do **Robocopy.exe**. Isso garante que a replicação do DFS não ignorar arquivos devido a problemas em versões de envio do Robocopy.

A fonte para a versão mais recente compatível do Robocopy depende da versão do Windows Server que está em execução no servidor. Para obter informações sobre como baixar o hotfix com a versão mais recente do Robocopy para Windows Server 2008 R2 ou Windows Server 2008, consulte [lista de hotfixes atualmente disponíveis para as tecnologias de sistema de arquivos distribuído (DFS) no Windows Server 2008 e no Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, você pode localizar e instalar o hotfix mais recente para um sistema operacional por meio das etapas a seguir.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Localizar e instalar a última versão do Robocopy para uma versão específica do Windows Server

1. Em um navegador da web, abra [ https://support.microsoft.com ](https://support.microsoft.com/).

2. Na **dá suporte à pesquisa**, digite a seguinte cadeia de caracteres, substituindo `<operating system version>` com o sistema operacional apropriado, em seguida, pressione a tecla Enter:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por exemplo, digite **robocopy.exe kbqfe "Windows Server 2008 R2"**.

3. Localizar e baixar o hotfix com o maior número de ID (ou seja, a versão mais recente).

4. Instale o hotfix no servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Etapa 2: Estabilizar arquivos que serão replicados

Depois de instalar a última versão do Robocopy no servidor, você deve impedir que arquivos bloqueados copiando bloqueio usando os métodos descritos na tabela a seguir. A maioria dos aplicativos não bloqueiam arquivos exclusivamente. No entanto, durante as operações normais, uma pequena porcentagem de arquivos pode ter sido bloqueada em servidores de arquivos.

|Origem do bloqueio|Explicação|Atenuação|
|---|---|---|
|Remotamente, os usuários abrem arquivos em compartilhamentos.|Os funcionários se conectar a um servidor de arquivos padrão e editar documentos, conteúdo multimídia ou outros arquivos. Às vezes, conhecido como a pasta base tradicional ou cargas de trabalho de dados compartilhada.|Executar somente operações de Robocopy durante o pico, fora do horário comercial. Isso minimiza o número de arquivos que Robocopy deve ignorar durante pré-propagar.<br><br>Considere definir temporariamente o acesso somente leitura em compartilhamentos de arquivos que serão replicados usando o Windows PowerShell **SmbShareAccess Grant** e **SmbSession fechar** cmdlets. Se você definir permissões para um grupo comum, como todos ou usuários autenticados para leitura, os usuários padrão podem ser menos prováveis abrir arquivos com bloqueios exclusivos (se seus aplicativos detectam o acesso somente leitura quando arquivos são abertos).<br><br>Você também pode definir uma regra de firewall temporário para a porta SMB 445 de entrada desse servidor para bloquear o acesso a arquivos ou use o **SmbShareAccess bloco** cmdlet. No entanto, esses dois métodos são muito perturbador para operações de usuário.|
|Aplicativos abrem arquivos locais.|Cargas de trabalho do aplicativo em execução em um servidor de arquivos, às vezes, bloquear arquivos.|Desabilitar temporariamente ou desinstalar os aplicativos que estão bloqueando arquivos. Você pode usar o Monitor de processos ou o Process Explorer para determinar quais aplicativos estão bloqueando arquivos. Para baixar o Process Monitor ou o Process Explorer, visite o [Monitor de processos](https://docs.microsoft.com/sysinternals/downloads/procmon) e [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) páginas.|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Etapa 3: Copie os arquivos replicados para o servidor de destino

Depois que você minimizar bloqueios nos arquivos que serão replicados, você pode pré-propagar os arquivos do servidor de origem para o servidor de destino.

>[!NOTE]
>Você pode executar o Robocopy no computador de origem ou o computador de destino. O procedimento a seguir descreve a execução de Robocopy no servidor de destino, que normalmente está executando um sistema operacional mais recente, para tirar proveito de quaisquer recursos adicionais do Robocopy que o sistema operacional mais recente pode fornecer.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Pré-propagar arquivos replicados no servidor de destino com o Robocopy

1. Entrar no servidor de destino com uma conta que seja membro do grupo Administradores local nos servidores de origem e de destino.

2. Abra um prompt de comando com privilégios elevados.

3. Para pré-propagar os arquivos de origem para o servidor de destino, execute o seguinte comando, substituindo sua própria origem, destino e os caminhos de arquivo de log para obter os valores entre colchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Este comando copia todo o conteúdo da pasta de origem para a pasta de destino, com os seguintes parâmetros:
    
    |Parâmetro|Descrição|
    |---|---|
    |"\<origem replicados caminho da pasta\>"|Especifica a pasta de origem para pré-propagar no servidor de destino.|
    |"\<destino replicada caminho da pasta\>"|Especifica o caminho para a pasta que armazenará os arquivos preseeded.<br><br>Não, a pasta de destino já deve existir no servidor de destino. Para obter os hashes de arquivo correspondente, o Robocopy deve criar a pasta raiz quando ele preseeds os arquivos.|
    |/e|Copia os subdiretórios e seus arquivos, bem como os subdiretórios vazios.|
    |/b|Copia os arquivos no modo de Backup.|
    |/copyal|Copia todas as informações de arquivo, incluindo dados, atributos, carimbos de data / hora, a lista de controle de acesso (ACL) do NTFS, informações sobre o proprietário e informações de auditoria.|
    |/r:6|Repete a operação de seis vezes quando ocorre um erro.|
    |/w:5|Espera cinco segundos entre repetições.|
    |MT:64|Copia os arquivos de 64 simultaneamente.|
    |/xd DfsrPrivate|Exclui a pasta DfsrPrivate.|
    |/tee|Grava a saída de status para a janela de console, bem como para o arquivo de log.|
    |/ log \<caminho do arquivo de log >|Especifica o arquivo de log para gravar. Substitui o conteúdo do arquivo existente. (Para acrescentar as entradas para o arquivo de log existente, use `/log+ <log file path>`.)|
    |/v|Produz saída detalhada que inclui arquivos ignorados.|
    
    Por exemplo, o comando a seguir replica os arquivos da pasta de origem são replicadas, e:\\RF01, para a unidade de dados D no servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >É recomendável que você use os parâmetros descritos acima, quando você usa o Robocopy para pré-propagar arquivos para a replicação DFS. No entanto, você pode alterar alguns dos seus valores ou adicionar parâmetros adicionais. Por exemplo, você pode descobrir por meio de testes que você tem a capacidade de definir um valor mais alto (contagem de threads) para o */MT* parâmetro. Além disso, se você replicará principalmente arquivos maiores, você poderá aumentar o desempenho de cópia, adicionando a **/j** opção de e/s não armazenada em buffer. Para obter mais informações sobre os parâmetros do Robocopy, consulte o [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) referência de linha de comando.

    >[!WARNING]
    >Para evitar a perda de dados ao usar o Robocopy para pré-propagar arquivos para a replicação DFS, não faça as seguintes alterações para os parâmetros recomendados:
    >- Não use o */mir* parâmetro (o que reflete uma árvore de diretório) ou o */mov* parâmetro (que move os arquivos e, em seguida, exclui-los da fonte).
    >-  Não remova os **/e**, **/b**, e **/copyall** opções.

4. Após a cópia é concluída, examine o log de erros ou arquivos ignorados. Use o Robocopy para copiar todos os arquivos individualmente, em vez de copiando novamente todo o conjunto de arquivos ignorados. Se os arquivos foram ignorados devido a bloqueios exclusivos, tente copiar arquivos individuais com Robocopy mais tarde ou aceite o que esses arquivos exigirá a replicação de over-the-wire pela replicação DFS durante a sincronização inicial.

## <a name="next-step"></a>Próximas etapas

Depois de concluir a cópia inicial e usar o Robocopy para resolver problemas com arquivos ignorados tantos quanto possível, você usará o **Get-DfsrFileHash** cmdlet do Windows PowerShell ou o **Dfsrdiag** comando Valide os arquivos preseeded, comparando os hashes de arquivo nos servidores de origem e destino. Para obter instruções detalhadas, consulte [etapa 2: Validar arquivos Preseeded para a replicação DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).