---
title: Visão geral da Atualização com Suporte a Cluster
description: A CAU (atualização com suporte a cluster) automatiza a instalação de atualização de software em clusters que executam o Windows Server.
ms.topic: article
ms.prod: windows-server
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 6a2b6ad06b8a003f9cbf020956994b08cb8cf194
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827989"
---
# <a name="cluster-aware-updating-overview"></a>Visão geral da Atualização com Suporte a Cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma visão geral da atualização de\-com reconhecimento de cluster \(CAU\), um recurso que automatiza o processo de atualização de software em servidores clusterizados, mantendo a disponibilidade.

> [!NOTE]
> Ao atualizar [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, é recomendável usar a atualização com suporte a cluster.
  
## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso  
A atualização com suporte a cluster é um recurso automatizado que permite atualizar servidores em um [cluster de failover](failover-clustering-overview.md) com pouca ou nenhuma perda na disponibilidade durante o processo de atualização. Durante uma execução de atualização, a atualização com suporte a cluster executa as seguintes tarefas de forma transparente:  

1. Coloca cada nó do cluster no modo de manutenção do nó.
2. Move as funções clusterizadas para fora do nó.
3. Instala as atualizações e quaisquer atualizações dependentes.
4. Executa uma reinicialização, se necessário.
5. Coloca o nó fora do modo de manutenção.
6. Restaura as funções clusterizadas no nó.
7. Move para atualizar o próximo nó.

Para várias funções clusterizadas no cluster, o processo de atualização automática dispara um failover planejado. Isso pode provocar uma interrupção de serviço transitória nos clientes conectados. No entanto, no caso de cargas de trabalho continuamente disponíveis, como o Hyper\-V com migração dinâmica ou servidor de arquivos com failover transparente de SMB, a atualização com suporte a cluster pode coordenar atualizações de cluster sem afetar a disponibilidade do serviço.    
  
## <a name="practical-applications"></a>Aplicações práticas  
  
-   A CAU reduz as interrupções de serviço em serviços clusterizados, reduz a necessidade de soluções alternativas de atualização manual e torna o\-final\-processo de atualização de cluster final mais confiável para o administrador. Quando o recurso CAU é usado em conjunto com as cargas de trabalho de cluster disponíveis continuamente, como servidores de arquivos continuamente disponíveis \(carga de trabalho do servidor de arquivos com o failover transparente do SMB\) ou o Hyper\-V, as atualizações de cluster podem ser executadas com impacto zero na disponibilidade do serviço para clientes.  
  
-   A CAU facilita a adoção de processos de TI consistentes em toda a empresa. A atualização de perfis de execução pode ser criada para diferentes classes de clusters de failover e, em seguida, gerenciada centralmente em um compartilhamento de arquivos para garantir que as implantações de CAU em toda a organização de ti apliquem atualizações de forma consistente, mesmo que os clusters sejam gerenciados por diferentes linhas\-de\-negócios ou administradores.  
  
-   A CAU pode agendar as Execuções de Atualização diariamente, semanalmente ou mensalmente para coordenar as atualizações de cluster com outros processos de gerenciamento de TI.  
  
-   A CAU fornece uma arquitetura extensível para atualizar o inventário de software de cluster em um cluster\-de modo ciente. Isso pode ser usado por editores para coordenar a instalação de atualizações de software que não são publicadas em Windows Update ou Microsoft Update ou que não estão disponíveis na Microsoft, por exemplo, atualizações de drivers de dispositivo não\-Microsoft.  
  
-   O modo de atualização de auto\-da CAU permite que um dispositivo "cluster pronto" \(um conjunto de máquinas físicas clusterizadas, normalmente empacotadas em um chassi\) para atualizar a si mesma. Normalmente, esses dispositivos são implantados nas filiais locais com suporte mínimo de TI para gerenciar os clusters. O modo de atualização de auto\-oferece um ótimo valor nesses cenários de implantação.  
  
## <a name="important-functionality"></a>Funcionalidade importante  
Veja a seguir uma descrição da funcionalidade de atualização importante com suporte a cluster:  
  
-   Uma interface do usuário \(UI\)-a janela de atualização com reconhecimento de cluster-e um conjunto de cmdlets que você pode usar para visualizar, aplicar, monitorar e relatar as atualizações  
  
-   Um\-final para\-a automação final do cluster\-operação de atualização \(uma execução de atualização\), orquestrada por um ou mais computadores do coordenador de atualização  
  
-   Um plug\-padrão no que se integra ao agente Windows Update existente \(WUA\) e Windows Server Update Services \(a infraestrutura\) do WSUS no Windows Server para aplicar atualizações importantes da Microsoft  
  
-   Um segundo plug\-no que pode ser usado para aplicar hotfixes da Microsoft e que pode ser personalizado para aplicar atualizações não\-Microsoft  
  
-   Perfis de Execução de Atualização que você configura com as definições das opções de Execução de Atualização, como o número máximo de vezes que a atualização será repetida por nó. Os Perfis de Execução de Atualização permitem que você reutilize rapidamente as mesmas configurações nas Execuções de Atualização e compartilhe facilmente as configurações de atualização com outros clusters de failover.  
  
-   Uma arquitetura extensível que dá suporte a novos plug\-no desenvolvimento para coordenar outros\-ferramentas de atualização de nó no cluster, como instaladores de software personalizados, ferramentas de atualização de BIOS e adaptador de rede ou adaptador de barramento de host \(ferramentas de atualização de HBA\).  
  
A atualização com suporte a cluster pode coordenar a operação de atualização de cluster completa em dois modos:  
  
-   **Modo de atualização de auto\-** Para esse modo, a função clusterizada CAU é configurada como uma carga de trabalho no cluster de failover que deve ser atualizada e uma agenda de atualização associada é definida. O cluster se atualiza em horários agendados usando um padrão ou um perfil personalizado para Execução de Atualização. Durante a Execução de Atualização, o processo do Coordenador de Atualização da CAU é iniciado no nó que possui a função clusterizada da CAU e executa as atualizações em cada nó do cluster sequencialmente. Para atualizar o nó do cluster atual, a função clusterizada da CAU faz failover para outro nó do cluster, e um novo processo do Coordenador de Atualização nesse nó assume o controle da Execução de Atualização. No modo de atualização de auto\-, a CAU pode atualizar o cluster de failover usando um\-final totalmente automatizado para\-finalizar o processo de atualização. Um administrador também pode disparar atualizações em\-demanda nesse modo ou simplesmente usar a abordagem de atualização de\-remota, se desejado. No modo de atualização de auto\-, um administrador pode obter informações resumidas sobre uma execução de atualização em andamento, conectando-se ao cluster e executando o cmdlet **get\-CauRun** do Windows PowerShell.  
  
-   **Modo de atualização de\-remoto** Para esse modo, um computador remoto, que é chamado de coordenador de atualização, é configurado com as ferramentas de CAU. O Coordenador de Atualização não é um membro do cluster que é atualizado durante a Execução de Atualização. No computador remoto, o administrador dispara uma execução de atualização de\-demanda usando um perfil de execução de atualização padrão ou personalizada. O modo de atualização de\-remoto é útil para monitorar o andamento real\-tempo durante a execução da atualização e para clusters em execução em instalações do Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisitos de hardware e software  

A CAU pode ser usada em todas as edições do Windows Server, incluindo instalações do Server Core. Para obter informações sobre requisitos detalhados, consulte [requisitos de atualização com suporte a cluster e práticas recomendadas](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalando a atualização com suporte a cluster
Para usar a CAU, instale o recurso de clustering de failover no Windows Server e crie um cluster de failover. Os componentes que suportam a funcionalidade CAU são instalados automaticamente em cada nó do cluster.

Para instalar o recurso Clustering de Failover, você pode usar as seguintes ferramentas:
- Adicionar assistente de funções e recursos no Gerenciador de Servidores
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) cmdlet do Windows PowerShell
- Ferramenta de linha de comando de Gerenciamento e Manutenção de Imagens de Implantação (DISM)

Para obter mais informações, consulte [instalar o recurso de cluster de failover](create-failover-cluster.md#install-the-failover-clustering-feature).

Você também deve instalar as ferramentas de clustering de failover, que fazem parte do Ferramentas de Administração de Servidor Remoto e são instaladas por padrão quando você instala o recurso de clustering de failover no Gerenciador do Servidor. As ferramentas de clustering de failover incluem a interface do usuário de atualização com suporte a cluster e os cmdlets do PowerShell. 

Você deve instalar as Ferramentas de Clustering de Failover da seguinte maneira para dar suporte aos diferentes modos de atualização da CAU:

- Para usar a CAU no modo de atualização de auto\-, instale as ferramentas de clustering de failover em cada nó de cluster.   
  
- Para habilitar o modo de atualização de\-remoto, instale as ferramentas de clustering de failover em um computador que tenha conectividade de rede com o cluster de failover.  
  
> [!NOTE]  
> -   Você não pode usar as ferramentas de clustering de failover no Windows Server 2012 para gerenciar a atualização com suporte a cluster em uma versão mais recente do Windows Server. 
> -   Para usar a CAU somente no modo de atualização de\-remota, a instalação das ferramentas de clustering de failover nos nós de cluster não é necessária. No entanto, alguns recursos da CAU não estarão disponíveis. Para obter mais informações, consulte [requisitos e práticas recomendadas para atualização com reconhecimento de\-de cluster](cluster-aware-updating-requirements.md).  
> -   A menos que você esteja usando a CAU somente no modo de atualização de auto\-, o computador no qual as ferramentas de CAU estão instaladas e que coordena as atualizações não pode ser um membro do cluster de failover.  
  
### <a name="enabling-self-updating-mode"></a>Habilitando o modo de autoatualização
Para habilitar o modo de autoatualização, você deve adicionar a função clusterizada de atualização com suporte de cluster ao cluster de failover. Para fazer isso, use um dos seguintes métodos:
- Em Gerenciador do Servidor, selecione **ferramentas** > **atualização com suporte a cluster**e, em seguida, na janela de atualização com suporte a cluster, selecione **Configurar opções de autoatualização de cluster**. 
- Em uma sessão do PowerShell, execute o cmdlet [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) .  
  
Para desinstalar a CAU, desinstale o recurso de clustering de failover ou as ferramentas de clustering de failover usando Gerenciador do Servidor, o cmdlet [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) ou o comando DISM\-ferramentas de linha.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisitos adicionais e práticas recomendadas  

Para que a CAU atualize os nós do cluster com êxito, e para obter mais diretrizes sobre como configurar o ambiente de cluster de failover para usar a CAU, execute o Analisador de Práticas Recomendadas da CAU.  
  
Para obter requisitos detalhados e práticas recomendadas para usar a CAU e informações sobre como executar o Analisador de Práticas Recomendadas CAU, consulte [requisitos e práticas recomendadas para atualização com reconhecimento de\-de cluster](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Iniciando atualização com suporte a cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para iniciar a atualização com suporte a cluster de Gerenciador do Servidor  
  
1.  Inicie o Gerenciador do Servidor.  
  
2.  Siga um destes procedimentos:  
  
    -   No menu **ferramentas** , clique em **cluster\-atualização de reconhecimento**.  
  
    -   Se um ou mais nós de cluster, ou o cluster, for adicionado ao Gerenciador do Servidor, na página **todos os servidores** , com o botão direito\-clique no nome de um nó \(ou o nome do\)de cluster e clique em **Atualizar cluster**.  
  
## <a name="see-also"></a>Consulte também  
Os links a seguir fornecem mais informações sobre como usar a atualização com suporte a cluster.  
  
-   [Requisitos e práticas recomendadas para Atualização com Suporte a Cluster\-  
  
-   [Atualização com reconhecimento de\-de cluster: perguntas frequentes](cluster-aware-updating-faq.md)  
  
-   [Opções avançadas e atualizando perfis de execução para a CAU](cluster-aware-updating-options.md)  
  
-   [Como funcionam os plug\-ins da CAU](cluster-aware-updating-plug-ins.md)  
  
-   [Cmdlets de atualização com reconhecimento de\-de cluster no Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [\-de plug-in de atualização de\-com reconhecimento de cluster em referência](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

