---
title: Solução de problemas NIC agrupamento
description: Este tópico fornece informações sobre como solucionar problemas de agrupamento de placa de rede no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>Solução de problemas NIC agrupamento

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece informações sobre como solucionar problemas de agrupamento de NIC e contém as seguintes seções, que descrevem as causas possíveis problemas com o agrupamento de placa de rede.  
  
-   [Hardware que não seguem a especificação](#bkmk_hardware)  
  
-   [Recursos de segurança do comutador físico](#bkmk_switch)  
  
-   [Desabilitar e habilitar com o Windows PowerShell](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>Hardware que não seguem a especificação  
Quando implementações de hardware de protocolos padrão não estão em conformidade com a especificação, desempenho NIC agrupamento pode ser afetado.  
  
Durante a operação normal, NIC agrupamento pode enviar pacotes do mesmo endereço IP, mas com acesso de mídia de origem diferentes vários endereços MAC (controle). Acordo com padrões de protocolo, os receptores desses pacotes devem resolver o endereço IP do host ou VM um endereço MAC específico em vez de responder para o endereço MAC do qual o pacote foi recebido.  Os clientes que implementam corretamente os protocolos de resolução de endereço, Address Resolution Protocol (ARP do IPv4) ou protocolo de descoberta de vizinho do IPv6 (NDP), enviarão pacotes com o endereço MAC (o endereço MAC do host que possui o endereço IP ou da VM) de destino correto.  
  
Alguns inserido hardware, no entanto, não implementam corretamente os protocolos de resolução de endereço e também podem não explicitamente resolver um endereço IP para um endereço MAC usando ARP ou NDP.  Um controlador de rede (SAN) de área de armazenamento é um exemplo de um dispositivo que pode realizar dessa maneira. Dispositivos em conformidade não copiar a origem de endereço MAC que está contido em um pacote recebido e usá-lo como o endereço MAC nos pacotes de saída correspondentes de destino.  
  
Isso resulta no pacote enviado para o endereço MAC de destino errado. Por isso, os pacotes são ignorados pelo Switch Hyper-V Virtual porque eles não corresponderem qualquer destino conhecido.  
  
Se você estiver tendo problemas ao se conectar a controladores de SAN ou outro hardware inserido, tirar capturas de pacote e determine se seu hardware é implementar corretamente ARP ou NDP e contate o fornecedor de hardware para suporte.  
  
## <a name="bkmk_switch"></a>Recursos de segurança do comutador físico  
Dependendo da configuração, a NIC agrupamento pode enviar pacotes do mesmo endereço IP com vários endereços MAC de fonte diferente.  Isso pode viagem a segurança proteger recursos sob o comutador físico, como inspeção dinâmica de ARP ou origem IP, especialmente se o comutador físico não está ciente de que as portas são parte de uma equipe. Isso pode ocorrer se você definir o agrupamento de NIC no modo independente do Switch.  Você deve inspecionar os logs de alternar para determinar se os recursos de segurança do switch estão causando problemas de conectividade com NIC agrupamento.  
  
## <a name="bkmk_ps"></a>Desabilitar e habilitar os adaptadores de rede usando o Windows PowerShell  
Um motivo comum para uma equipe de NIC falhar é que a interface de equipe está desabilitada. Em muitos casos, a interface está desabilitada por acidente quando a seguinte sequência do Windows PowerShell de comandos é executada:  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
Esta sequência de comandos não habilite todos os NetAdapters que ele desabilitado.  
  
Isso ocorre porque desabilitar todas as NICs físico membro subjacente faz com que a interface de equipe NIC a ser removido e não aparecerão mais no Get-NetAdapter. Por isso, o **NetAdapter habilitar \ *** comando não habilita a equipe de NIC, porque esse adaptador é removido.  
  
O **NetAdapter habilitar \ *** comando, no entanto, ativar a configuração membro NICs, que, em seguida, (após um curto período de tempo) faz com que a interface de equipe ser recriadas. Neste caso, a interface de equipe ainda está em um estado "desabilitado" porque não foi reativado. Habilitando a interface de equipe depois que ele é recriado permitirá que o tráfego de rede começar a fluir novamente.  
  
## <a name="see-also"></a>Consulte também  
[Agrupamento de NIC](NIC-Teaming.md)  
  


