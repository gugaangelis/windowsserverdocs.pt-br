---
title: Personalizar Inscrição para a tarefa do Microsoft Azure Online Backup
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c424be2d0815fbcb217e0453b27f4b7aceb07ee6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623631"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar Inscrição para a tarefa do Microsoft Azure Online Backup

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por padrão, a tarefa **Inscrever-se para o Serviço de Backup Online da Microsoft** na guia **DISPOSITIVOS** do Dashboard abra no site Microsoft Online Backup Service. O site fornece informações sobre o serviço e ajuda-o a inscrever-se para o serviço e baixar o software necessário.

 Você pode personalizar a tarefa **Inscrever-se para o Serviço de Backup Online da Microsoft** de duas maneiras:

-   Você pode substituir o URL padrão do site padrão por um URL que represente a experiência do usuário personalizada. Para substituir a URL padrão, abra o Editor de Registro, crie a chave de registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, e então atribua o URL personalizado como o valor para a chave.

-   Você pode ocultar a tarefa: Para ocultar a tarefa, abra o Editor do Registro e crie a chave de registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.

## <a name="see-also"></a>Consulte Também
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)