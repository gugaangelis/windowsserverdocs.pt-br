---
title: Atualizar Diretiva de Grupo
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 88d39f41f61ae7c7f6a1fb84aa99806c4796c8cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356191"
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
  


