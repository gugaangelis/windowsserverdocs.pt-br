---
title: Ajuste de desempenho para servidores de arquivos NFS
description: Ajuste de desempenho para servidores de arquivos NFS
ms.topic: article
author: phstee
ms.author: roopeshb, nedpyle
ms.date: 10/16/2017
ms.openlocfilehash: 897e45a99ac4640c5fbae4287ac99a0bce6eae66
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896181"
---
# <a name="performance-tuning-nfs-file-servers"></a>Servidores de arquivos NFS de ajuste de desempenho

## <a name="services-for-nfs-model"></a><a href="" id="servicesnfs"></a>Modelo de serviços para NFS


As seções a seguir fornecem informações sobre o modelo de serviços Microsoft para NFS (sistema de arquivos de rede) para comunicação cliente-servidor. Como o NFS v2 e o NFS v3 ainda são as versões mais implantadas do protocolo, todas as chaves do registro, exceto MaxConcurrentConnectionsPerIp, se aplicam somente ao NFS v2 e ao NFS v3.

Nenhum ajuste de registro é necessário para o protocolo NFS v 4.1.

### <a name="service-for-nfs-model-overview"></a>Visão geral do modelo de serviço para NFS

O Microsoft Services for NFS fornece uma solução de compartilhamento de arquivos para empresas que têm um ambiente misto do Windows e do UNIX. Esse modelo de comunicação consiste em computadores cliente e um servidor do. Os aplicativos nos arquivos de solicitação do cliente que estão localizados no servidor por meio do redirecionador (Rdbss.sys) e do mini-redirecionador NFS (Nfsrdr.sys). O mini-redirecionador usa o protocolo NFS para enviar sua solicitação por meio de TCP/IP. O servidor recebe várias solicitações dos clientes por meio de TCP/IP e roteia as solicitações para o sistema de arquivos local (Ntfs.sys), que acessa a pilha de armazenamento.

A figura a seguir mostra o modelo de comunicação para NFS.

