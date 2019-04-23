---
title: Visão geral da Atualização com Suporte a Cluster
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Atualização com suporte a cluster (CAU) automatiza a instalação de atualização de software em clusters que executam o Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827417"
---
# <a name="cluster-aware-updating-overview"></a>Visão geral da Atualização com Suporte a Cluster

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma visão geral do Cluster\-atualização com suporte \(CAU\), servidores de cluster de um recurso que automatiza o processo de atualização de software, mantendo a disponibilidade.

> [!NOTE]
> Ao atualizar [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, é recomendável usar a atualização com suporte a Cluster.
  
## <a name="BKMK_OVER"></a>Descrição do recurso  
Cluster-Aware Updating é um recurso automatizado que permite que você atualize os servidores em um [cluster de failover](failover-clustering-overview.md) com pouca ou nenhuma perda de disponibilidade durante o processo de atualização. Durante uma execução de atualização, a atualização com suporte a Cluster transparentemente executa as seguintes tarefas:  

1. Coloca cada nó do cluster no modo de manutenção de nó.
2. Move as funções clusterizadas do nó.
3. Instala as atualizações e as atualizações dependentes.
4. Executa uma reinicialização, se necessário.
5. Coloca o nó do modo de manutenção.
6. Restaura as funções clusterizadas no nó.
7. Move para atualizar o próximo nó.

Para várias funções clusterizadas no cluster, o processo de atualização automática dispara um failover planejado. Isso pode provocar uma interrupção de serviço transitória nos clientes conectados. No entanto, no caso de cargas de trabalho continuamente disponíveis, como Hyper\-V com o servidor de arquivo ou a migração ao vivo com Failover transparente do SMB, Cluster-Aware Updating pode coordenar as atualizações de cluster sem afetar a disponibilidade do serviço.    
  
## <a name="practical-applications"></a>Aplicações práticas  
  
-   CAU minimiza interrupções nos serviços em cluster, reduz a necessidade de manuais de atualização de soluções alternativas e faz o fim\-para\-mais confiável do processo de atualização para o administrador de cluster de end. Quando o recurso CAU é usado em conjunto com cargas de trabalho de cluster continuamente disponíveis, como servidores de arquivos disponíveis continuamente \(carga de trabalho de servidor de arquivos com Failover transparente do SMB\) ou Hyper\-V, o cluster as atualizações podem ser executadas sem nenhum impacto na disponibilidade do serviço para clientes.  
  
-   A CAU facilita a adoção de processos de TI consistentes em toda a empresa. Atualizar perfis de execução pode ser criado para diferentes classes de clusters de failover e gerenciado centralmente em um compartilhamento de arquivos para garantir que as implantações da CAU em toda a organização de TI apliquem as atualizações de forma consistente, mesmo se os clusters são gerenciados por diferentes linhas \-de\-negócios ou administradores.  
  
-   A CAU pode agendar as Execuções de Atualização diariamente, semanalmente ou mensalmente para coordenar as atualizações de cluster com outros processos de gerenciamento de TI.  
  
-   A CAU fornece uma arquitetura extensível para atualizar o inventário de software em um cluster\-com suporte a cluster. Isso pode ser usado por editores para coordenar a instalação das atualizações de software que não são publicados para o Windows Update ou Microsoft Update ou que não estão disponíveis na Microsoft, por exemplo, as atualizações para não\-drivers de dispositivo do Microsoft.  
  
-   A CAU self\-modo de atualização permite que um dispositivo de "cluster em uma caixa" \(um conjunto de computadores físicos clusterizados, normalmente empacotados sob um chassi\) atualize a mesmo. Normalmente, esses dispositivos são implantados nas filiais locais com suporte mínimo de TI para gerenciar os clusters. Self\-modo de atualização oferece um ótimo valor nesses cenários de implantação.  
  
## <a name="important-functionality"></a>Funcionalidade importante  
A seguir está uma descrição de funcionalidades importantes da atualização com suporte a Cluster:  
  
-   Uma interface do usuário \(interface do usuário\) -a janela de atualização com suporte do Cluster – e um conjunto de cmdlets que você pode usar para visualizar, aplicar, monitorar e relatar sobre atualizações  
  
-   Um término\-para\-terminar a automação do cluster\-operação de atualização \(uma execução de atualização\), organizada por um ou mais computadores coordenador de atualização  
  
-   Um padrão plug\-em que se integra com o Windows Update Agent existente \(WUA\) e o Windows Server Update Services \(WSUS\) infraestrutura no Windows Server para aplicar o importante a Microsoft atualizações  
  
-   Um segundo plug\-em que pode ser usado para aplicar hotfixes da Microsoft e que pode ser personalizado para aplicar não\-atualizações da Microsoft  
  
-   Perfis de Execução de Atualização que você configura com as definições das opções de Execução de Atualização, como o número máximo de vezes que a atualização será repetida por nó. Os Perfis de Execução de Atualização permitem que você reutilize rapidamente as mesmas configurações nas Execuções de Atualização e compartilhe facilmente as configurações de atualização com outros clusters de failover.  
  
-   Uma arquitetura extensível que dá suporte a novos plug\-no desenvolvimento para coordenar a outro nó\-atualizando ferramentas em todo o cluster, como instaladores de software personalizadas, BIOS atualizar ferramentas e o adaptador de rede ou adaptador de barramento do host \( HBA\) ferramentas de atualização.  
  
Cluster-Aware Updating pode coordenar o operação em dois modos de atualização de cluster completa:  
  
-   **Self\-modo de atualização** nesse modo, a função clusterizada CAU é configurada como uma carga de trabalho no cluster de failover que deve ser atualizada e uma agenda de atualização associada é definida. O cluster se atualiza em horários agendados usando um padrão ou um perfil personalizado para Execução de Atualização. Durante a execução de atualização, o processo do coordenador de atualização do CAU começa no nó que atualmente possui a função clusterizada CAU e o processo executa sequencialmente atualizações em cada nó do cluster. Para atualizar o nó do cluster atual, a função clusterizada da CAU faz failover para outro nó do cluster, e um novo processo do Coordenador de Atualização nesse nó assume o controle da Execução de Atualização. Em self\-modo de atualização, a CAU pode atualizar o cluster de failover usando um totalmente automatizada, finalizar\-para\-final ao processo de atualização. Um administrador também pode disparar atualizações em\-demanda nesse modo, ou simplesmente usar o controle remoto\-atualizando abordagem, se desejado. Em self\-modo de atualização, um administrador pode obter informações resumidas sobre uma execução de atualização em andamento conectando-se ao cluster e executando o **Obtenha\-CauRun** cmdlet do Windows PowerShell.  
  
-   **Remoto\-modo de atualização** nesse modo, um computador remoto, que é chamado de coordenador de atualização, é configurado com as ferramentas da CAU. O Coordenador de Atualização não é um membro do cluster que é atualizado durante a Execução de Atualização. No computador remoto, o administrador dispara uma em\-exigem a execução de atualização usando um padrão ou perfil de execução de atualização personalizada. Remoto\-modo de atualização é útil para monitorar a real\-andamento de tempo durante a execução de atualização e para clusters que estão sendo executados em instalações Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisitos de hardware e software  

A CAU pode ser usado em todas as edições do Windows Server, incluindo instalações do Server Core. Para obter informações detalhadas de requisitos, consulte [Cluster-Aware Updating requisitos e práticas recomendadas](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalar a atualização com suporte a Cluster
Para usar a CAU, instale o recurso de cluster de Failover no Windows Server e criar um cluster de failover. Os componentes que suportam a funcionalidade CAU são instalados automaticamente em cada nó do cluster.

Para instalar o recurso Clustering de Failover, você pode usar as seguintes ferramentas:
- Adicionar assistente de funções e recursos no Gerenciador de Servidores
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) cmdlet do Windows PowerShell
- Ferramenta de linha de comando de Gerenciamento e Manutenção de Imagens de Implantação (DISM)

Para obter mais informações, consulte [instalar o recurso de Clustering de Failover](create-failover-cluster.md#install-the-failover-clustering-feature).

Você também deve instalar as ferramentas de Clustering de Failover, que fazem parte das ferramentas de administração de servidor remoto e são instalados por padrão quando você instala o recurso de cluster de Failover no Gerenciador do servidor. As ferramentas de Clustering de Failover incluem a interface do usuário de Cluster-Aware Updating e cmdlets do PowerShell. 

Você deve instalar as Ferramentas de Clustering de Failover da seguinte maneira para dar suporte aos diferentes modos de atualização da CAU:

- Para usar o CAU no próprio\-atualizando modo, instalar as ferramentas de Clustering de Failover em cada nó de cluster.   
  
- Para habilitar remoto\-atualizando modo, instalar as ferramentas de Clustering de Failover em um computador que tenha conectividade de rede para o cluster de failover.  
  
> [!NOTE]  
> -   Você não pode usar as ferramentas de Clustering de Failover no Windows Server 2012 para gerenciar o Cluster-Aware Updating em uma versão mais recente do Windows Server. 
> -   Para usar o CAU apenas no remoto\-modo de atualização, a instalação das ferramentas de Clustering de Failover em nós de cluster não é necessária. No entanto, alguns recursos da CAU não estarão disponíveis. Para obter mais informações, consulte [requisitos e práticas recomendadas para o Cluster\-atualização com suporte a](cluster-aware-updating-requirements.md).  
> -   A menos que você estiver usando o CAU apenas no próprio\-modo de atualização, o computador no qual as ferramentas CAU estão instaladas e que coordena as atualizações não pode ser um membro do cluster de failover.  
  
### <a name="enabling-self-updating-mode"></a>Habilitando o modo de AutoAtualização
Para habilitar o modo de AutoAtualização, você deve adicionar a função clusterizada de atualização com suporte a Cluster para o cluster de failover. Para fazer isso, use um dos seguintes métodos:
- No Gerenciador do servidor, selecione **ferramentas** > **Cluster-Aware Updating**, em seguida, na janela de atualização com suporte a Cluster, selecione **deopçõesdeAutoAtualizaçãodeclusterdeconfigurar**. 
- Em uma sessão do PowerShell, execute as [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) cmdlet.  
  
Para desinstalar a CAU, desinstale o recurso cluster de Failover ou as ferramentas de Clustering de Failover usando o Gerenciador do servidor, o [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) cmdlet, ou o comando DISM\-ferramentas de linha.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisitos adicionais e práticas recomendadas  

Para que a CAU atualize os nós do cluster com êxito, e para obter mais diretrizes sobre como configurar o ambiente de cluster de failover para usar a CAU, execute o Analisador de Práticas Recomendadas da CAU.  
  
Para obter requisitos detalhados e práticas recomendadas para usar CAU, além de informações sobre como executar o analisador de práticas recomendadas do CAU, consulte [requisitos e práticas recomendadas para o Cluster\-atualização com suporte a](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Iniciando a atualização com suporte a Cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para iniciar a atualização com suporte a Cluster no Gerenciador do servidor  
  
1.  Inicie o Gerenciador do Servidor.  
  
2.  Siga um destes procedimentos:  
  
    -   Sobre o **ferramentas** menu, clique em **Cluster\-atualização com suporte a**.  
  
    -   Se um ou mais nós de cluster ou o cluster, é adicionado ao Gerenciador do servidor, o **todos os servidores** página direita\-clique no nome de um nó \(ou o nome do cluster\)e, em seguida, clique em  **Atualizar Cluster**.  
  
## <a name="see-also"></a>Consulte também  
Os links a seguir fornecem mais informações sobre como usar a atualização com suporte a Cluster.  
  
-   [Requisitos e práticas recomendadas para o Cluster\-atualização com suporte](cluster-aware-updating.md)  
  
-   [Cluster\-atualização com suporte: Perguntas frequentes](cluster-aware-updating-faq.md)  
  
-   [Opções avançadas e perfis de execução atualização da CAU](cluster-aware-updating-options.md)  
  
-   [Como conectar o CAU\-ins trabalho](cluster-aware-updating-plug-ins.md)  
  
-   [Cluster\-Cmdlets do Windows PowerShell de atualização com suporte](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Cluster\-Plug de atualização com suporte\-na referência](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

