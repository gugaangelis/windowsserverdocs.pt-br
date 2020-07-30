---
title: Personalizar Inscrição para a tarefa do Microsoft Azure Online Backup
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77d1c98688f58e767d4ad76c0abf5456ef7ba052
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181302"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar Inscrição para a tarefa do Microsoft Azure Online Backup

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por padrão, a tarefa **Inscrever-se para o Serviço de Backup Online da Microsoft** na guia **DISPOSITIVOS** do Dashboard abra no site Microsoft Online Backup Service. O site fornece informações sobre o serviço e ajuda-o a inscrever-se para o serviço e baixar o software necessário.

 Você pode personalizar a tarefa **Inscrever-se para o Serviço de Backup Online da Microsoft** de duas maneiras:

-   Você pode substituir o URL padrão do site padrão por um URL que represente a experiência do usuário personalizada. Para substituir a URL padrão, abra o Editor de Registro, crie a chave de registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, e então atribua o URL personalizado como o valor para a chave.

-   Você pode ocultar a tarefa: Para ocultar a tarefa, abra o Editor do Registro e crie a chave de registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.

## <a name="see-also"></a>Consulte Também
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)