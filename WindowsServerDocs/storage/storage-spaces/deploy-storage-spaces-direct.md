---
title: Implantar espaços de armazenamento diretos
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Instruções passo a passo para implantar o armazenamento definido por software com espaços de armazenamento diretos no Windows Server como infraestrutura hiperconvergente ou convergida infra-estrutura (também conhecida como desagregada).
ms.localizationpriority: medium
ms.openlocfilehash: a4159c85be23025ef57084b47dcc77d4f749888f
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812356"
---
# <a name="deploy-storage-spaces-direct"></a>Implantar espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece instruções passo a passo para implantar [espaços de armazenamento diretos](storage-spaces-direct-overview.md).

> [!Tip]
> Buscando para adquirir infraestrutura Hyper-Converged? A Microsoft recomenda a compra de uma solução de hardware/software validado de nossos parceiros, que incluem procedimentos e ferramentas de implantação. Essas soluções são projetadas, reunimos e validadas em relação a nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, para que você começar a trabalhar em funcionamento rapidamente. Para soluções do Windows Server 2019, visite o [site de soluções do Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Para soluções do Windows Server 2016, saiba mais em [definida pelo Software do Windows Server](https://microsoft.com/wssd).

> [!Tip]
> Você pode usar máquinas virtuais de Hyper-V, incluindo no Microsoft Azure, para [avaliar os espaços de armazenamento diretos sem hardware](storage-spaces-direct-in-vm.md). Também convém examinar o útil [scripts de implantação do Windows Server laboratório rápida](https://aka.ms/wslab), que usamos para fins de treinamento.

## <a name="before-you-start"></a>Antes de começar

Examine os [requisitos de hardware de espaços de armazenamento diretos](Storage-Spaces-Direct-Hardware-Requirements.md) e ler este documento para se familiarizar com a abordagem geral e notas importantes associadas a algumas etapas.

Reúna as seguintes informações:

- **Opção de implantação.** Dá suporte a espaços de armazenamento diretos [duas opções de implantação: hiperconvergente e convergido](storage-spaces-direct-overview.md#deployment-options), também conhecida como desagregada. Familiarize-se com as vantagens de cada para decidir qual é a certa para você. As etapas 1 a 3 abaixo se aplicam a ambas as opções de implantação. Etapa 4 só será necessário para a implantação convergida.

- **Nomes de servidor.** Se familiarizar com as políticas da nomenclatura da sua organização para computadores, arquivos, caminhos e outros recursos. Você precisará provisionar vários servidores, cada um com nomes exclusivos.

- **Nome de domínio.** Se familiarizar com as políticas da organização para nomeação de domínio e o ingresso no domínio.  Você ingressará os servidores ao seu domínio, e você precisará especificar o nome de domínio. 

- **Rede RDMA.** Há dois tipos de protocolos RDMA: iWarp e RoCE. Observe qual deles usarem de seus adaptadores de rede, e se RoCE, observe também a versão (v1 ou v2). Para RoCE, observe também o modelo de seu comutador top-of-rack.

- **ID DA VLAN.** Anote a ID de VLAN a ser usado para adaptadores de rede do sistema operacional de gerenciamento nos servidores, se houver. Você deve ser capaz de obter isso com o administrador da rede.

## <a name="step-1-deploy-windows-server"></a>Etapa 1: Implantar o Windows Server

### <a name="step-11-install-the-operating-system"></a>Etapa 1.1: Instalar o sistema operacional

A primeira etapa é instalar o Windows Server em todos os servidores que estarão no cluster. Espaços de armazenamento diretos requer o Windows Server 2016 Datacenter Edition. Você pode usar a opção de instalação Server Core ou servidor com experiência Desktop.

Quando você instala o Windows Server usando o Assistente de instalação, você pode escolher entre *Windows Server* (se referir ao Server Core) e *do Windows Server (servidor com experiência Desktop)* , que é o equivalente dos *completo* opção de instalação disponível no Windows Server 2012 R2. Se você não escolher, você terá a opção de instalação Server Core. Para obter mais informações, consulte [opções de instalação para o Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Etapa 1.2: Conectar-se aos servidores

Este guia concentra-se a opção de instalação Server Core e a implantar e gerenciar remotamente a partir de um sistema de gerenciamento separado, que deve ter:

- Windows Server 2016 com as mesmas atualizações dos servidores que ele está gerenciando
- Conectividade de rede para os servidores que está gerenciando
- Ingressado no mesmo domínio ou um domínio totalmente confiável
- RSAT (Ferramentas de Administração de Servidor Remoto) e módulos do PowerShell para Hyper-V e Cluster de Failover. Ferramentas RSAT e os módulos do PowerShell estão disponíveis no Windows Server e podem ser instalados sem a instalação de outros recursos. Você também pode instalar o [ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520) em um PC de gerenciamento do Windows 10.

No sistema de gerenciamento, instale as ferramentas de gerenciamento do Hyper-V e Cluster de Failover. Isso pode ser feito no Gerenciador do Servidor usando o assistente **Adicionar Funções e Recursos**. Na página **Recursos**, selecione **Ferramentas de Administração de Servidor Remoto** e selecione as ferramentas para instalação.

Entre na sessão do PS e use o nome do servidor ou o endereço IP do nó ao qual você deseja se conectar. Você será solicitado a fornecer uma senha depois de executar esse comando, digite a senha de administrador que você especificou ao configurar o Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Aqui está um exemplo de como fazer a mesma coisa de uma maneira que é mais útil em scripts, caso você precise fazer isso mais de uma vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se você estiver implantando remotamente a partir de um sistema de gerenciamento, você poderá receber um erro como *WinRM não é possível processar a solicitação.* Para corrigir isso, use o Windows PowerShell para adicionar cada servidor à lista de Hosts confiáveis em seu computador de gerenciamento:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Observação: a lista de hosts confiáveis oferece suporte a caracteres curinga, como `Server*`.
>
> Para exibir sua lista de Hosts confiáveis, digite `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Para esvaziar a lista, digite `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Etapa 1.3: Ingressar no domínio e adicione contas de domínio

Até agora você configurou servidores individuais com a conta de administrador local, `<ComputerName>\Administrator`.

Para gerenciar espaços de armazenamento diretos, você precisará ingressar os servidores em um domínio e usar uma conta de domínio do Active Directory Domain Services que está no grupo de administradores em todos os servidores.

Do sistema de gerenciamento, abra um console do PowerShell com privilégios de administrador. Use `Enter-PSSession` para se conectar a cada servidor e execute o cmdlet a seguir, substituindo seu próprio nome do computador, nome de domínio e as credenciais de domínio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Se sua conta de administrador de armazenamento não for um membro do grupo Admins. do domínio, adicione sua conta de administrador de armazenamento para o grupo de administradores local em cada nó - ou melhor ainda, adicione o grupo que você pode usar para os administradores de armazenamento. Você pode usar o comando a seguir (ou escrever uma função do Windows PowerShell para fazer isso – consulte [usar o PowerShell para adicionar usuários de domínio a um grupo Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) para obter mais informações):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Etapa 1.4: Instalar funções e recursos

A próxima etapa é instalar as funções de servidor em todos os servidores. Você pode fazer isso usando [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Gerenciador do servidor](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), ou o PowerShell. Aqui estão as funções para instalar:

- Clustering de failover
- Hyper-V
- Servidor de arquivos (se você deseja hospedar qualquer compartilhamentos de arquivos, por exemplo, para uma implantação convergida)
- Ponte de Data Center (se você estiver usando os adaptadores de rede de RoCEv2 em vez de iWARP)
- RSAT-Clustering-PowerShell
- PowerShell do Hyper-V

Para instalar por meio do PowerShell, use o [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet. Você pode usá-lo em um único servidor, como este:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para executar o comando em todos os servidores no cluster ao mesmo tempo, use esse pequeno trecho de script, modificando a lista de variáveis no início do script de acordo com seu ambiente.

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

Se você estiver implantando o espaços de armazenamento diretos dentro das máquinas virtuais, ignore esta seção.

Espaços de armazenamento diretos requer largura de banda alta, baixa latência de rede entre os servidores no cluster. Sistema de rede pelo menos 10 GbE é necessária e acesso remoto direto à memória (RDMA) é recomendada. Você pode usar iWARP ou RoCE, desde que ela tem o logotipo do Windows Server 2016, mas iWARP é geralmente mais fácil de configurar.

> [!Important]
> Dependendo do seu equipamento de rede e especialmente com RoCE v2, algumas configurações do comutador top-of-rack podem ser necessária. Configuração do comutador correta é importante para garantir a confiabilidade e desempenho dos espaços de armazenamento diretos.

Windows Server 2016 apresenta switch incorporado agrupamento (SET) no comutador virtual Hyper-V. Isso permite que as mesmas portas NIC físicas a ser usado para todo o tráfego de rede ao usar o RDMA, reduzindo o número de portas físicas do NIC necessárias. Switch incorporado agrupamento é recomendado para espaços de armazenamento diretos.

Para obter instruções para configurar a rede para espaços de armazenamento diretos, consulte [convergido NIC do Windows Server 2016 e o guia de implantação do convidado RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Etapa 3: Configurar os Espaços de Armazenamento Diretos

As etapas a seguir são realizadas em um sistema de gerenciamento que tem a mesma versão dos servidores que estão sendo configurados. As etapas a seguir não devem ser executadas remotamente usando uma sessão do PowerShell, mas em vez disso, execute em uma sessão do PowerShell local no sistema de gerenciamento, com permissões administrativas.

### <a name="step-31-clean-drives"></a>Etapa 3.1: Limpar unidades

Antes de habilitar espaços de armazenamento diretos, certifique-se de suas unidades estão vazias: não há partições antigas ou outros dados. Execute o script a seguir, substituindo seus nomes de computador, para remover todas as partições antigas ou outros dados.

> [!Warning]
> Esse script removerá permanentemente todos os dados em todas as unidades que não seja a unidade de inicialização do sistema operacional!

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

A saída será assim, onde **contagem** é o número de unidades de cada modelo em cada servidor:

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

### <a name="step-32-validate-the-cluster"></a>Etapa 3.2: Validar o cluster

Nesta etapa, você executará a ferramenta de validação de cluster para garantir que os nós do servidor estão configurados corretamente para criar um cluster usando espaços de armazenamento diretos. Quando a validação de cluster (`Test-Cluster`) é executado antes do cluster é criado, ele executa os testes que verificam se a configuração parece adequada para funcionar com êxito como um cluster de failover. O exemplo diretamente abaixo usa o `-Include` parâmetro e, em seguida, as categorias de testes são especificadas. Isso garante que os testes dos Espaços de Armazenamento Diretos específicos sejam incluídos na validação.

Use o seguinte comando do PowerShell para validar um conjunto de servidores para uso como um cluster de Espaços de Armazenamento Diretos.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Etapa 3.3: Criar o cluster

Nesta etapa, você criará um cluster com os nós validados para a criação de cluster na etapa anterior, usando o seguinte cmdlet do PowerShell.

Ao criar o cluster, você receberá um aviso dizendo: "Houve problemas ao criar a função clusterizada que pode impedir sua inicialização. Para saber mais, consulte o arquivo de relatório abaixo." Você pode ignorar com segurança esse aviso. Ele ocorre devido a indisponibilidade dos discos para o quórum do cluster. Recomendamos a configuração de uma testemunha de compartilhamento de arquivo ou de uma testemunha de nuvem após a criação do cluster.

> [!Note]
> Se os servidores estiverem usando endereços IP estáticos, modifique o comando a seguir para refletir o endereço IP estático, adicionando o seguinte parâmetro e especificando o endereço IP:-StaticAddress &lt;X.X.X.X&gt;.
> No comando a seguir, o espaço reservado ClusterName deve ser substituído por um nome de netbios exclusivo com 15 caracteres ou menos.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Após a criação do cluster, a replicação da entrada de DNS para o nome do cluster pode demorar um pouco. O tempo depende do ambiente e da configuração de replicação de DNS. Se a resolução do cluster não for bem-sucedida, na maioria dos casos, você pode ter sucesso usando o nome do computador de um nó que é membro ativo do cluster em vez de usar o nome do cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Etapa 3.4: Configurar uma testemunha do cluster

É recomendável que você configure uma testemunha para o cluster, para que os clusters com três ou mais servidores podem dar suporte a dois servidores falhar ou ficar offline. Uma implantação de dois servidores requer uma testemunha do cluster, caso contrário, o servidor ficar offline faz com que o outro indisponível também. Com esses sistemas, você pode usar um compartilhamento de arquivo como uma testemunha, ou usar uma testemunha de nuvem. 

Para obter mais informações, consulte os seguintes tópicos:

- [Configurar e gerenciar o quórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem para um Cluster de Failover](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Etapa 3.5: Habilitar os Espaços de Armazenamento Diretos

Depois de criar o cluster, use o `Enable-ClusterStorageSpacesDirect` cmdlet do PowerShell, que vai colocar o sistema de armazenamento no modo de espaços de armazenamento diretos e faça o seguinte automaticamente:

-   **Crie um pool:** Cria um grande pool único que tem um nome como "S2D no Cluster1".

-   **Configura os caches de espaços de armazenamento diretos:** Se houver mais de uma mídia de tipo de (unidade) disponível para uso de espaços de armazenamento diretos, permite que o mais rápido como dispositivos de cache (leitura e gravação na maioria dos casos)

-   **Camadas:** Cria duas camadas como as camadas padrão. Uma é chamada de "Capacidade" e a outra de "Desempenho". O cmdlet analisa os dispositivos e configura cada camada com a combinação de tipos de dispositivo e resiliência.

Do sistema de gerenciamento, em uma janela de comando do PowerShell aberta com privilégios de Administrador, inicie o comando a seguir. O nome do cluster é o nome do cluster que você criou nas etapas anteriores. Se esse comando for executado localmente em um de nós, o parâmetro -CimSession não será necessário.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar os Espaços de Armazenamento Diretos usando o comando acima, você também pode usar o nome do nó em vez do nome do cluster. O uso do nome de nó pode ser mais confiável devido a atrasos na replicação de DNS que podem ocorrer com o nome do cluster recém-criado.

Após a conclusão desse comando, o que pode demorar alguns minutos, o sistema estará pronto para a criação de volumes.

### <a name="step-36-create-volumes"></a>Etapa 3.6: Criar volumes

É recomendável usar o `New-Volume` cmdlet como ele fornece a experiência mais rápida e mais simples. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

Para obter mais informações, consulte [Criando volumes em Espaços de Armazenamento Diretos](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Etapa 3.7: Opcionalmente, habilitar o cache do CSV

Opcionalmente, você pode habilitar o cache compartilhado do cluster CSV (volume) usar a memória do sistema (RAM) como um cache write-through em nível de bloco de operações de leitura que já não são armazenados em cache pelo Gerenciador de cache do Windows. Isso pode melhorar o desempenho de aplicativos como o Hyper-V. O cache do CSV pode melhorar o desempenho de solicitações de leitura e também é útil para cenários de servidor de arquivos de escalabilidade horizontal.

Habilitar o cache do CSV reduz a quantidade de memória disponível para executar VMs em um cluster hiperconvergente, portanto, você precisará equilibrar o desempenho de armazenamento com a memória disponível para VHDs.

Para definir o tamanho do cache do CSV, abra uma sessão do PowerShell no sistema de gerenciamento com uma conta que tenha permissões de administrador no cluster de armazenamento e, em seguida, usar esse script, alterando a `$ClusterName` e `$CSVCacheSize` variáveis conforme apropriado (isso exemplo define um cache CSV de 2 GB por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obter mais informações, consulte [usar o CSV na memória cache de leitura](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Etapa 3.8: Implantar máquinas virtuais para implantações hiperconvergentes

Se você estiver implantando um cluster hiperconvergente, a última etapa é provisionar máquinas virtuais no cluster de espaços de armazenamento diretos.

Arquivos da máquina virtual devem ser armazenados no namespace CSV dos sistemas (exemplo: c:\\ClusterStorage\\Volume1) assim como as VMs em cluster em clusters de failover.

Você pode usar na caixa ferramentas ou outras ferramentas para gerenciar o armazenamento e máquinas virtuais, como o System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Etapa 4: Implantar o servidor de arquivos de expansão para soluções convergidas

Se você estiver implantando uma solução convergida, a próxima etapa é criar uma instância de servidor de arquivos de escalabilidade horizontal e alguns compartilhamentos de arquivos de instalação. Se você estiver implantando um cluster hiperconvergente - tiver terminado e não for necessário seção.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Etapa 4.1: Criar a função de servidor de arquivos de escalabilidade horizontal

A próxima etapa na configuração de serviços do cluster para seu servidor de arquivos está criando a função de servidor de arquivos em cluster, que é quando você cria a instância de servidor de arquivos de escalabilidade horizontal no qual seus compartilhamentos de arquivos continuamente disponíveis são hospedados.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Para criar uma função de servidor de arquivos de escalabilidade horizontal usando o Gerenciador do servidor

1. No Gerenciador de Cluster de Failover, selecione o cluster, acesse **funções**e, em seguida, clique em **configurar função...** .<br>O Assistente para alta disponibilidade é exibida.
2. Sobre o **Selecionar função** , clique em **servidor de arquivos**.
3. Sobre o **tipo de servidor de arquivos** , clique em **servidor de arquivos de escalabilidade horizontal para dados de aplicativo**.
4. Sobre o **ponto de acesso para cliente** página, digite um nome para o servidor de arquivos de escalabilidade horizontal.
5. Verifique se que a função foi configurada com êxito, vá para **funções** e confirmando que o **Status** coluna mostra **executando** ao lado da função de servidor de arquivos em cluster é criado, conforme mostrado na Figura 1.

   ![Captura de tela do Gerenciador de Cluster Failover mostrando o servidor de arquivos de escalabilidade horizontal](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Gerenciador de Cluster de Failover mostrando o servidor de arquivos de escalabilidade horizontal")

    **Figura 1** mostrando o servidor de arquivos de escalabilidade horizontal com o status de execução do Gerenciador de Cluster de Failover

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguma rede atrasos de propagação que pode impedir que você crie compartilhamentos de arquivos nele por alguns minutos, ou potencialmente mais.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Para criar uma função de servidor de arquivos de escalabilidade horizontal por meio do Windows PowerShell

 Em uma sessão do Windows PowerShell que está conectada ao cluster de servidor de arquivos, digite os seguintes comandos para criar a função de servidor de arquivos de escalabilidade horizontal, alterando *FSCLUSTER* para corresponder ao nome do cluster, e *SOFS* para corresponder ao nome que você deseja fornecer à função de servidor de arquivos de escalabilidade horizontal:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguma rede atrasos de propagação que pode impedir que você crie compartilhamentos de arquivos nele por alguns minutos, ou potencialmente mais. Se a função SOFS falhará imediatamente e não será iniciado, ele pode ser porque o objeto de computador do cluster não tem permissão para criar uma conta de computador para a função SOFS. Para obter ajuda, consulte esta postagem de blog: [Falha na função de servidor do arquivo inicial com IDs de evento 1205, 1069 e 1194 expansão](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Etapa 4.2: Criar compartilhamentos de arquivos

Depois que você criou seus discos virtuais e adicioná-las ao CSVs, é hora de criar compartilhamentos de arquivos neles – um compartilhamento de arquivos por CSV por disco virtual. Provavelmente, o System Center Virtual Machine Manager (VMM) é a maneira handiest fazer isso porque ele lida com as permissões para você, mas se você não o tiver em seu ambiente, você pode usar o Windows PowerShell para automatizar parcialmente a implantação.

Use os scripts incluídos os [a configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) script, que é parcialmente automatiza o processo de criação de grupos e compartilhamentos. Ele é gravado para cargas de trabalho do Hyper-V, portanto, se você estiver implantando outras cargas de trabalho, você talvez precise modificar as configurações ou executar etapas adicionais depois de criar os compartilhamentos. Por exemplo, se você estiver usando o Microsoft SQL Server, a conta de serviço do SQL Server deve ter controle total no compartilhamento e o sistema de arquivos.

> [!NOTE]
>  Você precisará atualizar a associação de grupo quando você adiciona nós de cluster, a menos que você usar o System Center Virtual Machine Manager para criar seus compartilhamentos.

Para criar compartilhamentos de arquivos por meio de scripts do PowerShell, faça o seguinte:

1. Baixe os scripts incluídos [configuração de compartilhamento de SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) para um de nós do cluster de servidor de arquivos.
2. Abra uma sessão do Windows PowerShell com credenciais de administrador de domínio no sistema de gerenciamento e, em seguida, use o seguinte script para criar um grupo do Active Directory para os objetos de computador do Hyper-V, alterando os valores para as variáveis conforme apropriado para seu ambiente:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abra uma sessão do Windows PowerShell com credenciais de administrador em um de nós de armazenamento e, em seguida, use o seguinte script para criar compartilhamentos para cada CSV e conceder permissões administrativas para os compartilhamentos para o grupo Admins. do domínio e o cluster de computação.

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Delegação restrita de Kerberos de habilitar a etapa 4.3

Para configurar a delegação restringido de Kerberos para gerenciamento remoto do cenário e aumentar a segurança migração ao vivo, de um de nós do cluster de armazenamento, usam o script KCDSetup.ps1 incluído no [a configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Aqui está um pouco wrapper para o script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Próximas etapas

Depois de implantar o servidor de arquivos em cluster, é recomendável testar o desempenho de sua solução usando cargas de trabalho sintéticas antes de colocar o backup de quaisquer cargas de trabalho real. Isso permite que você confirme que a solução está sendo executado corretamente e solucione os problemas remanescentes antes de adicionar a complexidade das cargas de trabalho. Para obter mais informações, consulte [teste de armazenamento de espaços de desempenho usando cargas de trabalho sintéticas](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Consulte também

-   [Espaços de armazenamento diretos no Windows Server 2016](storage-spaces-direct-overview.md)
-   [Entender o cache em espaços de armazenamento diretos](understand-the-cache.md)
-   [Planejando volumes em espaços de armazenamento diretos](plan-volumes.md)
-   [Tolerância a falhas de espaços de armazenamento](storage-spaces-fault-tolerance.md)
-   [Requisitos de Hardware de espaços de armazenamento diretos](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (blog do TechNet)
