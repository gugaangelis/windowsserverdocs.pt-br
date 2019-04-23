---
title: Apresentando a associação de Token
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884227"
---
# <a name="introducing-token-binding"></a>Apresentando a associação de Token

>Aplica-se a: Windows Server 2016 e Windows 10

O protocolo de associação de Token permite que aplicativos e serviços associar criptograficamente seus tokens de segurança para a camada de TLS para reduzir o roubo de token e ataques de repetição. As associações de TLS [RFC5246] exclusivamente identificáveis, de longa duração podem abranger várias conexões e sessões TLS.

Suporte de versão:

- Windows 10, versão 1507 – desativado por padrão
    - Adicionado o protocolo de associação de token [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - O WinInet & HTTP. O suporte de associação de token por meio de HTTP SYS [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, as versões 1511 e 1607 e Windows Server 2016 – em por padrão
    - Atualizado o protocolo de associação de token [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensão TLS para negociação de associação de token adicionada [[draft-popov-tokbind-negociação-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - O WinInet & HTTP. Suporte SYS de associação de token por meio de HTTP atualizado [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versão 1507, com a atualização de manutenção [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versão 1511 com a atualização de manutenção [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, versão 1607 e Windows Server 2016 com deatualizaçãodemanutenção[KB4034658](https://support.microsoft.com/kb/KB4034658) dar suporte à versão de protocolo de associação de Token 0,10 – em por padrão
    - Atualizado o protocolo de associação de token [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão TLS para negociação de associação de token adicionada [[draft-ietf-tokbind-negociação-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - O WinInet & HTTP. Suporte SYS de associação de token por meio de HTTP atualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versão 1703 oferece suporte a versão de protocolo de associação de Token 0.10 – por padrão
    - Atualizado o protocolo de associação de token [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão TLS para negociação de associação de token adicionada [[draft-ietf-tokbind-negociação-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - O WinInet & HTTP. Suporte SYS de associação de token por meio de HTTP atualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Dispositivos do Windows com segurança baseada em virtualização habilitada manterá as chaves de associação de token em um ambiente protegido que é isolado de sistema operacional em execução

Informações sobre o suporte do ASP .NET podem ser encontradas na [.NET Framework Reference Source](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obter informações sobre o .NET Framework, consulte os tópicos a seguir:

- [Aperfeiçoamentos de rede](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe TokenBinding .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
