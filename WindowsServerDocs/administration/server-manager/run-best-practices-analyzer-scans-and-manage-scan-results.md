---
title: Executar varreduras do analisador de práticas recomendadas e gerenciar Results_1 de verificação
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58fdf35e1d06fe7176b122155b2768094f2b01b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838217"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados das varreduras

>Aplica-se a: Windows Server 2016

No gerenciamento do Windows, as *práticas recomendadas* são diretrizes consideradas a maneira ideal, em circunstâncias normais, de configurar um servidor conforme definido pelos especialistas. Por exemplo, é considerada uma prática recomendada pela maioria dos aplicativos de servidores manter aberta somente as portas necessárias para que os aplicativos se comuniquem com outros computadores da rede e bloqueiem as portas não usadas. Embora as violações de práticas recomendadas, mesmo das importantes, não sejam necessariamente problemáticas, elas indicam configurações de servidores que podem resultar em desempenho baixo, confiabilidade fraca, conflitos inesperados, aumento nos riscos de segurança e outros problemas potenciais.

Analisador de práticas recomendadas (BPA) é uma ferramenta de gerenciamento de servidor que está disponível no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2. O BPA pode ajudar os administradores a reduzir as violações às práticas varredura das funções que são instaladas em servidores gerenciados que executam o Windows Server 2012 ou Windows Server 2008 R2 e relatórios dessas violações ao administrador.

Você pode executar varreduras do analisador de práticas recomendadas (BPA) o Gerenciador de servidores, usando a GUI do BPA ou usando cmdlets do Windows PowerShell. começando com o Windows Server 2012, você pode examinar uma ou várias funções ao mesmo tempo, em vários servidores, se você usar o bloco do analisador de práticas recomendadas no console do Gerenciador do servidor ou cmdlets do Windows PowerShell para executar verificações. Você também pode instruir o BPA a excluir ou ignorar os resultados da varredura que você não deseja visualizar.

Este tópico contém as seguintes seções.