![modelo de comunicação NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Parâmetros de ajuste para servidores de arquivos NFS

As seguintes \_ configurações de registro do reg DWORD podem afetar o desempenho dos servidores de arquivos NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    O padrão é 0. Esse parâmetro determina se os arquivos são abertos para \_ acesso aleatório de arquivo \_ ou \_ somente para arquivo sequencial \_ , dependendo das características de e/s de carga de trabalho. Defina esse valor como 1 para forçar a abertura de arquivos para \_ acesso aleatório de arquivo \_ . \_ \_ O acesso aleatório do arquivo impede que o sistema de arquivos e o Gerenciador de cache se probusquem.

    >[!NOTE]
    > Essa configuração deve ser avaliada com cuidado porque pode ter impacto potencial no aumento do cache de arquivos do sistema.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    O padrão é 5. Esse parâmetro controla o tempo de vida de uma entrada de cache NFS no cache de identificadores de arquivo. O parâmetro refere-se a entradas de cache que têm um identificador de arquivo NTFS aberto associado. O tempo de vida real é aproximadamente igual a RdWrHandleLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    O padrão é 5. Esse parâmetro controla o tempo de vida de uma entrada de cache NFS no cache de identificadores de arquivo. O parâmetro refere-se a entradas de cache que não têm um identificador de arquivo NTFS aberto associado. Os serviços de NFS usam essas entradas de cache para armazenar atributos de arquivo para um arquivo sem manter um identificador aberto com o sistema de arquivos. O tempo de vida real é aproximadamente igual a RdWrNfsHandleLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    O padrão é 5. Esse parâmetro controla o tempo de vida de uma entrada de cache de leitura de NFS no cache de identificadores de arquivo. O tempo de vida real é aproximadamente igual a RdWrNfsReadHandlesLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    O padrão é 5. Esse parâmetro controla o intervalo de espera antes de executar o thread de limpeza no cache de identificadores de arquivo. O valor está em tiques e não é determinístico. Um tique é equivalente a aproximadamente 100 nanossegundos. O mínimo é 1 e o máximo é 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    O padrão é 4. Esse parâmetro especifica a memória máxima a ser consumida por entradas de cache de identificador de arquivo. O mínimo é 1 e o máximo é 1 \* 1024 \* 1024 \* 1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    O padrão é 0. Esse parâmetro especifica se as páginas físicas alocadas para o tamanho do cache especificado por FileHandleCacheSizeInMB estão bloqueadas na memória. Definir esse valor como 1 habilita essa atividade. As páginas são bloqueadas na memória (não paginadas no disco), o que melhora o desempenho da resolução de identificadores de arquivo, mas reduz a memória que está disponível para os aplicativos.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    O padrão é 64. Esse parâmetro especifica o número máximo de identificadores por volume para o cache de dados de leitura. As entradas de cache de leitura são criadas somente em sistemas com mais de 1 GB de memória. O mínimo é 0 e o máximo é 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    O padrão é 1. Esse parâmetro controla se identificadores que são fornecidos pelo servidor de arquivos NFS são assinados criptograficamente. Configurá-lo como 0 desabilita a assinatura de identificador.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    O padrão é 60. Esse parâmetro é um tempo limite flexível que controla a duração do NFS v3 em cache de gravação de dados instável. O mínimo é 1 e o máximo é 600. O tempo de vida real é aproximadamente igual a RdWrNfsDeferredWritesFlushDelay multiplicado por RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    O padrão é 1 (habilitado). Esse parâmetro controla se os identificadores que são abertos durante o NFS v2 e v3 criam e MKDIR manipuladores de procedimento RPC são mantidos no cache de identificadores de arquivo. Defina esse valor como 0 para desabilitar a adição de entradas ao cache nos caminhos de código CREATE e MKDIR.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta o número de threads de trabalho atrasados que são criados para a fila de trabalho especificada. Threads de trabalho atrasados processam itens de trabalho que não são considerados de tempo crítico e que podem ter sua pilha de memória paginada enquanto aguardam itens de trabalho. Um número insuficiente de threads reduz a taxa em que os itens de trabalho são atendidos; um valor muito alto consome recursos do sistema desnecessariamente.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    O padrão no Windows Server 2012 e no Windows Server 2012 R2 é 2. Em versões anteriores ao Windows Server 2012, o padrão é 0. Esse parâmetro determina se o NTFS gera um nome curto na Convenção de nomenclatura 8dot3 (MSDOS) para nomes de arquivos longos e para nomes de arquivos que contêm caracteres do conjunto de caracteres estendido. Se o valor dessa entrada for 0, os arquivos poderão ter dois nomes: o nome que o usuário especifica e o nome curto que o NTFS gera. Se o nome especificado pelo usuário seguir a Convenção de nomenclatura 8dot3, o NTFS não gerará um nome curto. Um valor de 2 significa que esse parâmetro pode ser configurado por volume.

    >[!NOTE]
    > O volume do sistema tem 8dot3 habilitado por padrão. Todos os outros volumes no Windows Server 2012 e no Windows Server 2012 R2 têm o 8dot3 desabilitado por padrão. A alteração desse valor não altera o conteúdo de um arquivo, mas evita a criação de atributos de nome curto para o arquivo, o que também altera o modo como o NTFS exibe e gerencia o arquivo. Para a maioria dos servidores de arquivos, a configuração recomendada é 1 (desabilitada).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    O padrão é 1. Esse comutador global do sistema reduz a carga e as latências de e/s do disco desabilitando a atualização do carimbo de data/hora para o último acesso ao diretório ou ao arquivo.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    O valor padrão do parâmetro MaxConcurrentConnectionsPerIp é 16. Você pode aumentar esse valor até um máximo de 8192 para aumentar o número de conexões por endereço IP.
