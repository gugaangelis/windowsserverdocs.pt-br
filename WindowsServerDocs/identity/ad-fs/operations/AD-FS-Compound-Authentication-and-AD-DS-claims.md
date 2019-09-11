---
title: Autenticação composta e declarações de Active Directory Domain Services no Serviços de Federação do Active Directory (AD FS)
description: O documento a seguir discute a autenticação composta e as declarações de AD DS no AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b67177c8bf0ce9869aa51c3012d57f3208ac02f5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866292"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Compostos de autenticação e declarações do AD DS no AD FS
O Windows Server 2012 aprimora a autenticação Kerberos introduzindo a autenticação composta.  A autenticação composta permite que uma solicitação de TGS (serviço de concessão de tíquete) Kerberos inclua duas identidades: 

- a identidade do usuário
- a identidade do dispositivo do usuário.  

O Windows realiza a autenticação composta ao estender o [túnel seguro de autenticação flexível do Kerberos (rápido) ou a proteção Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 e versões posteriores permitem o consumo de AD DS declarações de usuário ou dispositivo emitidas que residem em um tíquete de autenticação Kerberos. Nas versões anteriores do AD FS, o mecanismo de declarações podia ler apenas as SIDs (IDs de segurança) de usuário e grupo do Kerberos, mas não foi capaz de ler nenhuma informação de declaração contida em um tíquete Kerberos.

Você pode habilitar o controle de acesso mais rico para aplicativos federados usando declarações de usuário e dispositivo emitidas por Active Directory Domain Services (AD DS) junto com Serviços de Federação do Active Directory (AD FS) (AD FS).

## <a name="requirements"></a>Requisitos
1.  Os computadores que acessam aplicativos federados devem se autenticar no AD FS usando a **autenticação integrada do Windows**. 
    - A autenticação integrada do Windows só está disponível durante a conexão com os servidores de AD FS de back-end.
    - Os computadores devem ser capazes de acessar os servidores de AD FS de back-end para Serviço de Federação nome
    - Os servidores de AD FS devem oferecer a autenticação integrada do Windows como um método de autenticação principal em suas configurações de intranet.

2.  A política de **suporte ao cliente Kerberos para declarações de autenticação composta e proteção Kerberos** deve ser aplicada a todos os computadores que acessam aplicativos federados protegidos pela autenticação composta. Isso é aplicável no caso de cenários de floresta única ou entre florestas.

3.  O domínio que hospeda os servidores de AD FS deve ter a configuração de política **composta de suporte do KDC e de proteção de Kerberos** aplicada aos controladores de domínio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Etapas para configurar o AD FS no Windows Server 2012 R2
Use as etapas a seguir para configurar a autenticação e declarações compostas 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1:  Habilitar o suporte do KDC para declarações, autenticação composta e proteção Kerberos na política de controlador de domínio padrão
1.  Em Gerenciador do Servidor, selecione Ferramentas, **Gerenciamento de política de grupo**.
2.  Navegue até a **política de controlador de domínio padrão**, clique com o botão direito do mouse e selecione **Editar**.
![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Na **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **KDC**.
4.  No painel direito, clique duas vezes em **suporte do KDC para declarações, autenticação composta e proteção Kerberos**.
![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Na janela da caixa de diálogo novo, defina suporte do KDC para declarações como **habilitadas**.
6.  Em opções, selecione **com suporte** no menu suspenso e clique em **aplicar** e em **OK**.
![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte ao cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em um Política de Grupo aplicado aos computadores que acessam aplicativos federados, na **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema** e selecione **Kerberos**.
2.  No painel direito da janela Editor de Gerenciamento de Política de Grupo, clique duas vezes em **suporte ao cliente Kerberos para declarações, autenticação composta e proteção Kerberos.**
3.  Na janela nova caixa de diálogo, defina suporte ao cliente Kerberos como **habilitado** e clique em **aplicar** e em **OK**.
![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Feche o Editor de Gerenciamento de Política de Grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Etapa 3: Verifique se os servidores de AD FS foram atualizados.
Você precisa garantir que as atualizações a seguir estejam instaladas em seus servidores de AD FS.

|Atualização|Descrição|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Atualização de segurança cumulativa (inclui KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Atualização para o servidor 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Esta atualização adiciona suporte para declarações de ID composta no Serviços de Federação do Active Directory (AD FS).|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Etapa 4: Configurar o provedor de autenticação primário

1. Defina o provedor de autenticação principal como **autenticação do Windows** para AD FS configurações de intranet.
2. Em gerenciamento de AD FS, **em políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** , clique em **Editar**.
3. Em **Editar política de autenticação global** em **intranet** , selecione **autenticação do Windows**.
4. Clique em **aplicar** e em **OK**.

![Gerenciamento de Política de Grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Usando o PowerShell, você pode usar o cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Em um farm baseado em WID, o comando do PowerShell deve ser executado no servidor de AD FS primário.
>Em um farm baseado em SQL, o comando do PowerShell pode ser executado em qualquer AD FS servidor que seja membro do farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Etapa 5:  Adicione a descrição da declaração a AD FS
1. Adicione a seguinte descrição de declaração ao farm. Essa descrição de declaração não está presente por padrão no ADFS 2012 R2 e precisa ser adicionada manualmente.
2. Em gerenciamento de AD FS, em **serviço**, clique com o botão direito do mouse em **Descrição da declaração** e selecione **Adicionar Descrição da declaração**
3. Insira as informações a seguir na descrição da declaração
   - Nome de exibição: ' Grupo de dispositivos Windows ' 
   - Descrição da declaração:<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>' ' '
4. Faça um check-in nas duas caixas.
5. Clique em **OK**.

![Descrição da declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Usando o PowerShell, você pode usar o cmdlet **Add-AdfsClaimDescription** .
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>Em um farm baseado em WID, o comando do PowerShell deve ser executado no servidor de AD FS primário.
>Em um farm baseado em SQL, o comando do PowerShell pode ser executado em qualquer AD FS servidor que seja membro do farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 6:  Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilite o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes na conta que você designou para executar o serviço de AD FS usando o cmdlet do PowerShell **set-ADServiceAccount** .  

>[!NOTE]
>Se você alterar a conta de serviço, deverá habilitar manualmente a autenticação composta executando os cmdlets **set-ADUser-compoundIdentitySupported: $true** Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie o serviço ADFS.

>[!NOTE]
>Depois que ' CompoundIdentitySupported ' for definido como true, a instalação do mesmo gMSA em novos servidores (2012R2/2016) falhará com o seguinte **erro – Install-ADServiceAccount: Não é possível instalar a conta de serviço. Mensagem de erro: ' O contexto fornecido não correspondeu ao destino. '** .
>
>**Solução**: Defina temporariamente CompoundIdentitySupported como $false. Esta etapa faz com que o ADFS pare de emitir declarações WindowsDeviceGroup. Set-ADServiceAccount-Identity ' conta de serviço ADFS '-CompoundIdentitySupported: $false instalar o gMSA no novo servidor e, em seguida, habilitar o CompoundIdentitySupported de volta para $True.
Desabilitar o CompoundIdentitySupported e reabilitar o não precisa que o serviço ADFS seja reiniciado.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 7: Atualizar a AD FS confiança do provedor de declarações para Active Directory

1. Atualize a relação de confiança do provedor de declarações AD FS para Active Directory para incluir a seguinte regra de declaração de "passagem" para a declaração "WindowsDeviceGroup".
2.  Em **Gerenciamento de AD FS**, clique em **relações de confiança do provedor de declarações** e no painel direito, direito, clique em **Active Directory** e selecione **Editar regras de declaração**.
3.  Em **Editar regras de declaração para o director ativo** , clique em **Adicionar regra**.
4.  No **Assistente Adicionar regra de declaração de transformação** , selecione **passar ou filtrar uma declaração de entrada** e clique em **Avançar**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos Windows** na lista suspensa **tipo de declaração de entrada** .
6.  Clique em **Finalizar**.  Clique em **aplicar** e em **OK**. 
![Descrição da declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 8: Na terceira parte confiável em que as declarações ' WindowsDeviceGroup ' são esperadas, adicione uma regra de declaração ' passagem ' ou ' transformação ' semelhante.
2. Em **Gerenciamento de AD FS**, clique em **relações de confiança** de terceira parte confiável e, no painel direito, direito o RP e selecione **Editar regras de declaração**.
3. Nas **regras de transformação de emissão** , clique em **Adicionar regra**.
4. No **Assistente Adicionar regra de declaração de transformação** , selecione **passar ou filtrar uma declaração de entrada** e clique em **Avançar**.
5. Adicione um nome de exibição e selecione **grupo de dispositivos Windows** na lista suspensa **tipo de declaração de entrada** .
6. Clique em **Finalizar**.  Clique em **aplicar** e em **OK**.
   ![Descrição da declaração](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Etapas para configurar o AD FS no Windows Server 2016
O seguinte detalhará as etapas para configurar a autenticação composta no AD FS para o Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Etapa 1:  Habilitar o suporte do KDC para declarações, autenticação composta e proteção Kerberos na política de controlador de domínio padrão
1.  Em Gerenciador do Servidor, selecione Ferramentas, **Gerenciamento de política de grupo**.
2.  Navegue até a **política de controlador de domínio padrão**, clique com o botão direito do mouse e selecione **Editar**.
3.  Na **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema**e selecione **KDC**.
4.  No painel direito, clique duas vezes em **suporte do KDC para declarações, autenticação composta e proteção Kerberos**.
5.  Na janela da caixa de diálogo novo, defina suporte do KDC para declarações como **habilitadas**.
6.  Em opções, selecione **com suporte** no menu suspenso e clique em **aplicar** e em **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Etapa 2: Habilitar o suporte ao cliente Kerberos para declarações, autenticação composta e proteção Kerberos em computadores que acessam aplicativos federados

1.  Em um Política de Grupo aplicado aos computadores que acessam aplicativos federados, na **Editor de gerenciamento de política de grupo**, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **sistema** e selecione **Kerberos**.
2.  No painel direito da janela Editor de Gerenciamento de Política de Grupo, clique duas vezes em **suporte ao cliente Kerberos para declarações, autenticação composta e proteção Kerberos.**
3.  Na janela nova caixa de diálogo, defina suporte ao cliente Kerberos como **habilitado** e clique em **aplicar** e em **OK**.
4.  Feche o Editor de Gerenciamento de Política de Grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Etapa 3: Configurar o provedor de autenticação primário

1. Defina o provedor de autenticação principal como **autenticação do Windows** para AD FS configurações de intranet.
2. Em gerenciamento de AD FS, **em políticas de autenticação**, selecione **autenticação primária** e, em **configurações globais** , clique em **Editar**.
3. Em **Editar política de autenticação global** em **intranet** , selecione **autenticação do Windows**.
4. Clique em **aplicar** e em **OK**.
5. Usando o PowerShell, você pode usar o cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Em um farm baseado em WID, o comando do PowerShell deve ser executado no servidor de AD FS primário.
>Em um farm baseado em SQL, o comando do PowerShell pode ser executado em qualquer AD FS servidor que seja membro do farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Etapa 4:  Habilitar o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes

1.  Habilite o bit de autenticação composta no atributo msDS-SupportedEncryptionTypes na conta que você designou para executar o serviço de AD FS usando o cmdlet do PowerShell **set-ADServiceAccount** .  

>[!NOTE]
>Se você alterar a conta de serviço, deverá habilitar manualmente a autenticação composta executando os cmdlets **set-ADUser-compoundIdentitySupported: $true** Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie o serviço ADFS.

>[!NOTE]
>Depois que ' CompoundIdentitySupported ' for definido como true, a instalação do mesmo gMSA em novos servidores (2012R2/2016) falhará com o seguinte **erro – Install-ADServiceAccount: Não é possível instalar a conta de serviço. Mensagem de erro: ' O contexto fornecido não correspondeu ao destino. '** .
>
>**Solução**: Defina temporariamente CompoundIdentitySupported como $false. Esta etapa faz com que o ADFS pare de emitir declarações WindowsDeviceGroup. Set-ADServiceAccount-Identity ' conta de serviço ADFS '-CompoundIdentitySupported: $false instalar o gMSA no novo servidor e, em seguida, habilitar o CompoundIdentitySupported de volta para $True.
Desabilitar o CompoundIdentitySupported e reabilitar o não precisa que o serviço ADFS seja reiniciado.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Etapa 5: Atualizar a AD FS confiança do provedor de declarações para Active Directory

1. Atualize a relação de confiança do provedor de declarações AD FS para Active Directory para incluir a seguinte regra de declaração de "passagem" para a declaração "WindowsDeviceGroup".
2.  Em **Gerenciamento de AD FS**, clique em **relações de confiança do provedor de declarações** e no painel direito, direito, clique em **Active Directory** e selecione **Editar regras de declaração**.
3.  Em **Editar regras de declaração para o director ativo** , clique em **Adicionar regra**.
4.  No **Assistente Adicionar regra de declaração de transformação** , selecione **passar ou filtrar uma declaração de entrada** e clique em **Avançar**.
5.  Adicione um nome de exibição e selecione **grupo de dispositivos Windows** na lista suspensa **tipo de declaração de entrada** .
6.  Clique em **Finalizar**.  Clique em **aplicar** e em **OK**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Etapa 6: Na terceira parte confiável em que as declarações ' WindowsDeviceGroup ' são esperadas, adicione uma regra de declaração ' passagem ' ou ' transformação ' semelhante.
2. Em **Gerenciamento de AD FS**, clique em **relações de confiança** de terceira parte confiável e, no painel direito, direito o RP e selecione **Editar regras de declaração**.
3. Nas **regras de transformação de emissão** , clique em **Adicionar regra**.
4. No **Assistente Adicionar regra de declaração de transformação** , selecione **passar ou filtrar uma declaração de entrada** e clique em **Avançar**.
5. Adicione um nome de exibição e selecione **grupo de dispositivos Windows** na lista suspensa **tipo de declaração de entrada** .
6. Clique em **Finalizar**.  Clique em **aplicar** e em **OK**.

## <a name="validation"></a>Validação
Para validar a versão das declarações ' WindowsDeviceGroup ', crie um aplicativo de reconhecimento de declarações de teste usando o .NET 4,6. Com o SDK do WIF 4,0.
Configure o aplicativo como uma terceira parte confiável no ADFS e atualize-o com a regra de declaração conforme especificado nas etapas acima.
Ao autenticar para o aplicativo usando o provedor de autenticação integrada do Windows do ADFS, as declarações a seguir são convertidas.
![Confirmação](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

As declarações do computador/dispositivo podem agora ser consumidas para controles de acesso mais ricos.

Por exemplo – o **AdditionalAuthenticationRules** a seguir informa AD FS para invocar o MFA se – o usuário que está Autenticando não é membro do grupo de segurança "-1-5-21-2134745077-1211275016-3050530490-1117" e o computador (onde é o usuário A autenticação de) não é membro do grupo de segurança "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

No entanto, se qualquer uma das condições acima for atendida, não invoque o MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
