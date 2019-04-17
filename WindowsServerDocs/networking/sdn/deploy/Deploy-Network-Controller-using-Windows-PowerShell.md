---
title: Implantar o controlador de rede usando o Windows PowerShell
description: Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em um ou mais computadores ou VMs (máquinas virtuais) que executam o Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: cfd06662f317381fb77bf31db5ed60c2489ff871
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Implantar o controlador de rede usando o Windows PowerShell

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece instruções sobre como usar o Windows PowerShell para implantar o controlador de rede em um ou mais VMs (máquinas virtuais) que executam o Windows Server 2016.

>[!IMPORTANT]
>Não implante a função de servidor de controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor de controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalada em um host do Hyper-V. Depois que você instalou o controlador de rede em VMs em três diferentes hosts de Hyper\-V, você deve habilitar os hosts Hyper\-V para Software de rede definidos \(SDN\) adicionando os hosts de controlador de rede usando o comando do Windows PowerShell **nova NetworkControllerServer**. Fazendo isso, você está ativando o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [nova NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Este tópico contém as seções a seguir.

- [Instalar a função de servidor de controlador de rede](#bkmk_role)

- [Configurar o controlador de rede de cluster](#bkmk_configure)

- [Configurar o aplicativo de controlador de rede](#bkmk_app)

- [Validação de implantação do controlador de rede](#bkmk_validation)

- [Comandos adicionais do Windows PowerShell para o controlador de rede](#bkmk_ps)

- [Script de exemplo a configuração controlador de rede](#bkmk_script)

- [Pós-Post-Deployment Etapas para implantações de Kerberos não](#bkmk_nonkerb)

## <a name="bkmk_role"></a>Instalar a função de servidor de controlador de rede

Você pode usar este procedimento para instalar a função de servidor de controlador de rede em uma máquina virtual \(VM\).

>[!IMPORTANT]
>Não implante a função de servidor de controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor de controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalada em um host do Hyper-V. Depois que você instalou o controlador de rede em VMs em três diferentes hosts de Hyper\-V, você deve habilitar os hosts Hyper\-V para Software de rede definidos \(SDN\) adicionando os hosts controlador de rede. Fazendo isso, você está ativando o balanceador de carga de Software SDN à função.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  

>[!NOTE]
>Se você quiser usar o Gerenciador do servidor em vez do Windows PowerShell para instalar o controlador de rede, consulte [instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](https://technet.microsoft.com/library/mt403348.aspx)

Para instalar o controlador de rede usando o Windows PowerShell, digite os seguintes comandos em um prompt do Windows PowerShell e pressione ENTER.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Instalação do controlador de rede requer que você reinicie o computador. Para fazer isso, digite o seguinte comando e pressione ENTER.

`Restart-Computer`

## <a name="bkmk_configure"></a>Configurar o controlador de rede de cluster

O cluster de controlador de rede fornece alta disponibilidade e escalabilidade para o aplicativo de controlador de rede, que você pode configurar depois de criar o cluster e que está hospedado em cima do cluster.

>[!NOTE]
>Você pode executar os procedimentos nas seções a seguir diretamente na VM em que você instalou o controlador de rede, ou você pode usar Remote Server Administration ferramentas para Windows Server 2016 para executar os procedimentos de um computador remoto que esteja executando o Windows Server 2016 ou Windows 10. Além disso, a associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento. Se o computador ou a VM no qual você instalou o controlador de rede é associado a um domínio, sua conta de usuário deve ser um membro do **usuários do domínio**.

Você pode criar um cluster de controlador de rede criando um objeto de nó e, depois configurando o cluster.

### <a name="create-a-node-object"></a>Crie um objeto de nó

Você precisa criar um objeto de nó para cada VM é um membro do cluster controlador de rede.

Para criar um objeto de nó, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para a implantação.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

A tabela a seguir apresenta descrições para cada parâmetro do **nova NetworkControllerNodeObject** comando.

|Parâmetro|Descrição|
|-------------|---------------|
|Nome|O **nome** parâmetro especifica o nome amigável do servidor que você deseja adicionar ao cluster|
|Servidor|O **servidor** parâmetro especifica o nome do host, totalmente o nome de domínio qualificado (FQDN) ou endereço IP do servidor que você deseja adicionar ao cluster. Para computadores de domínio, o FQDN é necessária.|
|FaultDomain|O **FaultDomain** parâmetro especifica o domínio de falha do servidor que você está adicionando ao cluster. Esse parâmetro define os servidores que podem ocorrer falha ao mesmo tempo que o servidor que você está adicionando ao cluster. Esta falha pode ser devido a compartilhado dependências físicas, como o consumo de energia e fontes de redes. Domínios de falha normalmente representam hierarquias relacionadas a essas dependências compartilhadas, com servidores mais prováveis que juntos uma falha de um ponto mais alto na árvore de domínio falha. Durante o tempo de execução, o controlador de rede considera os domínios de falha no cluster e tenta espalhadas os serviços de controlador de rede para que fiquem em domínios de falha separado. Esse processo ajuda a garantir que, em caso de falha de qualquer domínio de uma falha, que a disponibilidade de serviço e seu estado não seja comprometida. Domínios de falha são especificados em um formato hierárquico. Por exemplo: "Fd: / Rack1/DC1/Host1", onde DC1 é o nome de datacenter, Rack1 é o nome de rack e Host1 é o nome do host em que o nó é colocado.|
|RestInterface|O **RestInterface** parâmetro especifica o nome da interface no nó em que a comunicação de REST Representational State Transfer () é encerrada. Essa interface de rede controlador recebe solicitações de API em sentido norte da camada de gerenciamento da rede.|
|NodeCertificate|O **NodeCertificate** parâmetro especifica o certificado que usa o controlador de rede para autenticação do computador. O certificado é necessário se você usar a autenticação baseada em certificado para comunicação dentro do cluster; o certificado também é usado para criptografia de tráfego entre os serviços de controlador de rede. Nome do requerente do certificado deve ser igual ao nome DNS do nó.|

### <a name="configure-the-cluster"></a>Configurar o cluster

Para configurar o cluster, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para a implantação.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

A tabela a seguir apresenta descrições para cada parâmetro do **instalar NetworkControllerCluster** comando.
  
|Parâmetro|Descrição|
|-------------|---------------|
|ClusterAuthentication|O **ClusterAuthentication** parâmetro especifica o tipo de autenticação que é usado para proteger a comunicação entre nós e também é usado para criptografia de tráfego entre os serviços de controlador de rede. Os valores suportados são **Kerberos**, **X509** e **None**. Autenticação Kerberos usa contas de domínio e só pode ser usada se os nós de controlador de rede são ingressado no domínio. Se você especificar a autenticação baseada em X509, você deve fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve configurar manualmente o certificado antes de executar esse comando.|
|ManagementSecurityGroup|O **ManagementSecurityGroup** parâmetro especifica o nome do grupo de segurança que contém os usuários que têm permissão para executar os cmdlets de gerenciamento de um computador remoto. Isso só é aplicável se ClusterAuthentication for Kerberos. Você deve especificar um grupo de segurança de domínio e não a um grupo de segurança no computador local.|
|Nó|O **nó** parâmetro especifica a lista de nós de controlador de rede que você criou usando o **nova NetworkControllerNodeObject** comando.|
|DiagnosticLogLocation|O **DiagnosticLogLocation** parâmetro especifica o local de compartilhamento onde os logs de diagnósticos são carregados periodicamente. Se você não especificar um valor para esse parâmetro, os logs são armazenados localmente em cada nó. Logs são armazenados localmente na pasta % systemdrive%\Windows\tracing\SDNDiagnostics. Logs de cluster são armazenados localmente no %systemdrive%\ProgramData\Microsoft\Service a pasta Fabric\log\Traces.|
|LogLocationCredential|O **LogLocationCredential** parâmetro especifica as credenciais que são necessárias para acessar o local de compartilhamento onde os logs são armazenados.|
|CredentialEncryptionCertificate|O **CredentialEncryptionCertificate** parâmetro especifica o certificado que usa o controlador de rede para criptografar as credenciais que são usadas para acessar os binários do controlador de rede e o **LogLocationCredential**, se especificado. O certificado deve ser provisionado em todos os nós de controlador de rede antes de executar esse comando, e o mesmo certificado deve ser registrado em todos os nós de cluster. Usar esse parâmetro para proteger os logs e binários de controlador de rede é recomendado em ambientes de produção. Sem esse parâmetro, as credenciais são armazenadas em texto não criptografado e podem ser usados por qualquer usuário não autorizado incorretamente.|
|Credenciais|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **credenciais** parâmetro especifica uma conta de usuário que tenha permissão para executar esse comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **CertificateThumbprint** parâmetro especifica o digital certificado de chave pública (X509) de uma conta de usuário que tenha permissão para executar esse comando no computador de destino.|
|UseSSL|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **UseSSL** parâmetro especifica o protocolo Secure Sockets Layer (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, o SSL não é usado.|
|ComputerName|O **ComputerName** parâmetro especifica no nó do controlador de rede em que esse comando é executado. Se você não especificar um valor para esse parâmetro, o computador local é usado por padrão.|
|LogSizeLimitInMBs|Esse parâmetro especifica o tamanho máximo do log, em MB, que armazena o controlador de rede. Os logs são armazenados em forma circular. Se DiagnosticLogLocation for fornecida, o valor padrão desse parâmetro é 40 GB. Se não for fornecida DiagnosticLogLocation, os logs são armazenados em todos os nós de controlador de rede e o valor padrão desse parâmetro é 15 GB.|
|LogTimeLimitInDays|Esse parâmetro especifica o limite de duração, em dias, para que os logs são armazenados. Os logs são armazenados em forma circular. O valor padrão desse parâmetro é de 3 dias.|

## <a name="bkmk_app"></a>Configurar o aplicativo de controlador de rede
Para configurar o aplicativo controlador de rede, digite o seguinte comando no prompt de comando do Windows PowerShell e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para a implantação.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

A tabela a seguir apresenta descrições para cada parâmetro do **instalar NetworkController** comando.

|Parâmetro|Descrição|
|-------------|---------------|
|ClientAuthentication|O **ClientAuthentication** parâmetro especifica o tipo de autenticação que é usado para proteger a comunicação entre REST e controlador de rede. Os valores suportados são **Kerberos**, **X509** e **None**. Autenticação Kerberos usa contas de domínio e só pode ser usada se os nós de controlador de rede são ingressado no domínio. Se você especificar a autenticação baseada em X509, você deve fornecer um certificado no objeto NetworkControllerNode. Além disso, você deve configurar manualmente o certificado antes de executar esse comando.|
|Nó|O **nó** parâmetro especifica a lista de nós de controlador de rede que você criou usando o **nova NetworkControllerNodeObject** comando.|
|ClientCertificateThumbprint|Esse parâmetro é necessário somente quando você estiver usando autenticação baseada em certificado para clientes de controlador de rede. O **ClientCertificateThumbprint** parâmetro especifica a impressão digital do certificado que é registrado para clientes na camada em sentido norte.|
|ServerCertificate|O **ServerCertificate** parâmetro especifica o certificado que usa o controlador de rede para provar sua identidade para os clientes. O certificado do servidor deve incluir a finalidade de autenticação de servidor em extensões de uso avançado de chave e deve ser emitido para o controlador de rede por uma autoridade de certificação confiável pelos clientes.|
|RESTIPAddress|Você não precisa especificar um valor para **RESTIPAddress** com uma implantação único nó do controlador de rede. Para implantações de vários nós, a **RESTIPAddress** parâmetro especifica o endereço IP do ponto de extremidade REST na notação CIDR. Por exemplo, 192.168.1.10/24. O valor de nome do requerente do **ServerCertificate** deve ser resolvido para o valor da **RESTIPAddress** parâmetro. Este parâmetro deve ser especificado para todas as implantações de controlador de rede de vários nós quando todos os nós estão na mesma sub-rede. Se nós em várias sub-redes, você deve usar o **RestName** parâmetro em vez de usar **RESTIPAddress**.|
|RestName|Você não precisa especificar um valor para **RestName** com uma implantação único nó do controlador de rede. O único momento em que você deve especificar um valor para **RestName** é quando implantações de vários nós tem nós que estão em sub-redes diferentes. Para implantações de vários nós, a **RestName** parâmetro especifica o FQDN para o cluster de controlador de rede.|
|ClientSecurityGroup|O **ClientSecurityGroup** parâmetro especifica o nome do grupo de segurança do Active Directory cujos membros são clientes de controlador de rede. Esse parâmetro será obrigatório somente se você usar a autenticação Kerberos para **ClientAuthentication**. O grupo de segurança deve conter as contas da qual as APIs REST são acessadas, e você deve criar o grupo de segurança e adicionar membros antes de executar esse comando.|
|Credenciais|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **credenciais** parâmetro especifica uma conta de usuário que tenha permissão para executar esse comando no computador de destino.|
|CertificateThumbprint|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **CertificateThumbprint** parâmetro especifica o digital certificado de chave pública (X509) de uma conta de usuário que tenha permissão para executar esse comando no computador de destino.|
|UseSSL|Esse parâmetro é necessário somente se você estiver executando esse comando em um computador remoto. O **UseSSL** parâmetro especifica o protocolo Secure Sockets Layer (SSL) que é usado para estabelecer uma conexão com o computador remoto. Por padrão, o SSL não é usado.|

Depois de concluir a configuração do aplicativo controlador de rede, a implantação do controlador de rede é concluída.

## <a name="bkmk_validation"></a>Validação de implantação do controlador de rede

Para validar a implantação do controlador de rede, você pode adicionar uma credencial no controlador de rede e, em seguida, recuperar a credencial.

Se você estiver usando Kerberos como mecanismo de ClientAuthentication, a associação a **ClientSecurityGroup** que você criou é o requisito mínimo para executar este procedimento.

#### <a name="to-validate-deployment-of-network-controller"></a>Para validar a implantação do controlador de rede

1.  Em um computador cliente, se você estiver usando Kerberos como mecanismo de ClientAuthentication, faça logon com uma conta de usuário é um membro do seu **ClientSecurityGroup**.

2. Abra o Windows PowerShell, digite os seguintes comandos para adicionar uma credencial ao controlador de rede e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para a implantação.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Para recuperar as credenciais que você adicionou ao controlador de rede, digite o seguinte comando e pressione ENTER. Certifique-se de que você adicione valores para cada parâmetro que são apropriados para a implantação.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Examine a saída, que deve ser parecido com o seguinte exemplo de saída do comando.

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
    > Quando você executa o **Get-NetworkControllerCredential** de comando, você pode atribuir a saída do comando para uma variável usando o operador de ponto para listar as propriedades das credenciais. Por exemplo, $cred. Propriedades.

## <a name="bkmk_ps"></a>Comandos adicionais do Windows PowerShell para o controlador de rede

Depois de implantar o controlador de rede, você pode usar comandos do Windows PowerShell para gerenciar e modificar sua implantação. A seguir está algumas das alterações que você pode fazer para a implantação.

- Modificar o nó de controlador de rede, cluster e configurações do aplicativo

- Remover o cluster de controlador de rede e o aplicativo

- Gerencie nós de cluster de controlador de rede, incluindo adicionando, removendo, habilitando e desabilitando a nós.

A tabela a seguir fornece que a sintaxe para Windows PowerShell comandos que podem ser usados para realizar essas tarefas.

|Tarefa|Comando|Sintaxe|
|--------|-------|----------|
|Modificar as configurações de cluster de controlador de rede|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar as configurações de aplicativo de controlador de rede|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar as configurações de nó do controlador de rede|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificar configurações de diagnóstico de controlador de rede|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o aplicativo controlador de rede|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Remover o cluster de controlador de rede|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Adicione um nó de cluster controlador de rede|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Desabilitar um nó de cluster de controlador de rede|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Habilitar um nó de cluster de controlador de rede|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Remover um nó de controlador de rede de um cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Comandos do Windows PowerShell para o controlador de rede estão na biblioteca do TechNet em [Cmdlets de controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="bkmk_script"></a>Script de exemplo a configuração controlador de rede

O script de configuração de exemplo a seguir mostra como criar um cluster de controlador de rede de vários nó e instale o aplicativo de controlador de rede. Além disso, a variável $cert seleciona um certificado do repositório de certificados do computador local que corresponda a cadeia de caracteres de nome de assunto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="bkmk_nonkerb"></a>Pós-implantação etapas para não - Kerberos implantações

Se você não estiver usando Kerberos com a implantação do controlador de rede, você deve implantar certificados.

Para obter mais informações, consulte [pós-implantação etapas para o controlador de rede](../technologies/network-controller/post-deploy-steps-nc.md).


