---
title: Implantar SOFS em Espaços de Armazenamento Diretos de dois nós para armazenamento UPD no Azure
description: Saiba como usar Espaços de Armazenamento Diretos com o RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 792c9320f6976a4fc7f2ccd235f66daa0cb19b19
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805194"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Implantar um servidor de arquivos de escalabilidade horizontal de dois nós para armazenamento UPD no Azure

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Os Serviços de Área de Trabalho Remota (RDS) requerem um servidor de arquivos associados ao domínio para discos de perfil do usuário (UPDs). Para implantar um SOFS (Servidor de Arquivos de Escalabilidade Horizontal) ingressado no domínio de alta disponibilidade no Azure, use Espaços de Armazenamento Diretos com o Windows Server 2016. ***Se você não está familiarizado com os UPDs ou os Serviços de Área de Trabalho Remota, confira [Bem-vindo aos Serviços de Área de Trabalho Remota](welcome-to-rds.md).

> [!NOTE] 
> A Microsoft acabou de publicar um [modelo do Azure para implantar um servidor de arquivos de escalabilidade horizontal em Espaços de Armazenamento Diretos](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/). Você pode usar o modelo para criar sua implantação ou usar as etapas neste artigo. 

É recomendável implantar o SOFS com VMs da série DS e discos de dados de armazenamento premium, em que há o mesmo número e tamanho dos discos de dados em cada VM. Será necessário um mínimo de duas contas de armazenamento. 

Para implantações pequenas, é recomendável um cluster de dois nós com uma testemunha em nuvem, em que o volume é espelhado com duas cópias. Expanda as implantações pequenas adicionando discos de dados. Expanda as implantações maiores adicionando nós (VMs). 

Essas instruções são para uma implantação de dois nós. A tabela a seguir mostra os tamanhos de VMs e discos que você precisará para armazenar UPDs de acordo com os número de usuários em sua empresa. 

| Usuários | Total (GB) | VM | Nº de discos | Tipo de disco | Tamanho do disco (GB) | Configuração   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x (DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x (DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x (DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x (DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x (DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x (DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x (DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x (DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x (DS5 + 25 P30) | 

Use as etapas a seguir para criar um controlador de domínio (chamamos o nosso de "my-dc" abaixo) e dois nós de VMs ("my-fsn1" e "my-fsn2") e para configurar as VMs para serem um SOFS direto de Espaços de Armazenamento Diretos de dois nós.

1. Crie uma [Assinatura do Microsoft Azure](https://azure.microsoft.com).
2. Entre no [portal do Azure](https://ms.portal.azure.com).
3. Crie uma [conta de armazenamento do Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) no Azure Resource Manager. Crie-o em um novo grupo de recursos e use as seguintes configurações:
   - Modelo de implantação: Resource Manager
   - Tipo de conta de armazenamento: uso geral
   - Camada de desempenho: premium
   - Opção de replicação: LRS
4. Configure uma floresta do Active Directory usando um modelo de início rápido ou implantando a floresta manualmente. 
   - Implante usando um modelo de início rápido do Azure:
      - [Criar uma VM do Azure com uma nova floresta do AD](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Criar um novo domínio do AD com dois controladores de domínio](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (para alta disponibilidade)
   - [Implante a floresta](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) manualmente com as seguintes configurações:
      - Crie a rede virtual no mesmo grupo de recursos que a conta de armazenamento.
      - Tamanho recomendado: DS2 (aumente o tamanho se o controlador de domínio tiver que hospedar mais objetos de domínio)
      - Use uma VNet gerada automaticamente.
      - Siga as etapas para instalar o AD DS.
5. Configure o cluster de servidores de arquivos. Isso é possível implantando o [modelo do Azure para cluster de SOFS de Espaços de Armazenamento Diretos do Windows Server 2016](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) ou seguindo as etapas de 6 a 11 para implantar manualmente.
6. Para configurar manualmente os nós de cluster dos servidores de arquivos:
   1. Crie o primeiro nó: 
      1. Crie uma nova máquina virtual usando a imagem do Windows Server 2016. (Clique em **Novo > Máquinas Virtuais > Windows Server 2016.** Selecione **Gerenciador de Recursos** e clique em **Criar**.)
      2. Defina a configuração básica da seguinte maneira:
         - Nome: my-fsn1
         - SSD do tipo de disco de VM
         - Use um grupo de recursos existente, aquele que você criou na etapa 3. 
      3. Tamanho: DS1, DS2, DS3, DS4 ou DS5, dependendo das necessidades de seus usuários (confira a tabela no início destas instruções). Certifique-se de selecionar o suporte de disco premium.
      4. Configurações: 
         - Conta de armazenamento: escolha a conta de armazenamento que você criou na etapa 3.
         - Alta disponibilidade – crie um novo conjunto de disponibilidade. Clique em **Alta disponibilidade > Criar novo** e insira um nome, por exemplo, s2d-cluster. Use os valores padrão para **Atualizar domínios** e **Domínios com falhas**.
   2. Crie o segundo nó. Repita a etapa acima com as seguintes alterações:
      - Nome: my-fsn2
      - Alta disponibilidade – selecione que o conjunto de disponibilidade criado acima.  
7. [Anexar discos de dados](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) para as VMs do nó de cluster de acordo com as necessidades de seus usuários (como visto na tabela acima). Depois de criar e anexar os dados de discos à VM, defina o **cache de host** como **Nenhum**.
8. Defina os endereços IP de todas as VMs como **estáticos**. 
   1. No grupo de recursos, selecione uma VM e clique em **Interfaces de rede** (em **Configurações**). Selecione o adaptador de rede relacionado e clique em **Configurações de IP**. Selecione a configuração de IP listada, selecione **estático** e clique em **Salvar**.
   2. Observe o endereço IP privado (10.x.x.x) do controlador de domínio (my-dc em nosso exemplo).
9. Defina o endereço do servidor DNS primário em NICs das VMs do nó de cluster como o servidor my-dc. Selecione a VM e clique em **Interfaces de rede > Servidores DNS > DNS personalizado**. Insira o endereço IP privado que você anotou acima e clique em **Salvar**.
10. Crie uma [Conta de armazenamento do Azure para ser sua testemunha em nuvem](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). Se você usou as instruções vinculadas, pare ao chegar à "Configuração de testemunha em nuvem com GUI do Gerenciador de Cluster de Failover" – faremos isso na etapa abaixo.
11. Configure o servidor de arquivos dos Espaços de Armazenamento Diretos. Conecte-se a uma VM de nó e execute os seguintes cmdlets do Windows PowerShell.
    1. Instale o Recurso Cluster de Failover e o Recurso Servidor de Arquivos nas duas VMs de nó de cluster de servidor de arquivos:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Valide as VMs do nó de cluster e crie o cluster SOFS de dois nós:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configure a testemunha em nuvem. Use o nome e a chave de acesso da conta de armazenamento da testemunha em nuvem.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Habilite os Espaços de Armazenamento Diretos.

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. Crie um volume de disco virtual.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       Para exibir informações sobre o volume compartilhado do cluster no cluster SOFS, execute o seguinte cmdlet:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. Crie um servidor de arquivos de escalabilidade horizontal (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Crie um novo compartilhamento de arquivos SMB no cluster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

Agora você tem um compartilhamento em `\\my-sofs1\UpdStorage`, que pode ser usado para armazenamento UPD quando você [habilitar UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) para seus usuários. 
