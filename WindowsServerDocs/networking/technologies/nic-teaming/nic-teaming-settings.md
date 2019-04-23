---
title: Configurações de agrupamento NIC
description: Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 57957e88ff4c398be23355534d5cc0ad7f920bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877927"
---
# <a name="nic-teaming-settings"></a>Configurações de agrupamento NIC
Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.


  
![Propriedades de agrupamento NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modos de agrupamento 
As opções para o modo de agrupamento são **comutador independente** e **dependentes de comutador**. Inclui o modo dependentes de comutador **agrupamento estático** e **Link Aggregation Control Protocol (LACP)**. 

>[!TIP]
>Para melhor desempenho de agrupamento NIC, é recomendável que você use um modo de balanceamento de carga de distribuição dinâmica.  
  
### <a name="switch-independent"></a>Comutador independente
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Quando você usa o modo independente de comutador com a distribuição dinâmica, a carga do tráfego de rede é distribuída com base no hash de endereço de portas TCP conforme modificado pelo algoritmo de balanceamento de carga dinâmico. O algoritmo de balanceamento de carga dinâmico redistribui fluxos para otimizar a utilização de largura de banda de membro de equipe para que as transmissões de fluxo individual podem mover de um membro da equipe do Active Directory para outro. O algoritmo leva em conta a pequena possibilidade de que redistribuir o tráfego poderá causar fora de ordem entrega de pacotes, portanto, são necessárias etapas para minimizar essa possibilidade.  
  
### <a name="switch-dependent"></a>Dependentes de comutador  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Agrupamento dependentes de comutador requer que todos os membros da equipe estão conectados ao mesmo comutador físico ou um chassi com vários alterna que compartilha uma ID de alternar entre vários dentro do chassi.


- **Agrupamento estático.** Agrupamento estático exige que você configurar manualmente o comutador e o host para identificar quais links formam o agrupamento. Como esta é uma solução configurada estaticamente, não há nenhum protocolo adicional para ajudar o comutador e no host para identificar incorretamente conectados cabos ou outros erros que poderiam causar falhas executar no grupo. Normalmente, comutadores de classe de servidor dão suporte para esse modo.

- **Link Aggregation Control Protocol (LACP).** Ao contrário de um agrupamento estático, o modo de agrupamento LACP dinamicamente identifica links que estão conectados entre o host e o comutador. Essa conexão dinâmica permite a criação automática de uma equipe e, em teoria, mas raramente na prática, a expansão e redução de uma equipe simplesmente por transmissão ou recebimento de pacotes LACP da entidade de par. Todos os comutadores de classe de servidor dão suporte a LACP e exigem o operador de rede habilitar administrativamente LACP na porta do comutador. Quando você configura um modo de agrupamento de LACP, sempre agrupamento NIC opera em modo de Active Directory do LACP, com um temporizador curto.  Nenhuma opção está atualmente disponível para modificar o temporizador ou alterar o modo LACP.


Quando você usa os modos dependentes de comutador com a distribuição dinâmica, a carga do tráfego de rede é distribuída com base no hash de endereço TransportPorts conforme modificado pelo algoritmo de balanceamento de carga dinâmico.  O algoritmo de balanceamento de carga dinâmico redistribui fluxos para otimizar a utilização de largura de banda de membro de equipe. Transmissões individuais de fluxo podem mover de um Active Directory membro da equipe para outro como parte da distribuição dinâmica. O algoritmo leva em conta a pequena possibilidade de que redistribuir o tráfego poderá causar fora de ordem entrega de pacotes, portanto, são necessárias etapas para minimizar essa possibilidade.  
  
Como com todas as configurações dependentes de comutador, o comutador determina como distribuir o tráfego de entrada entre os membros da equipe.  Espera-se que a opção de fazer um trabalho razoável de distribuir o tráfego entre os membros da equipe, mas possui independência completa para determinar como ele faz isso.  


## <a name="load-balancing-modes"></a>Modos de balanceamento de carga  
As opções de balanceamento de carga de modo de distribuição são **Hash de endereço**, **porta Hyper-V**, e **dinâmico**.  
  
### <a name="address-hash"></a>Hash de endereço
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Use o Windows PowerShell para especificar valores para os seguintes componentes de função de hash.  
  
-   Endereços de IP de origem e de destino e portas TCP de origem e destino. Esse é o padrão quando você seleciona **Hash de endereço** como o modo de balanceamento de carga.  
  
-   Origem e destino somente por endereços IP.  
  
-   Origem e destino MAC somente por endereços.  
  
O hash de portas TCP cria a distribuição mais granular dos fluxos de tráfego, resultando em fluxos menores que podem ser movidos independentemente entre membros da equipe NIC. No entanto, você não pode usar o hash de portas TCP para o tráfego que não sejam TCP ou com base em UDP, ou em que as portas TCP e UDP ficam ocultas da pilha, como com tráfego IPsec protegido. Nesses casos, o hash usa o hash de endereço IP automaticamente ou, se o tráfego não for o tráfego IP, o hash de endereço MAC é usado.  
  
### <a name="hyper-v-port"></a>Porta do Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Como sempre, o comutador adjacente vê um endereço MAC específico em uma porta, o comutador distribui a carga de entrada (o tráfego do comutador para o host) em vários vínculos com base no endereço MAC (MAC VM) de destino. Isso é particularmente útil quando as filas de máquina Virtual (VMQs) são usadas, porque uma fila pode ser colocada na NIC específica onde se espera o tráfego chegue.  
  
No entanto, se o host tiver apenas algumas VMs, esse modo pode não ser suficientemente granular para alcançar uma distribuição bem equilibrada. Esse modo também sempre limitará uma única VM (ou seja, o tráfego da porta de um único comutador) para a largura de banda que está disponível em uma única interface. Agrupamento NIC usa a porta do comutador Virtual Hyper-V como o identificador em vez de usar o endereço MAC de origem, porque, em alguns casos, uma VM pode ser configurada com mais de um endereço MAC em uma porta de comutador.  
  
### <a name="dynamic"></a>Dinâmico
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
As cargas de saída nesse modo dinamicamente são balanceadas com base no conceito de flowlets. Assim como fala humana tem quebras naturais nos finais de palavras e frases, fluxos de TCP (fluxos de comunicação TCP) também têm quebras que ocorre naturalmente. A parte de um fluxo TCP entre duas quebras de tais é conhecida como um flowlet.  
  
Quando o algoritmo de modo dinâmico detecta que um limite de flowlet foi encontrado -, como quando uma quebra de tamanho suficiente tiver ocorrido no fluxo de TCP - o algoritmo redistribui automaticamente o fluxo para outro membro da equipe, se apropriado.  Em algumas circunstâncias o algoritmo também periodicamente pode reequilibrar fluxos que não contêm nenhum flowlets. Por isso, a afinidade entre o membro de equipe e o fluxo TCP pode alterar a qualquer momento como o algoritmo de balanceamento dinâmico funciona para balancear a carga de trabalho dos membros da equipe.  
  
Se a equipe é configurada com o comutador independente ou um dos modos dependentes de comutador, é recomendável que você use o modo de distribuição dinâmica para melhor desempenho.  
  
Há uma exceção a essa regra quando o agrupamento NIC tem apenas dois membros da equipe, é configurado no modo independente do comutador e tem o modo ativo/em espera habilitado com uma NIC ativa e o outro configurado para modo de espera. Com essa configuração de agrupamento NIC, distribuição de Hash de endereço fornece desempenho ligeiramente melhor do que dinâmico a distribuição.  


## <a name="standby-adapter-setting"></a>Configuração do adaptador em espera  
As opções para o adaptador em espera serão **nenhum (todos os adaptadores do Active Directory)** ou a seleção de um adaptador de rede específico na equipe NIC que atua como um adaptador de modo de espera. Quando você configura uma NIC como um adaptador de modo de espera, todos os outros membros da equipe não selecionadas são ativo, e nenhum tráfego de rede é enviado ou processado pelo adaptador, até que uma NIC ativa falhar. Depois que uma NIC ativa falhar, a NIC em espera se torna ativa e o tráfego de rede de processos. Quando todos os membros da equipe obterem restaurados para o serviço, o membro da equipe em espera retorna para o status em espera.  

Se você tiver uma equipe de dois NICs e você optar por configurar uma NIC como um adaptador de modo de espera, você perde a largura de banda vantagens de agregação que existem com duas NICs de Active Directory.  Não é necessário designar um adaptador de modo de espera para alcançar tolerância a falhas; tolerância a falhas está sempre presente sempre que há pelo menos dois adaptadores de rede em uma equipe NIC.
 
  
## <a name="primary-team-interface-property"></a>Propriedade de interface de equipe primária  
Para acessar a caixa de diálogo de Interface primária de equipe, você deve clicar no link que está realçado na ilustração abaixo.  
  
![Propriedade de Interface de equipe primária](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Depois de clicar no link realçado, o seguinte **Nova Interface de equipe** caixa de diálogo é aberta.  
  
![Caixa de diálogo Nova Interface de Equipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Se você estiver usando VLANs, você pode usar essa caixa de diálogo para especificar um número VLAN.  
  
Se você estiver usando VLANs, você pode especificar um nome de tNIC para o agrupamento NIC.  
  


---