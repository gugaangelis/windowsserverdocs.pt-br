---
title: Implantar espaços de armazenamento diretos
manager: eldenc
ms.author: stevenek
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 09/09/2020
description: Instruções passo a passo para implantar o armazenamento definido pelo software com o Espaços de Armazenamento Diretos no Windows Server como infraestrutura de hiperconvergente ou uma infraestrutura convergida (também conhecida como desagregada).
ms.localizationpriority: medium
ms.openlocfilehash: c7ff6b1cf017405d90ae7e27d1d5853286a78b89
ms.sourcegitcommit: c56e74743e5ad24b28ae81668668113d598047c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987306"
---
# <a name="deploy-storage-spaces-direct"></a>Implantar espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece instruções passo a passo para implantar [espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server. Para implantar Espaços de Armazenamento Diretos como parte do Azure Stack HCI, consulte [qual é o processo de implantação para Azure Stack HCI?](/azure-stack/hci/deploy/deployment-overview)

> [!Tip]
> Deseja adquirir uma infraestrutura com hiperconvergente? A Microsoft recomenda a compra de um hardware/software validado Azure Stack solução de HCI de nossos parceiros. Essas soluções são projetadas, montadas e validadas em relação à nossa arquitetura de referência para garantir a compatibilidade e a confiabilidade, para que você comece a trabalhar rapidamente. Para examinar um catálogo de soluções de hardware/software que funcionam com o HCI Azure Stack, consulte o [Catálogo de hci Azure Stack](https://azure.microsoft.com/products/azure-stack/hci/catalog/).

> [!Tip]
> Você pode usar máquinas virtuais do Hyper-V, incluindo no Microsoft Azure, para [avaliar espaços de armazenamento diretos sem hardware](storage-spaces-direct-in-vm.md). Você também pode querer examinar os convenientes [scripts de implantação do Windows Server Rapid Lab](https://aka.ms/wslab), que usaremos para fins de treinamento.

## <a name="before-you-start"></a>Antes de começar

Examine os [requisitos de hardware espaços de armazenamento diretos](Storage-Spaces-Direct-Hardware-Requirements.md) e leia este documento para se familiarizar com a abordagem geral e observações importantes associadas a algumas etapas.

Reúna as seguintes informações:

- **Opção de implantação.** O Espaços de Armazenamento Diretos dá suporte a [duas opções de implantação: hiperconvergente e convergida](storage-spaces-direct-overview.md#deployment-options), também conhecida como desagregada. Familiarize-se com as vantagens de cada um para decidir qual é a certa para você. As etapas 1-3 abaixo se aplicam a ambas as opções de implantação. A etapa 4 só é necessária para a implantação convergida.

- **Nomes de servidor.** Familiarize-se com as políticas de nomenclatura de sua organização para computadores, arquivos, caminhos e outros recursos. Você precisará provisionar vários servidores, cada um com nomes exclusivos.

- **Nome de domínio.** Familiarize-se com as políticas de sua organização para a nomenclatura de domínio e ingresso no domínio.  Você ingressará os servidores em seu domínio e precisará especificar o nome de domínio.

- **Rede RDMA.** Há dois tipos de protocolos RDMA: iWarp e RoCE. Observe qual é o uso dos adaptadores de rede e, se RoCE, observe também a versão (v1 ou v2). Para RoCE, observe também o modelo de seu comutador Top-of-rack.

- **ID DA VLAN.** Observe a ID da VLAN a ser usada para adaptadores de rede do so de gerenciamento nos servidores, se houver. Você deve ser capaz de obter isso com o administrador da rede.

## <a name="step-1-deploy-windows-server"></a>Etapa 1: Implantar o Windows Server

### <a name="step-11-install-the-operating-system"></a>Etapa 1,1: instalar o sistema operacional

A primeira etapa é instalar o Windows Server em cada servidor que estará no cluster. Espaços de Armazenamento Diretos requer o Windows Server Datacenter Edition. Você pode usar a opção de instalação Server Core ou o servidor com a experiência desktop.

Ao instalar o Windows Server usando o assistente de instalação, você pode escolher entre o *Windows Server* (referindo-se ao Server Core) e o *Windows Server (servidor com a experiência desktop)*, que é o equivalente da opção de instalação *completa* disponível no Windows Server 2012 R2. Se você não escolher, obterá a opção de instalação Server Core. Para obter mais informações, consulte [instalar o Server Core](/windows-server/get-started/getting-started-with-server-core).

### <a name="step-12-connect-to-the-servers"></a>Etapa 1,2: conectar-se aos servidores

Este guia enfoca a opção de instalação Server Core e a implantação/gerenciamento remotamente de um sistema de gerenciamento separado, que deve ter:

- Uma versão do Windows Server ou do Windows 10 pelo menos a novidade dos servidores que está gerenciando e com as atualizações mais recentes
- Conectividade de rede para os servidores que ele está gerenciando
- Ingressado no mesmo domínio ou em um domínio totalmente confiável
- RSAT (Ferramentas de Administração de Servidor Remoto) e módulos do PowerShell para Hyper-V e Cluster de Failover. As ferramentas do RSAT e os módulos do PowerShell estão disponíveis no Windows Server e podem ser instalados sem a instalação de outros recursos. Você também pode instalar o [ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520) em um PC de gerenciamento do Windows 10.

No sistema de gerenciamento, instale as ferramentas de gerenciamento do Hyper-V e Cluster de Failover. Isso pode ser feito no Gerenciador do Servidor usando o assistente **Adicionar Funções e Recursos**. Na página **Recursos**, selecione **Ferramentas de Administração de Servidor Remoto** e selecione as ferramentas para instalação.

Entre na sessão do PS e use o nome do servidor ou o endereço IP do nó ao qual você deseja se conectar. Você será solicitado a fornecer uma senha depois de executar esse comando, insira a senha de administrador que você especificou ao configurar o Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Veja um exemplo de como fazer a mesma coisa de uma maneira que seja mais útil em scripts, caso você precise fazer isso mais de uma vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se você estiver implantando remotamente de um sistema de gerenciamento, poderá receber um erro, como *o WinRM, não pode processar a solicitação.* Para corrigir isso, use o Windows PowerShell para adicionar cada servidor à lista de hosts confiáveis no seu computador de gerenciamento:
>
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>
> Observação: a lista de hosts confiáveis dá suporte a curingas, como `Server*` .
>
> Para exibir sua lista de hosts confiáveis, digite `Get-Item WSMAN:\Localhost\Client\TrustedHosts` .
>
> Para esvaziar a lista, digite `Clear-Item WSMAN:\Localhost\Client\TrustedHost` .

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Etapa 1,3: ingressar no domínio e adicionar contas de domínio

Até agora, você configurou os servidores individuais com a conta de administrador local, `<ComputerName>\Administrator` .

Para gerenciar Espaços de Armazenamento Diretos, você precisará unir os servidores a um domínio e usar uma conta de domínio Active Directory Domain Services que esteja no grupo Administradores em cada servidor.

No sistema de gerenciamento, abra um console do PowerShell com privilégios de administrador. Use `Enter-PSSession` o para se conectar a cada servidor e execute o seguinte cmdlet, substituindo seu próprio nome de computador, nome de domínio e credenciais de domínio:

```PowerShell
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force
```

Se sua conta de administrador de armazenamento não for um membro do grupo Admins. do domínio, adicione sua conta de administrador de armazenamento ao grupo local de administradores em cada nó, ou melhor ainda, adicione o grupo que você usa para administradores de armazenamento. Você pode usar o comando a seguir (ou gravar uma função do Windows PowerShell para fazer isso-consulte [usar o PowerShell para adicionar usuários de domínio a um grupo local](https://devblogs.microsoft.com/scripting/use-powershell-to-add-domain-users-to-a-local-group/) para obter mais informações):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Etapa 1,4: instalar funções e recursos

A próxima etapa é instalar funções de servidor em cada servidor. Você pode fazer isso usando o [centro de administração do Windows](../../manage/windows-admin-center/use/manage-servers.md), [Gerenciador do servidor](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)) ou o PowerShell. Aqui estão as funções a serem instaladas:

- Clustering de failover
- Hyper-V
- Servidor de arquivos (se você quiser hospedar qualquer compartilhamento de arquivos, como para uma implantação convergida)
- Ponte de Data Center (se você estiver usando os adaptadores de rede de RoCEv2 em vez de iWARP)
- RSAT-Clustering-PowerShell
- PowerShell do Hyper-V

Para instalar por meio do PowerShell, use o cmdlet [install-WindowsFeature](/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Você pode usá-lo em um único servidor como este:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para executar o comando em todos os servidores do cluster ao mesmo tempo, use esse pequeno script, modificando a lista de variáveis no início do script para se ajustar ao seu ambiente.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>Etapa 2: Configurar a rede

Se você estiver implantando Espaços de Armazenamento Diretos dentro de máquinas virtuais, pule esta seção.

Espaços de Armazenamento Diretos requer rede de alta largura de banda e baixa latência entre servidores no cluster. Pelo menos 10 redes GbE são necessárias e a RDMA (acesso remoto direto à memória) é recomendada. Você pode usar iWARP ou RoCE contanto que ele tenha o logotipo do Windows Server que corresponda à sua versão do sistema operacional, mas o iWARP geralmente é mais fácil de configurar.

> [!Important]
> Dependendo do equipamento de rede e, especialmente com o RoCE v2, talvez seja necessária alguma configuração do comutador Top-of-rack. A configuração correta do comutador é importante para garantir a confiabilidade e o desempenho de Espaços de Armazenamento Diretos.

O Windows Server 2016 introduziu a opção de agrupamento inserido (conjunto) no comutador virtual do Hyper-V. Isso permite que as mesmas portas NIC físicas sejam usadas para todo o tráfego de rede ao usar RDMA, reduzindo o número de portas NIC físicas necessárias. O agrupamento do switch-Embedded é recomendado para Espaços de Armazenamento Diretos.

Interconexões de nó alternadas ou alternadas
- Comutado: comutadores de rede devem ser configurados corretamente para lidar com a largura de banda e o tipo de rede. Se estiver usando RDMA que implementa o protocolo RoCE, a configuração do dispositivo de rede e do comutador será ainda mais importante.
- SWITCHLESS: os nós podem ser interconectados usando conexões diretas, evitando o uso de uma opção. É necessário que cada nó tenha uma conexão direta com todos os outros nós do cluster.

Para obter instruções sobre como configurar a rede para Espaços de Armazenamento Diretos, consulte o [Guia de implantação do Windows Server 2016 e 2019 RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Etapa 3: Configurar os Espaços de Armazenamento Diretos

As etapas a seguir são realizadas em um sistema de gerenciamento que tem a mesma versão dos servidores que estão sendo configurados. As etapas a seguir não devem ser executadas remotamente usando uma sessão do PowerShell, mas, em vez disso, executadas em uma sessão local do PowerShell no sistema de gerenciamento, com permissões administrativas.

### <a name="step-31-clean-drives"></a>Etapa 3,1: limpar unidades

Antes de habilitar Espaços de Armazenamento Diretos, verifique se as unidades estão vazias: não há partições antigas ou outros dados. Execute o script a seguir, substituindo os nomes dos computadores, para remover todas as partições antigas ou outros dados.

> [!Warning]
> Esse script removerá permanentemente todos os dados em qualquer unidade que não seja a unidade de inicialização do sistema operacional!

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

A saída terá a seguinte aparência, em que **Count** é o número de unidades de cada modelo em cada servidor:

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>Etapa 3,2: validar o cluster

Nesta etapa, você executará a ferramenta de validação de cluster para garantir que os nós de servidor estejam configurados corretamente para criar um cluster usando Espaços de Armazenamento Diretos. Quando a validação de cluster ( `Test-Cluster` ) é executada antes da criação do cluster, ela executa os testes que verificam se a configuração é adequada para funcionar com êxito como um cluster de failover. O exemplo diretamente abaixo usa o `-Include` parâmetro e, em seguida, as categorias específicas de testes são especificadas. Isso garante que os testes dos Espaços de Armazenamento Diretos específicos sejam incluídos na validação.

Use o seguinte comando do PowerShell para validar um conjunto de servidores para uso como um cluster de Espaços de Armazenamento Diretos.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Etapa 3,3: criar o cluster

Nesta etapa, você criará um cluster com os nós que você validou para a criação do cluster na etapa anterior usando o seguinte cmdlet do PowerShell.

Ao criar o cluster, você receberá um aviso informando: "houve problemas ao criar a função clusterizada que pode impedi-lo de iniciar. Para saber mais, consulte o arquivo de relatório abaixo." Ignore esse erro. Ele ocorre devido a indisponibilidade dos discos para o quórum do cluster. Recomendamos a configuração de uma testemunha de compartilhamento de arquivo ou de uma testemunha de nuvem após a criação do cluster.

> [!Note]
> Se os servidores estiverem usando endereços IP estáticos, modifique o comando a seguir para refletir o endereço IP estático, adicionando o seguinte parâmetro e especificando o endereço IP:-StaticAddress &lt;X.X.X.X&gt;.
> No comando a seguir, o espaço reservado ClusterName deve ser substituído por um nome de netbios exclusivo com 15 caracteres ou menos.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Após a criação do cluster, a replicação da entrada de DNS para o nome do cluster pode demorar um pouco. O tempo depende do ambiente e da configuração de replicação de DNS. Se a resolução do cluster não for bem-sucedida, na maioria dos casos, você pode ter sucesso usando o nome do computador de um nó que é membro ativo do cluster em vez de usar o nome do cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Etapa 3,4: configurar uma testemunha de cluster

Recomendamos que você configure uma testemunha para o cluster, de modo que clusters com três ou mais servidores possam resistir a dois servidores com falha ou estarem offline. Uma implantação de dois servidores requer uma testemunha de cluster; caso contrário, o servidor que ficará offline fará com que o outro fique indisponível também. Com esses sistemas, você pode usar um compartilhamento de arquivo como uma testemunha, ou usar uma testemunha de nuvem.

Para obter mais informações, veja os tópicos a seguir:

- [Configurar e gerenciar o quórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem para um Cluster de Failover](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Etapa 3.5: Habilitar os Espaços de Armazenamento Diretos

Depois de criar o cluster, use o `Enable-ClusterStorageSpacesDirect` cmdlet do PowerShell, que colocará o sistema de armazenamento no modo de espaços de armazenamento diretos e faça o seguinte automaticamente:

-   **Criar um pool:** Cria um único pool grande que tem um nome como "S2D em CLUSTER1".

-   **Configura os caches dos Espaços de Armazenamento Diretos:** se houver mais de um tipo de mídia (unidade) disponível para uso dos Espaços de Armazenamento Diretos, ele habilitará o mais rápido como dispositivos de cache (leitura e gravação na maioria dos casos)

-   **Camadas:** Cria duas camadas como camadas padrão. Uma é chamada de "Capacidade" e a outra de "Desempenho". O cmdlet analisa os dispositivos e configura cada camada com a combinação de tipos de dispositivo e resiliência.

Do sistema de gerenciamento, em uma janela de comando do PowerShell aberta com privilégios de Administrador, inicie o comando a seguir. O nome do cluster é o nome do cluster que você criou nas etapas anteriores. Se esse comando for executado localmente em um de nós, o parâmetro -CimSession não será necessário.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar os Espaços de Armazenamento Diretos usando o comando acima, você também pode usar o nome do nó em vez do nome do cluster. O uso do nome de nó pode ser mais confiável devido a atrasos na replicação de DNS que podem ocorrer com o nome do cluster recém-criado.

Após a conclusão desse comando, o que pode demorar alguns minutos, o sistema estará pronto para a criação de volumes.

### <a name="step-36-create-volumes"></a>Etapa 3.6: Criar volumes

É recomendável usar o `New-Volume` cmdlet, pois ele fornece a experiência mais rápida e direta. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

Para obter mais informações, consulte [Criando volumes em Espaços de Armazenamento Diretos](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Etapa 3,7: habilitar opcionalmente o cache CSV

Opcionalmente, você pode habilitar o cache do volume compartilhado do cluster (CSV) para usar a memória do sistema (RAM) como um cache de gravação no nível de bloco de operações de leitura que ainda não estão em cache pelo Gerenciador de cache do Windows. Isso pode melhorar o desempenho de aplicativos como o Hyper-V. O cache CSV pode impulsionar o desempenho de solicitações de leitura e também é útil para cenários de Scale-Out servidor de arquivos.

Habilitar o cache CSV reduz a quantidade de memória disponível para executar VMs em um cluster hiperconvergente, portanto, você terá que balancear o desempenho do armazenamento com a memória disponível para VHDs.

Para definir o tamanho do cache CSV, abra uma sessão do PowerShell no sistema de gerenciamento com uma conta que tenha permissões de administrador no cluster de armazenamento e, em seguida, use esse script, alterando as `$ClusterName` `$CSVCacheSize` variáveis e conforme apropriado (Este exemplo define um cache CSV de 2 GB por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obter mais informações, consulte [usando o cache de leitura em memória CSV](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Etapa 3,8: implantar máquinas virtuais para implantações hiperconvergentes

Se você estiver implantando um cluster hiperconvergente, a última etapa será provisionar máquinas virtuais no cluster Espaços de Armazenamento Diretos.

Os arquivos da máquina virtual devem ser armazenados no namespace CSV dos sistemas (exemplo: c: \\ ClusterStorage \\ Volume1), assim como as VMs clusterizadas em clusters de failover.

Você pode usar ferramentas na caixa ou outras ferramentas para gerenciar o armazenamento e as máquinas virtuais, como System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Etapa 4: implantar Scale-Out servidor de arquivos para soluções convergentes

Se você estiver implantando uma solução convergida, a próxima etapa será criar uma instância de servidor de arquivos Scale-Out e configurar alguns compartilhamentos de arquivos. Se você estiver implantando um cluster hiperconvergente-você terminou e não precisa desta seção.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Etapa 4,1: criar a função de servidor de arquivos Scale-Out

A próxima etapa na configuração dos serviços de cluster para o servidor de arquivos é criar a função de servidor de arquivos clusterizado, que é quando você cria a instância de servidor de arquivos Scale-Out na qual os compartilhamentos de arquivos disponíveis continuamente estão hospedados.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Para criar uma função de servidor de arquivos Scale-Out usando Gerenciador do Servidor

1. Em Gerenciador de Cluster de Failover, selecione o cluster, vá para **funções**e clique em **Configurar função...**.<br>O assistente de alta disponibilidade é exibido.
2. Na página **selecionar função** , clique em **servidor de arquivos**.
3. Na página **tipo de servidor de arquivos** , clique em **servidor de arquivos de escalabilidade horizontal para dados de aplicativo**.
4. Na página **ponto de acesso para cliente** , digite um nome para o servidor de arquivos de Scale-Out.
5. Verifique se a função foi configurada com êxito acessando **funções** e confirmando se a coluna **status** mostra a **execução** ao lado da função de servidor de arquivos clusterizado que você criou, como mostra a Figura 1.

   ![Captura de tela de Gerenciador de Cluster de Failover mostrando o Scale-Out servidor de arquivos](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Gerenciador de Cluster de Failover mostrando o servidor de arquivos de Scale-Out")

    **Figura 1** Gerenciador de Cluster de Failover mostrando o servidor de arquivos de Scale-Out com o status em execução

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguns atrasos de propagação de rede que podem impedi-lo de criar compartilhamentos de arquivos nele por alguns minutos ou possivelmente mais.

#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Para criar uma função de servidor de arquivos Scale-Out usando o Windows PowerShell

 Em uma sessão do Windows PowerShell que está conectada ao cluster de servidor de arquivos, insira os seguintes comandos para criar a função de servidor de arquivos Scale-Out, alterar *FSCLUSTER* para corresponder ao nome do cluster e *SOFS* para corresponder ao nome que você deseja dar à função de servidor de arquivos de Scale-Out:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguns atrasos de propagação de rede que podem impedi-lo de criar compartilhamentos de arquivos nele por alguns minutos ou possivelmente mais. Se a função SOFS falhar imediatamente e não iniciar, talvez seja porque o objeto de computador do cluster não tem permissão para criar uma conta de computador para a função SOFS. Para obter ajuda com isso, consulte esta postagem de blog: [servidor de arquivos de escalabilidade horizontal função falha ao iniciar com as IDs de evento 1205, 1069 e 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Etapa 4,2: criar compartilhamentos de arquivos

Depois de criar os discos virtuais e adicioná-los ao CSVs, é hora de criar compartilhamentos de arquivos neles – um compartilhamento de arquivos por CSV por disco virtual. O System Center Virtual Machine Manager (VMM) é provavelmente a maneira handiest de fazer isso porque ele lida com permissões para você, mas se você não o tiver em seu ambiente, poderá usar o Windows PowerShell para automatizar parcialmente a implantação.

Use os scripts incluídos na [configuração de compartilhamento SMB para o script de cargas de trabalho do Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , que automatiza parcialmente o processo de criação de grupos e compartilhamentos. Ele é escrito para cargas de trabalho do Hyper-V, portanto, se você estiver implantando outras cargas de trabalho, talvez seja necessário modificar as configurações ou executar etapas adicionais depois de criar os compartilhamentos. Por exemplo, se você estiver usando Microsoft SQL Server, a conta de serviço de SQL Server deverá receber controle total sobre o compartilhamento e o sistema de arquivos.

> [!NOTE]
>  Você precisará atualizar a associação de grupo ao adicionar nós de cluster, a menos que use System Center Virtual Machine Manager para criar seus compartilhamentos.

Para criar compartilhamentos de arquivos usando scripts do PowerShell, faça o seguinte:

1. Baixe os scripts incluídos na [configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) em um dos nós do cluster do servidor de arquivos.
2. Abra uma sessão do Windows PowerShell com credenciais de administrador de domínio no sistema de gerenciamento e use o script a seguir para criar um grupo de Active Directory para os objetos de computador Hyper-V, alterando os valores das variáveis conforme apropriado para o seu ambiente:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abra uma sessão do Windows PowerShell com credenciais de administrador em um dos nós de armazenamento e, em seguida, use o script a seguir para criar compartilhamentos para cada CSV e conceder permissões administrativas para os compartilhamentos para o grupo Admins. do domínio e o cluster de computação.

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Etapa 4,3 habilitar a delegação restrita de Kerberos

Para configurar a delegação restrita de Kerberos para o gerenciamento de cenário remoto e aumentar a segurança de Migração ao Vivo, de um dos nós de cluster de armazenamento, use o script KCDSetup.ps1 incluído na [configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Aqui está um pequeno wrapper para o script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Próximas etapas

Depois de implantar o servidor de arquivos clusterizado, é recomendável testar o desempenho da sua solução usando cargas de trabalho sintéticas antes de trazer qualquer carga de trabalho real. Isso permite confirmar se a solução está sendo executada corretamente e solucionar quaisquer problemas remanescentes antes de adicionar a complexidade das cargas de trabalho. Para obter mais informações, consulte [testar o desempenho de espaços de armazenamento usando cargas de trabalho sintéticas](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn894707(v=ws.11)).

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
-   [Noções básicas sobre o cache nos Espaços de Armazenamento Diretos](understand-the-cache.md)
-   [Planejamento de volumes nos Espaços de Armazenamento Diretos](plan-volumes.md)
-   [Tolerância a falhas de espaços de armazenamento](storage-spaces-fault-tolerance.md)
-   [Requisitos de hardware de Espaços de Armazenamento Diretos](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (blog do TechNet)
