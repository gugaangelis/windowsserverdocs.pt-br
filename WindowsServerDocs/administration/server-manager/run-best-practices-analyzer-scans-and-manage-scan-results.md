---
title: Executar verificações de Analisador de Práticas Recomendadas e gerenciar Results_1 de verificação
description: Gerenciador do Servidor
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8c0440da49e6e78afece1af3ee8357ddf846e7e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895734"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados das varreduras

>Aplica-se a: Windows Server 2016

No gerenciamento do Windows, as *práticas recomendadas* são diretrizes consideradas a maneira ideal, em circunstâncias normais, de configurar um servidor conforme definido pelos especialistas. Por exemplo, é considerada uma prática recomendada pela maioria dos aplicativos de servidores manter aberta somente as portas necessárias para que os aplicativos se comuniquem com outros computadores da rede e bloqueiem as portas não usadas. Embora as violações de práticas recomendadas, mesmo das importantes, não sejam necessariamente problemáticas, elas indicam configurações de servidores que podem resultar em desempenho baixo, confiabilidade fraca, conflitos inesperados, aumento nos riscos de segurança e outros problemas potenciais.

O Analisador de Práticas Recomendadas (BPA) é uma ferramenta de gerenciamento de servidor que está disponível no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2008 R2. O BPA pode ajudar os administradores a reduzir as violações de práticas recomendadas, examinando as funções instaladas em servidores gerenciados que executam o Windows Server 2012 ou o Windows Server 2008 R2 e relatando as violações de práticas recomendadas ao administrador.

Você pode executar verificações de Analisador de Práticas Recomendadas (BPA) de Gerenciador do Servidor, usando a GUI do BPA ou usando cmdlets no Windows PowerShell. a partir do Windows Server 2012, você pode verificar uma função ou várias funções ao mesmo tempo, em vários servidores, seja usando o bloco Analisador de Práticas Recomendadas no console do Gerenciador do Servidor ou cmdlets do Windows PowerShell para executar verificações. Você também pode instruir o BPA a excluir ou ignorar os resultados da varredura que você não deseja visualizar.

Este tópico inclui as seções a seguir.

