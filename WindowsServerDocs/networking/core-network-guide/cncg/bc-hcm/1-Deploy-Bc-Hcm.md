---
title: Implantar o modo de Cache hospedado BranchCache
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implantar o modo de Cache hospedado BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Guia de rede do Windows Server 2016 Core fornece instruções para planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo Active Directory&reg; domínio em uma nova floresta.

Este guia explica como criar na rede principal, fornecendo instruções para implantar BranchCache no modo de cache hospedado em filiais um ou mais com um controlador de domínio Read\-Only onde os computadores cliente estiverem executando Windows&reg; 10, Windows 8.1 ou Windows 8 e são ingressou no domínio.

>[!IMPORTANT]
>Não use este guia se você planeja implantar ou já tenha implantado um servidor de cache BranchCache hospedado que está executando o Windows Server 2008 R2. Este guia fornece instruções para implantar o modo de cache hospedado com um servidor de cache hospedado que está executando o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.

Este guia contém as seguintes seções.

- [Pré-requisitos para usar este guia](#bkmk_pre)

- [Sobre este guia](#bkmk_about)

- [O que este guia não fornece](#bkmk_not)

- [Visões gerais de tecnologia](#bkmk_tech)

- [BranchCache hospedado Cache modo visão geral da implantação](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache hospedado Cache modo planejamento de implantação](3-Bc-Hcm-Plan.md)

- [BranchCache hospedado implantação do modo de Cache](4-Bc-Hcm-Deployment.md)

- [Recursos adicionais](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Pré-requisitos para usar este guia

Este é um guia complementar ao guia de rede do Windows Server 2016 Core. Para implantar BranchCache no modo de cache hospedado com este guia, você deve primeiro fazer o seguinte.

- Implantar uma rede principal no seu escritório principal usando a guia da rede principal ou já tem as tecnologias fornecido no guia de rede principal instalados e funcionando corretamente em sua rede. Essas tecnologias incluem TCP\/IP v4, DHCP, serviços de domínio do Active Directory \(AD DS\) e DNS.

    > [!NOTE]
    > O Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponível na biblioteca técnica do Windows Server 2016.  

- Implante BranchCache servidores de conteúdo que executam o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016 no seu escritório principal ou em um centro de dados na nuvem. Para obter informações sobre como implantar BranchCache servidores de conteúdo, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).

- Estabelecer conexões de \(WAN\) de rede de longa distância entre sua filial seu escritório principal e, se apropriado, seus recursos de nuvem, usando uma privada virtual de rede \(VPN\), o DirectAccess ou outro método de conexão.

- Implante computadores cliente em sua filial que estão executando um dos seguintes sistemas operacionais, que fornecem BranchCache com suporte para o serviço de transferência inteligente em segundo plano (BITS), Hyper texto HTTP (Transfer Protocol) e bloco de mensagem de servidor (SMB).
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

>[!NOTE]
>Os seguintes sistemas operacionais, BranchCache não oferece suporte a funcionalidade HTTP e SMB, mas oferece suporte a funcionalidade de BITS BranchCache.
>     - Windows 10 Pro, BITS suportam somente
>     - Windows 8.1 Pro, BITS suportam somente
>     - Windows 8 Pro, BITS suportam somente

## <a name="bkmk_about"></a>Sobre este guia

Este guia foi projetado para os administradores de sistema e de rede que tem seguido as instruções no Windows Server 2016 guia da rede principal ou Windows Server 2012 guia da rede principal para implantar uma rede principal ou para aqueles que já implantaram as tecnologias incluídas no guia de rede principal, incluindo \(AD DS\) serviços de domínio do Active Directory Domain Name Service \(DNS\), Dynamic Host Configuration Protocol \(DHCP\) e v4 TCP\/IP.

É recomendável que você consulte os guias de design e implantação para cada uma das tecnologias que são usadas neste cenário de implantação. Estes guias podem ajudá-lo a determinar se esse cenário de implantação fornece os serviços e configuração que você precisa para rede da sua organização.

## <a name="bkmk_not"></a>O que este guia não fornece

Este guia não fornece informações conceituais sobre BranchCache, incluindo informações sobre modos de BranchCache e recursos.  

Este guia não fornece informações sobre como implantar conexões WAN ou outras tecnologias em sua filial, como DHCP, um RODC ou um servidor VPN.

Além disso, este guia não fornece orientações sobre o hardware que você deve usar quando você implanta um servidor de cache hospedado. É possível executar outros serviços e aplicativos em seu servidor de cache hospedado, no entanto, você deve fazer a determinação, com base na carga de trabalho, recursos de hardware e branch office tamanho, se é necessário instalar o servidor de cache BranchCache hospedado em um computador específico e quanto espaço alocar para o cache.  
Este guia não fornece instruções para configurar computadores que executam o Windows 7. Se você tiver computadores cliente que executam o Windows 7 em suas filiais, você deve configurá-los usando os procedimentos que são diferentes daquelas fornecidas neste guia para computadores cliente que executam o Windows 10, Windows 8.1 e Windows 8.
  
Além disso, se você tiver computadores que executam o Windows 7, você deve configurar seu servidor de cache hospedado com um certificado de servidor é emitido por uma autoridade de certificação confiam de computadores cliente. \ (Se todos os computadores cliente estiver executando o Windows 10, Windows 8.1 ou Windows 8, você não precisa configurar o servidor de cache hospedado com um certificado de servidor. \) 
> [!IMPORTANT]
> Se os servidores de cache hospedado estiver executando o Windows Server 2008 R2, use o Windows Server 2008 R2 [guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) em vez de neste guia para implantar BranchCache no modo de cache hospedado. Aplique as configurações de política de grupo são descritas nesse guia para todos os clientes BranchCache que estão executando as versões do Windows do Windows 7 para o Windows 10. Computadores que executam o Windows Server 2008 R2 não podem ser configurados usando as etapas neste guia.

## <a name="bkmk_tech"></a>Visões gerais de tecnologia

Para este guia complementar, BranchCache é a única tecnologia que você precisa instalar e configurar. Você deve executar comandos do Windows PowerShell BranchCache em seus servidores de conteúdo, como na Web e servidores de arquivos, porém você não precisa alterar ou reconfigurar servidores de conteúdo de qualquer outra forma. Além disso, você deve configurar computadores cliente usando a política de grupo nos controladores de domínio que executam o AD DS no Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016.

### <a name="branchcache"></a>BranchCache

BranchCache é uma tecnologia de otimização de largura de banda (WAN) de rede de longa distância que está incluída em algumas edições dos sistemas operacionais Windows Server 2016 e o Windows 10, bem como em algumas edições do Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7.

Para otimizar a largura de banda WAN quando os usuários acessam conteúdo em servidores remotos, BranchCache baixa o conteúdo solicitado pelo cliente de seu escritório principal ou hospedado servidores de conteúdo na nuvem e armazena em cache o conteúdo em filiais, permitindo que outros computadores cliente em filiais para acessar o mesmo conteúdo localmente, em vez de na WAN.

Quando você implanta BranchCache no modo de cache hospedado, você deve configurar computadores cliente na filial como clientes de modo de cache hospedado e, em seguida, você deve implantar um servidor de cache hospedado na filial. Este guia demonstra como implantar seu servidor de cache hospedado com conteúdo prehashed e pré-carregados da Web e servidores de conteúdo baseado em servidor \ do arquivo.

### <a name="group-policy"></a>Política de grupo

Política de grupo no Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 é uma infraestrutura usada para entregar e aplicar um ou mais configurações desejadas ou configurações de política a um conjunto de destino de usuários e computadores em um ambiente do Active Directory. 

Essa infraestrutura consiste em um mecanismo de política de grupo e vários \(CSEs\) client\ lado extensões que são responsáveis por ler as configurações de política em computadores cliente de destino.

Política de grupo é usada neste cenário para configurar computadores cliente membros do domínio com o modo de cache BranchCache hospedado.

Para continuar com este guia, consulte [BranchCache hospedado Cache modo visão geral da implantação](2-Bc-Hcm-Deploy-Overview.md).
