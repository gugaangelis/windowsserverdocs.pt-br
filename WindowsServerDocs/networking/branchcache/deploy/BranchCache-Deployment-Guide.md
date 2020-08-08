---
title: Guia de Implantação do BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2868bfcca87f44ee9c29aa4c36de3486c660ee62
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989176"
---
# <a name="branchcache-deployment-guide"></a>Guia de Implantação do BranchCache

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este guia para aprender a implantar o BranchCache no Windows Server 2016.

Além deste tópico, este guia contém as seções a seguir.

-   [Escolhendo um design do BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)

-   [Implantar o BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)

## <a name="branchcache-deployment-overview"></a>Visão geral da implantação do BranchCache

O BranchCache é uma tecnologia de otimização de largura de banda de WAN (rede de longa distância) incluída em algumas edições do Windows Server 2016, do Windows Server &reg; 2012 R2, do Windows server &reg; 2012, do windows Server &reg; 2008 R2 e dos sistemas operacionais Windows Client relacionados.

Para otimizar a largura de banda da WAN, o BranchCache copia o conteúdo dos servidores de conteúdo da matriz e o armazena nas filiais, permitindo que os computadores cliente das filiais acessem o conteúdo localmente e não pela WAN.

Em filiais, o conteúdo é armazenado em cache em servidores que executam o recurso BranchCache do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 ou, se não houver servidores disponíveis na filial, o conteúdo é armazenado em cache em computadores cliente que executam o Windows 10 &reg; , windows &reg; 8,1, Windows 8 ou Windows 7 &reg; .

Depois que um computador cliente solicita e recebe o conteúdo do escritório principal ou do datacenter de nuvem e o conteúdo é armazenado em cache na filial, outros computadores na mesma filial podem obter o conteúdo localmente, em vez de entrar em contato com o servidor de conteúdo pelo link WAN.

**Benefícios da implantação do BranchCache**

O BranchCache armazena em cache o conteúdo de arquivos, da Web e de aplicativos em locais de filiais, permitindo que os computadores cliente acessem dados usando a LAN (rede local) em vez de acessar o conteúdo em conexões WAN lentas.

O BranchCache reduz o tráfego de WAN e o tempo necessário para que os usuários da filial abram arquivos na rede.  O BranchCache sempre fornece aos usuários os dados mais recentes e protege a segurança do seu conteúdo criptografando os caches no servidor de cache hospedado e em computadores cliente.

### <a name="what-this-guide-provides"></a>O que é fornecido neste guia
Esta guia de implantação permite implantar o BranchCache nos seguintes modos:

-   Modo de cache distribuído. Nesse modo, os computadores cliente da filial baixam o conteúdo dos servidores de conteúdo no escritório principal ou na nuvem e, em seguida, armazenamos em cache o conteúdo para outros computadores na mesma filial. O modo de cache distribuído não exige um computador servidor na filial.

-   Modo de cache hospedado. Nesse modo, os computadores cliente do Branch Office baixam o conteúdo dos servidores de conteúdo no escritório principal ou na nuvem, e um servidor de cache hospedado recupera o conteúdo dos clientes. Em seguida, o servidor de cache hospedado armazena em cache o conteúdo para outros computadores cliente.

Este guia também fornece instruções sobre como implantar três tipos de servidores de conteúdo. Os servidores de conteúdo contêm o conteúdo de origem que é baixado pelos computadores cliente da filial e um ou mais servidores de conteúdo são necessários para implantar o BranchCache em qualquer modo. Os tipos de servidores de conteúdo são:

-   **Servidores de conteúdo baseados em servidor Web**. Esses servidores de conteúdo enviam conteúdo a computadores cliente BranchCache usando os protocolos HTTP e HTTPS. Esses servidores de conteúdo devem estar executando as versões Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e no qual o recurso BranchCache está instalado.

-   **Servidores de aplicativos baseados em bits**. Esses servidores de conteúdo enviam conteúdo para computadores cliente do BranchCache usando o Serviço de Transferência Inteligente em Segundo Plano (BITS). Esses servidores de conteúdo devem estar executando as versões Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e no qual o recurso BranchCache está instalado.