-   [Localizar BPA](#BKMK_find)

-   [Como o BPA funciona](#BKMK_how)

-   [Executando varreduras do Analisador de Práticas Recomendadas em funções](#BKMK_BPAscan)

-   [Gerenciar resultados da varredura](#BKMK_manage)

## <a name="find-bpa"></a><a name=BKMK_find></a>Localizar BPA
Você pode encontrar o bloco Analisador de Práticas Recomendadas em páginas de grupo de funções e de servidores do Gerenciador do Servidor no Windows Server 2012 R2 e no Windows Server 2012, ou pode abrir uma sessão do Windows PowerShell com direitos de usuário elevados para executar os cmdlets Analisador de Práticas Recomendadas.

## <a name="how-bpa-works"></a><a name=BKMK_how></a>Como o BPA funciona
O BPA funciona medindo a conformidade de uma função com as regras de práticas recomendadas em oito categorias diferentes de eficácia, confiabilidade e confiabilidade. Os resultados das medições podem ser qualquer um dos três níveis de gravidade descritos na tabela a seguir.

|Nível de severidade|Descrição|
|---------|--------|
|Erro|Os resultados do erro são retornados quando uma função não satisfaz as condições de uma regra de prática recomendada e problemas de funcionalidade podem ser esperados.|
|Informação|Os resultados de informações são retornados quando uma função satisfaz as condições de uma regra de prática recomendada.|
|Aviso|Os resultados de aviso são retornados quando os resultados de uma incompatibilidade podem causar problemas se as alterações não foram feitas. O aplicativo pode ser compatível com a operação atual, mas pode não satisfazer as condições de uma regra se não forem feitas alterações em sua configuração ou nas configurações da diretriz. Por exemplo, uma varredura dos Serviços de Área de Trabalho Remota pode mostrar um resultado de aviso se um servidor de licença estiver indisponível para a função, porque, mesmo se nenhuma conexão remota estiver ativa no momento da varredura, não ter o servidor de licença impede que novas conexões remotas obtenham licenças válidas de acesso ao cliente.|

### <a name="rule-categories"></a>Categorias de regras
A tabela a seguir descreve as categorias de regras de práticas recomendadas nas quais as funções são medidas durante uma verificação de Analisador de Práticas Recomendadas.

|Nome da categoria|Descrição|
|---------|--------|
|Segurança|As regras de segurança são aplicadas para medir o risco relativo de uma função para exposição a ameaças como usuários não autorizados ou mal-intencionados, ou perda ou roubo de dados confidenciais ou proprietários.|
|Desempenho|Regras de desempenho são aplicadas para medir a capacidade de uma função de processar solicitações e executar suas tarefas prescritas na empresa dentro dos períodos de tempo esperados, considerando a carga de trabalho da função.|
|Configuração|As regras de configuração são aplicadas para identificar as configurações de regras que podem precisar de modificações para a execução ideal da função. As regras de configuração podem ajudar a evitar conflitos nas configurações que possam resultar em mensagens de erro ou impedir que a função execute as obrigações prescritas em uma empresa.|
|Política|As regras de política são aplicadas para identificar Política de Grupo ou configurações de registro do Windows que podem exigir modificação para que uma função opere de forma ideal e segura.|
|Operação|As regras de operação são aplicadas para identificar possíveis falhas de uma função ao executar as tarefas prescritas em uma empresa.|
|Pré-implantação|As regras de pré-implantação são aplicadas antes que uma função instalada seja implantada na empresa. Elas permitem que os administradores avaliem se as práticas recomendadas foram executadas, antes da função ser usada na produção.|
|Pós-implantação|As regras de pós-implantação são aplicadas depois que todos os serviços exigidos foram iniciados para uma função e depois que a função é executada na empresa.|
|Pré-requisitos|As regras de pré-requisitos explicam os parâmetros de configurações, as configurações de políticas e os recursos necessários para uma função antes que o BPA possa aplicar regras específicas de outras categorias. Um pré-requisito nos resultados de varredura indica que uma configuração incorreta, um programa ausente, uma política desabilitada ou habilitada incorretamente, uma configuração de chave de registro ou outras configurações impediram que o BPA aplicasse uma ou mais regras durante a varredura. Um resultado de pré-requisito não indica compatibilidade ou incompatibilidade. Isso significa que não foi possível aplicar a regra e, assim, ela não faz parte dos resultados da varredura.|

## <a name="performing-best-practices-analyzer-scans-on-roles"></a><a name=BKMK_BPAscan></a>Executando varreduras do Analisador de Práticas Recomendadas em funções
Você pode executar verificações de BPA em funções usando a GUI do BPA no Gerenciador do Servidor ou usando cmdlets do Windows PowerShell.

No Windows Server 2012 R2 e no Windows Server 2012, algumas funções solicitam que você especifique parâmetros adicionais, como os nomes de servidores ou compartilhamentos específicos que estão executando partes da função, ou as IDs de submodelos, antes de iniciar uma verificação de BPA. Para as varreduras BPA em modelos que exigem que você especifique parâmetros adicionais, use os cmdlets do BPA; a GUI do BPA não pode aceitar parâmetros adicionais, como IDs de submodelo. Por exemplo, a ID do submodelo **FSRM** representa o submodelo BPA de Serviços de Arquivo no Gerenciador de Recursos de Servidor de Arquivos, um serviço de função dos Serviços de Arquivo e Armazenamento. Para executar uma verificação somente no serviço de função do Gerenciador de recursos do servidor de arquivos, execute uma verificação do BPA usando cmdlets do Windows PowerShell e adicione o parâmetro `SubmodelId` ao seu cmdlet.

Embora você não possa passar parâmetros adicionais para uma verificação iniciada na GUI do BPA, o bloco do BPA no Gerenciador do Servidor exibe os resultados para a varredura BPA mais recente, independentemente de como a verificação foi iniciada.

-   [Examinando funções usando a GUI do BPA](#BKMK_GUIscan)

-   [Examinando funções usando cmdlets do Windows PowerShell](#BKMK_PSscan)

### <a name="scanning-roles-by-using-the-bpa-gui"></a><a name=BKMK_GUIscan></a>Examinando funções usando a GUI do BPA
Execute as etapas a seguir para examinar uma ou mais funções na GUI do BPA.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Para examinar funções usando a GUI do BPA

1.  Siga um destes procedimentos para abrir Gerenciador do Servidor se ele ainda não estiver aberto.

    -   Na barra de tarefas do Windows, clique no botão Gerenciador do Servidor.

    -   Na tela **Iniciar** , clique no bloco Gerenciador do servidor.

2.  No painel de navegação, abra uma página de função ou grupo.

    Executar as varreduras BPA de uma página de função ou grupo verifica todas as funções que estão instaladas nos servidores desse grupo.

3.  No menu **tarefas** do bloco **analisador de práticas recomendadas** , clique em **Iniciar verificação do BPA**.

4.  Dependendo do número de regras que são avaliadas para a função ou o grupo selecionado, a varredura BPA pode levar alguns minutos para ser concluída.

### <a name="scanning-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_PSscan></a>Examinando funções usando cmdlets do Windows PowerShell
Use os procedimentos a seguir para verificar uma ou mais funções usando cmdlets do Windows PowerShell.

> [!NOTE]
> Os procedimentos nessa seção não mostram todos os parâmetros e cmdlets do BPA. Para obter mais informações sobre as operações do BPA no Windows PowerShell, em sua sessão do Windows PowerShell, digite **Get-Help***BPACmdlet***-Full**, em que *BPACmdlet* pode ser um dos valores a seguir. Você também pode encontrar tópicos de ajuda do cmdlet do BPA no [TechCenter do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="to-scan-a-single-role-by-using-windows-powershell-cmdlets"></a><a name=BKMK_singlerole></a>Para examinar uma única função usando cmdlets do Windows PowerShell

1.  Execute uma das ações a seguir para executar o Windows PowerShell com direitos de usuário elevados.

    -   Para executar o Windows PowerShell como administrador na tela **inicial** , clique com o botão direito do mouse no bloco do **Windows PowerShell** nos resultados dos **aplicativos** e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.

    -   Para executar o Windows PowerShell como administrador na área de trabalho, clique com o botão direito do mouse no atalho do **Windows PowerShell** na barra de tarefas e clique em **Executar como administrador**.

2.  a partir do Windows PowerShell 3,0, os módulos de cmdlet são automaticamente importados para sua sessão do Windows PowerShell na primeira vez em que um cmdlet do módulo é usado. Não é necessário importar ou carregar o módulo de cmdlet do BPA.

3.  Localize as IDs de modelo de todas as funções para as quais as verificações de BPA podem ser executadas inserindo o cmdlet **Get-BPAModel** , conforme mostrado no exemplo a seguir.

    `Get-Bpamodel`

4.  Nos resultados da etapa 3, localize as IDs do modelo das funções nas quais você deseja executar uma varredura BPA.

5.  Digite um dos comandos a seguir para iniciar a varredura BPA para uma função específica. Para múltiplas funções, separe as IDs de modelo com vírgulas.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Exemplo:** `Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > A ID do módulo inclui o caminho completo que aparece na coluna **Id**; por exemplo, **Microsoft/Windows/Hyper-V**.

    É possível também iniciar uma varredura em uma função específica dos resultados da etapa 3 redirecionando os resultados do cmdlet do `Get-BPAmodel` para o cmdlet do `Invoke-BPAmodel` conforme mostrado no exemplo a seguir.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    Executar esse cmdlet sem especificar uma ID de modelo canaliza todos os modelos que são retornados pelo `Get-BPAmodel` cmdlet para o `Invoke-BPAmodel` cmdlet, iniciando verificações em todos os modelos que estão disponíveis em servidores que foram adicionados ao pool de servidores do Gerenciador do servidor.

#### <a name="to-scan-all-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_allroles></a>Para examinar as funções usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados, se ainda não estiver aberta. Veja o procedimento anterior para obter instruções.

2.  Redirecione todas as funções para as quais as varreduras BPA podem ser executadas no cmdlet do `Invoke-BPAmodel` a fim de iniciar as varreduras.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Quando a verificação for concluída, o Windows PowerShell retornará resultados semelhantes ao seguinte, para cada função que foi verificada.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="manage-scan-results"></a><a name=BKMK_manage></a>Gerenciar resultados da varredura
Depois que uma varredura BPA é concluída na GUI, você pode exibir os resultados da varredura no bloco do BPA. Quando você seleciona um resultado no bloco, um painel de visualização no bloco exibe as propriedades do resultado, incluindo uma indicação de se a função está em conformidade com a prática recomendada associada. Se um resultado não estiver em conformidade e você quiser saber como resolver os problemas descritos nas propriedades do resultado, os hiperlinks nos tópicos de erro e de resultado de aviso abriram a resolução detalhada do tópico ajuda do Windows Server TechCenter.

> [!NOTE]
> Os resultados da varredura BPA não são salvos ou arquivados automaticamente. Executar uma nova varredura em um modelo ou submodelo substitui os resultados da última varredura. Para salvar os resultados da varredura BPA para arquivar, imprimir ou enviar a outras pessoas, consulte [Para exibir os resultados do BPA das sessões do Windows PowerShell em formatos diferentes](#BKMK_formats) nesta seção.

### <a name="exclude-and-include-bpa-results"></a>Excluir e incluir resultados do BPA
Se você não precisar ver alguns resultados do BPA, como os resultados que ocorrem com frequência em suas verificações de BPA, mas não exigir resolução, você pode excluir os resultados usando a GUI do BPA ou os cmdlets do BPA no Windows PowerShell. Os resultados podem ser incluídos novamente a qualquer momento.

> [!NOTE]
> Quando você exclui resultados, eles também são excluídos no modo de exibição nos servidores gerenciados. Outros administradores não podem visualizar os resultados excluídos nos servidores gerenciados. Para excluir os resultados da exibição em um console de Gerenciador do Servidor local somente, crie uma consulta personalizada em vez de usar o comando **excluir resultado** .

#### <a name="exclude-scan-results"></a><a name=BKMK_exclude></a>Excluir resultados da varredura
A configuração **Excluir** é persistente: os resultados que você excluir sempre serão excluídos das varreduras futuras do mesmo modelo no mesmo computador, a não ser que sejam novamente incluídos.

Você pode excluir resultados de varredura usando o cmdlet do `Set-BPAResult` com o parâmetro `Exclude` . Como no bloco Analisador de Práticas Recomendadas no Gerenciador do Servidor, você pode excluir objetos de resultado individuais ou também pode excluir um conjunto de resultados cujos campos (categoria, título e severidade, por exemplo) são iguais ou contêm valores especificados. Por exemplo, você pode excluir todos os resultados de **Desempenho** de um conjunto de resultados da varredura de um modelo.

> [!NOTE]
> Você deve executar pelo menos uma varredura BPA em um modelo antes de poder usar os procedimentos nesta seção.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Para excluir os resultados de varredura usando a GUI

1.  Abra uma página de grupo de função ou de servidor no Gerenciador do Servidor.

2.  No bloco Analisador de Práticas Recomendadas para a função ou grupo de servidores, clique com o botão direito do mouse em um resultado na lista e clique em **excluir resultado**.

    O resultado não é mais exibido na lista de resultados.

3.  Para exibir os resultados excluídos na GUI, execute a consulta interna **Resultados excluídos**. Clique em **Consultas de Pesquisa Salvas** e em **Resultados excluídos**.

    Após executar a consulta **Resultados excluídos**, observe que o texto do subtítulo do bloco, uma descrição dos resultados exibidos na lista, é alterado para **Resultados excluídos**. Somente os resultados excluídos são exibidos na lista.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Para excluir resultados de varredura usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Exclua resultados específicos de uma varredura de modelo executando o comando a seguir.

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq Value} | Set-BPAResult -Exclude $true`

    O comando anterior recupera os itens de resultado da verificação do BPA para a ID do modelo que é representada pela *ID do modelo*.

    A segunda seção do comando filtra os resultados do cmdlet `Get-BPAResult` para recuperar apenas os resultados da varredura em que o valor de um campo de resultado, representado pelo *Nome do Campo*, corresponde ao texto entre aspas.

    A seção final do comando, após o segundo caractere de barra vertical, exclui os resultados filtrados pela seção anterior do cmdlet.

    **Exemplo:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Incluir resultados da varredura
Quando desejar visualizar os resultados excluídos da varredura, você pode incluí-los nos resultados da varredura. A configuração **Incluir** é persistente: os resultados incluídos serão sempre incluídos nas varreduras futuras do mesmo modelo no mesmo computador.

##### <a name="to-include-scan-results-by-using-the-gui"></a><a name=BKMK_gui></a>Para incluir os resultados de varredura usando a GUI

1.  Abra uma página de grupo de função ou de servidor no Gerenciador do Servidor.

2.  No bloco Analisador de Práticas Recomendadas para a função ou grupo de servidores, clique com o botão direito do mouse em um resultado excluído na lista consulta de **resultados excluídos** e clique em **incluir resultado**.

    O resultado não é mais exibido na lista de resultados excluídos. Limpe a consulta clicando em **Limpar Tudo** para exibir o resultado incluído na lista de todos os resultados incluídos.

##### <a name="to-include-scan-results-by-using-windows-powershell-cmdlets"></a><a name=BKMK_cmdlets></a>Para incluir resultados de varredura usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Inclua resultados específicos de uma varredura de modelo digitando o seguinte comando e pressionando **Enter**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq Value } | Set-BPAResult -Exclude $false`

    O comando anterior recupera os itens de resultado da verificação do BPA para o modelo representado pela *ID do modelo*.

    A segunda parte do comando, após o primeiro caractere de pipe ( **|** ), filtra os resultados do cmdlet **Get-BPAResult** para recuperar apenas os resultados da verificação para os quais o valor do campo de resultado, representado pelo *nome do campo*, corresponde ao texto entre aspas.

    A parte final do comando, após o segundo caractere de barra vertical, inclui resultados filtrados pela segunda parte do cmdlet, ao configurar o valor do parâmetro **-Exclude** como **false**.

    **Exemplo:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Exibir e exportar os resultados da varredura BPA no Windows PowerShell
Para exibir e gerenciar os resultados da verificação usando cmdlets do Windows PowerShell, consulte os procedimentos a seguir. Antes que você possa usar qualquer um dos procedimentos a seguir, execute pelo menos uma varredura BPA em um modelo ou submodelo.

#### <a name="to-view-results-of-the-most-recent-scan-of-a-role-by-using-windows-powershell"></a><a name=BKMK_recentPS></a>Para exibir os resultados da varredura mais recente de uma função usando o Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Obtenha os resultados da varredura mais recente de uma identificação de modelo especificada. Digite o seguinte, no qual o modelo é representado por *ID de modelo*e pressione **Enter**. Você pode obter os resultados de várias identificações de modelo separando as identificações de modelo por vírgulas.

    `Get-BPAResult <model ID>`

    **Exemplo:** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Se você examinou um submodelo de um modelo, como um serviço de função, obtenha os resultados somente para esse submodelo, incluindo a ID do submodelo no cmdlet.

    **Exemplo:** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="to-view-or-save-bpa-results-from-windows-powershell-sessions-in-different-formats"></a><a name=BKMK_formats></a>Para exibir ou salvar os resultados do BPA das sessões do Windows PowerShell em formatos diferentes

-   No Windows PowerShell, cada resultado do BPA é semelhante ao seguinte.

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    Execute uma delas.

    -   Para formatar os resultados do BPA em uma tabela, execute o cmdlet a seguir, adicionando as propriedades de resultados que você deseja visualizar no exemplo anterior.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Exemplo:** `Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Para formatar os resultados do BPA em um visualizador de grade baseado em GUI, com um filtro de cadeia de caracteres de texto, e cabeçalhos de coluna que podem ser clicados para classificar os resultados, execute o cmdlet a seguir.

        `Get-BPAResult <model ID> | OGV`

    -   Para exportar resultados do BPA para um arquivo HTML que pode ser arquivado ou enviado para destinatários de email, execute o seguinte cmdlet, em que *path* representa o caminho e o nome do arquivo para o qual você deseja salvar os resultados HTML.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Exemplo:** `Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Para exportar resultados do BPA para um arquivo de texto CSV (valores separados por vírgula), execute o seguinte cmdlet, em que *caminho* representa o caminho e o nome do arquivo de texto no qual você deseja salvar os resultados do CSV. Os resultados de CSV podem ser importados para o Microsoft Excel ou outros programas que exibem dados em planilhas ou grades.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Exemplo:** `Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Consulte Também
[Analisador de práticas recomendadas o conteúdo de resolução no TechCenter](https://go.microsoft.com/fwlink/p/?LinkId=241597) 
 do Windows Server [Filtrar, classificar e consultar dados em blocos](filter-sort-and-query-data-in-server-manager-tiles.md) 
 de Gerenciador do servidor [Gerenciar vários servidores remotos com Gerenciador do servidor](manage-multiple-remote-servers-with-server-manager.md)