-   [encontrar o BPA](#BKMK_find)

-   [Como o BPA funciona](#BKMK_how)

-   [Varreduras do analisador de práticas recomendadas de execução em funções](#BKMK_BPAscan)

-   [Gerenciar os resultados da varredura](#BKMK_manage)

## <a name="BKMK_find"></a>encontrar o BPA
Você pode encontrar no bloco do analisador de práticas recomendadas na função e páginas de grupo do servidor do Gerenciador do servidor no Windows Server 2012 R2 e Windows Server 2012 ou você podem abrir uma sessão do Windows PowerShell com direitos de usuário com privilégios elevados para executar os cmdlets do analisador de práticas recomendadas.

## <a name="BKMK_how"></a>Como o BPA funciona
O BPA funciona medindo a conformidade da função com as regras de práticas recomendadas em oito categorias diferentes de efetividade e confiabilidade. Os resultados das medições podem ser qualquer um dos três níveis de gravidade descritos na tabela a seguir.

|Nível de gravidade|Descrição|
|---------|--------|
|Erro|Os resultados do erro são retornados quando uma função não satisfaz as condições de uma regra de prática recomendada e problemas de funcionalidade podem ser esperados.|
|Informações do|Os resultados de informações são retornados quando uma função satisfaz as condições de uma regra de prática recomendada.|
|Aviso|Os resultados de aviso são retornados quando os resultados de uma incompatibilidade podem causar problemas se as alterações não foram feitas. O aplicativo pode ser compatível com a operação atual, mas pode não satisfazer as condições de uma regra se não forem feitas alterações em sua configuração ou nas configurações da diretriz. Por exemplo, uma varredura dos Serviços de Área de Trabalho Remota pode mostrar um resultado de aviso se um servidor de licença estiver indisponível para a função, porque, mesmo se nenhuma conexão remota estiver ativa no momento da varredura, não ter o servidor de licença impede que novas conexões remotas obtenham licenças válidas de acesso ao cliente.|

### <a name="rule-categories"></a>Categorias de regras
A tabela a seguir descreve as categorias de regras de práticas recomendadas em relação a quais funções são medidas durante um analisador de práticas recomendadas de verificação.

|Nome da categoria|Descrição|
|---------|--------|
|Segurança|Regras de segurança são aplicadas para medir o risco de exposição a ameaças como usuários não autorizados ou mal-intencionados, ou perda ou roubo de dados confidenciais ou proprietários relativo da função.|
|Desempenho|Regras de desempenho são aplicadas para medir a capacidade da função para processar solicitações e executar as obrigações prescritas em uma empresa dentro do período de tempo considerando a carga de trabalho da função esperado.|
|Configuração|As regras de configuração são aplicadas para identificar as configurações de regras que podem precisar de modificações para a execução ideal da função. As regras de configuração podem ajudar a evitar conflitos nas configurações que possam resultar em mensagens de erro ou impedir que a função execute as obrigações prescritas em uma empresa.|
|Política|As regras de política são aplicadas para identificar as configurações de registro de diretiva de grupo ou do Windows que podem requerer modificação para uma função de forma segura e ideal.|
|Operação|As regras de operação são aplicadas para identificar possíveis falhas de uma função ao executar as tarefas prescritas em uma empresa.|
|Pré-implantação|As regras de pré-implantação são aplicadas antes que uma função instalada seja implantada na empresa. Elas permitem que os administradores avaliem se as práticas recomendadas foram executadas, antes da função ser usada na produção.|
|Pós-implantação|As regras de pós-implantação são aplicadas depois que todos os serviços exigidos foram iniciados para uma função e depois que a função é executada na empresa.|
|Pré-requisitos|As regras de pré-requisitos explicam os parâmetros de configurações, as configurações de políticas e os recursos necessários para uma função antes que o BPA possa aplicar regras específicas de outras categorias. Um pré-requisito nos resultados de varredura indica que uma configuração incorreta, um programa ausente, uma política desabilitada ou habilitada incorretamente, uma configuração de chave de registro ou outras configurações impediram que o BPA aplicasse uma ou mais regras durante a varredura. Um resultado de pré-requisito não implica em compatibilidade ou incompatibilidade. Isso significa que não foi possível aplicar a regra e, assim, ela não faz parte dos resultados da varredura.|

## <a name="BKMK_BPAscan"></a>Varreduras do analisador de práticas recomendadas de execução em funções
Você pode executar varreduras BPA em funções usando a GUI do BPA no Gerenciador do servidor, ou usando cmdlets do Windows PowerShell.

No Windows Server 2012 R2 e Windows Server 2012, algumas funções requerem que você especifique parâmetros adicionais, como os nomes de servidores específicos ou compartilhamentos que executam partes da função ou as IDs dos submodelos, antes de iniciar uma varredura BPA. Para as varreduras BPA em modelos que exigem que você especifique parâmetros adicionais, use os cmdlets do BPA; a GUI do BPA não pode aceitar parâmetros adicionais, como IDs de submodelo. Por exemplo, a ID do submodelo **FSRM** representa o submodelo BPA de Serviços de Arquivo no Gerenciador de Recursos de Servidor de Arquivos, um serviço de função dos Serviços de Arquivo e Armazenamento. Para executar uma verificação somente no serviço de função de Gerenciador de recursos de servidor de arquivos, executar uma varredura BPA usando cmdlets do Windows PowerShell e adicione o parâmetro `SubmodelId` ao cmdlet.

Embora você não pode passar parâmetros adicionais para uma varredura iniciada na GUI do BPA, o bloco do BPA no Gerenciador de servidores exibe os resultados da varredura BPA mais recente, independentemente de como a varredura foi iniciada.

-   [Examinando funções usando a GUI do BPA](#BKMK_GUIscan)

-   [Examinando funções usando cmdlets do Windows PowerShell](#BKMK_PSscan)

### <a name="BKMK_GUIscan"></a>Examinando funções usando a GUI do BPA
Execute as etapas a seguir para examinar uma ou mais funções na GUI do BPA.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Para examinar funções usando a GUI do BPA

1.  Siga um destes procedimentos para abrir o Gerenciador do servidor se ele não ainda estiver aberto.

    -   Na barra de tarefas do Windows, clique no botão de Gerenciador do servidor.

    -   Sobre o **iniciar** tela, clique no bloco do Gerenciador do servidor.

2.  No painel de navegação, abra uma página de função ou grupo.

    Executar as varreduras BPA de uma página de função ou grupo verifica todas as funções que estão instaladas nos servidores desse grupo.

3.  Sobre o **tarefas** menu da **analisador de práticas recomendadas** lado a lado, clique em **Iniciar varredura BPA**.

4.  Dependendo do número de regras que são avaliadas para a função ou o grupo selecionado, a varredura BPA pode levar alguns minutos para ser concluída.

### <a name="BKMK_PSscan"></a>Examinando funções usando cmdlets do Windows PowerShell
Use os procedimentos a seguir para examinar uma ou mais funções usando cmdlets do Windows PowerShell.

> [!NOTE]
> Os procedimentos nessa seção não mostram todos os parâmetros e cmdlets do BPA. Para obter mais informações sobre as operações do BPA no Windows PowerShell, na sua sessão do Windows PowerShell, digite **Get-help***BPACmdlet***-completo**, onde *BPACmdlet* pode ser um dos valores a seguir. Você também pode encontrar tópicos de ajuda de cmdlet do BPA na [TechCenter do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="BKMK_singlerole"></a>Para examinar uma única função usando cmdlets do Windows PowerShell

1.  Faça o seguinte para executar o Windows PowerShell com direitos de usuário elevados.

    -   Para executar o Windows PowerShell como administrador a partir de **iniciar** tela, clique com botão direito a **Windows PowerShell** lado a lado no **aplicativos** resultados e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.

    -   Para executar o Windows PowerShell como administrador na área de trabalho, clique com botão direito do **Windows PowerShell** atalho na barra de tarefas e clique **executar como administrador**.

2.  começando no Windows PowerShell 3.0, módulos de cmdlet são automaticamente importados para a sua sessão do Windows PowerShell na primeira vez que um cmdlet do módulo é usado. Não é necessário importar ou carregar o módulo de cmdlet do BPA.

3.  Encontre as IDs do modelo de todas as funções para qual varreduras BPA podem ser executadas ao inserir o **Get-Bpamodel** cmdlet, como mostrado no exemplo a seguir.

    `Get-Bpamodel`

4.  Nos resultados da etapa 3, localize as IDs do modelo das funções nas quais você deseja executar uma varredura BPA.

5.  Digite um dos comandos a seguir para iniciar a varredura BPA para uma função específica. Para múltiplas funções, separe as IDs de modelo com vírgulas.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Exemplo:**`Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > A ID do módulo inclui o caminho completo que aparece na coluna **Id**; por exemplo, **Microsoft/Windows/Hyper-V**.

    É possível também iniciar uma varredura em uma função específica dos resultados da etapa 3 redirecionando os resultados do cmdlet do `Get-BPAmodel` para o cmdlet do `Invoke-BPAmodel` conforme mostrado no exemplo a seguir.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    Execução desse cmdlet sem especificar uma ID de modelo redireciona todos os modelos que são retornados pelo `Get-BPAmodel` cmdlet para o `Invoke-BPAmodel` cmdlet, iniciando as varreduras em todos os modelos que estão disponíveis nos servidores que foram adicionados ao pool de servidores do Gerenciador do servidor.

#### <a name="BKMK_allroles"></a>Para examinar as funções usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário com privilégios elevados, se ele ainda não está aberto. Veja o procedimento anterior para obter instruções.

2.  Redirecione todas as funções para as quais as varreduras BPA podem ser executadas no cmdlet do `Invoke-BPAmodel` a fim de iniciar as varreduras.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Quando a verificação for concluída, o Windows PowerShell retorna resultados semelhantes à seguinte, para cada função que foi examinado.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="BKMK_manage"></a>Gerenciar os resultados da varredura
Depois que uma varredura BPA é concluída na GUI, você pode exibir os resultados da varredura no bloco do BPA. Quando você seleciona um resultado no bloco, um painel de visualização no bloco exibe as propriedades do resultado, incluindo uma indicação de se a função está em conformidade com a prática recomendada associada. Se um resultado não for compatível e você quiser saber como resolver os problemas descritos nas propriedades de resultados, hiperlinks nas propriedades de resultados de aviso e erro abrem tópicos de ajuda de resolução detalhados no Windows Server TechCenter.

> [!NOTE]
> Os resultados da varredura BPA não são salvos ou arquivados automaticamente. Executar uma nova varredura em um modelo ou submodelo substitui os resultados da última varredura. Para salvar os resultados da varredura BPA para arquivar, imprimir ou enviar a outras pessoas, consulte [Para exibir os resultados do BPA das sessões do Windows PowerShell em formatos diferentes](#BKMK_formats) nesta seção.

### <a name="exclude-and-include-bpa-results"></a>Excluir e incluir resultados do BPA
Se você não precisa ver alguns resultados do BPA, como os que ocorrem com frequência nas varreduras BPA, mas não requerem nenhuma solução, você pode excluir os resultados usando a GUI do BPA ou os cmdlets do BPA no Windows PowerShell. Os resultados podem ser incluídos novamente a qualquer momento.

> [!NOTE]
> Quando você exclui resultados, eles também são excluídos no modo de exibição nos servidores gerenciados. Outros administradores não podem visualizar os resultados excluídos nos servidores gerenciados. Para excluir resultados do modo de exibição em um console do Gerenciador do servidor local apenas, crie uma consulta personalizada em vez de usar o **Excluir resultado** comando.

#### <a name="BKMK_exclude"></a>Excluir resultados de varredura
A configuração **Excluir** é persistente: os resultados que você excluir sempre serão excluídos das varreduras futuras do mesmo modelo no mesmo computador, a não ser que sejam novamente incluídos.

Você pode excluir resultados de varredura usando o cmdlet do `Set-BPAResult` com o parâmetro `Exclude` . Como no bloco do analisador de práticas recomendadas no Gerenciador do servidor, você pode excluir objetos de resultado individual ou, você também pode excluir um conjunto de resultados daqueles campos (categoria, título e gravidade, por exemplo) são iguais a ou contenham valores especificados. Por exemplo, você pode excluir todos os resultados de **Desempenho** de um conjunto de resultados da varredura de um modelo.

> [!NOTE]
> Você deve executar pelo menos uma varredura BPA em um modelo antes de poder usar os procedimentos nesta seção.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Para excluir os resultados de varredura usando a GUI

1.  Abra uma página de grupo de funções ou servidores no Gerenciador do servidor.

2.  No bloco do analisador de práticas recomendadas para o grupo de funções ou servidores, clique com botão direito um resultado na lista e, em seguida, clique em **Excluir resultado**.

    O resultado não é mais exibido na lista de resultados.

3.  Para exibir os resultados excluídos na GUI, execute a consulta interna **Resultados excluídos** . Clique em **Consultas de Pesquisa Salvas**e em **Resultados excluídos**.

    Após executar a consulta **Resultados excluídos**, observe que o texto do subtítulo do bloco, uma descrição dos resultados exibidos na lista, é alterado para **Resultados excluídos**. Somente os resultados excluídos são exibidos na lista.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Para excluir resultados de varredura usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Exclua resultados específicos de uma varredura de modelo executando o comando a seguir.

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq "Value"} | Set-BPAResult -Exclude $true`

    O comando anterior recupera itens de resultado da varredura BPA referentes à ID de modelo é representada por *ID do modelo*.

    A segunda seção do comando filtra os resultados do cmdlet `Get-BPAResult` para recuperar apenas os resultados da varredura em que o valor de um campo de resultado, representado pelo *Nome do Campo*, corresponde ao texto entre aspas.

    A seção final do comando, após o segundo caractere de barra vertical, exclui os resultados filtrados pela seção anterior do cmdlet.

    **Exemplo:**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Incluir resultados da varredura
Quando desejar visualizar os resultados excluídos da varredura, você pode incluí-los nos resultados da varredura. A configuração **Incluir** é persistente: os resultados incluídos serão sempre incluídos nas varreduras futuras do mesmo modelo no mesmo computador.

##### <a name="BKMK_gui"></a>Para incluir resultados de varredura usando a GUI

1.  Abra uma página de grupo de funções ou servidores no Gerenciador do servidor.

2.  No bloco do analisador de práticas recomendadas para o grupo de funções ou servidores, clique com botão direito um resultado excluído na **resultados excluídos** lista de consulta e, em seguida, clique em **incluir resultado**.

    O resultado não é mais exibido na lista de resultados excluídos. Limpe a consulta clicando em **Limpar Tudo** para exibir o resultado incluído na lista de todos os resultados incluídos.

##### <a name="BKMK_cmdlets"></a>Para incluir resultados de varredura usando cmdlets do Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Inclua resultados específicos de uma varredura de modelo digitando o seguinte comando e pressionando **Enter**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq "Value" } | Set-BPAResult -Exclude $false`

    O comando anterior recupera itens de resultado da varredura BPA referentes ao modelo representado por *Id do modelo*.

    A segunda parte do comando, após o primeiro caractere de pipe ( **|** ) filtra os resultados do **Get-BPAResult** cmdlet para recuperar apenas os resultados de varredura para o qual o valor do resultado campo, representado por *nome do campo*, corresponde ao texto entre aspas.

    A parte final do comando, após o segundo caractere de barra vertical, inclui resultados filtrados pela segunda parte do cmdlet, ao configurar o valor do parâmetro **-Exclude** como **false**.

    **Exemplo:**`Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq "Information"} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Exibir e exportar os resultados da varredura BPA no Windows PowerShell
Para exibir e gerenciar resultados de varredura usando cmdlets do Windows PowerShell, consulte os procedimentos a seguir. Antes que você possa usar qualquer um dos procedimentos a seguir, execute pelo menos uma varredura BPA em um modelo ou submodelo.

#### <a name="BKMK_recentPS"></a>Para exibir os resultados da varredura mais recente de uma função usando o Windows PowerShell

1.  Abra uma sessão do Windows PowerShell com direitos de usuário elevados.

2.  Obtenha os resultados da varredura mais recente de uma identificação de modelo especificada. Digite o seguinte, em que o modelo é representado por *ID do modelo*, em seguida, pressione **Enter**. Você pode obter os resultados de várias identificações de modelo separando as identificações de modelo por vírgulas.

    `Get-BPAResult <model ID>`

    **Exemplo:** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Se você tiver examinado um submodelo de um modelo, como um serviço de função, obtenha os resultados para somente esse submodelo incluindo a ID do submodelo no cmdlet.

    **Exemplo:** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="BKMK_formats"></a>Para exibir ou salvar os resultados do BPA das sessões do Windows PowerShell em formatos diferentes

-   No Windows PowerShell, cada resultado do BPA é semelhante à seguinte.

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

    Siga um destes procedimentos.

    -   Para formatar os resultados do BPA em uma tabela, execute o cmdlet a seguir, adicionando as propriedades de resultados que você deseja visualizar no exemplo anterior.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Exemplo:**`Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Para formatar os resultados do BPA em um visualizador de grade baseado em GUI, com um filtro de cadeia de caracteres de texto, e cabeçalhos de coluna que podem ser clicados para classificar os resultados, execute o cmdlet a seguir.

        `Get-BPAResult <model ID> | OGV`

    -   Para exportar os resultados do BPA para um arquivo HTML que pode ser arquivado ou enviado por email aos destinatários, execute o seguinte cmdlet, onde *caminho* representa o nome de arquivo e caminho para o qual você deseja salvar os resultados em HTML.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Exemplo:**`Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Para exportar os resultados do BPA para um arquivo de texto de valores separados por vírgulas (CSV), execute o seguinte cmdlet, onde *caminho* representa o nome de arquivo de texto e o caminho ao qual você deseja salvar os resultados em CSV. Resultados em CSV podem ser importados para o Microsoft Excel ou outros programas que exibem dados em planilhas ou grades.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Exemplo:**`Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Consulte também
[Conteúdo de resolução do analisador de práticas recomendadas no Windows Server TechCenter recomendadas](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[filtrar, classificar e consulta dados em blocos do Gerenciador do servidor](filter-sort-and-query-data-in-server-manager-tiles.md)
[Manage Multiple, remote Servers com o Gerenciador do servidor](manage-multiple-remote-servers-with-server-manager.md)
