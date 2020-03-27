---
title: Implantar o modo Cache Hospedado do BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1da6df19933d3a4b9866b0428fb0088ac5f862b9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319092"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implantar o modo Cache Hospedado do BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O guia de rede do Windows Server 2016 core fornece instruções para planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo Active Directory&reg; domínio em uma nova floresta.

Este guia explica como criar a rede principal fornecendo instruções para implantar o BranchCache no modo de cache hospedado em uma ou mais filiais com um controlador de domínio de leitura\-somente em que os computadores cliente estão executando o Windows&reg; 10, Windows 8.1 ou Windows 8 e ingressaram no domínio.

>[!IMPORTANT]
>Não use este guia se você estiver planejando implantar ou já tiver implantado um servidor de cache hospedado do BranchCache que esteja executando o Windows Server 2008 R2. Este guia fornece instruções para implantar o modo de cache hospedado com um servidor de cache hospedado que está executando o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.

Este guia contém as seções a seguir.

- [Pré-requisitos para usar este guia](#bkmk_pre)

- [Sobre este guia](#bkmk_about)

- [O que este guia não contém](#bkmk_not)

- [Visões gerais de tecnologia](#bkmk_tech)

- [Visão geral da implantação do modo de cache hospedado do BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Planejamento de implantação do modo de cache hospedado do BranchCache](3-Bc-Hcm-Plan.md)

- [Implantação do modo de cache hospedado do BranchCache](4-Bc-Hcm-Deployment.md)

- [Recursos adicionais](11-Bc-Hcm-additional-resources.md)

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>Pré-requisitos para usar este guia

Este é um guia complementar para o guia de rede do Windows Server 2016 Core. Para implantar o BranchCache no modo de cache hospedado com este guia, você deve primeiro fazer o seguinte.

- Implantar uma rede principal em seu escritório principal usando o Guia da rede principal, ou já ter as tecnologias fornecidas no Guia da rede principal instaladas e funcionando corretamente em sua rede. Essas tecnologias incluem TCP\/IP V4, DHCP, Active Directory Domain Services \(AD DS\)e DNS.

    > [!NOTE]
    > O [Guia de rede](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) do windows Server 2016 Core está disponível na biblioteca técnica do windows Server 2016.  

- Implante servidores de conteúdo do BranchCache que estejam executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 em seu escritório principal ou em uma nuvem data center. Para obter informações sobre como implantar servidores de conteúdo do BranchCache, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).

- Estabeleça uma rede de longa distância \(conexões WAN\) entre sua filial, seu escritório principal e, se apropriado, seus recursos de nuvem, usando uma rede virtual privada \(VPN\), o DirectAccess ou outro método de conexão.

- Implante computadores cliente em sua filial que executam um dos seguintes sistemas operacionais, que fornecem ao BranchCache suporte para Serviço de Transferência Inteligente em Segundo Plano (BITS), protocolo HTTP e SMB (Server Message Block). .
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8,1 Enterprise
    - Windows 8 Enterprise

> [!NOTE]
> Nos sistemas operacionais a seguir, o BranchCache não dá suporte à funcionalidade HTTP e SMB, mas dá suporte à funcionalidade de BITS do BranchCache.
>     - Windows 10 pro, somente suporte a BITS
>     - Windows 8.1 Pro, somente suporte a BITS
>     - Windows 8 Pro, somente suporte a BITS

## <a name="about-this-guide"></a><a name="bkmk_about"></a>Sobre este guia

Este guia foi projetado para administradores de rede e de sistema que seguiram as instruções no guia de rede do Windows Server 2016 core ou no guia de rede do Windows Server 2012 Core para implantar uma rede principal ou para aqueles que implantaram anteriormente as tecnologias incluídas no guia de rede principal, incluindo Active Directory Domain Services \(AD DS\), o serviço de nome de domínio \(o DNS\), o protocolo de configuração de host\(\)\/

É recomendável que você examine os guias de design e implantação de cada uma das tecnologias usadas neste cenário de implantação. Esses guias podem ajudar a determinar se o cenário de implantação fornece os serviços e as configurações que você precisa para a rede de sua organização.

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>O que este guia não fornece

Este guia não fornece informações conceituais sobre o BranchCache, incluindo informações sobre recursos e modos do BranchCache.  

Este guia não fornece informações sobre como implantar conexões WAN ou outras tecnologias de filial, como DHCP, um RODC ou um servidor VPN.

Além disso, este guia não fornece orientação sobre o hardware que você deve usar ao implantar um servidor de cache hospedado. É possível executar outros serviços e aplicativos no seu servidor de cache hospedado. No entanto, você deve determinar, com base na carga de trabalho, nos recursos de hardware e no tamanho da filial, se quer instalar o servidor de cache hospedado do BranchCache em determinado computador e quanto espaço em disco alocar para o cache.  
Este guia não fornece instruções para configurar computadores que executam o Windows 7. Se você tiver computadores cliente que executam o Windows 7 em suas filiais, você deve configurá-los usando procedimentos diferentes daqueles fornecidos neste guia para computadores cliente que executam o Windows 10, Windows 8.1 e Windows 8.
  
Além disso, se você tiver computadores que executam o Windows 7, deverá configurar o servidor de cache hospedado com um certificado de servidor emitido por uma autoridade de certificação que os computadores cliente confiam. \(se todos os seus computadores cliente estiverem executando o Windows 10, Windows 8.1 ou Windows 8, você não precisará configurar o servidor de cache hospedado com um certificado de servidor.\) 
> [!IMPORTANT]
> Se os servidores de cache hospedados estiverem executando o Windows Server 2008 R2, use o [Guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) do windows Server 2008 R2, em vez deste guia, para implantar o BranchCache no modo de cache hospedado. Aplique as configurações de Política de Grupo que são descritas nesse guia para todos os clientes do BranchCache que estão executando versões do Windows do Windows 7 para o Windows 10. Os computadores que executam o Windows Server 2008 R2 não podem ser configurados usando as etapas deste guia.

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>Visões gerais de tecnologia

Para este guia complementar, o BranchCache é a única tecnologia que você precisa instalar e configurar. Você deve executar comandos de BranchCache do Windows PowerShell nos servidores de conteúdo, como servidores Web e de arquivos, porém você não precisa alterar nem reconfigurar os servidores de conteúdo de qualquer outra forma. Além disso, você deve configurar computadores cliente usando Política de Grupo em seus controladores de domínio que estão executando o AD DS no Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

### <a name="branchcache"></a>BranchCache

O BranchCache é uma tecnologia de otimização de largura de banda de WAN (rede de longa distância) incluída em algumas edições dos sistemas operacionais Windows Server 2016 e Windows 10, bem como em algumas edições do Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 e Windows 7.

Para otimizar a largura de banda WAN quando os usuários acessam o conteúdo em servidores remotos, o BranchCache baixa o conteúdo solicitado pelo cliente do seu escritório principal ou de seus servidores de conteúdo de nuvem hospedado e armazena em cache o conteúdo em locais de filiais, permitindo que outros computadores cliente em filiais para acessar o mesmo conteúdo localmente, em vez de via WAN.

Quando você implanta o BranchCache no modo de cache hospedado, deve configurar computadores cliente na filial como clientes do modo de cache hospedado e, em seguida, implantar um servidor de cache hospedado na filial. Este guia demonstra como implantar o servidor de cache hospedado com conteúdo de hash e pré-carregado do seu servidor de arquivos e da Web\-servidores de conteúdo baseados em.

### <a name="group-policy"></a>Diretiva de grupos

Política de Grupo no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012 é uma infraestrutura usada para fornecer e aplicar uma ou mais configurações ou definições de política desejadas a um conjunto de usuários e computadores de destino em um ambiente de Active Directory. 

Essa infraestrutura consiste em um mecanismo de Política de Grupo e várias extensões do lado do cliente do\-\(CSEs\) que são responsáveis pela leitura das configurações de política nos computadores cliente de destino.

A Política de Grupo é usada neste cenário para configurar computadores cliente membros do domínio com modo de cache hospedado do BranchCache.

Para continuar com este guia, consulte [visão geral da implantação do modo de cache hospedado do BranchCache](2-Bc-Hcm-Deploy-Overview.md).
