---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar a proteção de bloqueio de Extranet do AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869757"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>Bloqueio de Extranet e bloqueio inteligente de Extranet do AD FS

# <a name="overview"></a>Visão geral

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

No AD FS no Windows Server 2012 R2, apresentamos um recurso de segurança chamado [bloqueio de Extranet do Soft](configure-ad-fs-extranet-soft-lockout-protection.md).  Com esse recurso, o AD FS para autenticar usuários da extranet para um período de tempo.  Isso impede que as contas de usuário que está sendo bloqueado no Active Directory. Além de proteger os usuários de um bloqueio de conta do AD, o bloqueio de extranet do AD FS também protege contra ataques de adivinhação de senha de força bruta.

Em junho de 2018, o AD FS no Windows Server 2016 introduziu **bloqueio de Extranet inteligente (ESL)**.  ESL permite que o AD FS diferenciar entre tentativas de entrada que parecem do usuário válido e entradas do que pode ser um invasor. Como resultado, o AD FS pode bloquear os invasores, permitindo que os usuários válidos continuam a usar suas contas. Isso impede a negação de serviço no usuário e protege contra ataques direcionados, como ataques de "senha spray".  
ESL está disponível para o AD FS no Windows Server 2016 e é incorporado ao AD FS no Windows Server 2019.

> [!NOTE]
> Esse recurso funciona somente para o **cenário de extranet** nas quais solicitações de autenticação são fornecidos por meio do Proxy de aplicativo Web e se aplica somente aos **autenticação de nome de usuário e senha**.

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>Vantagens do bloqueio inteligente de Extranet do AD FS 2016
Bloqueio de extranet reversível no AD FS 2012 R2 fornecidas as seguintes vantagens essenciais:
- Protege suas contas de usuário do **ataques de força bruta** em que o invasor tenta adivinhar a senha do usuário enviando continuamente as solicitações de autenticação e de **ataques de senha de spray** onde os invasores tentam usar senhas comuns com muitas contas diferentes
- Protege suas contas de usuário do **bloqueio de conta do Active Directory** das solicitações de autenticação mal-intencionado com senhas erradas. Nesse caso, embora a conta de usuário será bloqueada para acesso à extranet, o usuário ainda poderá logon no AD da rede corporativa. Isso é conhecido como um **bloqueio reversível**.

Compilações extranet do bloqueio inteligente sobre as vantagens do software de bloqueio de extranet, adicionando o seguinte:
- Protege os usuários experimentem **bloqueio de conta de extranet** das solicitações de autenticação mal-intencionado.  Bloqueio inteligente impedirá que solicitações possivelmente mal-intencionados de locais desconhecidos, permitindo que o usuário fazer logon da extranet de locais familiares (do qual o usuário fez logon com êxito antes dos locais).
- Tem um único modo de log para que o sistema pode aprender a atividade de logon boa e potencialmente malicioso sem desabilitar todas as contas

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>Vantagens adicionais inteligente de bloqueio de Extranet do AD FS de 2019
Bloqueio inteligente de extranet do AD FS 2019 adiciona as seguintes vantagens em comparação comparadas o AD FS 2016:
- Definir limites de bloqueio independentes para familiarizados e locais para que usuários em locais bons conhecidos possam ter mais espaço para erro que as solicitações de locais suspeitos
- Habilitar o modo de auditoria para o bloqueio inteligente enquanto continua a impor o comportamento de bloqueio flexível anterior

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>Pré-requisitos para o bloqueio inteligente de Extranet do AD FS 2016
Os seguintes pré-requisitos são necessários para ESL com o AD FS de 2019.

### <a name="install-updates-on-all-nodes-in-the-farm"></a>Instalar atualizações em todos os nós no farm
Primeiro, verifique se todos os servidores do AD FS do Windows Server 2016 são atualizados a partir das atualizações do Windows de junho de 2018 e se o farm do AD FS 2016 está em execução no nível de comportamento de farm 2016.

### <a name="update-artifact-database-permissions"></a>Atualizar as permissões de banco de dados de artefato
Bloqueio de extranet do smart requer a conta de serviço do AD FS tenha permissões para uma nova tabela no banco de dados de artefato do ADFS.  Conceda essa permissão, executando o seguinte comando em uma janela de comando do PowerShell:
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
Onde `$cred` é uma conta com permissões de administrador do AD FS (permissões de administrador do AD FS são necessárias para alterar o banco de dados.)

>[!NOTE]
>Em um farm de vários servidores que usa o banco de dados WID, o cmdlet acima exige que o gerenciamento remoto do Windows ser habilitado em todos os servidores do AD FS

