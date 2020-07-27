---
title: Visão geral sobre a Replicação do DFS
ms.date: 03/08/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 380c8ba0e91db47c3313542b2d294a516f7a9466
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961178"
---
# <a name="dfs-replication-overview"></a>Visão geral sobre a Replicação do DFS

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (Canal Semestral)

A Replicação do DFS é um serviço de função no Windows Server que permite que você replique pastas com eficiência (incluindo aquelas referenciadas por um caminho da namespace do DFS) em vários servidores e sites. A Replicação DFS é um mecanismo de replicação eficiente de mestre múltiplo, que você pode usar para manter pastas sincronizadas entre servidores através das conexões de rede de largura de banda limitada. Ele substitui o FRS (Serviço de Replicação de Arquivo) como o mecanismo de replicação para namespaces DFS, bem como para replicar a pasta SYSVOL do AD DS (Active Directory Domain Services) em domínios que usam o nível funcional de domínio do Windows Server 2008 ou posterior.

A Replicação DFS usa um algoritmo de compactação conhecido como RDC (compactação diferencial remota). O RDC detecta alterações nos dados de um arquivo e permite que a Replicação DFS replique somente os blocos de arquivo alterados, em vez do arquivo inteiro.

Para obter mais informações sobre como replicar o SYSVOL usando a Replicação do DFS, confira o [Migrar a replicação SYSVOL para Replicação do DFS](migrate-sysvol-to-dfsr.md).

Para usar Replicação do DFS, crie grupos de replicação e adicione pastas replicadas aos grupos. Os grupos de replicação, as pastas replicadas e os membros são ilustrados na figura a seguir.

![Um grupo de replicação que contém uma conexão entre dois membros, cada um com duas pastas replicadas](media/dfsr-overview.gif)

Esta figura mostra que um grupo de replicação é um conjunto de servidores, conhecidos como membros, que participam da replicação de uma ou mais pastas replicadas. Uma pasta replicada é a que permanece sincronizada em cada membro. Na figura, há duas pastas replicadas: Projetos e Propostas. À medida que os dados são alterados em cada pasta replicada, as alterações são replicadas nas conexões entre os membros do grupo de replicação. As conexões entre todos os membros formam a topologia de replicação.
A criação de várias pastas replicadas em um grupo de replicação simplifica o processo de implantação de pastas replicadas, pois a topologia, a agenda e a limitação de largura de banda do grupo de replicação são aplicadas a cada pasta replicada. Para implantar pastas replicadas adicionais, você pode usar o Dfsradmin.exe ou seguir as instruções em um assistente para definir o caminho local e as permissões para a nova pasta replicada.

Cada pasta replicada tem configurações exclusivas, como filtros de arquivo e subpasta, para que você possa filtrar arquivos e subpastas diferentes para cada pasta replicada.

As pastas replicadas armazenadas em cada membro podem estar localizadas em volumes diferentes no membro e elas não precisam ser pastas compartilhadas nem fazer parte de um namespace. No entanto, o snap-in de Gerenciamento do DFS facilita o compartilhamento de pastas replicadas e, opcionalmente, a publicação em um namespace existente.

Você pode administrar a Replicação do DFS usando o Gerenciamento DFS, os comandos DfsrAdmin e Dfsrdiag ou scripts que chamam o WMI.

## <a name="requirements"></a>Requisitos

Antes de implantar a Replicação do DFS, você deve configurar os servidores da seguinte forma:

- Atualizar o esquema do AD DS (Active Directory Domain Services) para incluir adições de esquema do Windows Server 2003 R2 ou posteriores. Não é possível usar pastas replicadas somente leitura com as adições de esquema do Windows Server 2003 R2 ou anterior.
- Verifique se todos os servidores em um grupo de replicação estão localizados na mesma floresta. Você não pode habilitar a replicação entre servidores de florestas diferentes.
- Instale a Replicação do DFS em todos os servidores que atuarão como membros de um grupo de replicação.
- Entre em contato com o fornecedor do software antivírus para verificar se o software antivírus é compatível com a Replicação do DFS.
- Localize todas as pastas que deseja replicar em volumes formatados com o sistema de arquivos NTFS. A Replicação do DFS não oferece suporte ao Sistema de Arquivos Resiliente (ReFS) ou FAT. A Replicação do DFS também não dá suporte para replicar conteúdo armazenado em Volumes Compartilhados Clusterizados.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidade com máquinas virtuais do Azure

O uso de Replicação do DFS em uma máquina virtual no Azure foi testado com o Windows Server; no entanto, há algumas limitações e requisitos que devem ser seguidos.

