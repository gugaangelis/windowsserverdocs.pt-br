---
title: Implantar espaços de armazenamento diretos
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: Instruções passo a passo para implantar o armazenamento definido pelo software com espaços de armazenamento diretos no Windows Server como infraestrutura hiperconvergente ou convergido de infraestrutura (também conhecido como desagregada).
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429038"
---
# Implantar espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece instruções passo a passo para implantar [Espaços de armazenamento diretos](storage-spaces-direct-overview.md).

> [!Tip]
> Procurando para adquirir a infraestrutura hiperconvergente? A Microsoft recomenda essas soluções [Definidas pelo Software do Windows Server](https://microsoft.com/wssd) de nossos parceiros. Eles são projetados, montados e validados em relação a nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, para que você obtenha e comecem a trabalhar rapidamente.

> [!Tip]
> Você pode usar máquinas de virtuais do Hyper-V, incluindo no Microsoft Azure, para [avaliar os espaços de armazenamento diretos sem hardware](storage-spaces-direct-in-vm.md). Você também pode revisar os útil [scripts de implantação do Windows Server laboratório rápida](https://aka.ms/wslab), que usamos para fins de treinamento.

## Antes de começar

Examine os [requisitos de hardware de espaços de armazenamento diretos](Storage-Spaces-Direct-Hardware-Requirements.md) e ler este documento para se familiarizar com a abordagem geral e notas importantes associadas a algumas etapas.

Obtenha as seguintes informações:

- **Opção de implantação.** Dá suporte a espaços de armazenamento diretos [duas opções de implantação: hiperconvergente e convergido](storage-spaces-direct-overview.md#deployment-options), também conhecido como desagregada. Familiarize-se com as vantagens de cada para decidir o que é ideal para você. As etapas 1 a 3 abaixo se aplicam a ambas as opções de implantação. Etapa 4 só é necessário para implantação convergente.

- **Nomes de servidor.** Se familiarizar com políticas da nomenclatura da sua organização para computadores, arquivos, caminhos e outros recursos. Você precisará provisionar vários servidores, cada um com nomes exclusivos.

- **Nome de domínio.** Se familiarizar com as políticas da organização para nomeação de domínio e ingresso em domínio.  Você precisará ingressar em servidores ao seu domínio, e você precisará especificar o nome de domínio. 

- **Rede RDMA.** Existem dois tipos de protocolos RDMA: iWarp e RoCE. Observação qual deles usam seus adaptadores de rede, e se RoCE, observe também a versão (v1 ou v2). Para RoCE, observe também o modelo do comutador topo de rack.

- **ID DE VLAN.** Observe o ID de VLAN a ser usado para adaptadores de rede de gerenciamento do sistema operacional em servidores, se houver. Você deve ser capaz de obter isso com o administrador da rede.

## Etapa 1: Implantar o Windows Server

### Etapa 1.1: Instalar o sistema operacional

A primeira etapa é instalar o Windows Server em cada servidor que será no cluster. Espaços de armazenamento diretos requer o Windows Server 2016 Datacenter Edition. Você pode usar a opção de instalação Server Core ou Server com experiência Desktop.

Quando você instala o Windows Server usando o Assistente de instalação, você pode escolher entre o *Windows Server* (fazendo referência ao Server Core) e *Windows Server (servidor com experiência Desktop)*, que é o equivalente a opção de instalação *completa* está disponível no Windows Server 2012 R2. Se você não escolher, você terá a opção de instalação Server Core. Para obter mais informações, consulte as [Opções de instalação do Windows Server 2016](../../get-started/Windows-Server-2016.md).

### Etapa 1.2: Conectar-se aos servidores

Este guia se concentra a opção de instalação Server Core e implantar e gerenciar remotamente a partir de um sistema de gerenciamento separado, que deve ter:

- Windows Server 2016 com as mesmas atualizações dos servidores que ele está gerenciando
- Conectividade de rede para os servidores que ele está gerenciando
- Ingressado no mesmo domínio ou em um domínio totalmente confiável
- RSAT (Ferramentas de Administração de Servidor Remoto) e módulos do PowerShell para Hyper-V e Cluster de Failover. Ferramentas RSAT e módulos do PowerShell estão disponíveis no Windows Server e podem ser instalados sem instalar outros recursos. Você também pode instalar as [Ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520) em um computador de gerenciamento do Windows 10.

No sistema de gerenciamento, instale as ferramentas de gerenciamento do Hyper-V e Cluster de Failover. Isso pode ser feito no Gerenciador do Servidor usando o assistente **Adicionar Funções e Recursos**. Na página **Recursos**, selecione **Ferramentas de Administração de Servidor Remoto** e selecione as ferramentas para instalação.

Entre na sessão do PS e use o nome do servidor ou o endereço IP do nó ao qual você deseja se conectar. Você será solicitado a fornecer uma senha depois de executar esse comando, digite a senha de administrador que você especificou ao configurar o Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Aqui está um exemplo de como fazer a mesma coisa de maneira que seja mais útil em scripts, caso você precise fazer isso mais de uma vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se você estiver implantando remotamente de um sistema de gerenciamento, você pode receber um erro como *WinRM não pode processar a solicitação.* Para corrigir isso, use o Windows PowerShell para adicionar cada servidor à lista de Hosts confiáveis em seu computador de gerenciamento:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Observação: a lista de hosts confiáveis dá suporte a caracteres curinga, como `Server*`.
>
> Para exibir sua lista de Hosts confiáveis, digite `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Para esvaziar a lista, digite `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### Etapa 1.3: Ingressar no domínio e adicionar contas de domínio

Até agora você definiu servidores individuais com a conta de administrador local, `<ComputerName>\Administrator`.

Para gerenciar espaços de armazenamento diretos, você precisará adicionar os servidores a um domínio e usar uma conta de domínio do Active Directory Domain Services que esteja no grupo Administradores em cada servidor.

No sistema de gerenciamento, abra um console do PowerShell com privilégios de administrador. Use `Enter-PSSession` se conectar a cada servidor e execute o seguinte cmdlet, substituindo seu próprio nome de computador, nome de domínio e as credenciais de domínio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Se sua conta de administrador de armazenamento não for um membro do grupo Admins. do domínio, adicionar sua conta de administrador de armazenamento ao grupo de administradores local em cada nó - ou melhor ainda, adicione o grupo que você usa para os administradores de armazenamento. Você pode usar o comando a seguir (ou escreva uma função do Windows PowerShell para fazer isso - consulte [Usar o PowerShell para adicionar usuários de domínio a um grupo Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) para obter mais informações):

```
Net localgroup Administrators <Domain\Account> /add
```

### Etapa 1.4: Instalar funções e recursos

A próxima etapa é instalar funções de servidor em cada servidor. Você pode fazer isso usando o [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Gerenciador do servidor](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), ou o PowerShell. Aqui estão as funções para instalar:

- Clustering de failover
- Hyper-V
- Servidor de arquivos (se você quiser hospedar qualquer compartilhamentos de arquivos, por exemplo, para uma implantação convergente)
- Ponte de Data Center (se você estiver usando os adaptadores de rede de RoCEv2 em vez de iWARP)
- RSAT-Clustering-PowerShell
- PowerShell do Hyper-V

Para instalar por meio do PowerShell, use o cmdlet [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Você pode usá-lo em um único servidor desta forma:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para executar o comando em todos os servidores no cluster ao mesmo tempo, use esse pequeno trecho de script, modificação da lista de variáveis no início do script para ajustar seu ambiente.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## Etapa 2: Configurar a rede

Se você estiver implantando espaços de armazenamento diretos dentro das máquinas virtuais, ignore esta seção.

Espaços de armazenamento diretos requer grande largura de banda, baixa latência de rede entre servidores no cluster. Pelo menos 10 rede GbE é necessária e acesso remoto direto à memória (RDMA) é recomendada. Você pode usar iWARP ou RoCE, desde que tenha o logotipo do Windows Server 2016, mas iWARP é geralmente mais fácil de configurar.

> [!Important]
> Dependendo do seu equipamento de rede e especialmente com RoCE v2, algumas configurações do comutador topo de rack poderão ser exigida. Configuração do switch correto é importante garantir a confiabilidade e o desempenho dos espaços de armazenamento diretos.

Windows Server 2016 introduz incorporado do comutador SET (agrupamento) dentro do switch virtual do Hyper-V. Isso permite que as portas físicas NIC mesmas a ser usado para todo o tráfego de rede enquanto estiverem usando o RDMA, reduzindo o número de portas físicas NIC necessária. Agrupamento incorporado do comutador é recomendado para espaços de armazenamento diretos.

Para obter instruções para configurar a rede para espaços de armazenamento diretos, consulte o [Windows Server 2016 convergiu NIC e guia de implantação de RDMA convidado](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## Etapa 3: Configurar os Espaços de Armazenamento Diretos

As etapas a seguir são realizadas em um sistema de gerenciamento que tem a mesma versão dos servidores que estão sendo configurados. As etapas a seguir não devem ser executadas remotamente usando uma sessão do PowerShell, mas em vez disso, execute em uma sessão do PowerShell local no sistema de gerenciamento, com permissões administrativas.

### Etapa 3.1: Limpar unidades

Antes de habilitar espaços de armazenamento diretos, verifique se as unidades forem vazias: sem antigas partições ou outros dados. Execute o script a seguir, substituindo seus nomes de computador, para remover todas as partições antigas ou outros dados.

> [!Warning]
> Esse script permanentemente removerá todos os dados em todas as unidades que não seja a unidade de inicialização do sistema operacional!

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

A saída será assim, onde a **contagem** é o número de unidades de cada modelo em cada servidor:

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

### Etapa 3.2: Valide o cluster

Nesta etapa, você vai executar a ferramenta de validação de cluster para garantir que os nós do servidor estejam configurados corretamente para criar um cluster usando espaços de armazenamento diretos. Quando a validação de cluster (`Test-Cluster`) é executada antes da criação do cluster, ela executará os testes que verificam se a configuração parece adequada para funcionar com êxito como um cluster de failover. O exemplo diretamente abaixo usa o `-Include` parâmetro e, em seguida, as categorias de testes são especificadas. Isso garante que os testes dos Espaços de Armazenamento Diretos específicos sejam incluídos na validação.

Use o seguinte comando do PowerShell para validar um conjunto de servidores para uso como um cluster de Espaços de Armazenamento Diretos.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### Etapa 3.3: Criar o cluster

Nesta etapa, você criará um cluster com os nós validados para a criação de cluster na etapa anterior, usando o seguinte cmdlet do PowerShell.

Ao criar o cluster, você receberá um aviso informando: "Houve problemas ao criar a função clusterizada que pode impedir sua inicialização. Para saber mais, consulte o arquivo de relatório abaixo." Ignore esse erro. Ele ocorre devido a indisponibilidade dos discos para o quórum do cluster. Recomendamos a configuração de uma testemunha de compartilhamento de arquivo ou de uma testemunha de nuvem após a criação do cluster.

> [!Note]
> Se os servidores estiverem usando endereços IP estáticos, modifique o comando a seguir para refletir o endereço IP estático, adicionando o seguinte parâmetro e especificando o endereço IP:-StaticAddress &lt;X.X.X.X&gt;.
> No comando a seguir, o espaço reservado ClusterName deve ser substituído por um nome de netbios exclusivo com 15 caracteres ou menos.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Após a criação do cluster, a replicação da entrada de DNS para o nome do cluster pode demorar um pouco. O tempo depende do ambiente e da configuração de replicação de DNS. Se a resolução do cluster não for bem-sucedida, na maioria dos casos, você pode ter sucesso usando o nome do computador de um nó que é membro ativo do cluster em vez de usar o nome do cluster.

### Etapa 3.4: Configurar uma testemunha do cluster

Recomendamos que você configure uma testemunha para o cluster, portanto, clusters com três ou mais servidores podem suportar dois servidores ou ficar offline. Uma implantação de dois servidores requer uma testemunha do cluster, caso contrário, qualquer servidor off-line faz com que o outro indisponível também. Com esses sistemas, você pode usar um compartilhamento de arquivo como uma testemunha, ou usar uma testemunha de nuvem. 

Para obter mais informações, consulte os seguintes tópicos:

- [Configurar e gerenciar o quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem para um Cluster de Failover](../../failover-clustering/deploy-cloud-witness.md)

### Etapa 3.5: Habilitar os Espaços de Armazenamento Diretos

Depois de criar o cluster, use o `Enable-ClusterStorageSpacesDirect` cmdlet do PowerShell, que coloca o sistema de armazenamento para o modo de espaços de armazenamento diretos e fará o seguinte automaticamente:

-   **Criar um pool:** cria um grande Pool único com um nome como "S2D no Cluster1".

-   **Configura os caches dos Espaços de Armazenamento Diretos:** se houver mais de um tipo de mídia (unidade) disponível para uso dos Espaços de Armazenamento Diretos, ele habilitará o mais rápido como dispositivos de cache (leitura e gravação na maioria dos casos)

-   **Camadas:** Cria duas camadas como as camadas padrão. Uma é chamada de "Capacidade" e a outra de "Desempenho". O cmdlet analisa os dispositivos e configura cada camada com a combinação de tipos de dispositivo e resiliência.

Do sistema de gerenciamento, em uma janela de comando do PowerShell aberta com privilégios de Administrador, inicie o comando a seguir. O nome do cluster é o nome do cluster que você criou nas etapas anteriores. Se esse comando for executado localmente em um de nós, o parâmetro -CimSession não será necessário.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar os Espaços de Armazenamento Diretos usando o comando acima, você também pode usar o nome do nó em vez do nome do cluster. O uso do nome de nó pode ser mais confiável devido a atrasos na replicação de DNS que podem ocorrer com o nome do cluster recém-criado.

Após a conclusão desse comando, o que pode demorar alguns minutos, o sistema estará pronto para a criação de volumes.

### Etapa 3.6: Criar volumes

É recomendável usar o `New-Volume` cmdlet como ele fornece a experiência mais rápida e mais direta. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

Para obter mais informações, consulte [Criando volumes em Espaços de Armazenamento Diretos](create-volumes.md).

### Etapa 3.7: Opcionalmente, habilitar o cache CSV

Opcionalmente, você pode habilitar o cache de volume (CSV) compartilhados do cluster usar a memória do sistema (RAM) como um cache de nível de bloco através de gravação de operações de leitura que já não são armazenados em cache pelo Gerenciador de cache do Windows. Isso pode melhorar o desempenho para aplicativos como o Hyper-V. O cache CSV pode melhorar o desempenho de solicitações de leitura e também é útil para cenários de servidor de arquivos de escalabilidade horizontal.

Habilitar o cache CSV reduz a quantidade de memória disponível para executar VMs em um cluster hiperconvergente, e você terá equilibrar desempenho de armazenamento com memória disponível para VHDs.

Para definir o tamanho do cache CSV, abra uma sessão do PowerShell no sistema de gerenciamento com uma conta que tenha permissões de administrador no cluster de armazenamento e, em seguida, use esse script, alterando o `$ClusterName` e `$CSVCacheSize` variáveis conforme apropriado (Este exemplo define um 2 GB CSV cache por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obter mais informações, consulte [usando CSV na memória cache de leitura](csv-cache.md).

### Etapa 3.8: Implantar máquinas virtuais para implantações hiperconvergidas

Se você estiver implantando um cluster hiperconvergente, a última etapa é provisionar máquinas virtuais no cluster de espaços de armazenamento diretos.

Os arquivos da máquina virtual devem ser armazenados no namespace CSV dos sistemas (exemplo: c:\\ClusterStorage\\Volume1) assim como VMs em cluster em clusters de failover.

Você pode usar ferramentas nativas ou outras ferramentas para gerenciar o armazenamento e as máquinas virtuais, como o System Center Virtual Machine Manager.

## Etapa 4: Implantar o servidor de arquivos de escalabilidade horizontal para soluções convergentes

Se você estiver implantando uma solução convergida, a próxima etapa é criar uma instância de servidor de arquivos de escalabilidade horizontal e alguns compartilhamentos de arquivos de instalação. Se você estiver implantando um cluster hiperconvergente, você tiver terminado de trabalhar e não precisa isso seção.

### Etapa 4.1: Criar a função de servidor de arquivos de escalabilidade horizontal

A próxima etapa em configurar os serviços de cluster para seu servidor de arquivos está criando a função de servidor de arquivos em cluster, que é quando você cria a instância do servidor de arquivos de escalabilidade horizontal hospedados no qual seus compartilhamentos de arquivos disponíveis continuamente.

#### Criar uma função de servidor de arquivos de escalabilidade horizontal, usando o Gerenciador do servidor

1.  No Gerenciador de Cluster de Failover, selecione o cluster, vá para **funções**e, em seguida, clique em **Configurar função...**.<br>O Assistente para alta disponibilidade é exibida.
2.  Na página **Selecionar função** , clique em **Servidor de arquivos**.
3.  Na página **Tipo de servidor de arquivo** , clique em **Servidor de arquivos de escalabilidade horizontal para dados de aplicativo**.
4.  Na página do **Ponto de acesso do cliente** , digite um nome para o servidor de arquivos de escalabilidade horizontal.
5.  Verifique se a função foi configurada com êxito acessando **funções** e confirmando que a coluna **Status** mostra **em execução** ao lado da função de servidor de arquivos em cluster, você criou, conforme mostrado na Figura 1.

    ![Captura de tela de Failover Gerenciador de Cluster mostrando o servidor de arquivos de escalabilidade horizontal](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **Figura 1** Gerenciador de Cluster de failover, mostrando o servidor de arquivos de escalabilidade horizontal com o status de execução

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguns rede atrasos de propagação que podem impedir que você crie compartilhamentos de arquivos nele por alguns minutos, ou potencialmente mais.  
  
#### Para criar uma função de servidor de arquivos de escalabilidade horizontal usando o Windows PowerShell

 Em uma sessão do Windows PowerShell que está conectada ao cluster de servidor de arquivo, digite os seguintes comandos para criar a função de servidor de arquivos de escalabilidade horizontal, alterando *FSCLUSTER* para corresponder ao nome do cluster e *SOFS* para corresponder ao nome que você deseja dar a Função de servidor de arquivos de escalabilidade horizontal:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Depois de criar a função clusterizada, pode haver alguns rede atrasos de propagação que podem impedir que você crie compartilhamentos de arquivos nele por alguns minutos, ou potencialmente mais. Se a função SOFS falha imediatamente e não será iniciado, talvez seja porque o objeto de computador do cluster não tem permissão para criar uma conta de computador para a função SOFS. Para ajudar com esse, consulte esta postagem no blog: [Scale-Out arquivo servidor função falhar para iniciar com eventos IDs 1205, 1069 e 1194](http://www.aidanfinn.com/?p=14142).

### Etapa 4.2: Criar compartilhamentos de arquivos

Depois que você criou os discos virtuais e adicioná-los para CSVs, é hora de criar compartilhamentos de arquivos neles - compartilhamento de um arquivo por CSV por disco virtual. System Center Virtual Machine Manager (VMM) é provavelmente a maneira handiest fazer isso porque ele lida com permissões para você, mas se você não tivê-lo em seu ambiente, você pode usar o Windows PowerShell para automatizar a implantação parcialmente.

Use os scripts incluídos no script de [Configuração do compartilhamento SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , que parcialmente automatiza o processo de criação de grupos e compartilhamentos. Ele foi escrito para cargas de trabalho do Hyper-V, portanto, se você estiver implantando outras cargas de trabalho, talvez seja necessário modificar as configurações ou realizar etapas adicionais depois de criar os compartilhamentos. Por exemplo, se você estiver usando o Microsoft SQL Server, a conta de serviço do SQL Server deve ser concedida controle total sobre o compartilhamento e o sistema de arquivos.

> [!NOTE]
>  Você precisará atualizar a associação do grupo quando você adiciona nós de cluster, a menos que você use o System Center Virtual Machine Manager para criar seus compartilhamentos.

Para criar compartilhamentos de arquivos usando scripts do PowerShell, faça o seguinte:

1. Baixe os scripts incluídos na [Configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) para um de nós do cluster do servidor de arquivo.
2. Abra uma sessão do Windows PowerShell com credenciais de administrador de domínio no sistema de gerenciamento e, em seguida, use o script a seguir para criar um grupo do Active Directory para os objetos de computador do Hyper-V, alterando os valores das variáveis conforme apropriado para seu ambiente:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abra uma sessão do Windows PowerShell com credenciais de administrador em um de nós de armazenamento e, em seguida, use o seguinte script para criar compartilhamentos para cada CSV e conceder permissões administrativas para os compartilhamentos ao grupo Administradores do domínio e o cluster de computação.

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

### Delegação restrita de Kerberos de habilitar a etapa 4.3

Para configurar a delegação restringido de Kerberos para gerenciamento remoto do cenário e maior segurança de migração ao vivo, de um de nós do cluster de armazenamento, use o script KCDSetup.ps1 incluído na [Configuração de compartilhamento SMB para cargas de trabalho do Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Aqui está um pouco wrapper para o script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## Próximas etapas

Depois de implantar o servidor de arquivos em cluster, é recomendável testar o desempenho da sua solução usando cargas de trabalho sintéticas antes de acessar o cargas de trabalho reais. Isso permite que você confirmar que a solução está realizando corretamente e descobrir problemas remanescentes antes de adicionar a complexidade de cargas de trabalho. Para obter mais informações, consulte [Teste armazenamento espaços desempenho usando cargas de trabalho sintéticas](https://technet.microsoft.com/library/dn894707.aspx).

## Consulte também

-   [Espaços de Armazenamento Diretos no Windows Server 2016](storage-spaces-direct-overview.md)
-   [Noções básicas sobre o cache nos Espaços de Armazenamento Diretos](understand-the-cache.md)
-   [Planejamento de volumes nos Espaços de Armazenamento Diretos](plan-volumes.md)
-   [Tolerância a falhas de Espaços de Armazenamento](storage-spaces-fault-tolerance.md)
-   [Requisitos de hardware dos Espaços de Armazenamento Diretos](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (blog do TechNet)
