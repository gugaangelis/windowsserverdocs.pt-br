---
title: "Integração de rede Virtual Azure"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Integração de rede Virtual Azure

>Aplica-se a: Windows Server 2016 Essentials

Conforme as organizações faz sua maneira de computação em nuvem, raramente serão eles mover todos os seus recursos 100% de uma só vez, mas em vez disso, tirar uma abordagem em que alguns recursos não são na nuvem e alguns ainda estão no local. Essa abordagem híbrida fica mais fácil para as organizações não apenas mover alguns recursos de computação para a nuvem, mas também permite que elas aumentem sua infraestrutura de TI sem precisar adquirir o novo hardware.

Ao implementar essa abordagem híbrida a computação, uma maneira perfeita de recursos em dois locais para se comunicar uns com os outros é necessária. Redes virtuais Azure é um serviço do Azure que permite que as organizações criar uma ponto a ponto (P2P) ou -to-site (S2S) rede virtual privada que faz com que os recursos que estão em execução no Azure (como máquinas virtuais e armazenamento) aparência como se estivessem na rede local para acesso perfeito de recursos e dos aplicativos.

Configuração de uma rede Virtual do Azure pode ser complexa. Com o Windows Server Essentials 2016, você pode configurar facilmente sua rede Virtual do Azure por meio de um assistente simples que ajuda você a escolher os padrões mais apropriados para seu ambiente de rede. Conforme mostrado na captura de tela a seguir, uma nova tarefa de integração de rede Virtual do Azure foi adicionada à seção de serviços de nuvem da Microsoft do painel para introduzir redes virtuais do Azure, bem como fornecer um link rápido para iniciar a integração com o Windows Essentials.

![Uma captura de tela mostrando a guia de Introdução na Home page do painel do Windows Server Essentials. Na guia de Introdução, a seção serviços foi selecionada e o painel indica em integração de serviços de nuvem do Microsoft que a rede Virtual do Azure atualmente está desabilitada.](media/azure-virtual-network-1.PNG)

Quando você clica no **integrar agora** vincular para redes virtuais do Azure na captura de tela acima, uma caixa de diálogo será exibida solicitando que você faça logon sua conta do Microsoft Azure. Se você não tiver uma conta da Microsoft Azure, você terá a opção de inscrição para um nessa tela, o que a você será encaminhado para o portal de inscrição de conta do Azure:

![Uma captura de tela mostrando a página de entrada ao Microsoft Azure do Assistente de integração com o Azure Virtual de rede.](media/azure-virtual-network-2.PNG)

Depois de entrar no Azure, você verá a opção de escolher quais assinatura que desejam associar ao virtuais do Azure serviço de rede:

![Uma captura de tela mostrando a página minha assinatura do Microsoft Azure da integração com o Assistente de rede Virtual do Azure.](media/azure-virtual-network-3.PNG)

Depois que você escolheu assinatura do Azure que você deseja usar para redes virtuais do Azure, você verá a opção para criar uma nova rede Virtual do Azure, ou se um já foi configurado sob esta assinatura, a caixa de lista suspensa mostrará está disponível. Você também deverá escolher um nome para a rede Local que a rede Virtual do Azure usará para identificar recursos em sua rede local. Por fim, você deverá escolher qual região do Azure em que você deseja que suas redes virtuais do Azure para ser hospedado. Escolher um local que esteja fisicamente mais próxima da sua rede local é normalmente melhor para otimizar a velocidade de largura de banda para se comunicar com os recursos que você pode hospedar em seus serviços do Azure:

![Uma captura de tela mostrando a página Configurar o Azure Virtual de rede do Assistente de integração com o Azure Virtual de rede.](media/azure-virtual-network-4.PNG)

A última etapa do processo de integração é o dispositivo VPN que será usado para a conexão VPN S2S da instalação. Como a maioria das pequenas empresas tem apenas alguns servidores em seu ambiente e não têm a equipe de TI para configurar um roteador VPN para se conectar ao Microsoft Azure corretamente, a seleção padrão será configurar o servidor Windows Server Essentials como o servidor VPN que irá se conectar a recursos em sua rede local para acessar os recursos na rede Virtual do Azure. No entanto, se você preferir usar outro servidor em seu ambiente como o servidor VPN, ou você gostaria de usar um roteador VPN, você pode selecionar essas opções.

Devido a variação em tipos de roteador e modelos, Windows Server Essentials não tentar configurar automaticamente o roteador VPN. Selecionar o roteador VPN neste assistente de integração apenas notifica redes virtuais do Azure do tipo de dispositivo para configurações de roteamento apropriadas necessárias no Azure para conectividade.

Depois de concluir o Assistente de integração, uma nova guia ficará visível no painel do Windows Server Essentials para redes virtuais do Azure:

![Uma captura de tela mostrando a página VNet do Azure do painel do Windows Server Essentials. A guia Rede Virtual do Azure é selecionada e mostra o status como configurando.](media/azure-virtual-network-5.PNG)

>! Observação concluir a configuração de uma rede Virtual do Azure na nuvem pode levar algum tempo, para cima a 30 minutos. Durante esse tempo, o status de Configurando estará presente na página de status de rede Virtual do Azure do painel.

Depois que a configuração da rede Virtual do Azure for concluída, o status ficar conectado e mostrar os detalhes de sua rede Virtual do Azure como dados de entrada/saída, endereço IP do gateway, detalhes de endereço e a conta IP locais:

![Uma captura de tela mostrando a página VNet do Azure do painel do Windows Server Essentials. A guia Rede Virtual do Azure é selecionada e mostra o status como conectado e nessas informações de status os detalhes da rede virtual são exibidos.](media/azure-virtual-network-6.PNG)

No painel de tarefas no lado direito do painel são as diversas tarefas que você pode realizar com sua rede Virtual do Azure.

-   **Desconectar do Azure VNET** Configurando uma rede Virtual do Azure é gratuita, mas há um custo para o gateway VPN que se conecta ao local e outras VNETs no Azure. Desconectar-se de VNET o Azure interrompe cobrança todos.

-   **Alternar por dispositivo VPN** que você quiser mudar de um servidor de VPN para um roteador VPN, essa tarefa permitirá que você a fazer a mudança e notificar o VNET do Azure.

-   **Configurar o Azure VNET** essa tarefa permite que você altere as opções de configuração avançada de VNET o Azure redirecionando-los para a página configuração portal do Azure para VNET do Azure.

-   **Atualizar Status** atualiza a página de status, atualizar o status de conexão do VNET Azure incluindo dados in, fade out.

-   **Desabilitar a integração do Azure VNET** desconecta o VNET do Azure e remove a integração do painel do Windows Server Essentials. Observe que isso não exclui o VNET do Azure, configurações ainda são preservadas no Azure se você quiser mais tarde novamente integrar VNET do Azure com o painel.

-   **Saiba mais sobre o Azure VNET**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/).

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)
