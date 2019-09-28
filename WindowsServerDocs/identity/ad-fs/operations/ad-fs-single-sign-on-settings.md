---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Configurações de logon único do AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 311789fdec160faeeeba0ecf26491d1e0cd6105d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407391"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS configurações de logon único

O SSO (logon único) permite que os usuários se autentiquem uma vez e acessem vários recursos sem ser solicitado a fornecer credenciais adicionais.  Este artigo descreve o comportamento de AD FS padrão para SSO, bem como as definições de configuração que permitem que você personalize esse comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos de logon único com suporte

O AD FS dá suporte a vários tipos de experiências de logon único:  
  
-   **SSO de sessão**  
  
     Os cookies de SSO de sessão são escritos para o usuário autenticado, o que elimina solicitações adicionais quando o usuário alterna os aplicativos durante uma sessão específica. No entanto, se uma sessão específica terminar, as credenciais do usuário serão solicitadas novamente.  
  
     AD FS definirá os cookies de SSO de sessão por padrão se os dispositivos dos usuários não estiverem registrados. Se a sessão do navegador tiver terminado e for reiniciada, esse cookie de sessão será excluído e não será mais válido.  
  
-   **SSO persistente**  
  
     Os cookies de SSO persistentes são escritos para o usuário autenticado, o que elimina solicitações adicionais quando o usuário alterna os aplicativos enquanto o cookie de SSO persistente é válido. A diferença entre SSO persistente e SSO de sessão é que o SSO persistente pode ser mantido em diferentes sessões.  
  
     AD FS definirá os cookies de SSO persistentes se o dispositivo estiver registrado. AD FS também definirá um cookie de SSO persistente se um usuário selecionar a opção "Mantenha-me conectado". Se o cookie de SSO persistente não for mais válido, ele será rejeitado e excluído.  
  
-   **SSO específico do aplicativo**  
  
     No cenário OAuth, um token de atualização é usado para manter o estado de SSO do usuário dentro do escopo de um aplicativo específico.  
  
     Se um dispositivo estiver registrado, AD FS definirá o tempo de expiração de um token de atualização com base na vida útil dos cookies de SSO persistente para um dispositivo registrado que é 7 dias por padrão para AD FS 2012R2 e até um máximo de 90 dias com AD FS 2016 se usarem seu dispositivo para Acesse AD FS recursos dentro de uma janela de 14 dias. 

