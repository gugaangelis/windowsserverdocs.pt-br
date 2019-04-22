---
title: Impedindo Kerberos alterar a senha que usam chaves secretas do RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816267"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Evitando que o Kerberos altere senhas que usam chaves secretas RC4

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Este tópico para profissionais de TI explica algumas limitações no protocolo Kerberos que poderiam levar a um usuário mal-intencionado assumir o controle de uma conta de usuário. Há uma limitação no padrão de serviço de autenticação de rede Kerberos (V5) (RFC 4120), que é conhecido no setor de, no qual um invasor pode autenticar como um usuário ou alterar a senha do usuário se o invasor sabe a chave secreta do usuário.

A posse de derivado de senha Kerberos chaves secretas um usuário (RC4 e AES Advanced Encryption Standard [] por padrão) é validado durante a troca de alteração de senha Kerberos por RFC 4757. A senha do usuário em texto sem formatação nunca é fornecida para o Centro de distribuição de chaves (KDC) e, por padrão, os controladores de domínio do Active Directory não possuir uma cópia de senhas de texto sem formatação para contas. Se o controlador de domínio não dá suporte a um tipo de criptografia Kerberos, essa chave secreta não pode ser usado para alterar a senha. 

Nos sistemas operacionais Windows designados na lista se aplica a no início deste tópico, há três maneiras de bloquear a capacidade de alterar as senhas usando o Kerberos com chaves secretas do RC4:

- Configurar o usuário de conta para incluir a opção de conta de cartão inteligente é necessária para logon interativo. Isso limita o usuário a entrar apenas com um cartão inteligente válido, para que as solicitações de serviço de autenticação RC4 (AS-REQs) são rejeitadas. Para definir as opções de conta em uma conta, clique com botão direito na conta, clique em propriedades e clique na guia conta. 

- Desabilite o suporte de RC4 para Kerberos em todos os controladores de domínio. Isso requer um mínimo de um nível funcional de domínio do Windows Server 2008 e um ambiente em que todos os clientes, servidores de aplicativos e as relações de confiança de e para o domínio do Kerberos devem dar suporte a AES. Suporte a AES foi introduzido no Windows Server 2008 e Windows Vista.

    [!NOTE]
    Há um problema conhecido com a desabilitar o RC4, que pode causar o reinicialização do sistema. Consulte os seguintes hotfixes:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Nenhum hotfix está disponível para versões anteriores do Windows Server

- Implantar o conjunto de domínios para o nível funcional de domínio do Windows Server 2012 R2 ou superior e configurar os usuários como membros do grupo de segurança usuários protegidos. Porque esse recurso interrompe mais do que apenas o uso de RC4 no protocolo Kerberos, consulte os recursos a seguir [Consulte também](#see-also) seção.

## <a name="see-also"></a>Consulte também

- Para obter informações sobre como evitar o uso do tipo de criptografia RC4 em domínios do Windows Server 2012 R2, consulte [grupo de segurança usuários protegidos](/../credentials-protection-and-management/protected-users-security-group.md), e [How to Configure Protected Accounts](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obter explicações sobre RFC 4120 e RFC 4757, consulte [documentos da IETF](http://tools.ietf.org/html/).
