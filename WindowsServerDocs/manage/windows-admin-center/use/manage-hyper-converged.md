---
title: Gerenciar a infraestrutura Hiperconvergente com o Windows Admin Center
description: Gerenciar a infraestrutura Hiperconvergente com o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262839"
---
# Gerenciar a infraestrutura Hiperconvergente com o Windows Admin Center

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

## O que é a infraestrutura hiperconvergente

Infraestrutura Hiperconvergente consolida definido pelo software computação, armazenamento e rede em um cluster para oferecer alto desempenho, econômico e facilmente dimensionável virtualização. Essa funcionalidade foi introduzida no Windows Server 2016 com [Espaços de armazenamento diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Rede definida pelo Software](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) e de [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Procurando para adquirir a infraestrutura hiperconvergente? A Microsoft recomenda essas soluções [Definidas pelo Software do Windows Server](https://microsoft.com/wssd) de nossos parceiros. Eles são projetados, montados e validados nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, para que você obtenha e comecem a trabalhar rapidamente.

> [!IMPORTANT]
> Alguns dos recursos descritos neste artigo só estão disponíveis no Windows Admin Center Preview. [Como posso obter esta versão?](http://aka.ms/windowsadmincenter)

## O que é o Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) é a ferramenta de gerenciamento de última geração para o Windows Server, o sucessor tradicionais ferramentas "nativas" como o Gerenciador do servidor. Ele é gratuito e pode ser instalado e usado sem uma conexão de Internet. Você pode usar o Windows Admin Center para gerenciar e monitorar a infraestrutura hiperconvergente executando o Windows Server 2016 ou Windows Server 2019.

![Painel de cluster de hiperconvergência](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## Principais recursos

Destaques do Windows Admin Center para a infraestrutura hiperconvergente incluem:

- **Unificada-de-único painel de computação, armazenamento e rede em breve.** Exiba suas máquinas virtuais, servidores do host, volumes, unidades e muito mais dentro de uma experiência interconectada, desenvolvido especificamente consistente.
- **Criar e gerenciar máquinas virtuais Hyper-V e de espaços de armazenamento.** Fluxos de trabalho radicalmente simples para criar, abrir, redimensionar e excluir volumes; e criar, iniciar, conectar a e mover máquinas virtuais; e muito mais.
- **Monitoramento de todo o cluster poderosa.** O painel de gráficos memória e uso de CPU, capacidade de armazenamento, IOPS, taxa de transferência e latência em tempo real, em cada servidor no cluster, limpar alertas quando algo não está correto.
- **Suporte do software SDN (rede definida).** Gerenciar e monitorar redes virtuais, sub-redes, conectar máquinas virtuais a redes virtuais e monitorar infraestrutura SDN.

Windows Admin Center para a infraestrutura hiperconvergente está sendo ativamente desenvolvido pela Microsoft. Ele recebe atualizações frequentes que melhorar os recursos existentes e adicionar novos recursos.

## Antes de começar

Para gerenciar o cluster como a infraestrutura hiperconvergente no Centro de administração do Windows, ele precisa estar executando o Windows Server 2016 ou Windows Server 2019 e Hyper-V e espaços de armazenamento diretos habilitou. Opcionalmente, também podem ter rede definida pelo Software habilitado e gerenciado por meio do Windows Admin Center.

> [!Tip]
> Windows Admin Center também oferece um gerenciamento de finalidade geral experiência cluster oferecer suporte a qualquer carga de trabalho, disponível para o Windows Server 2012 e versões posteriores. Se isso parece uma opção melhor, quando você adiciona o cluster no Windows Admin Center, selecione o [**Cluster de Failover**](manage-failover-clusters.md) em vez de **Cluster hiperconvergente**.

### Preparar seu cluster do Windows Server 2016 para Windows Admin Center

Windows Admin Center para a infraestrutura hiperconvergente depende do gerenciamento de que APIs adicionadas após o lançamento do Windows Server 2016. Para que você possa gerenciar seu cluster do Windows Server 2016 com o Windows Admin Center, você precisará executar essas duas etapas:

1. Verifique se cada servidor no cluster tiver instalado a [atualização cumulativa para o Windows Server 2016 (KB4103723) de 2018-05](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou posterior. Para baixar e instalar essa atualização, vá para **configurações** > **Atualizar & segurança** > **Windows Update** e selecione **verificar online se há atualizações do Microsoft Update**.
2. Execute o seguinte cmdlet do PowerShell como administrador no cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Você só precisa executar o cmdlet uma vez, em qualquer servidor no cluster. Você pode executá-lo localmente no Windows PowerShell ou usar o provedor de serviços de segurança de credencial (CredSSP) para executá-lo remotamente. Dependendo da configuração, você não poderá executar este cmdlet de dentro do Windows Admin Center.

### Preparar seu cluster do Windows Server 2019 para o Windows Admin Center

Se seu cluster executa o Windows Server 2019, as etapas acima não são necessárias. Basta adicionar o cluster ao Windows Admin Center, conforme descrito na próxima seção, e você está pronto!

### Configurar rede (opcional) definida por Software ###

Você pode configurar sua infraestrutura hiperconvergente executando o Windows Server 2016 ou 2019 usar rede definida pelo Software (SDN) com as seguintes etapas:

1. Prepare o VHD do sistema operacional que é o mesmo sistema operacional instalado nos hosts infraestrutura hiperconvergente. Esse VHD será usado para todas as VMs SLB/NC/GW.
2. Baixar todos os arquivos sob SDN Express do e pasta [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Prepare uma VM diferente usando o console de implantação. Essa VM deve ser capaz de acessar os hosts SDN. Além disso, a VM deve ter a ferramenta RSAT Hyper-V instalada.
4. Copie tudo o que você baixou para SDN Express no console de implantação VM. E compartilhar essa pasta **SDNExpress** . Verifique se que cada host pode acessar a pasta compartilhada **SDNExpress** , conforme definido na linha do arquivo de configuração 8:
```
    \\$env:Computername\SDNExpress
```
5. Copie o VHD do sistema operacional para a pasta **imagens** sob a pasta **SDNExpress** no console do deployment VM.
6. Modificar a configuração expressa SDN com sua configuração do ambiente. Conclua as etapas a seguir depois de modificar a configuração expressa SDN com base nas informações de seu ambiente.
7. Execute o PowerShell com privilégios de administrador para implantar SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

A implantação levará cerca de 30 a 45 minutos.

## Introdução

Depois que sua infraestrutura hiperconvergente for implantada, você pode gerenciar usando o Windows Admin Center.

### Instalação do Windows Admin Center

Se você ainda não o fez, baixe e instale o Windows Admin Center. O mais rápido para obter e em execução são instalá-lo em seu computador Windows 10 e gerenciar seus servidores remotamente. Isso leva menos de cinco minutos. [Baixe agora](https://aka.ms/windowsadmincenter) ou [Saiba mais sobre outras opções de instalação](../deploy/install.md).

### Adicionar o Cluster Hiperconvergente

Para adicionar o cluster no Windows Admin Center:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha esta opção Adicionar uma **Conexão de Cluster hiperconvergente**.
3. Digite o nome do cluster e, se solicitado, as credenciais para usar.
4. Clique em **Adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique nele para iniciar o painel.

![Adicionar conexão de cluster hiperconvergente](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### Adicionar habilitado SDN Cluster de Hiperconvergência (o Windows Admin Center Preview)

O Windows Admin Center Preview mais recente oferece suporte ao gerenciamento de rede definida pelo Software para a infraestrutura hiperconvergente. Adicionando um URI de REST do controlador de rede para sua conexão de cluster de Hiperconvergência, você pode usar o Gerenciador de Cluster de hiperconvergência para gerenciar seus recursos SDN e monitorar infraestrutura SDN.

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha esta opção Adicionar uma **Conexão de Cluster hiperconvergente**.
3. Digite o nome do cluster e, se solicitado, as credenciais para usar.
4. Verifique a **Configurar o controlador de rede** para continuar.
5. Insira o **URI de controlador de rede** e clique em **Validar**.
6. Clique em **Adicionar** para concluir.

O cluster será adicionado à sua lista de conexões. Clique nele para iniciar o painel.

![Adicionar conexão habilitada SDN cluster hiperconvergente](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Ambientes SDN com a autenticação Kerberos para comunicação em sentido norte atualmente não têm suporte.

## Perguntas frequentes

### Há diferenças entre o gerenciamento do Windows Server 2016 e Windows Server 2019?

Sim. Windows Admin Center para a infraestrutura hiperconvergente recebe atualizações frequentes que melhoram a experiência para o Windows Server 2016 e Windows Server 2019. No entanto, determinados recursos novos só estão disponíveis para o Windows Server 2019 – por exemplo, o switch de alternância para eliminação de duplicação e compactação.

### Pode usar o Windows Admin Center para gerenciar espaços de armazenamento diretos para outros casos de uso (não hiperconvergido), como o servidor de arquivos de escalabilidade horizontal (SoFS) ou Microsoft SQL Server?

Windows Admin Center para a infraestrutura hiperconvergente não fornece gerenciamento ou monitoramento opções especificamente para outros casos de uso de espaços de armazenamento diretos – por exemplo, ele não pode criar compartilhamentos de arquivos. No entanto, os recursos de painel e core, como criar volumes ou substituindo unidades, funcionam para qualquer cluster de espaços de armazenamento diretos.

### Qual é a diferença entre um Cluster de Failover e um Cluster hiperconvergente?

Em geral, o termo "hiperconvergente" refere-se executando o Hyper-V e espaços de armazenamento diretos no mesmo servidores para virtualizar recursos de computação e armazenamento de cluster. No contexto do Windows Admin Center, quando você clica em **+ Adicionar** da lista de conexões, você pode escolher entre a adição de uma **conexão de Cluster de Failover** ou uma **conexão de Cluster hiperconvergente**:

- A **conexão de Cluster de Failover** é o sucessor para o aplicativo da área de trabalho do Gerenciador de Cluster de Failover. Ele fornece uma experiência de gerenciamento de finalidade geral, familiar cluster oferecer suporte a qualquer carga de trabalho, incluindo o Microsoft SQL Server. Ele está disponível para o Windows Server 2012 e versões posteriores.

- A **conexão de Cluster hiperconvergente** é uma nova experiência totalmente adaptada para espaços de armazenamento direto e Hyper-V. Contém recursos do Painel e destaca gráficos e alertas para monitoramento. Ele está disponível para Windows Server 2016 e Windows Server 2019.

### Por que preciso a atualização cumulativa mais recente para o Windows Server 2016?

Windows Admin Center para a infraestrutura hiperconvergente depende do gerenciamento de que APIs desenvolvido desde que o Windows Server 2016 foi lançada. Essas APIs são adicionadas a [atualização cumulativa para o Windows Server 2016 (KB4103723) de 2018-05](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponível a partir de 8 de maio de 2018.

### Quanto custa para usar o Windows Admin Center?

O Windows Admin Center não tem custo adicional além do Windows.

Você pode usar o Windows Admin Center (disponível como um download separado) com licenças válidas do Windows Server ou Windows 10 sem custo adicional - ele é licenciado sob um EULA complementar do Windows.

### O Windows Admin Center requer o System Center?

Não.

### Ele requer uma conexão de Internet?

Não.

Embora o Windows Admin Center oferece poderosos e conveniente integração com a nuvem do Microsoft Azure, o gerenciamento de core e monitoramento experiência para a infraestrutura hiperconvergente é completamente no local. Ele pode ser instalado e usado sem uma conexão de Internet.

## Coisas para tentar

Se você estiver apenas começando, aqui estão alguns tutoriais rápidos para ajudar você a saber como o Windows Admin Center para a infraestrutura hiperconvergente é organizado e funciona. Verifique exercício julgamento boa e tenha cuidado com ambientes de produção. Esses vídeos foram gravados com o Windows Admin Center versão 1804 e um build do Insider Preview do Windows Server 2019.

### Gerenciar volumes espaços de armazenamento diretos

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">como criar um volume de espelhamento de três vias</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">como criar um volume de paridade acelerada por espelho</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">como abrir um volume e adicionar arquivos</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">como ativar a eliminação de duplicação e compactação</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">como expandir um volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">como excluir um volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Criar volume, espelhamento de três vias</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Criar volume, paridade acelerada por espelho</strong>
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

### Criar uma nova máquina virtual

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** , clique em **novo** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre máquinas virtuais de geração 1 e 2.
4. Você pode escolher qual host para criar a máquina virtual no inicialmente ou usar o host recomendado.
5. Escolha um caminho para os arquivos da máquina virtual. Escolha um volume na lista suspensa ou clique em **Procurar** para escolher uma pasta usando o seletor de pasta. O arquivo do disco rígido virtual e arquivos de configuração de máquina virtual serão salvos em uma única pasta sob o `\Hyper-V\[virtual machine name]` caminho do volume selecionado ou caminho.
6. Escolha o número de processadores virtuais, se você deseja a virtualização aninhada habilitada, definir configurações de memória, adaptadores de rede, discos rígidos virtuais e escolha se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou da rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e aparece na lista de máquina virtual, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual é iniciada, você pode se conectar ao console da máquina virtual via VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais** > **Connect** para baixar o arquivo. rdp. Abra o arquivo. RDP no aplicativo da Conexão de área de trabalho remota. Já que isso está se conectando ao console da máquina virtual, você precisará inserir credenciais de administrador do host do Hyper-V.

[Saiba mais sobre o gerenciamento de máquina virtual com o Windows Admin Center](manage-virtual-machines.md).

### Pausar e reiniciar um servidor com segurança

1. No **painel**, selecione os **servidores** da navegação no lado esquerdo ou clicando no link **Exibir servidores gt _** no bloco no canto inferior direito do painel.
2. Na parte superior, alterne do **Resumo** para a guia **inventário** .
3. Selecione um servidor clicando em seu nome para abrir a página de detalhes do **servidor** .
4. Clique em **servidor de pausa para manutenção**. Se é seguro continuar, isso moverá máquinas virtuais para outros servidores no cluster. O servidor terá status esvaziamento enquanto isso acontece. Se você quiser, você pode assistir as máquinas virtuais mover na página **gt _ máquinas virtuais inventário** , onde o seu servidor host é mostrada claramente na grade. Quando tiver movido todas as máquinas virtuais, o status do servidor será **pausado**.
5. Clique em **Gerenciar servidor** para acessar todas as ferramentas de gerenciamento por servidor no Windows Admin Center.
6. Clique em **Reiniciar**, em seguida, **Sim**. Você vai devolvido à lista de conexões.
7. Novamente no **painel**, o servidor é vermelho enquanto ele está pressionado.
8. Após fazer backup, navegar novamente a página do **servidor** e clique em **retomar o servidor de manutenção** para definir o status do servidor para cima simplesmente. No momento, as máquinas virtuais moverá novamente – nenhuma ação do usuário é necessária.

### Substituir um disco com falha

1. Quando uma unidade falha, um alerta é exibido na área de **alertas** esquerda superior do **painel**.
2. Você também pode selecionar **unidades** da navegação no lado esquerdo ou clique no link de **unidades de modo de exibição gt _** no bloco no canto inferior direito para procurar unidades e ver o status de por conta própria. Na guia **inventário** , a grade dá suporte a classificação, agrupamento e pesquisa de palavra-chave.
3. No **painel**, clique no alerta para ver detalhes, como o local físico da unidade.
4. Para saber mais, clique no atalho **vá para a unidade** para a página de detalhes da **unidade** .
5. Se o hardware oferecer suporte a ele, você pode clicar em **Ativar/desativar a luz** para controlar a luz do indicador da unidade.
6. Espaços de armazenamento diretos automaticamente inutiliza e empurra unidades com falha. Quando isso acontecer, o status do disco está desativado, e sua barra de capacidade de armazenamento está vazia.
7. Remova a unidade com falha e insira sua substituição.
8. Em **unidades gt _ inventário**, a nova unidade será exibida. No momento, limpará o alerta, volumes irá reparar volta ao status Okey e armazenamento será rebalancear no novo disco – nenhuma ação do usuário é necessária.

### Gerenciar redes virtuais (HCI habilitado SDN clusters usando o Windows Admin Center Preview)

1. Selecione a navegação no lado esquerdo **Redes virtuais** .
2. Clique em **novo** para criar uma nova rede virtual e sub-redes, ou optar por uma rede virtual existente e clique em **configurações** para modificar a sua configuração.
3. Clique em uma rede virtual existente para exibir as conexões de VM para as sub-redes de rede virtual e acessar as listas de controle aplicadas a sub-redes de rede virtual.

![Gerenciar redes virtuais](../media/manage-hyper-converged/manage-virtual-networks.png)

### Conectar uma máquina virtual a uma rede virtual (HCI habilitado SDN clusters usando o Windows Admin Center Preview)

1. Selecione **as máquinas virtuais** a navegação no lado esquerdo.
2. Escolha um existente máquina virtual gt _ Click gt _ **configurações** abra a guia **redes** em **configurações**.
3. Configure os campos de **Rede Virtual** e **Sub-rede Virtual** para conectar-se a máquina virtual a uma rede virtual.

Você também pode configurar a rede virtual ao criar uma máquina virtual.

![Conectar uma máquina virtual a uma rede virtual](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### Infraestrutura de rede definida pelo Software de monitor (HCI habilitado SDN clusters usando o Windows Admin Center Preview)

1. Selecione **SDN monitoramento** a navegação no lado esquerdo.
2. Exibir informações detalhadas sobre a integridade do Gateway Virtual do controlador de rede, balanceador de carga de Software e monitorar o uso de Pool de Gateway Virtual, pública e privada Pool de IP e o status de host SDN.

![Monitore a infraestrutura SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## Comentários

Trata-se todos os seus comentários! O benefício mais importante das atualizações frequentes é ouvir que está funcionando e o que precisa ser melhorado. Aqui estão algumas maneiras para nos informar o que você está pensando:

- [Envie e vote em solicitações de recursos no UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Participe do Fórum do Windows Admin Center na Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet para `@servermgmt`

### Ver também

- [WindowsAdmin Center](../understand/windows-admin-center.md)
- [Espaços de Armazenamento Diretos](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Rede definida pelo software](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
