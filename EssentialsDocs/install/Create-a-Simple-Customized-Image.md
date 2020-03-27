---
title: Criar uma Imagem Personalizada Simples
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74a1729f9445710b6b9d279f1603295eff2a5d25
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312133"
---
# <a name="create-a-simple-customized-image"></a>Criar uma Imagem Personalizada Simples

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode usar o procedimento a seguir para criar uma imagem personalizada simples:  
  
#### <a name="to-create-the-image"></a>Para criar a imagem  
  
1.  Depois da instalação do servidor, na primeira página da Configuração Inicial, pressione Shift+F10 para iniciar a janela cmd.  
  
2.  Crie o arquivo SkipIC.txt sob a raiz da unidade do sistema.  
  
3.  Reinicie o servidor.  
  
4.  Inicialize o servidor usando a unidade flash USB ou o DVD, que inclui o arquivo unattend.xml. Para obter informações sobre a criação de uma unidade flash USB inicializável, consulte [Criar uma unidade flash USB inicializável](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Adicione a identidade visual do logotipo ao Dashboard. Para obter mais informações sobre como adicionar a identidade visual, consulte [Adicionar a Identidade Visual ao Dashboard, ao Acesso Remoto da Web e à Barra Inicial](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Crie o arquivo OOBE.xml para exibir informações personalizadas, como o nome da empresa, o logotipo e o EULA. Para obter mais informações sobre o arquivo OOBE.xml, consulte [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Altere o nome padrão do servidor se não defini-lo em unattend.xml.  
  
8.  Por padrão, o nome do servidor será uma cadeia de caracteres aleatória. Altere o nome do servidor para outra cadeia de caracteres (como ContosoServer) e informe seu cliente sobre o novo nome de servidor.  
  
9. Prepare a Imagem para Implantação conforme descrito em [Preparando a Imagem para Implantação](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Consulte também  
 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)