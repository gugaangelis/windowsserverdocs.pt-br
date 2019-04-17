---
title: "Informações importante sobre como usar o ADK do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Informações importante sobre como usar o ADK do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para criar e personalizar uma imagem do Windows Server Essentials, você usa muitas das ferramentas do [ADK do Windows 8](https://go.microsoft.com/fwlink/?LinkId=248647), mas há algumas diferenças importantes entre o ADK do Windows 8 e o ADK do Windows Server Essentials.  
  
 Você deve estar ciente das seguintes diferenças importantes:  
  
-   Algumas configurações foram alteradas na **%windir%\setup\script\SetupComplete.cmd**. Se você quiser usar esse comando, você pode adicionar cmdlines adicionais, mas não remova as linhas existentes.  
  
## <a name="working-with-passwords"></a>Trabalhando com senhas  
  
-   A senha de administrador é definida como Admin@123 e o logon automático está habilitado na Install.wim\unattend.xml. Portanto, você não precise digitar novamente a senha várias vezes durante a configuração inicial do servidor. Se você tiver um unattend.xml personalizado na raiz da mídia removível, essa configuração será substituído e você precisará definir a senha e logon durante a inicialização...  
  
-   Durante a configuração inicial, o usuário final é solicitado a criar uma nova conta e senha. Essa nova conta torna-se a conta de administrador de rede para o sistema operacional. O logon de conta e automática de administrador é desabilitado. Você pode automatizar esse processo usando o arquivo de cfg.ini para testes de garantia de qualidade.  
  
-   Consulte o [ADK do Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) documentação para obter detalhes sobre como criar um arquivo unattend.xml.  
  
## <a name="see-also"></a>Consulte também  

 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Introdução ao Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

