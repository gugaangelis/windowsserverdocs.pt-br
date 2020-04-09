---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: Atualização com suporte a cluster-perguntas frequentes
ms.topic: article
ms.prod: windows-server
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Respostas para perguntas frequentes sobre a atualização com suporte a cluster no Windows Server.
ms.openlocfilehash: ca81e952c0524af36ab6d241a205bd1cc971c74a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828099"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Atualização com suporte a cluster: perguntas frequentes

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A [atualização com suporte a Cluster](cluster-aware-updating.md) \(\) Cau é um recurso que coordena as atualizações de software em todos os servidores em um cluster de failover de uma forma que não afete a disponibilidade do serviço mais do que um failover planejado de um nó de cluster. Para alguns aplicativos com recursos de disponibilidade contínua \(como o Hyper\-V com migração dinâmica ou um servidor de arquivos SMB 3. x com\)de failover transparente SMB, a CAU pode coordenar a atualização automatizada do cluster sem afetar a disponibilidade do serviço.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>A CAU oferece suporte à atualização de clusters Espaços de Armazenamento Diretos?  
Sim. A CAU oferece suporte à atualização de clusters [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) , independentemente do tipo de implantação: hiperconvergente ou convergido. Especificamente, a orquestração da CAU garante que a suspensão de cada nó de cluster espere que o espaço de armazenamento em cluster subjacente esteja íntegro.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>A CAU funciona com o Windows Server 2008 R2 ou o Windows 7?  
Não. A CAU coordena a operação de atualização de cluster somente de computadores que executam o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012, o Windows 10, o Windows 8.1 ou o Windows 8. O cluster de failover que está sendo atualizado deve executar o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>A CAU é limitada a aplicativos em cluster específicos?  
Não. A CAU não tem tipo definido de aplicativo clusterizado. A CAU é um cluster externo\-a atualização da solução que está em camadas sobre as APIs de clustering e os cmdlets do PowerShell. Dessa forma, a CAU pode coordenar a atualização para qualquer aplicativo clusterizado configurado em um cluster de failover do Windows Server.  
  
> [!NOTE]  
> Atualmente, as seguintes cargas de trabalho clusterizadas são testadas e certificadas para CAU: SMB, Hyper\-V, Replicação do DFS, namespaces DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>A CAU oferece suporte às atualizações do Microsoft Update e do Windows Update?  
Sim. Por padrão, a CAU é configurada com um plug\-no que usa o agente Windows Update \(o WUA\) APIs do utilitário nos nós do cluster. A infraestrutura do WUA pode ser configurada para apontar para Microsoft Update e Windows Update ou para Windows Server Update Services \(WSUS\) como sua fonte de atualizações.  
  
## <a name="does-cau-support-wsus-updates"></a>A CAU oferece suporte às atualizações do WSUS?  
Sim. Por padrão, a CAU é configurada com um plug\-no que usa o agente Windows Update \(o WUA\) APIs do utilitário nos nós do cluster. A infraestrutura do WUA pode ser configurada para apontar para Microsoft Update e Windows Update ou para um Windows Server Update Services local \(servidor do WSUS\) como sua fonte de atualizações.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>A CAU pode aplicar as atualizações de versão de distribuição limitada?  
Sim. A versão de distribuição limitada \(LDR\) atualizações, também chamadas de hotfixes, não são publicadas por meio de Microsoft Update ou Windows Update, portanto, elas não podem ser baixadas pelo agente de Windows Update \(o\) do do WUA em que a CAU usa por padrão.\-  
  
No entanto, a CAU inclui um segundo plug\-em que você pode selecionar para aplicar atualizações de hotfix. Este hotfix plug\-in também pode ser personalizado para aplicar atualizações de BIOS, firmware e driver não\-Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Posso usar a CAU para aplicar atualizações cumulativas?  
Sim. Se as atualizações cumulativas forem atualizações de versão de distribuição geral ou LDR, a CAU as aplicará.  
  
## <a name="can-i-schedule-updates"></a>Posso agendar atualizações?  
Sim. A CAU oferece suporte aos seguintes modos de atualização, que permitem que as atualizações sejam agendadas:  
  
