---
title: Implantar um SOFS de direto de espaços de armazenamento de dois nós para armazenamento UPD no Azure
description: Saiba como usar espaços de armazenamento diretos com o RDS.
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
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805194"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Implantar um servidor de arquivos de escalabilidade horizontal de espaços de armazenamento diretos de dois nós para armazenamento UPD no Azure

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Serviços de área de trabalho remota (RDS) requer um servidor de arquivos associados ao domínio para discos de perfil do usuário (UPDs). Para implantar um servidor de arquivos de escalabilidade horizontal ingressado no domínio de alta disponibilidade (SOFS) no Azure, use espaços de armazenamento diretos com o Windows Server 2016. Se você não estiver familiarizado com os UPDs ou serviços de área de trabalho remota, fazer check-out [bem-vindo ao Remote Desktop Services](welcome-to-rds.md).

> [!NOTE] 
> Microsoft acabou de publicar uma [modelo do Azure para implantar um servidor de arquivos de escalabilidade horizontal de espaços de armazenamento diretos](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)! Você pode usar o modelo para criar sua implantação, ou use as etapas neste artigo. 

É recomendável implantar o SOFS com VMs da série DS e discos de dados do armazenamento premium, em que há o mesmo número e tamanho dos discos de dados em cada VM. Será necessário um mínimo de duas contas de armazenamento. 

Para implantações pequenas, recomendamos um cluster de 2 nós com uma testemunha de nuvem, em que o volume é espelhado com 2 cópias. Expandir as implantações pequenas adicionando discos de dados. Expandir as implantações maiores Adicionando nós (VMs). 

Essas instruções são para uma implantação de 2 nós. A tabela a seguir mostra os tamanhos VM e disco, que você precisará armazenar UPDs para o número de usuários em sua empresa. 

| Usuários | Total (GB) | VM | # Discos | Tipo de disco | Tamanho do disco (GB) | Configuração   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

Use as seguintes etapas para criar um controlador de domínio (chamamos a nossa "my dc" abaixo) e dois nós de VMs ("my fsn1" e "my fsn2") e configurar as VMs para ser um SOFS direto dos espaços de armazenamento de 2 nós.

1. Criar uma [assinatura do Microsoft Azure](https://azure.microsoft.com).
2. Entrar a [portal do Azure](https://ms.portal.azure.com).
3. Criar uma [conta de armazenamento do Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) no Azure Resource Manager. Criá-lo em um novo grupo de recursos e use as seguintes configurações:
   - Modelo de implantação: Gerenciador de recursos
   - Tipo de conta de armazenamento: Uso geral
   - Nível de desempenho: Premium
   - Opção de replicação: LRS
4. Configure uma floresta do Active Directory usando um modelo de início rápido ou implantando a floresta manualmente. 
   - Implante usando um modelo de início rápido do Azure:
      - [Criar uma VM do Azure com uma nova floresta do AD](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Criar um novo domínio do AD com 2 controladores de domínio](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (para alta disponibilidade)
   - Manualmente [implantar a floresta](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) com as seguintes configurações:
      - Crie a rede virtual no mesmo grupo de recursos como a conta de armazenamento.
      - Tamanho recomendado: DS2 (aumentar o tamanho se o controlador de domínio hospedará mais objetos de domínio)
      - Use um VNet gerado automaticamente.
      - Siga as etapas para instalar o AD DS.
5. Configure os nós de cluster de servidor de arquivos. Você pode fazer isso implantando os [cluster do Windows Server 2016 armazenamento SOFS dos espaços diretos modelo do Azure](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) ou seguindo as etapas 6 a 11 para implantar manualmente.
6. Para configurar manualmente os nós de cluster de servidor de arquivos:
   1. Crie o primeiro nó: 
      1. Crie uma nova máquina virtual usando a imagem do Windows Server 2016. (Clique **Novo > máquinas virtuais > Windows Server 2016.** Selecione **Gerenciador de recursos**e, em seguida, clique em **criar**.)
      2. Defina a configuração básica da seguinte maneira:
         - Name: my-fsn1
         - Tipo de disco VM SSD
         - Use um grupo de recursos existente, aquele que você criou na etapa 3. 
      3. Tamanho: DS1, DS2, DS3, DS4 ou DS5 dependendo do seu usuário precisa (consulte a tabela no início destas instruções). Certifique-se de que o suporte de disco premium está selecionado.
      4. Configurações: 
         - Conta de armazenamento: Escolha a conta de armazenamento que você criou na etapa 3.
         - Alta disponibilidade - criar um novo conjunto de disponibilidade. (Clique em **alta disponibilidade > Criar novo**e, em seguida, insira um nome (por exemplo, s2d-cluster). Use os valores padrão para **domínios de atualização** e **domínios de falha**.)
   2. Crie o segundo nó. Repita a etapa acima, com as seguintes alterações:
      - Name: my-fsn2
      - Alta disponibilidade - selecione que o conjunto de disponibilidade é criado acima.  
7. [Anexar discos de dados](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) para as VMs do nó de cluster acordo com seu usuário precisa (como visto na tabela acima). Após os dados de discos são criados e anexados à VM, defina **cache de host** à **None**.
8. Definir endereços IP para todas as VMs para **estático**. 
   1. No grupo de recursos, selecione uma VM e, em seguida, clique em **interfaces de rede** (sob **configurações**). Selecione o adaptador de rede listada e, em seguida, clique em **configurações de IP**. Selecione a configuração de IP listada, selecione **estáticos**e, em seguida, clique em **salvar**.
   2. Observe o domínio dc (controlador my-em nosso exemplo) endereço IP privado (10.x.x.x).
9. Defina o endereço do servidor DNS primário em NICs de VMs do nó de cluster para o servidor de Meu controlador de domínio. Selecione a VM e, em seguida, clique em **Interfaces de rede > servidores DNS > DNS personalizado**. Insira o endereço IP privado que você anotou acima e, em seguida, clique em **salvar**.
10. Criar uma [conta de armazenamento do Azure como a testemunha de nuvem](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Se você usar instruções vinculadas, pare quando chegar à "Configuração de nuvem testemunha com Failover Cluster GUI do Gerenciador de" – Vamos fazer essa etapa abaixo.)
11. Configure o servidor de arquivos de espaços de armazenamento diretos. Conectar-se a uma VM de nó e, em seguida, execute os seguintes cmdlets do Windows PowerShell.
    1. Instale o recurso de cluster de Failover e o recurso de servidor de arquivos no nó de cluster de servidor de duas arquivo VMs:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Validar VMs do nó de cluster e criar o cluster SOFS de 2 nós:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configure a testemunha de nuvem. Use sua nuvem testemunha conta acesso e o nome da chave de armazenamento.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Habilite os espaços de armazenamento diretos.

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
   
    6. Crie o servidor de arquivos de escalabilidade horizontal (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Crie um novo compartilhamento de arquivos SMB no cluster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

Agora você tem um compartilhamento em `\\my-sofs1\UpdStorage`, que pode ser usado para armazenamento UPD quando você [habilitar UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) para seus usuários. 
