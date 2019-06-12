---
title: Solucionar problemas de agrupamento NIC
description: Este tópico fornece informações sobre como solucionar problemas de agrupamento NIC no Windows Server 2016.
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: a6af3cbd038e97d889269b83d72c77c50680e513
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446173"
---
# <a name="troubleshooting-nic-teaming"></a>Solucionar problemas de agrupamento NIC

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento NIC, como o hardware e valores mobiliários de comutador físico.  Quando as implementações de protocolos padrão de hardware não está em conformidade com as especificações, o agrupamento NIC desempenho pode ser afetado. Além disso, dependendo da configuração, o agrupamento NIC pode enviar pacotes do mesmo endereço IP com vários endereços MAC enganar os recursos de segurança no comutador físico.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware que não se adequa à especificação  
  
Durante a operação normal, o agrupamento NIC pode enviar pacotes do mesmo endereço IP, ainda com vários endereços MAC. Acordo com os padrões de protocolo, os destinatários desses pacotes devem resolver o endereço IP do host ou VM para um endereço MAC específico em vez de responder ao endereço MAC do pacote de recebimento.  Os clientes que implementam corretamente os protocolos de resolução de endereço (ARP e NDP) enviam pacotes com os endereços MAC de destino correto — ou seja, o endereço MAC do host que possui esse endereço IP ou VM. 
  
Alguns hardwares incorporado não implementam os protocolos de resolução de endereço corretamente e também podem não explicitamente resolver um endereço IP para um endereço MAC usando ARP ou NDP.  Por exemplo, um controlador de rede (SAN) de área de armazenamento pode executar dessa maneira. Dispositivos não conformes copiem o endereço MAC de origem de um pacote recebido e, em seguida, usá-lo como o endereço MAC de destino em que os pacotes de saída correspondentes, resultando em pacotes que estão sendo enviados para o endereço MAC de destino incorreto. Por isso, os pacotes são descartados pelo comutador Virtual Hyper-V porque elas não corresponderem a qualquer destino conhecido.  
  
Se você estiver tendo problemas ao se conectar a controladores de SAN ou outro incorporado a hardware, você deve obter capturas de pacotes para determinar se o hardware corretamente está implementando ARP ou NDP e entre em contato com seu fornecedor de hardware para suporte.  

  
## <a name="physical-switch-security-features"></a>Recursos de segurança de comutador físico  
Dependendo da configuração, o agrupamento NIC enviar pacotes do mesmo endereço IP com vários endereços MAC de origem enganar os recursos de segurança no comutador físico. Por exemplo, inspeção dinâmica ARP ou proteção de origem IP, especialmente se o comutador físico não está ciente que as portas fazem parte de uma equipe, que ocorre quando você configurar o agrupamento NIC no modo de comutador independente. Inspecione os logs de comutador para determinar se os recursos de segurança do comutador estão causando problemas de conectividade. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Desabilitando e habilitando os adaptadores de rede usando o Windows PowerShell  

Uma razão comum para uma equipe NIC de falha é que a interface de equipe esteja desabilitada e em muitos casos, por engano ao executar uma sequência de comandos.  Essa sequência de comandos em particular não permite que todos os NetAdapters desabilitada porque a desabilitação de todos os membros físicos subjacentes de NICs remove a interface de equipe NIC. 

Nesse caso, a interface de equipe NIC não exibe em Get-NetAdapter e, por isso, **Enable-NetAdapter \\** * não permite o agrupamento NIC. O **Enable-NetAdapter \\** * comando, no entanto, habilitar NICs, o membro que recria a interface de equipe, em seguida, (após alguns instantes). A interface de equipe permanece no estado "desabilitado" até habilitado novamente, permitindo o tráfego de rede começar a fluir. 

A seguinte sequência de Windows PowerShell de comandos pode desabilitar a interface de equipe por engano:  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Tópicos relacionados  
- [Agrupamento NIC](NIC-Teaming.md): Neste tópico, nós fornecemos a você uma visão geral do agrupamento de placa de Interface de rede (NIC) no Windows Server 2016. Agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.   

- [Gerenciamento e uso do endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando você configura uma equipe NIC com a opção de modo independente e hash de endereço ou distribuição de carga dinâmico, a equipe usa que o acesso à mídia (MAC) de endereço do membro da equipe de NIC primário no tráfego de saída de controle. O membro da equipe de NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.
  


