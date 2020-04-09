---
title: Configurar o HGS para comunicações HTTPS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: de57a4026a33561760ad36fd78d732352b3aa340
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856839"
---
# <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Por padrão, quando você inicializar o servidor HGS, ele configurará os sites da Web do IIS para comunicações somente HTTP.
Todo material confidencial que está sendo transmitido para e do HGS é sempre criptografado usando a criptografia no nível da mensagem, no entanto, se você quiser um nível mais alto de segurança, também poderá habilitar o HTTPS Configurando o HGS com um certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

