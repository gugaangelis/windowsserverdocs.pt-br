---
title: Configurar o HGS para comunicações Https
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828997"
---
# <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Por padrão, quando você inicializa o servidor HGS será configurar os sites do IIS para comunicações HTTP-only.
Todo o material confidencial que estão sendo transmitido de e HGS são sempre criptografadas usando a criptografia de nível de mensagem, no entanto, se desejar que um nível mais alto de segurança você também pode habilitar o HTTPS ao configurar o HGS com um certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

