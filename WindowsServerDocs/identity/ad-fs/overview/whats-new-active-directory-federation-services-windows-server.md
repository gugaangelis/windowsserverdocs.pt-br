---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: O que há de novo no Active Directory federação serviços para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>O que há de novo no Active Directory federação serviços para Windows Server 2016

>Aplica-se a: Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>O que há de novo no Active Directory federação serviços para Windows Server 2016   
Se você estiver procurando informações sobre versões anteriores do AD FS, consulte os artigos a seguir:  
 [AD FS no Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) e [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Serviços de Federação do Active Directory fornece controle de acesso e logon único em uma ampla variedade de aplicativos, incluindo Office 365, aplicativos de SaaS baseado em nuvem e aplicativos na rede corporativa.  
* Para a organização de TI, permite que você forneça entrada em e controle de acesso para aplicativos modernos e herdados, no local e na nuvem, com base no mesmo conjunto de credenciais e políticas.    
* Para o usuário, ele fornece entrada perfeita logon usando as credenciais de conta mesmo familiar.  
* Para os desenvolvedores, ele fornece uma maneira fácil para autenticar os usuários cujas identidades live no diretório organizacional para que você possa se concentrar seus esforços em seu aplicativo, não autenticação ou identidade.  

Este artigo descreve as novidades do AD FS em Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Elimine as senhas de Extranet  
AD FS 2016 permite três novas opções para logon sem senhas, permitindo que as organizações evitar o risco de rede comprometer de capturados, perdidas ou roubadas senhas. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Faça logon com o Azure a autenticação multifator
AD FS 2016 compilações após a autenticação multifator funcionalidades (MFA) do AD FS no Windows Server 2012 R2, permitindo que o logon usando apenas um código de MFA do Azure, sem primeiro inserir um nome de usuário e senha.

* Com o Azure MFA, como o método de autenticação principal, o usuário será solicitado seu nome de usuário e o código OTP do aplicativo Azure Authenticator.  
* Com o Azure MFA, como o método de autenticação secundário ou adicional, o usuário fornece autenticação primária credenciais (usando a autenticação integrada do Windows, nome de usuário e senha, um cartão inteligente ou certificado de usuário ou dispositivo) e, em seguida, vê um prompt para texto, voz, ou OTP com base em logon MFA do Azure.  
* Com o novo adaptador de MFA Azure interno, instalação e configuração mfa do Azure com o AD FS nunca foi tão simples.
* As organizações podem tirar proveito do Azure MFA sem a necessidade de um servidor de MFA do Azure no local.
* MFA Azure pode ser configurado para intranet ou extranet ou como parte de qualquer política de controle de acesso.

Para obter mais informações sobre o MFA do Azure com o AD FS
*  [Configurar o AD FS 2016 e MFA Azure](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Acesso sem senha em dispositivos compatíveis com
AD FS 2016 baseia-se nos recursos de registro de dispositivo anterior para habilitar logon e controle de acesso com base o status de conformidade do dispositivo. Os usuários podem assinar sobre como usar as credenciais de dispositivo e conformidade é reavaliada quando alterar atributos do dispositivo, para que você sempre possa garantir políticas estão sendo impostas.  Isso permite que as políticas como

* Habilitar o acesso somente de dispositivos que estão em conformidade e/ou gerenciado  
* Habilitar o acesso Extranet apenas em dispositivos que estão em conformidade e/ou gerenciado  
* Exigir autenticação multifator para computadores que não são gerenciados ou não compatíveis  

AD FS fornece o componente local diante das políticas de acesso condicional em um cenário híbrida. Quando você registrar dispositivos com o Azure AD para acesso condicional a recursos de nuvem, a identidade do dispositivo pode ser usada para políticas do AD FS também.

![Quais são as novidades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para obter mais informações sobre como usar o dispositivo com base em acesso condicional na nuvem   
 *  [Acesso condicional do Active Directory do Azure](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

Para obter mais informações sobre como usar o dispositivo com base em acesso condicional com o AD FS
*  [Planejando para o dispositivo com base em acesso condicional com o AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Faça logon com o Windows Hello para empresas   
Dispositivos Windows 10 introduzem o Windows Hello e o Windows Hello para empresas, substituindo as senhas de usuário com credenciais do usuário de dispositivo associados forte protegidas pelo gesto do usuário (um PIN, um gesto biométrico como impressão digital ou reconhecimento facial). AD FS 2016 dá suporte a estes novos esses novos recursos do Windows 10 para que os usuários podem fazer logon aplicativos do AD FS de intranet ou extranet sem a necessidade de fornecer uma senha.

Para obter mais informações sobre como usar o Microsoft Windows Hello para empresas em sua organização
*  [Habilitar o Windows Hello para empresas em sua organização](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Acesso seguro a aplicativos

### <a name="modern-authentication"></a>Autenticação moderna
AD FS 2016 dá suporte a protocolos mais recentes modernos proporcionar uma melhor experiência de usuário do Windows 10, bem como o iOS mais recente e dispositivos Android e aplicativos.  

Para obter mais informações, consulte [AD FS cenários para desenvolvedores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar políticas de controle de acesso sem precisar saber reivindicar o idioma de regras  
Anteriormente, os administradores do AD FS tinham que configurar políticas usando a linguagem de regra do AD FS reivindicação, tornando difícil para configurar e manter políticas. Com as políticas de controle de acesso, os administradores podem usar modelos internos para aplicar políticas comuns, como
* Permitir o acesso de intranet somente
* Permitir todos e exigem MFA de Extranet
* Permitir todos e exigem MFA de um grupo específico

Os modelos são fáceis de personalizar usando um assistente controlado por processo para adicionar exceções ou as regras de política adicionais e podem ser aplicados a um ou vários aplicativos para a imposição de política consistente.

Para obter mais informações, consulte [políticas de controle de acesso no AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar o logon com diretórios de não - AD LDAP  
Muitas organizações têm uma combinação do Active Directory e diretórios de terceiros. Com a adição de suporte do AD FS para autenticar usuários armazenados em diretórios compatíveis com v3 LDAP, AD FS agora podem ser usado para:
* Usuários no terceiro, diretórios compatíveis com do LDAP v3
* Usuários em florestas do Active Directory para o qual uma relação de confiança do Active Directory bidirecional não está configurada
* Usuários do Active Directory Lightweight Directory Services (AD LDS)

Para obter mais informações, consulte [configurar o AD FS para autenticar usuários armazenados nos diretórios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Melhor experiência de entrada
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar o log na experiência para aplicativos do AD FS  
Ouvimos de você que a capacidade de personalizar a experiência de logon para cada aplicativo seria uma melhoria excelente usabilidade, especialmente para organizações que fornecem entrada sobre para aplicativos que representam várias empresas diferentes ou marcas.  

Anteriormente, o AD FS no Windows Server 2012 R2 fornecido um sinal comuns na experiência para todos os aplicativos de terceiros confiante, com a capacidade de personalizar um subconjunto de texto com base em conteúdo por aplicativo. Com o Windows Server 2016, você pode personalizar não apenas as mensagens, mas uma imagens, um tema de logotipo e da web por aplicativo. Além disso, você pode criar novos temas web personalizadas e aplicá-las por confiar terceiros.  

Para obter mais informações, consulte [personalização de entrada do usuário de AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Gerenciamento e as melhorias operacionais  
A seção a seguir descreve os cenários operacionais aprimorados que foram introduzidos com serviços de Federação do Active Directory no Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Auditoria de gerenciamento mais fácil administrativo simplificada  
No AD FS do Windows Server 2012 R2 lá foram vários eventos de auditoria gerados para uma única solicitação e as informações relevantes sobre uma atividade de emissão de logon ou token estão ausente (em algumas versões do AD FS) ou se espalham entre vários eventos de auditoria. Por padrão o AD FS eventos de auditoria estão desativados devido à sua natureza detalhada.  
Com o lançamento do AD FS 2016, auditoria tornou-se mais simplificada e menos detalhada.  

Para obter mais informações, consulte [auditoria melhorias para o AD FS no Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Melhor interoperabilidade com SAML 2.0 para participação no confederações  
AD FS 2016 contém suporte de protocolo SAML adicional, incluindo suporte para a importação de relações de confiança com base nos metadados que contém várias entidades. Isso permite que você configurar o AD FS para participar confederações como federação InCommon e outras implementações em conformidade com o padrão 2.0 eGov.  

Para obter mais informações, consulte [melhor interoperabilidade com SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gerenciamento de senhas simplificada para federados os usuários do O365  
Você pode configurar os serviços de Federação do Active Directory (AD FS) para enviar solicitações de expiração de senha para as terceira parte relações de confiança (aplicativos) que estão protegidas pelo AD FS. Como essas declarações são usadas dependem do aplicativo. Por exemplo, com o Office 365, como o terceiro, atualizações foram implementadas para Exchange e o Outlook para notificar os usuários federados das suas senhas ser expirou em breve.  

Para obter mais informações, consulte [configurar o AD FS para enviar solicitações de expiração de senha.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Mudando do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016 é mais fácil  
Anteriormente, a migração para uma nova versão do AD FS necessária Exportar configuração do farm antigo e importar para um farm totalmente novo, paralelo.  

Agora, mudando do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016 ficou muito mais fácil. Basta adicionar um novo servidor Windows Server 2016 para um farm de Windows Server 2012 R2 e o farm atuará no nível do comportamento do Windows Server 2012 R2 farm, portanto, ele parece e se comporta exatamente como um farm de Windows Server 2012 R2.  

Em seguida, adicione novos servidores do Windows Server 2016 ao farm, verifique se a funcionalidade e remover os servidores mais antigos do balanceador de carga. Depois que todos os nós de farm estão executando o Windows Server 2016, você estará pronto para atualizar o nível de comportamento farm para 2016 e começar a usar os novos recursos.  

Para obter mais informações, consulte [atualizar para o AD FS no Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
