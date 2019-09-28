---
title: Introdução à associação de token
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 52ba35808b34eb07ecd6ac92819e9dc7a693b15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403330"
---
# <a name="introducing-token-binding"></a>Introdução à associação de token

>Aplica-se a: Windows Server 2016 e Windows 10

O protocolo de associação de token permite que aplicativos e serviços associem criptograficamente seus tokens de segurança à camada TLS para atenuar o roubo de token e ataques de repetição. As associações do TLS [RFC5246] de longa duração, exclusivamente identificáveis, podem abranger várias sessões e conexões de TLS.

Suporte à versão:

- Windows 10, versão 1507 – desativado por padrão
    - Protocolo de associação de token adicionado [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Suporte a SYS da Associação de token sobre HTTP [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versões 1511 e 1607 e Windows Server 2016 – ativado por padrão
    - Protocolo de associação de token atualizado [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensão TLS para negociação de associação de token adicionada [[Draft-Popov-tokbind-Negotiation-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Suporte a SYS da Associação de token sobre HTTP atualizado [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versão 1507 com atualização de manutenção [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versão 1511 com atualização de serviço [KB4034660](https://support.microsoft.com/kb/KB4034660), windows 10, versão 1607 e Windows Server 2016 com atualização de serviço [KB4034658](https://support.microsoft.com/kb/KB4034658) suporte para protocolo de associação de token versão 0,10 – ativado por padrão
    - Protocolo de associação de token atualizado [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão TLS para negociação de associação de token adicionada [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Suporte a SYS da Associação de token sobre HTTP atualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- O Windows 10, versão 1703, dá suporte ao protocolo de ligação de token versão 0,10 – ativado por padrão
    - Protocolo de associação de token atualizado [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão TLS para negociação de associação de token adicionada [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Suporte a SYS da Associação de token sobre HTTP atualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Dispositivos Windows com segurança baseada em virtualização habilitada manterão as chaves de associação de token em um ambiente protegido isolado do sistema operacional em execução

Informações sobre o suporte ao ASP .NET podem ser encontradas na [fonte de referência do .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obter informações sobre .NET Framework, consulte os seguintes tópicos:

- [Aprimoramentos de rede](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe Tokenbinding do .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
