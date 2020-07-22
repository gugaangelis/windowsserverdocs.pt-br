---
title: Implantar a infraestrutura de hiperconvergente com o centro de administração do Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: f0f3e313124dd10cd508bf11853969cc67de6368
ms.sourcegitcommit: b35fbd2a67d7a3395b50b2a3acd0817ba4e36b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86891361"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Implantar a infraestrutura de hiperconvergente com o centro de administração do Windows

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Você pode usar o centro de administração do Windows [versão 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) ou posterior para implantar a infraestrutura de hiperconvergente usando dois ou mais servidores Windows adequados. Esse novo recurso assume a forma de um fluxo de trabalho de vários estágios que orienta você na instalação de recursos, na configuração de rede, na criação do cluster e na implantação de Espaços de Armazenamento Diretos e/ou SDN (rede definida pelo software), se selecionado.

A partir da versão 2007 do centro de administração do Windows, o centro de administração do Windows dá suporte ao sistema operacional Azure Stack do HCI. Leia sobre [como implantar um cluster no centro de administração do Windows no Azure Stack documentos do HCI](https://docs.microsoft.com/azure-stack/hci/getting-started). Pense que esta documentação está concentrada no Azure Stack HCI, as instruções também são adequadas para implantações do Windows Server. 

## <a name="undo-and-start-over"></a>Desfazer e reiniciar

Use estes cmdlets do Windows PowerShell para desfazer as alterações feitas pelo fluxo de trabalho e recomeçar.

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>Remover máquinas virtuais ou outros recursos clusterizados

Se você tiver criado máquinas virtuais ou outros recursos clusterizados, como os controladores de rede para a rede definida pelo software, remova-os primeiro.

Por exemplo, para remover recursos por nome, use:

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>Desfazer as etapas de armazenamento

Se você tiver habilitado Espaços de Armazenamento Diretos, desabilite-o com este script:

> [!Warning]
> Esses cmdlets excluem permanentemente todos os dados em volumes Espaços de Armazenamento Diretos. Isso não pode ser desfeito.

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>Desfazer as etapas de clustering

Se você criou um cluster, remova-o com este cmdlet:

```PowerShell
Remove-Cluster -CleanUpAD
```

Para remover também os relatórios de validação de cluster, execute este cmdlet em todos os servidores que fazem parte do cluster:

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>Desfazer as etapas de rede

Execute esses cmdlets em todos os servidores que fazem parte do cluster.

Se você criou um comutador virtual Hyper-V:

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> O `Remove-VMSwitch` cmdlet Remove automaticamente todos os adaptadores virtuais e desfaz o agrupamento de adaptadores físicos inseridos na opção.

Se você tiver modificado as propriedades do adaptador de rede, como nome, endereço IPv4 e ID de VLAN:

> [!Warning]
> Esses cmdlets removem os nomes de adaptador de rede e os endereços IP. Verifique se você tem as informações necessárias para se conectar posteriormente, como um adaptador para o gerenciamento excluído do script abaixo. Além disso, certifique-se de que você sabe como os servidores estão conectados em termos de propriedades físicas, como o endereço MAC, não apenas o nome do adaptador no Windows.

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

Agora você está pronto para iniciar o fluxo de trabalho.

## <a name="additional-references"></a>Referências adicionais

- [Olá, centro de administração do Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Implantar Espaços de Armazenamento Diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
