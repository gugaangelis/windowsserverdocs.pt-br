---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Opções avançadas de atualização com suporte a cluster e atualização de perfis de execução
description: Como configurar opções avançadas e atualizar perfis de execução para a atualização com suporte a cluster (CAU)
ms.topic: article
ms.prod: windows-server
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
ms.openlocfilehash: b6a5b51b0942685334c79d9523fffa5ba4eabc5f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720600"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Opções avançadas de atualização com suporte a cluster e atualização de perfis de execução

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012.

Este tópico descreve as opções de execução de atualização que podem ser configuradas para uma execução de atualização da cau ( [atualização com suporte a cluster](cluster-aware-updating.md) ). Essas opções avançadas podem ser configuradas quando você usa a interface do usuário da CAU ou os cmdlets do Windows PowerShell da CAU para aplicar atualizações ou para configurar opções de autoatualização.

É possível salvar a maioria das definições de configuração como um arquivo XML denominado Perfil de Execução de Atualização, que pode ser reutilizado para Execuções de Atualização posteriores. Os valores padrão das opções de Execução de Atualização, que são fornecidos pela CAU, também podem ser reutilizados em muitos ambientes de cluster.

Para obter informações sobre opções adicionais que você pode especificar para cada Execução de Atualização e sobre Perfis de Execução de Atualização, consulte as seguintes seções, posteriormente neste tópico:

As opções que você especifica ao solicitar uma execução de atualização usam opções de perfis de execução de atualização que podem ser definidas em um perfil de execução de atualização

A tabela a seguir lista as opções que podem ser definidas em uma Execução de Atualização da CAU. 

> [!NOTE] 
> Para definir a opção PreUpdateScript ou PostUpdateScript, verifique se o Windows PowerShell e o .NET Framework 4,6 ou 4,5 estão instalados e se a comunicação remota do PowerShell está habilitada em cada nó no cluster. Para obter mais informações, consulte Configurar os nós para gerenciamento remoto em [requisitos e práticas recomendadas para atualização com suporte a cluster](cluster-aware-updating-requirements.md).


