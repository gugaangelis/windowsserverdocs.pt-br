---
title: "Adicionar informações de parceiro de registro do Microsoft Online Service Partner contrato"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Adicionar informações de parceiro de registro do Microsoft Online Service Partner contrato

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Se você for um parceiro Microsoft Online Service MOSPA (Partner Agreement) para o Office 365, para garantir que será corretamente compensado quando um solicitação de assinatura for originado do Windows Server Essentials por meio do módulo de integração do Office 365, você precisa criar uma chave do registro que contém seu parceiro de registro POR ID (identificação). As informações a seguir é ler e passadas ao provedor de serviços as URLs de inscrição do Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Tipo = String Value  
  
-   Nome de chave = Partner  
  
-   Valor = xxxxx, em que xxxxx é seu POR ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Para adicionar a chave POR ID ao registro  
  
1.  No computador de referência, clique em **iniciar**, tipo **regedit**, e pressione ENTER.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**e, em seguida, expanda **Windows Server**.  
  
3.  Clique com botão direito **Windows Server**, aponte para **nova**e clique em **chave**.  
  
4.  Tipo **MSO** para o nome da chave.  
  
5.  Clique com botão direito a chave que você acabou criado e clique em **valor de cadeia de caracteres**.  
  
6.  Tipo **parceiro** para o nome da cadeia de caracteres e pressione ENTER.  
  
7.  Clique com botão direito do novo **parceiro** cadeia de caracteres no painel direito e clique em **modificar**.  
  
8.  Digite seu POR ID no **dados do valor** caixa de texto e clique em **Okey**.  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