- O uso de instantâneos ou estados salvos para restaurar um servidor que executa a Replicação do DFS para replicar qualquer outro item diferente da pasta SYSVOL provoca falha na Replicação do DFS, o que exige etapas especiais para a recuperação do banco de dados. Da mesma forma, não exporte, clone ou copie as máquinas virtuais. Para obter mais informações, consulte o artigo [2517913](https://support.microsoft.com/kb/2517913) da Base de Dados de Conhecimento Microsoft, bem como o artigo [Virtualização segura da DFSR](https://techcommunity.microsoft.com/t5/storage-at-microsoft/safely-virtualizing-dfsr/ba-p/424671).
- Ao fazer backup de dados em uma pasta replicada hospedada em uma máquina virtual, use o software de backup na máquina virtual convidada.
- A Replicação do DFS requer acesso aos controladores de domínio virtualizado ou físico – ela não pode se comunicar diretamente com o Azure AD.
- A Replicação do DFS exige uma conexão VPN entre os membros do grupo de replicação local e os membros hospedados em VMs do Azure. Você também precisa configurar o roteador local (por exemplo, o Forefront Threat Management Gateway) para permitir que o mapeador de pontos de extremidade RPC (porta 135) e uma porta atribuída aleatoriamente entre 49152 e 65535 passem pela conexão VPN. Você pode usar o cmdlet Set-DfsrMachineConfiguration ou a ferramenta de linha de comando Dfsrdiag para especificar uma porta estática em vez da porta aleatória. Para obter mais informações sobre como especificar uma porta estática para a Replicação do DFS, consulte [Set-DfsrServiceConfiguration](/powershell/module/dfsr/set-dfsrserviceconfiguration). Para obter informações sobre portas relacionadas a serem abertas para gerenciamento do Windows Server, consulte o artigo [832017](https://support.microsoft.com/kb/832017) na Base de Dados de Conhecimento da Microsoft.

Para saber mais sobre como começar a usar máquinas virtuais do Azure, visite o [site do Microsoft Azure](/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Como instalar a Replicação do DFS

A Replicação do DFS faz parte da função Serviços de Arquivo e Armazenamento. As ferramentas de gerenciamento de DFS (Gerenciamento de DFS, módulo de Replicação do DFS para Windows PowerShell e ferramentas de linha de comando) são instalados separadamente como parte das Ferramentas de Administração de Servidor Remoto.

Instale a Replicação do DFS usando o [Windows Admin Center](../../manage/windows-admin-center/overview.md), o Gerenciador do Servidor ou o PowerShell, conforme descrito nas seções a seguir.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS usando o Gerenciador do Servidor

1. Abra o Gerenciador do Servidor, clique em **Gerenciar**e em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.

2. Na página **Seleção de Servidor** , selecione o servidor no VHD (disco rígido virtual) de uma máquina virtual offline na qual deseja instalar o DFS.

3. Selecione os serviços de função e os recursos que deseja instalar.

    - Para instalar o serviço de Replicação do DFS, na página **Funções de Servidor**, selecione **Replicação do DFS**.

    - Para instalar apenas as Ferramentas de Gerenciamento de DFS, na página **Recursos** , expanda **Ferramentas de Administração do Servidor Remoto**, **Ferramentas de Administração de Funções**, expanda **Ferramentas de Serviços de Arquivo**e selecione **Ferramentas de Gerenciamento de DFS**.

         **Ferramentas de Gerenciamento do DFS** instala o snap-in Gerenciamento de DFS, a Replicação do DFS e os módulos de Namespaces do DFS para Windows PowerShell e as ferramentas de linha de comando, mas não instala serviços de DFS no servidor.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Para instalar a Replicação do DFS usando o Windows PowerShell

Abra uma sessão do Windows PowerShell com direitos de usuário elevados e digite o seguinte comando, em que <name\> é o serviço de função ou recurso que você deseja instalar (consulte a tabela abaixo para ver uma lista de nomes de serviço de função ou recurso relevantes):

```PowerShell
Install-WindowsFeature <name>
```

|Serviço de função ou recurso|Nome|
|---|---|
|Replicação do DFS|`FS-DFS-Replication`|
|Ferramentas de Gerenciamento de DFS|`RSAT-DFS-Mgmt-Con`|

Por exemplo, para instalar a parte das Ferramentas do Sistema de Arquivos Distribuído do recurso Ferramentas de Administração do Servidor Remoto, digite:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Para instalar as partes de Replicação do DFS e Ferramentas do Sistema de Arquivos Distribuído do recurso Ferramentas de Administração de Servidor Remoto, digite:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="additional-references"></a>Referências adicionais

- [Visão geral dos namespaces e da Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127250(v%3dws.11))
- [Lista de verificação: Implantar a Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772201(v%3dws.11))
- [Lista de verificação: Gerenciar a Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755035(v%3dws.11))
- [Como implantar a Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770925(v%3dws.11))
- [Como gerenciar a Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770925(v%3dws.11))
- [Solucionar problemas Replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732802(v%3dws.11))