**Atualização de auto\-** Permite que o cluster se atualize de acordo com um perfil definido e um agendamento regular, como durante uma janela de manutenção mensal. Você também pode iniciar uma execução de auto\-de atualização sob demanda a qualquer momento. Para habilitar o modo de atualização de\-automática, você deve adicionar a função clusterizada CAU ao cluster. O recurso de atualização de auto\-da CAU é executado como qualquer outra carga de trabalho clusterizada e pode funcionar diretamente com os failovers planejados e não planejados de um computador do coordenador de atualização.  
  
**Atualização\-remota** Permite iniciar uma execução de atualização a qualquer momento a partir de um computador que executa o Windows ou o Windows Server. Você pode iniciar uma atualização de execução por meio da janela de atualização com suporte a cluster ou usando o cmdlet **Invoke\-CauRun** do PowerShell. A atualização de\-remota é o modo de atualização padrão para a CAU. Você pode usar o Agendador de Tarefas para executar o cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) em um cronograma desejado de um computador remoto que não é um dos nós de cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Posso agendar atualizações a serem aplicadas durante um backup?  
Sim. A CAU não impõe nenhuma restrição nesse sentido. No entanto, a execução de atualizações de software em um servidor \(com as possíveis reinicializações associadas\) enquanto um backup de servidor está em andamento não é uma prática recomendada de ti. Esteja ciente de que a CAU depende apenas de APIs de clustering para determinar failovers e failbacks de recursos. Portanto, a CAU não tem conhecimento do status de backup do servidor.  
  
## <a name="can-cau-work-with-configuration-manager"></a>A CAU pode trabalhar com Configuration Manager?  
A CAU é uma ferramenta que coordena atualizações de software em um nó de cluster e Configuration Manager também executa atualizações de software de servidor. É importante configurar essas ferramentas para que elas não tenham cobertura sobreposta dos mesmos servidores em qualquer implantação de datacenter, incluindo o uso de diferentes servidores de Windows Server Update Services. Isso garante que o objetivo por trás do uso da CAU não seja inadvertidamente derrotado, porque Configuration Manager\-atualização controlada não incorpora o reconhecimento do cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Preciso de credenciais administrativas para executar a CAU?  
Sim. Para executar as ferramentas da CAU, a CAU precisa de credenciais administrativas no servidor local ou precisa do direito de usuário **Representar um Cliente Após Autenticação** no servidor local ou no computador cliente no qual estão sendo executadas. No entanto, para coordenar atualizações de software em nós de cluster, a CAU exige credenciais administrativas de cluster em cada nó. Embora a interface do usuário da CAU possa iniciar sem as credenciais, ela solicita as credenciais administrativas do cluster quando ele se conecta a uma instância de cluster para visualizar ou aplicar atualizações.  
  
## <a name="can-i-script-cau"></a>Posso criar um script da CAU?  
Sim. A CAU vem com cmdlets do PowerShell que oferecem um conjunto avançado de opções de script. Esses são os mesmos cmdlets que a interface do usuário da CAU chama para executar as ações da CAU.  

## <a name="what-happens-to-active-clustered-roles"></a>O que acontece com as funções clusterizadas ativas?

As funções clusterizadas \(anteriormente chamadas de aplicativos e serviços\) que estão ativas em um nó, fazem failover para outros nós antes que a atualização de software possa começar. A CAU orquestra esses failovers usando o modo de manutenção, que pausa e drena o nó de todas as funções clusterizadas ativas. Quando as atualizações de software estão completas, a CAU continua com o nó e as funções clusterizadas realizam failback para o nó atualizado. Isso garante que a distribuição de funções clusterizadas relativas aos nós permaneça a mesma nas Execução de Atualização da CAU de um cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Como a CAU seleciona nós de destino para funções clusterizadas?

A CAU confia nas APIs de clustering para coordenar os failovers. A implementação da API de clustering seleciona os nós de destino, contando com as métricas internas e a heurística de posicionamento inteligente \(como os níveis de carga de trabalho\) entre os nós de destino.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>A CAU equilibra a carga das funções clusterizadas?

