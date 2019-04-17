---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: "AD FS 2016 Sign-On único configurações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS único logon configurações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Único logon (SSO) permite que os usuários autenticar uma vez e acessar vários recursos sem precisar inserir credenciais adicionais.  Este artigo descreve o comportamento do AD FS padrão para SSO, bem como as configurações que permitem que você personalize esse comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos com suporte de logon único

AD FS dá suporte a vários tipos de experiências de logon único:  
  
-   **Sessão de logon único**  
  
     Os cookies de sessão SSO são escritos para o usuário autenticado que elimina solicitações posteriores quando o usuário alterna entre aplicativos durante uma sessão específica. No entanto, se uma determinada sessão termina, o usuário deverá suas credenciais novamente.  
  
     AD FS definirá sessão cookies SSO por padrão se dispositivos dos usuários não são registrados. Se a sessão do navegador terminou e é reiniciada, esse cookie de sessão será excluída e não será válido mais.  
  
-   **SSO persistente**  
  
     Cookies persistentes de SSO são escritos para o usuário autenticado que elimina ainda mais avisos quando o usuário alterna aplicativos para, desde o cookie SSO persistente é válido. A diferença entre persistente SSO e sessão SSO é que pode ser mantida SSO persistente entre as diferentes sessões.  
  
     AD FS definirá cookies persistentes de SSO se o dispositivo é registrado. AD FS também definirá um cookie SSO persistente, se um usuário seleciona a opção "Mantenha-me conectado". Se o cookie SSO persistente não for mais válido, ele será rejeitado e excluído.  
  
-   **SSO específico do aplicativo**  
  
     No cenário de OAuth, um token de atualização é usado para manter o estado SSO do usuário dentro do escopo de um aplicativo específico.  
  
     Se um dispositivo é registrado, o AD FS definirá o tempo de expiração de um token de atualização com base no tempo de vida de cookies SSO persistente para um dispositivo registrado que é 7 dias por padrão. Se um usuário seleciona a opção "Mantenha-me conectado", o tempo de expiração do token de atualização será igual do tempo de vida de cookies SSO persistente para "keep me conectado" que é 1 dia por padrão com o máximo de sete dias. Caso contrário, atualize a vida útil do token é igual a sessão SSO vida útil do cookie que é de 8 horas por padrão  
  
 Conforme mencionado acima, os usuários em dispositivos registrados sempre receberão um SSO persistente, a menos que o SSO persistente está desabilitado. Para dispositivos cancelados, SSO persistente pode ser obtido ao habilitar o keep"me conectado" recurso (KMSI). 
 
 Windows Server 2012 R2, para habilitar PSSO para o cenário "Mantenha-me conectado", você precisa instalar este [hotfix](https://support.microsoft.com/en-us/kb/2958298/) que também é parte da de [agosto de 2014 cumulativo de atualizações para o Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>Logon único e dispositivos autenticados  
Depois de fornecer credenciais pela primeira vez, por padrão os usuários com dispositivos registrados obter logon único por um período máximo de 90 dias, desde que eles usam o dispositivo para acessar recursos do AD FS pelo menos uma vez a cada 14 dias.  Se eles aguardem 15 dias depois de fornecer credenciais, os usuários precisarão credenciais novamente.  

SSO persistente é habilitada por padrão. Se estiver desativado, nenhum cookie PSSO será gravado. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
A janela de uso do dispositivo (14 dias por padrão) é regida pela propriedade AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
O único logon período máximo (90 dias por padrão) é regido pela propriedade AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantenha-Me conectado para dispositivos não autenticados 
Para dispositivos não registrada, o período de logon único é determinado pelo **Keep Me fez em (KMSI)** configurações de recursos.  KMSI está desabilitado por padrão e pode ser habilitado configurando a propriedade do AD FS KmsiEnabled como True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Com KMSI desabilitada, o único logon período padrão é de 8 horas.  Isso pode ser configurado usando a propriedade SsoLifetime.  A propriedade é medida em minutos, portanto, seu valor padrão é de 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Com KMSI habilitado, o único logon período padrão é 24 horas.  Isso pode ser configurado usando a propriedade KmsiLifetimeMins.  A propriedade é medida em minutos, portanto, seu valor padrão é 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento de autenticação multifator (MFA)  
É importante observar que, enquanto o fornecimento de relativamente longos períodos de logon único, o AD FS solicitará para autenticação adicional (autenticação de fator multi) quando um logon anterior foi com base em credenciais principais e não MFA, mas o atual de logon requer MFA.  Isso é, independentemente da configuração de SSO. AD FS, quando ele recebe uma solicitação de autenticação, primeiro determina se ou não há um contexto SSO (por exemplo, um cookie) e, se for necessária a MFA (por exemplo, se a solicitação é proveniente de fora) ele será avaliar se o contexto SSO contém MFA ou não.  Caso contrário, será solicitado a MFA.  


  
## <a name="psso-revocation"></a>Revogação PSSO  
 Para proteger a segurança, o AD FS rejeitarão qualquer cookie SSO persistente emitida anteriormente quando as seguintes condições forem atendidas. Isso exigirá que o usuário para fornecer suas credenciais para autenticar com o AD FS novamente. 
  
-   Alterações de senha  
  
-   Configuração de SSO persistente está desabilitada no AD FS  
  
-   Dispositivo está desabilitado pelo administrador no caso de perda ou roubo  
  
-   AD FS recebe um cookie SSO persistente que é emitido para um usuário registrado, mas o usuário ou o dispositivo não estiver registrado mais  
  
-   AD FS recebe um cookie SSO persistente para um usuário registrado, mas o usuário novamente registrado  
  
-   AD FS recebe um cookie SSO persistente que é emitido como resultado "Mantenha-me conectado" mas "Mantenha-me conectado" configuração está desabilitada no AD FS  
  
-   AD FS recebe um cookie SSO persistente que é emitido para um usuário registrado, mas certificado do dispositivo está ausente ou alterada durante a autenticação  
  
-   AD FS definido pelo administrador um tempo limite para SSO persistente. Quando isso é configurado, o AD FS rejeitarão qualquer cookie SSO persistente emitido antes desta vez  
  
 Para definir o tempo limite, execute o seguinte cmdlet do PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitar PSSO para os usuários do Office 365 acessar o SharePoint Online  
 Depois que PSSO está habilitado e configurado no AD FS, o AD FS gravará um cookie persistente depois que um usuário autenticado. Na próxima vez que o usuário entra, se um cookie persistente ainda é válido, um usuário não precisa fornecer as credenciais para autenticar novamente. Você também pode evitar a solicitação de autenticação adicional para o Office 365 e os usuários do SharePoint Online, configurando nas próximas duas declarações regras no AD FS persistência de gatilho em Microsoft Azure AD e o SharePoint Online.  Para habilitar PSSO para os usuários do Office 365 acessar o SharePoint online, você precisa instalá-lo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) que também é parte da de [agosto de 2014 cumulativo de atualizações para o Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Uma regra de emissão de transformação para passar por meio da reivindicação InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
  
    


