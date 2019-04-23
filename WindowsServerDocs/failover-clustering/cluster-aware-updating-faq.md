---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: Cluster-Aware Updating - perguntas frequentes
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Respostas para perguntas frequentes sobre a atualização com suporte a Cluster no Windows Server.
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882517"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Cluster-Aware Updating: Perguntas frequentes

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Cluster-Aware Updating](cluster-aware-updating.md) \(CAU\) é um recurso que coordena a software de atualizações em todos os servidores em um cluster de failover de uma maneira que não afete a disponibilidade do serviço de qualquer mais de um failover planejado de um nó de cluster. Para alguns aplicativos com recursos de disponibilidade contínua \(, como Hyper\-V com migração ao vivo ou um servidor de arquivos SMB 3.x com SMB Transparent Failover\), a CAU pode coordenar a atualização de cluster automatizada sem afetar a disponibilidade do serviço.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>A CAU oferece suporte a clusters de espaços de armazenamento diretos de atualização?  
Sim. A CAU oferece suporte à atualização [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters independentemente do tipo de implantação: hiperconvergente ou convergido. Especificamente, a orquestração de CAU garante que cada nó de cluster a suspensão aguarda o espaço de armazenamento em cluster subjacente seja íntegro.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>A CAU funciona com o Windows Server 2008 R2 ou o Windows 7?  
Nenhum. A CAU coordena o operação somente de computadores que executam o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 ou Windows 8 da atualização do cluster. O cluster de failover que está sendo atualizado deve executar o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU é limitado a aplicativos clusterizados específicos?  
Nenhum. A CAU não tem tipo definido de aplicativo clusterizado. A CAU é um cluster externo\-atualizar solução que fazem parte de cmdlets do PowerShell e APIs de clustering. Como tal, a CAU pode coordenar a atualização de qualquer aplicativo clusterizado que está configurado em um cluster de failover do Windows Server.  
  
> [!NOTE]  
> Atualmente, as seguintes cargas de trabalho em cluster são testadas e certificadas para CAU: SMB, Hyper\-V, replicação do DFS, Namespaces do DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>A CAU oferece suporte às atualizações do Microsoft Update e do Windows Update?  
Sim. Por padrão, a CAU é configurada com um plug\-que usa o Windows Update Agent \(WUA\) APIs do utilitário em nós de cluster. A infraestrutura do WUA pode ser configurada para apontar para o Microsoft Update e Windows Update ou para o Windows Server Update Services \(WSUS\) como a origem das atualizações.  
  
## <a name="does-cau-support-wsus-updates"></a>A CAU oferece suporte às atualizações do WSUS?  
Sim. Por padrão, a CAU é configurada com um plug\-que usa o Windows Update Agent \(WUA\) APIs do utilitário em nós de cluster. A infraestrutura do WUA pode ser configurada para apontar para o Microsoft Update e Windows Update ou para um banco de dados local do Windows Server Update Services \(WSUS\) server como a origem das atualizações.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>A CAU pode aplicar as atualizações de versão de distribuição limitada?  
Sim. Limitado a versão de distribuição \(LDR\) atualizações, também chamadas de hotfixes, não são publicadas por meio do Microsoft Update ou Windows Update, elas não podem ser baixadas pelo Windows Update Agent \(WUA\) conecte\-em que a CAU usa por padrão.  
  
No entanto, a CAU inclui um segundo plug\-em que você pode selecionar para aplicar atualizações do hotfix. Este hotfix plug\-pode também ser personalizado para aplicar não\-driver da Microsoft, firmware e atualizações de BIOS.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Posso usar a CAU para aplicar atualizações cumulativas?  
Sim. Se as atualizações cumulativas forem atualizações de versão de distribuição geral ou LDR, a CAU as aplicará.  
  
## <a name="can-i-schedule-updates"></a>Posso agendar atualizações?  
Sim. A CAU oferece suporte aos seguintes modos de atualização, que permitem que as atualizações sejam agendadas:  
  
**Self\-Atualizando** permite que o cluster atualize a mesmo acordo com um perfil definido e um agendamento regular, como durante uma janela de manutenção mensal. Você também pode iniciar um Self\-execução de atualização sob demanda a qualquer momento. Para habilitar o self\-modo de atualização, você deve adicionar a função clusterizada CAU no cluster. A CAU self\-recurso de atualização é executado como qualquer outra cluster carga de trabalho e ele pode funcionar perfeitamente com os failovers planejados e não planejados de um computador de coordenador de atualização.  
  
**Remoto\-Atualizando** permite que você inicie uma execução de atualização a qualquer momento em um computador que executa o Windows ou Windows Server. Você pode iniciar uma execução de atualização por meio da janela de Cluster-Aware Updating ou usando o **Invoke\-CauRun** cmdlet do PowerShell. Remoto\-a atualização é o modo de atualização da CAU de padrão. Você pode usar o Agendador de Tarefas para executar o cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) em um cronograma desejado de um computador remoto que não é um dos nós de cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Posso programar atualizações a serem aplicadas durante um backup?  
Sim. A CAU não impõe qualquer restrição em relação a isso. No entanto, executar atualizações de software em um servidor \(com o potencial associado reinicia\) enquanto um backup do servidor está em andamento não é uma prática recomendada de TI. Esteja ciente de que a CAU depende apenas de APIs de clustering para determinar failovers e failbacks de recursos. Portanto, a CAU não tem conhecimento do status de backup do servidor.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>A CAU pode trabalhar com o System Center Configuration Manager?  
A CAU é uma ferramenta que coordena as atualizações de software em um nó de cluster e do Configuration Manager também executa as atualizações de software do servidor. É importante configurar essas ferramentas para que eles não têm sobreposição de cobertura dos mesmos servidores em qualquer implantação de datacenter, incluindo o uso de diferentes servidores do Windows Server Update Services. Isso garante que o objetivo por trás do uso da CAU não seja inadvertidamente contrariado, porque o Configuration Manager\-atualizando orientado por não incorpora o reconhecimento de cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Preciso de credenciais administrativas para executar a CAU?  
Sim. Para executar as ferramentas da CAU, a CAU precisa de credenciais administrativas no servidor local ou precisa do direito de usuário **Representar um Cliente Após Autenticação** no servidor local ou no computador cliente no qual estão sendo executadas. No entanto, para coordenar atualizações de software em nós de cluster, a CAU exige credenciais administrativas de cluster em cada nó. Embora UI CAU pode ser iniciada sem as credenciais, ele solicitará as credenciais administrativas de cluster quando ele se conecta a uma instância de cluster para visualizar ou aplicar atualizações.  
  
