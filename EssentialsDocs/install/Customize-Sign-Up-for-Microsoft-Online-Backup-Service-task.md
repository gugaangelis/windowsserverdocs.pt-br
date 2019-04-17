---
title: "Personalizar inscrever-se para a tarefa de serviço de Backup Online da Microsoft"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar inscrever-se para a tarefa de serviço de Backup Online da Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por padrão, o **Inscreva-se para o serviço de Backup Online da Microsoft** de tarefas no **dispositivos** guia do painel abre o site do serviço de Backup Online da Microsoft. O site fornece informações sobre o serviço e ajuda você a se inscrever no serviço e baixar o software necessário.  
  
 Você pode personalizar o **Inscreva-se para o serviço de Backup Online da Microsoft** tarefa de duas maneiras:  
  
-   Você pode substituir a URL do site padrão com uma URL que representa uma experiência de usuário personalizadas. Para substituir a URL padrão, abra o Editor do registro, crie a chave do registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**e, em seguida, atribua a URL personalizada como o valor da chave.  
  
-   Você pode ocultar a tarefa. Para ocultar a tarefa, abra o Editor do registro e crie a chave do registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows server\onlinebackup\onlinebackupinstalled.**.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)