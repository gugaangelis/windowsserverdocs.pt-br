---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Configurações de logon único do AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3719277c80eae2bf2a4d923146920d17546601d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188735"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS único as configurações de logon

Single Sign-On (SSO) permite aos usuários autenticar uma vez e acessar vários recursos sem ser solicitado para credenciais adicionais.  Este artigo descreve o padrão de comportamento do AD FS para SSO, bem como as definições de configuração que permitem que você personalize esse comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos com suporte de logon único

O AD FS dá suporte a vários tipos de experiências de logon único:  
  
-   **Sessão de SSO**  
  
     Cookies de SSO de sessão são gravados para o usuário autenticado que elimina as solicitações posteriores quando o usuário alterna entre aplicativos durante uma sessão específica. No entanto, se uma determinada sessão for encerrada, o usuário será solicitado para suas credenciais novamente.  
  
     O AD FS definirá sessão cookies de SSO por padrão se os dispositivos dos usuários não registrados. Se a sessão do navegador terminou e é reiniciada, esse cookie de sessão é excluído e mais não é válido.  
  
-   **SSO persistente**  
  
     Cookies SSO persistentes são gravados para o usuário autenticado que ainda mais elimina avisos quando o usuário alterna a aplicativos para desde que o cookie do SSO persistente é válido. A diferença entre persistente SSO e sessão de SSO é que o SSO persistente pode ser mantido entre as diferentes sessões.  
  
     AD FS definirá cookies de SSO persistentes, se o dispositivo está registrado. AD FS também definirá um cookie SSO persistente, se um usuário seleciona a opção "manter-me conectado". Se o cookie SSO persistente não for mais válido, ela será rejeitada e excluída.  
  
-   **SSO específico do aplicativo**  
  
     No cenário de OAuth, um token de atualização é usado para manter o estado do SSO do usuário dentro do escopo de um aplicativo específico.  
  
     Se um dispositivo for registrado, o AD FS definirá o tempo de expiração de um token de atualização com base em que o tempo de vida de cookies SSO persistente para um dispositivo registrado que é de 7 dias por padrão para o AD FS 2012R2 e até um máximo de 90 dias com o AD FS 2016 se eles usarem seu dispositivo para acesse os recursos do AD FS em uma janela de 14 dias. 