## <a name="can-i-script-cau"></a>Posso criar CAU um script?  
Sim. A CAU vem com cmdlets do PowerShell que oferecem um conjunto avançado de opções de script. Esses são os mesmos cmdlets que a interface do usuário da CAU chama para executar as ações da CAU.  

## <a name="what-happens-to-active-clustered-roles"></a>O que acontece com funções clusterizadas ativas?

Funções clusterizadas \(chamados de aplicativos e serviços\) que estão ativos em um nó, faça o failover para outros nós antes que a atualização de software possa começar. A CAU orquestra esses failovers usando o modo de manutenção, que pausa e drena o nó de todas as funções clusterizadas ativas. Quando as atualizações de software estão completas, a CAU continua com o nó e as funções clusterizadas realizam failback para o nó atualizado. Isso garante que a distribuição de funções clusterizadas relativas aos nós permaneça a mesma nas Execução de Atualização da CAU de um cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Como a CAU seleciona nós de destino para as funções clusterizadas?

A CAU confia nas APIs de clustering para coordenar os failovers. A implementação da API de clustering seleciona os nós de destino confiando nas métricas internas e as heurísticas de posicionamento inteligente \(, como níveis de carga de trabalho\) em todos os nós de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Carga CAU equilibrar as funções de cluster?

