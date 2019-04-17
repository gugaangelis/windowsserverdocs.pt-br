---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: "Reconhecimento de atrativos atualizando opções avançadas e atualizar perfis de execução"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: "Como definir opções avançadas e atualizar perfis de execução para suporte a Cluster atualizando (CAU)"
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Suporte a cluster Atualizando as opções avançadas e atualizar perfis de execução

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as opções atualizando executar que podem ser configuradas para um [atualizando suporte a Cluster](cluster-aware-updating.md) (CAU) atualizando executar. Essas opções avançadas podem ser configuradas quando você usa o CAU UI ou os cmdlets do Windows PowerShell CAU para aplicar atualizações ou para configurar as opções de atualização automática.

A maioria das configurações pode ser salvo como um arquivo XML chamado um perfil de executar a atualização e reutilizado para execuções posteriores atualizando. Os valores padrão para as opções de atualização executar que são fornecidos pelo CAU também podem ser usados em muitos ambientes de cluster.

Para obter informações sobre opções adicionais que você pode especificar para executar cada atualização e atualizar perfis de execução, consulte as seções a seguir neste tópico:

Opções que você especificar quando você solicitar uma atualização executar uso atualizando executar perfis opções que podem ser definidas em um perfil de executar a atualização

A tabela a seguir lista as opções que podem ser definidas em um perfil de executar a atualização CAU. 

> [!NOTE] 
> Para definir a opção PreUpdateScript ou PostUpdateScript, certifique-se de que o Windows PowerShell e .NET Framework 4.6 ou 4.5 estão instalados e se o PowerShell remoto está habilitado em cada nó no cluster. Para obter mais informações, consulte Configurar os nós de gerenciamento remoto em [requisitos e as práticas recomendadas para suporte a Cluster Atualizando](cluster-aware-updating-requirements.md).


