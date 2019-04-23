---
Title: Visão geral da replicação DFS
ms.date: 03/08/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 17fa97e28d099806c9280e42dd900e8d6c708641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850237"
---
# <a name="dfs-replication-overview"></a>Visão geral da replicação DFS

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semestral)

A replicação DFS é um serviço de função no Windows Server que permite que você replique pastas (incluindo aquelas referenciadas por um caminho de namespace do DFS) com eficiência em vários servidores e sites. A Replicação DFS é um mecanismo de replicação eficiente de mestre múltiplo, que você pode usar para manter pastas sincronizadas entre servidores através das conexões de rede de largura de banda limitada. Ele substitui a replicação FRS (serviço) como o mecanismo de replicação para Namespaces do DFS, bem como para replicar a pasta SYSVOL de serviços de domínio Active Directory (AD DS) em domínios que usam o Windows Server 2008 ou posterior nível funcional do domínio.

A Replicação DFS usa um algoritmo de compactação conhecido como RDC (compactação diferencial remota). O RDC detecta alterações nos dados de um arquivo e permite que a Replicação DFS replique somente os blocos de arquivo alterados, em vez do arquivo inteiro.

Para obter mais informações sobre a replicação de SYSVOL usando a replicação do DFS, consulte [migrar a replicação do SYSVOL para replicação do DFS](migrate-sysvol-to-dfsr.md).

Para usar a replicação do DFS, você deve criar grupos de replicação e adicionar pastas replicadas para os grupos. Grupos de replicação, as pastas replicadas e membros são ilustrados na figura a seguir.

![Um grupo de replicação que contém uma conexão entre os dois membros, cada um com duas pastas replicadas](media\dfsr-overview.gif)

Esta figura mostra que um grupo de replicação é um conjunto de servidores, conhecidos como membros, que participa da replicação de uma ou mais pastas replicadas. Uma pasta replicada é uma pasta que permanece sincronizada em cada membro. Na figura, há duas pastas replicadas: Os projetos e propostas. Como os dados são alterados em cada pasta replicada, as alterações são replicadas em conexões entre os membros do grupo de replicação. As conexões entre todos os membros formam a topologia de replicação.
Criar várias pastas replicadas em um grupo de replicação único simplifica o processo de implantação de pastas replicadas porque a topologia, o agendamento e largura de banda para o grupo de replicação são aplicadas a cada pasta replicada. Para implantar pastas replicadas adicionais, você pode usar Dfsradmin.exe ou um siga as instruções em um Assistente para definir o caminho local e permissões para a nova pasta replicada.

Cada pasta replicada tem configurações exclusivas, como filtros de arquivo e subpasta, para que você pode filtrar os diferentes arquivos e subpastas para cada pasta replicada.

As pastas replicadas armazenadas em cada membro podem ser localizadas em diferentes volumes no membro e as pastas replicadas não precisa ser parte de um namespace ou de pastas compartilhadas. No entanto, o snap-in Gerenciamento DFS torna mais fácil compartilhar pastas replicadas e, opcionalmente, publique-os em um namespace existente.

Você pode administrar a replicação do DFS usando o Gerenciamento DFS, o DfsrAdmin e Dfsrdiag comandos ou scripts que chamam o WMI.

## <a name="requirements"></a>Requisitos

Antes de implantar a Replicação do DFS, você deve configurar os servidores da seguinte forma:

- Atualize o esquema de serviços de domínio Active Directory (AD DS) para incluir o Windows Server 2003 R2 ou posterior adições de esquema. Você não pode usar pastas replicadas somente leitura com o Windows Server 2003 R2 ou adições de esquema mais antigas.
- Verifique se todos os servidores em um grupo de replicação estão localizados na mesma floresta. Você não pode habilitar a replicação entre servidores de florestas diferentes.
- Instale a Replicação do DFS em todos os servidores que atuarão como membros de um grupo de replicação.
- Entre em contato com o fornecedor do software antivírus para verificar se o software antivírus é compatível com a Replicação do DFS.
- Localize todas as pastas que deseja replicar em volumes formatados com o sistema de arquivos NTFS. A Replicação do DFS não oferece suporte ao Sistema de Arquivos Resiliente (ReFS) ou FAT. A Replicação do DFS também não dá suporte para replicar conteúdo armazenado em Volumes Compartilhados Clusterizados.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidade com máquinas virtuais do Azure

