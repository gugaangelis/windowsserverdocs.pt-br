---
title: Configurar o HGS para comunicações https
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403676"
---
# <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Por padrão, quando você inicializar o servidor HGS, ele configurará os sites da Web do IIS para comunicações somente HTTP.
Todo material confidencial que está sendo transmitido para e do HGS é sempre criptografado usando a criptografia no nível da mensagem, no entanto, se você quiser um nível mais alto de segurança, também poderá habilitar o HTTPS Configurando o HGS com um certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

