---
title: Configurar o Serviço Web de Registro de Certificado para renovação baseada em chave de certificados em uma porta personalizada
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/12/2019
ms.topic: article
ms.openlocfilehash: 5e8618853e28c6deef4a15e84361e339c70bf052
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940330"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>Configurar o Serviço Web de Registro de Certificado para renovação baseada em chave de certificados em uma porta personalizada

> Autores: Jitesh Thakur, Meera Mohideen, consultores técnicos com o grupo do Windows.
Engenheiro de suporte do ankit Tyagi com o grupo do Windows

## <a name="summary"></a>Resumo

Este artigo fornece instruções passo a passo para implementar o Serviço Web de Política de Registro de Certificado (CEP) e o Serviço Web de Registro de Certificado (CES) em uma porta personalizada diferente de 443 para a renovação baseada em chave de certificado para aproveitar o recurso de renovação automática do CEP e do CES.

Este artigo também explica como o CEP e o CES funcionam e fornece diretrizes de instalação.

> [!Note]
> O fluxo de trabalho incluído neste artigo se aplica a um cenário específico. O mesmo fluxo de trabalho pode não funcionar para uma situação diferente. No entanto, os princípios permanecem os mesmos.
>
> Isenção de responsabilidade: essa configuração é criada para um requisito específico no qual você não deseja usar a porta 443 para a comunicação HTTPS padrão para servidores CEP e CES. Embora essa configuração seja possível, ela tem suporte limitado. Os serviços de atendimento ao cliente e o suporte podem ajudá-lo a se seguir este guia com cuidado usando o desvio mínimo da configuração de servidor Web fornecida.

## <a name="scenario"></a>Cenário

Para este exemplo, as instruções são baseadas em um ambiente que usa a seguinte configuração:

- Uma floresta Contoso.com que tem uma PKI (infraestrutura de chave pública) dos serviços de certificado Active Directory (AD CS).

- Duas instâncias CEP/CES configuradas em um servidor que está sendo executado em uma conta de serviço. Uma instância usa o nome de usuário e a senha para o registro inicial. O outro usa a autenticação baseada em certificado para a renovação baseada em chave no modo somente renovação.

- Um usuário tem um grupo de trabalho ou um computador não ingressado no domínio para o qual ele registrará o certificado do computador usando credenciais de nome de usuário e senha.

- A conexão do usuário para o CEP e o CES sobre HTTPS ocorre em uma porta personalizada, como 49999. (Essa porta é selecionada em um intervalo de portas dinâmicas e não é usada como uma porta estática por qualquer outro serviço.)

- Quando o tempo de vida do certificado está perto de seu fim, o computador usa a renovação baseada em chave de CES com base em certificado para renovar o certificado no mesmo canal.

