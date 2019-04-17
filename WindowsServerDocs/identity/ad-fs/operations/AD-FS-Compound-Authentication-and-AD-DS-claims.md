---
title: "Composta declarações de autenticação e os serviços de domínio do Active Directory nos serviços de Federação do Active Directory"
description: "Este documento discute a autenticação composta e declarações do AD DS no AD FS."
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticação composta e declarações do AD DS no AD FS
Windows Server 2012 aumenta a autenticação Kerberos, apresentando a autenticação composta.  A autenticação composta permite uma solicitação no serviço Kerberos concessão de tíquete (TGS) para incluir duas identidades: 

- A identidade do usuário
- A identidade do dispositivo do usuário.  

Windows realiza a autenticação composta estendendo [Kerberos flexível autenticação segura de encapsulamento (FAST) ou proteção Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 e versões posteriores permite o consumo do AD DS emitidos declarações do usuário ou dispositivo que residem em um tíquete de autenticação Kerberos. Em versões anteriores do AD FS, as declarações mecanismo só pode ler o usuário e grupo identificadores de segurança (SIDs) de Kerberos, mas não foi capaz de ler qualquer declarações informações contidas em um tíquete Kerberos.

Você pode habilitar o controle de acesso mais avançado para aplicativos federados usando os serviços de domínio do Active Directory (AD DS)-emitidos declarações de dispositivo e usuário juntos, aos serviços de Federação do Active Directory (AD FS).

## <a name="requirements"></a>Requisitos
1.  Os computadores que acessam aplicativos federados, deve autenticar usando o AD FS **autenticação integrada do Windows**. 
    - Autenticação integrada do Windows só está disponível ao se conectar aos servidores back-end AD FS.
    - Computadores devem ser capazes de acessar os servidores do back-end AD FS para o nome do serviço de Federação
    - AD FS servidores deve oferecer autenticação integrada do Windows como um método de autenticação principal em suas configurações de Intranet.

2.  A política **suporte do cliente Kerberos para autenticação composta declarações e proteção Kerberos** deve ser aplicado a todos os computadores que acessam aplicativos federados protegidos pela autenticação composta. Isso é aplicável em caso de floresta única ou entre cenários de floresta.

3.  O domínio que hospeda os servidores do AD FS deve ter o **o suporte KDC para declarações autenticação compõem e proteção Kerberos** configuração de política aplicada a controladores de domínio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Etapas para configurar o AD FS no Windows Server 2012 R2
Use as seguintes etapas para configurar a autenticação composta e requerimentos judiciais ou Extrajudiciais 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1: Habilitar o suporte KDC para declarações, autenticação composta e proteção Kerberos na política de controlador de domínio padrão
1.  No Gerenciador do servidor, selecione Ferramentas, **Group Policy Management**.
2.  Navegar para baixo até o **política de controlador de domínio padrão**, clique com botão direito e selecione **editar**.
![Gerenciamento de política de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Sobre o **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **KDC**.
4.  No painel direito, clique duas vezes em **suporte KDC para declarações, autenticação composta e proteção Kerberos**.
![Gerenciamento de política de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Na janela nova caixa de diálogo, defina o suporte KDC para declarações para **Enabled**.
6.  Em Opções, selecione **Supported** do menu suspenso e depois clique em **aplicar** e **Okey**.
![Gerenciamento de política de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte de cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em uma política de grupo aplicado aos computadores acessar aplicativos federados, no **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **administrativas Modelos de**, expanda **sistema**e selecione **Kerberos**.
2.  No painel à direita da janela do Editor de gerenciamento de política de grupo, clique duas vezes em **suporte do cliente Kerberos declarações, autenticação composta e proteção Kerberos.**
3.  Na nova janela caixa de diálogo, defina o suporte do cliente Kerberos como **Enabled** e clique em **aplicar** e **Okey**.
![Gerenciamento de política de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Feche o Editor de gerenciamento de política de grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Etapa 3: Verifique os servidores do AD FS foram atualizados.
Você precisa garantir que as atualizações a seguir estão instaladas em seus servidores do AD FS.

|Atualização|Descrição|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Atualização cumulativa de segurança (incluindo KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Atualização para Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Esta atualização adiciona suporte para declarações de ID compostas nos serviços de Federação do Active Directory.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Etapa 4: Configurar o provedor de autenticação principal

1. Defina o provedor de autenticação principal como **autenticação do Windows** para configurações do AD FS Intranet.
2. No AD FS gerenciamento, sob **políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** clique **editar**.
3. Em **Editar política de autenticação Global** em **Intranet** selecione **autenticação do Windows**.
4. Clique em **aplicar** e **Okey**.

![Gerenciamento de política de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Usando o PowerShell, você pode usar o **conjunto AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Em um trabalho com base em farm, o PowerShell comando deve ser executado no AD FS servidor primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor AD FS que é um membro do farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Etapa 5: Adicionar a descrição de declaração para AD FS
1.  Adicione a seguinte declaração descrição ao farm. Essa descrição de declaração não está presente por padrão no ADFS 2012 R2 e precisa ser adicionado manualmente.
2.  No AD FS gerenciamento, sob **serviço**, clique com botão direito **reivindicar descrição** e selecione **adicionar reivindicar descrição**
3.  Insira as seguintes informações na descrição da declaração
    - Nome de exibição: 'dispositivo grupo Windows' 
    - Descrição da reivindicação: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Coloque uma verificação nas duas caixas.
5. Clique em **Okey**.

![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Usando o PowerShell, você pode usar o **AdfsClaimDescription adicionar** cmdlet.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>Em um trabalho com base em farm, o PowerShell comando deve ser executado no AD FS servidor primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor AD FS que é um membro do farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 6: Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilitar a autenticação composta bit no atributo msDS-SupportedEncryptionTypes na conta designada para executar o serviço do AD FS usando o **conjunto ADServiceAccount** cmdlet do PowerShell.  

>[!NOTE]
>Se você alterar a conta de serviço, você deve habilitar a autenticação composta manualmente, executando o **Set-ADUser - compoundIdentitySupported: $true** cmdlets do Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reinicie o serviço ADFS.

>[!NOTE]
>Depois de 'CompoundIdentitySupported' é definida como true, instalação da mesma gMSA novo falhe (2012R2/2016) de servidores com o seguinte erro – **ADServiceAccount instalar: não pode instalar a conta de serviço. Mensagem de erro: 'o contexto fornecido não correspondia o destino.'**.
>
>**Solução**: defina temporariamente CompoundIdentitySupported como $false. Nesta etapa faz com que o AD FS parar de emitir WindowsDeviceGroup requerimentos judiciais ou Extrajudiciais. Set-ADServiceAccount-identidade 'Conta de serviço do AD FS' - CompoundIdentitySupported: $false instalar o gMSA no novo servidor e, em seguida, habilitar CompoundIdentitySupported para $True.
Desabilitar CompoundIdentitySupported e depois reabilitar não precisam serviço ADFS seja reiniciado.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 7: Atualizar o provedor de declarações do AD FS confiança do Active Directory

1. Atualize o AD FS requerimentos judiciais ou Extrajudiciais provedor confiança do Active Directory incluir a seguinte regra de declaração 'Passagem' para 'WindowsDeviceGroup' reivindicação.
2.  Em **AD FS gerenciamento**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** e no painel direito, clique em direita **do Active Directory** e selecione **editar regras de declaração**.
3.  Sobre o **editar regras de declaração para o Diretor de ativo** clique **Adicionar regra**.
4.  No **transformar reivindicação regra Assistente para adicionar** selecione **passagem ou filtro uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** do **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir **.  Clique em **aplicar** e **Okey**. 
![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 8: Na parte dependência onde as declarações de 'WindowsDeviceGroup' são esperadas, adicione uma regra de declaração 'Passagem' ou 'Transformar' semelhante.
2.  Em **AD FS gerenciamento**, clique em **confie dependência de terceiros** e no painel direito, clique em direita sua RP e selecione **editar regras de declaração**.
3.  Sobre o **regras de transformação de emissão** clique **Adicionar regra**.
4.  No **transformar reivindicação regra Assistente para adicionar** selecione **passagem ou filtro uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** do **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir **.  Clique em **aplicar** e **Okey**.
![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Etapas para configurar o AD FS no Windows Server 2016
O seguinte detalharemos as etapas para configurar a autenticação composta no AD FS para o Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1: Habilitar o suporte KDC para declarações, autenticação composta e proteção Kerberos na política de controlador de domínio padrão
1.  No Gerenciador do servidor, selecione Ferramentas, **Group Policy Management**.
2.  Navegar para baixo até o **política de controlador de domínio padrão**, clique com botão direito e selecione **editar**.
3.  Sobre o **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **KDC**.
4.  No painel direito, clique duas vezes em **suporte KDC para declarações, autenticação composta e proteção Kerberos**.
5.  Na janela nova caixa de diálogo, defina o suporte KDC para declarações para **Enabled**.
6.  Em Opções, selecione **Supported** do menu suspenso e depois clique em **aplicar** e **Okey**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte de cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em uma política de grupo aplicado aos computadores acessar aplicativos federados, no **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **administrativas Modelos de**, expanda **sistema**e selecione **Kerberos**.
2.  No painel à direita da janela do Editor de gerenciamento de política de grupo, clique duas vezes em **suporte do cliente Kerberos declarações, autenticação composta e proteção Kerberos.**
3.  Na nova janela caixa de diálogo, defina o suporte do cliente Kerberos como **Enabled** e clique em **aplicar** e **Okey**.
4.  Feche o Editor de gerenciamento de política de grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Etapa 3: Definir o provedor de autenticação principal

1. Defina o provedor de autenticação principal como **autenticação do Windows** para configurações do AD FS Intranet.
2. No AD FS gerenciamento, sob **políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** clique **editar**.
3. Em **Editar política de autenticação Global** em **Intranet** selecione **autenticação do Windows**.
4. Clique em **aplicar** e **Okey**.
5. Usando o PowerShell, você pode usar o **conjunto AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Em um trabalho com base em farm, o PowerShell comando deve ser executado no AD FS servidor primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor AD FS que é um membro do farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 4: Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilitar a autenticação composta bit no atributo msDS-SupportedEncryptionTypes na conta designada para executar o serviço do AD FS usando o **conjunto ADServiceAccount** cmdlet do PowerShell.  

>[!NOTE]
>Se você alterar a conta de serviço, você deve habilitar a autenticação composta manualmente, executando o **Set-ADUser - compoundIdentitySupported: $true** cmdlets do Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reinicie o serviço ADFS.

>[!NOTE]
>Depois de 'CompoundIdentitySupported' é definida como true, instalação da mesma gMSA novo falhe (2012R2/2016) de servidores com o seguinte erro – **ADServiceAccount instalar: não pode instalar a conta de serviço. Mensagem de erro: 'o contexto fornecido não correspondia o destino.'**.
>
>**Solução**: defina temporariamente CompoundIdentitySupported como $false. Nesta etapa faz com que o AD FS parar de emitir WindowsDeviceGroup requerimentos judiciais ou Extrajudiciais. Set-ADServiceAccount-identidade 'Conta de serviço do AD FS' - CompoundIdentitySupported: $false instalar o gMSA no novo servidor e, em seguida, habilitar CompoundIdentitySupported para $True.
Desabilitar CompoundIdentitySupported e depois reabilitar não precisam serviço ADFS seja reiniciado.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 5: Atualizar o provedor de declarações do AD FS confiança do Active Directory

1. Atualize o AD FS requerimentos judiciais ou Extrajudiciais provedor confiança do Active Directory incluir a seguinte regra de declaração 'Passagem' para 'WindowsDeviceGroup' reivindicação.
2.  Em **AD FS gerenciamento**, clique em **requerimentos judiciais ou Extrajudiciais provedor confie** e no painel direito, clique em direita **do Active Directory** e selecione **editar regras de declaração**.
3.  Sobre o **editar regras de declaração para o Diretor de ativo** clique **Adicionar regra**.
4.  No **transformar reivindicação regra Assistente para adicionar** selecione **passagem ou filtro uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** do **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir **.  Clique em **aplicar** e **Okey**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 6: Sobre o parceiro dependente onde espera as declarações de 'WindowsDeviceGroup', adicione uma regra de declaração 'Passagem' ou 'Transformar' semelhante.
2.  Em **AD FS gerenciamento**, clique em **confie dependência de terceiros** e no painel direito, clique em direita sua RP e selecione **editar regras de declaração**.
3.  Sobre o **regras de transformação de emissão** clique **Adicionar regra**.
4.  No **transformar reivindicação regra Assistente para adicionar** selecione **passagem ou filtro uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** do **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir **.  Clique em **aplicar** e **Okey**.

## <a name="validation"></a>Validação
Para validar o lançamento do 'WindowsDeviceGroup' requerimentos judiciais ou Extrajudiciais, crie um teste declarações do aplicativo com reconhecimento de uso do .net 4.6. Com o SDK WIF 4.0.
Configurar o aplicativo como um parceiro no ADFS dependente e atualizá-lo com a regra de declaração conforme especificado nas etapas acima.
Durante a autenticação para o aplicativo usando o provedor de autenticação integrada do Windows do ADFS, as declarações a seguir são convertidas.
![Validação](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

As declarações para o computador/dispositivo agora podem ser consumidas para controles de acesso mais avançados.

Por exemplo – a seguinte **AdditionalAuthenticationRules** informa o AD FS para invocar MFA se – a autenticação de usuário não for membro do grupo de segurança "-1-5-21-2134745077-1211275016-3050530490-1117" e o computador (onde o usuário é Autenticação de) não é membro do grupo de segurança "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

No entanto, se qualquer uma das condições acima forem atendidas, não invoca MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```