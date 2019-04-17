---
title: Evitando Kerberos alterar a senha que usam chaves secretas RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Evitando Kerberos alterar senha que usa chaves secretas RC4

>Aplica-se a: Windows Server (anual por canal) Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Este tópico para o profissional de TI explica algumas limitações no protocolo Kerberos que pode levar a um usuário mal-intencionado assuma o controle da conta do usuário. Há um limite no padrão de serviço de autenticação Kerberos rede (V5) (RFC 4120), que é conhecido no setor de, pelo qual um invasor pode autenticar como um usuário ou alterar a senha do usuário se o invasor souber a chave secreta do usuário.

Posse de derivados de senha chaves secretas Kerberos um usuário (RC4 e AES Advanced Encryption Standard [] por padrão) é validado durante a troca de alteração de senha Kerberos por RFC 4757. A senha do usuário em texto sem formatação nunca é fornecida para o Centro de distribuição de chaves (KDC) e, por padrão, os controladores de domínio do Active Directory não possuem uma cópia de senhas de texto sem formatação para contas. Se o controlador de domínio não dá suporte a um tipo de criptografia Kerberos, essa chave secreta não pode ser usado para alterar a senha. 

Nos sistemas operacionais Windows indicados na lista Aplica-se a no início deste tópico, há três maneiras de bloquear a capacidade de alterar as senhas usando Kerberos com chaves secretas RC4:

- Configurar o usuário conta para incluir a opção de conta cartão inteligente é necessária para logon interativo. Isso limita o usuário para registro em log apenas com um cartão inteligente válido para que as solicitações de serviço de autenticação RC4 (-REQs Kerberos) serão rejeitadas. Para definir as opções de conta em uma conta, clique com botão direito na conta, clique em propriedades e clique na guia conta. 

- Desabilite o suporte a RC4 Kerberos em todos os controladores de domínio. Isso requer um mínimo de um nível funcional de domínio do Windows Server 2008 e um ambiente onde todos os clientes, servidores de aplicativos e relações de confiança para e do domínio Kerberos devem oferecer suporte AES. Suporte para AES foi introduzido no Windows Server 2008 e Windows Vista.

    [!NOTE]
    Há um problema conhecido com a desativação RC4 que pode fazer com que o sistema seja reinicializado. Consulte os seguintes hotfixes:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Nenhum hotfix está disponível para versões anteriores do Windows Server

- Implantar o conjunto de domínios de nível funcional de domínio do Windows Server 2012 R2 ou superior e configurar os usuários como membros do grupo de segurança usuários protegido. Porque esse recurso atrapalha RC4 mais do que apenas o uso no protocolo Kerberos, consulte recursos nos seguintes [veja também](#see-also) seção.

## <a name="see-also"></a>Consulte também

- Para obter informações sobre como evitar o uso do tipo de criptografia RC4 em domínios do Windows Server 2012 R2, consulte [protegido por grupo de segurança de usuários](/../credentials-protection-and-management/protected-users-security-group.md), e [como configurar contas protegido](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obter explicações sobre RFC 4120 e RFC 4757, consulte [IETF documentos](http://tools.ietf.org/html/).
