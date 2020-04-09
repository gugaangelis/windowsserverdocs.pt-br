---
title: Visão geral de Namespaces DFS
ms.prod: windows-server
ms.author: jgerend
manager: daveba
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 06/07/2019
description: Este tópico descreve o namespaces do DFS, que é um serviço de função no Windows Serve que permite que você agrupe pastas compartilhadas localizadas em diferentes servidores em um ou mais namespaces estruturados logicamente.
ms.openlocfilehash: 07f6ac857164257810b297f9e2b83db4e4bd42be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858979"
---
# <a name="dfs-namespaces-overview"></a>Visão geral de Namespaces DFS

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semestral)

O Namespaces do DFS é um serviço de função no Windows Serve que permite que você agrupe pastas compartilhadas localizadas em diferentes servidores em um ou mais namespaces estruturados logicamente. Isso torna possível dar aos usuários uma exibição virtual de pastas compartilhadas, onde um único caminho leva a arquivos localizados em vários servidores, conforme mostrado na figura a seguir:

![Elementos da tecnologia de namespaces DFS](media/dfs-overview.png)

Aqui está uma descrição dos elementos que compõem um namespace DFS:

- **Servidor de Namespace** - Um servidor de namespace hospeda um namespace. O servidor de namespace pode ser um servidor membro ou um controlador de domínio.
- **Namespace raiz** - O namespace raiz é o ponto inicial do namespace. Na figura anterior, o nome da raiz é público e o caminho do namespace é \\\\contoso\\Public. Esse tipo de namespace é um namespace baseado em domínio, pois ele começa com um nome de domínio (por exemplo, contoso) e seus metadados são armazenados em Active Directory Domain Services (AD DS). Embora um servidor de namespace é mostrado na figura anterior, um namespace baseado em domínio pode ser hospedado em vários servidores de namespace para aumentar a disponibilidade do namespace.
- **Pasta** - pastas sem destinos de pasta adicionam estrutura e hierarquia ao namespace, e pastas com destinos de pasta fornecem aos usuários com conteúdo em si. Quando os usuários pesquisam uma pasta com destinos de pasta no namespace, o computador cliente recebe uma referência que redireciona transparentemente o computador cliente para um dos destinos de pasta.
- **Destino de pasta** - Um destino de pasta é o caminho UNC de uma pasta compartilhada ou outro namespace associado a uma pasta em um namespace. A pasta destino é onde os dados e conteúdo está armazenado. Na figura anterior, a pasta chamada Tools tem dois destinos de pasta, um em Londres e outro em Nova York, e a pasta denominada guias de treinamento tem um destino de pasta única em são Paulo. Um usuário que procura \\\\contoso\\Public\\software\\ferramentas é Redirecionado de forma transparente para a pasta compartilhada \\\\LDN-SVR-01\\Tools ou \\\\NYC-SVR-01\\Tools, dependendo de qual site o usuário está localizado no momento.

Este tópico discute como instalar o DFS, o que há de novo e onde encontrar informações de avaliação e implantação.

Você pode administrar namespaces usando Gerenciamento DFS, o [Cmdlets de Namespace de DFS (DFSN) no Windows PowerShell](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps), o **DfsUtil** comando ou scripts que chamam o WMI.

## <a name="server-requirements-and-limits"></a>Requisitos e limites de servidor

Não há requisitos de hardware ou software adicionais para executar o Gerenciamento do DFS ou usar Namespaces do DFS.

Um servidor de namespace é um controlador de domínio ou servidor membro que hospeda um namespace. O número de namespaces que você pode hospedar em um servidor é determinado pelo sistema operacional em execução no servidor do namespace.

Servidores que executam os seguintes sistemas operacionais podem hospedar vários namespaces baseados em domínio, além de um único namespace autônomo. 

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter e Enterprise Editions
- Windows Server (canal semestral)

Servidores que executam os seguintes sistemas operacionais podem hospedar um único namespace autônomo:

- Windows Server 2008 R2 Standard

A tabela a seguir descreve os fatores adicionais a serem considerados ao escolher servidores para hospedar um namespace.

| Servidor que hospeda Namespaces autônomos | Servidor que hospeda Namespaces baseados em domínio |
| ---                                   |        ---                                |
| Deve conter um volume NTFS para hospedar o namespace.|Deve conter um volume NTFS para hospedar o namespace. |
| Pode ser um servidor membro ou controlador de domínio.|Deve ser um servidor membro ou controlador de domínio no domínio em que o namespace é configurado. (Esse requisito se aplica a todos os servidores de namespace que hospeda um determinado namespace baseado em domínio). |
| Pode ser hospedado por um cluster de failover para aumentar a disponibilidade do namespace.|O namespace não pode ser um recurso de cluster em um cluster de failover. No entanto, você pode localizar o namespace em um servidor que também funciona como um nó em um cluster de failover, se você definir o namespace para usar apenas recursos locais no servidor. |

