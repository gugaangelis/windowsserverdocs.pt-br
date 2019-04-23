---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Como funcionam os plug-ins de atualização com suporte a Cluster
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Como usar o plug-ins para coordenar as atualizações ao usar o Cluster-Aware Updating no Windows Server para instalar atualizações em um cluster.
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854557"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Como funcionam os plug-ins de atualização com suporte a Cluster

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Cluster-Aware Updating](cluster-aware-updating.md) (CAU) usa o plug-ins para coordenar a instalação das atualizações em nós em um cluster de failover. Este tópico fornece informações sobre como usar a compilação\-plug CAU\-ins ou outro plug\-ins que você instala para CAU.

## <a name="BKMK_INSTALL"></a>Instalar um plug-\-em  
Um plug\-na diferente do padrão conecta\-ins que são instalados com o CAU \( **windowsupdateplugin** e **Microsoft. hotfixplugin** \)deve ser instalado separadamente. Se o CAU for usado no próprio\-atualizando modo, o plugue\-em deve ser instalado em todos os nós de cluster. Se for usado CAU no remoto\-atualizando modo, o plug\-em deve ser instalado no computador remoto do coordenador de atualização. Um plug\-na qual você instala pode ter requisitos adicionais de instalação em cada nó.  
  
Para instalar um plug\-, siga as instruções do plugue\-no publicador. Para registrar manualmente um plug\-com CAU, execute o [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet em cada computador em que o plugue\-é instalado.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Especificar um plug\-em e conecte\-argumentos de entrada  
  
### <a name="specify-a-cau-plug-in"></a>Especificar um plug CAU\-em

Na UI CAU, você seleciona um plug\-em de uma operação de soltar\-para baixo na lista de plugue disponível\-ins quando você usa o CAU para executar as seguintes ações:  
  
-   Aplicar atualizações ao cluster  
  
-   Visualizar atualizações para o cluster  
  
-   Configurar o cluster self\-opções de atualização  
  
Por padrão, a CAU seleciona o plugue\-na **windowsupdateplugin**. No entanto, você pode especificar qualquer plug\-em que está instalado e registrado com o CAU.

> [!TIP]  
> Na UI CAU, você só pode especificar um único plug\-na Cau usar para visualizar ou aplicar atualizações durante uma execução de atualização. Usando os cmdlets do PowerShell do CAU, você pode especificar um ou mais plug\-ins. Se você precisar instalar vários tipos de atualizações no cluster, ele normalmente é mais eficiente especificar vários plug\-ins em uma execução de atualização, em vez de usar uma execução atualizando separado para cada plug\-no. Por exemplo, normalmente ocorrerá um número menor de reinicializações de nós.

Usando os cmdlets do PowerShell de CAU que estão listados na tabela a seguir, você pode especificar um ou mais plug\-ins para uma execução de atualização ou examinar passando o **– CauPluginName** parâmetro. Você pode especificar vários plug\-em nomes, separando-as com vírgulas. Se você especificar vários plug\-ins, você também pode controlar como o plugue\-ins influenciam uns aos outros durante uma execução de atualização, especificando as  **\-RunPluginsSerially**,  **\- StopOnPluginFailure**, e **– SeparateReboots** parâmetros. Para obter mais informações sobre como usar vários plug\-ins, use os links fornecidos para a documentação de cmdlet na tabela a seguir.  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Adiciona a função clusterizada de CAU que fornece o self\-atualizando funcionalidade ao cluster especificado.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e instala essas atualizações através de uma Execução de Atualização no cluster especificado.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Executa uma varredura de nós do cluster quanto às atualizações aplicáveis ​​e retorna uma lista do conjunto inicial de atualizações que seriam aplicadas a cada nó no cluster especificado.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Define as propriedades de configuração para a função clusterizada do CAU no cluster especificado.|  
  
Se você não especificar um plug CAU\-no parâmetro usando esses cmdlets, o padrão é o plugue\-na **windowsupdateplugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Especifique a CAU conecte\-argumentos de entrada  
Quando você configura as opções de execução de atualização, você pode especificar um ou mais *nome\=valor* pares \(argumentos\) para o plugue selecionado\-para usar. Por exemplo, na IU do CAU, é possível especificar vários argumentos, da seguinte forma:  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
Eles *nome\=valor* pares devem ser significativos para o plugue\-em que você especifica. Para alguns plug\-ins os argumentos são opcionais.  
  
A sintaxe de plugue o CAU\-nos argumentos segue estas regras gerais:  
  
-   Vários *nome\=valor* pares são separados por ponto e vírgula.  
  
-   Um valor que contém espaços fica entre aspas, por exemplo: **Nome1\="Valor com espaços"**.  
  
-   A sintaxe exata do *valor* depende o plugue\-no.  
  
Para especificar o plug\-argumentos de entrada usando os cmdlets do PowerShell de CAU que dão suporte a **– CauPluginParameters** parâmetro, passe um parâmetro do formulário:  
  
**\-CauPluginArguments @{Name1\=Value1;Name2\=Value2;Name3\=Value3}**  
  
Você também pode usar uma tabela de hash do PowerShell predefinida. Para especificar plug\-argumentos de entrada para mais de um plug\-, passe várias tabelas de hash de argumentos, separados por vírgulas. Passe o plugue\-nos argumentos de plug\-que é especificado em **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Especificar plug opcional\-argumentos de entrada  
O plugue\-ins CAU instala \( **windowsupdateplugin** e **Microsoft. hotfixplugin** \) fornecem opções adicionais que você pode selecionar. Na UI do CAU, elas aparecem em uma **opções adicionais** página depois de configurar opções de execução de atualização para o plugue\-no. Se você estiver usando os cmdlets do PowerShell do CAU, essas opções são configuradas como opcional plug\-argumentos de entrada. Para obter mais informações, consulte [Use o Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usar o Microsoft.HotfixPlugin](#BKMK_HFP) mais adiante neste tópico.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gerenciar plug\-ins usando cmdlets do Windows PowerShell  
  
|Cmdlet|Descrição|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera informações sobre um ou mais plug de atualização de software\-ins registrados no computador local.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra um plug de atualização de software do CAU\-no computador local.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Remove um plug de atualização de software\-na lista de plug\-ins que podem ser usados pelo CAU. **Observação:** O plugue\-ins que são instalados com o CAU \( **windowsupdateplugin** e o **Microsoft. hotfixplugin** \) não pode ser cancelado.|  
  
## <a name="BKMK_WUP"></a>Usando o Microsoft. windowsupdateplugin  

O padrão de plugue\-na Cau, **Microsoft. windowsupdateplugin**, executa as seguintes ações:
- Comunica-se com o Windows Update Agent em cada nó do cluster de failover para aplicar atualizações necessárias aos produtos da Microsoft que estão sendo executados em cada nó.
- Instalações de cluster diretamente as atualizações do Windows Update ou Microsoft Update ou de na\-local do Windows Server Update Services \(WSUS\) server.
- Instala somente os selecionados, versão de distribuição geral \(GDR\) atualizações. Por padrão, o plugue\-em se aplica apenas atualizações de software importantes. Nenhuma configuração é necessária. A configuração padrão baixa e instala atualizações importantes de GDR em cada nó. 

> [!NOTE]
> Para aplicar as atualizações que não sejam as atualizações de software importantes selecionadas por padrão \(por exemplo, atualizações de driver\), você pode configurar um plug opcional\-no parâmetro. Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador de coordenador de atualização remoto \(se usado\) deve cumprir os requisitos para a CAU e a configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas da CAU ](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para aplicação de atualizações da Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA) e faça as alterações necessárias à configuração do Microsoft Update para os nós do cluster de failover.
- Para obter melhores resultados, recomendamos que você execute o analisador de práticas recomendadas do CAU \(BPA\) para garantir que o ambiente de cluster e as atualizações estejam configurados corretamente para aplicar atualizações usando CAU. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> As atualizações que exigem aceitação de termos de licença da Microsoft ou interação do usuário são excluídas, e devem ser instaladas manualmente.

### <a name="additional-options"></a>Opções Adicionais

Opcionalmente, você pode especificar os seguinte plug\-argumentos de entrada para aumentar ou restringir o conjunto de atualizações aplicadas pelo plugue\-em:
- Para configurar o plug\-para aplicar as atualizações recomendadas além das atualizações importantes em cada nó, da UI CAU, em de **opções adicionais** página, selecione o **Dê-me recomendado atualiza a mesma forma que Eu recebo atualizações importantes** caixa de seleção.
<br>Como alternativa, configure a **'IncludeRecommendedUpdates'\='True'** conecte\-no argumento.
- Para configurar o plugue\-para filtrar os tipos de atualizações GDR que são aplicados a cada nó de cluster, especifique uma cadeia de caracteres de consulta do Windows Update Agent usando um **QueryString** conecte\-no argumento. Para mais informações, consulte [Configurar a cadeia de caracteres de consulta do Windows Update Agent](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurar a cadeia de caracteres de consulta do Windows Update Agent  
Você pode configurar um plug\-no argumento para o padrão conecte\-no, **windowsupdateplugin**, que consiste em um Windows Update Agent \(WUA\) cadeia de caracteres de consulta. Essa instrução usa a API do WUA para identificar um ou mais grupos de atualizações da Microsoft para aplicar a cada nó, com base em critérios de seleção específicos. Você pode combinar vários critérios, usando um AND ou um OR lógico. A cadeia de caracteres de consulta do WUA é especificada em um plug\-no argumento da seguinte maneira:  
  
**QueryString\="critérion1\=Value1 e\/ou critérion2\=Value2 e\/ou..."**  
  
Por exemplo, o **Microsoft.WindowsUpdatePlugin** seleciona automaticamente as atualizações importantes, usando um argumento padrão **QueryString** que é construído usando os critérios **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:  
  
**QueryString\="IsInstalled\=0 e o tipo\='Software' e IsHidden\=0 e IsAssigned\=1"**  
  
Se você especificar um **QueryString** argumento, ele é usado em vez do padrão **QueryString** que está configurado para o plugue\-no.  
  
#### <a name="example-1"></a>Exemplo 1
  
Para configurar uma **QueryString** argumento que instala uma atualização específica conforme identificado pela ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="UpdateID\=' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' e IsInstalled\=0"**  
  
> [!NOTE]  
> O exemplo anterior é válido para aplicar atualizações usando o Cluster\-Assistente de atualização com suporte. Se você deseja instalar uma atualização específica configurando self\-opções de atualização, a UI do CAU ou usando o **Add\-CauClusterRole** ou **definir\-CauClusterRole**Cmdlet do PowerShell, é necessário formatar o valor UpdateID com dois único\-caracteres de aspas:  
>   
> **QueryString\="UpdateID\=' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' e IsInstalled\=0"**  
  
#### <a name="example-2"></a>Exemplo 2
  
Para configurar um argumento **QueryString** que instala somente drivers:  
  
**QueryString\="IsInstalled\=0 e o tipo\='Driver' e IsHidden\=0"**  
  
Para obter mais informações sobre cadeias de caracteres de consulta para o padrão conecte\-no, **windowsupdateplugin**, os critérios de pesquisa \(como **IsInstalled**\), e a sintaxe que você pode incluir nas cadeias de caracteres de consulta, consulte a seção sobre critérios de pesquisa a [referência de API do Windows Update Agent (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usar o Microsoft. hotfixplugin  
O plugue\-na **Microsoft. hotfixplugin** pode ser usado para aplicar a versão de distribuição limitada \(LDR\) atualizações \(também chamado de hotfixes e chamados de QFEs\) que você baixar de forma independente para abordar problemas específicos de software Microsoft. O plug-in instala as atualizações de uma pasta raiz em um compartilhamento de arquivo SMB e também podem ser personalizado para aplicar não\-driver da Microsoft, firmware e atualizações de BIOS.

> [!NOTE]
> Hotfixes, às vezes, estão disponíveis para download da Microsoft nos artigos da Base de dados de Conhecimento, mas eles também são fornecidos aos clientes que\-base necessária.

### <a name="requirements"></a>Requisitos

- O cluster de failover e o computador de coordenador de atualização remoto \(se usado\) deve cumprir os requisitos para a CAU e a configuração necessária para o gerenciamento remoto listado em [requisitos e práticas recomendadas da CAU ](cluster-aware-updating-requirements.md).
- Examine as [Recomendações para uso do Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Para obter melhores resultados, recomendamos que você execute o analisador de práticas recomendadas do CAU \(BPA\) modelo para garantir que o ambiente de cluster e as atualizações estejam configurados corretamente para aplicar atualizações usando CAU. Para mais informações, consulte [Testar a prontidão de atualização do CAU](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obter as atualizações do publicador e copiá-los ou extraia-os em um Server Message Block \(SMB\) compartilhamento de arquivos \(pasta raiz de hotfix\) que dá suporte a pelo menos SMB 2.0 e que é acessível por todos os do cluster nós e o computador de coordenador de atualização remoto \(se for usado CAU no remoto\-modo de atualização\). Para mais informações, consulte [Configurar uma estrutura de pastas raiz de hotfix](#BKMK_HF_ROOT) mais adiante neste tópico. 

    > [!NOTE]
    > Por padrão, esse plug-\-só instala hotfixes com as seguintes extensões de nome de arquivo:. msu,. msi e. msp.

- Copie o arquivo Defaulthotfixconfig \(que é fornecido na **% systemroot %\\System32\\WindowsPowerShell\\v1.0\\módulos\\ ClusterAwareUpdating** pasta em um computador em que as ferramentas da CAU são instaladas\) para a pasta raiz de hotfix que você criou e sob a qual você extraiu os hotfixes. Por exemplo, copie o arquivo de configuração para  *\\ \\MyFileServer\\Hotfixes\\raiz\\*. 

    > [!NOTE]
    > Para instalar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. Se o cenário exigir, você pode personalizar o arquivo de configuração como uma tarefa avançada. O arquivo de configuração pode incluir regras personalizadas, por exemplo, para processar arquivos de hotfix com extensões específicas ou definir comportamentos para condições de saída específicas. Para mais informações, consulte [Personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.

### <a name="configuration"></a>Configuração

Defina as configurações a seguir. Para mais informações, consulte os links para as seções mais adiante neste tópico.
- O caminho para a pasta raiz compartilhada de hotfix que contém as atualizações a serem aplicadas e o arquivo de configuração do hotfix. Você pode digitar este caminho na UI do CAU ou configurar o **HotfixRootFolderPath\=\<caminho >** PowerShell plug\-no argumento. 

   > [!NOTE]
   > Você pode especificar a pasta raiz de hotfix como um caminho de pasta local ou um caminho UNC no formato  *\\ \\ServerName\\compartilhamento\\RootFolderName*. Um domínio\-com base ou caminho de Namespace de DFS autônomo pode ser usado. No entanto, o plugue\-em recursos que verificar o acesso a permissões no arquivo de configuração de hotfix são incompatíveis com um caminho de Namespace do DFS, assim, se você configurar um, você deverá desabilitar a verificação de permissões de acesso por meio da UI CAU ou configurando o **DisableAclChecks\='True'** conecte\-no argumento.
- Pasta compartilhada de configurações no servidor que hospeda a pasta raiz de hotfix para verificar se há permissões adequadas para acessar a pasta e garantir a integridade dos dados acessados a partir do SMB \(assinatura SMB ou criptografia SMB\). Para obter mais informações, consulte [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL).

### <a name="additional-options"></a>Opções Adicionais

- Opcionalmente, configure o plugue\-assim que a criptografia SMB é imposta ao acessar dados do compartilhamento de arquivos de hotfix. Na UI CAU, sobre o **opções adicionais** página, selecione o **exigir criptografia SMB para acessar a pasta raiz de hotfix** ou configure a **RequireSMBEncryption\= 'True'** PowerShell plug\-no argumento. 
  > [!IMPORTANT]
  > Você deve executar etapas adicionais de configuração no servidor SMB para habilitar a integridade dos dados SMB com assinatura ou criptografia SMB. Para mais informações, consulte a Etapa 4 em [Restringir o acesso à pasta raiz de hotfix](#BKMK_ACL). Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para acesso com o uso dessa criptografia SMB, ocorrerá falha na Execução de Atualização.
- Opcionalmente, desabilite as verificações padrão quanto a permissões suficientes para a pasta raiz de hotfix e o arquivo de configuração do hotfix. Na UI CAU, selecione **desabilitar a verificação de acesso de administrador para o arquivo de pasta e a configuração de raiz do hotfix**, ou configurar o **DisableAclChecks\='True'** conecte\-em argumento.
- Opcionalmente, configure a **HotfixInstallerTimeoutMinutes\= <Integer>**  argumento para especificar quanto tempo o hotfix conecte\-em aguarda o retorno do processo de instalador de hotfix. \(O padrão é 30 minutos.\) Por exemplo, para especificar um período de tempo limite de duas horas, defina **HotfixInstallerTimeoutMinutes\=120**.
- Opcionalmente, configure a **HotfixConfigFileName \= <name>**  conecte\-no argumento para especificar um nome para o arquivo de configuração de hotfix que está localizado na pasta raiz de hotfix. Se não for especificado, o nome padrão DefaultHotfixConfig.xml será usado.
  
### <a name="BKMK_HF_ROOT"></a>Configurar uma estrutura de pasta raiz de hotfix

Para que o hotfix conecte\-na solução, hotfixes devem ser armazenados em um bem\-definido a estrutura em um compartilhamento de arquivos SMB \(pasta raiz de hotfix\), e você deve configurar o plugue hotfix\-com o caminho para o pasta raiz de hotfix por meio da UI CAU ou os cmdlets do PowerShell do CAU. Esse caminho é passado para o plugue\-em como o **HotfixRootFolderPath** argumento. Você pode escolher uma das várias estruturas para a pasta raiz de hotfix, de acordo com as suas necessidades de atualização, como mostrado nos exemplos a seguir. Arquivos ou pastas que não aderem à estrutura são ignorados.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemplo 1: estrutura de pastas usado para aplicar hotfixes a todos os nós de cluster
  
Para especificar que os hotfixes sejam aplicados a todos os nós de cluster, copie-os para uma pasta chamada **CAUHotfix\_todos os** sob a pasta raiz de hotfix. Neste exemplo, o **HotfixRootFolderPath** conecte\-argumento é definido como *\\ \\MyFileServer\\Hotfixes\\raiz\\*. O **CAUHotfix\_todos os** pasta contém três atualizações com as extensões. msu,. msi e. msp que serão aplicadas a todos os nós de cluster. Os nomes dos arquivos de atualização são apenas para fins de ilustração.  
  
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
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemplo 2 - estrutura de pastas usado para aplicar determinadas atualizações apenas a um nó específico
  
Para especificar hotfixes que se aplicam apenas a um nó específico, use uma subpasta da pasta raiz de hotfix com o nome do nó. Use o nome NetBIOS do nó de cluster, por exemplo, *ContosoNode1*. Em seguida, mova as atualizações que se aplicam apenas a esse nó para essa subpasta. No exemplo a seguir, o **HotfixRootFolderPath** conecte\-argumento é definido como  *\\ \\MyFileServer\\Hotfixes\\raiz\\*. Atualizações na **CAUHotfix\_todas as** pasta será aplicada a todos os nós de cluster, e *Node1\_específicos\_Update.msu* serão aplicadas somente a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Exemplo 3 - estrutura de pastas usada para aplicar as atualizações que não sejam arquivos. msu,. msi e. msp
  
Por padrão, o **Microsoft.HotfixPlugin** só aplica atualizações com as extensões .msu, .msi ou .msp. No entanto, algumas atualizações podem ter extensões diferentes e exigem diferentes comandos de instalação. Por exemplo, você pode precisar aplicar uma atualização de firmware com a extensão .exe a um nó em um cluster. Você pode configurar a pasta raiz de hotfix com uma subpasta que indique uma versão específica, não\-tipo de atualização padrão deve ser instalado. Você também deve configurar uma regra de instalação de pasta correspondente que especifica o comando de instalação no elemento `<FolderRules>` no arquivo XML de configuração de hotfix.  
  
No exemplo a seguir, o **HotfixRootFolderPath** conecte\-argumento é definido como  *\\ \\MyFileServer\\Hotfixes\\raiz\\*. Várias atualizações serão aplicadas a todos os nós do cluster e uma atualização de firmware *SpecialHotfix1.exe* será aplicada ao *ContosoNode1* usando *FolderRule1*. Para obter informações sobre como configurar *FolderRule1* no arquivo de configuração de hotfix, consulte [Personalizar o arquivo de configuração de hotfix](#BKMK_CONFIG_FILE) mais adiante neste tópico.  
  
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
  
**% systemroot %\\System32\\WindowsPowerShell\\v1.0\\módulos\\ClusterAwareUpdating pasta**  
  
Para personalizar o arquivo de configuração de hotfix, copie o arquivo de configuração de amostra DefaultHotfixConfig.xml deste local para a pasta raiz do hotfix e faça as modificações apropriadas para o seu cenário.  
  
> [!IMPORTANT]  
> Para aplicar a maioria dos hotfixes fornecidos pela Microsoft e outras atualizações, o arquivo de configuração padrão de hotfix pode ser usado sem modificação. A personalização do arquivo de configuração de hotfix é uma tarefa apenas em cenários de uso avançados.  
  
Por padrão, o arquivo XML de configuração de hotfix define regras de instalação e condições de saída para as duas categorias de hotfixes a seguir:  
  
-   Arquivos de hotfix com extensões que o plugue\-pode instalar por padrão \(arquivos. msu,. msi e. msp\).  
  
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
  
-   Atualizar o hotfix ou outros arquivos que não são. msi,. msu ou. msp, por exemplo, não\-drivers da Microsoft, firmware e atualizações de BIOS.  
  
    Cada não\-tipo de arquivo padrão está configurado como um `<Folder>` elemento o `<FolderRules>` elemento. O atributo de nome do elemento `<Folder>` deve ser idêntico ao nome de uma pasta na pasta raiz do hotfix que conterá as atualizações do tipo correspondente. A pasta pode estar na **CAUHotfix\_todas as** pasta ou em um nó\-pasta específica. Por exemplo, se *FolderRule1* estiver configurado na pasta raiz de hotfix, configure o seguinte elemento no arquivo XML para definir um modelo de instalação e condições de saída para as atualizações dessa pasta:  
  
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
  
### <a name="BKMK_ACL"></a>Restringir o acesso à pasta raiz de hotfix  
Você deve executar várias etapas para configurar o servidor de arquivos SMB e o compartilhamento de arquivos para ajudar a proteger os arquivos da pasta raiz de hotfix e o arquivo de configuração de hotfix para acesso somente no contexto do **Microsoft.HotfixPlugin**. Essas etapas habilitam vários recursos que ajudam a evitar possíveis violações dos arquivos de hotfix que podem comprometer o cluster de failover.  
  
As etapas gerais são as seguintes:  
  
1.  Identificar a conta de usuário que é usada para execuções de atualização usando o plug\-em  
  
2.  Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB  
  
3.  Configurar permissões para acessar a pasta raiz de hotfix  
  
4.  Definir configurações para a integridade dos dados SMB  
  
5.  Habilitar uma regra de Firewall do Windows no servidor SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Etapa 1. Identificar a conta de usuário que é usada para execuções de atualização usando o plugue hotfix\-em
  
A conta que é usada na CAU para verificar as configurações de segurança ao realizar uma execução de atualização usando **Microsoft. hotfixplugin** depende se a CAU é usada no remoto\-atualizando modo ou self\-atualização modo, como a seguir:  
  
-   **Remoto\-modo de atualização** a conta que tenha privilégios administrativos no cluster para visualizar e aplicar atualizações.  
  
-   **Self\-modo de atualização** o nome do objeto de computador virtual configurado no Active Directory para o CAU função clusterizada. Esse é o nome de um objeto de computador virtual de pré-teste no Active Directory para a função clusterizada do CAU ou o nome gerado pelo CAU para a função clusterizada. Para obter o nome se ele for gerado pelo CAU, execute as **Obtenha\-CauClusterRole** cmdlet do PowerShell de CAU. Na saída, **ResourceGroupName** é o nome da conta do objeto de computador virtual gerado.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Etapa 2. Configurar a conta do usuário nos grupos necessários em um servidor de arquivos SMB
  
> [!IMPORTANT]  
> Você deve adicionar a conta usada para as Execuções de Atualização como uma conta de administrador local no servidor SMB. Se isso não for permitido por causa das políticas de segurança de sua organização, configure essa conta com as permissões necessárias no servidor SMB usando o procedimento a seguir.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Para configurar uma conta de usuário no servidor SMB  
  
1.  Adicione a conta usada para as Execuções de Atualização ao grupo Distributed COM - Usuários e a um dos seguintes grupos: Usuário Avançado, Operação do Servidor ou Operador de Impressão.  
  
2.  Para habilitar as permissões WMI necessárias para a conta, inicie o Console de Gerenciamento de WMI no servidor SMB. Inicie o PowerShell e, em seguida, digite o seguinte comando:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Na árvore de console, com o botão direito\-clique em **Controle WMI \(Local\)** e, em seguida, clique em **propriedades**.  
  
4.  Clique em **Segurança** e expanda **Raiz**.  
  
5.  Clique em **CIMV2**e depois em **Segurança**.  
  
6.  Adicione a conta que é usada para as Execuções de Atualização à lista **Nomes de grupo ou de usuário**.  
  
7.  Conceda as permissões para **Executar Métodos** e **Habilitação Remota** para a conta usada para as Execuções de Atualização.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Etapa 3. Configurar permissões para acessar a pasta raiz de hotfix
  
Por padrão, quando você tenta aplicar as atualizações, o hotfix conecte\-em verifica a configuração das permissões do sistema de arquivos NTFS para o acesso à pasta raiz de hotfix. Se as permissões de acesso da pasta não estiverem configuradas corretamente, uma execução de atualização usando o plugue hotfix\-em pode falhar.  
  
Se você usar a configuração padrão de plugue o hotfix\-, certifique-se de que as permissões de acesso da pasta atendam aos requisitos a seguir.  
  
-   O grupo de Usuários tem permissão de Leitura.  
  
-   Se o plugue\-na aplicará atualizações com a extensão .exe, o grupo de usuários tem permissão Execute.  
  
-   Somente determinadas entidades de segurança são permitidas \(, mas não são necessárias\) para gravar ou modificar permissões. As entidades permitidas são o grupo local de Administradores, SISTEMA, PROPRIETÁRIO CRIADOR e TrustedInstaller. Outras contas ou grupos não têm permissão para Gravar ou Modificar na pasta raiz de hotfix.  
  
Opcionalmente, você pode desabilitar as verificações anteriores que o plugue\-executa por padrão. É possível fazer isso de duas maneiras:  
  
-   Se você estiver usando os cmdlets do PowerShell do CAU, configure a **DisableAclChecks\='True'** argumento no **CauPluginArguments** parâmetro para o plugue hotfix\-no.  
  
-   Se você estiver usando a IU do CAU, selecione a opção **Desabilitar verificação para acesso de administrador à pasta raiz de hotfix e ao arquivo de configuração** na página **Opções Adicionais de Atualização** do assistente usado para configurar as opções de Execução de Atualização.  
  
No entanto, como melhor prática em muitos ambientes, é recomendável usar a configuração padrão para impor essas verificações.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Etapa 4. Definir configurações para a integridade dos dados SMB
  
Para verificar a integridade de dados em que as conexões entre os nós do cluster e o compartilhamento de arquivos SMB, conecte o hotfix\-em requer que você habilite as configurações de compartilhamento de arquivos SMB para a assinatura SMB ou criptografia SMB. Criptografia SMB, que fornece segurança aprimorada e melhor desempenho em muitos ambientes, há suporte para a partir do Windows Server 2012. É possível habilitar uma ou ambas as configurações, da seguinte forma:  
  
-   Para habilitar assinatura SMB, consulte o procedimento no [artigo 887429](https://support.microsoft.com/kb/887429) na Base de Dados de Conhecimento Microsoft.  
  
-   Para habilitar a criptografia SMB para a pasta compartilhada de SMB, execute o seguinte cmdlet do PowerShell no servidor SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Em que <*ShareName*> é o nome da pasta compartilhada de SMB.  
  
Opcionalmente, para impor o uso da criptografia SMB nas conexões com o servidor SMB, selecione a **exigir criptografia SMB para acessar a pasta raiz de hotfix** da UI do CAU ou configure o **RequireSMBEncryption \='True'** conecte\-no argumento usando os cmdlets do PowerShell do CAU.  
  
> [!IMPORTANT]  
> Se você selecionar a opção para impor o uso da Criptografia SMB, e a pasta raiz de hotfix não estiver configurada para conexões que usam criptografia SMB, a Execução de Atualização falhará.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Etapa 5. Habilitar uma regra de Firewall do Windows no servidor SMB
  
Você deve habilitar o **gerenciamento remoto do servidor de arquivos \(SMB\-na\)**  regra no Firewall do Windows no servidor de arquivos SMB. Isso é habilitado por padrão no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da atualização com suporte a cluster](cluster-aware-updating.md)
  
-   [Cluster-Aware atualização Cmdlets do Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Cluster-Aware referência do plug-in de atualização](https://msdn.microsoft.com/library/hh418084.aspx)  
  
