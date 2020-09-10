---
title: Substituir Microsoft 365 módulo de integração comprar-Experimente a URL de ponto de extremidade para dar suporte ao contrato de revendedor de serviços online da Microsoft
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a8a5cf91c6de2971bc8270cc3c7ea92327b71224
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623370"
---
# <a name="replace-microsoft-365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Substituir Microsoft 365 módulo de integração comprar-Experimente a URL de ponto de extremidade para dar suporte ao contrato de revendedor de serviços online da Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>
 Se você for um parceiro do Microsoft Online Service Reseller Agreement (MOSRA), para garantir que as transações de inscrição do cliente sejam processadas por meio do portal, será necessário substituir as URLs do ponto de extremidade usadas pelo módulo de integração do Windows Server Essentials Microsoft 365.

 O módulo de integração usa os quatro seguintes URLs de ponto final:

1.  Um ponto de extremidade de compra de assinatura Microsoft 365 Enterprise.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG-SZ

    -   Nome da chave = MOSRASTDBUY

    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html

2.  Um ponto de extremidade de avaliação de assinatura Microsoft 365 Enterprise.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG-SZ

    -   Nome da chave = MOSRASTDTRY

    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html

3.  Um ponto de extremidade de compra de assinatura do Microsoft 365 Small Business Premium.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG-SZ

    -   Nome da chave = MOSRALITEBUY

    -   Valor = *xxxxx*, onde xxxxx é o seu URL de compra de assinatura empresarial. Por exemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html

4.  Um ponto de extremidade de avaliação de assinatura do Microsoft 365 Small Business Premium.

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

## <a name="see-also"></a>Consulte Também

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

