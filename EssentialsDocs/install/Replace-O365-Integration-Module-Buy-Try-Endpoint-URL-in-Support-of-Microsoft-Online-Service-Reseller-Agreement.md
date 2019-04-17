---
title: "Substituir integração O365 módulo URL de ponto de extremidade Try comprar para melhorar o contrato de revendedor de serviço Online do Microsoft"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Substituir integração O365 módulo URL de ponto de extremidade Try comprar para melhorar o contrato de revendedor de serviço Online do Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Se você for um Microsoft Online Service (parceiro Reseller Agreement), para garantir que as transações de inscrição do cliente sejam processadas por meio de seu portal, você precisará substituir as URLs de ponto de extremidade usadas pelo módulo de integração do Office 365 do Windows Server Essentials.  
  
 O módulo de integração usa as URLs de ponto de extremidade de quatro seguintes:  
  
1.  Uma empresa de compra de assinatura do Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome de chave = MOSRASTDBUY  
  
    -   Valor = *xxxxx*, em que xxxxx é sua URL de compra de assinatura do enterprise. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Uma empresa de avaliação do assinatura do Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome de chave = MOSRASTDTRY  
  
    -   Valor = *xxxxx*, em que xxxxx é sua URL de compra de assinatura do enterprise. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Um ponto de extremidade de compra assinatura do Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome de chave = MOSRALITEBUY  
  
    -   Valor = *xxxxx*, em que xxxxx é sua URL de compra de assinatura do enterprise. Por exemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Uma empresa de avaliação do assinatura do Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome de chave = MOSRALITETRY  
  
    -   Valor = *xxxxx*, em que xxxxx é sua URL de compra de assinatura do enterprise. Por exemplo, valor = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Para adicionar uma chave de URL de ponto de extremidade ao registro  
  
1.  No computador de referência, clique em **iniciar**, tipo **regedit**, e pressione ENTER.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**e, em seguida, expanda **MSO**.  
  
3.  Se MSO não existir, clique com botão direito **Windows Server**, aponte para **nova**, clique em **chave**e, em seguida, digite **MSO** para o nome da chave.  
  
4.  Clique com botão direito MSO e clique em **valor de cadeia de caracteres**. Digite um dos seguintes nomes de cadeia de caracteres de ponto de extremidade para o nome da cadeia de caracteres:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Clique com botão direito na nova cadeia no painel direito e clique em **modificar**.  
  
6.  Digite sua nova URL de ponto de extremidade no **dados do valor** caixa de texto e clique em **Okey**.  
  
7.  Repita as etapas 4-6 para cada nome de cadeia de caracteres listado na etapa 4.  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)[criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

