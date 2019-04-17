---
title: "Arquitetura de autenticação do Windows"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="windows-authentication-architecture"></a>Arquitetura de autenticação do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de visão geral para o profissional de TI explica o esquema de arquitetura básico para autenticação do Windows.

Autenticação é o processo pelo qual o sistema valida logon ou informações de entrada do usuário. Nome do usuário e senha são comparados com uma lista autorizada e se o sistema detecta uma correspondência, o acesso é concedido até o limite especificado na lista de permissões para esse usuário.

Como parte de uma arquitetura extensível, os sistemas operacionais Windows Server implementar um conjunto padrão de provedores de suporte de segurança autenticação, que incluem negociar, o protocolo da Kerberos, NTLM, Schannel (canal seguro) e Digest. Os protocolos usados por esses provedores de habilitar a autenticação de usuários, computadores e serviços, e o processo de autenticação permite que os usuários autorizados e serviços acessar recursos de maneira segura.

No Windows Server, aplicativos autenticam usuários usando a SSPI para abstrair chamadas para autenticação. Assim, os desenvolvedores não precisam de compreender as complexidades dos protocolos de autenticação específicos ou crie protocolos de autenticação em seus aplicativos.

Sistemas operacionais Windows Server incluem um conjunto de componentes de segurança que compõem o modelo de segurança do Windows. Esses componentes garantem que os aplicativos não podem obter acesso aos recursos sem autenticação e autorização. As seções a seguir descrevem os elementos da arquitetura de autenticação.

### <a name="local-security-authority"></a>Autoridade de segurança local
A autoridade de segurança Local (LSA) é um subsistema protegido que autentica e entra em usuários no computador local. Além disso, a LSA mantém informações sobre todos os aspectos de segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local). Ele também fornece vários serviços para conversão entre nomes e identificadores de segurança (SIDs).

O subsistema de segurança mantém o controle das políticas de segurança e as contas que estão em um sistema de computador. No caso de um domínio, controlador, essas políticas e contas são aqueles que estão em vigor para o domínio em que o controlador de domínio está localizado. Essas políticas e contas são armazenadas no Active Directory. O subsistema LSA fornece serviços para validar o acesso a objetos, verificando os direitos do usuário e geração de mensagens de auditoria.

### <a name="security-support-provider-interface"></a>Interface do provedor de suporte de segurança
A Interface de provedor de suporte de segurança (SSPI) é a API que obtém os serviços de segurança integrada para autenticação, integridade da mensagem, privacidade da mensagem e qualidade de serviço de segurança para qualquer protocolo de aplicativo distribuído.

SSPI é a implementação da API de serviço de segurança (GSSAPI) genérico. SSPI fornece um mecanismo pelo qual um aplicativo distribuído pode chamar um dos vários provedores de segurança para obter uma conexão autenticada sem o conhecimento dos detalhes do protocolo de segurança.

## <a name="see-also"></a>Consulte também

-   [Arquitetura de Interface do provedor de suporte de segurança](security-support-provider-interface-architecture.md)

-   [Processos de credenciais de autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Visão geral técnica de autenticação do Windows](https://technet.microsoft.com/library/dn169029.aspx)


