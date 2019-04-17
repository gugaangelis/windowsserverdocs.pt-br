---
title: Criar uma nova equipe de placa de rede em um computador Host ou VM
description: Este tópico fornece informações sobre a configuração de agrupamento de placa de rede para que você compreende as seleções que você deve fazer quando você estiver configurando uma nova equipe de placa de rede no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 6a9866d1f4e72b3c77c3233b5e5582d250cfe6a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Criar uma nova equipe de placa de rede em um computador Host ou VM

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece informações sobre a configuração de agrupamento de placa de rede para que você compreende as seleções que você deve fazer quando você estiver configurando uma nova equipe de placa de rede. Este tópico contém as seções a seguir.  
  
-   [Escolhendo um modo de agrupamento](#bkmk_teaming)  
  
-   [Escolher uma modo de balanceamento de carga](#bkmk_lb)  
  
-   [Escolhendo uma configuração de adaptador de modo de espera](#bkmk_standby)  
  
-   [Usando a propriedade de Interface de equipe principal](#bkmk_primary)  
  
> [!NOTE]  
> Se você já entenda esses itens de configuração, você pode usar os procedimentos a seguir para configurar o agrupamento de placa de rede.  
>   
> -   [Criar uma nova equipe NIC em uma VM](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
> -   [Criar uma nova equipe NIC](../../technologies/nic-teaming/Create-a-New-NIC-Team.md)  
  
Quando você cria uma nova equipe de placa de rede, você deve configurar as seguintes propriedades de equipe de placa de rede.  
  
-   Nome do time  
  
-   Adaptadores de membro  
  
-   Modo de agrupamento  
  
-   Modo de balanceamento de carga  
  
-   Adaptador de modo de espera  
  
Você também pode configurar a interface de equipe principal e configure um virtual LAN () número.  
  
Essas propriedades de equipe de NIC são exibidas na ilustração a seguir, que contém valores de exemplo para algumas propriedades de equipe de placa de rede.  
  
![Propriedades da equipe de NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  
  
## <a name="bkmk_teaming"></a>Escolhendo um modo de agrupamento  
As opções para o modo de agrupamento são **Switch independentes**, **agrupamento estático**, e **Link agregação Control Protocol (LACP)**. Agrupamento estático e LACP são dependentes de alternar modos. Para obter melhor desempenho de equipe de NIC com todos os três modos de agrupamento, é recomendável que você use um modo de balanceamento de carga de distribuição dinâmico.  
  
**Independente de alternar**  
  
Com o modo Alternar independentes, o switch ou alterna para o qual a equipe de NIC membros estão conectados não estão cientes da presença da equipe do NIC e não determinar como distribuir o tráfego de rede para membros da equipe de NIC - em vez disso, a equipe de NIC distribui o tráfego da rede em todos os membros da equipe de placa de rede.  
  
Quando você usa o modo Switch independente com a distribuição dinâmica, a carga do tráfego de rede é distribuída com base no hash de endereço de portas TCP conforme modificado pelo algoritmo de balanceamento de carga dinâmica. O algoritmo de balanceamento de carga dinâmica redistribui fluxos para otimizar a utilização de largura de banda de membro de equipe para que transmissões de fluxo individuais podem mover de um membro da equipe ativo para outro. O algoritmo leva em consideração a possibilidade de pequena que redistribuir o tráfego pode causar fora de ordem entrega de pacotes, portanto, são necessárias etapas para minimizar essa possibilidade.  
  
**Alternar dependentes**  
  
Com os modos de alternar dependentes, a opção para que os membros da equipe de NIC estão conectados determina como distribuir o tráfego de rede de entrada entre os membros da equipe de placa de rede. A opção tem independência completa para determinar como distribuir o tráfego de rede em todos os membros da equipe de placa de rede.  
  
> [!IMPORTANT]  
> Alternar dependentes agrupamento exige que todos os membros da equipe estão conectados ao mesmo switch físico ou um chassi várias alterna que compartilha uma ID de alternar entre vários chassis.  
  
Agrupamento estático exige que você configure manualmente a opção e o host para identificar quais links da equipe do. Porque essa é uma solução configurada estaticamente, não há nenhum protocolo adicional para ajudar a opção e o host para identificar incorretamente conectado cabos ou outros erros que poderiam causar a equipe falhar executar. Esse modo geralmente é compatível com opções de classe de servidor.  
  
Ao contrário de agrupamento estático, modo LACP agrupamento dinamicamente identifica links que estão conectados entre o host e o switch. Essa conexão dinâmico permite a criação automática de uma equipe e, na teoria, mas raramente na prática, expansão e redução de uma equipe bastando a transmissão ou o recebimento de pacotes de LACP da entidade par. Todas as opções de classe de servidor dão suporte a LACP e exigem o operador de rede habilitar administrativo LACP na porta do switch. Quando você configura um modo de agrupamento de LACP, sempre NIC agrupamento opera no modo de ativo do LACP com um temporizador curto.  Nenhuma opção está disponível no momento para modificar o temporizador ou alterar o modo de LACP.  
  
Quando você usa o Switch dependentes modos com a distribuição dinâmica, a carga do tráfego de rede é distribuída com base no hash de endereço TransportPorts conforme modificado pelo algoritmo de balanceamento de carga dinâmica.  O algoritmo de balanceamento de carga dinâmico redistribui fluxos para otimizar a utilização de largura de banda de membro de equipe. Transmissões de fluxo individuais podem mover de membro de uma equipe ativa para outro como parte da distribuição do dinâmica. O algoritmo leva em consideração a possibilidade de pequena que redistribuir o tráfego pode causar fora de ordem entrega de pacotes, portanto, são necessárias etapas para minimizar essa possibilidade.  
  
Como com todas as configurações de dependentes do switch, o switch determina como distribuir o tráfego de entrada entre os membros da equipe.  O switch é esperado para fazer um trabalho razoável de distribuir o tráfego em todos os membros da equipe, mas ela tem independência completa para determinar como ele faz isso.  
  
## <a name="bkmk_lb"></a>Escolher uma modo de balanceamento de carga  
As opções para o modo de distribuição de balanceamento de carga são **Hash de endereço**, **Hyper-V porta**, e **dinâmico**.  
  
**Hash de endereço**  
  
Esse modo de balanceamento de carga cria um hash que é baseado em componentes de endereço do pacote. Em seguida, ele atribui pacotes que possuem esse valor de hash para um dos adaptadores disponíveis. Geralmente, esse mecanismo sozinho é suficiente para criar um equilíbrio razoável em todos os adaptadores disponíveis.  
  
Você pode usar o Windows PowerShell para especificar valores para os seguintes componentes de função de hash.  
  
-   Portas TCP de origem e de destino e origem e destino endereços IP. Este é o padrão quando você seleciona **Hash de endereço** como o modo de balanceamento de carga.  
  
-   Origem e destino somente endereços IP.  
  
-   Origem e destino MAC aborda apenas.  
  
O hash de portas TCP cria a distribuição mais granular de fluxos de tráfego, resultando em fluxos menores que podem ser movidos de forma independente entre membros da equipe de placa de rede. No entanto, você não pode usar o hash de portas TCP para o tráfego que não seja TCP ou com base em UDP, ou onde as portas TCP e UDP estão ocultos da pilha, como com tráfego protegido por IPsec. Nesses casos, o hash usa automaticamente o hash de endereço IP ou, se o tráfego não é IP, o hash de endereço MAC é usado.  
  
**Porta do Hyper-V**  
  
Há uma vantagem em usar o modo de porta do Hyper-V para NIC equipes que estão configurados em hosts Hyper-V. Como VMs têm independentes endereços MAC, endereço MAC da VM - ou a porta que a VM está conectada no Hyper-V Virtual Switch - pode ser a base na qual dividir o tráfego de rede entre membros da equipe de placa de rede.  
  
> [!IMPORTANT]  
> As equipes de placa de rede que você cria em VMs não pode ser configuradas com o modo de balanceamento de carga de porta do Hyper-V. Use o Hash de endereço carregar modo balanceamento em vez disso.  
  
Porque o switch adjacente sempre vê um endereço MAC específico em uma porta, a opção distribui a carga de entrada (o tráfego no botão Alternar para o host) em vários links com base no destino endereço MAC (MAC VM). Isso é particularmente útil quando filas de máquina Virtual (VMQs) são usadas, porque uma fila pode ser colocada no NIC específico onde o tráfego é esperado para chegar.  
  
No entanto, se o host tem apenas alguns VMs, esse modo talvez não seja granular para alcançar uma distribuição bem equilibrada. Esse modo também sempre limitará uma única VM (isto é, o tráfego de uma porta de switch único) à largura de banda que está disponível em uma única interface. Agrupamento de NIC usa a porta do Switch Hyper-V Virtual como o identificador em vez de usar a endereço MAC de origem porque, em alguns casos, uma VM pode ser configurada com mais de um endereço MAC em uma porta do switch.  
  
**Dinâmico**  
  
Esse modo de balanceamento de carga utiliza os melhores aspectos de cada um dos dois modos e os combina em um único modo:  
  
-   Cargas de saída são distribuídas com base em um hash dos endereços IP e portas TCP.  Modo dinâmico redistribui também é carregado em tempo real para que um determinado fluxo de saída poderão se mover para frente e para trás entre os membros da equipe.  
  
-   Cargas de entrada são distribuídas da mesma maneira como o modo de porta do Hyper-V.  
  
O saída é carregado nesse modo é balanceado dinamicamente com base em conceito de flowlets. Assim como fala humana tem quebras naturais nas extremidades de palavras e frases, fluxos TCP (fluxos de comunicação TCP) também tem que ocorre naturalmente quebras. A parte de um fluxo TCP entre dois tais quebras é conhecida como um flowlet.  
  
Quando o algoritmo de modo dinâmico detecta que um limite de flowlet foi encontrado - por exemplo, quando uma quebra de comprimento suficiente ocorreu no fluxo de TCP - o algoritmo redistribui automaticamente o fluxo para outro membro da equipe, se apropriado.  Em algumas circunstâncias o algoritmo pode também periodicamente rebalancear fluxos que não contêm qualquer flowlets. Por isso, a afinidade entre membro de equipe e de fluxo TCP pode alterar a qualquer momento como funciona o algoritmo de balanceamento dinâmico para equilibrar a carga de trabalho dos membros da equipe.  
  
Se a equipe está configurada com Switch independente ou um dos modos de comutador dependentes, é recomendável que você use o modo de distribuição dinâmico para obter melhor desempenho.  
  
Há uma exceção a essa regra quando a equipe de NIC tem apenas dois membros da equipe, é configurada no modo Switch independente e tem modo ativo/Standby habilitado, com uma placa de rede ativa e o outro configurado para modo de espera. Com essa configuração de equipe de NIC, distribuição de Hash de endereço fornece um pouco melhor desempenho que dinâmico distribuição.  
  
## <a name="bkmk_standby"></a>Escolhendo uma configuração de adaptador de modo de espera  
As opções para o adaptador de modo de espera são None (todos os adaptadores Active) ou a seleção de um adaptador de rede específica na equipe do NIC que vai atuar como um adaptador de modo de espera, enquanto outros membros da equipe não selecionado estão ativo. Quando você configura uma placa de rede como um adaptador de modo de espera, nenhum tráfego de rede é enviado a ou processado pelo adaptador, a menos e até o momento quando uma placa de rede ativa falha. Depois que uma placa de rede ativa falhar, o modo de espera NIC fica ativo e tráfego de rede de processos. Se todos os membros da equipe são restaurados para o serviço, e quando o membro da equipe de modo de espera é retornado para o status de espera.  
  
Se você tiver uma equipe de dois NIC e escolha Configurar uma NIC como um adaptador de modo de espera, você perder a largura de banda vantagens de agregação existentes com dois NICs ativas.  
  
> [!IMPORTANT]  
> Você não precisa designar um adaptador de modo de espera para alcançar tolerância; tolerância está sempre presente sempre que há pelo menos dois adaptadores de rede em uma equipe de placa de rede.  
  
## <a name="bkmk_primary"></a>Usando a propriedade de Interface de equipe principal  
Para acessar a caixa de diálogo de Interface de equipe principal, você deve clicar no link que está realçado na ilustração abaixo.  
  
![Propriedade de Interface de equipe principal](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Depois que você clicar no link realçado, as seguintes **Nova Interface de equipe** caixa de diálogo é aberta.  
  
![Caixa de diálogo Nova Interface de equipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Se você estiver usando VLANs, você pode usar essa caixa de diálogo para especificar um número.  
  
Se você estiver usando VLANs, ou não, você pode especificar um nome de tNIC para a equipe de placa de rede.  
  
## <a name="see-also"></a>Consulte também  
[Criar uma nova equipe NIC em uma VM](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
[Agrupamento de NIC](NIC-Teaming.md)  
  


