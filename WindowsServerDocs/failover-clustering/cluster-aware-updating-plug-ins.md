---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Como funcionam os plug-ins de atualização com suporte a cluster
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Como usar plug-ins para coordenar atualizações ao usar a atualização com suporte a cluster no Windows Server para instalar atualizações em um cluster.
ms.openlocfilehash: f6c572a397530704dd91d9c67c5c1758ccc085c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361292"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Como funcionam os plug-ins de atualização com suporte a cluster

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A cau ( [atualização com suporte a cluster](cluster-aware-updating.md) ) usa plug-ins para coordenar a instalação de atualizações entre nós em um cluster de failover. Este tópico fornece informações sobre como usar o plug-in (no__t) do 0in CAU @ no__t-1ins interno ou outro plug @ no__t-2ins que você instala para a CAU.

## <a name="BKMK_INSTALL"></a>Instalar um plug @ no__t-1in  
Um plug @ no__t-0in diferente do padrão plug @ no__t-1ins que são instalados com a CAU \(**Microsoft. WindowsUpdatePlugin** e **microsoft. HotfixPlugin**\) devem ser instalados separadamente. Se a CAU for usada no modo Self @ no__t-0updating, o plug @ no__t-1in deverá ser instalado em todos os nós de cluster. Se a CAU for usada no modo @ no__t-0updating remoto, o plug @ no__t-1in deverá ser instalado no computador do coordenador de atualização remota. Um plug @ no__t-0in que você instala pode ter requisitos de instalação adicionais em cada nó.  
  
Para instalar um plug @ no__t-0in, siga as instruções do Publicador do plug @ no__t-1in. Para registrar manualmente um plug @ no__t-0in com a CAU, execute o cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) em cada computador em que o plug @ no__t-2in está instalado.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar os argumentos de plug @ no__t-0in e plug @ no__t-1in  
  
### <a name="specify-a-cau-plug-in"></a>Especificar um plug-in CAU @ no__t-0in

Na interface do usuário da CAU, selecione um plug @ no__t-0in em uma lista drop @ no__t-1down de plug @ no__t-2ins disponíveis ao usar a CAU para executar as seguintes ações:  
  
-   Aplicar atualizações ao cluster  
  
-   Visualizar atualizações para o cluster  
  
-   Configurar opções do cluster Self @ no__t-0updating  
  
Por padrão, a CAU seleciona o plug @ no__t-0in **Microsoft. WindowsUpdatePlugin**. No entanto, você pode especificar qualquer plug @ no__t-0in que esteja instalado e registrado com a CAU.

> [!TIP]  
> Na interface do usuário da CAU, você só pode especificar um único plug @ no__t-0in para a CAU usar para visualizar ou aplicar atualizações durante uma execução de atualização. Usando os cmdlets do PowerShell da CAU, você pode especificar um ou mais plug @ no__t-0ins. Se você precisar instalar vários tipos de atualizações no cluster, normalmente será mais eficiente especificar vários plug @ no__t-0ins em uma execução de atualização, em vez de usar uma execução de atualização separada para cada plug @ no__t-1in. Por exemplo, normalmente ocorrerá um número menor de reinicializações de nós.

Usando os cmdlets do PowerShell da CAU listados na tabela a seguir, você pode especificar um ou mais plug @ no__t-0ins para uma execução ou verificação de atualização passando o parâmetro **– CauPluginName** . Você pode especificar vários nomes de plug @ no__t-0in separando-os com vírgulas. Se você especificar vários plug @ no__t-0ins, também poderá controlar como o plug @ no__t-1ins influenciará um ao outro durante uma execução de atualização especificando o **\-RunPluginsSerially**, **\-StopOnPluginFailure**e **– SeparateReboots** parâmetros. Para obter mais informações sobre como usar vários plug-ins @ no__t-0ins, use os links fornecidos para a documentação do cmdlet na tabela a seguir.  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Adiciona a função clusterizada CAU que fornece a funcionalidade Self @ no__t-0updating ao cluster especificado.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e instala essas atualizações através de uma Execução de Atualização no cluster especificado.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e retorna uma lista do conjunto inicial de atualizações que seriam aplicadas a cada nó no cluster especificado.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Define as propriedades de configuração para a função clusterizada do CAU no cluster especificado.|  
  
