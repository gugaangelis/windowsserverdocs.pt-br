---
title: Crie uma imagem personalizada Simple
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>Crie uma imagem personalizada Simple

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode usar o procedimento a seguir para criar uma imagem personalizada simple:  
  
#### <a name="to-create-the-image"></a>Para criar a imagem  
  
1.  Após a instalação do servidor, na primeira página de configuração inicial, pressione Shift + F10 para iniciar a janela cmd.  
  
2.  Crie um arquivo SkipIC.txt sob a raiz da unidade do sistema.  
  
3.  Reinicie o servidor.  
  
4.  Inicie o servidor usando uma unidade flash USB ou DVD, que inclui o arquivo unattend.xml. Para obter informações sobre como criar uma unidade flash USB inicializável, consulte [criar uma unidade inicializável USB Flash](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Adicione o logotipo de identidade visual para o painel. Para obter mais informações sobre como adicionar identidade visual, consulte [adicionar identidade visual para o painel, acesso via Web remoto e Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Crie o arquivo OOBE.xml para exibir informações personalizadas, como nome da empresa, o logotipo e o EULA. Para obter mais informações sobre o arquivo OOBE.xml, veja [criar o Oobe.xml arquivo incluindo logotipo e o EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Altere o nome do servidor padrão se você não defini-la em unattend.xml.  
  
8.  Por padrão, o nome do servidor será uma cadeia de caracteres aleatória. Altere o nome do servidor para outra cadeia de caracteres (como ContosoServer) e então informar seu cliente sobre o novo nome do servidor.  
  
9. Preparar a imagem para implantação, conforme descrito em [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Consulte também  
 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)