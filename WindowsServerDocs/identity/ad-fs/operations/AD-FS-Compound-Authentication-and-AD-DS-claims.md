---
title: Composta de declarações de autenticação e serviços de domínio do Active Directory nos serviços de Federação do Active Directory
description: O documento a seguir aborda a autenticação composta e declarações de AD DS no AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 270fb6efd63e6355c410ee45d09e6fd16b14222b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867987"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Compostos de autenticação e declarações do AD DS no AD FS
Windows Server 2012 aprimora a autenticação Kerberos com a introdução de autenticação composta.  Autenticação composta permite que uma solicitação de serviço de concessão de tíquete do Kerberos (TGS) incluir duas identidades: 

- a identidade do usuário
- a identidade do dispositivo do usuário.  

Windows realiza a autenticação composta estendendo [Kerberos flexível autenticação FAST (encapsulamento seguro) ou a proteção Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

O AD FS 2012 e versões posteriores permite o consumo do AD DS emissões de declarações de usuário ou dispositivo que residem em um tíquete de autenticação Kerberos. Nas versões anteriores do AD FS, as declarações mecanismo só pode ler o usuário e grupos de segurança (SIDs) de IDs de Kerberos, mas não foi capaz de ler qualquer declarações informações contidas em um tíquete Kerberos.

Você pode habilitar o controle de acesso avançado para aplicativos federados usando os serviços de domínio do Active Directory (AD DS)-emissões de declarações de usuário e dispositivo juntos, com os serviços de Federação do Active Directory (AD FS).

## <a name="requirements"></a>Requisitos
1.  Os computadores que acessam aplicativos federados, deve ser autenticado usando o AD FS **autenticação integrada do Windows**. 
    - Autenticação integrada do Windows só está disponível ao conectar-se aos servidores back-end do AD FS.
    - Computadores devem ser capazes de alcançar os servidores back-end do AD FS para o nome do serviço de Federação
    - Servidores do AD FS deve oferecer autenticação integrada do Windows como um método de autenticação primária em suas configurações de Intranet.

2.  A diretiva **suporte a cliente Kerberos para autenticação de declarações composta e proteção Kerberos** deve ser aplicada a todos os computadores que acessam aplicativos federados que são protegidos por autenticação composta. Isso é aplicável no caso de floresta única ou entre cenários de floresta.

3.  O domínio que hospeda os servidores do AD FS deve ter o **suporte KDC para declarações de autenticação composta e proteção Kerberos** configuração da política aplicada aos controladores de domínio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Etapas para configurar o AD FS no Windows Server 2012 R2
Use as seguintes etapas para configurar as declarações e autenticação composta 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1:  Habilitar o suporte KDC para declarações, autenticação composta e proteção Kerberos na diretiva de controlador de domínio padrão
1.  No Gerenciador do servidor, selecione Ferramentas, **gerenciamento de política de grupo**.
2.  Navegue para baixo até a **política de controlador de domínio padrão**, clique com botão direito e selecione **editar**.
![Gerenciamento de diretiva de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Sobre o **Editor de gerenciamento de diretiva de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos** , expanda **System**e selecione **KDC**.
4.  No painel direito, clique duas vezes **suporte KDC para declarações, autenticação composta e proteção Kerberos**.
![Gerenciamento de diretiva de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Na nova janela de caixa de diálogo, defina o suporte KDC para declarações **Enabled**.
6.  Em Opções, escolha **com suporte** do menu suspenso e clique **Apply** e **Okey**.
![Gerenciamento de diretiva de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte a cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em uma política de grupo aplicadas aos computadores que acessam aplicativos federados, nos **Editor de gerenciamento de diretiva de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **Kerberos**.
2.  No painel direito da janela do Editor de gerenciamento de diretiva de grupo, clique duas vezes em **suporte a cliente Kerberos para declarações, autenticação composta e proteção Kerberos.**
3.  Na nova janela de caixa de diálogo, defina o suporte a cliente Kerberos **Enabled** e clique em **Apply** e **Okey**.
![Gerenciamento de diretiva de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Feche o Editor de Gerenciamento de Política de Grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Etapa 3: Certifique-se de que os servidores do AD FS foram atualizados.
Você precisa garantir que as seguintes atualizações são instaladas nos servidores do AD FS.

|Atualização|Descrição|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Atualização cumulativa de segurança (inclui o KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Atualização para o Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Essa atualização adiciona suporte para declarações de ID compostas nos serviços de Federação do Active Directory.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Etapa 4: Configurar o provedor de autenticação primária

1. Definir o provedor de autenticação primária **autenticação do Windows** para configurações de Intranet do AD FS.
2. No gerenciamento do AD FS, sob **políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** clique **editar**.
3. Na **Editar política de autenticação Global** sob **Intranet** selecionar **autenticação Windows**.
4. Clique em **aplique** e **Okey**.

![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Usando o PowerShell, você pode usar o **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Um WID baseado em farm, o PowerShell comando deve ser executado no servidor do AD FS primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor do AD FS que é um membro do farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Etapa 5:  Adicionar a descrição de declaração para o AD FS
1.  Adicione a seguinte descrição de declaração ao farm. Essa descrição de declaração não está presente por padrão no AD FS 2012 R2 e precisa ser adicionado manualmente.
2.  No gerenciamento do AD FS, sob **Service**, clique com botão direito **descrição de declaração** e selecione **adicionar descrição de declaração**
3.  Insira as seguintes informações na descrição de declaração
    - Nome de exibição: 'Grupo de dispositivos do Windows' 
    - Descrição de declaração: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Colocar uma marca de seleção em ambas as caixas.
5. Clique em **OK**.

![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Usando o PowerShell, você pode usar o **AdfsClaimDescription adicionar** cmdlet.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>Um WID baseado em farm, o PowerShell comando deve ser executado no servidor do AD FS primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor do AD FS que é um membro do farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 6:  Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilitar a autenticação composta bit no atributo msDS-SupportedEncryptionTypes da conta designada para executar o serviço do AD FS usando o **Set-ADServiceAccount** cmdlet do PowerShell.  

>[!NOTE]
>Se você alterar a conta de serviço, você deve habilitar manualmente a autenticação composta executando o **Set-ADUser - compoundIdentitySupported: $true** cmdlets do Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reinicie o serviço do ADFS.

>[!NOTE]
>Depois de 'CompoundIdentitySupported' está definido como true, instalação da mesma gMSA no novo falha de servidores (2012R2/2016) com o seguinte erro – **Install-ADServiceAccount: Não é possível instalar a conta de serviço. Mensagem de erro: 'O contexto fornecido não correspondeu ao destino.'** .
>
>**Solução**: Defina temporariamente CompoundIdentitySupported como $false. Esta etapa faz com que o ADFS interromper emissora WindowsDeviceGroup declarações. Set-ADServiceAccount-identidade 'Conta de serviço do ADFS' - CompoundIdentitySupported: $false instalar a gMSA no novo servidor e, em seguida, habilitar CompoundIdentitySupported para $True.
CompoundIdentitySupported de desabilitação e reabilitação, em seguida, não precisam de serviço do ADFS para ser reiniciado.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 7: Relação de confiança do provedor de declarações de atualização do AD FS para o Active Directory

1. Atualize o AD FS provedor confiável de declarações para o Active Directory incluir a seguinte regra de declaração de 'Passagem' para 'WindowsDeviceGroup' declaração.
2.  Na **gerenciamento do AD FS**, clique em **confianças do provedor de declarações** e, no painel direito, ao clicar **do Active Directory** e selecione **editar regras de declaração**.
3.  Sobre o **editar regras de declaração para o Active Directory** clique em **Adicionar regra**.
4.  Sobre o **Adicionar Assistente de regra de declaração de transformação** selecionar **passar ou filtrar uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** da **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir**.  Clique em **aplique** e **Okey**. 
![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 8: Sobre a terceira parte confiável em que as declarações de 'WindowsDeviceGroup' são esperadas, adicione uma regra de declaração 'Passagem' ou 'Transformar' semelhante.
2.  Na **gerenciamento do AD FS**, clique em **terceira** e, no painel direito, ao clicar o RP e selecione **editar regras de declaração**.
3.  Sobre o **regras de transformação de emissão** clique em **Adicionar regra**.
4.  Sobre o **Adicionar Assistente de regra de declaração de transformação** selecionar **passar ou filtrar uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** da **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir**.  Clique em **aplique** e **Okey**.
![Descrição de declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Etapas para configurar o AD FS no Windows Server 2016
O exemplo a seguir detalha as etapas para configurar a autenticação composta no AD FS do Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1:  Habilitar o suporte KDC para declarações, autenticação composta e proteção Kerberos na diretiva de controlador de domínio padrão
1.  No Gerenciador do servidor, selecione Ferramentas, **gerenciamento de política de grupo**.
2.  Navegue para baixo até a **política de controlador de domínio padrão**, clique com botão direito e selecione **editar**.
3.  Sobre o **Editor de gerenciamento de diretiva de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos** , expanda **System**e selecione **KDC**.
4.  No painel direito, clique duas vezes **suporte KDC para declarações, autenticação composta e proteção Kerberos**.
5.  Na nova janela de caixa de diálogo, defina o suporte KDC para declarações **Enabled**.
6.  Em Opções, escolha **com suporte** do menu suspenso e clique **Apply** e **Okey**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte a cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em uma política de grupo aplicadas aos computadores que acessam aplicativos federados, nos **Editor de gerenciamento de diretiva de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **Kerberos**.
2.  No painel direito da janela do Editor de gerenciamento de diretiva de grupo, clique duas vezes em **suporte a cliente Kerberos para declarações, autenticação composta e proteção Kerberos.**
3.  Na nova janela de caixa de diálogo, defina o suporte a cliente Kerberos **Enabled** e clique em **Apply** e **Okey**.
4.  Feche o Editor de Gerenciamento de Política de Grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Etapa 3: Configurar o provedor de autenticação primária

1. Definir o provedor de autenticação primária **autenticação do Windows** para configurações de Intranet do AD FS.
2. No gerenciamento do AD FS, sob **políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** clique **editar**.
3. Na **Editar política de autenticação Global** sob **Intranet** selecionar **autenticação Windows**.
4. Clique em **aplique** e **Okey**.
5. Usando o PowerShell, você pode usar o **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Um WID baseado em farm, o PowerShell comando deve ser executado no servidor do AD FS primário.
>Em um farm SQL com base, o comando do PowerShell pode ser executado em qualquer servidor do AD FS que é um membro do farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 4:  Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilitar a autenticação composta bit no atributo msDS-SupportedEncryptionTypes da conta designada para executar o serviço do AD FS usando o **Set-ADServiceAccount** cmdlet do PowerShell.  

>[!NOTE]
>Se você alterar a conta de serviço, você deve habilitar manualmente a autenticação composta executando o **Set-ADUser - compoundIdentitySupported: $true** cmdlets do Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reinicie o serviço do ADFS.

>[!NOTE]
>Depois de 'CompoundIdentitySupported' está definido como true, instalação da mesma gMSA no novo falha de servidores (2012R2/2016) com o seguinte erro – **Install-ADServiceAccount: Não é possível instalar a conta de serviço. Mensagem de erro: 'O contexto fornecido não correspondeu ao destino.'** .
>
>**Solução**: Defina temporariamente CompoundIdentitySupported como $false. Esta etapa faz com que o ADFS interromper emissora WindowsDeviceGroup declarações. Set-ADServiceAccount-identidade 'Conta de serviço do ADFS' - CompoundIdentitySupported: $false instalar a gMSA no novo servidor e, em seguida, habilitar CompoundIdentitySupported para $True.
CompoundIdentitySupported de desabilitação e reabilitação, em seguida, não precisam de serviço do ADFS para ser reiniciado.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 5: Relação de confiança do provedor de declarações de atualização do AD FS para o Active Directory

1. Atualize o AD FS provedor confiável de declarações para o Active Directory incluir a seguinte regra de declaração de 'Passagem' para 'WindowsDeviceGroup' declaração.
2.  Na **gerenciamento do AD FS**, clique em **confianças do provedor de declarações** e, no painel direito, ao clicar **do Active Directory** e selecione **editar regras de declaração**.
3.  Sobre o **editar regras de declaração para o Active Directory** clique em **Adicionar regra**.
4.  Sobre o **Adicionar Assistente de regra de declaração de transformação** selecionar **passar ou filtrar uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** da **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir**.  Clique em **aplique** e **Okey**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 6: Sobre a terceira parte confiável em que as declarações de 'WindowsDeviceGroup' são esperadas, adicione uma regra de declaração 'Passagem' ou 'Transformar' semelhante.
2.  Na **gerenciamento do AD FS**, clique em **terceira** e, no painel direito, ao clicar o RP e selecione **editar regras de declaração**.
3.  Sobre o **regras de transformação de emissão** clique em **Adicionar regra**.
4.  Sobre o **Adicionar Assistente de regra de declaração de transformação** selecionar **passar ou filtrar uma declaração de entrada** e clique em **próxima**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos do Windows** da **tipo de declaração de entrada** lista suspensa.
6.  Clique em **concluir**.  Clique em **aplique** e **Okey**.

## <a name="validation"></a>Validação
Para validar a versão de declarações de 'WindowsDeviceGroup', crie um teste usando o .net 4.6 de aplicativo com reconhecimento de declarações. Com o WIF SDK 4.0.
Configurar o aplicativo como uma terceira parte confiável no AD FS e atualizá-lo com a regra de declaração, conforme especificado nas etapas acima.
Ao autenticar para o aplicativo usando o provedor de autenticação integrada do Windows do AD FS, as seguintes declarações são convertidas.
![Validação](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

As declarações para o computador/dispositivo agora podem ser consumidas para controles de acesso mais avançados.

Por exemplo, o seguinte **AdditionalAuthenticationRules** informa ao AD FS para invocar o MFA se – o usuário de autenticação não é membro do grupo de segurança "-1-5-21-2134745077-1211275016-3050530490-1117" e o computador (em que é o usuário é a autenticação do) não é membro do grupo de segurança "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

No entanto, se qualquer uma das condições acima forem atendidas, não invoque MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
