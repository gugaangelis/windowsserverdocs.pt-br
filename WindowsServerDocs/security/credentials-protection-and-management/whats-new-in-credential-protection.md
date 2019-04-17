---
title: "Novidades na proteção de credenciais"
description: "Segurança do Windows Server"
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
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>Novidades na proteção de credenciais

## <a name="credential-guard-for-signed-in-user"></a>O Credential Guard para o usuário conectado

A partir do Windows 10, versão 1507, Kerberos e NTLM usam segurança baseada em virtualização para proteger segredos Kerberos e NTLM da sessão de logon do usuário conectado. 

A partir do Windows 10, versão 1511, o Gerenciador de credenciais usa segurança baseada em virtualização para proteger credenciais salvas do tipo de credenciais de domínio. Conectado credenciais e as credenciais de domínio salvos não serão passadas para um host remoto usando a área de trabalho remota. O Credential Guard pode ser habilitado sem bloqueio UEFI.

A partir do Windows 10, versão 1607, modo de usuário isolado está incluído com o Hyper-V para que ele não seja instalado separadamente para implantação do Credential Guard.

[Saiba mais sobre o Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>O Credential Guard remoto para o usuário conectado

A partir do Windows 10, versão 1607, remoto Credential Guard protege as credenciais do usuário conectado ao usar a área de trabalho remota protegendo os segredos Kerberos e NTLM no dispositivo cliente. Para o host remoto avaliar os recursos de rede como o usuário, solicitações de autenticação exigem o dispositivo cliente para usar os segredos.

A partir do Windows 10, versão 1703, remoto Credential Guard protege as credenciais do usuário fornecido ao usar a área de trabalho remota.

[Saiba mais sobre a proteção de credenciais remoto](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Proteções de domínio

Proteções de domínio exigem um domínio do Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos de domínio para autenticação usando a chave pública

Começando com o Windows 10 versão 1507 e Windows Server 2016, se um dispositivo ingressado no domínio é capaz de registrar sua chave pública associada com um controlador de domínio do Windows Server 2016 (DC), em seguida, o dispositivo pode autenticar com a chave pública usando autenticação Kerberos PKINIT para um controlador de domínio do Windows Server 2016.

A partir do Windows Server 2016, KDCs dão suporte à autenticação usando chaves Kerberos de relação de confiança.  

[Saiba mais sobre o suporte de chave público para dispositivos que ingressaram no domínio e confiança chave Kerberos](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Suporte a extensões PKINIT grau de atualização

A partir do Windows 10, versão 1507 e Windows Server 2016, os clientes Kerberos tentará a extensão do grau de atualização PKInit para públicos chave baseado em logons. 

A partir do Windows Server 2016, KDCs pode aceitar a extensão de renovação PKInit.  Por padrão, KDCs não oferecerá a extensão de renovação PKInit. 

[Saiba mais sobre o suporte à extensão de renovação PKINIT](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Implantando pública chave somente segredos do usuário NTLM

A partir do nível funcional do domínio do Windows Server 2016 (DFL), controladores de domínio podem dar suporte a Implantando uma pública chave somente segredos do usuário NTLM. Esse recurso é unavailble em DFLs inferiores.

> [!WARNING] 
> Adicionando um controlador de domínio em um domínio com o rolamento segredos NTLM habilitados antes que o controlador de domínio foi atualizado com pelo menos 8 de novembro de 2016 manutenção executa o risco de controlador de domínio que está falhando. 

Configuração: Para novos domínios, esse recurso é habilitado por padrão. Para domínios existentes, ele deve ser configurado no Centro Administrativo do Active Directory: 

1. Do Centro Administrativo do Active Directory, clique com botão direito no domínio no painel esquerdo e selecione **propriedades**.

    ![Propriedades de domínio](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Selecione **habilitar rolamento dos segredos NTLM vencidos durante a inscrição, para os usuários que são necessários para usar o Microsoft Passport ou um cartão inteligente para logon interativo**.

    ![Segredos NTLM vencidos Autoroll](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Clique em **Okey**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitindo que a rede NTLM quando o usuário estiver restrito aos dispositivos específicos de domínio

A partir do Windows Server 2016 nível funcional do domínio (DFL), controladores de domínio pode dar suporte a rede permitindo NTLM quando um usuário está restrito a específico dispositivos ingressados em domínio. Esse recurso não está disponível em DFLs inferiores.

Configuração: A política de autenticação, clique em **selecionado de autenticação de rede permitem NTLM quando o usuário está restrito a dispositivos**. 

[Saiba mais sobre políticas de autenticação](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
