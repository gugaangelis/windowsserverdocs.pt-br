---
title: Guia de implantação do BranchCache
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7aa35260213a8a236b7c27cf74e36692438cd2a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-deployment-guide"></a>Guia de implantação do BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este guia para aprender a implantar BranchCache no Windows Server 2016.  
  
Além de neste tópico, este guia contém as seguintes seções.  
  
-   [Escolher um Design BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Implantar BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Visão geral da implantação BranchCache

BranchCache é uma tecnologia de otimização de largura de banda WAN (rede) de longa distância que está incluída em algumas edições do Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 e sistemas de operacionais de cliente Windows relacionados.  
  
Para otimizar a largura de banda WAN, BranchCache copia o conteúdo de seus servidores de conteúdo principal do office e armazena em cache o conteúdo em filiais, permitindo que o cliente de computadores em filiais para acessar o conteúdo localmente, em vez de na WAN.  
  
Em filiais, conteúdo é armazenado em cache em servidores que executam o recurso BranchCache do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 - ou, se não houver nenhum servidores disponíveis na filial, conteúdo é armazenado em cache em computadores cliente que executam o Windows 10&reg;, Windows&reg; 8.1, Windows 8 ou Windows 7&reg; .  
  
Depois que um computador cliente solicita e recebe conteúdo de datacenter principal do escritório ou na nuvem e o conteúdo é armazenado em cache na filial, outros computadores na mesma filial podem obter o conteúdo localmente, em vez de entrar em contato com o servidor de conteúdo através da conexão WAN.  
  
**Benefícios de implantar BranchCache**  
  
BranchCache caches de arquivo, web e conteúdo do aplicativo em filiais, permite aos computadores clientes acessem dados usando a rede local (LAN) em vez de acessar o conteúdo em conexões WAN lentas.  
  
BranchCache reduz o tráfego de rede de longa distância e o tempo necessário para os usuários das filiais abrir arquivos na rede.  BranchCache sempre fornece aos usuários com os dados mais recentes, e ele protege a segurança do seu conteúdo criptografando caches no servidor cache hospedado e em computadores cliente.  
  
### <a name="what-this-guide-provides"></a>Este guia fornece  
Este guia de implantação permite que você implante BranchCache nos seguintes modos:  
  
-   Modo de cache distribuído. Nesse modo, computadores cliente filiais baixar conteúdo de servidores de conteúdo no office principal ou na nuvem em, em seguida, armazenar em cache o conteúdo para outros computadores na mesma filial. O modo de cache distribuído não exige um computador servidor filial.  
  
-   Modo de cache hospedado. Nesse modo, a ramificação office cliente computadores baixar conteúdo de servidores de conteúdo na nuvem ou office principal e um servidor de cache hospedado recupera o conteúdo dos clientes. O servidor de cache hospedado, em seguida, armazena em cache o conteúdo de outros computadores cliente.  
  
Este guia também fornece instruções sobre como implantar três tipos de servidores de conteúdo. Servidores de conteúdo contêm o conteúdo de origem é baixado por computadores cliente filiais e servidor de conteúdo de um ou mais é necessário para implantar BranchCache em qualquer modo. Os tipos de servidor de conteúdo são:  
  
-   **Servidores de conteúdo baseado em servidor Web**. Esses servidores conteúdos enviam conteúdo para os computadores cliente BranchCache usando os protocolos HTTP e HTTPS. Esses servidores de conteúdo devem estar executando o Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016 versões que suportam BranchCache e, após a qual o recurso BranchCache está instalado.  
  
-   **Servidores de aplicativos com base em BITS**. Esses servidores conteúdos enviam conteúdo para os computadores cliente BranchCache usando o serviço de transferência inteligente em segundo plano (BITS). Esses servidores de conteúdo devem estar executando o Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016 versões que suportam BranchCache e, após a qual o recurso BranchCache está instalado.  
  
-   **Servidores de conteúdo baseado em servidor de arquivos**. Esses servidores de conteúdo devem estar executando o Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016 versões que suportam BranchCache e mediante os serviços que o arquivo de função de servidor está instalada. Além disso, o **BranchCache para arquivos de rede** serviço de função da função de servidor Serviços de arquivo deve ser instalado e configurado. Esses servidores conteúdos enviam conteúdo para os computadores cliente BranchCache usando o protocolo SMB Server Message Block ().  
  
Para obter mais informações, consulte [versões do sistema operacional para BranchCache](https://technet.microsoft.com/en-us/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisitos de implantação do BranchCache

A seguir é os requisitos para implantar BranchCache usando neste guia.  
  
-   **Servidores de conteúdo do arquivo e Web** deve estar executando um dos seguintes sistemas operacionais para fornecer a funcionalidade de BranchCache: Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016. Windows 8 e posteriores clientes continuam vendo benefícios do BranchCache ao acessar conteúdos servidores que executam o Windows Server 2008 R2, no entanto, eles não são capazes de fazer uso dos novos de criação de partes e hash tecnologias no Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.  
  
-   **Computadores cliente** deve estar executando o Windows 10, Windows 8.1 ou Windows 8 para tornar usam o modelo de implantação mais recente e a criação de partes e hash melhorias que foram introduzidas com o Windows Server 2012.  
  
-   **Hospedado servidores de cache** deve estar executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 para usar os aprimoramentos da implantação e dimensionar recursos descritos neste documento.  Um computador que esteja executando um destes sistemas operacionais é configurado como um servidor de cache hospedado pode continuar a servir computadores cliente que executam o Windows 7, mas para fazer isso, ele deve estar equipado com um certificado que é adequado para Transport Layer Security (TLS), conforme descrito no Windows Server 2008 R2 e Windows 7 [guia de implantação do BranchCache](https://technet.microsoft.com/en-us/library/ee649232.aspx).  
  
-   **Um domínio do Active Directory** é necessária para tirar proveito da política de grupo e descoberta automática de cache hospedado, mas um domínio não é necessária para usar BranchCache.  Você pode configurar os computadores individuais usando o Windows PowerShell. Além disso, não é necessário que os controladores de domínio estão executando o Windows Server 2012 ou posterior para utilizar as novas configurações de política de grupo BranchCache; Você pode importar modelos administrativos BranchCache em controladores de domínio que executam sistemas operacionais anteriores, ou você pode criar os objetos de política de grupo remotamente em outros computadores que executam o Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012.

-   **Sites do Active Directory** são usados para limitar o escopo dos servidores de cache hospedado que são detectados automaticamente.  Para detectar automaticamente a um servidor de cache hospedado, computadores cliente e servidor devem pertencer ao mesmo site. BranchCache foi projetado para ter um impacto mínimo em clientes e servidores e não impõe os requisitos de hardware adicionais além daqueles necessários para executar seus respectivos sistemas operacionais.  

**Documentação e BranchCache histórico**

BranchCache foi introduzido no Windows 7&reg; e Windows Server&reg; 2008 R2 e foi aprimorado no Windows Server 2012, Windows 8 e sistemas operacionais posteriores.

> [!NOTE]
> Se você estiver implantando BranchCache nos sistemas operacionais que não sejam Windows Server 2016, os seguintes recursos de documentação estão disponíveis.
> 
> - Para obter informações sobre BranchCache no Windows 8, Windows 8.1, Windows Server 2012 e Windows Server 2012 R2, consulte [visão geral do BranchCache](https://technet.microsoft.com/en-us/library/hh831696.aspx).  
> - Para obter informações sobre BranchCache no Windows 7 e Windows Server 2008 R2, consulte [BranchCache para Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd996634.aspx).  
  