Se você não especificar um parâmetro de plug-0in do CAU do no__t usando esses cmdlets, o padrão será o plug @ no__t-1in **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar argumentos do plug-0in do CAU. no__t  
Ao configurar as opções de execução de atualização, você pode especificar um ou mais pares *nome @ no__t-1value* \(arguments @ no__t-3 para que o plug @ no__t-4in selecionado seja usado. Por exemplo, na IU do CAU, é possível especificar vários argumentos, da seguinte forma:  
  
**Nome1 @ no__t-1Value1; Nome2 @ no__t-2Value2; Name3 @ no__t-3Value3**  
  
Esses pares de *nome @ no__t-1value* devem ser significativos para o plug @ no__t-2in que você especificar. Para alguns plug @ no__t-0ins, os argumentos são opcionais.  
  
A sintaxe dos argumentos de plug @ no__t-0in da CAU segue estas regras gerais:  
  
-   Vários pares *de nome @ no__t-1value* são separados por ponto e vírgula.  
  
-   Um valor que contém espaços fica entre aspas, por exemplo: **Nome1 @ no__t-1 "valor com espaços"** .  
  
-   A sintaxe exata do *valor* depende do plug @ no__t-1in.  
  
Para especificar argumentos plug @ no__t-0in usando os cmdlets do PowerShell da CAU que dão suporte ao parâmetro **– CauPluginParameters** , passe um parâmetro do formulário:  
  
**\-CauPluginArguments @ {Nome1 @ no__t-2Value1; Nome2 @ no__t-3Value2; Name3 @ no__t-4Value3}**  
  
Você também pode usar uma tabela de hash predefinida do PowerShell. Para especificar argumentos plug @ no__t-0in para mais de um plug @ no__t-1in, passe várias tabelas de hash de argumentos, separados por vírgulas. Passe os argumentos plug @ no__t-0in na ordem plug @ no__t-1in especificada em **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar argumentos opcionais Plug @ no__t-0in  
O plug @ no__t-0ins que o CAU instala \(**Microsoft. WindowsUpdatePlugin** e **microsoft. HotfixPlugin**\) fornecem opções adicionais que podem ser selecionadas. Na interface do usuário da CAU, eles aparecem em uma página de **Opções adicionais** depois de configurar as opções de execução de atualização para o plug @ no__t-1in. Se você estiver usando os cmdlets do PowerShell da CAU, essas opções serão configuradas como argumentos plug @ no__t-0in opcionais. Para obter mais informações, consulte [Use o Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usar o Microsoft.HotfixPlugin](#BKMK_HFP) mais adiante neste tópico.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gerenciar plug @ no__t-0ins usando cmdlets do Windows PowerShell  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Get-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Recupera informações sobre uma ou mais atualizações de software plug @ no__t-0ins que estão registradas no computador local.|  
|[Registrar-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Registra um plug-in de atualização de software CAU @ no__t-0in no computador local.|  
|[Cancelar registro-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Remove uma atualização de software plug @ no__t-0in da lista de plug @ no__t-1ins que pode ser usada pela CAU. **Observação:** O plug @ no__t-0ins instalado com a CAU \(**Microsoft. WindowsUpdatePlugin** e o **microsoft. HotfixPlugin**\) não podem ter o registro cancelado.|  
  
## <a name="BKMK_WUP"></a>Usando o Microsoft. WindowsUpdatePlugin  

O padrão plug @ no__t-0in para CAU, **Microsoft. WindowsUpdatePlugin**, executa as seguintes ações:
- Comunica-se com o Windows Update Agent em cada nó do cluster de failover para aplicar atualizações necessárias aos produtos da Microsoft que estão sendo executados em cada nó.
- Instala atualizações de cluster diretamente do Windows Update ou Microsoft Update, ou de um no servidor @ no__t-0premises Windows Server Update Services \(WSUS @ no__t-2.
- Instala somente a versão de distribuição geral selecionada \(GDR @ no__t-1 atualizações. Por padrão, o plug @ no__t-0in aplica-se apenas a atualizações de software importantes. Nenhuma configuração é necessária. A configuração padrão baixa e instala atualizações importantes de GDR em cada nó. 

> [!NOTE]
> Para aplicar atualizações diferentes das atualizações de software importantes que são selecionadas por padrão \(for exemplo, atualizações de driver @ no__t-1, você pode configurar um parâmetro opcional plug @ no__t-2in. Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador do coordenador de atualização remota \(if usado @ no__t-1 devem atender aos requisitos da CAU e à configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas para a cau](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para aplicação de atualizações da Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) e faça as alterações necessárias à configuração do Microsoft Update para os nós do cluster de failover.
- Para obter melhores resultados, recomendamos que você execute o Analisador de Práticas Recomendadas CAU \(BPA @ no__t-1 para garantir que o cluster e o ambiente de atualização estejam configurados corretamente para aplicar atualizações usando a CAU. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> As atualizações que exigem aceitação de termos de licença da Microsoft ou interação do usuário são excluídas, e devem ser instaladas manualmente.

### <a name="additional-options"></a>Opções Adicionais

Opcionalmente, você pode especificar os seguintes argumentos de plug @ no__t-0in para aumentar ou restringir o conjunto de atualizações que são aplicadas pelo plug @ no__t-1in:
- Para configurar o plug @ no__t-0in para aplicar as atualizações recomendadas, além de atualizações importantes em cada nó, na interface do usuário da CAU, na página **Opções adicionais** , selecione a opção **fornecer atualizações recomendadas da mesma forma que recebo a verificação de atualizações importantes** quadro.
<br>Como alternativa, configure o argumento **' IncludeRecommendedUpdates ' \= ' true '** plug @ no__t-2in.
- Para configurar o plug @ no__t-0in para filtrar os tipos de atualizações GDR que são aplicadas a cada nó de cluster, especifique um Windows Update cadeia de consulta de agente usando um argumento **QueryString** plug @ no__t-2in. Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar a cadeia de consulta do agente de Windows Update  
Você pode configurar um argumento de plug @ no__t-0in para o padrão plug @ no__t-1in, **Microsoft. WindowsUpdatePlugin**, que consiste em um agente de Windows Update \(WUA @ no__t-4 cadeia de caracteres de consulta. Essa instrução usa a API do WUA para identificar um ou mais grupos de atualizações da Microsoft para aplicar a cada nó, com base em critérios de seleção específicos. Você pode combinar vários critérios, usando um AND ou um OR lógico. A cadeia de caracteres de consulta do WUA é especificada em um argumento plug @ no__t-0in da seguinte maneira:  
  
**QueryString @ no__t-1 "Critérion1 @ no__t-2Value1 e @ no__t-3or Criterion2 @ no__t-4Value2 e @ no__t-5or..."**  
  
Por exemplo, o **Microsoft.WindowsUpdatePlugin** seleciona automaticamente as atualizações importantes, usando um argumento padrão **QueryString** que é construído usando os critérios **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:  
  
**QueryString @ no__t-1 "IsInstalled @ no__t-20 e Type @ no__t-3'Software ' e IsHidden @ no__t-40 e isassign @ no__t-51"**  
  
Se você especificar um argumento **QueryString** , ele será usado no lugar da **QueryString** padrão configurada para o plug @ no__t-2in.  
  
#### <a name="example-1"></a>Exemplo 1
  
Para configurar um argumento **QueryString** que instala uma atualização específica, conforme identificado pela ID *f6ce46c1 @ no__t-2971c @ no__t-343f9 @ no__t-4a2aa @ no__t-5783df125f003*:  
  
**QueryString @ no__t-1 "UpdateId @ no__t-2'f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003 ' e IsInstalled @ no__t-70"**  
  
> [!NOTE]  
> O exemplo anterior é válido para aplicar atualizações usando o assistente de atualização do cluster @ no__t-0Aware. Se você quiser instalar uma atualização específica configurando as opções Self @ no__t-0updating com a interface do usuário da CAU ou usando o cmdlet **Add @ no__t-2CauClusterRole** ou **set @ no__t-4CauClusterRole**do PowerShell, deverá formatar o valor UpdateId com dois caracteres únicos @ no__t-5quote:  
>   
> **QueryString @ no__t-1 "UpdateId @ no__t-2''f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003 ' ' e isinstaled @ no__t-70"**  
  
#### <a name="example-2"></a>Exemplo 2
  
Para configurar um argumento **QueryString** que instala somente drivers:  
  
**QueryString @ no__t-1 "IsInstalled @ no__t-20 e Type @ no__t-3'Driver ' e IsHidden @ no__t-40"**  
  
Para obter mais informações sobre cadeias de caracteres de consulta para o padrão plug @ no__t-0in, **Microsoft. WindowsUpdatePlugin**, os critérios de pesquisa \(such como **isinstalled**\) e a sintaxe que você pode incluir nas cadeias de caracteres de consulta, consulte a seção sobre os critérios de pesquisa na [referência da API do WUA (agente de Windows Update)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Use o Microsoft. HotfixPlugin  
O plug @ no__t-0in **Microsoft. HotfixPlugin** pode ser usado para aplicar a versão de distribuição limitada da Microsoft \(LDR @ no__t-3 updates \(also chamado hotfixes e, anteriormente chamado de QFEs @ no__t-5, que você baixa de forma independente para resolver problemas específicos de software da Microsoft. O plug-in instala atualizações de uma pasta raiz em um compartilhamento de arquivos SMB e também pode ser personalizado para aplicar atualizações de BIOS, firmware e driver não no__t-0Microsoft.

> [!NOTE]
> Os hotfixes às vezes estão disponíveis para download da Microsoft nos artigos da base de dados de conhecimento, mas eles também são fornecidos aos clientes em uma base @ no__t-0needed.

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador do coordenador de atualização remota \(if usado @ no__t-1 devem atender aos requisitos da CAU e à configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas para a cau](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para uso do Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obter melhores resultados, recomendamos que você execute o modelo CAU Analisador de Práticas Recomendadas \(BPA @ no__t-1 para garantir que o cluster e o ambiente de atualização estejam configurados corretamente para aplicar atualizações usando a CAU. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenha as atualizações do Publicador e copie-as ou extraia-as para um bloco de mensagens do servidor \(SMB @ no__t-1 compartilhamento de arquivos \(hotfix pasta raiz @ no__t-3 que dá suporte a pelo menos SMB 2,0 e que pode ser acessado por todos os nós de cluster e pela atualização remota O computador do coordenador \(If CAU é usado no modo Remote @ no__t-5updating @ no__t-6. Para mais informações, consulte [Configurar uma estrutura de pastas raiz de hotfix](#BKMK_HF_ROOT) mais adiante neste tópico. 

    > [!NOTE]
    > Por padrão, esse plug @ no__t-0in só instala hotfixes com as seguintes extensões de nome de arquivo:. msu,. msi e. msp.

- Copie o arquivo DefaultHotfixConfig. xml \(which é fornecido na pasta **% systemroot% \\System32 @ no__t-3WindowsPowerShell @ no__t-4V 1.0 @ no__t-5Modules @ no__t-6ClusterAwareUpdating** em um computador em que as ferramentas de Cau são instalado o @ no__t-7 na pasta raiz do hotfix que você criou e sob a qual você extraiu os hotfixes. Por exemplo, copie o arquivo de configuração para *\\ @ no__t-2MyFileServer @ no__t-3Hotfixes @ no__t-4Root @ no__t-5*. 

    > [!NOTE]
    > Para instalar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. Se o cenário exigir, você pode personalizar o arquivo de configuração como uma tarefa avançada. O arquivo de configuração pode incluir regras personalizadas, por exemplo, para processar arquivos de hotfix com extensões específicas ou definir comportamentos para condições de saída específicas. Para mais informações, consulte [Personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.

### <a name="configuration"></a>Configuração

Defina as configurações a seguir. Para mais informações, consulte os links para as seções mais adiante neste tópico.
- O caminho para a pasta raiz compartilhada de hotfix que contém as atualizações a serem aplicadas e o arquivo de configuração do hotfix. Você pode digitar esse caminho na interface do usuário da CAU ou configurar o argumento **HotfixRootFolderPath @ no__t-1 @ no__t-2Path >** PowerShell plug @ no__t-3in. 

   > [!NOTE]
   > Você pode especificar a pasta raiz do hotfix como um caminho de pasta local ou como um caminho UNC do formulário *\\ @ no__t-2ServerName @ no__t-3Share @ no__t-4RootFolderName*. Um domínio @ no__t-0based ou um caminho de namespace de DFS autônomo pode ser usado. No entanto, os recursos plug @ no__t-0in que verificam as permissões de acesso no arquivo de configuração do hotfix são incompatíveis com um caminho de namespace do DFS, portanto, se você configurar um, deverá desabilitar a verificação de permissões de acesso usando a interface do usuário da CAU ou configurando o  **DisableAclChecks @ no__t-2'True '** plug @ no__t-3in argumento.
- Configurações no servidor que hospeda a pasta raiz do hotfix para verificar as permissões apropriadas para acessar a pasta e garantir a integridade dos dados acessados da pasta compartilhada SMB \(SMB assinatura ou criptografia SMB @ no__t-1. Para obter mais informações, consulte [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL).

### <a name="additional-options"></a>Opções Adicionais

- Opcionalmente, configure o plug @ no__t-0in para que a criptografia SMB seja imposta ao acessar dados do compartilhamento de arquivos do hotfix. Na interface do usuário da CAU, na página **Opções adicionais** , selecione a opção **exigir criptografia SMB no acesso à pasta raiz do hotfix** ou configure o argumento **RequireSMBEncryption @ no__t-3'True '** PowerShell plug @ no__t-4in. 
  > [!IMPORTANT]
  > Você deve executar etapas adicionais de configuração no servidor SMB para habilitar a integridade dos dados SMB com assinatura ou criptografia SMB. Para mais informações, consulte a Etapa 4 em [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL). Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para acesso com o uso dessa criptografia SMB, ocorrerá falha na Execução de Atualização.
- Opcionalmente, desabilite as verificações padrão quanto a permissões suficientes para a pasta raiz de hotfix e o arquivo de configuração do hotfix. Na interface do usuário da CAU, selecione **Desabilitar verificação de acesso de administrador para a pasta raiz de hotfix e o arquivo de configuração**, ou configure o argumento plug @ no__t-3in do **DisableAclChecks @ no__t-2'True** .
- Opcionalmente, configure o argumento **HotfixInstallerTimeoutMinutes @ no__t-1 @ no__t-2** para especificar por quanto tempo o hotfix plug @ no__t-3in aguarda o processo do instalador do hotfix retornar. o padrão de @no__t 0The é de 30 minutos. \) Por exemplo, para especificar um período de tempo limite de duas horas, defina **HotfixInstallerTimeoutMinutes @ no__t-1120**.
- Opcionalmente, configure o argumento **HotfixConfigFileName \= <name>** plug @ no__t-3in para especificar um nome para o arquivo de configuração de hotfix localizado na pasta raiz do hotfix. Se não for especificado, o nome padrão DefaultHotfixConfig.xml será usado.
  
### <a name="BKMK_HF_ROOT"></a>Configurar uma estrutura de pasta raiz de hotfix

Para que o hotfix plug @ no__t-0in funcione, os hotfixes devem ser armazenados em uma estrutura do tipo @ no__t-1defined em um compartilhamento de arquivos SMB \(hotfix pasta raiz @ no__t-3, e você deve configurar o hotfix plug @ no__t-4in com o caminho para a pasta raiz do hotfix usando a interface do usuário da CAU ou os cmdlets do PowerShell da CAU. Esse caminho é passado para o plug @ no__t-0in como o argumento **HotfixRootFolderPath** . Você pode escolher uma das várias estruturas para a pasta raiz de hotfix, de acordo com as suas necessidades de atualização, como mostrado nos exemplos a seguir. Arquivos ou pastas que não aderem à estrutura são ignorados.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemplo 1-estrutura de pasta usada para aplicar hotfixes a todos os nós de cluster
  
Para especificar que os hotfixes se apliquem a todos os nós de cluster, copie-os para uma pasta chamada **CAUHotfix @ no__t-1All** na pasta raiz do hotfix. Neste exemplo, o argumento **HotfixRootFolderPath** plug @ no__t-1in é definido como *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. A pasta **CAUHotfix @ no__t-1All** contém três atualizações com as extensões. msu,. msi e. msp que serão aplicadas a todos os nós de cluster. Os nomes dos arquivos de atualização são apenas para fins de ilustração.  
  
> [!NOTE]  
> Neste e nos exemplos a seguir, o arquivo de configuração do hotfix com seu nome padrão DefaultHotfixConfig.xml é mostrado em seu local exigido na pasta raiz de hotfix.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemplo 2-estrutura de pasta usada para aplicar determinadas atualizações apenas a um nó específico
  
Para especificar hotfixes que se aplicam apenas a um nó específico, use uma subpasta da pasta raiz de hotfix com o nome do nó. Use o nome NetBIOS do nó de cluster, por exemplo, *ContosoNode1*. Em seguida, mova as atualizações que se aplicam apenas a esse nó para essa subpasta. No exemplo a seguir, o argumento **HotfixRootFolderPath** plug @ no__t-1in está definido como *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. As atualizações na pasta **CAUHotfix @ no__t-1All** serão aplicadas a todos os nós de cluster, e *Node1 @ no__t-3Specific\_Update.msu* será aplicado somente a *ContosoNode1*.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Exemplo 3-estrutura de pasta usada para aplicar atualizações diferentes de arquivos. msu,. msi e. msp
  
Por padrão, o **Microsoft.HotfixPlugin** só aplica atualizações com as extensões .msu, .msi ou .msp. No entanto, algumas atualizações podem ter extensões diferentes e exigem diferentes comandos de instalação. Por exemplo, você pode precisar aplicar uma atualização de firmware com a extensão .exe a um nó em um cluster. Você pode configurar a pasta raiz do hotfix com uma subpasta que indica um tipo de atualização específico, que não é @ no__t-0default deve ser instalado. Você também deve configurar uma regra de instalação de pasta correspondente que especifica o comando de instalação no elemento `<FolderRules>` no arquivo XML de configuração de hotfix.  
  
No exemplo a seguir, o argumento **HotfixRootFolderPath** plug @ no__t-1in está definido como *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. Várias atualizações serão aplicadas a todos os nós do cluster e uma atualização de firmware *SpecialHotfix1.exe* será aplicada ao *ContosoNode1* usando *FolderRule1*. Para obter informações sobre como configurar *FolderRule1* no arquivo de configuração de hotfix, consulte [Personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>Personalizar o arquivo de configuração de hotfix  
O arquivo de configuração de hotfix controla como o **Microsoft.HotfixPlugin** instala tipos de arquivo de hotfix específicos em um cluster de failover. O esquema XML do arquivo de configuração é definido no HotfixConfigSchema.xsd, que está localizado na seguinte pasta em um computador onde as ferramentas do CAU estão instaladas:  
  
**% SystemRoot% \\System32 @ no__t-2WindowsPowerShell @ no__t-3V 1.0 @ no__t-4Modules @ no__t-5ClusterAwareUpdating pasta**  
  
Para personalizar o arquivo de configuração de hotfix, copie o arquivo de configuração de amostra DefaultHotfixConfig.xml deste local para a pasta raiz do hotfix e faça as modificações apropriadas para o seu cenário.  
  
> [!IMPORTANT]  
> Para aplicar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. A personalização do arquivo de configuração de hotfix é uma tarefa apenas em cenários de uso avançados.  
  
Por padrão, o arquivo XML de configuração de hotfix define regras de instalação e condições de saída para as duas categorias de hotfixes a seguir:  
  
-   Arquivos de hotfix com extensões que o plug @ no__t-0in pode instalar por padrão \(. msu,. msi e arquivos. msp @ no__t-2.  
  
    Eles são definidos como elementos `<ExtensionRules>` no elemento `<DefaultRules>`. Há um elemento `<Extension>` para cada um dos tipos de arquivos padrão suportados. A estrutura XML geral é a seguinte:  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    Se você precisar aplicar determinados tipos de atualização a todos os nós do cluster em seu ambiente, poderá definir elementos `<Extension>` adicionais.  
  
-   Hotfix ou outros arquivos de atualização que não são arquivos. msi,. msu ou. msp, por exemplo, drivers que não são @ no__t-0Microsoft, firmware e atualizações do BIOS.  
  
    Cada tipo de arquivo não @ no__t-0default é configurado como um elemento `<Folder>` no elemento `<FolderRules>`. O atributo de nome do elemento `<Folder>` deve ser idêntico ao nome de uma pasta na pasta raiz do hotfix que conterá as atualizações do tipo correspondente. A pasta pode estar na pasta **CAUHotfix @ no__t-1All** ou em uma pasta node @ no__t-2specific. Por exemplo, se *FolderRule1* estiver configurado na pasta raiz de hotfix, configure o seguinte elemento no arquivo XML para definir um modelo de instalação e condições de saída para as atualizações dessa pasta:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
As tabelas a seguir descrevem os atributos `<Template>` e os possíveis subelementos `<ExitConditions>` .  
  
|Atributo `<Template>`|Descrição|  
|--------------------------|---------------|  
|`path`|O caminho completo para o programa de instalação para o tipo de arquivo definido no atributo `<Extension name>` .<br /><br />Para especificar o caminho para um arquivo de atualização na estrutura da pasta raiz de hotfix, use `$update$`.|  
|`parameters`|Uma cadeia de caracteres de parâmetros necessários e opcionais para o programa que está especificado em `path`.<br /><br />Para especificar um parâmetro que seja o caminho para um arquivo de atualização na estrutura da pasta raiz de hotfix, use `$update$`.|  
  
|Subelemento `<ExitConditions>`|Descrição|  
|---------------------------------|---------------|  
|`<Success>`|Define um ou mais códigos de saída que indicam que a atualização especificada foi bem-sucedida. Este é um subelemento obrigatório.|  
|`<Success_RebootRequired>`|Opcionalmente, defina um ou mais códigos de saída que indicam que a atualização especificada foi bem-sucedida e que o nó deve ser reiniciado. <br>**Observação:** Opcionalmente, o elemento `<Folder>` pode conter o atributo `alwaysReboot`. Se esse atributo for definido, ele indicará que, se um hotfix instalado por essa regra retornar um dos códigos de saída definidos como `<Success>`, ele será interpretado como uma condição de saída `<Success_RebootRequired>` .|  
|`<Fail_RebootRequired>`|Você também pode definir um ou mais códigos de saída que indicam que a atualização especificada falhou e o nó deve ser reiniciado.|  
|`<AlreadyInstalled>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque já está instalada.|  
|`<NotApplicable>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque não se aplica ao nó do cluster.|  
  
> [!IMPORTANT]  
> Qualquer código de saída que não seja explicitamente definido em `<ExitConditions>` é interpretado como falha na atualização e o nó não reinicia.  
  
### <a name="BKMK_ACL"></a>Restringir o acesso à pasta raiz do hotfix  
Você deve executar várias etapas para configurar o servidor de arquivos SMB e o compartilhamento de arquivos para ajudar a proteger os arquivos da pasta raiz de hotfix e o arquivo de configuração de hotfix para acesso somente no contexto do **Microsoft.HotfixPlugin**. Essas etapas habilitam vários recursos que ajudam a evitar possíveis violações dos arquivos de hotfix que podem comprometer o cluster de failover.  
  
As etapas gerais são as seguintes:  
  
1.  Identificar a conta de usuário usada para a atualização de execuções usando o plug @ no__t-0in  
  
2.  Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB  
  
3.  Configurar permissões para acessar a pasta raiz de hotfix  
  
4.  Definir configurações para a integridade dos dados SMB  
  
5.  Habilitar uma regra de Firewall do Windows no servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Etapa 1. Identificar a conta de usuário usada para a atualização de execuções usando o hotfix plug @ no__t-0in
  
A conta usada na CAU para verificar as configurações de segurança durante a execução de uma execução de atualização usando **o Microsoft. HotfixPlugin** depende se a cau é usada no modo Remote @ no__t-1updating ou no modo Self @ no__t-2updating, da seguinte maneira:  
  
-   **Modo @ no__t-1Updating remoto** A conta que tem privilégios administrativos no cluster para visualizar e aplicar atualizações.  
  
-   **Modo Self @ no__t-1updating** O nome do objeto de computador virtual que é configurado no Active Directory para a função clusterizada CAU. Esse é o nome de um objeto de computador virtual de pré-teste no Active Directory para a função clusterizada do CAU ou o nome gerado pelo CAU para a função clusterizada. Para obter o nome, se for gerado pela CAU, execute o cmdlet **Get @ no__t-1CauClusterRole** Cau PowerShell. Na saída, **ResourceGroupName** é o nome da conta do objeto de computador virtual gerado.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Etapa 2. Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB
  
> [!IMPORTANT]  
> Você deve adicionar a conta usada para as Execuções de Atualização como uma conta de administrador local no servidor SMB. Se isso não for permitido por causa das políticas de segurança de sua organização, configure essa conta com as permissões necessárias no servidor SMB usando o procedimento a seguir.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar uma conta de usuário no servidor SMB  
  
1.  Adicione a conta usada para as Execuções de Atualização ao grupo Distributed COM - Usuários e a um dos seguintes grupos: Usuário Avançado, Operação do Servidor ou Operador de Impressão.  
  
2.  Para habilitar as permissões WMI necessárias para a conta, inicie o Console de Gerenciamento de WMI no servidor SMB. Inicie o PowerShell e digite o seguinte comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Na árvore de console, clique com o botão direito @ no__t-0click **controle WMI \(Local @ no__t-3**e clique em **Propriedades**.  
  
4.  Clique em **Segurança** e expanda **Raiz**.  
  
5.  Clique em **CIMV2**e depois em **Segurança**.  
  
6.  Adicione a conta que é usada para as Execuções de Atualização à lista **Nomes de grupo ou de usuário**.  
  
7.  Conceda as permissões para **Executar Métodos** e **Habilitação Remota** para a conta usada para as Execuções de Atualização.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Etapa 3. Configurar permissões para acessar a pasta raiz de hotfix
  
Por padrão, quando você tenta aplicar atualizações, o hotfix plug @ no__t-0in verifica a configuração das permissões do sistema de arquivos NTFS para acessar a pasta raiz do hotfix. Se as permissões de acesso à pasta não estiverem configuradas corretamente, uma execução de atualização usando o hotfix plug @ no__t-0in poderá falhar.  
  
Se você usar a configuração padrão do hotfix plug @ no__t-0in, verifique se as permissões de acesso à pasta atendem aos seguintes requisitos.  
  
-   O grupo de Usuários tem permissão de Leitura.  
  
-   Se o plug @ no__t-0in aplicar as atualizações com a extensão. exe, o grupo usuários terá a permissão executar.  
  
-   Somente determinadas entidades de segurança são permitidas \(but não são necessárias para que o no__t-1 tenha permissão de gravação ou modificação. As entidades permitidas são o grupo local de Administradores, SISTEMA, PROPRIETÁRIO CRIADOR e TrustedInstaller. Outras contas ou grupos não têm permissão para Gravar ou Modificar na pasta raiz de hotfix.  
  
Opcionalmente, você pode desabilitar as verificações anteriores que o plug @ no__t-0in executa por padrão. É possível fazer isso de duas maneiras:  
  
-   Se você estiver usando os cmdlets do PowerShell da CAU, configure o argumento **DisableAclChecks @ no__t-1'True '** no parâmetro **CauPluginArguments** para o hotfix plug @ no__t-3in.  
  
-   Se você estiver usando a IU do CAU, selecione a opção **Desabilitar verificação para acesso de administrador à pasta raiz de hotfix e ao arquivo de configuração** na página **Opções Adicionais de Atualização** do assistente usado para configurar as opções de Execução de Atualização.  
  
No entanto, como melhor prática em muitos ambientes, é recomendável usar a configuração padrão para impor essas verificações.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Etapa 4. Definir configurações para a integridade dos dados SMB
  
Para verificar a integridade dos dados nas conexões entre os nós de cluster e o compartilhamento de arquivos SMB, o hotfix plug @ no__t-0in requer que você habilite as configurações no compartilhamento de arquivos SMB para assinatura SMB ou criptografia SMB. A criptografia SMB, que fornece segurança aprimorada e melhor desempenho em vários ambientes, tem suporte a partir do Windows Server 2012. É possível habilitar uma ou ambas as configurações, da seguinte forma:  
  
-   Para habilitar assinatura SMB, consulte o procedimento no [artigo 887429](https://support.microsoft.com/kb/887429) na Base de Dados de Conhecimento Microsoft.  
  
-   Para habilitar a criptografia SMB para a pasta compartilhada SMB, execute o seguinte cmdlet do PowerShell no servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Em que <*ShareName*> é o nome da pasta compartilhada de SMB.  
  
Opcionalmente, para impor o uso da criptografia SMB nas conexões com o servidor SMB, selecione a opção **exigir criptografia SMB no acesso à pasta raiz do hotfix** na interface do usuário da cau ou configure o **RequireSMBEncryption @ no__t-2'True '** plug @ no__ argumento t-3in usando os cmdlets do PowerShell da CAU.  
  
> [!IMPORTANT]  
> Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para conexões que usam criptografia SMB, a Execução de Atualização falhará.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Etapa 5. Habilitar uma regra de Firewall do Windows no servidor SMB
  
Você deve habilitar a regra de **gerenciamento remoto do servidor de arquivos \(SMB @ no__t-2in @ no__t-3** no firewall do Windows no servidor de arquivos SMB. Isso é habilitado por padrão no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da atualização com suporte a cluster](cluster-aware-updating.md)
  
-   [Cmdlets de atualização do Windows PowerShell com suporte a cluster](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Referência de plug-in de atualização com suporte a cluster](https://msdn.microsoft.com/library/hh418084.aspx)  
  
