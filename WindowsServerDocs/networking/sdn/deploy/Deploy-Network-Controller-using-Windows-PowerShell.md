---
title: Implantar controlador de rede usando o Windows PowerShell
description: Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em um ou mais computadores ou máquinas virtuais (VMs) que executam o Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 294466ef70a9ffc230953b48bb292938be519eac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406117"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implantar controlador de rede usando o Windows PowerShell

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em uma ou mais máquinas virtuais (VMs) que executam o Windows Server 2016.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual do Hyper-V \(VM @ no__t-1 que está instalado em um host do Hyper-V. Depois de ter instalado o controlador de rede em VMs em três hosts Hyper @ no__t-0V diferentes, você deve habilitar os hosts do Hyper @ no__t-1V para a rede definida pelo software \(SDN @ no__t-3 adicionando os hosts ao controlador de rede usando o Windows PowerShell comando **New-NetworkControllerServer**. Ao fazer isso, você está permitindo que o software SDN Load Balancer funcione. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Este tópico contém as seguintes seções.

- [Instalar a função de servidor do controlador de rede](#install-the-network-controller-server-role)

- [Configurar o cluster do controlador de rede](#configure-the-network-controller-cluster)

- [Configurar o aplicativo do controlador de rede](#configure-the-network-controller-application)

- [Validação de implantação do controlador de rede](#network-controller-deployment-validation)

- [Comandos adicionais do Windows PowerShell para o controlador de rede](#additional-windows-powershell-commands-for-network-controller)

- [Exemplo de script de configuração do controlador de rede](#sample-network-controller-configuration-script)

- [Etapas pós-implantação para implantações não Kerberos](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>Instalar a função de servidor do controlador de rede

Você pode usar este procedimento para instalar a função de servidor do controlador de rede em uma máquina virtual \(VM @ no__t-1.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual do Hyper-V \(VM @ no__t-1 que está instalado em um host do Hyper-V. Depois de ter instalado o controlador de rede em VMs em três hosts Hyper @ no__t-0V diferentes, você deve habilitar os hosts Hyper @ no__t-1V para a rede definida pelo software \(SDN @ no__t-3 adicionando os hosts ao controlador de rede. Ao fazer isso, você está permitindo que o software SDN Load Balancer funcione.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  

>[!NOTE]
>Se você quiser usar Gerenciador do Servidor em vez do Windows PowerShell para instalar o controlador de rede, consulte [instalar a função de servidor do controlador de rede usando Gerenciador do servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar o controlador de rede usando o Windows PowerShell, digite os seguintes comandos em um prompt do Windows PowerShell e pressione ENTER.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

A instalação do controlador de rede requer que você reinicie o computador. Para fazer isso, digite o comando a seguir e pressione ENTER.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurar o cluster do controlador de rede

O cluster do controlador de rede fornece alta disponibilidade e escalabilidade para o aplicativo do controlador de rede, que você pode configurar depois de criar o cluster e que é hospedado na parte superior do cluster.

>[!NOTE]
>Você pode executar os procedimentos nas seções a seguir diretamente na VM em que você instalou o controlador de rede ou pode usar o Ferramentas de Administração de Servidor Remoto para o Windows Server 2016 para executar os procedimentos de um computador remoto que esteja executando o o Windows Server 2016 ou o Windows 10. Além disso, a associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para executar esse procedimento. Se o computador ou a VM na qual você instalou o controlador de rede estiver ingressado em um domínio, sua conta de usuário deverá ser membro de **usuários de domínio**.

Você pode criar um cluster de controlador de rede criando um objeto de nó e, em seguida, configurando o cluster.

### <a name="create-a-node-object"></a>Criar um objeto de nó

Você precisa criar um objeto de nó para cada VM que seja membro do cluster do controlador de rede.

Para criar um objeto de nó, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de adicionar valores para cada parâmetro que são apropriados para sua implantação.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

A tabela a seguir fornece descrições para cada parâmetro do comando **New-NetworkControllerNodeObject** .

|Parâmetro|Descrição|
|-------------|---------------|
|Nome|O parâmetro **Name** especifica o nome amigável do servidor que você deseja adicionar ao cluster|
|Servidor|O parâmetro **Server** especifica o nome do host, o FQDN (nome de domínio totalmente qualificado) ou o endereço IP do servidor que você deseja adicionar ao cluster. Para computadores ingressados no domínio, o FQDN é necessário.|
|FaultDomain|O parâmetro **FaultDomain** especifica o domínio de falha do servidor que você está adicionando ao cluster. Esse parâmetro define os servidores que podem apresentar falha ao mesmo tempo que o servidor que você está adicionando ao cluster. Essa falha pode ser causada por dependências físicas compartilhadas, como fontes de energia e rede. Os domínios de falha normalmente representam hierarquias relacionadas a essas dependências compartilhadas, com mais servidores que provavelmente falham juntos em um ponto mais alto na árvore de domínio de falha. Durante o tempo de execução, o controlador de rede considera os domínios de falha no cluster e tenta distribuir os serviços do controlador de rede para que eles estejam em domínios de falha separados. Esse processo ajuda a garantir, em caso de falha de qualquer domínio de falha, que a disponibilidade desse serviço e seu estado não sejam comprometidos. Os domínios de falha são especificados em um formato hierárquico. Por exemplo: "FD:/DC1/Rack1/Host1", em que DC1 é o nome do datacenter, Rack1 é o nome do rack e Host1 é o nome do host onde o nó é colocado.|
|RestInterface|O parâmetro **RestInterface** especifica o nome da interface no nó em que a comunicação de transferência de estado de reapresentação (REST) é encerrada. Essa interface de controlador de rede recebe solicitações de API Northbound da camada de gerenciamento da rede.|
|NodeCertificate|O parâmetro **NodeCertificate** especifica o certificado que o controlador de rede usa para autenticação do computador. O certificado será necessário se você usar a autenticação baseada em certificado para comunicação dentro do cluster; o certificado também é usado para criptografia de tráfego entre os serviços do controlador de rede. O nome da entidade do certificado deve ser o mesmo que o nome DNS do nó.|

### <a name="configure-the-cluster"></a>Configurar o cluster

Para configurar o cluster, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de adicionar valores para cada parâmetro que são apropriados para sua implantação.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

A tabela a seguir fornece descrições para cada parâmetro do comando **install-NetworkControllerCluster** .
  
|Parâmetro|Descrição|
|-------------|---------------|
|ClusterAuthentication|O parâmetro **ClusterAuthentication** especifica o tipo de autenticação que é usado para proteger a comunicação entre os nós e também é usado para criptografia de tráfego entre os serviços do controlador de rede. Os valores com suporte são **Kerberos**, **X509** e **None**. A autenticação Kerberos usa contas de domínio e só pode ser usada se os nós do controlador de rede estiverem ingressados no domínio. Se você especificar a autenticação baseada em X509, deverá fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve provisionar manualmente o certificado antes de executar esse comando.|
|ManagementSecurityGroup|O parâmetro **ManagementSecurityGroup** especifica o nome do grupo de segurança que contém os usuários que têm permissão para executar os cmdlets de gerenciamento de um computador remoto. Isso só será aplicável se ClusterAuthentication for Kerberos. Você deve especificar um grupo de segurança de domínio e não um grupo de segurança no computador local.|
|Nó|O parâmetro **node** especifica a lista de nós de controlador de rede que você criou usando o comando **New-NetworkControllerNodeObject** .|
|DiagnosticLogLocation|O parâmetro **DiagnosticLogLocation** especifica o local de compartilhamento onde os logs de diagnóstico são carregados periodicamente. Se você não especificar um valor para esse parâmetro, os logs serão armazenados localmente em cada nó. Os logs são armazenados localmente na pasta%systemdrive%\Windows\tracing\SDNDiagnostics. Os logs de cluster são armazenados localmente na pasta%systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|O parâmetro **LogLocationCredential** especifica as credenciais que são necessárias para acessar o local de compartilhamento onde os logs são armazenados.|
|CredentialEncryptionCertificate|O parâmetro **CredentialEncryptionCertificate** especifica o certificado que o controlador de rede usa para criptografar as credenciais que são usadas para acessar os binários do controlador de rede e o **LogLocationCredential**, se especificado. O certificado deve ser provisionado em todos os nós do controlador de rede antes de executar esse comando, e o mesmo certificado deve ser registrado em todos os nós do cluster. O uso desse parâmetro para proteger binários e logs do controlador de rede é recomendado em ambientes de produção. Sem esse parâmetro, as credenciais são armazenadas em texto não criptografado e podem ser usadas inalteradas por qualquer usuário não autorizado.|
|Credential|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **Credential** especifica uma conta de usuário que tem permissão para executar esse comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **CertificateThumbprint** especifica o certificado de chave pública digital (X509) de uma conta de usuário que tem permissão para executar esse comando no computador de destino.|
|UseSSL|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **UseSSL** especifica o protocolo de protocolo SSL (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, SSL não é usado.|
|ComputerName|O parâmetro **ComputerName** especifica o nó do controlador de rede no qual esse comando é executado. Se você não especificar um valor para esse parâmetro, o computador local será usado por padrão.|
|LogSizeLimitInMBs|Esse parâmetro especifica o tamanho máximo do log, em MB, que o controlador de rede pode armazenar. Os logs são armazenados de maneira circular. Se DiagnosticLogLocation for fornecido, o valor padrão desse parâmetro será 40 GB. Se DiagnosticLogLocation não for fornecido, os logs serão armazenados nos nós do controlador de rede e o valor padrão desse parâmetro será 15 GB.|
|LogTimeLimitInDays|Esse parâmetro especifica o limite de duração, em dias, para o qual os logs são armazenados. Os logs são armazenados de maneira circular. O valor padrão desse parâmetro é 3 dias.|

## <a name="configure-the-network-controller-application"></a>Configurar o aplicativo do controlador de rede
Para configurar o aplicativo do controlador de rede, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de adicionar valores para cada parâmetro que são apropriados para sua implantação.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

A tabela a seguir fornece descrições para cada parâmetro do comando **install-NetworkController** .

|Parâmetro|Descrição|
|-------------|---------------|
|Clientauthentication válido|O parâmetro **clientauthentication válido** especifica o tipo de autenticação que é usado para proteger a comunicação entre o REST e o controlador de rede. Os valores com suporte são **Kerberos**, **X509** e **None**. A autenticação Kerberos usa contas de domínio e só pode ser usada se os nós do controlador de rede estiverem ingressados no domínio. Se você especificar a autenticação baseada em X509, deverá fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve provisionar manualmente o certificado antes de executar esse comando.|
|Nó|O parâmetro **node** especifica a lista de nós de controlador de rede que você criou usando o comando **New-NetworkControllerNodeObject** .|
|ClientCertificateThumbprint|Esse parâmetro é necessário somente quando você estiver usando a autenticação baseada em certificado para clientes do controlador de rede. O parâmetro **ClientCertificateThumbprint** especifica a impressão digital do certificado registrado nos clientes na camada northbound.|
|ServerCertificate|O parâmetro **serverCertificate** especifica o certificado que o controlador de rede usa para provar sua identidade para os clientes. O certificado do servidor deve incluir a finalidade da autenticação do servidor em extensões de uso avançado de chave e deve ser emitido para o controlador de rede por uma autoridade de certificação confiável para clientes.|
|RESTIPAddress|Você não precisa especificar um valor para **RESTIPAddress** com uma implantação de nó único do controlador de rede. Para implantações de vários nós, o parâmetro **RESTIPAddress** especifica o endereço IP do ponto de extremidade REST na notação CIDR. Por exemplo, 192.168.1.10/24. O valor do nome da entidade de **serverCertificate** deve ser resolvido para o valor do parâmetro **RESTIPAddress** . Esse parâmetro deve ser especificado para todas as implantações do controlador de rede de vários nós quando todos os nós estiverem na mesma sub-rede. Se os nós estiverem em sub-redes diferentes, você deverá usar o parâmetro **REST** , em vez de usar **RESTIPAddress**.|
|RestName|Você não precisa especificar um valor para **restname** com uma implantação de nó único do controlador de rede. A única vez que você deve especificar um valor para **restname** é quando implantações de vários nós têm nós que estão em sub-redes diferentes. Para implantações de vários nós, o parâmetro **restname** especifica o FQDN para o cluster do controlador de rede.|
|ClientSecurityGroup|O parâmetro **ClientSecurityGroup** especifica o nome do grupo de segurança Active Directory cujos membros são clientes do controlador de rede. Esse parâmetro será necessário apenas se você usar a autenticação Kerberos para **clientauthentication válido**. O grupo de segurança deve conter as contas das quais as APIs REST são acessadas e você deve criar o grupo de segurança e adicionar membros antes de executar esse comando.|
|Credential|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **Credential** especifica uma conta de usuário que tem permissão para executar esse comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **CertificateThumbprint** especifica o certificado de chave pública digital (X509) de uma conta de usuário que tem permissão para executar esse comando no computador de destino.|
|UseSSL|Esse parâmetro será necessário somente se você estiver executando esse comando de um computador remoto. O parâmetro **UseSSL** especifica o protocolo de protocolo SSL (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, SSL não é usado.|

Depois de concluir a configuração do aplicativo do controlador de rede, a implantação do controlador de rede será concluída.

## <a name="network-controller-deployment-validation"></a>Validação de implantação do controlador de rede

Para validar a implantação do controlador de rede, você pode adicionar uma credencial ao controlador de rede e, em seguida, recuperar a credencial.

Se você estiver usando o Kerberos como o mecanismo clientauthentication válido, a associação no **ClientSecurityGroup** que você criou é o mínimo necessário para executar esse procedimento.

**Procedure**

1.  Em um computador cliente, se você estiver usando o Kerberos como o mecanismo clientauthentication válido, faça logon com uma conta de usuário que seja membro de seu **ClientSecurityGroup**.

2. Abra o Windows PowerShell, digite os seguintes comandos para adicionar uma credencial ao controlador de rede e pressione ENTER. Certifique-se de adicionar valores para cada parâmetro que são apropriados para sua implantação.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar a credencial que você adicionou ao controlador de rede, digite o comando a seguir e pressione ENTER. Certifique-se de adicionar valores para cada parâmetro que são apropriados para sua implantação.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Examine a saída do comando, que deve ser semelhante à saída de exemplo a seguir.

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
    > Ao executar o comando **Get-NetworkControllerCredential** , você pode atribuir a saída do comando a uma variável usando o operador ponto para listar as propriedades das credenciais. Por exemplo, $cred. Properties.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Comandos adicionais do Windows PowerShell para o controlador de rede

Depois de implantar o controlador de rede, você pode usar comandos do Windows PowerShell para gerenciar e modificar sua implantação. A seguir estão algumas das alterações que você pode fazer em sua implantação.

- Modificar as configurações do nó do controlador de rede, do cluster e do aplicativo

- Remover o cluster do controlador de rede e o aplicativo

- Gerencie nós de cluster do controlador de rede, incluindo adição, remoção, habilitação e desabilitação de nós.

A tabela a seguir fornece a sintaxe para comandos do Windows PowerShell que você pode usar para realizar essas tarefas.

|Tarefa|Comando|Sintaxe|
|--------|-------|----------|
|Modificar configurações de cluster do controlador de rede|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar configurações do aplicativo do controlador de rede|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar configurações de nó do controlador de rede|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar configurações de diagnóstico do controlador de rede|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o aplicativo do controlador de rede|Desinstalar-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o cluster do controlador de rede|Desinstalar-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Adicionar um nó ao cluster do controlador de rede|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Desabilitar um nó de cluster do controlador de rede|Desabilitar-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Habilitar um nó de cluster do controlador de rede|Habilitar-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Remover um nó de controlador de rede de um cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Os comandos do Windows PowerShell para o controlador de rede estão na biblioteca do TechNet em [cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Exemplo de script de configuração do controlador de rede

O script de configuração de exemplo a seguir mostra como criar um cluster de controlador de rede com vários nós e instalar o aplicativo do controlador de rede. Além disso, a variável $cert seleciona um certificado do repositório de certificados do computador local que corresponde à cadeia de caracteres de nome de entidade "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Etapas pós-implantação para implantações não Kerberos

Se você não estiver usando o Kerberos com a implantação do controlador de rede, deverá implantar certificados.

Para obter mais informações, consulte [etapas de pós-implantação para o controlador de rede](../technologies/network-controller/post-deploy-steps-nc.md).