|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**StopAfter**|Tempo ilimitado|Tempo em minutos após o qual a executar a atualização será interrompida se não for concluída. **Observação:** se você especificar uma atualização de pré-lançamento ou um script de PowerShell pós-atualização, todo o processo de execução de scripts e realizar atualizações deve ser concluído dentro do **StopAfter** limite de tempo.|  
|**WarnAfter**|Por padrão, nenhum aviso será exibido|Tempo em minutos após o qual um aviso será exibido se não tiver sido concluída a atualização de execução (incluindo um script de atualização de pré-lançamento e um script de pós-atualização, se eles estiverem configurados).|  
|**MaxRetriesPerNode**|3|Número máximo de vezes que o processo de atualização (incluindo um script de atualização de pré-lançamento e um script de pós-atualização, se eles estiverem configurados) será repetido por nó. O máximo é 64.|  
|**MaxFailedNodes**|Para a maioria dos clusters, um inteiro que é de aproximadamente um terço do número de nós de cluster|Número máximo de nós no qual a atualização poderá falhar, ou porque os nós falharem ou as paradas de serviço de Cluster em execução. Se um nó mais falhar, execute a atualização é interrompido.<br /><br /> O intervalo de valores válido é 0 para 1 menor que o número de nós de cluster.|  
|**RequireAllNodesOnline**|None|Especifica que todos os nós devem estar online e acessível antes de atualizar começa.|  
|**RebootTimeoutMinutes**|15|Tempo em minutos CAU permitirá para reiniciar um nó (se um reinício é necessário) e iniciar todos os serviços de inicialização automática. Se o processo de reinicialização não concluir dentro desse tempo, a atualização executados no nó é marcado como em falha.|  
|**PreUpdateScript**|None|O nome de arquivo e caminho de um script PowerShell para ser executado em cada nó antes de inicia a atualização, e antes que o nó é colocado no modo de manutenção. A extensão de nome de arquivo deve ser **. ps1**, e o comprimento total do nome do arquivo além de caminho não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco no armazenamento de cluster ou em um compartilhamento de arquivos de rede altamente disponíveis, para garantir que ela esteja sempre acessível a todos os nós de cluster. Se o script está localizado em um compartilhamento de rede, certifique-se de que você configure o compartilhamento de arquivos de permissão de leitura para todos em grupo e restringir o acesso de gravação para evitar a violação com os arquivos por usuários não autorizados.<br /><br /> Se você especificar um script de atualização de pré-lançamento, certifique-se de que limita a configurações como o tempo (por exemplo, **StopAfter**) estão configurados para permitir que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalar atualizações, não apenas o processo de instalação de atualizações.|  
|**PostUpdateScript**|None|O nome de arquivo e caminho para um script PowerShell para ser executado após a atualização for concluída (depois que o nó sai do modo de manutenção). A extensão de nome de arquivo deve ser **. ps1** e o comprimento total do nome do arquivo além de caminho não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco no armazenamento de cluster ou em um compartilhamento de arquivos de rede altamente disponíveis, para garantir que ela esteja sempre acessível a todos os nós de cluster. Se o script está localizado em um compartilhamento de rede, certifique-se de que você configure o compartilhamento de arquivos de permissão de leitura para todos em grupo e restringir o acesso de gravação para evitar a violação com os arquivos por usuários não autorizados.<br /><br /> Se você especificar um script de pós-atualização, certifique-se de que limita a configurações como o tempo (por exemplo, **StopAfter**) estão configurados para permitir que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalar atualizações, não apenas o processo de instalação de atualizações.|  
|**ConfigurationName**|Essa configuração só tem efeito se você executar scripts.<br /><br /> Se você especificar um script de atualização de pré-lançamento ou um script de pós-atualização, mas você não especificar um **ConfigurationName**, a sessão padrão configuração para PowerShell (PowerShell) é usada.|Especifica a configuração de sessão do PowerShell que define a sessão na quais scripts (especificado pela **PreUpdateScript** e **PostUpdateScript**) são executadas e pode limitar os comandos que podem ser executados.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Plug-ins que você configure o suporte a Cluster atualizando usar para visualizar atualizações ou realizar uma atualização executar. Para obter mais informações, consulte [plug-ins como suporte a Cluster atualizando funcionam](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|None|Um conjunto de *name = value* pares (argumentos) para a atualização plug-in use, por exemplo:<br /><br /> **Domain=Domain.local**<br /><br /> Esses *name = value* pares devem ser significativos para o plug-in que você especifica na **CauPluginName**.<br /><br /> Para especificar um argumento usando o CAU UI, digite o *nome*, pressione a tecla Tab e digite as correspondentes *valor*. Pressione a tecla Tab novamente para fornecer o argumento próximo. Cada *nome* e *valor* automaticamente são separadas por um sinal de igual (=). Vários pares automaticamente são separados por ponto e vírgula.<br /><br /> Para obter o padrão **Microsoft.WindowsUpdatePlugin** plug-in, sem argumentos são necessárias. No entanto, você pode especificar um argumento opcional, por exemplo, para especificar uma cadeia de caracteres de consulta Windows Update Agent padrão para filtrar o conjunto de atualizações que são aplicadas pelo plug-in. Para um *nome*, use **QueryString**e para um *valor*, coloque a consulta completa entre aspas.<br /><br /> Para obter mais informações, consulte [plug-ins como suporte a Cluster atualizando funcionam](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a>Opções que você especifica quando você solicita a executar uma atualização  
 A tabela a seguir lista Opções (que não sejam aqueles em um perfil de executar a atualização) que você pode especificar quando você solicita a executar uma atualização. Para obter informações sobre as opções que podem ser definidas em um perfil de executar a atualização, consulte a tabela anterior.  
  
|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**ClusterName**|None <br>**Observação:** essa opção deve ser definida somente quando o CAU UI não estiver em execução em um nó de cluster de failover, ou se quiser fazer referência a um cluster de failover diferente de onde o CAU UI é executado.|Nome NetBIOS do cluster na qual deseja realizar a executar a atualização.|  
|**Credenciais**|Credenciais de conta atual|Credenciais administrativas para o destino do cluster no qual a executar a atualização será executada. Você talvez já tenha as credenciais necessárias se você iniciar o CAU UI (ou abre uma sessão do PowerShell, se você estiver usando os cmdlets do PowerShell CAU) de uma conta que tenha direitos de administrador e permissões no cluster.|  
|**NodeOrder**|Por padrão, CAU começa com o nó que possui o menor número de funções clusterizados, em seguida, caminha para o nó que tem o número de segundo menor e assim por diante.|Nomes de nós de cluster na ordem em que eles devem ser atualizados (se possível).|  
  
##  <a name="BKMK_profile"></a>Usar perfis de execução atualizando  
 Executar cada atualização pode ser associado um perfil específico do atualizando executar. O padrão atualizando o perfil de execução é armazenado na *%windir%\cluster* pasta. Se você estiver usando o CAU UI na atualização remoto modo, você pode especificar um perfil de executar a atualização no momento em que você aplique as atualizações, ou você pode usar o perfil de executar a atualização do padrão. Se você estiver usando CAU no modo de atualização automática, você pode importar as configurações de uma atualização executar perfil especificado quando você configurar as opções de atualização automática. Em ambos os casos, você pode substituir os valores exibidos para as opções de atualização executar acordo com suas necessidades. Se você quiser, você pode salvar as opções de atualização executado como um perfil de executar a atualização com o mesmo nome de arquivo ou um nome de arquivo diferente. Na próxima vez que você aplica atualizações ou configurar as opções de atualização automática, CAU automaticamente seleciona o perfil de executar a atualização que anteriormente foi selecionado.  
  
 Você pode modificar um atualizando executar perfil existente ou criar um novo selecionando **criar ou modificar o perfil de execução Atualizando** na CAU UI.

Aqui estão algumas observações importantes sobre como usar perfis de executar a atualização:

* Um perfil de executar a atualização não armazena informações específicas do cluster, como credenciais administrativas. Se você estiver usando CAU no modo de atualização automática, o perfil de executar a atualização também não armazena as informações de agendamento de atualização automática. Isso torna possível compartilhar um perfil de executar a atualização em todos os clusters de failover em uma classe especificada.
* Se você configurar as opções de atualização automática usando um perfil de executar a atualização e posteriormente modificar o perfil com valores diferentes para as opções de atualização executado, a configuração de atualização automática não altera automaticamente. Para aplicar as novas configurações de executar a atualização, você deve configurar as opções de atualização automática novamente.
* O Editor de perfil executar Infelizmente não dá suporte a caminhos de arquivo que incluem espaços, como *C:\Program Files*. Como alternativa, armazene o pré e postar atualização scripts em um caminho que não incluem espaços ou use o PowerShell exclusivamente para gerenciar perfis de executar, colocando o caminho entre aspas ao executar **Invoke CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell
  
 Você pode importar as configurações de um perfil de executar a atualização quando você executa o **Invoke CauRun**, **adicionar CauClusterRole**, ou **conjunto CauClusterRole** cmdlet.  
  
 O exemplo a seguir executa uma verificação e um completo atualizando executados no cluster denominado *CONTOSO FC1*, usando as opções de atualização executar que são especificadas no *C:\Windows\Cluster\DefaultParameters.xml*. Valores padrão são usados para os parâmetros de cmdlet restantes.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 Usando um perfil de executar a atualização, você pode atualizar um cluster de failover na forma repetitiva com configurações consistentes para gerenciamento de exceção, limites de tempo e outros parâmetros operacionais. Como essas configurações são normalmente específicas a uma classe de clusters de failover — como "Clusters de todos os Microsoft SQL Server" ou "Meu clusters essenciais" — você pode querer nomear cada perfil de executar atualização de acordo com a classe de Clusters de Failover, ele será usado com. Além disso, é recomendável gerenciar o perfil de executar a atualização em um compartilhamento de arquivos que seja acessível a todos os clusters de failover de uma classe específica em sua organização de TI.  
  
  
  
## <a name="see-also"></a>Consulte também

-   [Suporte a cluster atualizando](cluster-aware-updating.md)
  
-   [Suporte a cluster Cmdlets de atualização no Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)