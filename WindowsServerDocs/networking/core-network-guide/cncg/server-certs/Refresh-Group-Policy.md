---
title: Atualizar Diretiva de Grupo
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b9522909960470d9f5f3e183afbd97ab1b919019
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318255"
---
# <a name="refresh-group-policy"></a>Atualizar Diretiva de Grupo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para atualizar manualmente a Diretiva de Grupo no computador local. Quando a Diretiva de Grupo é atualizada, se o registro automático de certificados estiver configurado e funcionando corretamente, o computador local terá o registro automático de um certificado pela autoridade de certificação (CA).  
  
> [!NOTE]  
> A Diretiva de Grupo é atualizada automaticamente quando você reinicia ou um usuário faz logon em um computador membro do domínio. Além disso, a Diretiva de Grupo é atualizada periodicamente. Por padrão, essa atualização periódica é realizada a cada 90 minutos com uma diferença de horário aleatória de até 30 minutos.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para atualizar a Diretiva de Grupo no computador local  
  
1.  No computador em que o NPS está instalado, abra o Windows PowerShell&reg; usando o ícone na barra de tarefas.  
  
2.  No prompt do Windows PowerShell, digite **gpupdate**e pressione Enter.  
  


