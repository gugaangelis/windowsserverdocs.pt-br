---
title: Guia de Implantação do BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9bccf69f0a913159a395fabc670a63e2c159bd91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888177"
---
# <a name="branchcache-deployment-guide"></a>Guia de Implantação do BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este guia para aprender a implantar o BranchCache no Windows Server 2016.  
  
Além deste tópico, este guia contém as seções a seguir.  
  
-   [Escolhendo um Design do BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Implantar o BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Visão geral da implantação do BranchCache

BranchCache é uma tecnologia de otimização de largura de banda de rede de (longa distância WAN) de longa distância que é incluída em algumas edições do Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 e relacionados Sistemas de operacionais de cliente do Windows.  
  
Para otimizar a largura de banda da WAN, o BranchCache copia o conteúdo dos servidores de conteúdo da matriz e o armazena nas filiais, permitindo que os computadores cliente das filiais acessem o conteúdo localmente e não pela WAN.  
  
Nas filiais, conteúdo é armazenado em cache em servidores que executam o recurso BranchCache do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 - ou, se não houver nenhum servidor disponível na filial, conteúda é cac hed em computadores cliente que executam o Windows 10&reg;, Windows&reg; 8.1, Windows 8 ou Windows 7&reg; .  
  
Depois que um computador cliente solicita e recebe o conteúdo do datacenter principal do office ou na nuvem e o conteúdo é armazenado em cache na filial, outros computadores na mesma filial podem obter o conteúdo localmente em vez de entrar em contato com o servidor de conteúdo sobre o Link WAN.  
  
**Benefícios de implantar o BranchCache**  
  
Arquivo de caches de BranchCache, web e conteúdo do aplicativo nas filiais, permitindo que os cliente computadores acessar dados usando a rede local (LAN) em vez de acessar o conteúdo em conexões WAN lentas.  
  
BranchCache reduz o tráfego da WAN e a hora em que é necessária para usuários da filial abrir arquivos na rede.  BranchCache sempre fornece aos usuários com os dados mais recentes e protege a segurança de seu conteúdo, criptografando os caches no servidor de cache hospedado e nos computadores cliente.  
  
### <a name="what-this-guide-provides"></a>O que é fornecido neste guia  
Esta guia de implantação permite implantar o BranchCache nos seguintes modos:  
  
-   Modo de cache distribuído. Nesse modo, os computadores cliente das filiais baixam conteúdo de servidores de conteúdo na nuvem ou escritório principal em, em seguida, armazenar em cache o conteúdo para outros computadores na mesma filial. O modo de cache distribuído não exige um computador servidor na filial.  
  
-   Modo de cache hospedado. Nesse modo, o branch office cliente computadores baixam o conteúdo de servidores de conteúdo na nuvem ou escritório principal e um servidor de cache hospedado recupera o conteúdo dos clientes. O servidor de cache hospedado, em seguida, armazena em cache o conteúdo para outros computadores cliente.  
  
Este guia também fornece instruções sobre como implantar os três tipos de servidores de conteúdo. Servidores de conteúdo contêm o conteúdo de origem que é baixado por computadores cliente filiais e o servidor de conteúdo de um ou mais é necessário para implantar o BranchCache em modo. Os tipos de servidores de conteúdo são:  
  
-   **Servidores de conteúdo baseado em servidor Web**. Esses servidores de conteúdo enviam conteúdo a computadores cliente BranchCache usando os protocolos HTTP e HTTPS. Esses servidores de conteúdo deve estar executando as versões do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e, após a qual o recurso BranchCache está instalado.  
  
-   **Servidores de aplicativos baseados em BITS**. Esses servidores de conteúdo enviam conteúdo a computadores de cliente do BranchCache usando o serviço de transferência inteligente em segundo plano (BITS). Esses servidores de conteúdo deve estar executando as versões do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e, após a qual o recurso BranchCache está instalado.  
  
-   **Servidores de conteúdo com base no servidor de arquivos**. Esses servidores de conteúdo deve estar executando as versões do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 que dão suporte ao BranchCache e no qual serviços de arquivo de função de servidor está instalada. Além disso, o serviço de função **BranchCache para arquivos da rede** da função de servidor de Serviços de Arquivo deve estar instalado e configurado. Esses servidores de conteúdo enviam conteúdo a computadores cliente BranchCache usando o protocolo SMB.  
  
Para obter mais informações, consulte [versões do sistema operacional para BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisitos de implantação do BranchCache

A seguir está os requisitos para implantar o BranchCache usando este guia.  
  
-   **Servidores de conteúdo do arquivo e Web** deve estar executando um dos seguintes sistemas operacionais para fornecer a funcionalidade do BranchCache: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2. Windows 8 e posteriores clientes continuam a ver os benefícios do BranchCache ao acessar servidores de conteúdo que estiver executando o Windows Server 2008 R2, no entanto, eles não conseguem fazer uso do novo agrupamento e hash de tecnologias no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
-   **Computadores cliente** deve estar executando o Windows 10, Windows 8.1 ou Windows 8 para fazer uso do modelo de implantação mais recente e o agrupamento e o hash de melhorias que foram introduzidas com o Windows Server 2012.  
  
-   **Servidores de cache hospedado** deve estar executando Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 para usar os aprimoramentos de implantação e dimensionar os recursos descritos neste documento.  Um computador que esteja executando um desses sistemas operacionais que é configurado como um servidor de cache hospedado pode continuar para servir computadores clientes que executam o Windows 7, mas para fazer isso, ele deve estar equipado com um certificado que é adequado para segurança TLS (Transport Layer ), conforme descrito no Windows Server 2008 R2 e Windows 7 [guia de implantação do BranchCache](https://technet.microsoft.com/library/ee649232.aspx).  
  
-   **Um domínio do Active Directory** é necessário para aproveitar a diretiva de grupo e a descoberta automática de cache hospedado, mas um domínio não é necessária para usar o BranchCache.  Você pode configurar computadores individuais usando o Windows PowerShell. Além disso, não é necessário que os controladores de domínio estão executando o Windows Server 2012 ou posterior para utilizar as novas configurações de diretiva de grupo do BranchCache; Você pode importar os modelos administrativos do BranchCache em controladores de domínio que executam sistemas operacionais anteriores, ou você pode criar os objetos de diretiva de grupo remotamente em outros computadores que estão executando o Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012.

-   **Sites do Active Directory** são usadas para limitar o escopo dos servidores de cache hospedado que são descobertos automaticamente.  Para descobrir automaticamente um servidor de cache hospedado, computadores cliente e servidor devem pertencer ao mesmo site. BranchCache foi projetado para ter um impacto mínimo sobre os clientes e servidores e não impõe requisitos de hardware adicionais além daquelas necessárias para executar seus respectivos sistemas operacionais.  

**Documentação e o histórico do BranchCache**

BranchCache foi introduzido pela primeira vez no Windows 7&reg; e no Windows Server&reg; 2008 R2 e foi aprimorado no Windows Server 2012, Windows 8 e sistemas operacionais posteriores.

> [!NOTE]
> Se você estiver implantando o BranchCache em sistemas operacionais diferentes do Windows Server 2016, os seguintes recursos de documentação estão disponíveis.
> 
> - Para obter informações sobre o BranchCache no Windows 8, Windows 8.1, Windows Server 2012 e Windows Server 2012 R2, consulte [visão geral do BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Para obter informações sobre o BranchCache no Windows 7 e Windows Server 2008 R2, consulte [BranchCache para Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


