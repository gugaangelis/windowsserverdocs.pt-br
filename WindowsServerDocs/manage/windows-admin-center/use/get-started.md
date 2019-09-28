---
title: Introdução ao centro de administração do Windows
description: Introdução ao centro de administração do Windows
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: 68b5c7b2c5bc8e93d653514b2664d96b97b07a9e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406841"
---
# <a name="get-started-with-windows-admin-center"></a>Introdução ao centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centro de administração do Windows instalado no Windows 10

> [!IMPORTANT]
> Você deve ser membro do grupo do administrador local para usar o centro de administração do Windows no Windows 10

### <a name="selecting-a-client-certificate"></a>Selecionando um certificado de cliente

Na primeira vez que você abrir o centro de administração do Windows no Windows 10, certifique-se de selecionar o certificado do *cliente do centro de administração do Windows* (caso contrário, você receberá um erro HTTP 403 dizendo "não é possível chegar a esta página").

No Microsoft Edge, quando você receber uma solicitação com esta caixa de diálogo:
 
1. Clique em **mais opções**

    ![](../media/launch-cert-1.png)

2. Selecione o certificado rotulado **cliente do centro de administração do Windows** e clique em **OK**

    ![](../media/launch-cert-2.png)

3. Certifique-se de **que sempre permitir acesso** esteja selecionado e clique em **permitir**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectando-se a nós e clusters gerenciados

Depois de concluir a instalação do centro de administração do Windows, você pode adicionar servidores ou clusters a serem gerenciados na página Visão geral principal.

 **Adicionar um único servidor ou um cluster como um nó gerenciado**

1. Clique em **+ Adicionar** em **todas as conexões**.

   ![](../media/launch/addserver0.png)

2. Escolha Adicionar um servidor, cluster de failover ou conexão de cluster hiperconvergente:
    
   ![](../media/launch/addserver1.png)

3. Digite o nome do servidor ou cluster a ser gerenciado e clique em **Enviar**. O servidor ou cluster será adicionado à sua lista de conexões na página Visão geral.

   ![](../media/launch/addserver2.png)

   **--OU--**

**Importação em massa de vários servidores**

 1. Na página **Adicionar conexão do servidor** , escolha a guia **importar servidores** .

    ![](../media/launch/import-servers.png)

 2. Clique em **procurar** e selecione um arquivo de texto que contenha uma vírgula ou uma nova linha separada por uma lista de FQDNs para os servidores que você deseja adicionar.

