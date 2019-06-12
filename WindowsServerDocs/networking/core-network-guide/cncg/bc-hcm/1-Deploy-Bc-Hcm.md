---
title: Implantar o modo Cache Hospedado do BranchCache
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 54991b343623b934118bb62af1bd91871a726996
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446487"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implantar o modo Cache Hospedado do BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Guia de rede do Windows Server 2016 Core fornece instruções para planejar e implantar os componentes principais necessários para uma rede totalmente operacional e um novo Active Directory&reg; domínio em uma nova floresta.

Este guia explica como criar a rede principal, fornecendo instruções para implantar o BranchCache no modo de cache hospedado em uma ou mais filiais com uma leitura\-somente controlador de domínio em que os computadores cliente estão executando Windows&reg; 10, Windows 8.1 ou Windows 8 e estão associados ao domínio.

>[!IMPORTANT]
>Não use este guia se você estiver planejando implantar ou já implantou um servidor de cache hospedado do BranchCache que esteja executando o Windows Server 2008 R2. Este guia fornece instruções para implantar o modo de cache hospedado com um servidor de cache hospedado que está executando o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.

Este guia contém as seções a seguir.

- [Pré-requisitos para usar este guia](#bkmk_pre)

- [Sobre este guia](#bkmk_about)

- [O que este guia não contém](#bkmk_not)

- [Visões gerais de tecnologia](#bkmk_tech)

- [Visão geral da implantação de modo de Cache de hospedado BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Modo de Cache hospedado do BranchCache planejamento da implantação](3-Bc-Hcm-Plan.md)

- [Implantação do modo de Cache de hospedado BranchCache](4-Bc-Hcm-Deployment.md)

- [Recursos adicionais](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Pré-requisitos para usar este guia

Isso é um guia complementar para o guia de rede do Windows Server 2016 Core. Para implantar o BranchCache no modo de cache hospedado com este guia, você deve primeiro fazer o seguinte.

- Implantar uma rede principal em seu escritório principal usando o Guia da rede principal, ou já ter as tecnologias fornecidas no Guia da rede principal instaladas e funcionando corretamente em sua rede. Essas tecnologias incluem o TCP\/IP v4, DHCP, Active Directory Domain Services \(AD DS\)e o DNS.

    > [!NOTE]
    > O Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponível na biblioteca técnica do Windows Server 2016.  

- Implante servidores de conteúdo do BranchCache que estão executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 em seu escritório principal ou em um data center na nuvem. Para obter informações sobre como implantar servidores de conteúdo do BranchCache, consulte [recursos adicionais](11-Bc-Hcm-additional-resources.md).

- Estabelecer a rede de longa distância \(WAN\) conexões entre o escritório da filial, seu escritório principal e, se apropriado, seus recursos de nuvem, usando uma rede virtual privada \(VPN\), DirectAccess, ou outros método de conexão.

- Implantar computadores cliente da filial que estão executando um dos seguintes sistemas operacionais, que fornecem o BranchCache com suporte para o serviço de transferência inteligente em segundo plano (BITS), o Hyper texto Transfer Protocol (HTTP) e o bloco de mensagens de servidor (SMB) .
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

> [!NOTE]
> Nos seguintes sistemas operacionais, o BranchCache não oferece suporte a funcionalidades HTTP e o SMB, mas oferece suporte a funcionalidade BranchCache BITS.
>     - Windows 10 Pro, BITS suportam apenas
>     - Windows 8.1 Pro, BITS suportam apenas
>     - Windows 8 Pro, BITS suportam apenas

## <a name="bkmk_about"></a>Sobre este guia

Este guia foi projetado para administradores de rede e do sistema que seguiram as instruções no guia de rede do Windows Server 2016 Core ou o guia de rede do Windows Server 2012 Core para implantar uma rede principal, ou para aqueles que já tiver implantado anteriormente o tecnologias incluídas no guia da rede principal, incluindo o Active Directory Domain Services \(AD DS\), o serviço de nomes de domínio \(DNS\), Dynamic Host Configuration Protocol \(DHCP\)e TCP\/IP v4.

É recomendável que você examine os guias de design e implantação de cada uma das tecnologias usadas neste cenário de implantação. Esses guias podem ajudar a determinar se o cenário de implantação fornece os serviços e as configurações que você precisa para a rede de sua organização.

## <a name="bkmk_not"></a>O que este guia não fornece

Este guia não fornece informações conceituais sobre o BranchCache, incluindo informações sobre recursos e modos do BranchCache.  

Este guia não fornece informações sobre como implantar conexões WAN ou outras tecnologias de filial, como DHCP, um RODC ou um servidor VPN.

Além disso, este guia não fornece orientação sobre o hardware que você deve usar ao implantar um servidor de cache hospedado. É possível executar outros serviços e aplicativos no seu servidor de cache hospedado. No entanto, você deve determinar, com base na carga de trabalho, nos recursos de hardware e no tamanho da filial, se quer instalar o servidor de cache hospedado do BranchCache em determinado computador e quanto espaço em disco alocar para o cache.  
Este guia não fornece instruções para configurar computadores que executam o Windows 7. Se você tiver computadores cliente que executam o Windows 7 em suas filiais, você deve configurá-los usando os procedimentos são diferentes daquelas fornecidas neste guia para computadores cliente que estão executando o Windows 10, Windows 8.1 e Windows 8.
  
Além disso, se você tiver computadores que executam o Windows 7, você deve configurar seu servidor de cache hospedado com um certificado de servidor emitido por uma autoridade de certificação confiável de computadores cliente. \(Se todos os computadores cliente estão executando o Windows 10, Windows 8.1 ou Windows 8, você não precisará configurar o servidor de cache hospedado com um certificado de servidor.\) 
> [!IMPORTANT]
> Se seus servidores de cache hospedado estiver executando o Windows Server 2008 R2, use o Windows Server 2008 R2 [guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) em vez deste guia para implantar o BranchCache no modo de cache hospedado. Aplique as configurações de diretiva de grupo são descritas no guia para todos os clientes do BranchCache que estão executando versões do Windows do Windows 7 para o Windows 10. Computadores que executam o Windows Server 2008 R2 não podem ser configurados usando as etapas neste guia.

## <a name="bkmk_tech"></a>Visões gerais de tecnologia

Para este guia complementar, o BranchCache é a única tecnologia que você precisa instalar e configurar. Você deve executar comandos de BranchCache do Windows PowerShell nos servidores de conteúdo, como servidores Web e de arquivos, porém você não precisa alterar nem reconfigurar os servidores de conteúdo de qualquer outra forma. Além disso, você deve configurar computadores cliente usando a diretiva de grupo nos controladores de domínio que estão executando o AD DS no Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache é uma tecnologia de otimização de largura de banda (WAN) de rede de longa distância que é incluída em algumas edições dos sistemas operacionais Windows 10 e Windows Server 2016, bem como em algumas edições do Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 e Windows 7.

Para otimizar a largura de banda WAN quando os usuários acessam conteúdo em servidores remotos, BranchCache baixa o conteúdo solicitado pelo cliente de seu escritório principal ou servidores de conteúdo de nuvem hospedado e armazena nas filiais, permitindo que os outros computadores cliente filiais para acessar o mesmo conteúdo localmente em vez de pela WAN.

Quando você implanta o BranchCache no modo de cache hospedado, deve configurar computadores cliente na filial como clientes do modo de cache hospedado e, em seguida, implantar um servidor de cache hospedado na filial. Este guia demonstra como implantar o servidor de cache hospedado com conteúdo de hash e pré-carregados de seu servidor Web e o arquivo\-com base em servidores de conteúdo.

### <a name="group-policy"></a>Política de Grupo

Política de grupo no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 é uma infraestrutura usada para fornecer e aplicar uma ou mais configurações desejadas ou configurações de política a um conjunto de usuários de destino e computadores em um ambiente do Active Directory. 

Essa infraestrutura consiste em um mecanismo de diretiva de grupo e o cliente vários\-extensões do lado \(CSEs\) responsáveis pela leitura de configurações de política nos computadores cliente de destino.

A Política de Grupo é usada neste cenário para configurar computadores cliente membros do domínio com modo de cache hospedado do BranchCache.

Para continuar com este guia, consulte [BranchCache Hosted Cache de modo de visão geral da implantação](2-Bc-Hcm-Deploy-Overview.md).
