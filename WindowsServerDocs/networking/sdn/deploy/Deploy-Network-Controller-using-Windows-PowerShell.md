---
title: Implantar controlador de rede usando o Windows PowerShell
description: Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em um ou mais computadores ou máquinas virtuais (VMs) que estão executando o Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 31c1579dc840f6f4eb805ac4e10f51192a6b4c99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816187"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implantar controlador de rede usando o Windows PowerShell

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em um ou mais VMs (máquinas virtuais) que estão executando o Windows Server 2016.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalado em um host do Hyper-V. Depois que você instalou o controlador de rede em máquinas virtuais no Hyper diferentes três\-hosts V, você deve habilitar o Hyper\-hosts V para a rede definida pelo Software \(SDN\) adicionando hosts usando o controlador de rede o comando do Windows PowerShell **New-NetworkControllerServer**. Ao fazer isso, você habilitará o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Este tópico contém as seguintes seções.

- [Instalar a função de servidor do controlador de rede](#bkmk_role)

- [Configurar o cluster de controlador de rede](#bkmk_configure)

- [Configurar o aplicativo do controlador de rede](#bkmk_app)

- [Validação de implantação do controlador de rede](#bkmk_validation)

- [Comandos adicionais do Windows PowerShell para o controlador de rede](#bkmk_ps)

- [Exemplo de script de configuração de controlador de rede](#bkmk_script)

- [Etapas de pós-implantação para implantações não Kerberos](#bkmk_nonkerb)

## <a name="install-the-network-controller-server-role"></a>Instalar a função de servidor do controlador de rede

Você pode usar este procedimento para instalar a função de servidor do controlador de rede em uma máquina virtual \(VM\).

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalado em um host do Hyper-V. Depois que você instalou o controlador de rede em máquinas virtuais no Hyper diferentes três\-hosts V, você deve habilitar o Hyper\-hosts V para a rede definida pelo Software \(SDN\) adicionando os hosts ao controlador de rede. Ao fazer isso, você habilitará o balanceador de carga de Software SDN à função.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  

>[!NOTE]
>Se você quiser usar o Gerenciador do servidor em vez do Windows PowerShell para instalar o controlador de rede, consulte [instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar o controlador de rede usando o Windows PowerShell, digite os seguintes comandos em um prompt do Windows PowerShell e pressione ENTER.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Instalação do controlador de rede requer que você reinicie o computador. Para fazer isso, digite o seguinte comando e pressione ENTER.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurar o cluster de controlador de rede

O cluster de controlador de rede fornece alta disponibilidade e escalabilidade para o aplicativo do controlador de rede, que você pode configurar depois de criar o cluster e que é hospedado na parte superior do cluster.

>[!NOTE]
>Você pode executar os procedimentos nas seções a seguir diretamente na VM onde você instalou o controlador de rede, ou você pode usar o Remote Server Administration ferramentas para Windows Server 2016 para realizar os procedimentos de um computador remoto que está em execução Windows Server 2016 ou Windows 10. Além disso, a associação **administradores**, ou equivalente, é o mínimo necessário para executar este procedimento. Se o computador ou VM que você instalou o controlador de rede está associado a um domínio, sua conta de usuário deve ser um membro da **os usuários do domínio**.

Você pode criar um cluster de controlador de rede criando um objeto de nó e, em seguida, configurando o cluster.

### <a name="create-a-node-object"></a>Criar um objeto de nó

Você precisa criar um objeto de nó para cada VM que seja membro do cluster de controlador de rede.

Para criar um objeto de nó, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para sua implantação.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

A tabela a seguir fornece descrições para cada parâmetro do **New-NetworkControllerNodeObject** comando.

|Parâmetro|Descrição|
|-------------|---------------|
|Nome|O **nome** parâmetro especifica o nome amigável do servidor que você deseja adicionar ao cluster|
|Servidor|O **Server** parâmetro especifica o nome do host, totalmente o nome de domínio qualificado (FQDN) ou endereço IP do servidor que você deseja adicionar ao cluster. Para computadores que ingressaram no domínio, o FQDN é necessária.|
|FaultDomain|O **FaultDomain** parâmetro especifica o domínio de falha para o servidor que você está adicionando ao cluster. Esse parâmetro define os servidores que podem apresentar falha ao mesmo tempo em que o servidor que você está adicionando ao cluster. Essa falha pode ser devido às dependências físicas compartilhadas, como energia e fontes de sistema de rede. Normalmente, os domínios de falha representam hierarquias relacionadas a essas dependências compartilhadas, com mais probabilidade de falhar juntos em um ponto mais alto na árvore de domínio de falha de servidores. Durante o tempo de execução, o controlador de rede considera os domínios de falha no cluster e tenta distribuir os serviços de controlador de rede para que eles fiquem em domínios de falha separados. Esse processo ajuda a garantir que, em caso de falha de qualquer domínio de falha, que a disponibilidade do serviço e seu estado não seja comprometida. Domínios de falha são especificados em um formato hierárquico. Por exemplo:  "Fd: / Rack1/DC1/Host1", em que DC1 é o nome do data center, Rack1 é o nome do rack e Host1 é o nome do host no qual o nó é colocado.|
|RestInterface|O **RestInterface** parâmetro especifica o nome da interface no nó em que a comunicação do REST Representational State Transfer () é encerrada. Essa interface do controlador de rede recebe solicitações de API Northbound da camada de gerenciamento da rede.|
|NodeCertificate|O **NodeCertificate** parâmetro especifica o certificado que usa o controlador de rede para autenticação de computador. O certificado é necessário se você usar a autenticação baseada em certificado para a comunicação dentro do cluster. o certificado também é usado para criptografia de tráfego entre os serviços de controlador de rede. O nome de assunto do certificado deve ser o mesmo que o nome DNS do nó.|

### <a name="configure-the-cluster"></a>Configurar o cluster

Para configurar o cluster, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para sua implantação.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

A tabela a seguir fornece descrições para cada parâmetro do **Install-NetworkControllerCluster** comando.
  
|Parâmetro|Descrição|
|-------------|---------------|
|ClusterAuthentication|O **ClusterAuthentication** parâmetro especifica o tipo de autenticação que é usado para proteger a comunicação entre nós e também é usado para criptografia de tráfego entre os serviços de controlador de rede. Os valores suportados são **Kerberos**, **X509** e **None**. A autenticação Kerberos usa contas de domínio e só pode ser usada se os nós de controlador de rede estiverem ingressados no domínio. Se você especificar a autenticação baseada em X509, você deve fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve provisionar manualmente o certificado antes de executar esse comando.|
|ManagementSecurityGroup|O **ManagementSecurityGroup** parâmetro especifica o nome do grupo de segurança que contém os usuários que têm permissão para executar os cmdlets de gerenciamento de um computador remoto. Isso é aplicável somente se ClusterAuthentication é Kerberos. Você deve especificar um grupo de segurança de domínio e não um grupo de segurança no computador local.|
|Nó|O **nó** parâmetro especifica a lista de nós de controlador de rede que você criou usando o **New NetworkControllerNodeObject** comando.|
|DiagnosticLogLocation|O **DiagnosticLogLocation** parâmetro especifica o local de compartilhamento em que os logs de diagnóstico são carregados periodicamente. Se você não especificar um valor para esse parâmetro, os logs são armazenados localmente em cada nó. Os logs são armazenados localmente na pasta % systemdrive%\Windows\tracing\SDNDiagnostics. Logs de cluster são armazenados localmente na pasta %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|O **LogLocationCredential** parâmetro especifica as credenciais que são necessárias para acessar o local de compartilhamento onde os logs são armazenados.|
|CredentialEncryptionCertificate|O **CredentialEncryptionCertificate** parâmetro especifica o certificado que usa o controlador de rede para criptografar as credenciais que são usadas para acessar os binários do controlador de rede e o  **LogLocationCredential**, se especificado. O certificado deve ser provisionado em todos os nós de controlador de rede antes de executar esse comando, e o mesmo certificado deve ser registrado em todos os nós do cluster. O uso desse parâmetro para proteger os logs e os binários do controlador de rede é recomendável em ambientes de produção. Sem esse parâmetro, as credenciais são armazenadas em texto não criptografado e podem ser usado indevidamente por qualquer usuário não autorizado.|
|Credential|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **credencial** parâmetro especifica uma conta de usuário que tenha permissão para executar este comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **CertificateThumbprint** parâmetro especifica digital certificado de chave pública (X509) uma conta de usuário que tenha permissão para executar este comando no computador de destino.|
|UseSSL|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **UseSSL** parâmetro especifica o protocolo Secure Sockets Layer (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, SSL não é usado.|
|ComputerName|O **ComputerName** parâmetro especifica o nó do controlador de rede em que esse comando é executado. Se você não especificar um valor para esse parâmetro, o computador local será usado por padrão.|
|LogSizeLimitInMBs|Esse parâmetro especifica o tamanho máximo do log, em MB, o que o controlador de rede pode armazenar. Os logs são armazenados em forma circular. Se DiagnosticLogLocation for fornecido, o valor padrão desse parâmetro é de 40 GB. Se DiagnosticLogLocation não for fornecido, os logs são armazenados em nós de controlador de rede e o valor padrão desse parâmetro é de 15 GB.|
|LogTimeLimitInDays|Esse parâmetro especifica o limite de duração, em dias, para o qual os logs são armazenados. Os logs são armazenados em forma circular. O valor padrão desse parâmetro é de 3 dias.|

## <a name="configure-the-network-controller-application"></a>Configurar o aplicativo do controlador de rede
Para configurar o aplicativo do controlador de rede, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para sua implantação.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

A tabela a seguir fornece descrições para cada parâmetro do **Install-NetworkController** comando.

|Parâmetro|Descrição|
|-------------|---------------|
|ClientAuthentication|O **ClientAuthentication** parâmetro especifica o tipo de autenticação que é usado para proteger a comunicação entre REST e o controlador de rede. Os valores suportados são **Kerberos**, **X509** e **None**. A autenticação Kerberos usa contas de domínio e só pode ser usada se os nós de controlador de rede estiverem ingressados no domínio. Se você especificar a autenticação baseada em X509, você deve fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve provisionar manualmente o certificado antes de executar esse comando.|
|Nó|O **nó** parâmetro especifica a lista de nós de controlador de rede que você criou usando o **New NetworkControllerNodeObject** comando.|
|ClientCertificateThumbprint|Esse parâmetro é necessário somente quando você estiver usando a autenticação baseada em certificado para clientes do controlador de rede. O **ClientCertificateThumbprint** parâmetro especifica a impressão digital do certificado registrado para os clientes na camada Northbound.|
|ServerCertificate|O **ServerCertificate** parâmetro especifica o certificado que usa o controlador de rede para provar sua identidade para os clientes. O certificado do servidor deve incluir a finalidade de autenticação de servidor nas extensões de uso avançado de chave e deve ser emitido por uma autoridade de certificação confiável pelos clientes ao controlador de rede.|
|RESTIPAddress|Você não precisará especificar um valor para **RESTIPAddress** com uma implantação de nó único do controlador de rede. Para implantações de vários nós, o **RESTIPAddress** parâmetro especifica o endereço IP do ponto de extremidade REST na notação CIDR. Por exemplo, 192.168.1.10/24. O valor do nome da entidade de **ServerCertificate** deve ser resolvido para o valor da **RESTIPAddress** parâmetro. Esse parâmetro deve ser especificado para todas as implantações de controlador de rede de vários nós quando todos os nós estiverem na mesma sub-rede. Se nós estiverem em sub-redes diferentes, você deve usar o **RestName** parâmetro em vez de usar **RESTIPAddress**.|
|RestName|Você não precisará especificar um valor para **RestName** com uma implantação de nó único do controlador de rede. A única vez que você deve especificar um valor para **RestName** é quando as implantações de vários nós tem nós que estão em sub-redes diferentes. Para implantações de vários nós, o **RestName** parâmetro especifica o FQDN para o cluster de controlador de rede.|
|ClientSecurityGroup|O **ClientSecurityGroup** parâmetro especifica o nome do grupo de segurança do Active Directory cujos membros são os clientes do controlador de rede. Esse parâmetro é necessário apenas se você usar a autenticação Kerberos para **ClientAuthentication**. O grupo de segurança deve conter as contas do qual as APIs REST são acessadas, e você deve criar o grupo de segurança e adicionar membros antes de executar esse comando.|
|Credential|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **credencial** parâmetro especifica uma conta de usuário que tenha permissão para executar este comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **CertificateThumbprint** parâmetro especifica digital certificado de chave pública (X509) uma conta de usuário que tenha permissão para executar este comando no computador de destino.|
|UseSSL|Esse parâmetro é necessário apenas se você estiver executando este comando de um computador remoto. O **UseSSL** parâmetro especifica o protocolo Secure Sockets Layer (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, SSL não é usado.|

Depois de concluir a configuração do aplicativo controlador de rede, a implantação do controlador de rede foi concluída.

## <a name="network-controller-deployment-validation"></a>Validação de implantação do controlador de rede

Para validar sua implantação do controlador de rede, você pode adicionar uma credencial para o controlador de rede e, em seguida, recuperar a credencial.

Se você estiver usando o Kerberos como o mecanismo de ClientAuthentication, associação a **ClientSecurityGroup** que você criou é o mínimo necessário para executar este procedimento.

**Procedimento:**

1.  Em um computador cliente, se você estiver usando o Kerberos como o mecanismo de ClientAuthentication, faça logon com uma conta de usuário que seja membro do seu **ClientSecurityGroup**.

2. Abra o Windows PowerShell, digite os seguintes comandos para adicionar uma credencial para o controlador de rede e, em seguida, pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para sua implantação.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar a credencial que você adicionou ao controlador de rede, digite o seguinte comando e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para sua implantação.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Examine a saída do comando, que deve ser semelhante ao exemplo a seguir.

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > Quando você executa o **Get-NetworkControllerCredential** de comando, você pode atribuir a saída do comando a uma variável usando o operador ponto para listar as propriedades das credenciais. Por exemplo, $cred. Propriedades.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Comandos adicionais do Windows PowerShell para o controlador de rede

Depois de implantar o controlador de rede, você pode usar comandos do Windows PowerShell para gerenciar e modificar sua implantação. A seguir estão algumas das alterações que você pode fazer a sua implantação.

- Modificar as configurações do aplicativo, cluster e nó do controlador de rede

- Remover o cluster de controlador de rede e o aplicativo

- Gerencie nós de cluster de controlador de rede, inclusive adicionando, removendo, habilitando e desabilitando nós.

A tabela a seguir fornece que a sintaxe para o Windows PowerShell comandos que você pode usar para realizar essas tarefas.

|Tarefa|Comando|Sintaxe|
|--------|-------|----------|
|Modificar as configurações de cluster de controlador de rede|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar as configurações de aplicativo do controlador de rede|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar as configurações de nó do controlador de rede|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar configurações de diagnóstico do controlador de rede|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o aplicativo do controlador de rede|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o cluster de controlador de rede|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Adicionar um nó no cluster de controlador de rede|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Desativar um nó de cluster de controlador de rede|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Ativar um nó de cluster de controlador de rede|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Remover um nó de controlador de rede de um cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Comandos do Windows PowerShell para o controlador de rede estão na biblioteca do TechNet em [Cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Exemplo de script de configuração de controlador de rede

O seguinte script de configuração de exemplo mostra como criar um cluster de controlador de rede com vários nó e instalar o aplicativo do controlador de rede. Além disso, a variável $cert seleciona um certificado do repositório de certificados do computador local que corresponde a cadeia de caracteres de nome de assunto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Etapas de pós-implantação para implantações não Kerberos

Se você não estiver usando o Kerberos com a implantação do controlador de rede, você deve implantar certificados.

Para obter mais informações, consulte [etapas de pós-implantação para controlador de rede](../technologies/network-controller/post-deploy-steps-nc.md).


