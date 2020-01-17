---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: Operações do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2db9cd83ed08673835a38e443e90c5eb092f43ac
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162477"
---
# <a name="ad-fs-operations"></a>Operações do AD FS



Este documento contém uma lista de todas as operações de documentação para AD FS. 

## <a name="service-configuration"></a>Configuração do serviço
- [Atualizar certificados SSL em AD FS e WAP 2016](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [Ferramenta de restauração rápida do AD FS](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [Configurar a associação de nome de host alternativo para autenticação de certificado no AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Adicionar um repositório de atributos](../ad-fs/operations/Add-an-Attribute-Store.md)
- [Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS 2019](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [Delegar acesso do Powershell Commandlet do AD FS para usuários não administradores](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [Ajuste fino do SQL e da latência de endereço](../ad-fs/operations/adfs-sql-latency.md)
- [Grupos de Disponibilidade AlwaysOn](../ad-fs/operations/ad-fs-always-on.md) 


## <a name="authentication-configuration"></a>Configuração de autenticação
### <a name="strong-authentication-mfa--password-less"></a>Autenticação forte (MFA) & sem senha
- [Configurar provedores de autenticação externa como primário no AD FS (2019 ou posterior)](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [Configurar AD FS (2016 ou posterior) e Azure MFA](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [Configurar métodos de autenticação adicionais para AD FS](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>Proteção contra bloqueio
- [Configurar a proteção de bloqueio suave da extranet do AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [Configurar a proteção de bloqueio inteligente da extranet do AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar IPs proibidos da extranet do AD FS](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>Configuração de política
- [Configurar políticas de autenticação](../ad-fs/operations/Configure-Authentication-Policies.md)
- [Como configurar a ID de logon alternativa](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [Configurar o comportamento do prompt de logon do Azure AD para trabalhar com a política de AD FS](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Autenticação de certificado Kerberos &
- [Habilitar declarações de AD DS & autenticação composta Kerberos no AD FS](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [Configurar AD FS para autenticação de certificado de usuário](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [Configurar a associação de nome de host alternativo para autenticação de certificado no AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>Dispositivo
- [Controles de autenticação de dispositivo no AD FS](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>Configuração de autorização
- [Configurar políticas de controle de acesso no AD FS](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [Configurar acesso condicional baseado em dispositivo local](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>Configuração de CPT de & de relatório
- [Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [Configurar regras de declaração](../ad-fs/operations/Configure-Claim-Rules.md) 
- [Criar uma relação de confiança do provedor de declarações](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [Criar um objeto de confiança de terceira parte confiável sem reconhecimento de declarações](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [Criar um objeto de confiança de terceira parte confiável](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [Configurar AD FS para trabalhar com o provedor de Federação agregado (por exemplo, incomum)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>Configuração da experiência de entrada
- [Definir configurações de logon único do AD FS 2016](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [Configurar AD FS entrada paginada](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [Configurar AD FS personalização de entrada do usuário](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [Configurar o AD FS para enviar declarações de expiração de senha](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [Configurar a autenticação baseada em formulários de intranet para dispositivos que não são compatíveis com a WIA](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>Outro
- [Ingresso no Local de Trabalho em qualquer dispositivo de SSO e autenticação de dois fatores contínua em aplicativos da empresa](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Gerenciar riscos com Autenticação Multifator adicional para aplicativos confidenciais](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Gerenciar risco com o Controle de Acesso Condicional](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [Configurar um ambiente de laboratório do AD FS](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [Guia de instruções: gerenciar riscos com autenticação multifator adicional para aplicativos confidenciais](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Guia de instruções: gerenciar riscos com o controle de acesso condicional](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [Walkthrough: Workplace Join com um dispositivo Windows](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [Walkthrough: Workplace Join com um dispositivo iOS](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  


