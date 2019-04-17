---
title: "Apresentando o Token associação"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>Apresentando o Token associação

>Aplica-se a: Windows Server 2016 e Windows 10

O protocolo de associação Token permite que os aplicativos e serviços associar criptograficamente respectivos tokens de segurança para a camada TLS para atenuar o roubo de token e ataques de repetição. As associações de longa duração, identificáveis único TLS [RFC5246] podem abranger várias sessões TLS e conexões.

Suporte de versão:

- Windows 10, versão 1507 – desativado por padrão
    - Token protocolo Binding adicionado [[rascunho-ietf-tokbind-protocolo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Suporte a SYS de associação token por HTTP [[rascunho-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versões 1511 e 1607 e Windows Server 2016 – por padrão
    - Token protocolo Binding atualizado [[rascunho-ietf-tokbind-protocolo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensão do TLS negociação de associação token adicionada [[rascunho-popov-tokbind-negociação-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Suporte a SYS de associação token por HTTP atualizado [[rascunho-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versão 1507 com atualização de manutenção [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versão 1511 com atualização de manutenção [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, versão 1607 e Windows Server 2016 com atualização de manutenção [KB4034658](https://support.microsoft.com/kb/KB4034658) suporte a versão do protocolo de associação Token 0,10 – em por padrão
    - Token protocolo Binding atualizado [[rascunho-ietf-tokbind-protocolo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão do TLS negociação de associação token adicionada [[rascunho-ietf-tokbind-negociação-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Suporte a SYS de associação token por HTTP atualizado [[rascunho-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versão 1703 dá suporte a versão do protocolo de associação Token 0,10 – por padrão
    - Token protocolo Binding atualizado [[rascunho-ietf-tokbind-protocolo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensão do TLS negociação de associação token adicionada [[rascunho-ietf-tokbind-negociação-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Suporte a SYS de associação token por HTTP atualizado [[rascunho-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Os dispositivos Windows com segurança baseada em virtualização habilitada manterá as chaves de associação token em um ambiente protegido que é isolado do sistema operacional em execução

Informações sobre o suporte a ASP .NET podem ser encontradas no [origem de referência do .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obter informações sobre o .NET Framework, consulte os tópicos a seguir:

- [Aperfeiçoamentos de rede](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe TokenBinding .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
