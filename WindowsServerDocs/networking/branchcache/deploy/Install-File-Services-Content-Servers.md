---
title: Instalar servidores de conteúdo de serviços de arquivo
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>Instalar servidores de conteúdo de serviços de arquivo

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Para implantar os servidores de conteúdo que estiver executando a função de servidor Serviços de arquivo, você deve instalar o BranchCache para o serviço de função de arquivos de rede da função de servidor Serviços de arquivo. Além disso, você deve habilitar BranchCache em compartilhamentos de arquivos de acordo com suas necessidades.  
  
Durante a configuração do servidor de conteúdo, você pode permitir que BranchCache publicação de conteúdo para todos os compartilhamentos de arquivos ou você pode selecionar um subconjunto dos compartilhamentos de arquivos para publicar.  
  
> [!NOTE]  
> Quando você implanta um servidor de arquivo ativado BranchCache ou da Web como um servidor de conteúdo, informações de conteúdo agora são calculadas offline, bem antes de um cliente BranchCache solicita um arquivo. Por causa dessa melhoria, você não precisa configurar a publicação de hash para servidores de conteúdo, como você fez na versão anterior do BranchCache.  
>   
> Essa geração automática de hash fornece desempenho mais rápido e mais economia de largura de banda, como informações de conteúdo estão prontas para o primeiro cliente que solicita o conteúdo e cálculos já foi executados.  
  
Veja os tópicos a seguir para implantar servidores de conteúdo.  
  
-   [Configurar a função de servidor Serviços de arquivo](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Habilitar a publicação de Hash para servidores de arquivos](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Habilitar o BranchCache em um compartilhamento de arquivos & #40; opcional & #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


