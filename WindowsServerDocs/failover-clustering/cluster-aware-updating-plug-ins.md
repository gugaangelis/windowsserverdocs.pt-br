---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Como funcionam os plug-ins de atualização com suporte a cluster
description: Como usar plug-ins para coordenar atualizações ao usar a atualização com suporte a cluster no Windows Server para instalar atualizações em um cluster.
ms.topic: article
ms.prod: windows-server
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
ms.openlocfilehash: 21585ab376830f37ca6432849dd8e9b3773af9ab
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473293"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Como funcionam os plug-ins de atualização com suporte a cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A cau ( [atualização com suporte a cluster](cluster-aware-updating.md) ) usa plug-ins para coordenar a instalação de atualizações entre nós em um cluster de failover. Este tópico fornece informações sobre como usar os \- plug-ins de Cau internos \- ou outros plug \- -ins que você instala para a cau.

## <a name="install-a-plug-in"></a><a name="BKMK_INSTALL"></a>Instalar um plug- \- in
Um plug- \- in diferente dos plug- \- ins padrão instalados com a cau \( **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) deve ser instalado separadamente. Se a CAU for usada no \- modo de autoatualização, o plug- \- in deverá ser instalado em todos os nós de cluster. Se a CAU for usada no \- modo de atualização remota, o plug- \- in deverá ser instalado no computador do coordenador de atualização remota. Um plug \- -in que você instala pode ter requisitos de instalação adicionais em cada nó.

Para instalar um plug- \- in, siga as instruções do editor do plug- \- in. Para registrar manualmente um plug \- -in com a cau, execute o cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) em cada computador em que o plug- \- in está instalado.

## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar um plug- \- in e os \- argumentos de plug-in

### <a name="specify-a-cau-plug-in"></a>Especificar um plug- \- in Cau

Na interface do usuário da CAU, você seleciona um plug- \- in em uma \- lista suspensa de plug- \- ins disponíveis quando usa a cau para executar as seguintes ações:

-   Aplicar atualizações ao cluster

-   Visualizar atualizações para o cluster

-   Configurar opções de \- autoatualização de cluster

Por padrão, a CAU seleciona o plug- \- in **Microsoft. WindowsUpdatePlugin**. No entanto, você pode especificar qualquer plug- \- in instalado e registrado com a cau.

> [!TIP]
> Na interface do usuário da CAU, você só pode especificar um único plug- \- in para a cau usar para visualizar ou aplicar atualizações durante uma execução de atualização. Usando os cmdlets do PowerShell da CAU, você pode especificar um ou mais plug- \- ins. Se você precisar instalar vários tipos de atualizações no cluster, normalmente será mais eficiente especificar vários plug- \- ins em uma execução de atualização, em vez de usar uma execução de atualização separada para cada plug- \- in. Por exemplo, normalmente ocorrerá um número menor de reinicializações de nós.

Usando os cmdlets do PowerShell da CAU listados na tabela a seguir, você pode especificar um ou mais plug- \- ins para uma execução ou verificação de atualização passando o parâmetro **– CauPluginName** . Você pode especificar vários \- nomes de plug-in separando-os com vírgulas. Se você especificar vários plug- \- ins, também poderá controlar como os plug \- -ins influenciam uns aos outros durante uma execução de atualização especificando os parâmetros ** \- RunPluginsSerially**, ** \- StopOnPluginFailure**e **– SeparateReboots** . Para obter mais informações sobre como usar vários plug- \- ins, use os links fornecidos para a documentação do cmdlet na tabela a seguir.