|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**StopAfter**|Tempo ilimitado|O tempo em minutos após o qual a Execução de Atualização será interrompida se não tiver sido concluída. **Observação:**  Se você especificar um script de pré-atualização ou pós-atualização do PowerShell, todo o processo de execução de scripts e execução de atualizações deverá ser concluído dentro do limite de tempo de **StopAfter** .|  
|**WarnAfter**|Por padrão, nenhum aviso é exibido|Tempo em minutos após o qual um aviso será exibido se a Execução de Atualização (incluindo um script de pré-atualização e de pós-atualização, se estiverem configurados) não tiver sido concluída.|  
|**MaxRetriesPerNode**|3|Número máximo de vezes que o processo de atualização (incluindo um script de pré-atualização e de pós-atualização, se estiverem configurados) será repetido por nó. O máximo é 64.|  
|**MaxFailedNodes**|Para a maioria dos clusters, um número inteiro que corresponde a aproximadamente um terço do número de nós do cluster|O número máximo de nós em que a atualização pode falhar devido a falhas nos nós ou interrupção do Serviço de cluster. Se um ou mais nós falharem, a Execução de Atualização será interrompida.<p>O intervalo válido de valores é de 0 a 1, menor do que número de nós do cluster.|  
|**RequireAllNodesOnline**|Nenhum|Especifica se todos os nós devem estar online e acessíveis antes de começar a atualização.|  
|**RebootTimeoutMinutes**|15|Tempo em minutos durante o qual a CAU permitirá a reinicialização de um nó (caso seja necessária uma reinicialização) e a inicialização de todos os serviços de início automático. Se o processo de reinicialização não for concluído dentro desse período, a execução da atualização nesse nó será marcada como com falha.|  
|**PreUpdateScript**|Nenhum|O caminho e o nome de arquivo para um script do PowerShell a ser executado em cada nó antes do início da atualização e antes que o nó seja colocado no modo de manutenção. A extensão de nome de arquivo deve ser **. ps1**e o comprimento total do caminho mais o nome de arquivo não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco, no armazenamento de cluster, ou em um compartilhamento de arquivos de rede com alta disponibilidade, para garantir que estará sempre acessível em todos os nós do cluster. Se o script estiver localizado em um compartilhamento de arquivos de rede, configure o compartilhamento de arquivos com a permissão de leitura para o grupo Todos e restrinja o acesso para gravação para impedir que usuários não autorizados falsifiquem os arquivos.<p> Se você especificar um script de pré-atualização, verifique se algumas configurações, como limites de tempo (por exemplo, **StopAfter**), estão configuradas para que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalação de atualizações, não apenas o processo de instalação de atualizações.|  
|**PostUpdateScript**|Nenhum|O caminho e o nome de arquivo para um script do PowerShell a ser executado após a conclusão da atualização (depois que o nó sair do modo de manutenção). A extensão de nome de arquivo deve ser **. ps1** e o comprimento total do caminho mais o nome de arquivo não deve exceder 260 caracteres. Como prática recomendada, o script deve estar localizado em um disco, no armazenamento de cluster, ou em um compartilhamento de arquivos de rede com alta disponibilidade, para garantir que estará sempre acessível em todos os nós do cluster. Se o script estiver localizado em um compartilhamento de arquivos de rede, configure o compartilhamento de arquivos com a permissão de leitura para o grupo Todos e restrinja o acesso para gravação para impedir que usuários não autorizados falsifiquem os arquivos.<p> Se você especificar um script de pós-atualização, verifique se algumas configurações, como limites de tempo (por exemplo, **StopAfter**), estão configuradas para que o script seja executado com êxito. Esses limites abrangem todo o processo de execução de scripts e instalação de atualizações, não apenas o processo de instalação de atualizações.|  
|**ConfigurationName**|Essa configuração só terá efeito se você executar scripts.<p> Se você especificar um script de pré-atualização ou um script de pós-atualização, mas não especificar um **ConfigurationName**, a configuração de sessão padrão para o PowerShell (Microsoft. PowerShell) será usada.|Especifica a configuração de sessão do PowerShell que define a sessão na qual os scripts (especificados por **PreUpdateScript** e **PostUpdateScript**) são executados e podem limitar os comandos que podem ser executados.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Plug-in no qual você configura a Atualização com Suporte a Cluster a ser usada para visualizar atualizações ou executar uma Execução de Atualização. Para obter mais informações, consulte [como funcionam os plug-ins de atualização com suporte a cluster](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Nenhum|Um conjunto de pares (argumentos) *nome=valor* do plug-in para atualização a ser usado, por exemplo:<p>**Domínio = domínio. local**<p>Esses pares *nome=valor* devem ser significativos para o plug-in especificado em **CauPluginName**.<p>Para especificar um argumento usando a interface do usuário da CAU, digite o *name* (nome), pressione a tecla Tab e digite o *value* (valor) correspondente. Pressione a tecla TAB novamente para fornecer o próximo argumento. Cada *nome* e *valor* é automaticamente separado por um sinal de igual (=). Vários pares são separados automaticamente por ponto e vírgula.<p>Para o plug-in padrão **Microsoft. WindowsUpdatePlugin** , nenhum argumento é necessário. Entretanto, é possível especificar um argumento opcional; por exemplo, para definir um consulta padrão do Windows Update Agent para filtrar o conjunto de atualizações a ser aplicado pelo plug-in. Para um *nome*, use **QueryString**e, para um *valor*, coloque a consulta completa entre aspas.<p> Para obter mais informações, consulte [como funcionam os plug-ins de atualização com suporte a cluster](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="options-that-you-specify-when-you-request-an-updating-run"></a><a name="BKMK_runtime"></a>Opções que você especifica ao solicitar uma execução de atualização  
 A tabela a seguir lista opções (que não aquelas em um Perfil de Execução de Atualização) que você pode especificar ao solicitar uma Execução de Atualização. Para obter informações sobre as opções que podem ser definidas em um Perfil de Execução de Atualização, consulte a tabela anterior.  
  
|Opção|Valor padrão|Detalhes|  
|------------|-------------------|-------------|  
|**ClusterName**|Nenhum <br>**Observação:**  Essa opção deve ser definida somente quando a interface do usuário da CAU não é executada em um nó de cluster de failover ou você deseja fazer referência a um cluster de failover diferente de onde a interface do usuário da CAU é executada.|Nome do NetBIOS no qual a Execução de Atualização será realizada.|  
|**Provedores**|Credenciais atuais da conta|Credenciais administrativas do cluster de destino no qual realizar a Execução de Atualização. Talvez você já tenha as credenciais necessárias se iniciar a interface do usuário da CAU (ou abrir uma sessão do PowerShell, se você estiver usando os cmdlets do PowerShell da CAU) de uma conta que tenha direitos de administrador e permissões no cluster.|  
|**NodeOrder**|Por padrão, a CAU é iniciada com o nó que contém o menor número de funções clusterizadas; depois, avança para o nó com o segundo menor número de funções e assim por diante.|Nomes dos nós do cluster na ordem em que precisam ser atualizados (se possível).|  
  
##  <a name="use-updating-run-profiles"></a><a name="BKMK_profile"></a>Usar perfis de execução de atualização  
 Cada Execução de Atualização pode ser associada a um Perfil de Execução de Atualização específico. O perfil de execução de atualização padrão é armazenado na pasta *%windir%\Cluster* Se você estiver usando a interface do usuário da CAU no modo de atualização remota, poderá especificar um perfil de execução de atualização no momento em que aplicar as atualizações ou usar o perfil de execução de atualização padrão. Se você estiver usando a CAU no modo de autoatualização, poderá importar as configurações de um perfil de execução de atualização especificado ao configurar as opções de autoatualização. Nos dois casos, é possível substituir os valores exibidos para as opções de Execução de Atualização, de acordo com as suas necessidades. Se quiser, você poderá salvar as opções de Execução de Atualização como um Perfil de Execução de Atualização, usando o mesmo nome de arquivo ou um diferente. Na próxima vez que aplicar atualizações ou configurar opções de autoatualização, a CAU selecionará automaticamente o Perfil de Execução de Atualização escolhido antes.  
  
 Você pode modificar um perfil de execução de atualização existente ou criar um novo selecionando **criar ou modificar perfil de execução de atualização** na interface do usuário da cau.

Aqui estão algumas observações importantes sobre o uso de perfis de execução de atualização:

* Um perfil de execução de atualização não armazena informações específicas do cluster, como credenciais administrativas. Se você estiver usando a CAU no modo de autoatualização, o perfil de execução de atualização também não armazenará as informações de agendamento de autoatualização. Isso torna possível o compartilhamento de um Perfil de Execução de Atualização em todos os clusters de failover de uma classe específica.
* Se você configurar opções de autoatualização usando um perfil de execução de atualização e posteriormente modificar o perfil com valores diferentes para as opções de execução de atualização, a configuração de autoatualização não será alterada automaticamente. Para aplicar as novas configurações da Execução de Atualização, configure novamente as opções de autoatualização.
* O editor de perfil de execução infelizmente não dá suporte a caminhos de arquivo que incluem espaços, como *c:\Arquivos de programas*. Como alternativa, armazene seus scripts anteriores e pós-atualização em um caminho que não inclua espaços ou use o PowerShell exclusivamente para gerenciar perfis de execução, colocando aspas em torno do caminho ao executar **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell
  
 Você pode importar as configurações de um perfil de execução de atualização ao executar o cmdlet **Invoke-CauRun**, **Add-CauClusterRole**ou **set-CauClusterRole** .  
  
 O exemplo a seguir realiza uma verificação e uma Execução de Atualização completa no cluster denominado *CONTOSO-FC1* usando as opções de Execução de Atualização especificadas em *C:\Windows\Cluster\DefaultParameters.xml*. Os valores padrão são usados para o restante dos parâmetros de cmdlet.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 Usando um Perfil de Execução de Atualização, você pode atualizar repetidamente um cluster de failover, com definições consistentes ao gerenciamento de exceções, tempo associado e outros parâmetros operacionais. Como essas configurações normalmente são específicas para uma classe de clusters de failover — como "todos os clusters Microsoft SQL Server" ou "meus clusters críticos para os negócios" — Talvez você queira nomear cada perfil de execução de atualização de acordo com a classe de clusters de failover com a qual ele será usado. Também pode ser conveniente gerenciar o Perfil de Execução de Atualização em um compartilhamento de arquivos que seja acessível a todos os clusters de failover de uma determinada classe, na sua organização de TI.  
  
  
  
## <a name="see-also"></a>Confira também

-   [Atualização com suporte a cluster](cluster-aware-updating.md)
  
-   [Cmdlets de atualização com suporte a cluster no Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)