Se o dispositivo não está registrado, mas um usuário seleciona a opção "manter-me conectado", o tempo de expiração do token de atualização será igual a duração de cookies SSO persistente para "manter-me conectado" que é 1 dia por padrão com o máximo de 7 dias. Caso contrário, atualize a vida útil do token é igual a sessão SSO vida útil do cookie que é 8 horas por padrão  
  
 Conforme mencionado acima, os usuários em dispositivos registrados sempre obterá um SSO persistente, a menos que o SSO persistente está desabilitado. Para dispositivos não registrados, o SSO persistente pode ser obtido habilitando "manter-me conectado" recurso (KMSI). 
 
 Para Windows Server 2012 R2, para habilitar PSSO para o cenário "Mantenha-me conectado", você precisará instalá-lo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) que também é parte dos do [atualização cumulativa de agosto de 2014 para Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Tarefa | PowerShell | Descrição
------------ | ------------- | -------------
Habilitar/desabilitar o SSO persistente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| SSO persistente é habilitado por padrão. Se ele estiver desabilitado, nenhum cookie PSSO será gravado.
"Habilitar/desabilitar"Mantenha-me conectado" | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | O recurso de "Mantenha-me conectado" está desabilitado por padrão. Se ele estiver habilitado, o usuário final verá uma opção "manter-me conectado" na página de entrada do AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016 - Sign-On único e dispositivos autenticados
AD FS 2016 altera o PSSO ao solicitante é autenticar a partir de um dispositivo registrado, o aumento máximo 90 dias, mas que exigem um authenticvation dentro de um período de 14 dias (janela de uso do dispositivo).
Depois de fornecer as credenciais pela primeira vez, por padrão os usuários com dispositivos registrados obtém logon único por um período máximo de 90 dias, desde que usam o dispositivo para acessar recursos do AD FS pelo menos uma vez a cada 14 dias.  Se eles aguardam 15 dias depois de fornecer credenciais, os usuários deverão credenciais novamente.  

SSO persistente é habilitado por padrão. Se ele estiver desabilitado, nenhum cookie PSSO será gravado. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
A janela de uso do dispositivo (14 dias, por padrão) é controlada pela propriedade do AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
O período máximo de single Sign-On (90 dias, por padrão) é controlado pela propriedade do AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantenha-Me conectado para o dispositivo não autenticados 
Para dispositivos não registrados, o período de logon único é determinado pela **mantenha-Me conectado no (KMSI)** as configurações de recurso.  KMSI está desabilitado por padrão e pode ser habilitada definindo a propriedade do AD FS KmsiEnabled como True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Com KMSI desabilitado, o único logon período padrão é 8 horas.  Isso pode ser configurado usando a propriedade SsoLifetime.  A propriedade é medida em minutos, portanto, seu valor padrão é de 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Com KMSI habilitado, o único logon período padrão é de 24 horas.  Isso pode ser configurado usando a propriedade KmsiLifetimeMins.  A propriedade é medida em minutos, portanto, seu valor padrão é 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento de autenticação multifator (MFA)  
É importante observar que, ao mesmo tempo, fornecer relativamente longos períodos de logon único, o AD FS solicitará autenticação adicional (multi-factor authentication) quando um logon anterior foi com base em credenciais principais e não o MFA, mas atual de logon requer a MFA.  Isso é, independentemente da configuração de SSO. O AD FS, quando ele recebe uma solicitação de autenticação, primeiro determina se há um contexto SSO (como um cookie) e, em seguida, se a MFA é necessária (por exemplo, se a solicitação é proveniente fora) ele vai avaliar se o contexto SSO contém MFA.  Caso contrário, a MFA é solicitado.  


  
## <a name="psso-revocation"></a>Revogação PSSO  
 Para proteger a segurança, o AD FS rejeitará qualquer cookie do SSO persistente anteriormente emitido quando as seguintes condições forem atendidas. Isso exige que o usuário a fornecer suas credenciais para autenticar novamente com o AD FS. 
  
-   Usuário altera a senha  
  
-   Configuração de SSO persistente está desabilitada no AD FS  
  
-   Dispositivo é desativado pelo administrador no caso de perda ou roubo  
  
-   O AD FS recebe um cookie SSO persistente que é emitido para um usuário registrado, mas o usuário ou o dispositivo não está mais registrado  
  
-   O AD FS recebe um cookie SSO persistente para um usuário registrado, mas o usuário registrado novamente  
  
-   O AD FS recebe um cookie SSO persistente que é emitido como resultado de "Mantenha-me conectado", mas "Mantenha-me conectado" configuração está desabilitada no AD FS  
  
-   AD FS recebe um cookie SSO persistente que é emitido para um usuário registrado, mas o certificado do dispositivo está ausente ou alterada durante a autenticação  
  
-   O administrador do AD FS tiver definido uma hora de corte para SSO persistente. Quando isso estiver configurado, o AD FS rejeitará qualquer cookie do SSO persistente emitido antes dessa hora  
  
 Para definir a hora de corte, execute o seguinte cmdlet do PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitar PSSO para usuários do Office 365 acessar o SharePoint Online  
 Depois que PSSO está habilitado e configurado no AD FS, o AD FS gravará um cookie persistente depois que um usuário autenticado. Na próxima vez que o usuário entra, se um cookie persistente ainda for válido, um usuário não precisa fornecer credenciais para autenticar novamente. Você também pode evitar o prompt de autenticação adicionais para o Office 365 e declarações de usuários do SharePoint Online, configure as duas seguintes regras no AD FS para o gatilho de persistência no Microsoft Azure AD e o SharePoint Online.  Para habilitar PSSO para usuários do Office 365 acessar o SharePoint online, você precisará instalá-lo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) que também é parte dos do [atualização cumulativa de agosto de 2014 para Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Uma regra de transformação de emissão para passar a declaração InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
Resumo:
<table>
  <tr>
    <th colspan="1">Experiência de logon único</th>
    <th colspan="3">ADFS 2012 R2 <br> Dispositivo é registrado?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> Dispositivo é registrado?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NÃO</th>
    <th>NÃO mas KMSI</th>
    <th>SIM</th>
    <th></th>
    <th>NÃO</th>
    <th>NÃO mas KMSI</th>
    <th>SIM</th>
  </tr>
 <tr align="center">
    <td>SSO = > definir Token de atualização = ></td>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
    <th></th>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
  </tr>

 <tr align="center">
    <td>PSSO = > definir Token de atualização = ></td>
    <td>N/D</td>
    <td>24 horas</td>
    <td>7 dias</td>
    <th></th>
    <td>N/D</td>
    <td>24 horas</td>
    <td>Máximo 90 dias com a janela de 14 dias</td>
  </tr>

 <tr align="center">
    <td>Vida útil do token</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**Dispositivo registrado?** Você obtém um PSSO / SSO persistente <br>
**Dispositivo não registrado?** Você obtém um SSO <br>
**Dispositivo não está registrado, mas KMSI?** Você obtém um PSSO / SSO persistente <p>
IF:
 - [x] administrador tiver habilitado o recurso KMSI [AND]
 - [x] o usuário clica na caixa de seleção KMSI na página de logon de formulários
 
**Bom saber:** <br>
Usuários federados que não têm o **LastPasswordChangeTimestamp** atributo sincronizado são emitidos cookies de sessão e tokens de atualização que têm um **valor de Max Age de 12 horas**.<br>
Isso ocorre porque o Azure AD não pode determinar quando revogar tokens que estão relacionadas a uma credencial antiga (como uma senha que foi alterada). Portanto, o AD do Azure deve verificar com mais frequência para certificar-se de que o usuário e tokens associados ainda estão em boa situação.