|Cmdlet|Descrição|
|----------|---------------|
|[Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/add-cauclusterrole)|Adiciona a função clusterizada CAU que fornece a \- funcionalidade de autoatualização para o cluster especificado.|
|[Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/invoke-caurun)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e instala essas atualizações através de uma Execução de Atualização no cluster especificado.|
|[Invoke-CauScan](https://docs.microsoft.com/powershell/module/clusterawareupdating/invoke-causcan)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e retorna uma lista do conjunto inicial de atualizações que seriam aplicadas a cada nó no cluster especificado.|
|[Set-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/set-cauclusterrole)|Define as propriedades de configuração para a função clusterizada do CAU no cluster especificado.|

Se você não especificar um \- parâmetro de plug-in Cau usando esses cmdlets, o padrão será o plug- \- in **Microsoft. WindowsUpdatePlugin**.

### <a name="specify-cau-plug-in-arguments"></a>Especificar \- argumentos de plug-in Cau
Ao configurar as opções de execução de atualização, você pode especificar um ou *mais \= * \( argumentos de pares \) de valor de nome para o plug- \- in selecionado a ser usado. Por exemplo, na IU do CAU, é possível especificar vários argumentos, da seguinte forma:

**Nome1 \= value1; Nome2 \= value2; Name3 \= Value3**

Esses pares de * \= valor de nome* devem ser significativos para o plug- \- in que você especificar. Para alguns plug- \- ins, os argumentos são opcionais.

A sintaxe dos argumentos do plug- \- in Cau segue estas regras gerais:

-   Vários pares de * \= valores de nome* são separados por ponto e vírgula.

-   Um valor que contém espaços está entre aspas, por exemplo: **Nome1 \= "valor com espaços"**.

-   A sintaxe exata do *valor* depende do plug- \- in.

Para especificar argumentos de plug- \- in usando os cmdlets do PowerShell da cau que dão suporte ao parâmetro **– CauPluginParameters** , passe um parâmetro do formulário:

**\-CauPluginArguments @ {Nome1 \= value1; Nome2 \= value2; Name3 \= Value3}**

Você também pode usar uma tabela de hash predefinida do PowerShell. Para especificar argumentos de plug- \- in para mais de um plug- \- in, passe várias tabelas de hash de argumentos, separados por vírgulas. Passe os argumentos do plug-in \- na ordem do plug- \- in especificada em **CauPluginName**.

### <a name="specify-optional-plug-in-arguments"></a>Especificar argumentos plug-in opcionais \-
Os plug- \- ins que o Cau instala \( **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) fornecem opções adicionais que você pode selecionar. Na interface do usuário da CAU, eles aparecem em uma página de **Opções adicionais** depois de configurar as opções de execução de atualização para o plug- \- in. Se você estiver usando os cmdlets do PowerShell da CAU, essas opções serão configuradas como argumentos plug-in opcionais \- . Para mais informações, consulte [Usar o Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usar o Microsoft.HotfixPlugin](#BKMK_HFP), mais adiante neste tópico.

## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gerenciar plug- \- ins usando cmdlets do Windows PowerShell

|Cmdlet|Descrição|
|----------|---------------|
|[Get-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/get-cauplugin)|Recupera informações sobre um ou mais plug-ins de atualização de software \- registrados no computador local.|
|[Register-CauPlugin]((https://docs.microsoft.com/powershell/module/clusterawareupdating/register-cauplugin))|Registra um plug-in de atualização de software CAU \- no computador local.|
|[Unregister-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/unregister-cauplugin)|Remove um plug-in de atualização de software \- da lista de plug- \- ins que podem ser usados pela Cau. **Observação:** Os plug- \- ins instalados com a cau \( **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) não podem ter o registro cancelado.|

## <a name="using-the-microsoftwindowsupdateplugin"></a><a name="BKMK_WUP"></a>Usando o Microsoft. WindowsUpdatePlugin

O plug- \- in padrão para cau, **Microsoft. WindowsUpdatePlugin**, executa as seguintes ações:
- Comunica-se com o Windows Update Agent em cada nó do cluster de failover para aplicar atualizações necessárias aos produtos da Microsoft que estão sendo executados em cada nó.
- Instala atualizações de cluster diretamente do Windows Update ou Microsoft Update, ou de um \- servidor Windows Server Update Services \( WSUS local \) .
- Instala apenas as atualizações de versão de distribuição geral selecionadas, \( GDR \) . Por padrão, o plug \- -in aplica-se apenas a atualizações de software importantes. Nenhuma configuração é necessária. A configuração padrão baixa e instala atualizações importantes de GDR em cada nó.

> [!NOTE]
> Para aplicar atualizações diferentes das atualizações de software importantes que são selecionadas por padrão \( , por exemplo, atualizações de driver \) , você pode configurar um \- parâmetro de plug-in opcional. Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador coordenador de atualização remota \( se usados \) devem atender aos requisitos para a Cau e à configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas para a cau](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para aplicação de atualizações da Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) e faça as alterações necessárias à configuração do Microsoft Update para os nós do cluster de failover.
- Para obter melhores resultados, recomendamos que você execute a CAU Analisador de Práticas Recomendadas \( BPA \) para garantir que o cluster e o ambiente de atualização estejam configurados corretamente para aplicar atualizações usando a cau. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> As atualizações que exigem aceitação de termos de licença da Microsoft ou interação do usuário são excluídas, e devem ser instaladas manualmente.

### <a name="additional-options"></a>Opções adicionais

Opcionalmente, você pode especificar os seguintes argumentos de plug- \- in para aumentar ou restringir o conjunto de atualizações que são aplicadas pelo plug- \- in:
- Para configurar o plug- \- in para aplicar as atualizações recomendadas, além de atualizações importantes em cada nó, na interface do usuário da cau, na página **Opções adicionais** , marque a caixa de seleção **fornecer atualizações recomendadas da mesma maneira que recebo atualizações importantes** .
<br>Como alternativa, configure o argumento **' \= true '** do plug-in ' IncludeRecommendedUpdates ' \- .
- Para configurar o plug- \- in para filtrar os tipos de atualizações GDR que são aplicadas a cada nó de cluster, especifique uma cadeia de consulta de agente de Windows Update usando um argumento de plug-in de **QueryString** \- . Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="configure-the-windows-update-agent-query-string"></a><a name="BKMK_QUERY"></a>Configurar a cadeia de caracteres de consulta do Windows Update Agent
Você pode configurar um argumento de plug- \- in para o plug- \- in padrão, **Microsoft. WindowsUpdatePlugin**, que consiste em uma cadeia de caracteres de consulta do Windows Update Agent \( \) . Essa instrução usa a API do WUA para identificar um ou mais grupos de atualizações da Microsoft para aplicar a cada nó, com base em critérios de seleção específicos. Você pode combinar vários critérios, usando um AND ou um OR lógico. A cadeia de caracteres de consulta do WUA é especificada em um argumento de plug- \- in da seguinte maneira:

**QueryString \= "critérion1 \= value1 e \/ or Criterion2 \= value2 e \/ or..."**

Por exemplo, o **Microsoft.WindowsUpdatePlugin** seleciona automaticamente as atualizações importantes, usando um argumento padrão **QueryString** que é construído usando os critérios **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:

**QueryString \= "IsInstalled \= 0 e Type \= ' software ' e IsHidden \= 0 e isassigned \= 1"**

Se você especificar um argumento **QueryString** , ele será usado no lugar da **QueryString** padrão configurada para o plug- \- in.

#### <a name="example-1"></a>Exemplo 1

Para configurar um argumento **QueryString** que instala uma atualização específica, conforme identificado pela ID *f6ce46c1 \- 971c \- 43f9 \- a2aa \- 783df125f003*:

**QueryString \= "UpdateId" \= f6ce46c1 \- 971c \- 43f9 \- a2aa \- 783df125f003 e IsInstalled \= 0 "**

> [!NOTE]
> O exemplo anterior é válido para aplicar atualizações usando o \- Assistente para atualização com suporte de cluster. Se você quiser instalar uma atualização específica Configurando \- Opções de atualização automática com a interface do usuário da cau ou usando o cmdlet **Add \- CauClusterRole** ou **set \- CauClusterRole**do PowerShell, deverá formatar o valor UpdateId com dois \- caracteres de aspas simples:
>
> **QueryString \= "UpdateId \= ' ' f6ce46c1 \- 971c \- 43f9 \- a2aa \- 783df125f003 ' ' e IsInstalled \= 0"**

#### <a name="example-2"></a>Exemplo 2

Para configurar um argumento **QueryString** que instala somente drivers:

**QueryString \= "IsInstalled \= 0 e Type \= ' Driver ' e IsHidden \= 0"**

Para obter mais informações sobre cadeias de caracteres de consulta para o plug \- -in padrão, **Microsoft. WindowsUpdatePlugin**, os critérios de pesquisa, \( como **IsInstalled** \) , e a sintaxe que você pode incluir nas cadeias de caracteres de consulta, consulte a seção sobre critérios de pesquisa na [referência de API do WUA (agente de Windows Update)](https://go.microsoft.com/fwlink/p/?LinkId=223304).

## <a name="use-the-microsofthotfixplugin"></a><a name="BKMK_HFP"></a>Usar o Microsoft.HotfixPlugin
O plug- \- in **Microsoft. HotfixPlugin** pode ser usado para aplicar as atualizações de atualização de distribuição limitada da Microsoft \( \) \( , chamadas de hotfixes, e anteriormente chamado de QFEs \) que você baixa de forma independente para resolver problemas de software específicos da Microsoft. O plug-in instala atualizações de uma pasta raiz em um compartilhamento de arquivos SMB e também pode ser personalizado para aplicar \- atualizações de BIOS, firmware e driver não Microsoft.

> [!NOTE]
> Os hotfixes às vezes estão disponíveis para download da Microsoft nos artigos da base de dados de conhecimento, mas também são fornecidos aos clientes de acordo com a \- necessidade.

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador coordenador de atualização remota \( se usados \) devem atender aos requisitos para a Cau e à configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas para a cau](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para uso do Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obter melhores resultados, recomendamos que você execute o modelo de BPA Analisador de Práticas Recomendadas do CAU \( \) para garantir que o cluster e o ambiente de atualização estejam configurados corretamente para aplicar atualizações usando a cau. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenha as atualizações do Publicador e copie-as ou extraia-as para uma pasta raiz de hotfix de compartilhamento de mensagens do servidor \( SMB \) \( \) que dá suporte a pelo menos SMB 2,0 e que pode ser acessada por todos os nós de cluster e pelo computador coordenador de atualização remota \( se a cau for usada no \- modo de atualização remota \) . Para mais informações, consulte [Configurar uma estrutura de pastas raiz de hotfix](#BKMK_HF_ROOT) mais adiante neste tópico.

    > [!NOTE]
    > Por padrão, esse plug- \- in instala apenas hotfixes com as seguintes extensões de nome de arquivo:. msu,. msi e. msp.

- Copie o arquivo de DefaultHotfixConfig.xml \( fornecido na pasta **% systemroot% \\ System32 \\ WindowsPowerShell \\ v 1.0 \\ modules \\ ClusterAwareUpdating** em um computador em que as ferramentas Cau estão instaladas na \) pasta raiz de hotfix que você criou e sob a qual você extraiu os hotfixes. Por exemplo, copie o arquivo de configuração para a * \\ \\ \\ \\ raiz \\ de hotfixes do MyFileServer*.

    > [!NOTE]
    > Para instalar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. Se o cenário exigir, você pode personalizar o arquivo de configuração como uma tarefa avançada. O arquivo de configuração pode incluir regras personalizadas, por exemplo, para processar arquivos de hotfix com extensões específicas ou definir comportamentos para condições de saída específicas. Para mais informações, consulte [Personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.

### <a name="configuration"></a>Configuração

Defina as configurações a seguir. Para mais informações, consulte os links para as seções mais adiante neste tópico.
- O caminho para a pasta raiz compartilhada de hotfix que contém as atualizações a serem aplicadas e o arquivo de configuração do hotfix. Você pode digitar esse caminho na interface do usuário da cau ou configurar o argumento do plug-in do PowerShell do **HotfixRootFolderPath \= \<Path> ** \- .

   > [!NOTE]
   > Você pode especificar a pasta raiz do hotfix como um caminho de pasta local ou como um caminho UNC do formulário * \\ \\ ServerName \\ share \\ RootFolderName*. Um \- caminho de namespace DFS autônomo ou baseado em domínio pode ser usado. No entanto, os recursos de plug- \- in que verificam as permissões de acesso no arquivo de configuração do hotfix são incompatíveis com um caminho de namespace do DFS, portanto, se você configurar um, deverá desabilitar a verificação de permissões de acesso usando a interface do usuário da cau ou configurando o **DisableAclChecks \= ' true '** plug \- in.
- Configurações no servidor que hospeda a pasta raiz do hotfix para verificar as permissões apropriadas para acessar a pasta e garantir a integridade dos dados acessados da assinatura SMB da pasta compartilhada SMB \( ou da criptografia SMB \) . Para mais informações, consulte [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL).

### <a name="additional-options"></a>Opções adicionais

- Opcionalmente, configure o plug- \- in para que a criptografia SMB seja imposta ao acessar dados do compartilhamento de arquivos do hotfix. Na interface do usuário da CAU, na página **Opções adicionais** , selecione a opção **exigir criptografia SMB no acesso à pasta raiz do hotfix** ou configure o argumento do plug-in do PowerShell ** \= ' true ' do RequireSMBEncryption** \- .
  > [!IMPORTANT]
  > Você deve executar etapas adicionais de configuração no servidor SMB para habilitar a integridade dos dados SMB com assinatura ou criptografia SMB. Para mais informações, consulte a Etapa 4 em [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL). Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para acesso com o uso dessa criptografia SMB, ocorrerá falha na Execução de Atualização.
- Opcionalmente, desabilite as verificações padrão quanto a permissões suficientes para a pasta raiz de hotfix e o arquivo de configuração do hotfix. Na interface do usuário da CAU, selecione **Desabilitar verificação de acesso de administrador para a pasta raiz de hotfix e o arquivo de configuração**, ou configure o argumento de plug-in **DisableAclChecks \= ' true '** \- .
- Opcionalmente, configure o **argumento \= <Integer> HotfixInstallerTimeoutMinutes** para especificar por quanto tempo o plug-in do hotfix \- aguarda o processo do instalador do hotfix retornar. \(O padrão é 30 minutos. \) Por exemplo, para especificar um período de tempo limite de duas horas, defina **HotfixInstallerTimeoutMinutes \= 120**.
- Opcionalmente, configure o **argumento \= <name> HotfixConfigFileName** plug \- in para especificar um nome para o arquivo de configuração de hotfix localizado na pasta raiz do hotfix. Se não for especificado, o nome padrão DefaultHotfixConfig.xml será usado.

### <a name="configure-a-hotfix-root-folder-structure"></a><a name="BKMK_HF_ROOT"></a>Configurar uma estrutura de pastas raiz de hotfix

Para que o plug-in do hotfix \- funcione, os hotfixes devem ser armazenados em uma \- estrutura bem definida em uma pasta raiz de hotfix de compartilhamento de arquivos SMB \( \) , e você deve configurar o plug-in do hotfix \- com o caminho para a pasta raiz do hotfix usando a interface do usuário da cau ou os cmdlets do PowerShell da cau. Esse caminho é passado para o plug- \- in como o argumento **HotfixRootFolderPath** . Você pode escolher uma das várias estruturas para a pasta raiz de hotfix, de acordo com as suas necessidades de atualização, como mostrado nos exemplos a seguir. Arquivos ou pastas que não aderem à estrutura são ignorados.

#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemplo 1-estrutura de pasta usada para aplicar hotfixes a todos os nós de cluster

Para especificar que os hotfixes se apliquem a todos os nós de cluster, copie-os para uma pasta chamada **CAUHotfix \_ All** na pasta raiz do hotfix. Neste exemplo, o argumento **HotfixRootFolderPath** plug \- in é definido como * \\ \\ \\ \\ raiz \\ de hotfixes MyFileServer*. A pasta **CAUHotfix \_ All** contém três atualizações com as extensões. msu,. msi e. msp que serão aplicadas a todos os nós de cluster. Os nomes dos arquivos de atualização são apenas para fins de ilustração.

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

Para especificar hotfixes que se aplicam apenas a um nó específico, use uma subpasta da pasta raiz de hotfix com o nome do nó. Use o nome NetBIOS do nó de cluster, por exemplo, *ContosoNode1*. Em seguida, mova as atualizações que se aplicam apenas a esse nó para essa subpasta. No exemplo a seguir, o argumento **HotfixRootFolderPath** plug \- in é definido como * \\ \\ \\ \\ raiz \\ de hotfixes MyFileServer*. As atualizações na pasta **CAUHotfix \_ All** serão aplicadas a todos os nós de cluster, e *Node1 \_ specific \_ Update. msu* será aplicada somente ao *ContosoNode1*.

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

Por padrão, o **Microsoft.HotfixPlugin** só aplica atualizações com as extensões .msu, .msi ou .msp. No entanto, algumas atualizações podem ter extensões diferentes e exigem diferentes comandos de instalação. Por exemplo, você pode precisar aplicar uma atualização de firmware com a extensão .exe a um nó em um cluster. Você pode configurar a pasta raiz do hotfix com uma subpasta que indica que um tipo de atualização específico e não \- padrão deve ser instalado. Você também deve configurar uma regra de instalação de pasta correspondente que especifica o comando de instalação no elemento `<FolderRules>` no arquivo XML de configuração de hotfix.

No exemplo a seguir, o argumento **HotfixRootFolderPath** plug \- in é definido como * \\ \\ \\ \\ raiz \\ de hotfixes MyFileServer*. Várias atualizações serão aplicadas a todos os nós do cluster e uma atualização de firmware *SpecialHotfix1.exe* será aplicada ao *ContosoNode1* usando *FolderRule1*. Para informações sobre como configurar *FolderRule1* no arquivo de configuração de hotfix, consulte [Personalizar o arquivo de configuração do hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.

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

### <a name="customize-the-hotfix-configuration-file"></a><a name="BKMK_CONFIG_FILE"></a>Personalizar o arquivo de configuração de hotfix
O arquivo de configuração de hotfix controla como o **Microsoft.HotfixPlugin** instala tipos de arquivo de hotfix específicos em um cluster de failover. O esquema XML do arquivo de configuração é definido no HotfixConfigSchema.xsd, que está localizado na seguinte pasta em um computador onde as ferramentas do CAU estão instaladas:

**% SystemRoot% \\ System32 \\ WindowsPowerShell \\ v 1.0 \\ modules \\ ClusterAwareUpdating Folder**

Para personalizar o arquivo de configuração de hotfix, copie o arquivo de configuração de amostra DefaultHotfixConfig.xml deste local para a pasta raiz do hotfix e faça as modificações apropriadas para o seu cenário.

> [!IMPORTANT]
> Para aplicar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. A personalização do arquivo de configuração de hotfix é uma tarefa apenas em cenários de uso avançados.

Por padrão, o arquivo XML de configuração de hotfix define regras de instalação e condições de saída para as duas categorias de hotfixes a seguir:

-   Arquivos de hotfix com extensões que o plug \- -in pode instalar por padrão \( . msu,. msi e arquivos. msp \) .

    Eles são definidos como elementos `<ExtensionRules>` no elemento `<DefaultRules>` . Há um elemento `<Extension>` para cada um dos tipos de arquivos padrão suportados. A estrutura XML geral é a seguinte:

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

-   Hotfix ou outros arquivos de atualização que não são arquivos. msi,. msu ou. msp, por exemplo, \- drivers não Microsoft, firmware e atualizações do BIOS.

    Cada \- tipo de arquivo não padrão é configurado como um `<Folder>` elemento no `<FolderRules>` elemento. O atributo de nome do elemento `<Folder>` deve ser idêntico ao nome de uma pasta na pasta raiz do hotfix que conterá as atualizações do tipo correspondente. A pasta pode estar na pasta **CAUHotfix \_ All** ou em um nó \- específico. Por exemplo, se *FolderRule1* estiver configurado na pasta raiz de hotfix, configure o seguinte elemento no arquivo XML para definir um modelo de instalação e condições de saída para as atualizações dessa pasta:

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
|`path`|O caminho completo para o programa de instalação para o tipo de arquivo definido no atributo `<Extension name>` .<p>Para especificar o caminho para um arquivo de atualização na estrutura da pasta raiz de hotfix, use `$update$`.|
|`parameters`|Uma cadeia de caracteres de parâmetros necessários e opcionais para o programa que está especificado em `path`.<p>Para especificar um parâmetro que seja o caminho para um arquivo de atualização na estrutura da pasta raiz de hotfix, use `$update$`.|

|Subelemento `<ExitConditions>`|Descrição|
|---------------------------------|---------------|
|`<Success>`|Define um ou mais códigos de saída que indicam que a atualização especificada foi bem-sucedida. Este é um subelemento obrigatório.|
|`<Success_RebootRequired>`|Opcionalmente, defina um ou mais códigos de saída que indicam que a atualização especificada foi bem-sucedida e que o nó deve ser reiniciado. <br>**Observação:** Opcionalmente, o `<Folder>` elemento pode conter o `alwaysReboot` atributo. Se esse atributo for definido, ele indicará que, se um hotfix instalado por essa regra retornar um dos códigos de saída definidos como `<Success>`, ele será interpretado como uma condição de saída `<Success_RebootRequired>` .|
|`<Fail_RebootRequired>`|Você também pode definir um ou mais códigos de saída que indicam que a atualização especificada falhou e o nó deve ser reiniciado.|
|`<AlreadyInstalled>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque já está instalada.|
|`<NotApplicable>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque não se aplica ao nó do cluster.|

> [!IMPORTANT]
> Qualquer código de saída que não seja explicitamente definido em `<ExitConditions>` é interpretado como falha na atualização e o nó não reinicia.

### <a name="restrict-access-to-the-hotfix-root-folder"></a><a name="BKMK_ACL"></a>Restringir o acesso à pasta raiz de hotfix
Você deve executar várias etapas para configurar o servidor de arquivos SMB e compartilhamento de arquivos para ajudar a proteger os arquivos da pasta raiz de hotfix e o arquivo de configuração de hotfix para acesso somente no contexto do **Microsoft.HotfixPlugin**. Essas etapas habilitam vários recursos que ajudam a evitar possíveis violações dos arquivos de hotfix que podem comprometer o cluster de failover.

As etapas gerais são as seguintes:

1.  Identificar a conta de usuário usada para a atualização de execuções usando o plug- \- in

2.  Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB

3.  Configurar permissões para acessar a pasta raiz de hotfix

4.  Definir configurações para a integridade dos dados SMB

5.  Habilitar uma regra de Firewall do Windows no servidor SMB

#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Etapa 1. Identificar a conta de usuário usada para a atualização de execuções usando o plug-in de hotfix \-

A conta usada na CAU para verificar as configurações de segurança durante a execução de uma execução de atualização usando **o Microsoft. HotfixPlugin** depende se a cau é usada no \- modo de atualização remota ou no \- modo de autoatualização, da seguinte maneira:

-   ** \- Modo de atualização remota** a conta que tem privilégios administrativos no cluster para visualizar e aplicar atualizações.

-   ** \- Modo de autoatualização** o nome do objeto de computador virtual configurado no Active Directory para a função clusterizada Cau. Esse é o nome de um objeto de computador virtual de pré-teste no Active Directory para a função clusterizada do CAU ou o nome gerado pelo CAU para a função clusterizada. Para obter o nome, se for gerado pela CAU, execute o cmdlet **Get \- CauClusterRole** Cau PowerShell. Na saída, **ResourceGroupName** é o nome da conta do objeto de computador virtual gerado.

#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Etapa 2. Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB

> [!IMPORTANT]
> Você deve adicionar a conta usada para as Execuções de Atualização como uma conta de administrador local no servidor SMB. Se isso não for permitido por causa das políticas de segurança de sua organização, configure essa conta com as permissões necessárias no servidor SMB usando o procedimento a seguir.

##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar uma conta de usuário no servidor SMB

1.  Adicione a conta usada para as Execuções da Atualização ao grupo de Usuários do Distributed COM e a um dos seguintes grupos: Usuário Avançado, Operação do Servidor ou Operador de Impressão.

2.  Para habilitar as permissões WMI necessárias para a conta, inicie o Console de Gerenciamento de WMI no servidor SMB. Inicie o PowerShell e digite o seguinte comando:

    ```
    wmimgmt.msc
    ```

3.  Na árvore de console, clique com o botão direito do mouse \- em **controle de WMI \( \) local**e clique em **Propriedades**.

4.  Clique em **Segurança** e expanda **Raiz**.

5.  Clique em **CIMV2** e depois em **Segurança**.

6.  Adicione a conta que é usada para as Execuções de Atualização à lista **Nomes de grupo ou de usuário**.

7.  Conceda as permissões para **Executar Métodos** e **Habilitação Remota** para a conta usada para as Execuções de Atualização.

#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Etapa 3. Configurar permissões para acessar a pasta raiz de hotfix

Por padrão, quando você tenta aplicar atualizações, o plug-in do hotfix \- verifica a configuração das permissões do sistema de arquivos NTFS para acessar a pasta raiz do hotfix. Se as permissões de acesso à pasta não estiverem configuradas corretamente, uma execução de atualização usando o plug-in de hotfix \- poderá falhar.

Se você usar a configuração padrão do plug \- -in de hotfix, verifique se as permissões de acesso à pasta atendem aos seguintes requisitos.

-   O grupo de Usuários tem permissão de Leitura.

-   Se o plug- \- in aplicar atualizações com a extensão. exe, o grupo usuários terá a permissão executar.

-   Somente determinadas entidades de segurança são permitidas \( , mas não precisam \) ter permissão para gravar ou modificar. As entidades permitidas são o grupo local de Administradores, SISTEMA, PROPRIETÁRIO CRIADOR e TrustedInstaller. Outras contas ou grupos não têm permissão para Gravar ou Modificar na pasta raiz de hotfix.

Opcionalmente, você pode desabilitar as verificações anteriores que o plug- \- in executa por padrão. É possível fazer isso de duas formas:

-   Se você estiver usando os cmdlets do PowerShell da CAU, configure o argumento ** \= ' true ' do DisableAclChecks** no parâmetro **CauPluginArguments** para o plug-in do hotfix \- .

-   Se você estiver usando a IU do CAU, selecione a opção **Desabilitar verificação para acesso de administrador à pasta raiz de hotfix e ao arquivo de configuração** na página **Opções Adicionais de Atualização** do assistente usado para configurar as opções de Execução de Atualização.

No entanto, como melhor prática em muitos ambientes, é recomendável usar a configuração padrão para impor essas verificações.

#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Etapa 4. Definir configurações para a integridade dos dados SMB

Para verificar a integridade dos dados nas conexões entre os nós de cluster e o compartilhamento de arquivos SMB, o plug-in de hotfix \- requer que você habilite as configurações no compartilhamento de arquivos SMB para assinatura SMB ou criptografia SMB. A criptografia SMB, que fornece segurança aprimorada e melhor desempenho em vários ambientes, tem suporte a partir do Windows Server 2012. É possível habilitar uma ou ambas as configurações, da seguinte forma:

-   Para habilitar assinatura SMB, consulte o procedimento no [artigo 887429](https://support.microsoft.com/kb/887429) na Base de Dados de Conhecimento Microsoft.

-   Para habilitar a criptografia SMB para a pasta compartilhada SMB, execute o seguinte cmdlet do PowerShell no servidor SMB:

    ```PowerShell
    Set-SmbShare <ShareName> -EncryptData $true
    ```

    Em que <*ShareName*> é o nome da pasta compartilhada de SMB.

Opcionalmente, para impor o uso da criptografia SMB nas conexões com o servidor SMB, selecione a opção **exigir criptografia SMB no acesso à pasta raiz do hotfix** na interface do usuário da cau ou configure o argumento do plug-in do **RequireSMBEncryption \= ' true '** \- usando os cmdlets do PowerShell da cau.

> [!IMPORTANT]
> Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para conexões que usam criptografia SMB, a Execução de Atualização falhará.

#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Etapa 5. Habilitar uma regra de Firewall do Windows no servidor SMB

Você deve habilitar o **SMB de gerenciamento remoto do servidor de arquivos \( \- em \) ** regra no firewall do Windows no servidor de arquivos SMB. Isso é habilitado por padrão no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012.

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral da atualização com suporte a cluster](cluster-aware-updating.md)

-   [Cluster-Aware Updating Windows PowerShell Cmdlets (Cmdlets do Windows PowerShell para Atualização com Suporte a Cluster)](https://docs.microsoft.com/powershell/module/clusterawareupdating)

-   [Referência do plug-in de atualização com suporte a cluster](https://msdn.microsoft.com/library/hh418084.aspx)