## <a name="installing-dfs-namespaces"></a>Instalando os Namespaces de DFS

Os Namespaces e a Replicação do DFS são parte da função Serviços de Arquivo e Armazenamento. As ferramentas de gerenciamento de DFS (Gerenciamento de DFS, módulo de Namespaces do DFS para Windows PowerShell e ferramentas de linha de comando) são instalados separadamente como parte das Ferramentas de Administração de Servidor Remoto.

Instale os namespaces do DFS usando o [centro de administração do Windows](../../manage/windows-admin-center/understand/windows-admin-center.md), Gerenciador do servidor ou PowerShell, conforme descrito nas próximas seções.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS usando o Gerenciador do Servidor

1. Abra o Gerenciador do Servidor, clique em **Gerenciar** e em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.

2. Na página **Seleção de Servidor**, selecione o servidor no VHD (disco rígido virtual) de uma máquina virtual offline na qual deseja instalar o DFS.

3. Selecione os serviços de função e os recursos que deseja instalar.

    - Para instalar o serviço de Namespaces de DFS, o **funções de servidor** página, selecione **Namespaces DFS**.

    - Para instalar apenas as Ferramentas de Gerenciamento de DFS, na página **Recursos**, expanda **Ferramentas de Administração do Servidor Remoto**, **Ferramentas de Administração de Funções**, expanda **Ferramentas de Serviços de Arquivo** e selecione **Ferramentas de Gerenciamento de DFS**.

         **Ferramentas de Gerenciamento de DFS** instala o snap-in Gerenciamento de DFS, o módulo de Namespaces do DFS para Windows PowerShell e as ferramentas de linha de comando, mas não instala servidos de DFS no servidor.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Para instalar DFS usando o Windows PowerShell

Abra uma sessão do Windows PowerShell com direitos de usuário elevados e digite o seguinte comando, em que <name\> é o serviço de função ou recurso que você deseja instalar (consulte a tabela abaixo para ver uma lista de nomes de serviço de função ou recurso relevantes):

```PowerShell
Install-WindowsFeature <name>
```

| Serviço de função ou recurso | {1&gt;Nome&lt;1} |
| ----------------------- | ---- |
| Namespaces DFS          | `FS-DFS-Namespace` |
| Ferramentas de Gerenciamento de DFS    | `RSAT-DFS-Mgmt-Con` |

Por exemplo, para instalar a parte das Ferramentas do Sistema de Arquivos Distribuído do recurso Ferramentas de Administração do Servidor Remoto, digite:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Para instalar as partes de Namespaces do DFS e Ferramentas do Sistema de Arquivos Distribuído do recurso Ferramentas de Administração do Servidor Remoto, digite:

```PowerShell
Install-WindowsFeature "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidade com máquinas virtuais do Azure

O uso de Namespaces em uma máquina virtual no Microsoft Azure foi testado; no entanto, há algumas limitações e requisitos que devem ser seguidos.

- Não é possível clusterizar namespaces autônomos em máquinas virtuais do Azure.

- Você pode hospedar Namespaces baseados em domínio em máquinas virtuais do Azure, incluindo ambientes com Azure Active Directory.

Para saber mais sobre como começar a usar máquinas virtuais do Windows Azure, consulte [Documentação de máquinas virtuais do Windows Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="see-also"></a>Consulte também

Para obter informações adicionais relacionadas, consulte os seguintes recursos.

| Tipo de conteúdo        | Referências |
| ------------------  | ----------------|
| **Avaliação do produto** | [O que há de novo nos namespaces do DFS e Replicação do DFS no Windows Server](https://technet.microsoft.com/library/dn281957(v=ws.11).aspx) |
| **Implantação**    | [Considerações sobre escalabilidade de namespace do DFS](https://blogs.technet.com/b/filecab/archive/2012/08/26/dfs-namespace-scalability-considerations.aspx) |
| **Operações**    | [Namespaces do DFS: perguntas frequentes](https://technet.microsoft.com/library/ee404780.aspx) |
| **Recursos da comunidade** | [O fórum serviços de arquivos e armazenamento do TechNet](https://social.technet.microsoft.com/forums/winserverfiles/threads/) |
| **Protocolos**        | [Protocolos de serviços de arquivos no Windows Server](https://msdn.microsoft.com/library/cc239318.aspx) (preterido) |
| **Tecnologias relacionadas** | [Clustering de failover](../../failover-clustering/failover-clustering-overview.md)|
| **Suporte** | [Suporte do Windows IT Pro](https://www.microsoft.com/itpro/windows/support)|
