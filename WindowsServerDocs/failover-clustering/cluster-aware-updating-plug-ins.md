---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Como funcionam os plug-ins atualizando suporte a Cluster
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: "Como usar plug-ins para coordenar as atualizações ao usar o suporte a Cluster atualizar no Windows Server para instalar atualizações em um cluster."
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Como funcionam os plug-ins atualizando suporte a Cluster

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Suporte a cluster Atualizando](cluster-aware-updating.md) (CAU) usa plug-ins para a instalação das atualizações de coordenadas entre nós em um cluster de failover. Este tópico fornece informações sobre como usar os built\-in CAU plug\-ins ou outros plug\-ins que você instala para CAU.

## <a name="BKMK_INSTALL"></a>Instale um plug\-in  
Um plug\-in que não seja os padrão plug\-ins que são instalados com CAU \ (**Microsoft.WindowsUpdatePlugin** e **Microsoft.HotfixPlugin**\) deve ser instalado separadamente. Se CAU é usado no modo de atualização self\, plug\-in deve ser instalado em todos os nós de cluster. Se CAU é usado no modo de atualização remote\, plug\-in deve ser instalado no computador remoto coordenador de atualização. Um plug\-in que você instale pode ter requisitos de instalação adicionais em cada nó.  
  
Para instalar um plug\-in, siga as instruções do fornecedor plug\-in. Para registrar manualmente um plug\-in com CAU, execute o [Register CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet em cada computador onde o plug\-in está instalado.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar um argumentos plug\-in e plug\-in  
  
### <a name="specify-a-cau-plug-in"></a>Especificar um CAU plug\-in

Na CAU UI, você selecionar um plug\-in em uma lista suspensa drop\ de plug\-ins disponíveis quando você usa CAU para executar as seguintes ações:  
  
-   Aplicar atualizações ao cluster  
  
-   Atualizações de visualização para o cluster  
  
-   Configurar as opções de atualização self\ de cluster  
  
Por padrão, CAU seleciona o plug\-in **Microsoft.WindowsUpdatePlugin**. No entanto, você pode especificar qualquer plug\-in que está instalado e registrado com CAU.

> [!TIP]  
> Na CAU UI, você pode especificar apenas um único plug\-in para CAU usar para visualizar ou para aplicar atualizações durante a executar uma atualização. Usando os cmdlets do PowerShell CAU, você pode especificar um ou mais plug\-ins. Se você precisar instalar vários tipos de atualizações no cluster, é geralmente mais eficiente para especificar vários plug\-ins em uma atualização executar, em vez de usar um separado atualizando executado para cada plug\-in. Por exemplo, menos reinicializações do nó normalmente ocorrerá.

Usando os cmdlets do PowerShell CAU que estão listados na tabela a seguir, você pode especificar um ou mais plug\-ins para uma atualização executar ou verificação passando o **– CauPluginName** parâmetro. Você pode especificar vários nomes de plug\ separando-os com vírgulas. Se você especificar vários plug\-ins, você também pode controlar como os plug\-ins influenciam uns aos outros durante uma atualização executar especificando o **\-RunPluginsSerially**, **\-StopOnPluginFailure**, e **– SeparateReboots** parâmetros. Para obter mais informações sobre como usar vários plug\-ins, use os links fornecidos para a documentação do cmdlet na tabela a seguir.  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Adiciona a função clusterizada CAU que fornece a funcionalidade atualizar self\ ao cluster especificado.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Faz uma verificação de nós de cluster de atualizações aplicáveis e instala essas atualizações por meio de uma atualização executados no cluster especificado.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Faz uma verificação de nós de cluster de atualizações aplicáveis e retorna uma lista do conjunto inicial das atualizações que poderiam ser aplicadas para cada nó no cluster especificado.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Define as propriedades de configuração para a função clusterizada CAU no cluster especificado.|  
  
Se você não especificar um parâmetro CAU plug\-in usando esses cmdlets, o padrão é o plug\-in **Microsoft.WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especificar CAU plug\-in argumentos  
Quando você configurar as opções de atualização executar, você pode especificar um ou mais *name\ = value* pares \(arguments\) para selecionadas plug\-in para usar. Por exemplo, na CAU UI, você pode especificar vários argumentos da seguinte maneira:  
  
**Name1\ = valor1; Name2\ = valor2; Name3\ = valor3**  
  
Esses *name\ = value* pares devem ser significativos para plug\ do que você especificar. Para alguns plug\-ins, os argumentos são opcionais.  
  
A sintaxe dos argumentos de plug\ de CAU segue essas regras gerais:  
  
-   Vários *name\ = value* pares são separados por ponto e vírgula.  
  
-   Um valor que contém espaços entre aspas, por exemplo: **Name1\ = "Value com espaços"**.  
  
-   A sintaxe exata de *valor* depende do plug\-in.  
  
Para especificar os argumentos de plug\ usando os cmdlets do PowerShell CAU que dão suporte a **– CauPluginParameters** parâmetro, passo um parâmetro do formulário:  
  
**\-CauPluginArguments @{Name1\ = valor1; Name2\ = valor2; Name3\ = valor3}**  
  
Você também pode usar uma tabela de hash predefinida do PowerShell. Para especificar argumentos de plug\ por mais de um plug\-in, passe várias tabelas de hash dos argumentos, separados por vírgulas. Passe os argumentos de plug\ na ordem de plug\ que é especificada no **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar os argumentos de plug\ opcionais  
Plug\-ins que instala CAU \ (**Microsoft.WindowsUpdatePlugin** e **Microsoft.HotfixPlugin**\) fornecem opções adicionais que você pode selecionar. Na CAU UI, elas aparecem em um **opções adicionais** página depois de configurar as opções de atualização executar plug\-in do. Se você estiver usando os cmdlets do PowerShell CAU, essas opções são configuradas como argumentos de plug\ opcionais. Para obter mais informações, consulte [usar o Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [usar o Microsoft.HotfixPlugin](#BKMK_HFP) posteriormente neste tópico.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gerenciar plug\-ins usando os cmdlets do Windows PowerShell  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera informações sobre um ou mais software atualizando plug\-ins que são registrados no computador local.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra um software CAU atualizando plug\-in no computador local.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Remove um software de atualização plug\-in na lista de plug\-ins que podem ser usados por CAU. **Observação:** plug\-ins que são instalados com CAU \ (**Microsoft.WindowsUpdatePlugin** e o **Microsoft.HotfixPlugin**\) não pode ser cancelado.|  
  
## <a name="BKMK_WUP"></a>Usando o Microsoft.WindowsUpdatePlugin  

O padrão plug\-in para CAU, **Microsoft.WindowsUpdatePlugin**, executa as seguintes ações:
- Se comunica com o Windows Update Agent em cada nó de cluster de failover para aplicar as atualizações que são necessárias para os produtos da Microsoft que estão em execução em cada nó.
- Instala atualizações de cluster diretamente do Windows Update ou o Microsoft Update, ou de um servidor do Windows Server Update Services \(WSUS\) on\ local.
- Instalações só selecionadas, distribuição geral lançam \(GDR\) atualizações. Por padrão, o plug\-in se aplica somente atualizações de software importantes. Nenhuma configuração é necessária. A configuração padrão baixa e instala as atualizações GDR importantes em cada nó. 

> [!NOTE]
> Para aplicar as atualizações que não sejam as atualizações de software importantes que estão selecionadas por padrão \ (por exemplo, atualizações de driver), você pode configurar um parâmetro de plug\ opcional. Para obter mais informações, consulte [configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- O cluster de failover e remoto coordenador de atualização computador \(if used\) devem atender aos requisitos para CAU e a configuração é necessária para o gerenciamento remoto listados nas [requisitos e as práticas recomendadas para CAU](cluster-aware-updating-requirements.md).
- Análise [recomendações para aplicar atualizações do Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA)e, em seguida, faça as alterações necessárias para a configuração do Microsoft Update para os nós de cluster de failover.
- Para obter melhores resultados, recomendamos que você execute o \(BPA\) CAU analisador de práticas recomendadas para garantir que o ambiente de cluster e atualização estão configuradas corretamente para aplicar atualizações usando CAU. Para obter mais informações, consulte [CAU de teste de preparação para a atualização](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> As atualizações que exigem a aceitação dos termos de licença do Microsoft ou requerem interação do usuário são excluídas e deve ser instalados manualmente.

### <a name="additional-options"></a>Opções adicionais

Opcionalmente, você pode especificar os seguintes argumentos plug\-in para ampliar ou restringir o conjunto de atualizações que são aplicadas pelo plug\-in:
- Para configurar o plug\-in para aplicar as atualizações recomendadas além de atualizações importantes em cada nó, na CAU UI, o **opções adicionais** página, selecione o **envie-me atualizações recomendadas da mesma maneira que eu recebo atualizações importantes** caixa de seleção.
<br>Como alternativa, configurar o **'IncludeRecommendedUpdates' \ = 'True'** plug\ no argumento.
- Para configurar o plug\-in para filtrar os tipos de atualizações GDR que são aplicadas a cada nó de cluster, especificar uma cadeia de caracteres de consulta de agente de atualização do Windows usando um **QueryString** plug\ no argumento. Para obter mais informações, consulte [configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar a cadeia de caracteres de consulta de agente do Windows Update  
Você pode configurar um argumento de plug\ para o padrão plug\-in, **Microsoft.WindowsUpdatePlugin**, que consiste em uma cadeia de caracteres do agente do Windows Update \(WUA\) consulta. Esta instrução usa a API WUA para identificar um ou mais grupos de atualizações da Microsoft para aplicar a cada nó, com base em critérios de seleção específica. Você pode combinar vários critérios usando uma lógica e ou um OR lógico. A cadeia de caracteres de consulta WUA é especificada em um argumento de plug\ da seguinte maneira:  
  
**QueryString\ = "Criterion1\ = e Value1 \ / ou Criterion2\ = e Value2 \ / ou..."**  
  
Por exemplo, **Microsoft.WindowsUpdatePlugin** selecionará automaticamente as atualizações importantes usando um padrão **QueryString** argumento que é construído usando o **IsInstalled**, **tipo**, **IsHidden**, e **IsAssigned** critérios:  
  
**QueryString\ = "IsInstalled\ = 0 e Type\ = 'Software' e IsHidden\ = 0 e IsAssigned\ = 1"**  
  
Se você especificar um **QueryString** argumento, ele é usado no lugar do padrão **QueryString** que está configurado para o plug\-in.  
  
#### <a name="example-1"></a>Exemplo 1
  
Para configurar um **QueryString** argumento que instala uma atualização específica, conforme identificado pela ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\ = "UpdateID\ = 'f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' e IsInstalled\ = 0"**  
  
> [!NOTE]  
> O exemplo anterior é válido para aplicar atualizações usando o Assistente de atualização Cluster\ reconhecimento. Se você deseja instalar uma atualização específica, configurando opções de atualização self\ com o CAU UI ou usando o **Add\ CauClusterRole** ou **Set \ CauClusterRole**cmdlet do PowerShell, você deve formatar o valor UpdateID com dois caracteres single\ citação:  
>   
> **QueryString\ = "UpdateID\ = ' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' e IsInstalled\ = 0"**  
  
#### <a name="example-2"></a>Exemplo 2
  
Para configurar um **QueryString** argumento que instala somente os drivers:  
  
**QueryString\ = "IsInstalled\ = 0 e Type\ = 'Driver' e IsHidden\ = 0"**  
  
Para saber mais sobre cadeias de caracteres de consulta para o padrão plug\-in, **Microsoft.WindowsUpdatePlugin**, os critérios de pesquisa \ (como **IsInstalled**\), e a sintaxe que você pode incluir nas cadeias de caracteres de consulta, consulte a seção sobre critérios de pesquisa a [referência de API do Windows Update Agent (WUA)](http://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Use o Microsoft.HotfixPlugin  
O plug\-in **Microsoft.HotfixPlugin** pode ser usado para aplicar as atualizações do Microsoft limitada distribuição release \(LDR\) \ (também chamado de hotfixes e anteriormente conhecido como QFEs\) que você baixe independentemente para resolver problemas específicos de software Microsoft. O plug-in instala atualizações de uma pasta raiz em um compartilhamento de arquivos SMB e também pode ser personalizado para aplicar atualizações de BIOS, firmware e driver non\ da Microsoft.

> [!NOTE]
> Hotfixes, às vezes, estão disponíveis para download da Microsoft nos artigos da Base de Conhecimento, mas eles também são fornecidos para clientes em uma base necessário as\.

### <a name="requirements"></a>Requisitos

- O cluster de failover e remoto coordenador de atualização computador \(if used\) devem atender aos requisitos para CAU e a configuração é necessária para o gerenciamento remoto listados nas [requisitos e as práticas recomendadas para CAU](cluster-aware-updating-requirements.md).
- Análise [recomendações para usar o Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obter melhores resultados, recomendamos que você execute o modelo \(BPA\) CAU analisador de práticas recomendadas para garantir que o ambiente de cluster e atualização estão configuradas corretamente para aplicar atualizações usando CAU. Para obter mais informações, consulte [CAU de teste de preparação para a atualização](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obter as atualizações do fornecedor e copie-os ou extraí-los para um compartilhamento de arquivos Server Message Block \(SMB\) \ (hotfix raiz pasta \) que dá suporte a pelo menos SMB 2.0 e o que é acessível por todos os nós de cluster e o computador remoto de coordenador de atualização \ (se CAU é usado no modo de atualização remote\ \). Para obter mais informações, consulte [configurar uma estrutura de pastas raiz hotfix](#BKMK_HF_ROOT) posteriormente neste tópico. 

    > [!NOTE]
    > Por padrão, este plug\-in só instala hotfixes com as seguintes extensões de nome de arquivo:. msp,. msi e. msu.

- Copie o arquivo DefaultHotfixConfig.xml \ (que é fornecido no **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating** pasta em um computador em que as ferramentas CAU são installed\) para a pasta raiz hotfix que você criou e em que você extraiu os hotfixes. Por exemplo, copie o arquivo de configuração *\\\MyFileServer\\Hotfixes\\Root\\*. 

    > [!NOTE]
    > Para instalar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração de hotfix padrão pode ser usado sem modificação. Se seu cenário exigir, você pode personalizar o arquivo de configuração como uma tarefa avançada. O arquivo de configuração pode incluir regras personalizadas, por exemplo, para manipular os arquivos de hotfix com extensões específicas, ou para definir comportamentos para condições de saída específico. Para obter mais informações, consulte [personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) posteriormente neste tópico.

### <a name="configuration"></a>Configuração

Defina as seguintes configurações. Para obter mais informações, consulte os links para seções deste tópico.
- O caminho para a pasta raiz hotfix compartilhado que contém as atualizações para aplicar e que contém o arquivo de configuração de hotfix. Você pode digitar este caminho na UI CAU ou definir o **HotfixRootFolderPath\ = \ < Path >** PowerShell plug\ no argumento. 

   > [!NOTE]
   > Você pode especificar a pasta raiz de hotfix como um caminho da pasta local ou um caminho UNC do formulário *\\\ServerName\\Share\\RootFolderName *. Um caminho de Namespace de DFS autônomos ou com base em domain\ pode ser usado. No entanto, os recursos de plug\ que verifique as permissões de acesso no arquivo de configuração hotfix são incompatíveis com um caminho de Namespace de DFS, portanto, se você definir um, você deve desabilitar a verificação de permissões de acesso usando a CAU UI ou configurando o **DisableAclChecks\ = 'True'** plug\ no argumento.
- As configurações no servidor que hospeda a pasta raiz de hotfix para verificar se há permissões adequadas para acessar a pasta e garantir a integridade dos dados acessados a partir do SMB compartilhadas pasta \ (a assinatura SMB ou Encryption\ SMB). Para obter mais informações, consulte [restringir o acesso à pasta raiz de hotfix ](#BKMK_ACL).

### <a name="additional-options"></a>Opções adicionais

- Opcionalmente, configure o plug\-in para criptografia SMB é aplicada ao acessar dados do compartilhamento de arquivo de hotfix. Na CAU UI, sobre o **opções adicionais** página, selecione o **exigir criptografia de SMB em acessar a pasta raiz de hotfix** opção ou definir o **RequireSMBEncryption\ = 'True'** PowerShell plug\ no argumento. 
  > [!IMPORTANT]
  > Você deve executar etapas adicionais de configuração no servidor SMB para habilitar a integridade dos dados SMB com SMB criptografia ou assinatura SMB. Para obter mais informações, consulte a etapa 4 em [restringir o acesso à pasta raiz de hotfix ](#BKMK_ACL). Se você selecionar a opção para impor o uso da criptografia de SMB e a pasta raiz de hotfix não está configurada para acesso usando a criptografia de SMB, execute a atualização falhará.
- Opcionalmente, desabilite as verificações de padrão de permissões suficientes para a pasta raiz de hotfix e o arquivo de configuração de hotfix. Na CAU UI, selecione **desabilitar a seleção de acesso de administrador para o arquivo de configuração e a pasta de raiz hotfix**, ou definir o **DisableAclChecks\ = 'True'** plug\ no argumento.
- Opcionalmente, configure o **HotfixInstallerTimeoutMinutes\ =<Integer>** argumento para especificar por quanto tempo o hotfix plug\-in aguarda até que o processo de instalador de hotfix retornar. \ (O padrão é 30 minutos. \) por exemplo, para especificar um período de tempo limite de duas horas, defina **HotfixInstallerTimeoutMinutes\ = 120**.
- Opcionalmente, configure o **HotfixConfigFileName \ = <name>**plug\ no argumento para especificar um nome para o arquivo de configuração de hotfix que está localizado na pasta raiz hotfix. Se não for especificado, o nome padrão DefaultHotfixConfig.xml é usado.
  
### <a name="BKMK_HF_ROOT"></a>Configurar uma estrutura de pastas raiz hotfix

Para o hotfix plug\-in para trabalhar, hotfixes devem ser armazenados em uma estrutura definida pelo well\ em um compartilhamento de arquivos SMB \ (hotfix raiz pasta \), e você deve configurar o hotfix plug\-in com o caminho para a pasta raiz de hotfix usando o CAU UI ou os cmdlets do PowerShell CAU. Esse caminho é passado para o plug\-in como o **HotfixRootFolderPath** argumento. Você pode escolher uma das várias estruturas pasta raiz hotfix, de acordo com suas necessidades de atualização, conforme mostrado nos exemplos a seguir. Arquivos ou pastas que não são compatíveis com a estrutura são ignoradas.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemplo 1 - estrutura de pastas usado para aplicar hotfixes a todos os nós de cluster
  
Para especificar que hotfixes se aplicam a todos os nós de cluster, copiá-los para uma pasta chamada **CAUHotfix\_All** sob a pasta raiz de hotfix. Neste exemplo, o **HotfixRootFolderPath** plug\ no argumento é definido como *\\\MyFileServer\\Hotfixes\\Root\\*. O **CAUHotfix\_All** pasta contém três atualizações com as extensões msu,. msi e. msp que serão aplicadas a todos os nós de cluster. Os nomes de arquivo de atualização são apenas para fins de ilustração.  
  
> [!NOTE]  
> Neste e os exemplos a seguir, o arquivo de configuração de hotfix com o nome padrão DefaultHotfixConfig.xml é mostrado na necessário local na pasta raiz hotfix.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemplo 2 - estrutura de pastas usado para aplicar algumas atualizações somente a um nó específico
  
Para especificar hotfixes que se aplicam somente a um nó específico, use uma subpasta sob a pasta raiz de hotfix com o nome do nó. Use o nome NetBIOS do nó de cluster, por exemplo, *ContosoNode1*. Em seguida, mova as atualizações que se aplicam somente a esse nó nesta subpasta. No exemplo a seguir, o **HotfixRootFolderPath** plug\ no argumento é definido como *\\\MyFileServer\\Hotfixes\\Root\\*. As atualizações no **CAUHotfix\_All** pasta será aplicada a todos os nós de cluster, e *Node1\_Specific\_Update.msu* será aplicada somente a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Exemplo 3 - estrutura de pastas usado para aplicar as atualizações que não sejam arquivos. msu,. msi e. msp
  
Por padrão, **Microsoft.HotfixPlugin** só se aplica atualizações com a extensão. msp,. msi ou. msu. No entanto, algumas atualizações podem ter diversas extensões e exigem que os comandos de instalação diferente. Por exemplo, talvez seja necessário aplicar uma atualização de firmware com a extensão .exe a um nó em um cluster. Você pode configurar a pasta raiz de hotfix com uma subpasta que indica um específico, tipo de atualização padrão non\ deve ser instalado. Você também deve configurar uma regra de instalação de pasta correspondente que especifica o comando de instalação no `<FolderRules>`elemento no arquivo de configuração XML hotfix.  
  
No exemplo a seguir, o **HotfixRootFolderPath** plug\ no argumento é definido como *\\\MyFileServer\\Hotfixes\\Root\\*. Várias atualizações serão aplicadas a todos os nós de cluster e uma atualização de firmware *SpecialHotfix1.exe* serão aplicadas a *ContosoNode1* usando *FolderRule1*. Para obter informações sobre como configurar *FolderRule1* no arquivo de configuração hotfix, consulte [personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) posteriormente neste tópico.  
  
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
A configuração de hotfix do arquivo como controles **Microsoft.HotfixPlugin** instala os tipos de arquivo hotfix específico em um cluster de failover. O esquema XML para o arquivo de configuração é definido em HotfixConfigSchema.xsd, que está localizado na seguinte pasta em um computador em que as ferramentas CAU são instaladas:  
  
**Pasta de %systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**  
  
Para personalizar o arquivo de configuração de hotfix, copie o arquivo de configuração de exemplo DefaultHotfixConfig.xml deste local para a pasta raiz de hotfix e fazer modificações apropriadas para seu cenário.  
  
> [!IMPORTANT]  
> Para aplicar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração de hotfix padrão pode ser usado sem modificação. Personalização do arquivo de configuração hotfix é uma tarefa apenas em cenários de uso avançado.  
  
Por padrão, o arquivo de XML de configuração de hotfix define as regras de instalação e condições de saída para duas categorias a seguir de hotfixes:  
  
-   Hotfix arquivos com extensões que o plug\ podem instalar por padrão \ (Files \. msu,. msi e. msp).  
  
    Esses itens são definidos como `<ExtensionRules>`elementos no `<DefaultRules>`elemento. Há um `<Extension>`elemento para cada um dos tipos de arquivo padrão com suporte. A estrutura geral do XML é o seguinte:  
  
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
  
    Se você precisar aplicar determinados tipos de atualização em todos os nós de cluster em seu ambiente, você pode definir adicionais `<Extension>`elementos.  
  
-   Hotfix ou outros arquivos de atualização que não são arquivos. msp,. msu ou. msi, por exemplo, BIOS, firmware e drivers da Microsoft non\ atualizações.  
  
    Cada tipo de arquivo padrão non\ for configurado como um `<Folder>`elemento o `<FolderRules>`elemento. O atributo name do `<Folder>`elemento deve ser idêntico ao nome de uma pasta na pasta raiz hotfix que conterá as atualizações do tipo correspondente. A pasta pode ser no **CAUHotfix\_All** pasta ou em uma pasta específica do node\. Por exemplo, se *FolderRule1* é configurado na pasta raiz hotfix, configure o seguinte elemento no arquivo XML para definir um modelo de instalação e sair condições para as atualizações nessa pasta:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
As tabelas a seguir descrevem o `<Template>`atributos e a possível `<ExitConditions>`subelementos.  
  
|`<Template>` atributo|Descrição|  
|--------------------------|---------------|  
|`path`|O caminho completo para o programa de instalação para o tipo de arquivo que é definido na `<Extension name>`atributo.<br /><br />Para especificar o caminho para um arquivo de atualização na estrutura de pastas raiz hotfix, use `$update$`.|  
|`parameters`|Uma cadeia de caracteres de parâmetros necessárias e opcionais para o programa especificado no `path`.<br /><br />Para especificar um parâmetro que é o caminho para um arquivo de atualização na estrutura de pastas raiz hotfix, use `$update$`.|  
  
|`<ExitConditions>` subelemento|Descrição|  
|---------------------------------|---------------|  
|`<Success>`|Define um ou mais códigos de saída que indicam a atualização especificada foi bem-sucedida. Este é um subelemento necessário.|  
|`<Success_RebootRequired>`|Opcionalmente, define um ou mais códigos de saída que indicam a atualização especificada foi bem-sucedida e reinicie o nó. <br>**Observação:** opcionalmente, o `<Folder>`elemento pode conter o `alwaysReboot`atributo. Se este atributo é definido, ele indica que se um hotfix instalado por essa regra retornar um dos códigos de saída que é definido em `<Success>`, ele é interpretado como uma `<Success_RebootRequired>`sair condição.|  
|`<Fail_RebootRequired>`|Opcionalmente, define um ou mais códigos de saída que indicam a atualização especificada falhou e reinicie o nó.|  
|`<AlreadyInstalled>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque ele já está instalado.|  
|`<NotApplicable>`|Opcionalmente, define um ou mais códigos de saída que indicam que a atualização especificada não foi aplicada porque ele não se aplica ao nó de cluster.|  
  
> [!IMPORTANT]  
> Qualquer código que não é definido explicitamente de saída `<ExitConditions>`é interpretado como a atualização falhou e o nó não reiniciar.  
  
### <a name="BKMK_ACL"></a>Restringir o acesso à pasta raiz de hotfix  
Você deve executar várias etapas para configurar o compartilhamento SMB de arquivo e o servidor de arquivos para ajudar a proteger o hotfix arquivos da pasta raiz e o arquivo de configuração de hofix para acesso somente no contexto de **Microsoft.HotfixPlugin**. Essas etapas permitem que vários recursos que ajudam a evitar possíveis adulterações com os arquivos de hotfix de forma que possam comprometer o cluster de failover.  
  
As etapas gerais são da seguinte maneira:  
  
1.  Identificar a conta de usuário que é usada para atualizar executado usando o plug\-in  
  
2.  Configurar essa conta de usuário nos grupos necessários em um servidor de arquivos do SMB  
  
3.  Configurar permissões para acessar a pasta raiz de hotfix  
  
4.  Definir as configurações de integridade de dados do SMB  
  
5.  Habilitar uma regra de Firewall do Windows no servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Etapa 1. Identificar a conta de usuário que é usada para atualizar execuções usando o hotfix plug\-in
  
A conta que é usada em CAU para verificar as configurações de segurança durante a execução de uma atualização executar usando **Microsoft.HotfixPlugin** depende se CAU é usada no atualizando remote\ modo ou atualizando self\, da seguinte maneira:  
  
-   **Modo de atualização Remote\** a conta que possui privilégios administrativos no cluster para visualizar e aplicar as atualizações.  
  
-   **Modo de atualização Self\** o nome do objeto de computador virtual configurado no Active Directory para o CAU clusterizado função. Isso é o nome de um objeto de computador virtual inseridos no Active Directory para a função clusterizado CAU ou o nome que é gerado pelo CAU para a função de cluster. Para obter o nome se ele é gerado pelo CAU, execute o **Get\ CauClusterRole** cmdlet do PowerShell CAU. Na saída do **ResourceGroupName** é o nome da conta do objeto de computador virtual gerado.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Etapa 2. Configurar essa conta de usuário nos grupos necessários em um servidor de arquivos do SMB
  
> [!IMPORTANT]  
> Você deve adicionar a conta que é usada para atualizar execuções como uma conta de administrador local no servidor SMB. Se isso não é permitido devido as políticas de segurança em sua organização, configure essa conta com as permissões necessárias no servidor SMB usando o procedimento a seguir.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar uma conta de usuário no servidor SMB  
  
1.  Adicione a conta que é usada para execuções atualizando para o grupo distribuído COM usuários e um dos seguintes grupos: usuário avançado, a operação de servidor ou operador Print.  
  
2.  Para habilitar as permissões necessárias do WMI para a conta, inicie o Console de gerenciamento WMI no servidor SMB. Inicie o PowerShell e, em seguida, digite o seguinte comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Na árvore de console, clique em right\ **\(Local\) controle de WMI**e clique em **propriedades**.  
  
4.  Clique em **segurança**e, em seguida, expanda **raiz**.  
  
5.  Clique em **CIMV2**e clique em **segurança**.  
  
6.  Adicione a conta que é usada para execuções de atualizar para o **nomes de usuário ou grupo** lista.  
  
7.  Concessão o **executar métodos** e **ativação remota** permissões para a conta que é usado para execuções de atualização.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Etapa 3. Configurar permissões para acessar a pasta raiz de hotfix
  
Por padrão, quando você tentar aplicar atualizações, o hotfix plug\-in verifica a configuração das permissões de sistema de arquivo NTFS para acessar a pasta raiz hotfix. Se as permissões de acesso à pasta não estiverem configuradas corretamente, uma atualização executados usando-se o hotfix plug\-in poderá falhar.  
  
Se você usar a configuração padrão do hotfix plug\-in, certifique-se de que as permissões de acesso à pasta atendem aos seguintes requisitos.  
  
-   O grupo usuários tem permissão de leitura.  
  
-   Se o plug\-in as atualizações serão aplicadas com a extensão .exe, o grupo usuários tem permissão de execução.  
  
-   São permitidas apenas determinadas entidades de segurança \ (mas não são required\) de gravação ou modificar a permissão. Os objetos permitidos são o local Administradores grupo, sistema, proprietário criador e TrustedInstaller. Outras contas ou grupos não são permitidos para gravação ou modificar a permissão na pasta raiz hotfix.  
  
Opcionalmente, você pode desabilitar as verificações anteriores que o plug\-in executa por padrão. Você pode fazer isso de duas maneiras:  
  
-   Se você estiver usando os cmdlets do PowerShell CAU, configure o **DisableAclChecks\ = 'True'** argumento no **CauPluginArguments** parâmetro para o hotfix plug\-in.  
  
-   Se você estiver usando o CAU UI, selecione o **desabilitar a seleção de acesso de administrador para o arquivo de configuração e a pasta de raiz hotfix** opção no **opções adicionais de atualização** página do assistente que é usado para configurar as opções de atualização executar.  
  
No entanto, como uma prática recomendada em muitos ambientes, recomendamos que você use a configuração padrão para impor essas verificações.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Etapa 4. Definir as configurações de integridade de dados do SMB
  
Para verificar a integridade de dados nas conexões entre os nós de cluster e o compartilhamento de arquivos SMB, o hotfix plug\-in exige que você habilite as configurações no compartilhamento de arquivos SMB para SMB criptografia ou assinatura SMB. Criptografia de SMB, que fornece segurança aprimorada e melhor desempenho em muitos ambientes, há suporte para a partir do Windows Server 2012. Você pode habilitar uma ou ambas essas configurações, da seguinte maneira:  
  
-   Para habilitar a assinatura SMB, consulte o procedimento a [887429 do artigo](http://support.microsoft.com/kb/887429) na Base de Conhecimento Microsoft.  
  
-   Para ativar a criptografia do SMB para a pasta compartilhada SMB, execute o seguinte cmdlet do PowerShell no servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Onde <*compartilhamento*> é o nome da pasta compartilhada SMB.  
  
Opcionalmente, para impor o uso da criptografia SMB nas conexões para o servidor SMB, selecione o **exigir criptografia de SMB em acessar a pasta raiz de hotfix** opção na UI CAU ou definir o **RequireSMBEncryption\ = 'True'** argumento de plug\ usando os cmdlets do PowerShell CAU.  
  
> [!IMPORTANT]  
> Se você selecionar a opção para impor o uso da criptografia de SMB e a pasta raiz de hotfix não está configurada para conexões que usam criptografia SMB, execute a atualização falhará.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Etapa 5. Habilitar uma regra de Firewall do Windows no servidor SMB
  
Você deve habilitar o **gerenciamento remoto do servidor de arquivo \(SMB\-in\)** regra no Firewall do Windows no servidor de arquivos SMB. Isso é habilitado por padrão no Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da atualização suporte a cluster](cluster-aware-updating.md)
  
-   [Suporte a cluster atualização Cmdlets do Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Suporte a cluster Atualizando referência plug-in](http://msdn.microsoft.com/library/hh418084.aspx)  
  