A CAU não equilibra a carga dos nós clusterizados, mas tenta preservar a distribuição de funções clusterizadas. Quando a CAU conclui a atualização de um nó de cluster, tenta realizar o failback das funções clusterizadas hospedadas anteriormente no nó. A CAU depende das APIs de clustering para realizar failback dos recursos para o início do processo de pausa. Portanto, na ausência de failovers não planejados e configurações de proprietário preferenciais, a distribuição de funções clusterizadas deve permanecer inalterada.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Como a CAU seleciona a ordem dos nós a serem atualizados?  
Por padrão, a CAU seleciona a ordem dos nós a serem atualizados com base no nível de atividade. Os nós que estão hospedando as funções menos clusterizadas são atualizados primeiro. No entanto, um administrador pode especificar uma ordem específica para atualizar os nós, especificando um parâmetro para a execução de atualização na interface do usuário da CAU ou usando os cmdlets do PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>O que acontecerá se um nó de cluster estiver offline?

O administrador que inicia uma Execução de Atualização pode especificar o limite aceitável para o número de nós que pode estar offline. Portanto, uma Execução de Atualização pode continuar em um cluster mesmo se todos os nós do cluster não estiverem online.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Posso usar a CAU para atualizar apenas um único nó?  
Não. A CAU é um cluster\-ferramenta de atualização com escopo, portanto, ele só permite que você selecione clusters para atualizar. Se você deseja atualizar um único nó, pode usar as ferramentas de atualização de servidor existentes independentemente da CAU.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>As atualizações de relatório da CAU podem ser iniciadas de fora da CAU?  
Não. A CAU só pode relatar as Execuções de Atualizações que são iniciadas na CAU. No entanto, quando uma execução de atualização de CAU subsequente é iniciada, as atualizações que foram instaladas por meio de métodos de CAU não\-são consideradas adequadamente para determinar as atualizações adicionais que podem ser aplicáveis a cada nó de cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>A CAU pode oferecer suporte a minhas necessidades de processo de ti exclusivas?  
Sim. A CAU oferece suporte às seguintes dimensões de flexibilidade para atender às necessidades de processos de TI exclusivas de clientes da empresa:  
  
**Scripts** do Uma execução de atualização pode especificar um script do PowerShell de atualização pré\-e uma postagem\-atualizar o script do PowerShell. O script de atualização pré\-é executado em cada nó de cluster antes de o nó ser pausado. O script post\-Update é executado em cada nó do cluster depois que as atualizações do nó são instaladas.  
  
> [!NOTE]  
> .NET Framework 4,6 ou 4,5 e o PowerShell devem ser instalados em cada nó de cluster no qual você deseja executar a atualização anterior\-e postar\-scripts de atualização. Você também deve habilitar a comunicação remota do PowerShell nos nós do cluster. Para obter os requisitos de sistema detalhados, consulte [requisitos e práticas recomendadas para atualização com suporte a cluster](cluster-aware-updating-requirements.md).  
  
**Opções de execução de atualização avançada** O administrador também pode especificar em um grande conjunto de opções de execução de atualização avançadas, como o número máximo de vezes que o processo de atualização é repetido em cada nó. Essas opções podem ser especificadas usando a interface do usuário da CAU ou os cmdlets do PowerShell da CAU. Essas configurações personalizadas podem ser salvas em um Perfil de Execução da Atualização e reutilizadas para execuções de atualização mais tarde.  
  
