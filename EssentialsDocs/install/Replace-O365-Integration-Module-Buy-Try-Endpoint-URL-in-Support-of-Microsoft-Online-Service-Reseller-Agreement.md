---
title: Substituir a URL de ponto de extremidade de compra/teste do módulo de integração O365 em suporte ao Contrato de Revendedor de Serviços Online da Microsoft
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 11e18d7d61edb0a618fb71791f77c4df84c9d708
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311539"
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Substituir a URL de ponto de extremidade de compra/teste do módulo de integração O365 em suporte ao Contrato de Revendedor de Serviços Online da Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Se você for um parceiro do Microsoft Online Service Reseller Agreement (MOSRA), para garantir que as transações de inscrição do cliente sejam processadas por meio do portal, será necessário substituir as URLs do ponto de extremidade usadas pelo módulo de integração do Office 365 do Windows Server Essentials.  
  
 O módulo de integração usa os quatro seguintes URLs de ponto final:  
  
1.  Um ponto final de compra de assinatura do Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome da chave = MOSRASTDBUY  
  
    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Um ponto final de teste de assinatura do Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome da chave = MOSRASTDTRY  
  
    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Um ponto de extremidade de compra de assinatura do Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome da chave = MOSRALITEBUY  
  
    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Um ponto de extremidade de avaliação de assinatura do Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome da chave = MOSRALITETRY  
  
    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Para adicionar uma chave de URL de ponto de final ao registro  
  
1.  No computador de referência, clique em **Iniciar**, digite **regedit** e pressione ENTER.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server** e então expanda **MSO**.  
  
3.  Se o MSO não existir, clique com o botão direito em **Windows Server**, aponte para **Novo**, clique em **Chave**, e depois digite **MSO** para o nome da chave.  
  
4.  Clique com o botão direito no MSO e, em seguida, clique em **Valor da Cadeia de Caracteres**. Digite um dos seguintes nomes da cadeia de caracteres do ponto final para o nome da cadeia de caracteres:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Clique com o botão direito do mouse na nova cadeia de caracteres no painel à direita e clique em **Modificar**.  
  
6.  Digite seu novo ID de ponto final na caixa de texto **Dados do valor** e depois clique em **OK**.  
  
7.  Repita as etapas de 4 a 6 para cada nome de cadeia de caracteres listado na etapa 4.  
  
## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md) ao [criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](../install/Testing-the-Customer-Experience.md)

