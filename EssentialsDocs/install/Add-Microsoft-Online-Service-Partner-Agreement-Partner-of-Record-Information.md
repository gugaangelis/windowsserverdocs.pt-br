---
title: Adicionar informações de parceiro de registro do Contrato de Parceiro de Serviço Online da Microsoft
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ec8387c59ebf42eb4287807e5959a50cea4215c4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310240"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Adicionar informações de parceiro de registro do Contrato de Parceiro de Serviço Online da Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Se você for um parceiro do Microsoft Online Service Partner Agreement (MOSPA) para Office 365, para garantir que você seja compensado corretamente quando uma solicitação de assinatura for originada do Windows Server Essentials por meio do módulo de integração do Office 365, você precisará criar um chave do registro que contém sua identificação de parceiro de registro (POR ID). As informações a seguir são lidas e enviadas ao provedor de serviços através dos URLs de sign-up do Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Tipo = valor da cadeia de caracteres  
  
-   Nome da chave = Partner  
  
-   Valor = xxxxx, onde xxxxx é seu POR ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Para adicionar a chave POR ID ao registro  
  
1.  No computador de referência, clique em **Iniciar**, digite **regedit** e pressione ENTER.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**.  
  
3.  Clique com o botão direito do mouse em **Windows Server**, aponte para **Novo** e clique em **Chave**.  
  
4.  Digite **MSO** como nome da chave.  
  
5.  Clique com o botão direito do mouse na chave que você acabou de criar e, em seguida, clique em **Valor da Cadeia de Caracteres**.  
  
6.  Digite **Partner** para o nome da cadeia de caracteres e pressione ENTER.  
  
7.  Clique com o botão direito na nova cadeia de caracteres de **Partner** no painel à direita e, em seguida, clique em **Modificar**.  
  
8.  Digite seu POR ID na caixa de texto **Dados do valor** e depois clique em **OK**.  
  
## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)

 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](../install/Testing-the-Customer-Experience.md)

