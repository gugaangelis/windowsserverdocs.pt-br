---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Cluster-Aware Updating opções avançadas e perfis de execução de atualização
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
description: Como configurar opções avançadas e perfis de execução para reconhecimento de Cluster de atualização de CAU (atualização)
ms.openlocfilehash: 5fac31ad35422e623b98caaabdd9eae183e2e5d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830547"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Cluster-Aware Updating opções avançadas e perfis de execução de atualização

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve opções de execução de atualização que podem ser configuradas para um [Cluster-Aware Updating](cluster-aware-updating.md) execução de atualização de (CAU). Essas opções avançadas podem ser configuradas quando você usa a UI do CAU ou os cmdlets do PowerShell do Windows de CAU para aplicar atualizações ou configurar opções de AutoAtualização.

É possível salvar a maioria das definições de configuração como um arquivo XML denominado Perfil de Execução de Atualização, que pode ser reutilizado para Execuções de Atualização posteriores. Os valores padrão das opções de Execução de Atualização, que são fornecidos pela CAU, também podem ser reutilizados em muitos ambientes de cluster.

Para obter informações sobre opções adicionais que você pode especificar para cada Execução de Atualização e sobre Perfis de Execução de Atualização, consulte as seguintes seções, posteriormente neste tópico:

Opções que você pode especificar ao solicitar uma atualização executar Use atualizando perfis opções de execução que podem ser definidas em um perfil de execução de atualização

A tabela a seguir lista as opções que podem ser definidas em uma Execução de Atualização da CAU. 

> [!NOTE] 
> Para definir a opção PreUpdateScript ou PostUpdateScript, certifique-se de que o Windows PowerShell e .NET Framework 4.6 ou 4.5 estão instalados e que a comunicação remota do PowerShell é habilitada em cada nó no cluster. Para obter mais informações, consulte Configurar os nós para gerenciamento remoto no [requisitos e práticas recomendadas para atualização com suporte a Cluster](cluster-aware-updating-requirements.md).


