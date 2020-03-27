---
title: Etapa 3 verificar a implantação
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e1a8d3208acb0e0ce891e517492916357b9fbfcc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314351"
---
# <a name="step-3-verify-the-deployment"></a>Etapa 3 verificar a implantação

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação para o gerenciamento remoto de clientes DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Para verificar a implantação adequada  
  
1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha o objeto Política de Grupo.  
  
2.  No computador cliente, clique no ícone **conexões de rede** na área de notificação para acessar o Gerenciador de mídia do DirectAccess.  
  
3.  Clique em **conexão DirectAccess**e você verá que o status é **conectado localmente**.  
  
4.  Remova o computador da rede corporativa e conecte-o a uma rede pública.  
  
5.  Em um prompt de comando, digite **nltest/dsgetdc: [nome de domínio totalmente qualificado]** . Esse comando verificará se a rede corporativa está acessível ao cliente. Se o controlador de domínio não estiver acessível, a seguinte mensagem de erro exibirá o relatório de que o domínio não existe: ERROR_NO_SUCH_DOMAIN.  
  