Os nós clusterizados, mas tenta preservar a distribuição de funções de cluster de balanceamento de carga não CAU. Quando a CAU conclui a atualização de um nó de cluster, tenta realizar o failback das funções clusterizadas hospedadas anteriormente no nó. A CAU depende das APIs de clustering para realizar failback dos recursos para o início do processo de pausa. Portanto, na ausência de failovers não planejados e configurações de proprietário preferenciais, a distribuição de funções clusterizadas deve permanecer inalterada.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Como a CAU seleciona a ordem dos nós a serem atualizados?  
Por padrão, a CAU seleciona a ordem dos nós a serem atualizados com base no nível de atividade. Os nós que estão hospedando as funções menos clusterizadas são atualizados primeiro. No entanto, um administrador pode especificar uma ordem específica para atualizar os nós especificando um parâmetro para a execução de atualização na UI do CAU ou usando os cmdlets do PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>O que acontece se um nó de cluster estiver offline?

O administrador que inicia uma Execução de Atualização pode especificar o limite aceitável para o número de nós que pode estar offline. Portanto, uma Execução de Atualização pode continuar em um cluster mesmo se todos os nós do cluster não estiverem online.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Pode usar o CAU para atualizar apenas um único nó?  
Nenhum. A CAU é um cluster\-com escopo de atualização de ferramenta, portanto, ele só permite que você selecione os clusters a serem atualizados. Se você deseja atualizar um único nó, pode usar as ferramentas de atualização de servidor existentes independentemente da CAU.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>A CAU pode relatar atualizações que são iniciadas de fora da CAU?  
Nenhum. A CAU só pode relatar as Execuções de Atualizações que são iniciadas na CAU. No entanto, quando um subsequentes CAU execução de atualização for iniciada, as atualizações que foram instaladas por meio de não\-métodos CAU são considerados adequadamente para determinar as atualizações adicionais que podem ser aplicáveis a cada nó do cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Possível suporte a CAU precisa de meus processos de TI exclusivas?  
Sim. A CAU oferece suporte às seguintes dimensões de flexibilidade para atender às necessidades de processos de TI exclusivas de clientes da empresa:  
  
**Scripts** uma execução de atualização pode especificar uma versão anterior\-atualizar o script do PowerShell e uma postagem\-Atualizar script do PowerShell. O pré\-script de atualização é executado em cada nó de cluster antes do nó está em pausa. A postagem\-script de atualização é executado em cada nó de cluster após a instalação das atualizações de nó.  
  
> [!NOTE]  
> .NET framework 4.6 ou 4.5 e o PowerShell devem ser instalado em cada nó de cluster no qual você deseja executar o pré\-atualizar e lançar\-scripts de atualização. Você também deve habilitar a comunicação remota do PowerShell em nós de cluster. Para requisitos de sistema detalhados, consulte [requisitos e práticas recomendadas para atualização com suporte a Cluster](cluster-aware-updating-requirements.md).  
  
**Opções de execução de atualização avançadas** Além disso, o administrador pode especificar um grande conjunto de opções de execução de atualização avançadas, como o número máximo de vezes que o processo de atualização é repetido em cada nó. Essas opções podem ser especificadas usando a UI do CAU ou os cmdlets do PowerShell do CAU. Essas configurações personalizadas podem ser salvas em um Perfil de Execução da Atualização e reutilizadas para execuções de atualização mais tarde.  
  