-   **Servidores de conteúdo baseados em servidor de arquivos**. Esses servidores de conteúdo devem estar executando as versões Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e no qual a função de servidor serviços de arquivo está instalada. Além disso, o serviço de função **BranchCache para arquivos da rede** da função de servidor de Serviços de Arquivo deve estar instalado e configurado. Esses servidores de conteúdo enviam conteúdo a computadores cliente BranchCache usando o protocolo SMB.

Para obter mais informações, consulte [versões do sistema operacional para BranchCache](../branchcache.md#bkmk_os).

### <a name="branchcache-deployment-requirements"></a>Requisitos de implantação do BranchCache

A seguir estão os requisitos para implantar o BranchCache usando este guia.

-   Os **servidores de conteúdo da Web e de arquivo** devem estar executando um dos seguintes sistemas operacionais para fornecer a funcionalidade do BranchCache: windows Server 2016, windows Server 2012 R2, windows Server 2012 ou windows Server 2008 R2. Os clientes do Windows 8 e posteriores continuam a ver os benefícios do BranchCache ao acessar servidores de conteúdo que executam o Windows Server 2008 R2, no entanto, eles não podem usar as novas tecnologias de agrupamento e de hash no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012.

-   Os **computadores cliente** devem estar executando o Windows 10, Windows 8.1 ou o Windows 8 para usar o modelo de implantação mais recente e os aprimoramentos de agrupamento e de hash que foram introduzidos com o Windows Server 2012.

-   Os **servidores de cache hospedados** devem estar executando o windows Server 2016, o windows Server 2012 R2 ou o windows Server 2012 para fazer uso dos recursos de escala e aprimoramentos de implantação descritos neste documento.  Um computador que esteja executando um desses sistemas operacionais configurado como um servidor de cache hospedado pode continuar a servir computadores cliente que executam o Windows 7, mas, para fazer isso, ele deve estar equipado com um certificado adequado para TLS (Transport Layer Security), conforme descrito no [Guia de implantação](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649232(v=ws.10))do windows Server 2008 R2 e Windows 7 BranchCache.

-   **Um domínio Active Directory** é necessário para aproveitar o política de grupo e a descoberta automática de cache hospedado, mas um domínio não é necessário para usar o BranchCache.  Você pode configurar computadores individuais usando o Windows PowerShell. Além disso, não é necessário que os controladores de domínio estejam executando o Windows Server 2012 ou posterior para utilizar novas configurações de Política de Grupo do BranchCache; Você pode importar os modelos administrativos do BranchCache para controladores de domínio que executam sistemas operacionais anteriores ou pode criar objetos de diretiva de grupo remotamente em outros computadores que estejam executando o Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012.

-   **Active Directory sites** são usados para limitar o escopo de servidores de cache hospedados que são descobertos automaticamente.  Para descobrir automaticamente um servidor de cache hospedado, os computadores cliente e servidor devem pertencer ao mesmo site. O BranchCache foi projetado para ter um impacto mínimo sobre clientes e servidores e não impõe requisitos de hardware adicionais além daqueles necessários para executar seus respectivos sistemas operacionais.

**Histórico e documentação do BranchCache**

O BranchCache foi introduzido pela primeira vez no Windows 7 &reg; e no Windows Server &reg; 2008 R2 e foi aprimorado no windows Server 2012, Windows 8 e sistemas operacionais posteriores.

> [!NOTE]
> Se você estiver implantando o BranchCache em sistemas operacionais diferentes do Windows Server 2016, os recursos de documentação a seguir estarão disponíveis.
>
> - Para obter informações sobre o BranchCache no Windows 8, Windows 8.1, Windows Server 2012 e Windows Server 2012 R2, consulte [visão geral do BranchCache](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831696(v=ws.11)).
> - Para obter informações sobre o BranchCache no Windows 7 e no Windows Server 2008 R2, consulte [BranchCache para Windows server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd996634(v=ws.10)).