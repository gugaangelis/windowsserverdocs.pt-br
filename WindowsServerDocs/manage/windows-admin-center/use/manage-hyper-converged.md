---
title: Gerenciar a infraestrutura Hiperconvergente com Windows Admin Center
description: Gerenciar a infraestrutura Hiperconvergente com Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fe00072932d9c7f283ebd887a5292ac9a9d0e37f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446035"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gerenciar a infraestrutura Hiperconvergente com Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

## <a name="what-is-hyper-converged-infrastructure"></a>O que é infraestrutura Hyper-Converged

Infraestrutura Hiperconvergente consolida definida pelo software de computação, armazenamento e rede em um cluster para oferecer alto desempenho, econômico e a virtualização facilmente escalável. Esse recurso foi introduzido no Windows Server 2016 com [espaços de armazenamento diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [rede definida pelo Software](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) e [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Buscando para adquirir infraestrutura Hyper-Converged? A Microsoft recomenda essas [definida pelo Software do Windows Server](https://microsoft.com/wssd) soluções de nossos parceiros. Eles são projetados, montados e validados em relação a nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, para que você começar a trabalhar em funcionamento rapidamente.

> [!IMPORTANT]
> Alguns dos recursos descritos neste artigo só estão disponíveis na versão prévia do Windows Admin Center. [Como posso obter essa versão?](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>O que é o Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) é a ferramenta de gerenciamento de última geração para o Windows Server, o sucessor do tradicionais "na caixa" ferramentas como o Gerenciador do servidor. Ele é gratuito e pode ser instalado e usado sem uma conexão de Internet. Você pode usar o Windows Admin Center para gerenciar e monitorar a infraestrutura de Hyper-Converged que executam o Windows Server 2016 ou Windows Server 2019.

![Painel do cluster hiperconvergente](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principais recursos

Destaques do Windows Admin Center para infraestrutura Hyper-Converged incluem:

- **Unificada-de-único painel para computação, armazenamento e rede em breve.** Exiba suas máquinas virtuais, servidores de host, volumes, unidades e muito mais dentro de uma única experiência interconectada, consistente com finalidade específica.
- **Criar e gerenciar máquinas virtuais do Hyper-V e espaços de armazenamento.** Fluxos de trabalho radicalmente simples para criar, abrir, redimensionar e excluir volumes; criar, iniciar, conectar-se ao e mover máquinas virtuais; e muito mais.
- **Monitoramento poderosa de todo o cluster.** O painel apresenta gráficos de memória e uso da CPU, capacidade de armazenamento, IOPS, taxa de transferência e latência em tempo real, em todos os servidores no cluster, com alertas claras quando algo não está correto.
- **Suporte a software Defined de Networking (SDN).** Gerenciar e monitorar redes virtuais, sub-redes, se conectar a máquinas virtuais para redes virtuais e monitorar a infraestrutura de SDN.

Windows Admin Center para infraestrutura Hyper-Converged está sendo ativamente desenvolvido pela Microsoft. Ele recebe atualizações frequentes que melhoram os recursos existentes e adicionam novos recursos.

## <a name="before-you-start"></a>Antes de começar

Para gerenciar o cluster como infraestrutura Hyper-Converged no Windows Admin Center, ele precisa estar executando o Windows Server 2016 ou Windows Server 2019 e tem Hyper-V e espaços de armazenamento diretos habilitados. Opcionalmente, também pode ter rede definida pelo Software habilitado e gerenciado por meio do Windows Admin Center.

> [!Tip]
> Windows Admin Center também oferece a experiência de um gerenciamento de uso geral para qualquer cluster que dão suporte a qualquer carga de trabalho disponível para o Windows Server 2012 e posterior. Se isso parece mais adequadas, quando você adiciona seu cluster para Windows Admin Center, selecione [ **Cluster de Failover** ](manage-failover-clusters.md) em vez de **Hyper-Converged Cluster**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparar o cluster do Windows Server 2016 para Windows Admin Center

Windows Admin Center para infraestrutura Hyper-Converged depende do gerenciamento de que APIs adicionadas após o lançamento do Windows Server 2016. Antes de poder gerenciar o cluster do Windows Server 2016 com o Windows Admin Center, você precisará executar essas duas etapas:

1. Verifique se todos os servidores no cluster tenham instalado o [2018-05 atualização cumulativa do Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou posterior. Para baixar e instalar essa atualização, acesse **as configurações** > **atualização e segurança** > **Windows atualizar** e selecione  **Verificar online se há atualizações do Microsoft Update**.
2. Execute o seguinte cmdlet do PowerShell como administrador no cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Você só precisará executar o cmdlet de uma vez, em qualquer servidor no cluster. Você pode executá-lo localmente no Windows PowerShell ou usar o Credential Security Service Provider (CredSSP) para executá-lo remotamente. Dependendo da sua configuração, você não poderá executar esse cmdlet de dentro do Windows Admin Center.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparar o cluster do Windows Server 2019 para Windows Admin Center

Se o cluster executar 2019 do Windows Server, as etapas acima não são necessárias. Basta adicionar o cluster para Windows Admin Center, conforme descrito na próxima seção e você está pronto para começar!

### <a name="configure-software-defined-networking-optional"></a>Configurar a rede (opcional) definida pelo Software ###

Você pode configurar sua infraestrutura de Hyper-Converged executando o Windows Server 2016 ou 2019 para usar o Software Defined Networking (SDN) com as seguintes etapas:

1. Prepare o VHD do sistema operacional que é o mesmo sistema operacional instalado nos hosts hiperconvergentes infraestrutura. Esse VHD será usado para todas as VMs NC/SLB/GW.
2. Baixar todos os arquivos em SDN Express no e pasta [ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Prepare uma VM diferente usando o console de implantação. Essa VM deve ser capaz de acessar os hosts SDN. Além disso, a VM deve ter a ferramenta de RSAT Hyper-V instalada.
4. Copie tudo o que você baixou para SDN Express para o console de implantação de VM. Compartilhar isto **SDNExpress** pasta. Verifique se todos os hosts podem acessar o **SDNExpress** pasta compartilhada, conforme definido na linha do arquivo de configuração 8:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copie o VHD do sistema operacional para o **imagens** pasta sob o **SDNExpress** pasta em que o console de implantação de VM.
6. Modificar a configuração de SDN Express com sua configuração do ambiente. Conclua as etapas a seguir depois de modificar a configuração de SDN Express com base em suas informações de ambiente.
7. Execute o PowerShell com privilégios de administrador para implantar SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

A implantação levará cerca de 30 a 45 minutos.

## <a name="get-started"></a>Introdução

Depois que sua infraestrutura de Hyper-Converged for implantada, você pode gerenciá-lo usando o Windows Admin Center.

### <a name="install-windows-admin-center"></a>Instalar o Windows Admin Center

Se você ainda não o fez, baixe e instale o Windows Admin Center. O mais rápido para começar a trabalhar e em execução é instalá-lo em seu computador com Windows 10 e gerencie seus servidores remotamente. Isso leva menos de cinco minutos. [Baixe agora mesmo](https://aka.ms/windowsadmincenter) ou [Saiba mais sobre outras opções de instalação](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Adicionar um Cluster Hiperconvergente

Para adicionar o cluster para Windows Admin Center:

1. Clique em **+ adicionar** em todas as conexões.
2. Optar por adicionar um **Conexão de Cluster Hyper-Converged**.
3. Digite o nome do cluster e, se solicitado, as credenciais a usar.
4. Clique em **adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique para iniciar o painel.

![Adicionar conexão de cluster hiperconvergente](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Adicionar SDN Hiperconvergente Cluster habilitado (Windows Admin Center versão prévia)

A visualização mais recente do Windows Admin Center oferece suporte ao gerenciamento de rede definida pelo Software para infra-estrutura Hyper-Converged. Adicionando um URI de REST do controlador de rede para sua conexão de cluster Hiperconvergente, você pode usar o Gerenciador de Cluster hiperconvergente para gerenciar seus recursos SDN e monitorar a infraestrutura de SDN.

1. Clique em **+ adicionar** em todas as conexões.
2. Optar por adicionar um **Conexão de Cluster Hyper-Converged**.
3. Digite o nome do cluster e, se solicitado, as credenciais a usar.
4. Verifique **configurar o controlador de rede** para continuar.
5. Insira o **URI de controlador de rede** e clique em **validar**.
6. Clique em **adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique para iniciar o painel.

![Adicionar conexão habilitada para SDN cluster hiperconvergente](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Ambientes de SDN com a autenticação Kerberos para comunicação Northbound não têm suporte no momento.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Há diferenças entre gerenciar o Windows Server 2016 e Windows Server 2019?

Sim. Windows Admin Center para infraestrutura Hyper-Converged recebe atualizações frequentes que melhoram a experiência para o Windows Server 2016 e Windows Server 2019. No entanto, alguns novos recursos só estão disponíveis para Windows Server 2019 – por exemplo, a opção de alternância de eliminação de duplicação e compactação.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>Pode usar o Windows Admin Center para gerenciar espaços de armazenamento diretos para outros casos de uso (não hiperconvergida), como o servidor de arquivos de escalabilidade horizontal (SoFS) ou o Microsoft SQL Server convergida?

Windows Admin Center para infraestrutura Hyper-Converged não oferece gerenciamento ou monitoramento opções específicas para outros casos de uso de espaços de armazenamento diretos – por exemplo, ele não é possível criar compartilhamentos de arquivos. No entanto, os recursos de painel e core, como criar volumes ou substituindo unidades, funcionam para qualquer cluster de espaços de armazenamento diretos.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>O que é a diferença entre um Cluster de Failover e um Cluster Hyper-Converged?

Em geral, o termo "hiperconvergente" refere-se à execução do Hyper-V e espaços de armazenamento diretos no mesmo cluster de servidores para virtualizar os recursos de computação e armazenamento. No contexto do Windows Admin Center, quando você clica **+ adicionar** na lista de conexões, é possível escolher entre adicionar um **conexão de Cluster de Failover** ou um **Hyper-Converged Cluster conexão**:

- O **conexão de Cluster de Failover** é o sucessor para o aplicativo de área de trabalho do Gerenciador de Cluster de Failover. Ele fornece uma experiência familiar e de uso geral de gerenciamento para qualquer cluster que dão suporte a qualquer carga de trabalho, incluindo o Microsoft SQL Server. Ele está disponível para o Windows Server 2012 e versões posteriores.

- O **conexão de Cluster Hyper-Converged** é uma experiência totalmente nova adaptada para espaços de armazenamento diretos e Hyper-V. Contém recursos do Painel e destaca gráficos e alertas para monitoramento. Ele está disponível para Windows Server 2016 e Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Por que eu preciso a atualização cumulativa mais recente para o Windows Server 2016?

Windows Admin Center para infraestrutura Hyper-Converged depende do gerenciamento de que APIs desenvolvidas desde o lançamento do Windows Server 2016. Essas APIs são adicionadas a [2018-05 atualização cumulativa do Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponível a partir de 8 de maio de 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto custa para usar o Windows Admin Center?

O Windows Admin Center não tem custo adicional além do Windows.

Você pode usar o Windows Admin Center (disponível como um download separado) com licenças válidas do Windows Server ou Windows 10 sem nenhum custo adicional - ele está licenciado sob um EULA complementar do Windows.

### <a name="does-windows-admin-center-require-system-center"></a>O Windows Admin Center requer o System Center?

Não.

### <a name="does-it-require-an-internet-connection"></a>Ele requer uma conexão de Internet?

Não.

Embora o Windows Admin Center oferece poderosos e integração conveniente com a nuvem do Microsoft Azure, o gerenciamento central e experiência de monitoramento de infraestrutura Hyper-Converged é completamente no local. Podem ser instalado e usado sem uma conexão de Internet.

## <a name="things-to-try"></a>Ações recomendadas

Se você estiver apenas começando, aqui estão alguns tutoriais rápidos para ajudar você a aprender como o Windows Admin Center para infraestrutura Hyper-Converged é organizado e funciona. Verifique a exercitar o bom senso e tenha cuidado com os ambientes de produção. Esses vídeos foram gravados com versão 1804 do Windows Admin Center e uma compilação do Insider Preview do Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gerenciar volumes de espaços de armazenamento diretos

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">como criar um volume de três vias</a></li>
               <li>(17:1) <a href="https://youtu.be/R72QHudqWpE">como criar um volume de paridade de aceleração de espelho</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">como abrir um volume e adicionar arquivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">como ativar a eliminação de duplicação e compactação</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">como expandir um volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">como excluir um volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Criar volume, o espelho de três vias</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Criar volume, paridade acelerada de espelho</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Abra o volume e adicionar arquivos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Ativar a eliminação de duplicação e compactação</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Expanda o volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Excluir volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Criar uma nova máquina virtual

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **estoque** guia e, em seguida, clique em **New** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre as máquinas virtuais de geração 1 e 2.
4. Você pode escolher qual host para criar a máquina virtual em inicialmente ou usar o host recomendado.
5. Escolha um caminho para os arquivos de máquina virtual. Escolha um volume na lista suspensa ou clique em **procurar** para escolher uma pasta usando o seletor de pasta. Os arquivos de configuração de máquina virtual e o arquivo de disco rígido virtual serão salvo em uma única pasta sob o `\Hyper-V\[virtual machine name]` caminho do volume selecionado ou caminho.
6. Escolha o número de processadores virtuais, se você virtualização aninhada habilitada, configure as configurações de memória, adaptadores de rede, discos rígidos virtuais e escolher se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou de rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e aparece na lista de máquinas virtuais, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual é iniciada, você pode se conectar ao console da máquina virtual por meio do VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais** > **Connect** para baixar o arquivo. rdp. Abra o arquivo. RDP no aplicativo da Conexão de área de trabalho remota. Como isso está se conectando ao console da máquina virtual, você precisará inserir credenciais de administrador do host do Hyper-V.

[Saiba mais sobre o gerenciamento de máquina virtual com Windows Admin Center](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Pausar e reiniciar com segurança um servidor

1. Dos **Dashboard**, selecione **servidores** na barra de navegação à esquerda ou clicando o **Exibir servidores >** link no bloco no canto inferior direito do painel .
2. Na parte superior, alternar da **resumo** para o **inventário** guia.
3. Selecione um servidor clicando em seu nome para abrir o **Server** página de detalhes.
4. Clique em **servidor pausar para manutenção**. Se é seguro continuar, isso irá mover máquinas virtuais para outros servidores no cluster. O servidor terá o status drenagem enquanto isso acontece. Se você quiser, você pode assistir as máquinas virtuais de passar o **máquinas virtuais > inventário** página, em que o servidor de host é mostrado claramente na grade. Quando todas as máquinas virtuais foram movidas, o status do servidor estará **pausado**.
5. Clique em **Gerenciar servidor** para acessar todas as ferramentas de gerenciamento por servidor no Windows Admin Center.
6. Clique em **reinicie**, em seguida, **Sim**. Você será devolvido à lista de conexões.
7. Volta a **Dashboard**, o servidor está em vermelho, enquanto ele está inativo.
8. Quando for fazer backup, navegue novamente o **Server** da página e clique em **retomar o servidor de manutenção** para definir o status do servidor para cima simplesmente. No momento, as máquinas virtuais serão mover de volta – é necessária nenhuma ação do usuário.

### <a name="replace-a-failed-drive"></a>Substituir uma unidade com falha

1. Quando um disco falhar, um alerta será exibido no canto superior esquerdo **alertas** área da **painel**.
2. Você também pode selecionar **unidades** na barra de navegação no lado esquerdo ou clique a **unidades de exibição >** link no bloco no canto inferior direito para procurar unidades e ver seu status por conta própria. No **inventário** guia, a grade dá suporte à classificação, agrupamento e pesquisa de palavra-chave.
3. Dos **Dashboard**, clique no alerta para ver os detalhes, como o local físico da unidade.
4. Para saber mais, clique o **ir para a unidade** atalho para o **unidade** página de detalhes.
5. Se seu hardware dá suporte a ele, você poderá clicar **Ativar luz/desativar** para controlar a luz indicadora da unidade.
6. Espaços de armazenamento diretos automaticamente desativa e evacua unidades com falha. Quando isso tiver ocorrido, o status da unidade está desativado e sua barra de capacidade de armazenamento está vazia.
7. Remova a unidade com falha e insira sua substituição.
8. Na **unidades > inventário**, a nova unidade será exibida. No momento, limpará o alerta, volumes irá reparar volta ao status Okey e armazenamento irá rebalancear no novo disco – é necessária nenhuma ação do usuário.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gerenciar redes virtuais (clusters HCI habilitado de SDN usando o Windows Admin Center Preview)

1. Selecione **redes virtuais** na barra de navegação à esquerda.
2. Clique em **New** para criar uma nova rede virtual e sub-redes, ou escolha uma rede virtual existente e clique em **configurações** para modificar sua configuração.
3. Clique em uma rede virtual existente para exibir conexões de máquina virtual para as sub-redes da rede virtual e aplicadas às sub-redes de rede virtual de listas de controle de acesso.

![Gerenciar redes virtuais](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Conectar uma máquina virtual a uma rede virtual (clusters HCI habilitado de SDN usando o Windows Admin Center Preview)

1. Selecione **máquinas virtuais** na barra de navegação à esquerda.
2. Escolha uma máquina virtual > clique em **configurações** > Abra o **redes** guia **configurações**.
3. Configurar o **rede Virtual** e **sub-rede Virtual** campos para conectar-se a máquina virtual a uma rede virtual.

Você também pode configurar a rede virtual ao criar uma máquina virtual.

![Conectar uma máquina virtual a uma rede virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Infraestrutura de rede definida pelo Software de monitor (clusters HCI habilitado de SDN usando o Windows Admin Center Preview)

1. Selecione **SDN monitoramento** na barra de navegação à esquerda.
2. Exibir informações detalhadas sobre a integridade do Gateway Virtual do controlador de rede, balanceador de carga de Software e monitorar o uso de Pool de Gateway Virtual, público e o Pool de IP privado e o status do host SDN.

![Monitorar a infraestrutura de SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Privacidade Jurídica

É tudo sobre seus comentários! O benefício mais importante de atualizações frequentes é ouvir o que está funcionando e o que precisa ser melhorado. Aqui estão algumas maneiras para nos informar sobre o que você está pensando:

- [Envie e vote em solicitações de recurso no UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Junte-se o fórum do Windows Admin Center no Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet para `@servermgmt`

### <a name="see-also"></a>Consulte também

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Espaços de Armazenamento Diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Rede definida pelo software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
