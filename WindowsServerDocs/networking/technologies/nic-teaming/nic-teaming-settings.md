---
title: Configurações de agrupamento NIC
description: Neste tópico, fornecemos uma visão geral das propriedades da equipe da NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.
manager: dougkim
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 0758acd5732969d6924abfa025ca3736d34ad5e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952813"
---
# <a name="nic-teaming-settings"></a>Configurações de agrupamento NIC
Neste tópico, fornecemos uma visão geral das propriedades da equipe da NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.



![Propriedades da equipe da NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)

## <a name="teaming-modes"></a>Modos de agrupamento
As opções para o modo de agrupamento são **independentes de comutação** e **comutadores dependentes**. O modo dependente de comutador inclui **agrupamento estático** e **LACP (protocolo de controle de agregação de link)**.

>[!TIP]
>Para melhor desempenho da equipe NIC, recomendamos que você use um modo de balanceamento de carga de distribuição dinâmica.

### <a name="switch-independent"></a>Independente do Comutador

[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

Quando você usa o modo independente de comutador com distribuição dinâmica, a carga de tráfego de rede é distribuída com base no hash de endereço de portas TCP, conforme modificado pelo algoritmo de balanceamento de carga dinâmico. O algoritmo de balanceamento de carga dinâmico redistribui fluxos para otimizar a utilização de largura de banda de membros da equipe para que as transmissões de fluxo individuais possam passar de um membro da equipe ativo para outro. O algoritmo leva em conta a pequena possibilidade de que a redistribuição do tráfego possa causar a entrega de pacotes fora de ordem, por isso, ele executa etapas para minimizar essa possibilidade.

### <a name="switch-dependent"></a>Alternar dependente

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]

> [!IMPORTANT]
> Alternar agrupamento dependente requer que todos os membros da equipe estejam conectados ao mesmo comutador físico ou a uma opção de vários chassis que compartilhe uma ID de comutador entre vários chassis.


- **Agrupamento estático.** O agrupamento estático exige que você configure manualmente o comutador e o host para identificar quais links formam a equipe. Como essa é uma solução configurada estaticamente, não há nenhum protocolo adicional para ajudar o comutador e o host a identificar cabos conectados incorretamente ou outros erros que poderiam fazer com que a equipe não funcione. Normalmente, comutadores de classe de servidor dão suporte para esse modo.

- **Protocolo de controle de agregação de link (LACP).** Diferentemente do agrupamento estático, o modo de agrupamento do LACP identifica dinamicamente os links que estão conectados entre o host e o comutador. Essa conexão dinâmica permite a criação automática de uma equipe e, teoricamente, mas raramente na prática, a expansão e a redução de uma equipe simplesmente pela transmissão ou recebimento de pacotes de LACP da entidade de mesmo nível. Todas as opções de classe de servidor dão suporte a LACP e todas exigem que o operador de rede habilite o LACP administrativamente na porta do comutador. Quando você configura um modo de agrupamento do LACP, o agrupamento NIC sempre opera no modo ativo do LACP com um temporizador curto.  Nenhuma opção está disponível atualmente para modificar o temporizador ou alterar o modo LACP.


Quando você usa os modos dependente de switch com distribuição dinâmica, a carga de tráfego de rede é distribuída com base no hash de endereço TransportPorts conforme modificado pelo algoritmo de balanceamento de carga dinâmico.  O algoritmo de balanceamento de carga dinâmico redistribui fluxos para otimizar a utilização de largura de banda de membros da equipe. As transmissões de fluxo individuais podem passar de um membro da equipe ativo para outro como parte da distribuição dinâmica. O algoritmo leva em conta a pequena possibilidade de que a redistribuição do tráfego possa causar a entrega de pacotes fora de ordem, por isso, ele executa etapas para minimizar essa possibilidade.

Assim como todas as configurações dependentes do comutador, a opção determina como distribuir o tráfego de entrada entre os membros da equipe.  O switch deve fazer um trabalho razoável de distribuir o tráfego entre os membros da equipe, mas tem a independência completa para determinar como ele faz isso.


## <a name="load-balancing-modes"></a>Modos de balanceamento de carga
As opções para o modo de distribuição de balanceamento de carga são o **hash de endereço**, **a porta do Hyper-V**e o **dinâmico**.

### <a name="address-hash"></a>Hash de endereço

