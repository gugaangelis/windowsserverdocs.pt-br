---
title: Impedindo que o Kerberos altere a senha que usa chaves de segredo RC4
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
author: justinha
ms.author: Justinha
ms.date: 11/09/2016
ms.openlocfilehash: a98e5e6ed62f4a43ca5e36af7051e9ece106c074
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078553"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Evitando que o Kerberos altere senhas que usam chaves secretas RC4

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Este tópico para o profissional de ti explica algumas limitações no protocolo Kerberos que podem levar a um usuário mal-intencionado assumindo o controle da conta de um usuário. Há uma limitação no padrão v5 (serviço de autenticação de rede) Kerberos (RFC 4120), que é conhecido no setor, no qual um invasor pode autenticar como um usuário ou alterar a senha do usuário se o invasor souber a chave secreta do usuário.

A posse de chaves de segredo Kerberos derivadas de senha de um usuário (RC4 e criptografia AES [AES] por padrão) é validada durante a troca de alteração de senha do Kerberos por RFC 4757. A senha de texto sem formatação do usuário nunca é fornecida para o centro de distribuição de chaves (KDC) e, por padrão, Active Directory controladores de domínio não possuem uma cópia de senhas de texto não criptografado para contas. Se o controlador de domínio não oferecer suporte a um tipo de criptografia Kerberos, essa chave secreta não poderá ser usada para alterar a senha.

Nos sistemas operacionais Windows designados na lista aplica-se a ao início deste tópico, há três maneiras de bloquear a capacidade de alterar as senhas usando o Kerberos com chaves secretas RC4:

- Configure a conta de usuário para incluir a opção de conta o cartão inteligente é necessário para logon interativo. Isso limita o usuário a entrar apenas com um cartão inteligente válido para que as solicitações do serviço de autenticação RC4 (AS-requisitos) sejam rejeitadas. Para definir as opções de conta em uma conta, clique com o botão direito do mouse na conta, clique em Propriedades e clique na guia conta.

- Desabilite o suporte RC4 para Kerberos em todos os controladores de domínio. Isso exige um mínimo de um nível funcional de domínio do Windows Server 2008 e um ambiente em que todos os clientes Kerberos, servidores de aplicativos e relações de confiança de e para o domínio devem oferecer suporte a AES. O suporte para AES foi introduzido no Windows Server 2008 e no Windows Vista.

    [!NOTE]
    Há um problema conhecido com a desabilitação de RC4, que pode fazer com que o sistema seja reiniciado. Consulte os seguintes hotfixes:
    - [Windows Server 2012 R2](https://support.microsoft.com/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/kb/3086213)
    - Nenhum hotfix está disponível para versões anteriores do Windows Server

- Implante domínios definidos para o nível funcional de domínio do Windows Server 2012 R2 ou superior e configure os usuários como membros do grupo de segurança usuários protegidos. Como esse recurso interrompe mais do que apenas o uso de RC4 no protocolo Kerberos, consulte os recursos na seção [Consulte também](#see-also) a seguir.

## <a name="see-also"></a>Consulte Também

- Para obter informações sobre como evitar o uso do tipo de criptografia RC4 em domínios do Windows Server 2012 R2, consulte [grupo de segurança usuários protegidos](/../credentials-protection-and-management/protected-users-security-group.md)e [como configurar contas protegidas](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obter explicações sobre RFC 4120 e RFC 4757, consulte [documentos de IETF](http://tools.ietf.org/html/).
