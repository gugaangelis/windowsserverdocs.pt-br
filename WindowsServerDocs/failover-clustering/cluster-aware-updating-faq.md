---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: Suporte a cluster atualizando - perguntas frequentes
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: "Respostas para perguntas frequentes sobre a atualização de suporte a Cluster no Windows Server."
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Suporte a cluster atualizando: Perguntas frequentes

> Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Suporte a cluster Atualizando](cluster-aware-updating.md) \(CAU\) é um recurso que organize atualizações de software em todos os servidores em um cluster de failover de forma que não afeta a disponibilidade de serviço mais um failover planejado de um nó de cluster. Para alguns aplicativos com recursos de disponibilidade contínua \ como (Hyper\-V na migração ao vivo), ou um servidor de arquivos do SMB 3. x com Failover\ transparente SMB, CAU pode coordenar cluster automatizada atualizando sem causar impacto sobre a disponibilidade de serviço.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU dá suporte a atualização clusters direta de espaços de armazenamento?  
Sim. CAU dá suporte à atualização [direta de espaços de armazenamento](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters independentemente do tipo de implantação: hyper convergidos ou convergida. Especificamente, coordenação CAU garante que suspender cada nó de cluster aguarda o espaço de armazenamento de cluster subjacente ser comprovada.

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>CAU funciona com o Windows Server 2008 R2 ou Windows 7?  
Não. CAU coordenadas do cluster atualizando operação somente de computadores que executam o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 ou Windows 8. O cluster de failover sendo atualizado deve executar o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU é limitada a aplicativos clusterizados específicos?  
Não. CAU é independente para o tipo do aplicativo clusterizado. CAU é uma solução de atualização cluster\ externa que encontra-se em cima de APIs e PowerShell cmdlets de cluster. Dessa forma, CAU pode coordenar atualizando para qualquer aplicativo clusterizado que está configurado em um cluster de failover do Windows Server.  
  
> [!NOTE]  
> Atualmente, as cargas de trabalho clusterizadas seguintes são testadas e certificadas para CAU: SMB, Hyper\-V, replicação do DFS, Namespaces DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU dá suporte a atualizações do Microsoft Update e o Windows Update?  
Sim. Por padrão, CAU está configurado com um plug\-in que usa os APIs de utilitários do Windows Update Agent \(WUA\) em nós de cluster. A infraestrutura WUA pode ser configurada para apontar para o Microsoft Update e o Windows Update ou Windows Server Update Services \(WSUS\) como fonte de atualizações.  
  
## <a name="does-cau-support-wsus-updates"></a>CAU dá suporte a atualizações do WSUS?  
Sim. Por padrão, CAU está configurado com um plug\-in que usa os APIs de utilitários do Windows Update Agent \(WUA\) em nós de cluster. A infraestrutura WUA pode ser configurada para apontar para o Microsoft Update e o Windows Update ou para um servidor Windows Server Update Services \(WSUS\) local como fonte de atualizações.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU pode aplicar atualizações de versão de distribuição limitada?  
Sim. Atualizações \(LDR\) de versão de distribuição limitada, também chamadas de hotfixes, não são publicadas por meio do Microsoft Update ou o Windows Update, portanto, eles não podem ser baixados por \(WUA\) plug\ o Windows Update Agent-nesse CAU usa por padrão.  
  
No entanto, CAU inclui um segundo plug\-no que você pode optar por aplicar as atualizações de hotfix. Esse hotfix plug\-in também pode ser personalizado para aplicar atualizações de BIOS, firmware e driver non\ da Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Pode usar CAU para aplicar atualizações cumulativas?  
Sim. Se as atualizações cumulativas são atualizações de versão de distribuição geral ou LDR, CAU pode aplicá-las.  
  
## <a name="can-i-schedule-updates"></a>Posso agendar atualizações?  
Sim. CAU suporta os seguintes modos de atualização, que permitem que as atualizações para ser agendados:  
  
**Atualizando Self\** permite que o cluster atualizar a próprio acordo com um perfil definido e uma agenda regular, como durante uma janela de manutenção mensal. Você também pode iniciar uma atualização Self\ executar sob demanda a qualquer momento. Para habilitar o modo de atualização self\, você deve adicionar a função clusterizada CAU ao cluster. O recurso de atualização self\ CAU realiza como qualquer outra clusterizada carga de trabalho, e ele pode funcionar perfeitamente com o failovers planejados e de um computador de coordenador de atualização.  
  
**Atualizando Remote\** permite que você comece a executar uma atualização a qualquer momento em um computador que executa o Windows ou Windows Server. Você pode iniciar uma atualização executar através da janela atualizar suporte a Cluster ou usando o **Invoke\ CauRun** cmdlet do PowerShell. Atualizando Remote\ é o padrão atualizando modo para CAU. Você pode usar o Agendador de tarefas para executar o [Invoke CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet em um agendamento de um computador remoto que não seja um de nós de cluster desejado.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Eu pode agendar atualizações para aplicar durante um backup?  
Sim. CAU não impõe quaisquer restrições nesse sentido. No entanto, executar atualizações de software em um servidor \ (com o restarts\ potencial associados) enquanto um backup do servidor está em andamento não é uma prática recomendada de IT. Lembre-se de que CAU depende apenas clustering APIs para determinar failbacks; e os recursos. Assim, CAU desconhece o status de backup do servidor.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>CAU pode trabalhar com o System Center Configuration Manager?  
CAU é uma ferramenta que organize atualizações de software em um nó de cluster e Configuration Manager também executa as atualizações de software de servidor. É importante configurar essas ferramentas para que eles não tiverem a sobreposição de cobertura dos servidores mesmo em qualquer implantação datacenter, incluindo o uso de diferentes servidores do Windows Server Update Services. Isso garante que o objetivo por trás do uso CAU não é inadvertidamente desfeito, pois orientado no Gerenciador de configuração \ atualizando não incorpora o reconhecimento de cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>É necessário credenciais administrativas para executar CAU?  
Sim. Para executar as ferramentas CAU, CAU precisa de credenciais administrativas no servidor local, ou ele precisa o **representar um cliente após autenticação** direito de usuário no servidor local ou o computador cliente no qual ele é executado. No entanto, para coordenar as atualizações de software em nós de cluster, CAU requer credenciais administrativas do cluster em cada nó. Embora o CAU UI possa iniciar sem as credenciais, ele solicitará as credenciais administrativas do cluster quando ele se conecta a uma instância de cluster para visualizar ou aplicar as atualizações.  
  
## <a name="can-i-script-cau"></a>Crio scripts CAU?  
Sim. CAU vem com os cmdlets do PowerShell que oferecem um amplo conjunto de opções de script. Estes são os cmdlets do mesmo que o CAU UI chama para realizar ações CAU.  

## <a name="what-happens-to-active-clustered-roles"></a>O que acontece com funções clusterizadas active?

Clusterizado funções \ (anteriormente conhecido como aplicativos e Services \) que estão ativas em um nó, failover para outros nós antes de atualização de software pode começar. CAU organiza essas failovers usando o modo de manutenção, que interrompe e descarrega o nó de todas as funções de cluster ativos. Quando as atualizações de software são concluídas, CAU retoma o nó e as funções clusterizadas falharem volta para o nó atualizado. Isso garante que a distribuição das funções clusterizadas em relação a nós permanece o mesmo entre a execução da atualização CAU de um cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Como o CAU selecionar nós de destino para funções de cluster?

CAU depende clustering APIs para coordenar o failovers. A implementação da API cluster seleciona os nós de destino contando com métricas internas e heurística posicionamento inteligente \ (como níveis de carga de trabalho) em todos os nós de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Carga CAU equilibrar as funções clusterizadas?

Os nós de cluster, mas ele tenta preservar a distribuição das funções de cluster de balanceamento de carga não CAU. Quando termina de CAU atualizando um nó de cluster, ele tenta falhar voltar anteriormente hospedado clusterizadas funções ao nó. CAU depende clustering APIs para falhar novamente os recursos para o início do processo de pausa. Portanto, na ausência de failovers planejado e configurações do proprietário preferencial, a distribuição das funções clusterizadas deve permanecer inalterada.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Como o CAU selecionar a ordem de nós para atualizar?  
Por padrão, CAU seleciona a ordem de nós atualizar com base no nível de atividade. Os nós que hospedam as funções clusterizadas menor são atualizados pela primeira vez. No entanto, um administrador pode especificar uma ordem específica para atualizar os nós especificando um parâmetro para a atualização executados na UI CAU ou usando os cmdlets do PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>O que acontece se um nó de cluster estiver offline?

O administrador que inicia a executar uma atualização pode especificar o limite aceitável para o número de nós que podem estar offline. Portanto, executar uma atualização pode continuar em um cluster mesmo que todos os nós de cluster não estão online.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Pode usar CAU atualizar apenas um único nó?  
Não. CAU é uma ferramenta de atualização no escopo cluster\, portanto, ele apenas permite que você selecione clusters de atualizar. Se você quiser atualizar um único nó, você pode usar ferramentas independentemente CAU a atualização de servidor existente.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>CAU pode relatar as atualizações que são iniciadas no CAU fora?  
Não. CAU pode somente relatório Atualizando as execuções que é iniciado a partir CAU. No entanto, quando um CAU atualizando executar subsequentes é iniciado, as atualizações que foram instaladas por meio de métodos non\ CAU adequadamente são consideradas para determinar as atualizações adicionais que podem ser aplicáveis a cada nó de cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Pode suporte CAU meu processo de TI exclusivo precisa?  
Sim. CAU oferece as seguintes dimensões da flexibilidade de acordo com a empresa precisa de processo de TI exclusivo dos clientes:  
  
**Scripts** atualizando um executar pode especificar um script de PowerShell pre\ atualização e um script de PowerShell post\ atualização. O script de atualização pre\ é executado em cada nó de cluster antes do nó está pausado. O script de atualização post\ é executado em cada nó de cluster após as nó atualizações são instaladas.  
  
> [!NOTE]  
> .NET framework 4.6 ou 4.5 e PowerShell devem ser instalado em cada nó de cluster no qual você deseja executar os scripts pre\ update e update post\. Você também deve habilitar o PowerShell remoto em nós de cluster. Para requisitos de sistema detalhadas, consulte [requisitos e as práticas recomendadas para suporte a Cluster Atualizando](cluster-aware-updating-requirements.md).  
  
**Atualizando executar opções avançadas** Além disso, o administrador pode especificar de um grande conjunto de opções avançadas de executar a atualização, como o número máximo de vezes que o processo de atualização é tentada novamente em cada nó. Essas opções podem ser especificadas usando o CAU UI ou os cmdlets do PowerShell CAU. Essas configurações personalizadas podem ser salvo em um perfil de executar a atualização e reutilizadas para execuções posteriores atualizando.  
  
**Pública plug\ na arquitetura** CAU inclui recursos para registrar, Unregister, e selecione plug\-ins. CAU vem com dois plug\-ins padrão: uma coordenadas \(WUA\) o Windows Update Agent APIs em cada nó de cluster; o segundo aplica hotfixes manualmente são copiados para um compartilhamento de arquivos que seja acessível para os nós de cluster. Se uma empresa tiver necessidades exclusivas que não podem ser atendidas com esses dois plug\-ins, a empresa pode criar um novo CAU plug\-in de acordo com a especificação de API pública. Para obter mais informações, consulte [Cluster\ reconhecimento Atualizando referência de Plug\](http://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obter informações sobre como configurar e personalizar CAU plug\-ins para dar suporte a diferentes atualizando cenários, consulte [funcionamento Plug\-ins](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Como exportar a visualização CAU e atualizar resultados?  
CAU oferece opções de exportação por meio da interface de linha de command\ e na interface do usuário.  
  
**Opções de interface de linha de Command\:**  
  
-   Visualizar resultados usando o cmdlet do PowerShell **Invoke\ CauScan | Xml ConvertTo\**. Saída: XML  
  
-   Relatar resultados usando o cmdlet do PowerShell **Invoke\ CauRun | Xml ConvertTo\**. Saída: XML  
  
-   Relatar resultados usando o cmdlet do PowerShell **Get\ CauReport | Export\ CauReport**. Saída: HTML, CSV  
  
**Opções de interface do usuário:**  
  
-   Copiar os resultados do relatório do **visualizar atualizações** tela. Saída: CSV  
  
-   Copiar os resultados do relatório do **Gerar relatório** tela. Saída: CSV  
  
-   Exportar os resultados do relatório do **Gerar relatório** tela. Saída: HTML  
  
## <a name="how-do-i-install-cau"></a>Como instalo CAU?  
Uma instalação CAU está completamente integrada ao recurso cluster de Failover. CAU é instalado da seguinte maneira:  
  
-   Quando o cluster de Failover é instalado em um nó de cluster, o provedor \(WMI\) CAU Windows Management Instrumentation é automaticamente instalado.  
  
-   Quando o recurso ferramentas de cluster de Failover é instalado em um computador cliente ou servidor, os cmdlets do PowerShell e suporte a Cluster Atualizando interface do usuário são instalados automaticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU precisa componentes em execução em nós de cluster que estão sendo atualizados?  
CAU não precisa de um serviço em execução em nós de cluster. No entanto, CAU precisa de um componente de software \ (o provider\ WMI) instalado em nós de cluster. Esse componente é instalado com o recurso de cluster de Failover.  
  
Para habilitar o modo de atualização self\, a função clusterizada CAU também deve ser adicionada ao cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Qual é a diferença entre usar CAU e VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) enfoca atualizando apenas clusters de Hyper\-V, enquanto CAU pode atualizar qualquer tipo de cluster de failover com suporte, incluindo clusters Hyper\-V.  
  
-   VMM requer uma licença adicional, enquanto CAU é licenciado para todos os Windows Server. Os recursos, ferramentas e interface do usuário CAU são instalados com componentes de cluster de Failover.  
  
-   Se você já tem uma licença do System Center, você pode continuar a usar VMM atualizar clusters Hyper\-V, porque ele oferece um gerenciamento integrado e a experiência de atualização de software.  
  
-   CAU é compatível apenas com clusters que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016. VMM também dá suporte a clusters Hyper\-V em computadores que executam o Windows Server 2008 R2 e Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Posso usar atualizando remote\ em um cluster que está configurado para a atualização self\?  
Sim. Um cluster de failover em uma configuração atualizando self\ pode ser atualizado por meio de atualização remote\ on\ demanda, assim como você pode forçar uma verificação do Windows Update a qualquer momento no seu computador, mesmo se o Windows Update está configurado para instalar atualizações automaticamente. No entanto, você precisa certificar-se de que executar uma atualização não já está em andamento.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Pode reutilizar minhas configurações de atualização do cluster em clusters?  
Sim. CAU dá suporte a várias opções de atualização executar que determinam como executar a atualização se comporta quando ele atualiza o cluster. Essas opções podem ser salvos como um perfil de executar a atualização, e eles podem ser reutilizados em qualquer cluster. Recomendamos que você salve e reutilizar as configurações em clusters de failover com necessidades de atualização semelhantes. Por exemplo, você pode criar um "Business\ críticos SQL Server Cluster atualizando executar perfil" para todos os clusters Microsoft SQL Server que dão suporte a serviços business\ críticos.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Onde está a CAU plug\ na especificação?  
  
-   [Cluster\ voltada para atualizar a referência de Plug\](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Atualizando ciente plug\ na amostra do cluster](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da atualização Cluster\ reconhecimento](cluster-aware-updating.md)  
  
