---
title: Política de grupo de atualização
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>Política de grupo de atualização

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para atualizar manualmente a política de grupo no computador local. Quando a política de grupo é atualizada, se o registro de certificado automático é configurado e funcionando corretamente, o computador local é registrado automaticamente um certificado pela autoridade de certificação (CA).  
  
> [!NOTE]  
> Política de grupo é atualizada automaticamente quando você reiniciar o computador membro do domínio, ou quando um usuário faz logon em um computador de membro do domínio. Além disso, política de grupo é atualizada periodicamente. Por padrão, essa atualização periódica é executada a cada 90 minutos com um deslocamento aleatório de até 30 minutos.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para atualizar a política de grupo no computador local  
  
1.  No computador onde o NPS está instalado, abra o Windows PowerShell&reg; usando o ícone na barra de tarefas.  
  
2.  No prompt do Windows PowerShell, digite **gpupdate**, e pressione ENTER.  
  


