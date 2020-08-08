---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar computadores cliente no parceiro de conta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b78c3d3b4c5562acfcc7b2657f6e49119fc491e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967593"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar computadores cliente no parceiro de conta

A maneira mais fácil de um administrador em uma organização parceira de conta preparar os computadores cliente para acesso a Serviços de Federação do Active Directory (AD FS) \( AD FS \) aplicativos federados é usar política de grupo. A Política de Grupo fornece uma maneira conveniente de você enviar por push configurações e certificados específicos necessários para a federação para todos os computadores cliente que serão usados para acessar os aplicativos federados.

Para que os computadores cliente possam acessar diretamente aplicativos federados sem prompts de certificado ou prompts relacionados ao site confiável, recomendamos que você primeiro Prepare cada computador cliente antes de implantar AD FS amplamente em sua organização. Considere usar a Política de Grupo para automaticamente:

-   Configure o Internet Explorer em cada computador cliente para confiar no servidor de Federação da conta.

    Para obter mais informações, consulte [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).

-   Instale o servidor de Federação de conta apropriado, o servidor de Federação de recursos e o servidor Web protocolo SSL \( \) certificados SSL \( ou certificados equivalentes que se encadeadom a uma raiz confiável \) em cada computador cliente.

    Para obter mais informações, consulte [distribuir certificados para computadores cliente usando política de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).


## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
