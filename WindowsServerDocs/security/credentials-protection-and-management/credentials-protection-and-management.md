---
title: "Gerenciamento e proteção de credenciais"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>Gerenciamento e proteção de credenciais

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI discute os recursos e métodos introduzidos no Windows Server 2012 R2 e no Windows 8.1 para proteção de credenciais e controles de autenticação de domínio para reduzir o roubo de credenciais.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modo de administrador restrito para Conexão de área de trabalho remota
Modo administrador restrito fornece um método de forma interativa fazer logon em um servidor host remoto sem transmitir suas credenciais para o servidor. Isso impede que suas credenciais sendo coletados durante o processo de conexão inicial, se o servidor foi comprometido.

Usando esse modo com credenciais de administrador, o cliente de área de trabalho remota tenta fazer logon interativamente um host que também dá suporte a esse modo sem enviar as credenciais. Quando o host verifica se a conta de usuário se conectar a ele com direitos de administrador e dá suporte ao modo administrador restrito, a conexão tiver êxito. Caso contrário, a tentativa de conexão falhar. Modo administrador restrito faz não em qualquer enviar mensagem ponto de texto sem formatação ou outras formas reutilizáveis de credenciais para computadores remotos.

### <a name="lsa-protection"></a>Proteção da LSA
A LSA Local Security Authority (), que reside dentro do processo de serviço de segurança de autoridade de segurança Local (LSASS), valida os usuários para locais e remotos acessos e impõe políticas de segurança local. O sistema operacional Windows 8.1 fornece proteção adicional para a LSA evitar a injeção de código por processos não protegidos. Isso proporciona segurança adicional para as credenciais que a LSA armazena e gerencia. Essa configuração de processo protegido para LSA pode ser configurada no Windows 8.1, mas é ativado por padrão no Windows RT 8.1 e não pode ser alterada.

Para obter informações sobre como configurar a proteção de LSA, consulte [configurando proteção adicional de LSA](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Grupo de segurança de usuários protegido
Esse novo grupo global de domínio dispara nova proteção não configuráveis em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1. O grupo usuários protegido permite proteções adicionais para controladores de domínio e em domínios em domínios do Windows Server 2012 R2. Isso reduz significativamente os tipos de credenciais disponíveis quando os usuários são registradas em log em computadores na rede de um computador não seja comprometida.

Membros do grupo usuários protegidos são limitados mais pelos seguintes métodos de autenticação:

-   Um membro do grupo usuários protegido só pode entrar no usando o protocolo Kerberos. A conta não poderá autenticar usando NTLM, a autenticação Digest ou CredSSP. Em um dispositivo executando o Windows 8.1, as senhas não são armazenadas em cache, para que o dispositivo que usa qualquer um desses provedores de suporte de segurança (SSPs) falhará se autenticar em um domínio quando a conta é um membro do grupo de usuários protegido.

-   O protocolo Kerberos não usará os tipos de criptografia DES ou RC4 mais fracos no processo de pré-autenticação. Isso significa que o domínio deve ser configurado para dar suporte pelo menos o AES criptográfico suite.

-   A conta do usuário não pode ser recebida com Kerberos restrita ou estão irrestritos delegação. Isso significa que o primeiro conexões com outros sistemas podem falhar se o usuário é um membro do grupo usuários protegidos.

-   A configuração de tempo de vida de tíquetes de concessão de tíquete Kerberos (TGTs) padrão de quatro horas é configurável usando políticas de autenticação e Silos acessados por meio do Active Directory administrativas central (ADAC). Isso significa que, após quatro horas, o usuário deve autenticar novamente.

> [!WARNING]
> Contas de computadores e serviços não devem ser membros do grupo usuários protegido. Esse grupo não fornece nenhuma proteção local porque a senha ou certificado sempre está disponível no host. Autenticação falhará com o erro "o nome de usuário ou a senha está incorreta" para qualquer serviço ou um computador que é adicionado ao grupo usuários protegido.

Para obter mais informações sobre esse grupo, consulte [protegido por grupo de segurança de usuários](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Política de autenticação e Silos de política de autenticação
As políticas com base em floresta do Active Directory são introduzidas, e elas podem ser aplicadas a contas em um domínio com um nível funcional de domínio do Windows Server 2012 R2. Essas políticas de autenticação podem controlar quais hosts que um usuário pode usar para fazer logon. Elas funcionam em conjunto com o grupo de segurança de proteger usuários e administradores podem aplicar condições de controle de acesso para autenticação a contas. Essas políticas de autenticação isolam contas relacionadas para restringir o escopo de uma rede.

A nova classe de objeto do Active Directory, política de autenticação, permite que você aplique a configuração de autenticação para classes de conta em domínios com um nível funcional de domínio do Windows Server 2012 R2. Políticas de autenticação são impostas durante AS o Kerberos ou o TGS exchange. Classes de conta do Active Directory são:

-   Usuário

-   Computador

-   Conta de serviço gerenciado

-   Conta de grupo serviço gerenciado

Para obter mais informações, consulte [políticas de autenticação e Silos de política de autenticação](authentication-policies-and-authentication-policy-silos.md).

Para saber mais como configurar protegido contas, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Consulte também
Para saber mais sobre o LSA e o LSASS, consulte o [Logon do Windows e visão geral técnica das autenticação](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