**\-do plug-in público na arquitetura** A CAU inclui recursos para registrar, cancelar o registro e selecionar plug\-ins. a CAU é fornecida com dois\-plug-ins padrão: um coordena o agente de Windows Update \(as APIs de\) do WUA em cada nó de cluster; o segundo aplica hotfixes que são manualmente copiados para um compartilhamento de arquivos que é acessível para os nós de cluster. Se uma empresa tiver necessidades exclusivas que não podem ser atendidas com esses dois plug\-ins, a empresa poderá criar uma nova\-de plug-in CAU de acordo com a especificação da API pública. Para obter mais informações, consulte [\-do plug-in de atualização com suporte do Cluster\-em referência](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Para obter informações sobre como configurar e personalizar o plug-in CAU\-ins para dar suporte a diferentes cenários de atualização, consulte [como plug\-ins funcionam](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Como posso exportar os resultados de visualização e atualização da CAU?  
A CAU oferece opções de exportação por meio do comando\-interface de linha e por meio da interface do usuário.  
  
**Opções de interface de linha de comando\-:**  
  
-   Visualizar resultados usando a invocação de cmdlet do PowerShell **\-CauScan | ConvertTo\-XML**. Saída: XML  
  
-   Relatar resultados usando o cmdlet do PowerShell **Invoke\-CauRun | ConvertTo\-XML**. Saída: XML  
  
-   Relatar resultados usando o cmdlet do PowerShell **Get\-CauReport | Exportar\-CauReport**. Saída: HTML, CSV  
  
**Opções da interface do usuário:**  
  
-   Copie os resultados de relatório da tela **Visualizar atualizações**. Saída: CSV  
  
-   Copie os resultados de relatório da tela **Gerar relatório**. Saída: CSV  
  
-   Exporte os resultados de relatório da tela **Gerar relatório**. Saída: HTML  
  
## <a name="how-do-i-install-cau"></a>Como posso instalar a CAU?  
A instalação da CAU é totalmente integrada ao recurso Clustering de Failover. A CAU é instalada da seguinte forma:  
  
-   Quando o clustering de failover é instalado em um nó de cluster, o provedor de\) da CAU Instrumentação de Gerenciamento do Windows \(WMI é instalado automaticamente.  
  
-   Quando o recurso de ferramentas de clustering de failover é instalado em um computador cliente ou servidor, a interface do usuário de atualização com suporte a cluster e os cmdlets do PowerShell são instalados automaticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>A CAU precisa de componentes em execução nos nós de cluster que estão sendo atualizados?  
A CAU não precisa de um serviço em execução nos nós do cluster. No entanto, a CAU precisa de um componente de software \(o provedor WMI\) instalado nos nós do cluster. Este componente é instalado com o recurso Clustering de Failover.  
  
Para habilitar o modo de atualização de auto\-, a função clusterizada CAU também deve ser adicionada ao cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Qual é a diferença entre usar o CAU e o VMM?  
  
-   System Center Virtual Machine Manager \(\) do VMM se concentra na atualização somente de clusters do Hyper\-V, enquanto a CAU pode atualizar qualquer tipo de cluster de failover com suporte, incluindo clusters do Hyper\-V.  
  
-   O VMM requer licenciamento adicional, enquanto a CAU é licenciada para todos os servidores Windows. Os recursos, as ferramentas e a interface do usuário da CAU são instalados com os componentes de Clustering de Failover.  
  
-   Se você já possui uma licença do System Center, pode continuar a usar o VMM para atualizar clusters do Hyper\-V porque ele oferece uma experiência integrada de gerenciamento e atualização de software.  
  
-   Há suporte para a CAU somente em clusters que executam o Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012. O VMM também dá suporte a clusters Hyper\-V em computadores que executam o Windows Server 2008 R2 e o Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Posso usar a atualização de\-remota em um cluster configurado para auto\-atualização?  
Sim. Um cluster de failover em uma configuração de atualização\-pode ser atualizado por meio da atualização de\-remota em\-demanda, assim como você pode forçar uma verificação de Windows Update a qualquer momento no computador, mesmo se o Windows Update estiver configurado para instalar atualizações automaticamente. No entanto, você precisa verificar se uma Execução de Atualização já não está em andamento.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Posso reutilizar minhas configurações de atualização de cluster nos clusters?  
Sim. A CAU oferece suporte a várias opções de Execução de Atualização que determinam como a Execução de Atualização se comporta quando ela atualiza o cluster. Essas opções podem ser salvas como um Perfil de Execução da Atualização e podem ser reutilizadas em qualquer cluster. Recomendamos que você salve e reutilize suas configurações em clusters de failover que têm necessidades de atualização semelhantes. Por exemplo, você pode criar um perfil de execução de atualização de cluster de SQL Server crítico de\-de negócios "para todos os clusters de Microsoft SQL Server que dão suporte aos serviços críticos de negócios\-.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Onde o plug-in CAU\-em especificação?  
  
-   [\-de plug-in de atualização de\-com reconhecimento de cluster em referência](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [\-plug-in de atualização com reconhecimento de cluster em exemplo](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da Atualização com Suporte a Cluster\-  
  
