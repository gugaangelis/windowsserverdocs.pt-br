---
title: Etapa 3 verificar a implantação avançada do DirectAccess
description: Este tópico faz parte do guia implantar um único servidor DirectAccess com as configurações avançadas do Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6d8f9b5ecbc322de12cb00179a4ebb044fbd2baf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309092"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Etapa 3 verificar a implantação avançada do DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação do DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para verificar o acesso aos recursos internos por meio do DirectAccess  
  
1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha o objeto Política de Grupo.  
  
2.  Clique no ícone **conexões de rede** na área de notificação para acessar o Gerenciador de mídia do DirectAccess.  
  
3.  Clique em **conexão DirectAccess**e você verá que o status é **conectado localmente**.  
  
4.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.  
  
    Você deve conseguir acessar todos os recursos corporativos.  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Etapa anterior  
  
-   [Etapa 2: Configurando servidores DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  


