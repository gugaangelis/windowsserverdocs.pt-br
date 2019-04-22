---
title: Proteção e gerenciamento de credenciais
description: Segurança do Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812197"
---
# <a name="credentials-protection-and-management"></a>Proteção e gerenciamento de credenciais

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI discute recursos e métodos introduzidos no Windows Server 2012 R2 e Windows 8.1 para proteção de credenciais e controles de autenticação de domínio para reduzir o roubo de credencial.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modo Administrador Restrito para a Conexão de Área de Trabalho Remota
O modo Administrador Restrito fornece um método de efetuar logon interativamente a um servidor host remoto sem transmitir suas credenciais ao servidor. Isso previne que suas credenciais sejam coletadas durante o processo de conexão inicial, caso o servidor tenha sido comprometido.

Usar esse modo com as credenciais de administrador, o cliente da área de trabalho remota tenta efetuar logon interativamente a um host que também dê suporte a esse modo, sem enviar as credenciais. Quando o host verifica que a conta de usuário que está se conectando a ele possui direitos de administrador e dá suporte ao modo Administrador Restrito, a conexão é bem-sucedida. Caso contrário, a tentativa de conexão falha. O modo Administrador Restrito, em nenhum ponto, envia um texto completo ou outros formatos reutilizáveis de credenciais para computadores remotos.

### <a name="lsa-protection"></a>Proteção de LSA
O LSA (Autoridade de Segurança Local), que fica dentro do processo LSASS, valida os usuários para credenciais locais e remotas e impõe políticas de segurança local. O sistema operacional Windows 8.1 fornece proteção adicional ao LSA prevenir injeção de código por processos não protegidos. Isso proporciona mais segurança para as credenciais que a LSA armazena e gerencia. Essa configuração de processo protegida para LSA pode ser configurada no Windows 8.1, mas é ativado por padrão no Windows RT 8.1 e não pode ser alterada.

Para obter informações sobre como configurar a proteção de LSA, confira [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Grupo de segurança de Usuários Protegidos
Esse novo grupo global de domínios dispara novas proteções não configuráveis em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1. O grupo usuários protegidos permite proteções adicionais para controladores de domínio e domínios nos domínios do Windows Server 2012 R2. Isso reduz consideravelmente os tipos de credenciais disponíveis quando os usuários são registrados em computadores na rede, em um computador não comprometido.

Os membros do grupo Usuários Protegidos são limitados ainda pelos seguintes métodos de autenticação:

-   Um membro do grupo Usuários Protegidos só pode se registrar usando o protocolo Kerberos. A conta não pode ser autenticada usando NTLM, Autenticação Digest ou CredSSP. Em um dispositivo executando o Windows 8.1, as senhas não estão em cache, portanto, o dispositivo que usa qualquer um desses provedores de suporte de segurança (SSPs) falhará ao autenticar em um domínio quando a conta é membro do grupo de usuários protegidos.

-   O protocolo Kerberos não usará os tipos de criptografia DES ou RC4 mais fracos no processo de pré-autenticação. Isso significa que o domínio deve ser configurado para dar suporte ao menos ao suite criptográfico AES.

-   A conta do usuário não pode ser delegada com Kerberos restrita ou irrestrita delegação. Isso significa que as conexões antigas aos outros sistemas podem falhar, caso o usuário seja um membro do grupo Usuários Protegidos.

-   A configuração de tempo de vida dos TGTs (Tíquetes de Concessão de Tíquete) Kerberos padrão de quatro horas é configurável usando as Políticas de Autenticação e Silos acessados por meio do ADAC (Centro Administrativo do Active Directory). Isso significa que passadas quatro horas, o usuário deve se autenticar novamente.

> [!WARNING]
> Contas de serviços e computadores não devem ser membros do grupo Usuários protegidos. Este grupo não oferece proteção local, pois a senha ou o certificado sempre está disponível no host. A autenticação falhará com o erro "o nome de usuário ou senha está incorreta" para qualquer serviço ou um computador que é adicionado ao grupo usuários protegidos.

Para obter mais informações sobre esse grupo, consulte [Grupo de segurança Usuários Protegidos](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Política de Autenticação e Silos de Política de Autenticação
Políticas do Active Directory baseadas em florestas são introduzidas e podem ser aplicadas a contas em um domínio com um nível funcional de domínio do Windows Server 2012 R2. Essas políticas de autenticação podem controlar quais hosts que um usuário pode usar para entrar. Elas trabalham em conjunto com o grupo de segurança Usuários Protegidos e os administradores podem aplicar condições de controle de acesso para autenticação às contas. Essas políticas de autenticação isolam contas relacionadas para limitar o escopo de uma rede.

A nova classe de objeto do Active Directory, política de autenticação, permite que você aplicar a configuração de autenticação para as classes da conta em domínios com um nível funcional de domínio do Windows Server 2012 R2. As políticas de autenticação são impostas durante o compartilhamento de Kerberos AS ou TGS. As classes da conta do Active Directory são:

-   User

-   Computador

-   Conta de Serviço Gerenciado

-   Conta de Serviço Gerenciado de Grupo

Para obter mais informações, consulte [Políticas de Autenticação e Silos de Política de Autenticação](authentication-policies-and-authentication-policy-silos.md).

Para obter mais informações sobre como configurar as contas protegidas, consulte [Como configurar contas protegidas](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Consulte também
Para obter mais informações sobre o LSA e o LSASS, confira a [Visão geral técnica de autenticação e Logon do Windows](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



