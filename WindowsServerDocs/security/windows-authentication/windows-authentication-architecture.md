---
title: Arquitetura de autenticação do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855857"
---
# <a name="windows-authentication-architecture"></a>Arquitetura de autenticação do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de visão geral para profissionais de TI explica o esquema de arquitetura básico para a autenticação do Windows.

Autenticação é o processo pelo qual o sistema valida logon ou informações de logon do usuário. Nome de um usuário e senha são comparados com uma lista de autorizados, e se o sistema detectar uma correspondência, o acesso é concedido a extensão especificada na lista de permissões para que o usuário.

Como parte de uma arquitetura extensível, os sistemas operacionais Windows Server implementam um conjunto padrão de autenticação provedores de segurança, que incluem Negotiate, o protocolo da Kerberos, NTLM, o Schannel (canal seguro) e Digest. Os protocolos usados por esses provedores permitem a autenticação de usuários, computadores e serviços, e o processo de autenticação permite que os usuários autorizados e serviços acessar recursos de maneira segura.

No Windows Server, aplicativos de autenticam usuários usando a SSPI para abstrair chamadas para autenticação. Portanto, os desenvolvedores não precisam compreender as complexidades de protocolos de autenticação específicos ou criar protocolos de autenticação em seus aplicativos.

Sistemas operacionais Windows Server incluem um conjunto de componentes de segurança que compõem o modelo de segurança do Windows. Esses componentes Certifique-se de que os aplicativos não é possível obter acesso aos recursos sem autenticação e autorização. As seções a seguir descrevem os elementos da arquitetura de autenticação.

### <a name="local-security-authority"></a>Autoridade de Segurança Local
A autoridade de segurança Local (LSA) é um subsistema protegido que autentica e conecta os usuários no computador local. Além disso, a LSA mantém informações sobre todos os aspectos de segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local). Ele também fornece vários serviços para conversão entre os nomes e identificadores de segurança (SIDs).

O subsistema de segurança controla de políticas de segurança e as contas que estão em um computador. No caso de um domínio, controlador, essas políticas e as contas são aqueles que estão em vigor para o domínio no qual o controlador de domínio está localizado. Essas políticas e as contas são armazenadas no Active Directory. O subsistema LSA fornece serviços para validar o acesso a objetos, a verificação de direitos de usuário e gerar mensagens de auditoria.

### <a name="security-support-provider-interface"></a>Security Support Provider Interface
A Interface de provedor de suporte de segurança (SSPI) é a API que obtém os serviços de segurança integrada para autenticação, integridade de mensagens, mensagem privacidade e qualidade de serviço de segurança para qualquer protocolo de aplicativo distribuído.

SSPI é a implementação do genérico segurança serviço GSSAPI (API). SSPI fornece um mecanismo pelo qual um aplicativo distribuído pode chamar um dos vários provedores de segurança para obter uma conexão autenticada sem o conhecimento dos detalhes do protocolo de segurança.

## <a name="see-also"></a>Consulte também

-   [Arquitetura de Interface de provedor de suporte de segurança](security-support-provider-interface-architecture.md)

-   [Processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Visão geral técnica de autenticação do Windows](https://technet.microsoft.com/library/dn169029.aspx)


