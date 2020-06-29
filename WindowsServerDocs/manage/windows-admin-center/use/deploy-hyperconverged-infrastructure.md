---
title: Implantar a infraestrutura de hiperconvergente com o centro de administração do Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: 088fb7b8f03ab7e575b562572f2e29e1b5774760
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474483"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Implantar a infraestrutura de hiperconvergente com o centro de administração do Windows

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Você pode usar o centro de administração do Windows [versão 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) ou posterior para implantar a infraestrutura de hiperconvergente usando dois ou mais servidores Windows adequados. Esse novo recurso assume a forma de um fluxo de trabalho de vários estágios que orienta você na instalação de recursos, na configuração de rede, na criação do cluster e na implantação de Espaços de Armazenamento Diretos e/ou SDN (rede definida pelo software), se selecionado.

![Captura de tela do fluxo de trabalho de criação do cluster](../media/deploy-hyperconverged-infrastructure/deployment-workflow-screenshot.png)

  > [!Important]
  > Esse recurso está em desenvolvimento ativo. Ele está disponível na versão prévia para que você possa experimentá-lo no início e compartilhar seus comentários.

## <a name="preview-the-workflow"></a>Visualizar o fluxo de trabalho

### <a name="1-prerequisites"></a>1. Pré-requisitos

O fluxo de trabalho de criação de cluster no centro de administração do Windows não executa a instalação do sistema operacional bare-metal, portanto, você precisa instalar o Windows Server em cada servidor primeiro. As versões com suporte são o Windows Server 2016, o Windows Server 2019 e o Windows Server Insider Preview. Você também precisa ingressar cada servidor no mesmo domínio Active Directory em que o centro de administração do Windows está em execução antes de iniciar o fluxo de trabalho.

### <a name="2-install-windows-admin-center"></a>2. instalar o centro de administração do Windows

Siga as instruções para [baixar e instalar](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) a versão mais recente do centro de administração do Windows.

### <a name="3-install-the-cluster-creation-extension"></a>3. instalar a extensão de criação de cluster

O fluxo de trabalho de criação de cluster é entregue e atualizado por meio do feed de extensão do centro de administração do Windows. Isso é para que possamos solucionar seus comentários e tornar os aprimoramentos mais rapidamente durante a fase de visualização.

Para instalar a extensão, inicie o centro de administração do Windows e, em seguida:

1.  No canto superior direito, selecione o ícone de configurações
2.  Navegar até extensões
3.  Localizar a extensão de criação de cluster
4.  Selecione instalar

![Capturas de tela das quatro etapas para instalar a extensão de criação de cluster](../media/deploy-hyperconverged-infrastructure/how-to-install.png)

Depois de instalado, selecione a extensão de criação de cluster na lista suspensa no canto superior esquerdo:

![Captura de tela da inicialização da extensão de criação de cluster](../media/deploy-hyperconverged-infrastructure/how-to-launch.png)

## <a name="limitations-of-the-preview-release"></a>Limitações da versão de visualização

O fluxo de trabalho de criação de cluster está em desenvolvimento ativo – em outras palavras, ele não é concluído.

Aqui estão algumas limitações da versão de visualização atual:

