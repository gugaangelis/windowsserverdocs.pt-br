---
title: Ajuste de desempenho Área de Trabalho Remota gateways
description: Recomendações de ajuste de desempenho para gateways de Área de Trabalho Remota
ms.topic: article
ms.author: hammadbu; vladmis
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8aeaf524b3621c61cfdd6b0e4edd9d0f8f73ebae
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896043"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Ajuste de desempenho Área de Trabalho Remota gateways

> [!NOTE]
> No Windows 8 + e no Windows Server 2012 R2 +, o gateway de Área de Trabalho Remota (gateway de área de trabalho remota) dá suporte a TCP, UDP e aos transportes de RPC herdados. A maioria dos dados a seguir se refere ao transporte RPC herdado. Se o transporte RPC herdado não estiver sendo usado, esta seção não será aplicável.

Este tópico descreve os parâmetros relacionados ao desempenho que ajudam a melhorar o desempenho de uma implantação de cliente e os ajustes que dependem dos padrões de uso de rede do cliente.

Em seu núcleo, o gateway de área de trabalho remota executa muitas operações de encaminhamento de pacotes entre Conexão de Área de Trabalho Remota instâncias e as instâncias de servidor Host da Sessão RD na rede do cliente.

> [!NOTE]
> Os parâmetros a seguir se aplicam somente ao transporte RPC.

Serviços de Informações da Internet (IIS) e Gateway RD, exporte os seguintes parâmetros de registro para ajudar a melhorar o desempenho do sistema no gateway de área de trabalho remota.

**Ajustes de thread**

-   **MaxIOThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Esse pool de threads específicos do aplicativo especifica o número de threads que o gateway de área de trabalho remota cria para tratar as solicitações de entrada. Se essa configuração do registro estiver presente, ela entrará em vigor. O número de threads é igual ao número de processos lógicos. Se o número de processadores lógicos for menor que 5, o padrão será 5 threads.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Esse parâmetro especifica o número de threads do pool do IIS a serem criados por processador lógico. Os threads do pool do IIS inspecionam a rede em busca de solicitações e processam todas as solicitações de entrada. A contagem de **MaxPoolThreads** não inclui threads consumidos pelo gateway de área de trabalho remota. O valor padrão é 4.

**Ajustes de chamada de procedimento remoto para Gateway RD**

Os parâmetros a seguir podem ajudar a ajustar as chamadas de procedimento remoto (RPC) que são recebidas por Conexão de Área de Trabalho Remota e computadores de gateway de área de trabalho remota. Alterar as janelas ajuda a limitar a quantidade de dados fluindo por cada conexão e pode melhorar o desempenho para cenários RPC sobre HTTP v2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    O valor padrão é 64 KB. Esse valor especifica a janela que o servidor usa para dados que são recebidos do proxy RPC. O valor mínimo é definido como 8 KB e o valor máximo é definido como 1 GB. Se um valor não estiver presente, o valor padrão será usado. Quando são feitas alterações nesse valor, o IIS deve ser reiniciado para que a alteração entre em vigor.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    O valor padrão é 64 KB. Esse valor especifica a janela que o cliente usa para dados que são recebidos do proxy RPC. O valor mínimo é 8 KB e o valor máximo é 1 GB. Se um valor não estiver presente, o valor padrão será usado.

## <a name="monitoring-and-data-collection"></a>Monitoramento e coleta de dados

A lista de contadores de desempenho a seguir é considerada um conjunto base de contadores quando você monitora o uso de recursos no gateway de área de trabalho remota:

-   \\Gateway de serviço de terminal\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP por servidor\\\*

-   \\Serviço Web\\\*

-   \\O W3SVC \_ w3wp\\\*

-   \\IPv6\\\*

-   \\Memória\\\*

-   \\Interface de rede ( \* )\\\*

-   \\Processo ( \* )\\\*

-   \\Informações do processador ( \* )\\\*

-   \\Sincronização ( \* )\\\*

-   \\Sistema\\\*

-   \\TCPv4\\\*

Os contadores de desempenho a seguir são aplicáveis somente para transporte RPC herdado:

-   \\RPC de proxy RPC/HTTP \\ \*

-   \\Proxy RPC/HTTP por servidor \\ \* RPC

-   \\RPC do serviço Web \\ \*

-   \\\_RPC de w3wp de W3SVC \\ \*

> [!NOTE]
> Se aplicável, adicione os \\ \\ \* objetos IPv6 e \\ TCPv6 \\ \* . ReplaceThisText