[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

Use o Windows PowerShell para especificar valores para os seguintes componentes de função de hash.

-   Portas TCP de origem e de destino e endereços IP de origem e de destino. Esse é o padrão quando você seleciona o **hash de endereço** como o modo de balanceamento de carga.

-   Somente endereços IP de origem e de destino.

-   Somente endereços MAC de origem e de destino.

O hash de portas TCP cria a distribuição mais granular de fluxos de tráfego, resultando em fluxos menores que podem ser movidos independentemente entre os membros da equipe NIC. No entanto, você não pode usar o hash de portas TCP para o tráfego que não é baseado em TCP ou UDP, ou onde as portas TCP e UDP estão ocultas da pilha, como com o tráfego protegido por IPsec. Nesses casos, o hash usa automaticamente o hash de endereço IP ou, se o tráfego não for um tráfego IP, o hash de endereço MAC será usado.

### <a name="hyper-v-port"></a>Porta do Hyper-V

[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

Como a opção adjacente sempre vê um endereço MAC específico em uma porta, o comutador distribui a carga de entrada (o tráfego da mudança para o host) em vários links com base no endereço MAC (Mac de VM) de destino. Isso é particularmente útil quando as filas de máquina virtual (VMQs) são usadas, porque uma fila pode ser colocada na NIC específica em que se espera que o tráfego chegue.

No entanto, se o host tiver apenas algumas VMs, esse modo poderá não ser granular o suficiente para alcançar uma distribuição bem balanceada. Esse modo também limitará sempre uma única VM (ou seja, o tráfego de uma única porta de comutador) para a largura de banda disponível em uma única interface. O agrupamento NIC usa a porta do comutador virtual Hyper-V como o identificador em vez de usar o endereço MAC de origem porque, em alguns casos, uma VM pode ser configurada com mais de um endereço MAC em uma porta de comutador.

### <a name="dynamic"></a>Dinâmico

[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

As cargas de saída nesse modo são balanceadas dinamicamente com base no conceito de flowlets. Assim como a fala humana tem quebras naturais nas extremidades de palavras e sentenças, os fluxos de TCP (fluxos de comunicação TCP) também apresentam quebras naturalmente. A parte de um fluxo de TCP entre duas dessas interrupções é chamada de flowlet.

Quando o algoritmo de modo dinâmico detecta que um limite de flowlet foi encontrado-por exemplo, quando ocorreu uma interrupção de comprimento suficiente no fluxo de TCP, o algoritmo reequilibra automaticamente o fluxo para outro membro da equipe, se apropriado.  Em algumas circunstâncias, o algoritmo também pode reequilibrar periodicamente os fluxos que não contêm nenhum flowlets. Por isso, a afinidade entre o fluxo TCP e o membro da equipe pode mudar a qualquer momento, pois o algoritmo de balanceamento dinâmico funciona para balancear a carga de trabalho dos membros da equipe.

Se a equipe estiver configurada com switch independente ou um dos modos dependentes de switch, é recomendável usar o modo de distribuição dinâmica para obter o melhor desempenho.

Há uma exceção a essa regra quando a equipe NIC tem apenas dois membros da equipe, está configurada no modo independente de comutador e tem o modo ativo/em espera habilitado, com uma NIC ativa e a outra configurada para espera. Com essa configuração de equipe de NIC, a distribuição de hash de endereço fornece um desempenho ligeiramente melhor do que a distribuição dinâmica.


## <a name="standby-adapter-setting"></a>Configuração do adaptador de espera
As opções para o adaptador em espera são **nenhuma (todos os adaptadores ativos)** ou sua seleção de um adaptador de rede específico na equipe NIC que atua como um adaptador em espera. Quando você configura uma NIC como um adaptador em espera, todos os outros membros não selecionados da equipe estão ativos e nenhum tráfego de rede é enviado ou processado pelo adaptador até que uma NIC ativa falhe. Após a falha de uma NIC ativa, a NIC em espera torna-se ativa e processa o tráfego de rede. Quando todos os membros da equipe são restaurados para o serviço, o membro da equipe em espera retorna ao status de espera.

Se você tiver uma equipe de duas NICs e optar por configurar uma NIC como um adaptador em espera, perderá as vantagens de agregação de largura de banda que existem com duas NICs ativas.  Você não precisa designar um adaptador em espera para obter tolerância a falhas; a tolerância a falhas sempre estará presente sempre que houver pelo menos dois adaptadores de rede em uma equipe NIC.


## <a name="primary-team-interface-property"></a>Propriedade da interface da equipe principal
Para acessar a caixa de diálogo principal da interface da equipe, você deve clicar no link que está realçado na ilustração abaixo.

![Propriedade da interface da equipe principal](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)

Depois que você clicar no link realçado, a caixa de diálogo **nova interface de equipe** a seguir será aberta.

![Caixa de diálogo Nova Interface de Equipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)

Se você estiver usando VLANs, poderá usar essa caixa de diálogo para especificar um número de VLAN.

Se você estiver ou não usando VLANs, poderá especificar um nome NIC para a equipe NIC.



---