|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**StopAfter**|Tempo ilimitado|O tempo em minutos após o qual a Execução de Atualização será interrompida se não tiver sido concluída. **Observação:**  Se você especificar um anterior à atualização ou um script do PowerShell de pós-atualização, todo o processo de execução de scripts e atualizações deve ser concluído dentro de **StopAfter** limite de tempo.|  
|**WarnAfter**|Por padrão, nenhum aviso é exibido|Tempo em minutos após o qual um aviso será exibido se a Execução de Atualização (incluindo um script de pré-atualização e de pós-atualização, se estiverem configurados) não tiver sido concluída.|  
|**MaxRetriesPerNode**|3|Número máximo de vezes que o processo de atualização (incluindo um script de pré-atualização e de pós-atualização, se estiverem configurados) será repetido por nó. O máximo é 64.|  
|**MaxFailedNodes**|Para a maioria dos clusters, um número inteiro que corresponde a aproximadamente um terço do número de nós do cluster|O número máximo de nós em que a atualização pode falhar devido a falhas nos nós ou interrupção do Serviço de cluster. Se um ou mais nós falharem, a Execução de Atualização será interrompida.<br /><br /> O intervalo válido de valores é de 0 a 1, menor do que número de nós do cluster.|  
|**RequireAllNodesOnline**|Nenhuma|Especifica se todos os nós devem estar online e acessíveis antes de começar a atualização.|  
|**RebootTimeoutMinutes**|15|Tempo em minutos durante o qual a CAU permitirá a reinicialização de um nó (caso seja necessária uma reinicialização) e a inicialização de todos os serviços de início automático. Se o processo de reinicialização não for concluída nesse período, a execução de atualização nesse nó será marcada como com falha.|  
|**PreUpdateScript**|Nenhuma|O nome de arquivo e caminho para um script do PowerShell ser executado em cada nó antes do início da atualização e antes que o nó seja colocado em modo de manutenção. A extensão de nome de arquivo deve ser **. ps1**, e o comprimento total do caminho e arquivo o nome não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco, no armazenamento de cluster, ou em um compartilhamento de arquivos de rede com alta disponibilidade, para garantir que estará sempre acessível em todos os nós do cluster. Se o script estiver localizado em um compartilhamento de arquivos de rede, configure o compartilhamento de arquivos com a permissão de leitura para o grupo Todos e restrinja o acesso para gravação para impedir que usuários não autorizados falsifiquem os arquivos.<br /><br /> Se você especificar um script de pré-atualização, verifique se algumas configurações, como limites de tempo (por exemplo, **StopAfter**), estão configuradas para que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalação de atualizações, não apenas o processo de instalação de atualizações.|  
|**PostUpdateScript**|Nenhuma|O nome de arquivo e caminho para um script do PowerShell a ser executado após a conclusão da atualização (depois que o nó sai do modo de manutenção). A extensão de nome de arquivo deve ser **. ps1** e o comprimento total do caminho e arquivo o nome não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco, no armazenamento de cluster, ou em um compartilhamento de arquivos de rede com alta disponibilidade, para garantir que estará sempre acessível em todos os nós do cluster. Se o script estiver localizado em um compartilhamento de arquivos de rede, configure o compartilhamento de arquivos com a permissão de leitura para o grupo Todos e restrinja o acesso para gravação para impedir que usuários não autorizados falsifiquem os arquivos.<br /><br /> Se você especificar um script de pós-atualização, verifique se algumas configurações, como limites de tempo (por exemplo, **StopAfter**), estão configuradas para que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalação de atualizações, não apenas o processo de instalação de atualizações.|  
|**ConfigurationName**|Essa configuração só terá efeito se você executar scripts.<br /><br /> Se você especificar um script de pré-atualização ou pós-atualização, mas você não especificar uma **ConfigurationName**, a sessão padrão de configuração para o PowerShell (PowerShell) é usada.|Especifica a configuração de sessão do PowerShell que define a sessão na qual os scripts (especificados por **PreUpdateScript** e **PostUpdateScript**) são executados e pode limitar os comandos que podem ser executados.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Plug-in no qual você configura a Atualização com Suporte a Cluster a ser usada para visualizar atualizações ou executar uma Execução de Atualização. Para obter mais informações, consulte [como o Cluster-Aware Updating plug-ins funcionam](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Nenhuma|Um conjunto de pares (argumentos) *nome=valor* do plug-in para atualização a ser usado, por exemplo:<br /><br /> **Domain=Domain.local**<br /><br /> Esses pares *nome=valor* devem ser significativos para o plug-in especificado em **CauPluginName**.<br /><br /> Para especificar um argumento usando a interface do usuário da CAU, digite o *name* (nome), pressione a tecla Tab e digite o *value* (valor) correspondente. Pressione a tecla TAB novamente para fornecer o próximo argumento. Cada *nome* e *valor* é automaticamente separado por um sinal de igual (=). Vários pares são separados automaticamente por ponto e vírgula.<br /><br /> Para o padrão **windowsupdateplugin** plug-in, nenhum argumento é necessário. Entretanto, é possível especificar um argumento opcional; por exemplo, para definir um consulta padrão do Windows Update Agent para filtrar o conjunto de atualizações a ser aplicado pelo plug-in. Para um *nome*, use **QueryString**e para uma *valor*, coloque a consulta inteira entre aspas.<br /><br /> Para obter mais informações, consulte [como o Cluster-Aware Updating plug-ins funcionam](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a> Opções que você especifica ao solicitar uma execução de atualização  
 A tabela a seguir lista opções (que não aquelas em um Perfil de Execução de Atualização) que você pode especificar ao solicitar uma Execução de Atualização. Para obter informações sobre as opções que podem ser definidas em um Perfil de Execução de Atualização, consulte a tabela anterior.  
  
|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**ClusterName**|Nenhuma <br>**Observação:**  Essa opção só deverá ser configurada se a interface do usuário da CAU não for executada em um nó de cluster de failover ou quando você quiser referenciar um cluster de failover diferente daquele em que a interface do usuário da CAU está em execução.|Nome do NetBIOS no qual a Execução de Atualização será realizada.|  
|**Credential**|Credenciais atuais da conta|Credenciais administrativas do cluster de destino no qual realizar a Execução de Atualização. Você talvez já tenha as credenciais necessárias se iniciar UI CAU (ou abra uma sessão do PowerShell, se você estiver usando os cmdlets do PowerShell de CAU) de uma conta que tenha permissões e direitos de administrador no cluster.|  
|**NodeOrder**|Por padrão, a CAU é iniciada com o nó que contém o menor número de funções clusterizadas; depois, avança para o nó com o segundo menor número de funções e assim por diante.|Nomes dos nós do cluster na ordem em que precisam ser atualizados (se possível).|  
  
##  <a name="BKMK_profile"></a> Use perfis de execução de atualização  
 Cada Execução de Atualização pode ser associada a um Perfil de Execução de Atualização específico. O padrão de atualização de perfil de execução é armazenado na *%Windir%\Cluster.* pasta. Se você estiver usando a UI CAU em atualização remota modo, você pode especificar um perfil de execução de atualização no momento em que você aplique as atualizações, ou você pode usar o perfil de execução de atualização padrão. Se você estiver usando o CAU no modo de AutoAtualização, você pode importar as configurações de um especificado Atualizando perfil de execução quando você configura as opções de AutoAtualização. Nos dois casos, é possível substituir os valores exibidos para as opções de Execução de Atualização, de acordo com as suas necessidades. Se quiser, você poderá salvar as opções de Execução de Atualização como um Perfil de Execução de Atualização, usando o mesmo nome de arquivo ou um diferente. Na próxima vez que aplicar atualizações ou configurar opções de autoatualização, a CAU selecionará automaticamente o Perfil de Execução de Atualização escolhido antes.  
  
 Você pode modificar um existente Atualizando perfil de execução ou criar um novo, selecionando **criar ou modificar o perfil de execução de atualização** na UI do CAU.

Aqui estão algumas observações importantes sobre como usar perfis de execução de atualização:

* Um perfil de execução de atualização não armazena informações específicas do cluster, como as credenciais administrativas. Se você estiver usando o CAU no modo de AutoAtualização, o perfil de execução de atualização também não armazena as informações de agenda de AutoAtualização. Isso torna possível o compartilhamento de um Perfil de Execução de Atualização em todos os clusters de failover de uma classe específica.
* Se você configurar opções de AutoAtualização usando um perfil de execução de atualização e depois modificar o perfil com valores diferentes para as opções de execução de atualização, a configuração de AutoAtualização não será alterado automaticamente. Para aplicar as novas configurações da Execução de Atualização, configure novamente as opções de autoatualização.
* O Editor de perfil de execução Infelizmente não dá suporte a caminhos de arquivo que incluem espaços, como *C:\Program Files*. Como alternativa, armazenar o pré e publicar scripts de atualização em um caminho que não inclua espaços ou usar o PowerShell exclusivamente para gerenciar perfis de execução, colocando o caminho entre aspas quando em execução **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell
  
 Você pode importar as configurações de um perfil de execução de atualização, quando você executa o **Invoke-CauRun**, **Add-CauClusterRole**, ou **Set-CauClusterRole** cmdlet.  
  
 O exemplo a seguir realiza uma verificação e uma Execução de Atualização completa no cluster denominado *CONTOSO-FC1* usando as opções de Execução de Atualização especificadas em *C:\Windows\Cluster\DefaultParameters.xml*. Os valores padrão são usados para o restante dos parâmetros de cmdlet.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 Usando um Perfil de Execução de Atualização, você pode atualizar repetidamente um cluster de failover, com definições consistentes ao gerenciamento de exceções, tempo associado e outros parâmetros operacionais. Essas configurações são tipicamente específicas de uma classe de clusters de failover – como "Todos os clusters do Microsoft SQL Server" ou "Meus clusters essenciais aos negócios" – e, por isso, pode ser conveniente nomear cada Perfil de Execução de Atualização de acordo com a classe de Clusters de Failover com a qual serão usadas. Também pode ser conveniente gerenciar o Perfil de Execução de Atualização em um compartilhamento de arquivos que seja acessível a todos os clusters de failover de uma determinada classe, na sua organização de TI.  
  
  
  
## <a name="see-also"></a>Consulte também

-   [Cluster-Aware Updating](cluster-aware-updating.md)
  
-   [Cmdlets de atualização com suporte a cluster no Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)