Se você não tiver permissões de administrador do AD FS, você pode configurar permissões de banco de dados manualmente em SQL ou um WID, executando o comando a seguir quando conectado ao banco de dados AdfsArtifactStore.
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verifique se o que log de auditoria de segurança do AD FS está habilitado
Este recurso faz uso de auditoria de segurança de logs, portanto, a auditoria devem ser habilitados no AD FS, bem como a política local em todos os servidores do AD FS.

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>Pré-requisitos para o bloqueio inteligente de Extranet do AD FS de 2019
Os seguintes pré-requisitos são necessários para ESL com o AD FS 2016.

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verifique se o que log de auditoria de segurança do AD FS está habilitado
Este recurso faz uso de auditoria de segurança de logs, portanto, a auditoria devem ser habilitados no AD FS, bem como a política local em todos os servidores do AD FS.

## <a name="lockout-settings"></a>Configurações de bloqueio
Bloqueio de extranet do smart consiste em um conjunto de novos recursos controlado por propriedades novas e existentes do AD FS.

### <a name="extranet-lockout-enabled"></a>Bloqueio de extranet habilitado
Bloqueio de extranet do inteligente usa a mesma propriedade do AD FS que anteriormente era usada apenas para controlar o bloqueio de extranet "soft".  A propriedade é chamada ExtranetLockoutEnabled e pode ser exibida por meio de Get-AdfsProperties.

### <a name="extranet-smart-lockout-mode"></a>Modo de bloqueio inteligente de extranet
Uma propriedade do AD FS novo chamada ExtranetLockoutMode foi adicionada para controlar o comportamento de bloqueio "soft" inteligente vs.  Ele pode ser definido por meio de Set-AdfsProperties e contém 3 valores:

    - **ADPasswordCounter** – isso é herdado, modo de "bloqueio de extranet reversível" do AD FS que não diferencia com base no local.  Este é o valor padrão.

    - **ADFSSmartLockoutLogOnly** – esse é o bloqueio inteligente de Extranet, mas em vez de rejeitar solicitações de autenticação, o AD FS será somente gravação eventos admin e auditoria.

    - **ADFSSmartLockoutEnforce** -esse é o bloqueio inteligente de Extranet com suporte completo para bloquear solicitações não familiares, quando os limites são atingidos.

No AD FS 2019, os valores ADPasswordCounter e ADFSSmartLockoutLogOnly podem ser combinados para que o bloqueio de soft continua a ser aplicada enquanto você estiver se preparando para o bloqueio inteligente.

### <a name="lockout-threshold-and-observation-window"></a>Limite de bloqueio e a janela de Observação
Bloqueio inteligente no AD FS 2019 usa as mesmas duas propriedades do AD FS como bloqueio flexível usado anteriormente: ExtranetObservationWindow e ExtranetLockoutThreshold.

- **ExtranetLockoutThreshold &lt;inteiro&gt;**  define o número máximo de tentativas de senha incorreta. Quando o limite é atingido, no ADFSSmartLockoutEnforce modo AD FS rejeitará as solicitações da extranet até que a janela de Observação tenha passado.  No modo de ADFSSmartLockoutLogOnly, AD FS irá escrever apenas entradas de log.  
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  Isso determina quanto tempo o nome de usuário e senha solicitações de locais desconhecidos serão bloqueadas. O AD FS será iniciado realizar a autenticação de nome de usuário e senha novamente quando a janela é passada.

> [!NOTE]
> Funções de bloqueio de extranet do AD FS independentemente das políticas de bloqueio do AD. É recomendável que você defina as **ExtranetLockoutThreshold** valor do parâmetro para um valor que é menor que o limite de bloqueio de conta do AD. Falha ao fazer isso resultaria no AD FS não ser capaz de proteger as contas de serem bloqueados no Active Directory. 

No AD FS 2019, apresentamos um novo limite de bloqueio específico para locais bons conhecidos: ExtranetLockoutThresholdFamiliarLocation.
- **ExtranetLockoutThresholdFamiliarLocation &lt;inteiro&gt;**  define o número máximo de tentativas de senha incorreta de locais familiares. No AD FS 2019, o parâmetro original ExtranetLockoutThreshold se aplica a locais desconhecidos (endereços IP não é conhecidos em boas condições).

### <a name="primary-domain-controller-requirement"></a>Requisito de controlador de domínio primário
O AD FS 2016 oferece um parâmetro que permite o fallback para outro controlador de domínio quando o controlador de domínio primário não está disponível.

- **ExtranetLockoutRequirePDC &lt;Boolean&gt;**  quando habilitada, o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado, o bloqueio de extranet fará fallback para outro controlador de domínio caso o controlador de domínio primário não está disponível.

   O exemplo a seguir mostra o cmdlet para habilitar o bloqueio com o requisito de PDC desabilitado:

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>Configuração do AD FS com o bloqueio inteligente no modo somente de Log