Se o dispositivo não estiver registrado, mas um usuário selecionar a opção "manter-me conectado", a hora de expiração do token de atualização será igual ao tempo de vida dos cookies de SSO persistentes para "Mantenha-me conectado", que é 1 dia por padrão, com o máximo de 7 dias. Caso contrário, o tempo de vida do token de atualização será igual ao tempo de vida do cookie SSO da sessão, que é 8  
  
 Conforme mencionado acima, os usuários em dispositivos registrados sempre receberão um SSO persistente, a menos que o SSO persistente esteja desabilitado. Para dispositivos não registrados, o SSO persistente pode ser obtido habilitando o recurso "Mantenha-me conectado" (KMSI). 
 
 Para o Windows Server 2012 R2, para habilitar o PSSO para o cenário "Mantenha-me conectado", você precisa instalar esse [hotfix](https://support.microsoft.com/en-us/kb/2958298/) que também faz parte do [pacote cumulativo de atualizações de agosto de 2014 para Windows RT 8,1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Tarefa | PowerShell | Descrição
------------ | ------------- | -------------
Habilitar/desabilitar SSO persistente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| O SSO persistente é habilitado por padrão. Se estiver desabilitado, nenhum cookie PSSO será gravado.
"Habilitar/desabilitar" Mantenha-me conectado " | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | O recurso "Mantenha-me conectado" está desabilitado por padrão. Se estiver habilitado, o usuário final verá uma opção "Mantenha-me conectado" na página de entrada AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-logon único e dispositivos autenticados
AD FS 2016 altera o PSSO quando o solicitante está Autenticando de um dispositivo registrado aumentando para no máximo 90 dias, mas exigindo uma autenticação dentro de um período de 14 dias (janela de uso do dispositivo).
Depois de fornecer credenciais pela primeira vez, por padrão, os usuários com dispositivos registrados obtêm logon único por um período máximo de 90 dias, desde que eles usem o dispositivo para acessar AD FS recursos pelo menos uma vez a cada 14 dias.  Se eles aguardarem 15 dias depois de fornecer credenciais, os usuários serão solicitados a fornecer as credenciais novamente.  

O SSO persistente é habilitado por padrão. Se estiver desabilitado, nenhum cookie PSSO será gravado. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
A janela de uso do dispositivo (14 dias por padrão) é regida pela propriedade AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
O período de logon único máximo (90 dias por padrão) é regido pela propriedade AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantenha-me conectado por dispositivos não autenticados 
Para dispositivos não registrados, o período de logon único é determinado pelas configurações do recurso **manter-me conectado (KMSI)** .  KMSI é desabilitado por padrão e pode ser habilitado definindo a propriedade AD FS KmsiEnabled como true.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Com o KMSI desabilitado, o período de logon único padrão é de 8 horas.  Isso pode ser configurado usando a propriedade SsoLifetime.  A propriedade é medida em minutos, portanto, seu valor padrão é 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Com o KMSI habilitado, o período de logon único padrão é de 24 horas.  Isso pode ser configurado usando a propriedade KmsiLifetimeMins.  A propriedade é medida em minutos, portanto, seu valor padrão é 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento da autenticação multifator (MFA)  
É importante observar que, embora forneça períodos relativamente longos de logon único, AD FS solicitará autenticação adicional (autenticação multifator) quando um logon anterior se basear em credenciais primárias e não MFA, mas o logon atual requer MFA.  Isso é independente da configuração de SSO. AD FS, quando ele recebe uma solicitação de autenticação, primeiro determina se há um contexto SSO (como um cookie) e, em seguida, se a MFA for necessária (como se a solicitação for proveniente de fora), ele avaliará se o contexto SSO contém MFA ou não.  Caso contrário, a MFA será solicitada.  


  
## <a name="psso-revocation"></a>Revogação de PSSO  
 Para proteger a segurança, AD FS rejeitará qualquer cookie de SSO persistente emitido anteriormente quando as condições a seguir forem atendidas. Isso exigirá que o usuário forneça suas credenciais para se autenticar com o AD FS novamente. 
  
- Usuário altera a senha  
  
- A configuração de SSO persistente está desabilitada no AD FS  
  
- O dispositivo está desabilitado pelo administrador em caso perdido ou roubado  
  
- AD FS recebe um cookie de SSO persistente que é emitido para um usuário registrado, mas o usuário ou o dispositivo não está mais registrado  
  
- AD FS recebe um cookie de SSO persistente para um usuário registrado, mas o usuário se registrou novamente  
  
- AD FS recebe um cookie de SSO persistente que é emitido como resultado da configuração "Mantenha-me conectado", mas "Mantenha-me conectado" é desabilitado no AD FS  
  
- AD FS recebe um cookie de SSO persistente que é emitido para um usuário registrado, mas o certificado do dispositivo está ausente ou alterado durante a autenticação  
  
- AD FS administrador definiu um tempo de corte para o SSO persistente. Quando isso estiver configurado, AD FS rejeitará qualquer cookie de SSO persistente emitido antes desta hora  
  
  Para definir o tempo de corte, execute o seguinte cmdlet do PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitar PSSO para que os usuários do Office 365 acessem o SharePoint Online  
 Depois que o PSSO estiver habilitado e configurado no AD FS, AD FS gravará um cookie persistente depois que um usuário for autenticado. Na próxima vez que o usuário chegar, se um cookie persistente ainda for válido, um usuário não precisará fornecer credenciais para autenticar novamente. Você também pode evitar o prompt de autenticação adicional para os usuários do Office 365 e do SharePoint Online configurando as duas regras de declarações a seguir em AD FS para disparar a persistência em Microsoft Azure AD e SharePoint Online.  Para permitir que o PSSO para usuários do Office 365 acesse o SharePoint Online, você precisa instalar esse [hotfix](https://support.microsoft.com/en-us/kb/2958298/) , que também faz parte do [pacote cumulativo de atualizações de agosto de 2014 para Windows RT 8,1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Uma regra de transformação de emissão para passar pela declaração InsideCorporateNetwork  
  
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
  
Para resumir:
<table>
  <tr>
    <th colspan="1">Experiência de logon único</th>
    <th colspan="3">ADFS 2012 R2 <br> O dispositivo está registrado?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> O dispositivo está registrado?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NÃO</th>
    <th>Não, mas KMSI</th>
    <th>SIM</th>
    <th></th>
    <th>NÃO</th>
    <th>Não, mas KMSI</th>
    <th>SIM</th>
  </tr>
 <tr align="center">
    <td>SSO =&gt;definir token de atualização =&gt;</td>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
    <th></th>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;definir token de atualização =&gt;</td>
    <td>N/D</td>
    <td>24 horas</td>
    <td>7 dias</td>
    <th></th>
    <td>N/D</td>
    <td>24 horas</td>
    <td>Máximo de 90 dias com janela de 14 dias</td>
  </tr>

 <tr align="center">
    <td>Tempo de vida do token</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**Dispositivo registrado?** Você Obtém um SSO PSSO/persistente <br>
**Dispositivo não registrado?** Você Obtém um SSO <br>
**Dispositivo não registrado, mas KMSI?** Você Obtém um SSO PSSO/persistente <p>
QUE
 - [x] o administrador habilitou o recurso KMSI [e]
 - [x] o usuário clica na caixa de seleção KMSI na página de logon do Forms
 
**Bom saber:** <br>
Os usuários federados que não têm o atributo **LastPasswordChangeTimestamp** sincronizado são cookies de sessão emitidos e tokens de atualização que têm um **valor de idade máxima de 12 horas**.<br>
Isso ocorre porque o Azure AD não pode determinar quando revogar tokens relacionados a uma credencial antiga (como uma senha que foi alterada). Portanto, o Azure AD deve verificar com mais frequência para garantir que o usuário e os tokens associados ainda estejam em boas condições.
