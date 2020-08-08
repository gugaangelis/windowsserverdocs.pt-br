---
title: Ajuste de desempenho para servidores de arquivos
description: Ajuste de desempenho para servidores de arquivos que executam o Windows Server
ms.topic: article
author: phstee
ms.author: nedpyle; danlo; dkruse; v-tea
ms.date: 12/12/2019
manager: dcscontentpm
audience: Admin
ms.openlocfilehash: ecbd1bc751f133b80cf1d9cb264cf70a4ac4f47c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992195"
---
# <a name="performance-tuning-for-file-servers"></a>Ajuste de desempenho para servidores de arquivos

Você deve selecionar o hardware adequado para atender à carga esperada do servidor de arquivos, considerando a carga média, a carga de pico, a capacidade, os planos de crescimento e os tempos de resposta. Os gargalos de hardware limitam a eficiência do ajuste do software.

## <a name="general-tuning-parameters-for-clients"></a>Parâmetros de ajustes gerais para clientes

As seguintes configurações do Registro REG\_DWORD podem afetar o desempenho dos computadores cliente que interagem com os servidores de arquivos SMB:

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    Aplica-se ao Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    O padrão é 1 e é altamente recomendável usá-lo. O intervalo válido é de 1 a 16. O número máximo de conexões por interface a ser estabelecido com um servidor para interfaces não RSS.


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    Aplica-se ao Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    O padrão é 4 e é altamente recomendável usá-lo. O intervalo válido é de 1 a 16. O número máximo de conexões por interface a ser estabelecido com um servidor para interfaces RSS.

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    Aplica-se ao Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    O padrão é 2 e é altamente recomendável usá-lo. O intervalo válido é de 1 a 16. O número máximo de conexões por interface a ser estabelecido com um servidor para interfaces RDMA.

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    Aplica-se ao Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    O padrão é 32, com um intervalo válido de 1 a 64. O número máximo de conexões a ser estabelecido com um único servidor executando o Windows Server 2012 em todas as interfaces.

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    Aplica-se ao Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    O padrão é 600 segundos. O diretório do servidor de horário máximo é tratado mantido aberto com concessões de diretório.

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 10 segundos. O período de tempo limite do cache de informações de arquivo.

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 10 segundos. Esse é o tempo limite do cache do diretório.

    > [!NOTE]
    > Esse parâmetro controla o cache de metadados de diretório na ausência de concessões de diretório.

     > [!NOTE]
     > Um problema conhecido no Windows 10 versão 1803 afeta a capacidade do Windows 10 de armazenar em cache grandes diretórios. Depois de fazer o upgrade de um computador para o Windows 10 versão 1803, você acessa um compartilhamento de rede que contém milhares de arquivos e pastas e abre um documento que está localizado nesse compartilhamento. Durante ambas as operações, você enfrenta atrasos significativos.
     >
     > Para resolver esse problema, instale o Windows 10 versão 1809 ou uma versão posterior.
     >
     > Para contornar esse problema, defina **DirectoryCacheLifetime** como **0**.
     >
     > Esse problema afeta as seguintes edições do Windows 10:
     > - Windows 10 Enterprise, versão 1803
     > - Windows 10 Pro for Workstations, versão 1803
     > - Windows 10 Pro Education, versão 1803
     > - Windows 10 Professional, versão 1803
     > - Windows 10 Education, versão 1803
     > - Windows 10 Home, versão 1803

-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 64 KB. Esse é o tamanho máximo de entradas de cache de diretório.

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 5 segundos. O arquivo não encontrou o período de tempo limite de cache.

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    Aplica-se a Windows 8.1, Windows 8, Windows Server 2012, Windows Server 2012 R2 e Windows 7

    O padrão é 10 segundos. Essa configuração controla o período de tempo (em segundos) que o redirecionador manterá os dados em cache para um arquivo após o último identificador para o arquivo ser fechado por um aplicativo.

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 0. Por padrão, o redirecionador SMB limita a taxa de transferência em conexões de rede de alta latência, em alguns casos, para evitar tempos limite relacionados à rede. Definir esse valor de Registro como 1 desabilita essa limitação, permitindo uma taxa de transferência de arquivo mais alta por conexões de rede de alta latência.

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 0 somente para o Windows 8. No Windows 8, o redirecionador SMB transfere conteúdos tão grandes quanto 1 MB por solicitação, o que pode melhorar a velocidade de transferência de arquivo. Definir esse valor de Registro como 1 limita o tamanho da solicitação a 64 KB. Você deve avaliar o impacto dessa configuração antes de aplicá-la.

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 0, que desabilita a Assinatura SMB. Alterar esse valor para 1 habilita a assinatura SMB para toda a comunicação SMB, impedindo a comunicação SMB com computadores nos quais a assinatura SMB está desabilitada. A assinatura SMB pode aumentar o custo da CPU e as idas e voltas da rede, mas ajuda a bloquear ataques man-in-the-middle. Se a assinatura SMB não for necessária, verifique se esse valor de Registro é 0 em todos os clientes e servidores.

    Para saber mais, confira [The Basics of SMB Signing](/archive/blogs/josebda/the-basics-of-smb-signing-covering-both-smb1-and-smb2) (As noções básicas da assinatura SMB).

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 64, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de metadados de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando um grande número de arquivos é acessado.

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 16, com um intervalo válido de 1 a 4096. Esse valor é usado para determinar a quantidade de informações de diretório que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando diretórios grandes são acessados.

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 128, com um intervalo válido de 1 a 65536. Esse valor é usado para determinar a quantidade de informações de nome de arquivo que pode ser armazenada em cache pelo cliente. Aumentar o valor pode reduzir o tráfego de rede e aumentar o desempenho quando um grande número de nomes de arquivos é acessado.

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 15. Esse parâmetro limita o número de solicitações pendentes em uma sessão. Aumentar o valor pode usar mais memória, mas pode melhorar o desempenho habilitando um pipeline de solicitação mais profundo. Aumentar o valor em conjunto com MaxMpxCt também pode eliminar erros encontrados devido aos grandes números de solicitações de arquivo de longo prazo pendentes, como chamadas FindFirstChangeNotification. Esse parâmetro não afeta as conexões com servidores SMB 2.0.

-   **DormantFileLimit**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    Aplica-se a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    O padrão é 1023. Esse parâmetro especifica o número máximo de arquivos que deve ser aberto em um recurso compartilhado após o aplicativo fechar o arquivo.

### <a name="client-tuning-example"></a>Exemplo de ajuste de cliente

Os parâmetros de ajustes gerais para computadores cliente podem otimizar um computador para acessar compartilhamentos de arquivos remotos, particularmente por meio de algumas redes de alta latência (como filiais, comunicação entre datacenters, home offices e banda larga móvel). As configurações não são ideais nem adequadas em todos os computadores. Você deve avaliar o impacto de configurações individuais antes de aplicá-las.

| Parâmetro                   | Valor | Padrão |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32768 | 64      |
| DirectoryCacheEntriesMax    | 4096  | 16      |
| FileNotFoundCacheEntriesMax | 32768 | 128     |
| MaxCmds                     | 32768 | 15      |



Começando com o Windows 8, é possível definir muitas configurações SMB usando os cmdlets **Set-SmbClientConfiguration** e **Set-SmbServerConfiguration** do Windows PowerShell. Configurações somente de Registro podem ser definidas usando o Windows PowerShell também.

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```