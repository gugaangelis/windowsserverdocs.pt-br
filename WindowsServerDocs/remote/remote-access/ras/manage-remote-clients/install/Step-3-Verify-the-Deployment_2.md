---
title: Etapa 3 verificar a implantação
description: Este tópico faz parte do guia de clientes do DirectAccess de gerenciar remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8d2b0b27d4b1d33971564672954667b49a87a4e0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282764"
---
# <a name="step-3-verify-the-deployment"></a>Etapa 3 verificar a implantação

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação para o gerenciamento remoto de clientes DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Para verificar a implantação apropriada  
  
1.  Conectar um computador cliente DirectAccess à rede corporativa e obter o objeto de diretiva de grupo.  
  
2.  No computador cliente, clique o **conexões de rede** ícone na área de notificação para acessar o Gerenciador de mídia do DirectAccess.  
  
3.  Clique em **Conexão do DirectAccess**, e você verá que o status é **conectado localmente**.  
  
4.  Remover o computador da rede corporativa e conectá-lo a uma rede pública.  
  
5.  Em um prompt de comando, digite **nltest /dsgetdc: [nome de domínio totalmente qualificado]** . Esse comando verificará a rede corporativa é acessível ao cliente. Se o controlador de domínio não estiver acessível, a seguinte mensagem de erro será exibida a relatar que o domínio não existe: ERROR_NO_SUCH_DOMAIN.  
  


