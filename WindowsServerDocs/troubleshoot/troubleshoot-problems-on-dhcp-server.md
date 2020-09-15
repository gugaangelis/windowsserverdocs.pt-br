---
title: Solucionar problemas no servidor DHCP
description: Este Artilce apresenta como solucionar problemas no servidor DHCP e coletar dados.
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: a6b5e4128c2e07e51ab8a9c07155a8c0212fcad8
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078583"
---
# <a name="troubleshoot-problems-on-the-dhcp-server"></a>Solucionar problemas no servidor DHCP

Este artigo discute como solucionar problemas que ocorrem no servidor DHCP.

## <a name="troubleshooting-checklist"></a>Lista de verificação de solução de problemas

Verifique as seguintes configurações:

  - O serviço do servidor DHCP foi iniciado e está em execução. Para verificar essa configuração, execute o comando **net start** e procure o **servidor DHCP**.

  - O servidor DHCP está autorizado. Consulte [autorização do servidor DHCP do Windows no cenário ingressado no domínio](/openspecs/windows_protocols/ms-dhcpe/56f8870b-a7c1-4db1-8a86-f69079fe5077).

  - Verifique se as concessões de endereço IP estão disponíveis no escopo do servidor DHCP para a sub-rede em que o cliente DHCP está. Para fazer isso, consulte a estatística do escopo apropriado no console de gerenciamento do servidor DHCP.

  - Verifique se alguma \_ lista de endereços INválidos pode ser encontrada em concessões de endereço.

  - Verifique se algum dispositivo na rede tem endereços IP estáticos que não foram excluídos do escopo DHCP.

  - Verifique se o endereço IP ao qual o servidor DHCP está associado está dentro da sub-rede dos escopos do qual os endereços IP devem ser concedidos. Isso ocorre caso nenhum agente de retransmissão esteja disponível. Para fazer isso, execute o cmdlet **Get-DhcpServerv4Binding** ou **Get-DhcpServerv6Binding** .

  - Verifique se apenas o servidor DHCP está escutando na porta UDP 67 e 68. Nenhum outro processo ou outro serviço (como WDS ou PXE) deve ocupar essas portas. Para fazer isso, execute o `netstat -anb` comando.

  - Verifique se a isenção de IPsec do servidor DHCP foi adicionada se você estiver lidando com um ambiente implantado por IPsec.

  - Verifique se o endereço IP do agente de retransmissão pode receber ping do servidor DHCP.

  - Enumere e verifique as políticas e filtros DHCP configurados.

## <a name="event-logs"></a>Logs de eventos

Verifique os logs de eventos do serviço do servidor DHCP e do sistema (**logs de aplicativos e serviços** \> **Microsoft** \> **Windows** \> **DHCP-Server**) para problemas relatados relacionados ao problema observado.
Dependendo do tipo de problema, um evento é registrado em um dos seguintes canais de eventos: servidor DHCP eventos [operacionais](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))servidor DHCP eventos administrativos de servidor DHCP eventos do sistema DHCP servidor filtro eventos de 
 [DHCP Server Administrative Events](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [DHCP Server System Events](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [notificação](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [servidor](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) DHCP eventos de auditoria

## <a name="data-collection"></a>Coleta de dados

### <a name="dhcp-server-log"></a>Log do servidor DHCP

Os logs de depuração do serviço do servidor DHCP fornecem mais informações sobre a atribuição de concessão de endereço IP e as atualizações dinâmicas de DNS feitas pelo servidor DHCP. Esses logs por padrão estão localizados em% windir% \\ System32 \\ DHCP.
Para obter mais informações, consulte [analisar arquivos de log do servidor DHCP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd183591\(v=ws.10\)).

### <a name="network-trace"></a>Rastreamento de rede

Um rastreamento de rede correlacionado pode indicar o que o servidor DHCP estava fazendo no momento em que o evento foi registrado. Para criar um rastreamento desse tipo, siga estas etapas:

1.  Acesse o [GitHub](https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS)e baixe o arquivo de [ \_tools.zipTSS ](https://github.com/CSS-Windows/WindowsDiag/blob/master/ALL/TSS/tss_tools.zip) .

2.  Copie o \_ arquivo detools.zip TSS e expanda-o para um local no disco local, como a pasta C: \\ Tools.

3.  Execute o seguinte comando em C: \\ Tools em uma janela de prompt de comandos com privilégios elevados:
    ```console
    TSS Ron Trace <Stop:Evt:>20321:<Other:>DhcpAdminEvents NoSDP NoPSR NoProcmon NoGPresult
    ```

    >[!Note]
    >Nesse comando, substitua \<*Stop:Evt:*\> e \<*Other:*\> pela ID do evento e pelo canal de evento no qual você vai se concentrar em sua sessão de rastreamento.
    >O Leiame do TSS. cmd \_ \_Help.docx arquivos contidos no arquivo TSS \_tools.zip fornece mais informações sobre todas as configurações disponíveis.

4.  Depois que o evento é disparado, a ferramenta cria uma pasta denominada C: \\ MS \_ Data. Essa pasta conterá alguns arquivos de saída úteis que fornecem informações gerais sobre a configuração de rede e domínio do computador.
    O arquivo mais interessante nesta pasta é% ComputerName% \_ Date \_ time \_ packetcapture \_ internetclient \_ DBG. etl.
    Usando o aplicativo [Monitor de rede](https://www.microsoft.com/download/4865) , você pode carregar o arquivo e definir o filtro de exibição no protocolo "DHCP ou DNS" para examinar o que está acontecendo nos bastidores.