Usando a replicação do DFS em uma máquina virtual no Azure foi testado com o Windows Server. No entanto, há algumas limitações e requisitos que devem ser seguidas.

- O uso de instantâneos ou estados salvos para restaurar um servidor que executa a Replicação do DFS para replicar qualquer outro item diferente da pasta SYSVOL provoca falha na Replicação do DFS, o que exige etapas especiais para a recuperação do banco de dados. Da mesma forma, não exporte, clone ou copie as máquinas virtuais. Para obter mais informações, consulte o artigo [2517913](http://support.microsoft.com/kb/2517913) da Base de Dados de Conhecimento Microsoft, bem como o artigo [Virtualização segura da DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Ao fazer backup de dados em uma pasta replicada hospedada em uma máquina virtual, você deve usar o software de backup de dentro da máquina virtual convidada.
- Replicação do DFS requer acesso aos controladores de domínio virtualizado ou físico – ela não pode se comunicar diretamente com o Azure AD.
- A Replicação do DFS requer uma conexão VPN entre os membros do grupo de replicação local em e quaisquer membros hospedados em máquinas virtuais do Azure. Você também precisa configurar o roteador local (por exemplo, o Forefront Threat Management Gateway) para permitir que o mapeador de ponto de extremidade RPC (porta 135) e uma porta atribuída aleatoriamente entre 49152 e 65535 passem pela conexão VPN. Você pode usar o cmdlet Set-DfsrMachineConfiguration ou a ferramenta de linha de comando Dfsrdiag para especificar uma porta estática em vez da porta aleatória. Para obter mais informações sobre como especificar uma porta estática para a Replicação do DFS, consulte [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Para obter informações sobre portas relacionadas a serem abertas para gerenciamento do Windows Server, consulte o artigo [832017](http://support.microsoft.com/kb/832017) na Base de Dados de Conhecimento da Microsoft.

Para saber mais sobre como começar a usar máquinas virtuais do Azure, visite o [site do Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Instalando a replicação do DFS

A replicação DFS é uma parte da função Serviços de arquivo e armazenamento. Ferramentas de gerenciamento de DFS (gerenciamento de DFS, o módulo de replicação do DFS para Windows PowerShell e ferramentas de linha de comando) são instaladas separadamente como parte das ferramentas de administração de servidor remoto.

Instalar a replicação do DFS usando [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md), Gerenciador de servidores ou o PowerShell, conforme descrito nas próximas seções.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS usando o Gerenciador do Servidor

1. Abra o Gerenciador do Servidor, clique em **Gerenciar** e em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.

2. Na página **Seleção de Servidor**, selecione o servidor no VHD (disco rígido virtual) de uma máquina virtual offline na qual deseja instalar o DFS.

3. Selecione os serviços de função e os recursos que deseja instalar.

    - Para instalar o serviço de replicação do DFS, o **funções de servidor** página, selecione **replicação do DFS**.

    - Para instalar apenas as Ferramentas de Gerenciamento de DFS, na página **Recursos**, expanda **Ferramentas de Administração do Servidor Remoto**, **Ferramentas de Administração de Funções**, expanda **Ferramentas de Serviços de Arquivo** e selecione **Ferramentas de Gerenciamento de DFS**.

         **Ferramentas de gerenciamento de DFS** instala o Gerenciamento DFS snap-in, a replicação DFS e módulos de Namespaces do DFS para Windows PowerShell e ferramentas de linha de comando, mas não instala servidos de DFS no servidor.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Para instalar a replicação do DFS usando o Windows PowerShell

Abra uma sessão do Windows PowerShell com direitos de usuário elevados e, em seguida, digite o seguinte comando, onde < nome\> é o serviço de função ou recurso que você deseja instalar (consulte a tabela a seguir para obter uma lista dos nomes de serviço ou recurso de função relevantes):

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

Para instalar a replicação DFS e as partes de ferramentas do sistema de arquivos distribuído do recurso ferramentas de administração de servidor remoto, digite:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Consulte também

- [Visão geral de Namespaces do DFS e replicação do DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Lista de verificação: Implantar a replicação DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Lista de verificação: Gerenciar a replicação DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Implantação da replicação DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Gerenciando a replicação do DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Solução de problemas de replicação do DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