### <a name="ad-fs-2016"></a>AD FS 2016
É recomendável que você defina primeiro o comportamento de bloqueio para fazer logon apenas executando o seguinte cmdlet:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

Nesse modo, o AD FS preenche as informações de localização familiar de usuários e grava eventos de auditoria de segurança, mas não bloqueia todas as solicitações.  Esse modo é usado para validar que o bloqueio inteligente está em execução e para habilitar o AD FS para "saber" locais familiares para os usuários antes de habilitar o "modo de imposição".
Como o AD FS aprende, ele armazena a atividade de logon por usuário (em modo somente de log ou modo de imposição). 

>[!NOTE]
>Configurando `ExtranetLockoutMode` à `AdfsSmartlockoutLogOnly` fará com que o comportamento herdado de FS "reversível bloqueio de extranet" AD não precisa mais estar em vigor, mesmo se o `EnableExtranetLockout` propriedade é definida como True.  Isso significa que os usuários que excedem os limites de bloqueio de endereços IP familiares ou desconhecidos não ser bloqueados pelo bloqueio inteligente do AD FS. No entanto, no local AD pode bloqueio do usuário com base na configuração de lá.   Consulte [política de bloqueio de conta](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy) para saber como local AD pode bloquear usuários. "  Isso serve para ser um estado temporário para que o sistema possa aprender o comportamento de logon antes de reintrodução imposição de bloqueio com o novo comportamento de bloqueio inteligente.

Para o novo modo entrem em vigor, reinicie o serviço do AD FS em todos os nós no farm
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
Depois que o modo é configurado, você pode habilitar o bloqueio inteligente usando o `EnableExtranetLockout` parâmetro


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

Observe que você pode usar o mesmo cmdlet para desabilitar o bloqueio

Exemplo: Desabilitar bloqueio

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS 2019
Se não estiver usando atualmente o AD FS Extranet Lockout reversível, é recomendável que você siga as mesmas orientações para AD FS 2016 acima.
Se você estiver usando o bloqueio flexível, no entanto, recomendamos que você defina o comportamento de bloqueio do AD FS de 2019 para fazer logon para o bloqueio inteligente, mas manter a imposição de bloqueio flexível, usando o powershell abaixo:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

Depois de executar esse cmdlet, você pode usar Get-AdfsProperties para consultar o valor da propriedade ExtranetLockoutMode AD FS.  Você verá que seu valor foi atualizado para uma combinação bit a bit de ADPasswordCounter e ADFSSmartLockoutLogOnly.

## <a name="observing-audit-events"></a>Observando os eventos de auditoria
O AD FS gravará eventos de bloqueio de extranet para o log de auditoria de segurança:
-   Quando um usuário está bloqueado out (atingir o limite de bloqueio de tentativas de logon malsucedida)
-   Quando o AD FS recebe uma tentativa de logon para um usuário que já está em estado de bloqueio

No modo somente de log, você pode verificar o log de auditoria de segurança para eventos de bloqueio.  Para todos os eventos encontrados, você pode verificar o estado do usuário usando o cmdlet Get-ADFSAccountActivity para determinar se o bloqueio ocorreu de endereços IP familiares ou desconhecidos e verifique a lista de endereços IP familiares para o usuário.

Evento de exemplo:
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>Observando a atividade do usuário
O AD FS fornece cmdlets do powershell para exibir e gerenciar dados de atividade de conta de usuário.  Para ler a atividade da conta atual para uma conta de usuário.  Use o cmdlet abaixo

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

Exemplo de saída
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

A saída da atividade atual contém os seguintes dados:

**Identificador**: isso é o nome de usuário

**BadPwdCountFamiliar**: essa é a contagem atual de tentativas de logon de senha incorreta de endereços IP que estavam na lista de "FamiliarIps" no momento da tentativa

**BadPwdCountUnknown**: essa é a contagem atual de tentativas de logon de senha incorreta de endereços IP que não estavam na lista de "FamiliarIps" no momento da tentativa

**LastFailedAuthFamiliar**: isso é a hora da última tentativa de logon de senha incorreta de um endereço IP que estava na lista de "FamiliarIps" na hora da tentativa

**LastFailedAuthUnknown**: isso é a hora da última tentativa de logon de senha incorreta de um endereço IP que não estava na lista de "FamiliarIps" na hora da tentativa

**FamiliarLockout**: isso indica se o usuário está atualmente em um estado de bloqueio de tentativas de senha correta de endereços IP na lista de "FamiliarIps" 

**UnknownLockout**: isso indica se o usuário está atualmente em um estado de bloqueio de tentativas de senha correta de endereços IP não está na lista de "FamiliarIps" FamiliarIps: esta é a lista atual de familiarizados endereços IP para o usuário