![implantação](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>Instruções de configuração

### <a name="overview"></a>Visão geral

1. Configure o modelo para a renovação baseada em chave.

2. Como pré-requisito, configure um servidor CEP e CES para autenticação de nome de usuário e senha.
   Nesse ambiente, nos referimos à instância como "CEPCES01".

3.  Configure outra instância de CEP e CES usando o PowerShell para autenticação baseada em certificado no mesmo servidor. A instância do CES usará uma conta de serviço.

    Nesse ambiente, nos referimos à instância como "CEPCES02". A conta de serviço usada é "cepcessvc".

4.  Defina as configurações do lado do cliente.

### <a name="configuration"></a>Configuração

Esta seção fornece as etapas para configurar o registro inicial.

> [!Note]
> Você também pode configurar qualquer conta de serviço de usuário, MSA ou GMSA para o CES funcionar.

Como pré-requisito, você deve configurar o CEP e o CES em um servidor usando a autenticação de nome de usuário e senha.

#### <a name="configure-the-template-for-key-based-renewal"></a>Configurar o modelo para a renovação baseada em chave

Você pode duplicar um modelo de computador existente e definir as seguintes configurações do modelo:

1. Na guia nome da entidade do modelo de certificado, verifique se a opção **fornecer nas opções solicitar** e **usar informações de assunto de certificados existentes para solicitações de renovação de registro automático** está selecionada.
   ![Novos modelos](media/certificate-enrollment-certificate-key-based-renewal-2.png)

2. Alterne para a guia **requisitos de emissão** e marque a caixa de seleção aprovação do Gerenciador de certificados de autoridade de **certificação** .
   ![Requisitos de emissão](media/certificate-enrollment-certificate-key-based-renewal-3.png)

3. Atribua a permissão **ler** e **registrar** à conta de serviço **cepcessvc** para este modelo.

4. Publique o novo modelo na autoridade de certificação.

> [!Note]
> Verifique se as configurações de compatibilidade no modelo estão definidas para o **Windows Server 2012 R2** , pois há um problema conhecido em que os modelos não estarão visíveis se a compatibilidade estiver definida como Windows Server 2016 ou versão posterior. Para obter mais informações, consulte [não é possível selecionar modelos de certificado compatíveis com a AC do Windows server 2016 do Windows server 2016 ou servidores de CAS ou CEP baseados em versões posteriores ](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016).


#### <a name="configure-the-cepces01-instance"></a>Configurar a instância de CEPCES01

##### <a name="step-1-install-the-instance"></a>Etapa 1: instalar a instância

Para instalar a instância do CEPCES01, use um dos métodos a seguir.

**Método 1**

Consulte os artigos a seguir para obter orientações passo a passo para habilitar o CEP e o CES para autenticação de nome de usuário e senha:

[Diretrizes de Serviço Web de Política de Registro de Certificado](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[Diretrizes de Serviço Web de Registro de Certificado](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> Certifique-se de que você não selecionou a opção "Habilitar renovação baseada em chave" se configurar as instâncias CEP e CES de autenticação de nome de usuário e senha.

**Método 2**

Você pode usar os seguintes cmdlets do PowerShell para instalar as instâncias de CEP e CES:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature Adcs-Enroll-Web-Pol
Add-WindowsFeature Adcs-Enroll-Web-Svc
```

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Username -SSLCertThumbprint "sslCertThumbPrint"
```

Esse comando instala o Serviço Web de Política de Registro de Certificado (CEP) especificando que um nome de usuário e uma senha são usados para autenticação.

> [!Note]
> Nesse comando, \<**SSLCertThumbPrint**\> é a impressão digital do certificado que será usado para associar o IIS.

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

Esse comando instala o Serviço Web de Registro de Certificado (CES) para usar a autoridade de certificação para um nome de computador de **CA1.contoso.com** e um nome comum de CA de **contoso-CA1-CA**. A identidade do CES é especificada como a identidade do pool de aplicativos padrão. O tipo de autenticação é **username**. SSLCertThumbPrint é a impressão digital do certificado que será usado para associar o IIS.

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>Etapa 2 verificar o console do Gerenciador do Serviços de Informações da Internet (IIS)

Após uma instalação bem-sucedida, você espera ver a exibição a seguir no console do Gerenciador do Serviços de Informações da Internet (IIS).
![Gerenciador do IIS](media/certificate-enrollment-certificate-key-based-renewal-4.png)

Em **site padrão**, selecione **ADPolicyProvider_CEP_UsernamePassword**e, em seguida, abra **configurações do aplicativo**. Observe a **ID** e o **URI**.

Você pode adicionar um **nome amigável** para o gerenciamento.

#### <a name="configure-the-cepces02-instance"></a>Configurar a instância de CEPCES02

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>Etapa 1: Instale o CEP e o CES para a renovação baseada em chave no mesmo servidor.

Execute o seguinte comando no PowerShell:

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

Esse comando instala o Serviço Web de Política de Registro de Certificado (CEP) e especifica que um certificado é usado para autenticação.

> [!Note]
> Nesse comando, \<SSLCertThumbPrint\> é a impressão digital do certificado que será usado para associar o IIS.

A renovação baseada em chave permite que os clientes de certificado renovem seus certificados usando a chave de seu certificado existente para autenticação. Quando estiver no modo de renovação baseado em chave, o serviço retornará apenas os modelos de certificado definidos para a renovação baseada em chave.

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

Esse comando instala o Serviço Web de Registro de Certificado (CES) para usar a autoridade de certificação para um nome de computador de **CA1.contoso.com** e um nome comum de CA de **contoso-CA1-CA**.

Nesse comando, a identidade do Serviço Web de Registro de Certificado é especificada como a conta de serviço **cepcessvc** . O tipo de autenticação é **certificado**. **SSLCertThumbPrint** é a impressão digital do certificado que será usado para associar o IIS.

O cmdlet **RenewalOnly** permite que o CES seja executado no modo somente renovação. O cmdlet **AllowKeyBasedRenewal** também especifica que o CES aceitará solicitações de renovação com base em chave para o servidor de registro. Eles são certificados de cliente válidos para autenticação que não são mapeados diretamente para uma entidade de segurança.

> [!Note]
> A conta de serviço deve fazer parte do grupo **IISUsers** no servidor.

##### <a name="step-2-check-the-iis-manager-console"></a>Etapa 2 verificar o console do Gerenciador do IIS

Após uma instalação bem-sucedida, você espera ver a exibição a seguir no console do Gerenciador do IIS.
![Gerenciador do IIS](media/certificate-enrollment-certificate-key-based-renewal-5.png)

Selecione **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** em **site padrão** e abra **configurações do aplicativo**. Anote a **ID** e o **URI**. Você pode adicionar um **nome amigável** para o gerenciamento.

> [!Note]
> Se a instância estiver instalada em um novo servidor, clique duas vezes na ID para certificar-se de que a ID é a mesma que foi gerada na instância CEPCES01. Você pode copiar e colar o valor diretamente se ele for diferente.

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>Concluir a configuração de serviços Web de registro de certificado

Para poder registrar o certificado em nome da funcionalidade do CEP e do CES, você precisa configurar a conta de computador do grupo de trabalho no Active Directory e configurar a delegação restrita na conta de serviço.

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>Etapa 1: criar uma conta de computador do computador do grupo de trabalho no Active Directory

Essa conta será usada para autenticação em relação à renovação baseada em chave e à opção "publicar no Active Directory" no modelo de certificado.

> [!Note]
> Você não precisa ingressar no domínio no computador cliente. Essa conta entra em imagem durante a autenticação baseada em certificado no KBR para o serviço dsmapper.

![Novo objeto](media/certificate-enrollment-certificate-key-based-renewal-6.png)

##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>Etapa 2: configurar a conta de serviço para a delegação restrita (S4U2Self)

Execute o seguinte comando do PowerShell para habilitar a delegação restrita (S4U2Self ou qualquer protocolo de autenticação):

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> Nesse comando, \<cepcessvc\> é a conta de serviço e <CA1.contoso.com >é a autoridade de certificação.

> [!Important]
> Não estamos habilitando o sinalizador RENEWALONBEHALOF na autoridade de certificação nessa configuração porque estamos usando a delegação restrita para fazer o mesmo trabalho para nós. Isso nos permite evitar a adição da permissão para a conta de serviço à segurança da autoridade de certificação.

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>Etapa 3: configurar uma porta personalizada no servidor Web do IIS

1. No console do Gerenciador do IIS, selecione site padrão.

2. No painel Ação, selecione Editar Associação de site.

3. Altere a configuração de porta padrão de 443 para sua porta personalizada. A captura de tela de exemplo mostra uma configuração de porta de 49999.
   ![Alterar porta](media/certificate-enrollment-certificate-key-based-renewal-7.png)

##### <a name="step-4-edit-the-ca-enrollment-services-object-on-active-directory"></a>Etapa 4: editar o objeto de serviços de registro de autoridade de certificação no Active Directory

1. Em um controlador de domínio, abra ADSIEdit. msc.

2. [Conecte-se à partição de configuração](/previous-versions/windows/it-pro/windows-server-2003/ff730188(v=ws.10))e navegue até o objeto serviços de registro de autoridade de certificação:

   CN = ENTCA, CN = Serviços de registro, CN = Serviços de chave pública, CN = Serviços, CN = Configuração, DC = contoso, DC = com

3. Clique com o botão direito do mouse e edite o objeto CA. Altere o atributo **msPKI-Enroll-Servers** usando a porta personalizada com seus URIs de servidor CEP e CES que foram encontrados nas configurações do aplicativo. Por exemplo:

   ```
   140https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0
   181https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1
   ```

   ![ADSI Edit](media/certificate-enrollment-certificate-key-based-renewal-8.png)

#### <a name="configure-the-client-computer"></a>Configurar o computador cliente

No computador cliente, configure as políticas de registro e a política de registro automático. Para fazer isso, execute estas etapas:

1. Selecione **Iniciar**  >  **execução**e digite **gpedit. msc**.

2. Vá para **configuração do computador**  >  **configurações do Windows**configurações  >  de**segurança**e clique em políticas de **chave pública**.

3. Habilite a **política de registro automático de cliente dos serviços de certificados** para corresponder às configurações na captura de tela a seguir.
   ![Política de grupo de certificados](media/certificate-enrollment-certificate-key-based-renewal-9.png)

4. Habilitar **a política de registro de certificado do cliente dos serviços de certificados**.

   a. Clique em **Adicionar** para adicionar a política de registro e insira o URI do CEP com **UserNamePassword** que editamos em ADSI.

   b. Para **tipo de autenticação**, selecione **nome de usuário/senha**.

   c. Defina uma prioridade de **10**e, em seguida, valide o servidor de políticas.
      ![Política de registro](media/certificate-enrollment-certificate-key-based-renewal-10.png)

   > [!Note]
   > Certifique-se de que o número da porta seja adicionado ao URI e seja permitido no firewall.

5. Registre o primeiro certificado para o computador por meio de certlm. msc.
   ![Política de registro](media/certificate-enrollment-certificate-key-based-renewal-11.png)

   Selecione o modelo KBR e registre o certificado.
   ![Política de registro](media/certificate-enrollment-certificate-key-based-renewal-12.png)

6. Abra **gpedit. msc** novamente. Edite a **política de registro cliente de serviços de certificados – certificado**e adicione a política de registro de renovação baseada em chave:

   a. Clique em **Adicionar**, insira o URI do CEP com o **certificado** que editamos no ADSI.

   b. Defina uma prioridade de **1**e, em seguida, valide o servidor de políticas. Você será solicitado a autenticar e escolher o certificado que registramos inicialmente.

   ![Política de registro](media/certificate-enrollment-certificate-key-based-renewal-13.png)

> [!Note]
> Certifique-se de que o valor de prioridade da política de registro de renovação baseada em chave seja menor que a prioridade da prioridade de política de registro de senha de nome de usuário. A primeira preferência é dada à prioridade mais baixa.

## <a name="testing-the-setup"></a>Testando a instalação

Para garantir que a renovação automática esteja funcionando, verifique se a renovação manual funciona renovando o certificado com a mesma chave usando o MMC. Além disso, você deve ser solicitado a selecionar um certificado durante a renovação. Você pode escolher o certificado que registramos anteriormente. O prompt é esperado.

Abra o repositório de certificados pessoais do computador e adicione a exibição "certificados arquivados". Para fazer isso, adicione o snap-in de conta do computador local a mmc.exe, realce **certificados (computador local)** clicando nele, clique em **Exibir** na **guia ação** à direita ou na parte superior do MMC, clique em **Opções de exibição**, selecione **certificados arquivados**e clique em **OK**.

### <a name="method-1"></a>Método 1

Execute o seguinte comando:

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![.](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>Método 2

Avance a hora e a data no computador cliente para o horário de renovação do modelo de certificado.

Por exemplo, o modelo de certificado tem uma configuração de validade de 2 dias e uma configuração de renovação de 8 horas configurada. O certificado de exemplo foi emitido às 4:00 A.M. no dia 18 do mês, expira às 4:00 da manhã no dia 20. O mecanismo de registro automático é disparado na reinicialização e a cada intervalo de 8 horas (aproximadamente).

Portanto, se você adiantar o tempo até 8:10 P.M. no dia 19, uma vez que nossa janela de renovação foi definida como 8 horas no modelo, executar certutil-Pulse (para disparar o mecanismo da AE) registra o certificado para você.

![.](media/certificate-enrollment-certificate-key-based-renewal-15.png)

Após a conclusão do teste, reverta a configuração de hora para o valor original e reinicie o computador cliente.

> [!Note]
> A captura de tela anterior é um exemplo para demonstrar que o mecanismo de registro automático funciona conforme o esperado porque a data da autoridade de certificação ainda está definida como 18. Portanto, ele continua a emitir certificados. Em uma situação real, essa grande quantidade de renovações não ocorrerá.

## <a name="references"></a>Referências

[Test Lab Guide: Demonstrating Certificate Key-Based Renewal](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3dws.11))

[Serviços Web de registro de certificado](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[Install-AdcsEnrollmentPolicyWebService](/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice?view=win10-ps)

[Install-AdcsEnrollmentWebService](/powershell/module/adcsdeployment/install-adcsenrollmentwebservice?view=win10-ps)

Consulte também

[Fórum de Segurança do Windows Server](https://aka.ms/adcsforum)

[Perguntas frequentes sobre PKI (infraestrutura de chave pública) do AD CS (Serviços de Certificados do Active Directory)](https://aka.ms/adcsfaq)

[Biblioteca e referência de documentação do Windows PKI](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/windows-pki-documentation-reference/ba-p/1128393)

[Blog do Windows PKI](/archive/blogs/pki/)

[Como configurar a delegação restrita de Kerberos (somente S4U2Proxy ou Kerberos) em uma conta de serviço personalizada para páginas de proxy de registro da Web](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)