1.  **Requisitos de domínio.** Atualmente, o fluxo de trabalho requer que todos os servidores que você adicionar já tenham ingressado no mesmo domínio Active Directory em que o centro de administração do Windows está em execução.
2.  **Mostrar script.** Com a versão de visualização atual, você não pode ver os scripts que o fluxo de trabalho executa usando a funcionalidade Mostrar script no centro de administração do Windows, nem pode baixar um relatório completo de tudo o que aconteceu no final. Sabemos que isso é importante para muitos usuários – fique atento. Enquanto isso, use os cmdlets fornecidos abaixo para [ver o que está acontecendo](#see-whats-happening).
3.  **Retomar ou recomeçar.** Embora você possa sair do MSRC durante por meio do fluxo de trabalho, a versão de visualização atual não lida com a retomada de onde você parou e pode limpar completamente antes de iniciar novamente. Se você quiser implantar repetidamente nos mesmos servidores para teste, use os cmdlets fornecidos abaixo para [desfazer e reiniciar](#undo-and-start-over).
4.  **Acesso remoto direto à memória (RDMA).** A versão de visualização atual não pode habilitar a rede RDMA, que é comumente usada com a infraestrutura hiperconvergente. Por enquanto, conclua o fluxo de trabalho sem habilitar o RDMA e, em seguida, use outras ferramentas para habilitar o RDMA da mesma forma que faria.

Além de abordar essas limitações, há inúmeros recursos potenciais que poderiam ser adicionados em versões futuras: aplicando atualizações do Windows, configurando uma testemunha para quorum de cluster, importando máquinas virtuais e assim por diante. Para nos ajudar a priorizar os recursos desejados, [Compartilhe comentários](#feedback) conforme descrito abaixo.

## <a name="see-whats-happening"></a>Veja o que está acontecendo

Use estes cmdlets do Windows PowerShell para acompanhar e ver o que o fluxo de trabalho está fazendo.

Para ver quais recursos do Windows estão instalados, use o `Get-WindowsFeature` cmdlet. Por exemplo:

```PowerShell
Get-WindowsFeature "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "BitLocker"
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-1.png)

  > [!Note]
  > Quais recursos são instalados depende do tipo de cluster selecionado.

Para ver os adaptadores de rede e suas propriedades, como nome, endereços IPv4 e ID de VLAN:

```PowerShell
Get-NetAdapter | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
Get-NetAdapter | Where Status -Eq "Up" | Get-NetIPAddress -AddressFamily IPv4 -ErrorAction SilentlyContinue | Sort InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-2.png)

Para ver os comutadores virtuais do Hyper-V e como os adaptadores de rede física são agrupados:

```PowerShell
Get-VMSwitch
Get-VMSwitchTeam
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-3.png)

Para ver os adaptadores de rede virtual do host, se houver, execute:

```PowerShell
Get-VMNetworkAdapter -ManagementOS | Format-Table Name, IsManagementOS, SwitchName
Get-VMNetworkAdapterTeamMapping -ManagementOS | Format-Table NetAdapterName, ParentAdapter
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-4.png)

Para ver o cluster de failover atual e seus nós de membro, execute:

```PowerShell
Get-Cluster
Get-ClusterNode
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-5.png)

Para ver se Espaços de Armazenamento Diretos está habilitado e o pool de armazenamento padrão que é criado automaticamente:

```PowerShell
Get-ClusterStorageSpacesDirect
Get-StoragePool -IsPrimordial $False
```

![Captura de tela da saída do PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-6.png)

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

## <a name="feedback"></a>Comentários

Esta versão de visualização é sobre seus comentários. Aqui estão algumas maneiras que você pode acessar nossa equipe:

- [Enviar e votar em solicitações de recursos no UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Participe do fórum do centro de administração do Windows na Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Email HCI-implantação [em] microsoft.com
- Tweet para[@servermgmt](https://twitter.com/servermgmt)

## <a name="report-an-issue"></a>Relatar um problema

Use os canais listados acima para relatar um problema com o fluxo de trabalho de criação do cluster.

Se possível, inclua as seguintes informações para nos ajudar a reproduzir e resolver rapidamente o problema:

- Tipo de cluster selecionado (exemplo: *"hiperconvergente"*)
- Etapa em que você encontrou o problema (exemplo: *"3,2 criar cluster"*)
- Versão da extensão de criação de cluster. Vá para **configurações**  >  **extensões**versões  >  **instaladas** e veja a coluna **versão** (exemplo: *"1.0.30"*).
- Mensagens de erro, se na tela ou no console do navegador, que você pode abrir pressionando **F12**.
- Quaisquer outras informações relevantes sobre seu ambiente

## <a name="additional-references"></a>Referências adicionais

- [Olá, centro de administração do Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Implantar Espaços de Armazenamento Diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
