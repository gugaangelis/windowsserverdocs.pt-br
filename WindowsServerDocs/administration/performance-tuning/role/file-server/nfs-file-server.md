---
title: Ajuste de desempenho para servidores de arquivos NFS
description: Ajuste de desempenho para servidores de arquivos NFS
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879007"
---
# <a name="performance-tuning-nfs-file-servers"></a>Servidores de arquivos NFS de ajuste de desempenho

## <a href="" id="servicesnfs"></a>Serviços para o modelo NFS


As seções a seguir fornecem informações sobre o Microsoft Services para o modelo de sistema de arquivos de rede (NFS) para comunicação cliente-servidor. Como NFS v2 e v3 do NFS são ainda mais difundido versões do protocolo, todas as chaves do registro, exceto MaxConcurrentConnectionsPerIp se aplicam a v2 NFS e NFS v3 apenas.

Nenhum ajuste de registro é necessário para o protocolo de v 4.1 NFS.

### <a name="service-for-nfs-model-overview"></a>Serviço de visão geral do modelo NFS

Microsoft Services for NFS oferece uma solução de compartilhamento de arquivos para empresas que têm um ambiente misto de Windows e UNIX. Esse modelo de comunicação consiste em computadores cliente e um servidor. Os aplicativos no cliente solicitar os arquivos que estão localizados no servidor por meio do redirecionador (Rdbss) e o mini-redirecionador de NFS (Nfsrdr.sys). O minirredirecionador usa o protocolo NFS para enviar sua solicitação por meio de TCP/IP. O servidor recebe várias solicitações de clientes por meio de TCP/IP e encaminha as solicitações para o sistema de arquivos local (sys), que acessa a pilha de armazenamento.

A figura a seguir mostra o modelo de comunicação de NFS.

![modelo de comunicação do NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Ajustar parâmetros de NFS para servidores de arquivos

O seguinte REG\_configurações do Registro DWORD podem afetar o desempenho dos servidores de arquivos NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    O padrão é 0. Esse parâmetro determina se os arquivos são abertos para arquivo\_RANDOM\_acesso ou para o arquivo\_SEQUENCIAL\_apenas, dependendo das características de carga de trabalho e/s. Defina esse valor como 1 para forçar os arquivos a ser aberto para o arquivo\_RANDOM\_acesso. ARQUIVO\_RANDOM\_acesso impede que o Gerenciador de cache e o sistema de arquivos a pré-busca.

    >[!NOTE]
    > Essa configuração deve ser avaliada cuidadosamente, porque ele pode ter impacto em potencial no cache de arquivo do sistema crescem.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    O padrão é 5. Este parâmetro controla o tempo de vida de uma entrada de cache NFS no cache de identificador de arquivo. O parâmetro refere-se em cache as entradas que têm um arquivo NTFS aberto associado a manipular. Tempo de vida real é aproximadamente igual ao RdWrHandleLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é de 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    O padrão é 5. Este parâmetro controla o tempo de vida de uma entrada de cache NFS no cache de identificador de arquivo. O parâmetro refere-se a entradas de cache que não têm um arquivo NTFS aberto associado manipular. Serviços de NFS usa essas entradas de cache para armazenar os atributos de arquivo para um arquivo sem manter um identificador aberto com o sistema de arquivos. Tempo de vida real é aproximadamente igual ao RdWrNfsHandleLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é de 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    O padrão é 5. Este parâmetro controla o tempo de vida de um NFS ler a entrada de cache no cache de identificador de arquivo. Tempo de vida real é aproximadamente igual ao RdWrNfsReadHandlesLifeTime multiplicado por RdWrThreadSleepTime. O mínimo é 1 e o máximo é de 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    O padrão é 5. Este parâmetro controla o intervalo de espera antes de executar o thread de limpeza do cache de identificador de arquivo. O valor é em tiques, e é não determinística. Um tique é equivalente a cerca de 100 nanossegundos. O mínimo é 1 e o máximo é de 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    O padrão é 4. Esse parâmetro especifica o máximo de memória a ser consumido por entradas de cache de identificador de arquivo. O mínimo é 1 e o máximo é de 1\*1024\*1024\*1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    O padrão é 0. Esse parâmetro especifica se a páginas físicas alocadas para o tamanho de cache especificado pelo FileHandleCacheSizeInMB são bloqueadas na memória. Definir esse valor como 1 habilita essa atividade. As páginas são bloqueadas na memória (não paginada em disco), que melhora o desempenho da solução de identificadores de arquivos, mas reduz a memória que está disponível para aplicativos.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    O padrão é 64. Esse parâmetro especifica o número máximo de identificadores por volume para o cache de leitura de dados. Entradas de cache de leitura são criadas apenas em sistemas que têm mais de 1 GB de memória. O mínimo é 0 e o máximo é de 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    O padrão é 1. Este parâmetro controla se os identificadores fornecidos pelo servidor de arquivos NFS são assinados criptograficamente. Configurando-a como 0 desabilita o identificador de assinatura.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    O padrão é 60. Esse parâmetro é um tempo limite flexível que controla a duração do cache de dados de gravação do NFS V3 instável. O mínimo é 1 e o máximo é de 600. Tempo de vida real é aproximadamente igual ao RdWrNfsDeferredWritesFlushDelay multiplicado por RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    O padrão é 1 (habilitado). Este parâmetro controla se os identificadores são abertos durante NFS V2 e V3 criar e RPC MKDIR procedimento manipuladores são mantidos no arquivo de lidar com cache. Defina esse valor como 0 para desabilitar a adicionar as entradas no cache em CREATE e MKDIR caminhos de código.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta o número de threads de trabalho atrasado que são criados para a fila de trabalhos especificada. Atrasada worker threads processo itens de trabalho que não são considerados críticos em termos de tempo e que pode ter sua pilha de memória paginada enquanto aguarda a itens de trabalho. Um número insuficiente de threads reduz a taxa em que o trabalho itens são atendidas; um valor que é muito alto consome recursos do sistema desnecessariamente.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    O padrão no Windows Server 2012 e Windows Server 2012 R2 é 2. Em versões anteriores ao Windows Server 2012, o padrão é 0. Esse parâmetro determina se o NTFS gera um nome curto no 8dot3 convenção de nomenclatura (MSDOS) para nomes de arquivo longos em nomes de arquivo que contêm caracteres do conjunto de caracteres estendidos. Se o valor dessa entrada for 0, os arquivos podem ter dois nomes: o nome especificado pelo usuário e o nome curto que gera NTFS. Se o nome especificado pelo usuário segue a convenção de nomenclatura de 8dot3, NTFS não gera um nome curto. Um valor de 2 significa que esse parâmetro pode ser configurado por volume.

    >[!NOTE]
    > O volume do sistema tem 8dot3 habilitado por padrão. Todos os outros volumes no Windows Server 2012 e Windows Server 2012 R2 tem 8dot3 desabilitado por padrão. A alteração desse valor não altera o conteúdo de um arquivo, mas evita a criação de atributo de nome curto para o arquivo, que também altera como NTFS exibe e gerencia o arquivo. A maioria dos servidores de arquivos, a configuração recomendada é 1 (desativado).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    O padrão é 1. Essa opção global do sistema reduz a carga de e/s de disco e latências, desabilitando a atualização do carimbo de data e hora para o último acesso ao arquivo ou diretório.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    O valor padrão do parâmetro MaxConcurrentConnectionsPerIp é 16. Você pode aumentar esse valor até um máximo de 8192 para aumentar o número de conexões por endereço IP.
