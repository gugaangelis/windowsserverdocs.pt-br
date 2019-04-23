---
title: Novidades na proteção de credenciais
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ec41e85949cb61c8130d8765b4786eefe39ebd0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855587"
---
# <a name="whats-new-in-credential-protection"></a>Novidades na proteção de credenciais

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard para o usuário conectado

Começando com o Windows 10 versão 1507, Kerberos e NTLM usam segurança baseada em virtualização para proteger segredos do Kerberos e NTLM da sessão de logon do usuário conectado. 

Começando com o Windows 10 versão 1511, o Gerenciador de credenciais usa segurança baseada em virtualização para proteger as credenciais salvas do tipo de credencial de domínio. As credenciais de logon e credenciais de domínio salvas não serão passadas para um host remoto usando a área de trabalho remota. Credential Guard pode ser habilitada sem bloqueio UEFI.

Começando com o Windows 10, versão 1607, modo de usuário isolado é incluído com o Hyper-V para que ele não é instalado separadamente para a implantação do Credential Guard.

[Saiba mais sobre o Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Remote Credential Guard para o usuário conectado

Começando com o Windows 10, versão 1607, remoto do Credential Guard protege as credenciais do usuário conectado ao usar a área de trabalho remota, protegendo os segredos de Kerberos e NTLM no dispositivo cliente. Para o host remoto avaliar os recursos de rede como o usuário, as solicitações de autenticação exigem que o dispositivo cliente para usar os segredos.

Começando com o Windows 10, versão 1703, remoto do Credential Guard protege as credenciais de usuário fornecido ao usar a área de trabalho remota.

[Saiba mais sobre o credential guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Proteções de domínio

As proteções de domínio exigem um domínio do Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos ingressados no domínio para autenticação usando a chave pública

Começando com o Windows 10 versão 1507 e Windows Server 2016, se um dispositivo ingressado no domínio é capaz de registrar sua chave pública associado um controlador de domínio (DC) do Windows Server 2016, em seguida, o dispositivo pode autenticar com a chave pública usando Kerberos PKINIT autenticação para um controlador de domínio do Windows Server 2016.

Começando com o Windows Server 2016, os KDCs suportam a autenticação usando Kerberos confiança de chave.  

[Saiba mais sobre o suporte de chave público para dispositivos ingressados no domínio e confiança de chaves Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Suporte à extensão de atualização de PKINIT

Começando com o Windows 10, versão 1507 e Windows Server 2016, os clientes do Kerberos tentará a extensão de atualização de PKInit para público chave baseados em logons. 

Começando com o Windows Server 2016, os KDCs podem dar suporte a extensão de atualização de PKInit.  Por padrão, os KDCs não oferecerá a extensão de atualização de PKInit. 

[Saiba mais sobre o suporte de extensão de atualização PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Sem interrupção pública chave única segredos do usuário NTLM

Começando com o nível funcional de domínio do Windows Server 2016 (DFL), controladores de domínio podem dar suporte sem interrupção de um público chave única segredos do usuário NTLM. Esse recurso é unavailble no DFLs inferiores.

> [!WARNING] 
> Adicionando um controlador de domínio a um domínio com sem interrupção segredos NTLM habilitados antes que o controlador de domínio foi atualizado com pelo menos 8 de novembro de 2016 manutenção é executado o risco do travamento do controlador de domínio. 

Configuração: Para novos domínios, esse recurso é habilitado por padrão. Para domínios existentes, ele deve ser configurado no Centro Administrativo do Active Directory: 

1. Do Centro Administrativo do Active Directory, o domínio no painel esquerdo e selecione **propriedades**.

    ![Propriedades do domínio](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Selecione **habilitar a distribuição dos segredos prestes a expirar de NTLM durante a inscrição, para os usuários que são necessários para usar o Microsoft Passport ou cartão inteligente para logon interativo**.

    ![Segredos NTLM expirando Autoroll](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Clique em **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitindo que a rede NTLM quando o usuário é restrito aos dispositivos de domínio específicos

A partir do Windows Server 2016 nível funcional de domínio (DFL), controladores de domínio pode dar suporte a rede, permitindo que NTLM quando um usuário é restrito a específicos de dispositivos ingressados no domínio. Esse recurso não está disponível em DFLs inferiores.

Configuração: Na política de autenticação, clique em **dispositivos selecionados de autenticação de rede permitir NTLM quando o usuário é restrito ao**. 

[Saiba mais sobre as políticas de autenticação](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
