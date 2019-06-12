---
title: Gateways de área de trabalho remotos de ajuste de desempenho
description: Desempenho de recomendações de ajuste para Gateways de área de trabalho remota
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f3ac020b3137621f6b2535c973ab7759443e1535
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811429"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Gateways de área de trabalho remotos de ajuste de desempenho

> [!NOTE]
> No Windows 8 e posteriores e no Windows Server 2012 R2 +, o Gateway de área de trabalho remota (Gateway RD) oferece suporte a TCP, UDP e os transportes RPC herdados. A maioria dos seguintes dados é em relação ao transporte RPC herdado. Se o transporte herdado de RPC não está sendo usado, esta seção não é aplicável.

Este tópico descreve os parâmetros relacionados ao desempenho que ajudam a melhorar o desempenho de uma implantação de cliente e o tunings que se baseiam em padrões de uso de rede do cliente.

Em seu núcleo, o Gateway de área de trabalho remota executa muitas operações entre instâncias de Conexão de área de trabalho remota e as instâncias de servidor de Host de sessão de área de trabalho remota na rede do cliente de encaminhamento de pacote.

> [!NOTE]
> Os parâmetros a seguir se aplicam a apenas a transporte RPC.

Serviços de informações da Internet (IIS) e o Gateway de área de trabalho remota exportar os seguintes parâmetros de registro para ajudar a melhorar o desempenho do sistema no Gateway de área de trabalho remota.

**Thread tunings**

-   **MaxIOThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Esse pool de threads específicos do aplicativo especifica o número de threads que o Gateway de área de trabalho remota cria para lidar com solicitações de entrada. Se essa configuração do registro estiver presente, ele entra em vigor. O número de threads é igual ao número de processos lógicos. Se o número de processadores lógicos for menor que 5, o padrão é 5 segmentos.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Esse parâmetro especifica o número de threads de pool do IIS para criar por processador lógico. Os threads do pool IIS Assista a rede para solicitações e processam todas as solicitações de entrada. O **MaxPoolThreads** contagem não inclui os threads que consome o Gateway de área de trabalho remota. O valor padrão é 4.

**Tunings de chamada de procedimento remoto para o Gateway de área de trabalho remota**

Os parâmetros a seguir podem ajudar a ajustar as chamadas de procedimento remoto (RPC) que são recebidas pela Conexão de área de trabalho remota e Gateway de área de trabalho remota de computadores. Alterar o windows ajuda a limitar a quantidade de dados está fluindo por meio de cada conexão e pode melhorar o desempenho de RPC sobre cenários de v2 HTTP.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    O valor padrão é 64 KB. Esse valor Especifica a janela que o servidor usa para dados recebidos do proxy RPC. O valor mínimo é definido como 8 KB e o valor máximo é definido em 1 GB. Se um valor não estiver presente, o valor padrão é usado. Quando forem feitas alterações a esse valor, o IIS deve ser reiniciado para que a alteração tenha efeito.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    O valor padrão é 64 KB. Esse valor Especifica a janela que o cliente usa para dados recebidos do proxy RPC. O valor mínimo é de 8 KB e o valor máximo é de 1 GB. Se um valor não estiver presente, o valor padrão é usado.

## <a name="monitoring-and-data-collection"></a>Monitoramento e coleta de dados

A seguinte lista de contadores de desempenho é considerada um conjunto básico de contadores ao monitorar o uso de recursos no Gateway de área de trabalho remota:

-   \\Gateway de serviços de terminal\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP por servidor\\\*

-   \\Serviço Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\Memória\\\*

-   \\Network Interface(\*)\\\*

-   \\Processo (\*)\\\*

-   \\Informações do processador (\*)\\\*

-   \\Sincronização (\*)\\\*

-   \\Sistema\\\*

-   \\TCPv4\\\*

Os seguintes contadores de desempenho são aplicáveis somente para o transporte RPC herdado:

-   \\Proxy RPC/HTTP\\ \* RPC

-   \\Proxy RPC/HTTP por servidor\\ \* RPC

-   \\Serviço Web\\ \* RPC

-   \\W3SVC\_W3WP\\\* RPC

> [!NOTE]
> Se aplicável, adicione a \\IPv6\\ \* e \\TCPv6\\ \* objetos. ReplaceThisText

