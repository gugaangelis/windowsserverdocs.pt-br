---
title: Adicionar informações de parceiro de registro do Contrato de Parceiro de Serviço Online da Microsoft
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 807aacc5039bf90ea4dd7c7859c232d8c8b3011a
ms.sourcegitcommit: 34f9577ef32cbdc7ef96040caabc9d83517f9b79
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554339"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Adicionar informações de parceiro de registro do Contrato de Parceiro de Serviço Online da Microsoft

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>
 Se você for um parceiro do Microsoft Online Service Partner Agreement (MOSPA) para Microsoft 365, para garantir que você seja compensado corretamente quando uma solicitação de assinatura for originada do Windows Server Essentials por meio do módulo de integração de Microsoft 365, será necessário criar uma chave do registro que contenha a identificação do parceiro de registro (POR ID). As informações a seguir são lidas e passadas para o provedor de serviços por meio das URLs de inscrição Microsoft 365.

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

## <a name="see-also"></a>Consulte Também

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

