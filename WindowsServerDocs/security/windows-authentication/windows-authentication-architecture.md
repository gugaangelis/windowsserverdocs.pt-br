---
title: Arquitetura de autenticação do Windows
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 7f2ad45a12f8542e4af9a77a9dc76a9596df2143
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638719"
---
# <a name="windows-authentication-architecture"></a>Arquitetura de autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de visão geral do profissional de ti explica o esquema de arquitetura básica para a autenticação do Windows.

A autenticação é o processo pelo qual o sistema valida as informações de logon ou entrada de um usuário. O nome e a senha de um usuário são comparados com uma lista autorizada e, se o sistema detectar uma correspondência, o acesso será concedido à extensão especificada na lista de permissões para esse usuário.

Como parte de uma arquitetura extensível, os sistemas operacionais Windows Server implementam um conjunto padrão de provedores de suporte de segurança de autenticação, que incluem Negotiate, o protocolo Kerberos, NTLM, Schannel (canal seguro) e Digest. Os protocolos usados por esses provedores permitem a autenticação de usuários, computadores e serviços, e o processo de autenticação permite que usuários e serviços autorizados acessem recursos de maneira segura.

No Windows Server, os aplicativos autenticam usuários usando as chamadas SSPI para abstratas para autenticação. Portanto, os desenvolvedores não precisam entender as complexidades de protocolos de autenticação específicos ou criar protocolos de autenticação em seus aplicativos.

Os sistemas operacionais Windows Server incluem um conjunto de componentes de segurança que compõem o modelo de segurança do Windows. Esses componentes garantem que os aplicativos não possam obter acesso aos recursos sem autenticação e autorização. As seções a seguir descrevem os elementos da arquitetura de autenticação do.

### <a name="local-security-authority"></a>Autoridade de Segurança Local
A autoridade de segurança local (LSA) é um subsistema protegido que autentica e assina usuários no computador local. Além disso, a LSA mantém informações sobre todos os aspectos da segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local). Ele também fornece vários serviços para conversão entre nomes e SIDs (identificadores de segurança).

O subsistema de segurança controla as políticas de segurança e as contas que estão em um sistema de computador. No caso de um controlador de domínio, essas políticas e contas são aquelas que estão em vigor para o domínio no qual o controlador de domínio está localizado. Essas políticas e contas são armazenadas em Active Directory. O subsistema LSA fornece serviços para validar o acesso a objetos, verificar os direitos de usuário e gerar mensagens de auditoria.

### <a name="security-support-provider-interface"></a>Interface do provedor de suporte de segurança
A interface de provedor de suporte de segurança (SSPI) é a API que obtém serviços de segurança integrados para autenticação, integridade de mensagem, privacidade de mensagem e qualidade de serviço de segurança para qualquer protocolo de aplicativo distribuído.

SSPI é a implementação da API de serviço de segurança genérica (GSSAPI). O SSPI fornece um mecanismo pelo qual um aplicativo distribuído pode chamar um de vários provedores de segurança para obter uma conexão autenticada sem conhecimento dos detalhes do protocolo de segurança.

## <a name="additional-references"></a>Referências adicionais

-   [Arquitetura de Interface SSPI](security-support-provider-interface-architecture.md)

-   [Processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md)

-   [Visão geral técnica de autenticação do Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169029(v=ws.10))