> [!Note]
> O arquivo. csv criado exportando [suas conexões com o PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contém informações adicionais além dos nomes de servidor e não é compatível com esse método de importação.

  **--OU--**

**Adicionar servidores pesquisando Active Directory**

 1. Na página **Adicionar conexão do servidor** , escolha a guia **Pesquisar Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Insira seus critérios de pesquisa e clique em **Pesquisar**. Há suporte para curingas (*).

 3. Após a conclusão da pesquisa-selecione um ou mais dos resultados, opcionalmente, adicione marcas e clique em **Adicionar**.

## <a name="authenticate-with-the-managed-node"></a>Autenticar com o nó gerenciado ##

O centro de administração do Windows dá suporte a vários mecanismos de autenticação com um nó gerenciado. O logon único é o padrão.

**Logon único**

Você pode usar suas credenciais atuais do Windows para autenticar com o nó gerenciado. Esse é o padrão, e o centro de administração do Windows tenta o logon quando você adiciona um servidor. 

**Logon único quando implantado como um serviço no Windows Server**

Se você tiver instalado o centro de administração do Windows no Windows Server, a configuração adicional será necessária para o logon único.  [Configurar seu ambiente para delegação](../configure/user-access-control.md)

**--OU--**

**Usar *gerenciar como* para especificar credenciais**

Em **todas as conexões**, selecione um servidor na lista e escolha **gerenciar como** para especificar as credenciais que serão usadas para autenticar o nó gerenciado:

![](../media/launch-use-6.png)

Se o centro de administração do Windows estiver sendo executado no modo de serviço no Windows Server, mas você não tiver a delegação Kerberos configurada, deverá inserir novamente suas credenciais do Windows:

![](../media/launch-use-7.png)

Você pode aplicar as credenciais a todas as conexões, que as armazenará em cache para essa sessão específica do navegador. Se você recarregar o navegador, deverá inserir novamente as credenciais de **gerenciar como** .

**Solução de senha de administrador local (LAPSos)**

Se seu ambiente usa [lapsos](https://technet.microsoft.com/mt227395.aspx)e você tem o centro de administração do Windows instalado em seu PC com Windows 10, você pode usar as credenciais de lapso para autenticar com o nó gerenciado. **Se você usar esse cenário,** [forneça comentários](http://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Usando marcas para organizar suas conexões

Você pode usar marcas para identificar e filtrar servidores relacionados na sua lista de conexões.  Isso permite que você veja um subconjunto dos servidores na lista de conexões.  Isso é especialmente útil se você tiver muitas conexões.

### <a name="edit-tags"></a>Editar marcas

* Selecione um servidor ou vários servidores na lista todas as conexões
* Em **todas as conexões**, clique em **Editar marcas**

![](../media/launch/tags-5.png)

O painel **Editar marcas de conexão** permite que você modifique, adicione ou remova marcas de suas conexões selecionadas:

* Para adicionar uma nova marca às conexões selecionadas, selecione **adicionar marca** e insira o nome da marca que você deseja usar.

* Para marcar as conexões selecionadas com um nome de marca existente, marque a caixa ao lado do nome da marca que você deseja aplicar.

* Para remover uma marca de todas as conexões selecionadas, desmarque a caixa ao lado da marca que você deseja remover.

* Se uma marca for aplicada a um subconjunto das conexões selecionadas, a caixa de seleção será mostrada em um estado intermediário. Você pode clicar na caixa para verificá-la e aplicar a marca a todas as conexões selecionadas ou clicar novamente para desmarcar e remover a marca de todas as conexões selecionadas.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrar conexões por marca

Depois que as marcas tiverem sido adicionadas a uma ou mais conexões de servidor, você poderá exibir as marcas na lista de conexões e filtrar a lista de conexões por marcas.

* Para filtrar por uma marca, selecione o ícone de filtro ao lado da caixa de pesquisa.
![](../media/launch/tags-7.png)
* Você pode selecionar "or", "and" ou "Not" para modificar o comportamento do filtro das marcas selecionadas.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar o PowerShell para importar ou exportar suas conexões (com marcas)

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>Formato de arquivo CSV para importar conexões

O formato do arquivo CSV começa com os quatro cabeçalhos ```"name","type","tags","groupId"```, seguidos por cada conexão em uma nova linha.

**nome** é o FQDN da conexão

**tipo** é o tipo de conexão. Para as conexões padrão incluídas no centro de administração do Windows, você usará um dos seguintes:

| Tipo de conexão | Cadeia de conexão |
|------|-------------------------------|
| Windows Server | MSFT. SME. Connection-Type. Server |
| PC com Windows 10 | MSFT. SME. Connection-Type. Windows-Client |
| Cluster de failover | MSFT. SME. Connection-Type. cluster |
| Cluster hiperconvergente | MSFT. SME. Connection-Type. hiperconvergente-cluster |

as **marcas** são separadas por pipe.

**GroupId** é usado para conexões compartilhadas. Use o valor ```global``` desta coluna para fazer com que esta seja uma conexão compartilhada.

### <a name="example-csv-file-for-importing-connections"></a>Exemplo de arquivo CSV para importar conexões

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importar conexões RDCman

Use o script abaixo para exportar conexões salvas no [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) para um arquivo. Em seguida, você pode importar o arquivo para o centro de administração do Windows, mantendo sua hierarquia de agrupamento RDCMan usando marcas. Experimente!

1. Copie e cole o código abaixo em sua sessão do PowerShell:

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. Para criar um. Arquivo CSV, execute o seguinte comando:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importe o resultado. Arquivo CSV no centro de administração do Windows e todas as suas hierarquias de agrupamento RDCMan serão representadas por marcas na lista de conexões. Para obter detalhes, consulte [usar o PowerShell para importar ou exportar suas conexões (com marcas)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Exibir scripts do PowerShell usados no centro de administração do Windows

Depois de se conectar a um servidor, cluster ou PC, você pode examinar os scripts do PowerShell que capacitam as ações da interface do usuário no centro de administração do Windows. De dentro de uma ferramenta, clique no ícone do PowerShell na barra de aplicativos superior. Selecione um comando de interesse na lista suspensa para navegar até o script do PowerShell correspondente.

![](../media/launch/showscript.png)