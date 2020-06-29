---
title: Gerenciar a infraestrutura hiperconvergente com o centro de administração do Windows
description: Gerenciar a infraestrutura hiperconvergente com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 71e45622292f7393b19978ec3235492c5065a8a1
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474023"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gerenciar a infraestrutura hiperconvergente com o centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

## <a name="what-is-hyper-converged-infrastructure"></a>O que é a infraestrutura hiperconvergente

A infraestrutura hiperconvergente consolida a computação, o armazenamento e a rede definidos pelo software em um cluster para fornecer virtualização de alto desempenho, econômica e facilmente escalonável. Esse recurso foi introduzido no Windows Server 2016 com [espaços de armazenamento diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [rede definida pelo software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking) e [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Procurando adquirir a infraestrutura hiperconvergente? A Microsoft recomenda essas soluções [definidas por software do Windows Server](https://microsoft.com/wssd) de nossos parceiros. Eles são projetados, montados e validados em nossa arquitetura de referência para garantir a compatibilidade e a confiabilidade, para que você comece a trabalhar rapidamente.

> [!IMPORTANT]
> Alguns dos recursos descritos neste artigo estão disponíveis apenas na versão prévia do centro de administração do Windows. [Como fazer obter esta versão?](https://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>O que é o Windows Admin Center

O [centro de administração do Windows](../overview.md) é a ferramenta de gerenciamento de última geração para o Windows Server, o sucessor das ferramentas tradicionais "prontas", como Gerenciador do servidor. Ele é gratuito e pode ser instalado e usado sem uma conexão com a Internet. Você pode usar o centro de administração do Windows para gerenciar e monitorar a infraestrutura hiperconvergente que executa o Windows Server 2016 ou o Windows Server 2019.

![Painel de cluster hiperconvergente](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principais recursos

Os destaques do centro de administração do Windows para a infraestrutura hiperconvergente incluem:

- **Um único painel unificado para computação, armazenamento e rede em breve.** Exiba suas máquinas virtuais, servidores host, volumes, unidades e muito mais dentro de uma experiência de finalidade, consistente e estabelecida.
- **Crie e gerencie espaços de armazenamento e máquinas virtuais do Hyper-V.** Fluxos de trabalho radicalmente simples para criar, abrir, redimensionar e excluir volumes; e criar, iniciar, conectar e mover máquinas virtuais; e muito mais.
- **Monitoramento avançado em todo o cluster.** O painel graphs de memória e uso de CPU, capacidade de armazenamento, IOPS, taxa de transferência e latência em tempo real, em todos os servidores no cluster, com alertas claros quando algo não está certo.
- **Suporte a SDN (rede definida pelo software).** Gerencie e monitore redes virtuais, sub-redes, conecte máquinas virtuais a redes virtuais e monitore a infraestrutura de SDN.

O centro de administração do Windows para a infraestrutura hiperconvergente está sendo ativamente desenvolvido pela Microsoft. Ele recebe atualizações frequentes que melhoram os recursos existentes e adicionam novos recursos.

## <a name="before-you-start"></a>Antes de começar

Para gerenciar o cluster como uma infraestrutura hiperconvergente no centro de administração do Windows, ele precisa estar executando o Windows Server 2016 ou o Windows Server 2019 e ter o Hyper-V e o Espaços de Armazenamento Diretos habilitados. Opcionalmente, ele também pode ter uma rede definida pelo software habilitada e gerenciada por meio do centro de administração do Windows.

> [!Tip]
> O centro de administração do Windows também oferece uma experiência de gerenciamento de uso geral para qualquer cluster que ofereça suporte a qualquer carga de trabalho, disponível para o Windows Server 2012 e posterior. Se isso se parece com um melhor ajuste, ao adicionar o cluster ao centro de administração do Windows, selecione [**cluster de failover**](manage-failover-clusters.md) em vez de **cluster hiperconvergente**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparar o cluster do Windows Server 2016 para o centro de administração do Windows

O centro de administração do Windows para infraestrutura hiperconvergente depende das APIs de gerenciamento adicionadas após o lançamento do Windows Server 2016. Para poder gerenciar o cluster do Windows Server 2016 com o centro de administração do Windows, você precisará executar estas duas etapas:

1. Verifique se todos os servidores no cluster instalaram a [atualização cumulativa 2018-05 para o Windows server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou posterior. Para baixar e instalar essa atualização, vá para **configurações**  >  **Atualizar & segurança**  >  **Windows Update** e selecione **verificar online se há atualizações de Microsoft Update**.
2. Execute o seguinte cmdlet do PowerShell como administrador no cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Você só precisa executar o cmdlet uma vez, em qualquer servidor no cluster. Você pode executá-lo localmente no Windows PowerShell ou usar o Credential Security Service Provider (CredSSP) para executá-lo remotamente. Dependendo de sua configuração, talvez você não consiga executar esse cmdlet no centro de administração do Windows.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparar o cluster do Windows Server 2019 para o centro de administração do Windows

Se o cluster executar o Windows Server 2019, as etapas acima não serão necessárias. Basta adicionar o cluster ao centro de administração do Windows conforme descrito na próxima seção e você estará pronto!

### <a name="configure-software-defined-networking-optional"></a>Configurar a rede definida pelo software (opcional) ###

Você pode configurar a infraestrutura hiperconvergente que executa o Windows Server 2016 ou 2019 para usar a SDN (rede definida pelo software) com as seguintes etapas:

1. Prepare o VHD do sistema operacional que é o mesmo sistema operacional instalado nos hosts de infraestrutura hiperconvergente. Esse VHD será usado para todas as VMs NC/SLB/GW.
2. Baixe todos os arquivos e pastas em SDN Express de [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress) .
3. Prepare uma VM diferente usando o console de implantação. Essa VM deve ser capaz de acessar os hosts SDN. Além disso, a VM deve ter a ferramenta do Hyper-V do RSAT instalada.
4. Copie tudo o que você baixou para o SDN Express para a VM do console de implantação. E compartilhe essa pasta **SDNExpress** . Verifique se todos os hosts podem acessar a pasta compartilhada **SDNExpress** , conforme definido no arquivo de configuração linha 8:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copie o VHD do sistema operacional para a pasta **imagens** na pasta **SDNExpress** na VM do console de implantação.
6. Modifique a configuração do SDN Express com a configuração do seu ambiente. Conclua as duas etapas a seguir depois de modificar a configuração do SDN Express com base nas informações do seu ambiente.
7. Execute o PowerShell com privilégio de administrador para implantar SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose
```

A implantação levará cerca de 30 a 45 minutos.

## <a name="get-started"></a>Introdução

Depois que a infraestrutura hiperconvergente for implantada, você poderá gerenciá-la usando o centro de administração do Windows.

### <a name="install-windows-admin-center"></a>Instalar o Windows Admin Center

Se você ainda não fez isso, baixe e instale o centro de administração do Windows. A maneira mais rápida de colocar em funcionamento é instalá-lo em seu computador com Windows 10 e gerenciar seus servidores remotamente. Isso leva menos de cinco minutos. [Baixe agora](https://aka.ms/windowsadmincenter) ou [saiba mais sobre outras opções de instalação](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Adicionar cluster hiperconvergente

Para adicionar o cluster ao centro de administração do Windows:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha Adicionar uma **conexão de cluster hiperconvergente**.
3. Digite o nome do cluster e, se solicitado, as credenciais a serem usadas.
4. Clique em **Adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique nele para iniciar o painel.

![Adicionar conexão de cluster hiperconvergente](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Adicionar cluster hiperconvergente habilitado para SDN (versão prévia do centro de administração do Windows)

A versão prévia mais recente do centro de administração do Windows dá suporte ao gerenciamento de rede definido pelo software para a infraestrutura hiperconvergente. Ao adicionar um URI de REST do controlador de rede à sua conexão de cluster hiperconvergente, você pode usar o Gerenciador de cluster hiperconvergente para gerenciar seus recursos de SDN e monitorar a infraestrutura de SDN.

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha Adicionar uma **conexão de cluster hiperconvergente**.
3. Digite o nome do cluster e, se solicitado, as credenciais a serem usadas.
4. Marque **Configurar o controlador de rede** para continuar.
5. Insira o **URI do controlador de rede** e clique em **validar**.
6. Clique em **Adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique nele para iniciar o painel.

![Adicionar conexão de cluster hiperconvergente habilitada para SDN](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Atualmente, não há suporte para ambientes SDN com autenticação Kerberos para comunicação northbound.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Há diferenças entre o gerenciamento do Windows Server 2016 e do Windows Server 2019?

Sim. O centro de administração do Windows para infraestrutura hiperconvergente recebe atualizações frequentes que melhoram a experiência do Windows Server 2016 e do Windows Server 2019. No entanto, determinados recursos novos estão disponíveis apenas para o Windows Server 2019 – por exemplo, o comutador de alternância para eliminação de duplicação e compactação.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>Posso usar o centro de administração do Windows para gerenciar Espaços de Armazenamento Diretos para outros casos de uso (não hiperconvergente), como Servidor de Arquivos de Escalabilidade Horizontal convergido (SoFS) ou Microsoft SQL Server?

O centro de administração do Windows para a infraestrutura hiperconvergente não fornece opções de gerenciamento ou monitoramento especificamente para outros casos de uso de Espaços de Armazenamento Diretos – por exemplo, ele não pode criar compartilhamentos de arquivos. No entanto, os recursos de painel e núcleo, como a criação de volumes ou a substituição de unidades, funcionam para qualquer cluster de Espaços de Armazenamento Diretos.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>Qual é a diferença entre um cluster de failover e um cluster hiperconvergente?

Em geral, o termo "hiperconvergente" refere-se à execução do Hyper-V e Espaços de Armazenamento Diretos nos mesmos servidores clusterizados para virtualizar os recursos de computação e armazenamento. No contexto do centro de administração do Windows, ao clicar em **+ Adicionar** na lista de conexões, você pode escolher entre adicionar uma **conexão de cluster de failover** ou uma conexão de **cluster hiperconvergente**:

- A **conexão de cluster de failover** é o sucessor do aplicativo de área de trabalho Gerenciador de cluster de failover. Ele fornece uma experiência de gerenciamento familiar e de uso geral para qualquer cluster com suporte para qualquer carga de trabalho, incluindo Microsoft SQL Server. Ele está disponível para o Windows Server 2012 e posterior.

- A **conexão de cluster hiperconvergente** é uma experiência totalmente nova, adaptada para espaços de armazenamento diretos e Hyper-V. Contém recursos do Painel e destaca gráficos e alertas para monitoramento. Ele está disponível para o Windows Server 2016 e o Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Por que preciso da atualização cumulativa mais recente para o Windows Server 2016?

O centro de administração do Windows para infraestrutura hiperconvergente depende das APIs de gerenciamento desenvolvidas desde que o Windows Server 2016 foi lançado. Essas APIs são adicionadas na [atualização cumulativa 2018-05 para o Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponível a partir de 8 de maio de 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto custa usar o Windows Admin Center?

O Windows Admin Center não tem custo adicional além do Windows.

É possível usar o Windows Admin Center (disponível como um download separado) com licenças válidas do Windows Server ou Windows 10 sem custo adicional – ele é licenciado sob um EULA complementar do Windows.

### <a name="does-windows-admin-center-require-system-center"></a>O Windows Admin Center requer o System Center?

Não.

### <a name="does-it-require-an-internet-connection"></a>Ele requer uma conexão com a Internet?

Não.

Embora o centro de administração do Windows ofereça uma integração poderosa e conveniente com o Microsoft Azure Cloud, a experiência básica de gerenciamento e monitoramento para a infraestrutura hiperconvergente é completamente local. Ele pode ser instalado e usado sem uma conexão com a Internet.

## <a name="things-to-try"></a>Ações recomendadas

Se você estiver apenas começando, aqui estão alguns tutoriais rápidos para ajudá-lo a aprender como o centro de administração do Windows para a infraestrutura hiperconvergente é organizado e funciona. Faça um bom Judgement e tenha cuidado com ambientes de produção. Esses vídeos foram registrados com a versão 1804 do centro de administração do Windows e uma compilação do insider preview do Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gerenciar volumes de Espaços de Armazenamento Diretos

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">como criar um volume de espelho de três vias</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">como criar um volume de paridade acelerado por espelhamento</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">como abrir um volume e adicionar arquivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">como ativar a eliminação de duplicação e a compactação</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">como expandir um volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">como excluir um volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Criar um volume, espelho de três vias</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Criar volume, paridade acelerada por espelhamento</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Abrir volume e adicionar arquivos</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Ativar a eliminação de duplicação e a compactação</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Expandir volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Excluir volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Criar uma nova máquina virtual

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** e clique em **novo** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre as máquinas virtuais de geração 1 e 2.
4. Você pode escolher em qual host a máquina virtual será criada inicialmente ou usar o host recomendado.
5. Escolha um caminho para os arquivos de máquina virtual. Escolha um volume na lista suspensa ou clique em **procurar** para escolher uma pasta usando o seletor de pasta. Os arquivos de configuração de máquina virtual e o arquivo de disco rígido virtual serão salvos em uma única pasta sob o `\Hyper-V\[virtual machine name]` caminho do volume ou caminho selecionado.
6. Escolha o número de processadores virtuais, se você deseja habilitar a virtualização aninhada, definir configurações de memória, adaptadores de rede, discos rígidos virtuais e escolher se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou da rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e exibida na lista de máquinas virtuais, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual for iniciada, você poderá se conectar ao console da máquina virtual por meio do VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais**  >  **conexão** para baixar o arquivo. rdp. Abra o arquivo. rdp no aplicativo Conexão de Área de Trabalho Remota. Como isso está se conectando ao console da máquina virtual, será necessário inserir as credenciais de administrador do host Hyper-V.

[Saiba mais sobre o gerenciamento de máquinas virtuais com o centro de administração do Windows](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Pausar e reiniciar com segurança um servidor

1. No **painel**, selecione **servidores** na navegação no lado esquerdo ou clicando no link **Exibir servidores >** no bloco no canto inferior direito do painel.
2. Na parte superior, mude de **Resumo** para a guia **inventário** .
3. Selecione um servidor clicando em seu nome para abrir a página de detalhes do **servidor** .
4. Clique em **Pausar servidor para manutenção**. Se for seguro continuar, isso moverá as máquinas virtuais para outros servidores no cluster. O servidor terá o status drenando enquanto isso acontece. Se desejar, você pode assistir à movimentação das máquinas virtuais na página **máquinas virtuais > inventário** , onde o servidor host é mostrado claramente na grade. Quando todas as máquinas virtuais forem movidas, o status do servidor será **pausado**.
5. Clique em **gerenciar servidor** para acessar todas as ferramentas de gerenciamento por servidor no centro de administração do Windows.
6. Clique em **reiniciar**e em **Sim**. Você será inicializado novamente para a lista de conexões.
7. De volta ao **painel**, o servidor fica em vermelho quando está inoperante.
8. Depois de fazer o backup, navegue novamente na página do **servidor** e clique em **retomar servidor da manutenção** para definir o status do servidor de volta como simples. No tempo, as máquinas virtuais se moverão de volta – nenhuma ação do usuário é necessária.

### <a name="replace-a-failed-drive"></a>Substituir uma unidade com falha

1. Quando uma unidade falha, um alerta é exibido na área de **alertas** superior esquerdo do **painel**.
2. Você também pode selecionar **unidades** da navegação no lado esquerdo ou clicar no link **Exibir unidades >** no bloco no canto inferior direito para procurar unidades e ver seu status por conta própria. Na guia **inventário** , a grade dá suporte à classificação, agrupamento e pesquisa de palavra-chave.
3. No **painel**, clique no alerta para ver os detalhes, como o local físico da unidade.
4. Para saber mais, clique no atalho **ir para unidade** para a página de detalhes da **unidade** .
5. Se o hardware oferecer suporte a ele, você poderá clicar em **Ativar/desativar luz** para controlar a luz do indicador da unidade.
6. Espaços de Armazenamento Diretos desativa automaticamente e evacua unidades com falha. Quando isso aconteceu, o status da unidade é desativado e sua barra de capacidade de armazenamento está vazia.
7. Remova a unidade com falha e insira sua substituição.
8. Em **unidades > inventário**, a nova unidade será exibida. No tempo, o alerta será limpo, os volumes serão reparados para o status OK e o armazenamento será Rebalanceado na nova unidade – nenhuma ação do usuário é necessária.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gerenciar redes virtuais (clusters de HCI habilitados para SDN usando a versão prévia do centro de administração do Windows)

1. Selecione **redes virtuais** na navegação no lado esquerdo.
2. Clique em **novo** para criar uma nova rede virtual e sub-redes, ou escolha uma rede virtual existente e clique em **configurações** para modificar sua configuração.
3. Clique em uma rede virtual existente para exibir as conexões de VM com as sub-redes da rede virtual e as listas de controle de acesso aplicadas às sub-redes da rede virtual.

![Gerenciar redes virtuais](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Conectar uma máquina virtual a uma rede virtual (clusters de HCI habilitados para SDN usando a versão prévia do centro de administração do Windows)

1. Selecione **máquinas virtuais** na navegação no lado esquerdo.
2. Escolha uma máquina virtual existente > clique em **configurações** > abrir a guia **redes** em **configurações**.
3. Configure a **rede virtual** e os campos de **sub-rede virtual** para conectar a máquina virtual a uma rede virtual.

Você também pode configurar a rede virtual ao criar uma máquina virtual.

![Conectar uma máquina virtual a uma rede virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Monitorar a infraestrutura de rede definida pelo software (clusters de HCI habilitados para SDN usando a versão prévia do centro de administração do Windows)

1. Selecione **monitoramento de Sdn** na navegação no lado esquerdo.
2. Exiba informações detalhadas sobre a integridade do controlador de rede, Load Balancer de software, gateway virtual e monitore o seu pool de gateway virtual, o uso do pool de IPS público e privado e o status do host SDN.

![Monitorar a infraestrutura de SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Comentários

Isso é tudo sobre seus comentários! O benefício mais importante das atualizações frequentes é ouvir o que está funcionando e o que precisa ser melhorado. Aqui estão algumas maneiras de nos informar o que você está pensando:

- [Enviar e votar em solicitações de recursos no UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Participe do fórum do centro de administração do Windows na Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet para`@servermgmt`

### <a name="additional-references"></a>Referências adicionais

- [Windows Admin Center](../overview.md)
- [Espaços de Armazenamento Diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Rede definida pelo software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
