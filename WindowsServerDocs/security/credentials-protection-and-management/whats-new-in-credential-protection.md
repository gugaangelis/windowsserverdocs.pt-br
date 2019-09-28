---
title: O que há de novo na proteção de credenciais
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 2351be82ad1d8b9af17715ce363836f57c71ea66
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386914"
---
# <a name="whats-new-in-credential-protection"></a>O que há de novo na proteção de credenciais

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard para usuário conectado

A partir do Windows 10, a versão 1507, o Kerberos e o NTLM usam segurança baseada em virtualização para proteger os segredos Kerberos & NTLM da sessão de logon do usuário conectado. 

A partir do Windows 10, versão 1511, o Credential Manager usa a segurança baseada em virtualização para proteger as credenciais salvas do tipo de credencial do domínio. As credenciais conectadas e as credenciais de domínio salvas não serão passadas para um host remoto usando a área de trabalho remota. O Credential Guard pode ser habilitado sem o bloqueio UEFI.

A partir do Windows 10, versão 1607, o modo de usuário isolado está incluído no Hyper-V, portanto, ele não é mais instalado separadamente para a implantação do Credential Guard.

[Saiba mais sobre o Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Remote Credential Guard para usuário conectado

A partir do Windows 10, versão 1607, o Remote Credential Guard protege as credenciais do usuário conectado ao usar o Área de Trabalho Remota protegendo os segredos Kerberos e NTLM no dispositivo cliente. Para que o host remoto avalie os recursos de rede como o usuário, as solicitações de autenticação exigem que o dispositivo cliente use os segredos.

A partir do Windows 10, versão 1703, o Remote Credential Guard protege as credenciais de usuário fornecidas ao usar o Área de Trabalho Remota.

[Saiba mais sobre o Remote Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Proteções de domínio

As proteções de domínio exigem um domínio Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos ingressados no domínio para autenticação usando chave pública

A partir do Windows 10 versão 1507 e do Windows Server 2016, se um dispositivo ingressado no domínio for capaz de registrar sua chave pública vinculada com um controlador de domínio do Windows Server 2016 (DC), o dispositivo poderá autenticar com a chave pública usando o Kerberos PKINIT autenticação para um controlador de domínio do Windows Server 2016.

A partir do Windows Server 2016, o KDCs dá suporte à autenticação usando a confiança de chave Kerberos.  

[Saiba mais sobre o suporte de chave pública para dispositivos ingressados no domínio & confiança de chave Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Suporte à extensão de atualização de PKINIT

A partir do Windows 10, versão 1507 e Windows Server 2016, os clientes Kerberos tentarão a extensão de atualização de PKInit para os logons baseados em chave pública. 

A partir do Windows Server 2016, o KDCs pode dar suporte à extensão de atualização PKInit.  Por padrão, o KDCs não oferecerá a extensão de atualização PKInit. 

[Saiba mais sobre o suporte à extensão de atualização de PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Segredos de NTLM do usuário de chave pública sem interrupção

A partir do DFL (nível funcional de domínio) do Windows Server 2016, os DCs podem dar suporte à distribuição de segredos de NTLM de um usuário de chave pública. Esse recurso é unavailble no DFLs inferior.

> [!WARNING] 
> Adicionar um controlador de domínio a um domínio com segredos NTLM sem interrupção habilitados antes que o DC seja atualizado com pelo menos o 8 de novembro de 2016, a manutenção do serviço corre o risco de falha do DC. 

Configuração: Para novos domínios, esse recurso é habilitado por padrão. Para domínios existentes, ele deve ser configurado no centro administrativo Active Directory: 

1. No centro administrativo Active Directory, clique com o botão direito do mouse no domínio no painel esquerdo e selecione **Propriedades**.

    ![Propriedades do domínio](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Selecione **habilitar a interrupção dos segredos de NTLM expirando durante o logon, para os usuários que precisam usar Microsoft Passport ou cartão inteligente para logon interativo**.

    ![Autolance expirando segredos NTLM](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Clique em **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitindo a rede NTLM quando o usuário está restrito a dispositivos ingressados no domínio específico

A partir do DFL (nível funcional de domínio) do Windows Server 2016, os DCs podem dar suporte à permissão de NTLM de rede quando um usuário está restrito a dispositivos ingressados no domínio específico. Este recurso não está disponível no DFLs inferior.

Configuração: Na política de autenticação, clique em **Permitir autenticação de rede NTLM quando o usuário estiver restrito a dispositivos selecionados**. 

[Saiba mais sobre as políticas de autenticação](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
