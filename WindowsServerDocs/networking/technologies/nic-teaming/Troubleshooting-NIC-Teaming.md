---
title: Solucionar problemas de agrupamento NIC
description: Este tópico fornece informações sobre como solucionar problemas de agrupamento NIC no Windows Server 2016.
manager: dougkim
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 33e287cdc13fdfe115fe1f42c9009d18bda8277a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952184"
---
# <a name="troubleshooting-nic-teaming"></a>Solucionar problemas de agrupamento NIC

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento NIC, como valores de hardware e comutador físico.  Quando as implementações de hardware de protocolos padrão não estão em conformidade com as especificações, o desempenho de agrupamento NIC pode ser afetado. Além disso, dependendo da configuração, o agrupamento NIC pode enviar pacotes do mesmo endereço IP com vários endereços MAC, liberando os recursos de segurança no comutador físico.


## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware que não está em conformidade com a especificação

Durante a operação normal, o agrupamento NIC pode enviar pacotes do mesmo endereço IP, ainda com vários endereços MAC. De acordo com os padrões de protocolo, os receptores desses pacotes devem resolver o endereço IP do host ou da VM para um endereço MAC específico, em vez de responder ao endereço MAC do pacote de recebimento.  Os clientes que implementam corretamente os protocolos de resolução de endereço (ARP e NDP) enviam pacotes com os endereços MAC de destino corretos, ou seja, o endereço MAC da VM ou host que possui esse endereço IP.

Alguns hardwares incorporados não implementam corretamente os protocolos de resolução de endereço e também podem não resolver explicitamente um endereço IP para um endereço MAC usando ARP ou NDP.  Por exemplo, um controlador SAN (rede de área de armazenamento) pode ser executado dessa maneira. Dispositivos de não conformidade copiam o endereço MAC de origem de um pacote recebido e, em seguida, usam-o como o endereço MAC de destino nos pacotes de saída correspondentes, resultando em pacotes enviados para o endereço MAC de destino errado. Por isso, os pacotes são descartados pelo comutador virtual do Hyper-V porque não correspondem a nenhum destino conhecido.

Se você estiver tendo problemas para se conectar a controladores SAN ou a outro hardware incorporado, deverá fazer capturas de pacote para determinar se o hardware está implementando o ARP ou NDP corretamente e entre em contato com seu fornecedor de hardware para obter suporte.


## <a name="physical-switch-security-features"></a>Recursos de segurança do comutador físico
Dependendo da configuração, o agrupamento NIC pode enviar pacotes do mesmo endereço IP com vários endereços MAC de origem, liberando os recursos de segurança no comutador físico. Por exemplo, inspeção ARP dinâmica ou proteção de origem IP, especialmente se o comutador físico não estiver ciente de que as portas fazem parte de uma equipe, o que ocorre quando você configura o agrupamento NIC no modo independente de comutador. Inspecione os logs de comutador para determinar se os recursos de segurança do comutador estão causando problemas de conectividade.

## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Desabilitando e habilitando adaptadores de rede usando o Windows PowerShell

Um motivo comum para uma equipe NIC falhar é que a interface da equipe está desabilitada e, em muitos casos, por acidente ao executar uma sequência de comandos.  Essa sequência específica de comandos não habilita todos os adaptadores de netdesabilitados porque desabilitar todos os membros físicos subjacentes de NICs remove a interface de equipe da NIC.

Nesse caso, a interface de equipe NIC não é mais mostrada no Get-netadapter e, por isso, **Enable-netadapter \\ *** não HABILITA a equipe NIC. O comando **Enable-netadapter \\ *** faz, no entanto, habilitar as NICs do membro, que então (após um curto período) recria a interface da equipe. A interface de equipe permanece no estado "desabilitado" até que seja habilitada novamente, permitindo que o tráfego de rede comece a fluir.

A seguinte sequência de comandos do Windows PowerShell pode desabilitar a interface de equipe por acidente:

```PowerShell
Disable-NetAdapter *
Enable-NetAdapter *
```



## <a name="related-topics"></a>Tópicos relacionados
- [Agrupamento NIC](NIC-Teaming.md): neste tópico, fornecemos uma visão geral do agrupamento NIC (placa de interface de rede) no Windows Server 2016. O agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet físicos em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.

- [Uso e gerenciamento de endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): quando você configura uma equipe NIC com o modo independente de comutador e o hash ou a distribuição de carga dinâmica, a equipe usa o endereço MAC (controle de acesso à mídia) do membro da equipe NIC primário no tráfego de saída. O membro da equipe NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): neste tópico, fornecemos uma visão geral das propriedades da equipe NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.



