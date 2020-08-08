---
title: Configurar o HGS para comunicações HTTPS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 4e708b61bd629b5b784926338b1aee122e2ecbee
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966153"
---
# <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Por padrão, quando você inicializar o servidor HGS, ele configurará os sites da Web do IIS para comunicações somente HTTP.
Todo material confidencial que está sendo transmitido para e do HGS é sempre criptografado usando a criptografia no nível da mensagem, no entanto, se você quiser um nível mais alto de segurança, também poderá habilitar o HTTPS Configurando o HGS com um certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)]