**Público plug\-arquitetura** CAU inclui recursos para registrar, cancelar o registro, e selecione conecta\-ins. CAU é fornecido com dois padrão plug\-ins: um coordena o Windows Update Agent \(WUA\) APIs em cada nó do cluster; o segundo aplica hotfixes que serão copiados manualmente para um compartilhamento de arquivo que está acessível a nós do cluster. Se uma empresa tiver necessidades exclusivas que não podem ser atendidas com esses dois conecte\-ins, a empresa pode criar um novo plug CAU\-além de acordo com a especificação da API pública. Para obter mais informações, consulte [Cluster\-ciente Plug Atualizando\-na referência](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obter informações sobre como configurar e personalizar CAU conecte\-ins para dar suporte a diferentes atualizando cenários, consulte [como conecte\-ins funcionam](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Como posso exportar os resultados de visualização e atualização da CAU?  
Opções por meio do comando de exportação de ofertas CAU\-interface de linha e por meio da interface do usuário.  
  
**Comando\-opções de interface de linha:**  
  
-   Visualize os resultados usando o cmdlet do PowerShell **Invoke\-CauScan | ConvertTo\-Xml**. Saída: XML  
  
-   Os resultados de relatório usando o cmdlet do PowerShell **Invoke\-CauRun | ConvertTo\-Xml**. Saída: XML  
  
-   Os resultados de relatório usando o cmdlet do PowerShell **obter\-CauReport | Exportar\-CauReport**. Saída: HTML, CSV  
  
**Opções de interface do usuário:**  
  
-   Copie os resultados de relatório da tela **Visualizar atualizações**. Saída: CSV  
  
-   Copie os resultados de relatório da tela **Gerar relatório**. Saída: CSV  
  
-   Exporte os resultados de relatório da tela **Gerar relatório** . Saída: HTML  
  
## <a name="how-do-i-install-cau"></a>Como posso instalar a CAU?  
A instalação da CAU é totalmente integrada ao recurso Clustering de Failover. A CAU é instalada da seguinte forma:  
  
-   Quando o Clustering de Failover é instalado em um nó de cluster, a instrumentação de gerenciamento do Windows CAU \(WMI\) provedor é instalado automaticamente.  
  
-   Quando o recurso ferramentas de Clustering de Failover é instalado em um servidor ou computador cliente, os IU de atualização com suporte a Cluster e cmdlets do PowerShell são instalados automaticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>A CAU precisa de componentes em execução em nós de cluster que estão sendo atualizados?  
A CAU não precisa de um serviço em execução em nós de cluster. No entanto, a CAU precisa de um componente de software \(o provedor WMI\) instalado em nós de cluster. Este componente é instalado com o recurso Clustering de Failover.  
  
Para habilitar o self\-modo de atualização, a função clusterizada CAU também deve ser adicionada ao cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>O que é a diferença entre usar a CAU e o VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) enfoca Hyper apenas atualizando\-V clusters, enquanto a CAU possa atualizar qualquer tipo de cluster de failover com suporte, incluindo Hyper\-clusters V.  
  
-   O VMM exige licença adicional, enquanto a CAU está licenciada para todos os Windows Server. Os recursos, as ferramentas e a interface do usuário da CAU são instalados com os componentes de Clustering de Failover.  
  
-   Se você já possui uma licença do System Center, você pode continuar a usar o VMM para atualizar Hyper\-V clusters porque ele oferece um gerenciamento integrado e experiência de atualização de software.  
  
-   A CAU tem suporte apenas em clusters que executam o Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. O VMM também dá suporte a Hyper\-V clusters em computadores que executam o Windows Server 2008 R2 e Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Posso usar remote\-a atualização em um cluster que está configurado para self\-atualizando?  
Sim. Um cluster de failover em um self\-atualizando a configuração pode ser atualizada por meio de remote\-atualizando em\-de demanda, assim como você pode forçar uma verificação de atualização do Windows a qualquer momento em seu computador, mesmo que o Windows Update está configurado para Instale atualizações automaticamente. No entanto, você precisa verificar se uma Execução de Atualização já não está em andamento.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Posso reutilizar minhas configurações de atualização de cluster nos clusters?  
Sim. A CAU oferece suporte a várias opções de Execução de Atualização que determinam como a Execução de Atualização se comporta quando ela atualiza o cluster. Essas opções podem ser salvas como um Perfil de Execução da Atualização e podem ser reutilizadas em qualquer cluster. Recomendamos que você salve e reutilize suas configurações em clusters de failover que têm necessidades de atualização semelhantes. Por exemplo, você pode criar um "Business\-perfil de execução de atualização de Cluster do SQL Server crítica" para todos os clusters do Microsoft SQL Server que dão suporte a negócios\-serviços críticos.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Onde está o plugue CAU\-na especificação?  
  
-   [Cluster\-Plug de atualização com suporte\-na referência](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Plug de atualização com suporte a cluster\-na amostra](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Cluster\-visão geral da atualização com suporte](cluster-aware-updating.md)  
  
