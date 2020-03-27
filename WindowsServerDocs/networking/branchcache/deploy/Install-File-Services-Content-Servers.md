---
title: Instalar servidores de conteúdo dos Serviços de Arquivo
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4fb9e40ce34a82a8797db1bf6d61c739f742c2d3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319246"
---
# <a name="install-file-services-content-servers"></a>Instalar servidores de conteúdo dos Serviços de Arquivo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Para implantar servidores de conteúdo que executam a função de servidor de Serviços de Arquivo, instale o BranchCache para o serviço de função de arquivos de rede da função de servidor de Serviços de Arquivo. Além disso, você deve habilitar o BranchCache em compartilhamentos de arquivos de acordo com suas necessidades.  
  
Durante a configuração do servidor de conteúdo, você pode permitir a publicação do BranchCache de conteúdo para todos os compartilhamentos de arquivos ou pode selecionar um subconjunto de compartilhamentos de arquivos para publicar.  
  
> [!NOTE]  
> Quando você implanta um servidor de arquivos habilitado para BranchCache ou um servidor Web como um servidor de conteúdo, as informações de conteúdo agora são calculadas offline, bem antes de um cliente do BranchCache solicitar um arquivo. Por causa dessa melhoria, você não precisa configurar a publicação de hash para servidores de conteúdo, como fez na versão anterior do BranchCache.  
>   
> Essa geração automática de hash fornece um desempenho mais rápido e mais economia de largura de banda, pois as informações de conteúdo estão prontas para o primeiro cliente que solicita o conteúdo, e os cálculos já foram realizados.  
  
Consulte os tópicos a seguir para implantar servidores de conteúdo.  
  
-   [Configurar a função de servidor de Serviços de Arquivo](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Habilitar a publicação de hash para servidores de arquivos](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Habilitar o BranchCache em um compartilhamento &#40;de arquivos opcional&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


