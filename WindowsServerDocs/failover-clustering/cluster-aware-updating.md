---
title: "Visão geral do suporte a cluster atualizando"
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: "Atualizando suporte a cluster (CAU) automatiza a instalação de atualização do software em clusters executando o Windows Server."
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>Visão geral do suporte a cluster atualizando

> Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma visão geral dos \(CAU\) atualizando Cluster\ reconhecimento, um recurso que automatiza o software atualizando o processo em servidores clusterizados enquanto mantém a disponibilidade.

> [!NOTE]
> Ao atualizar [direta de espaços de armazenamento](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, recomendamos usar o suporte a Cluster atualizando.
  
## <a name="BKMK_OVER"></a>Descrição dos recursos  
Suporte a cluster atualizando é um recurso automatizado que permite que você atualize servidores em uma [cluster de failover](failover-clustering-overview.md) com pouca ou nenhuma perda na disponibilidade durante o processo de atualização. Durante uma atualização de execução, a atualização de suporte a Cluster transparente executa as seguintes tarefas:  

1. Coloca cada nó de cluster no modo de manutenção do nó.
2. Move as funções clusterizadas desativar o nó.
3. Instala as atualizações e as atualizações dependentes.
4. Executa uma reinicialização, se necessário.
5. Traz o nó sair do modo de manutenção.
6. Restaura as funções no nó de cluster.
7. Se move para atualizar o próximo nó.

Para muitas funções clusterizadas no cluster, o processo de atualização automática dispara um failover planejado. Isso pode causar uma interrupção de serviço temporária para os clientes conectados. No entanto, no caso de cargas de trabalho continuamente disponíveis, como Hyper\-V na migração ao vivo ou servidor de arquivos com o Failover transparente SMB, suporte a Cluster atualizando pode coordenar o atualizações de cluster sem afetar a disponibilidade de serviço.    
  
## <a name="practical-applications"></a>Aplicativos práticos  
  
-   CAU reduz falhas no serviço nos serviços clusterizados, reduz a necessidade de manual atualizando soluções alternativas e torna o cluster end\ to\ ponta mais confiável do processo de atualização para o administrador. Quando o recurso CAU é usado em conjunto com cargas de trabalho de cluster continuamente disponíveis, como servidores de arquivos disponíveis continuamente \ (arquivo servidor carga de trabalho com Failover\ transparente SMB) ou Hyper\-V, o cluster atualizações podem ser executadas com impacto zero a disponibilidade de serviços para clientes.  
  
-   CAU facilita a adoção de processos de TI consistentes em toda a empresa. Perfis de executar a atualização pode ser criada para diferentes classes de clusters de failover e gerenciada centralmente em um compartilhamento de arquivo para garantir que implantações CAU em toda a organização de TI aplicam atualizações de forma consistente, mesmo se o clusters são gerenciados por diferentes lines\-of\-business ou administradores.  
  
-   CAU pode agendar atualização executa em intervalos regulares de mensais, diários ou semanais para ajudar a coordenar as atualizações do cluster com outros processos de gerenciamento de TI.  
  
-   CAU fornece uma arquitetura extensível para atualizar o inventário de software de cluster de maneira cluster\ reconhecimento. Isso pode ser usado pelos fornecedores coordene a instalação de atualizações de software que não são publicados no Windows Update ou o Microsoft Update ou que não estão disponíveis na Microsoft, por exemplo, atualizações de drivers de dispositivo non\ da Microsoft.  
  
-   O modo de atualização self\ CAU permite que um dispositivo "em uma caixa de cluster" \ (um conjunto de computadores físicos clusterizados, geralmente são empacotados em um chassis\) para atualizar a próprio. Normalmente, esses dispositivos são implantados em filiais com suporte mínimo de TI local para gerenciar os clusters. Atualizando Self\ modo oferece excelente valor nesses cenários de implantação.  
  
## <a name="important-functionality"></a>Funcionalidade importante  
A seguir está uma descrição da funcionalidade de reconhecimento de Cluster atualizando importante:  
  
-   Uma interface de usuário \(UI\) - a janela Atualizar ciente do Cluster - e um conjunto de cmdlets que você pode usar para visualizar, aplicar, monitorar e relatar as atualizações  
  
-   Uma automação end\ to\ ponta da operação de atualização cluster\ \ coordenadas (uma atualização Run\), por um ou mais computadores de coordenador de atualização  
  
-   Um padrão plug\-no que se integra com o existente \(WUA\) agente do Windows Update e Windows Server Update Services \(WSUS\) infraestrutura no Windows Server para aplicar atualizações importantes do Microsoft  
  
-   Um segundo plug\-in que podem ser usados para aplicar hotfixes da Microsoft e que pode ser personalizado para aplicar atualizações de non\ da Microsoft  
  
-   Atualizando executar os perfis que você configurar com as configurações de atualização executar opções, como o número máximo de vezes que a atualização será repetida por nó. Atualizando executar perfis permitem reutilizar rapidamente as mesmas configurações entre execuções de atualização e compartilhe facilmente as configurações de atualização com outros clusters de failover.  
  
-   Uma arquitetura extensível que dê suporte ao desenvolvimento de plug\ no novo para coordenar outras ferramentas de atualização node\ no cluster, como instaladores de software personalizado, ferramentas, a atualização do BIOS e adaptador host barramento adaptador \(HBA\) atualização ferramentas ou de rede.  
  
Suporte a cluster atualizando pode coordenar o cluster completo atualizando operação em dois modos:  
  
-   **Modo de atualização Self\** nesse modo, a função clusterizada CAU é configurada como uma carga de trabalho no cluster de failover será atualizado e um agendamento de atualização associado é definido. Atualiza o cluster em si em horários agendados usando um padrão ou um perfil personalizado atualizando executar. Durante a atualização de execução, o processo de coordenador de atualização CAU começa no nó que atualmente possui a função clusterizada CAU e o processo sequencialmente executa atualizações em cada nó de cluster. Para atualizar o nó de cluster atual, a função clusterizada CAU faz failover para outro nó de cluster e um novo processo de atualização coordenador no nó assume o controle de executar a atualização. No modo de atualização self\, CAU pode atualizar o cluster de failover usando um processo de atualização end\ to\ ponta totalmente automatizado. Um administrador também pode gatilho atualiza demanda on\ nesse modo, ou simplesmente usar a atualização remote\ abordagem se desejado. No modo de atualização self\, um administrador pode obter informações de resumo sobre como executar uma atualização em andamento conectando-se ao cluster e executando o **Get\ CauRun** cmdlet do Windows PowerShell.  
  
-   **Modo de atualização Remote\** para esse modo, um computador remoto, que é chamado um coordenador de atualização, é configurado com as ferramentas CAU. O coordenador de atualização não é um membro do cluster que é atualizado durante a executar a atualização. No computador remoto, o administrador dispara on\ demanda atualizando executar usando um padrão ou um perfil personalizado atualizando executar. Atualizando Remote\ modo é útil para monitorar o progresso de tempo de real\ durante a atualização executar e para clusters que estão sendo executados em instalações Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisitos de hardware e software  

CAU pode ser usado em todas as edições do Windows Server, inclusive instalações Server Core. Para obter informações de requisitos detalhados, consulte [práticas recomendadas e requisitos de suporte a Cluster Atualizando](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Instalando, atualizando suporte a Cluster
Para usar CAU, instale o recurso de cluster de Failover no Windows Server e crie um cluster de failover. Os componentes que dão suporte à funcionalidade CAU são instalados automaticamente em cada nó de cluster.

Para instalar o recurso de cluster de Failover, você pode usar as ferramentas a seguir:
- Adição de funções e recursos de assistente no Gerenciador do servidor
- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) cmdlet do Windows PowerShell
- Ferramenta de linha de comando DISM (gerenciamento) e manutenção de imagens de implantação

Para obter mais informações, consulte [instalar ou desinstalar funções, serviços de função ou recursos](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx).

Você também deve instalar as ferramentas de cluster de Failover, que fazem parte das ferramentas de administração de servidor remoto e são instaladas por padrão quando você instala o recurso de cluster de Failover no Gerenciador do servidor. As ferramentas de cluster de Failover incluem o suporte a Cluster atualizando a interface do usuário e cmdlets do PowerShell. 

Você deve instalar as ferramentas de cluster de Failover da seguinte maneira para dar suporte os diferentes modos de atualização CAU:

- Para usar CAU no modo de atualização self\, instale as ferramentas de cluster de Failover em cada nó de cluster.   
  
- Para habilitar o modo de atualização remote\, instale as ferramentas de cluster de Failover em um computador que tenha conectividade de rede ao cluster de failover.  
  
> [!NOTE]  
> -   Você não pode usar as ferramentas de cluster de Failover no Windows Server 2012 para gerenciar o suporte a Cluster atualizando em uma versão mais recente do Windows Server. 
> -   Para usar CAU somente no modo de atualização remote\, a instalação das ferramentas de cluster de Failover em nós de cluster não é necessária. No entanto, alguns recursos CAU não estarão disponíveis. Para obter mais informações, consulte [requisitos e as práticas recomendadas para a atualização de reconhecimento de Cluster\](cluster-aware-updating-requirements.md).  
> -   Se você estiver usando CAU somente no modo de atualização self\, o computador no qual as ferramentas CAU são instaladas e que as atualizações de coordenadas não pode ser um membro do cluster de failover.  
  
### <a name="enabling-self-updating-mode"></a>Ativando o modo de atualização automática
Para habilitar o modo de atualização automática, você deve adicionar a função clusterizada atualizando suporte a Cluster ao cluster de failover. Para fazer isso, use um dos seguintes métodos:
- No Gerenciador do servidor, selecione **ferramentas** > **atualizando suporte a Cluster**, na janela atualizar suporte a Cluster, selecione **cluster configurar opções de atualização automática**. 
- Em uma sessão do PowerShell, execute o [CauClusterRole adicionar](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet.  
  
Para desinstalar CAU, desinstale o recurso de cluster de Failover ou ferramentas de cluster de Failover usando o Gerenciador do servidor, o [desinstalar-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature) cmdlet, ou as ferramentas de linha de command\ do DISM.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisitos adicionais e práticas recomendadas  

Para garantir que CAU pode atualizar os nós de cluster com êxito e, para obter diretrizes adicionais para configurar seu ambiente de cluster de failover para usar CAU, você pode executar o analisador de práticas recomendadas CAU.  
  
Para requisitos detalhados e práticas recomendadas para usar CAU e informações sobre como executar o analisador de práticas recomendadas CAU, consulte [requisitos e as práticas recomendadas para a atualização de reconhecimento de Cluster\](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>A partir do suporte a Cluster atualizando  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Para iniciar a atualização de suporte a Cluster do Gerenciador do servidor  
  
1.  Inicie o Gerenciador do servidor.  
  
2.  Siga um destes procedimentos:  
  
    -   Sobre o **ferramentas** menu, clique em **atualizando Cluster\ reconhecimento**.  
  
    -   Se um ou mais nós de cluster ou cluster, é adicionado ao Gerenciador do servidor, o **todos os servidores** de página, clique right\ o nome de um nó \ (ou o nome do cluster\) e, em seguida, clique em **atualização Cluster**.  
  
## <a name="see-also"></a>Consulte também  
Os links a seguir fornecem mais informações sobre como usar o suporte a Cluster atualizando.  
  
-   [Requisitos e as práticas recomendadas para atualizar Cluster\ reconhecimento](cluster-aware-updating.md)  
  
-   [Atualizando Cluster\ reconhecimento: Perguntas frequentes](cluster-aware-updating-faq.md)  
  
-   [Opções avançadas e atualizar perfis de execução para CAU](cluster-aware-updating-options.md)  
  
-   [Como funcionam CAU Plug\-ins](cluster-aware-updating-plug-ins.md)  
  
-   [Reconhecimento de Cluster\ Cmdlets de atualização no Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Cluster\ voltada para atualizar a referência de Plug\](https://msdn.microsoft.com/library/hh418084.aspx)  
  