## <a name="adjust-threshold-and-window"></a>Ajustar o limite e a janela
Depois que você estava executando no modo de log somente para uma quantidade suficiente de tempo para o AD FS saber os locais de logon, talvez você queira ajustar a janela de observação ou de limite das configurações padrão.  Isso é feito usando `Set-AdfsProperties` como nos exemplos a seguir:

A janela de observação é definida usando `ExtranetObservationWindow`:

Exemplo: 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

Onde o valor é um intervalo de tempo

### <a name="setting-threshold-value-in-ad-fs-2016"></a>Valor de limite de configuração do AD FS 2016
No AD FS 2016, o limite é definido usando ExtranetLockoutThreshold:

Exemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>Valores de limite de configuração do AD FS de 2019
No AD FS 2019, há valores de limite distintos para locais conhecidos do BOM e desconhecidos

Para definir o valor de limite de locais desconhecidos, use a mesma propriedade usada para o AD FS 2016 acima:

Exemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

Para definir o valor de limite para locais de BOM conhecidos, use a nova propriedade ExtranetLockoutThresholdFamiliarLocation, conforme mostrado no exemplo a seguir:

Exemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>Habilitar o modo de imposição
Depois que você estava executando no modo de log somente por tempo suficiente para o AD FS para saber os locais de logon e observar qualquer atividade de bloqueio e, quando estiver familiarizado com a janela de observação e de limite de bloqueio, bloqueio inteligente pode ser movido para "forçar" o uso de modo a Cmdlet PSH abaixo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

Para o novo modo entrem em vigor, reinicie o serviço do AD FS em todos os nós no farm

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>Gerenciar a atividade de conta de usuário
O AD FS fornece 3 cmdlets para gerenciar dados de atividade de conta de usuário.  Esses cmdlets se conectar automaticamente para o nó no farm que mantém a função de mestre (embora esse comportamento pode ser substituído, passando o parâmetro - Server).

> [!NOTE] 
> Para obter informações sobre como delegar permissões para usar esses cmdlets, consulte [delegado AD FS Powershell Commandlet acesso a usuários não-administrador](delegate-ad-fs-pshell-access.md)

Esses cmdlets são:

`Get-ADFSAccountActivity`

Leia a atividade da conta atual para uma conta de usuário.  O cmdlet sempre automaticamente se conecta ao mestre do farm usando o ponto de extremidade REST da atividade de conta, portanto, todos os dados sempre devem ser consistentes

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

Atualize a atividade de conta para uma conta de usuário.  Isso pode ser usado para adicionar novos locais familiares ou apagar o estado de qualquer conta

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

Redefine o contador de bloqueio para uma conta de usuário

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>Solução de problemas ESL
A seguir pode ajudá-lo com o recurso de bloqueio inteligente de extranet de solução de problemas.

### <a name="updating-database-permissions-for-esl"></a>Atualizar as permissões de banco de dados para ESL
Se erros forem retornados do `Update-AdfsArtifactDatabasePermission` cmdlet, verifique o seguinte

1.  A lista de nós do farm está correta.  Se um nó está na lista de farm de AD FS, mas não há mais a verificação de patch do Active Directory falhará.  Isso pode ser corrigido executando `remove-adfsnode <node name >`
2.  Verifique se que o patch é implantado em todos os nós no farm
3.  Verifique se as credenciais passadas para o cmdlet tem permissão para modificar o proprietário do esquema de banco de dados de artefato do ad fs.  

### <a name="logging--auditing"></a>Registro em log / auditoria
Quando uma solicitação de autenticação é rejeitada porque a conta excede o limite de bloqueio, o AD FS gravará um `ExtranetLockoutEvent` no fluxo de auditoria de segurança.  

Evento de exemplo:

Ocorreu um evento de bloqueio de extranet. Consulte o XML para detalhes da falha. 

**ID da atividade: 172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>Endereços IP proibidos
Além dos recursos de bloqueio inteligente de extranet, a atualização de junho de 2018 do AD FS permite configurar um conjunto de endereços IP globalmente no AD FS, para que as solicitações provenientes de endereços IP, ou que tenham esses endereços IP **x-forwarded-for**  ou **x-ms-forwarded-client-ip** cabeçalhos, serão bloqueados pelo AD FS.

##### <a name="adding-banned-ips"></a>Adicionando IPs proibidos
Para adicionar IPs proibidos à lista global, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR com IPv4 ou v6
4.  Intervalo de IP com o IPv4 ou v6 (ou seja, 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>Removendo IPs proibidos
Para remover banidas IPs da lista global, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>IPs proibidos de leitura
Para ler o conjunto atual de endereços IP proibidos, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Exemplo de saída:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referências adicionais  
[Práticas recomendadas para proteger os serviços de Federação do Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
