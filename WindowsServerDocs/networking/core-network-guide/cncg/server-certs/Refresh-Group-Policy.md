---
title: Atualizar Diretiva de Grupo
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863447"
---
# <a name="refresh-group-policy"></a>Atualizar Diretiva de Grupo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para atualizar manualmente a Diretiva de Grupo no computador local. Quando a Diretiva de Grupo é atualizada, se o registro automático de certificados estiver configurado e funcionando corretamente, o computador local terá o registro automático de um certificado pela autoridade de certificação (CA).  
  
> [!NOTE]  
> A Diretiva de Grupo é atualizada automaticamente quando você reinicia ou um usuário faz logon em um computador membro do domínio. Além disso, a Diretiva de Grupo é atualizada periodicamente. Por padrão, essa atualização periódica é realizada a cada 90 minutos com uma diferença de horário aleatória de até 30 minutos.  
  
Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para atualizar a Diretiva de Grupo no computador local  
  
1.  No computador em que o NPS está instalado, abra o Windows PowerShell&reg; usando o ícone na barra de tarefas.  
  
2.  No prompt do Windows PowerShell, digite **gpupdate**, e pressione ENTER.  
  


