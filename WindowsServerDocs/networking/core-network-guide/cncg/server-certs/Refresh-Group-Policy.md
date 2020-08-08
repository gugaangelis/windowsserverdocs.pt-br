---
title: Atualizar Diretiva de Grupo
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b0bc677d56e97f6b10d1ca12ece92067e210eab9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949390"
---
# <a name="refresh-group-policy"></a>Atualizar Diretiva de Grupo

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para atualizar manualmente a Diretiva de Grupo no computador local. Quando a Diretiva de Grupo é atualizada, se o registro automático de certificados estiver configurado e funcionando corretamente, o computador local terá o registro automático de um certificado pela autoridade de certificação (CA).

> [!NOTE]
> A Diretiva de Grupo é atualizada automaticamente quando você reinicia ou um usuário faz logon em um computador membro do domínio. Além disso, a Diretiva de Grupo é atualizada periodicamente. Por padrão, essa atualização periódica é realizada a cada 90 minutos com uma diferença de horário aleatória de até 30 minutos.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para atualizar a Diretiva de Grupo no computador local

1.  No computador em que o NPS está instalado, abra o Windows PowerShell &reg; usando o ícone na barra de tarefas.

2.  No prompt do Windows PowerShell, digite **gpupdate**e pressione Enter.



