---
title: Introdução ao Windows Admin Center
description: Introdução ao Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: 61fdd70e53a49b704e11f71f0e5eb3176c31c378
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876007"
---
# <a name="get-started-with-windows-admin-center"></a>Introdução ao Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows Admin Center instalado no Windows 10

> [!IMPORTANT]
> Você deve ser um membro do grupo de administradores locais para usar o Windows Admin Center no Windows 10

### <a name="selecting-a-client-certificate"></a>Selecionar um certificado de cliente

Na primeira vez que você abrir o Windows Admin Center no Windows 10, certifique-se de selecionar o *Windows Admin Center cliente* certificado (caso contrário, você receberá um erro HTTP 403 dizendo "não é possível obter a esta página").

No Microsoft Edge, quando for solicitado com essa caixa de diálogo:
 
1. Clique em **mais opções**

    ![](../media/launch-cert-1.png)

2. Selecione o certificado rotulado **cliente do Windows Admin Center** e clique em **Okey**

    ![](../media/launch-cert-2.png)

3. Certifique-se **sempre permitir acesso** está selecionado e clique em **permitir**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Conectar-se a nós gerenciados e clusters

Depois de concluir a instalação do Windows Admin Center, você pode adicionar servidores ou clusters para gerenciar a partir da página Visão geral de principal.

 **Adicionar um único servidor ou um cluster como um nó gerenciado**

 1. Clique em **+ adicionar** sob **todas as conexões**.

    ![](../media/launch/addserver0.png)

 2. Escolha esta opção para adicionar uma conexão de servidor, Cluster de Failover ou Hyper-Converged Cluster:
    
    ![](../media/launch/addserver1.png)

 3. Digite o nome do servidor ou cluster para gerenciar e clique em **enviar**. O servidor ou cluster será adicionado à sua lista de conexão na página de visão geral.

    ![](../media/launch/addserver2.png)

   **-OU-**  

**Vários servidores de importação em massa**

 1. Sobre o **Adicionar Conexão de servidor** , escolha o **importar servidores** guia.

    ![](../media/launch/import-servers.png)

 2. Clique em **procurar** e selecione um arquivo de texto que contém uma vírgula ou nova linha, lista separada de FQDNs para os servidores que você deseja adicionar.

## <a name="authenticate-with-the-managed-node"></a>Autenticar com o nó gerenciado ##

Windows Admin Center oferece suporte a vários mecanismos para autenticação com um nó gerenciado. O logon único é o padrão.

**O logon único**

Você pode usar suas credenciais atuais do Windows para autenticar com o nó gerenciado. Esse é o padrão e o Windows Admin Center tentativas de logon quando você adiciona um servidor. 

**O logon único quando implantado como um serviço no Windows Server**

Se você tiver instalado o Windows Admin Center no Windows Server, a configuração adicional é necessária para o logon único.  [Configurar seu ambiente para delegação](..\configure\user-access-control.md)

**-OU-**

**Use *gerenciar como* para especificar credenciais**

Sob **todas as conexões**, selecione um servidor na lista e escolha **gerenciar como** para especificar as credenciais que você usará para autenticar para o nó gerenciado:

![](../media/launch-use-6.png)

Se Windows Admin Center está em execução no modo de serviço no Windows Server, mas você não tiver configurada a delegação de Kerberos, você deve inserir novamente suas credenciais do Windows:

![](../media/launch-use-7.png)

Você pode aplicar as credenciais para todas as conexões, que serão o cache-los para a sessão de navegador específico. Se você recarregar seu navegador, você deve inserir novamente sua **gerenciar como** credenciais.

**Solução de senha de administrador local LAPS)**

Se seu ambiente usa [LAPS](https://technet.microsoft.com/mt227395.aspx), você pode usar credenciais LAPS para autenticar com o nó gerenciado. **Se você usar esse cenário, por favor** [fornecer comentários](http://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Usando marcas para organizar suas conexões

Você pode usar marcas para identificar e filtrar servidores relacionados na sua lista de conexão.  Isso permite que você veja um subconjunto de seus servidores na lista de conexão.  Isso é especialmente útil se você tiver várias conexões.

### <a name="edit-tags"></a>Editar marcas

* Selecione um servidor ou vários servidores na lista de todas as conexões
* Sob **todas as conexões**, clique em **editar marcas**

![](../media/launch/tags-5.png)

O **Editar Conexão marcas** painel permite que você modificar, adicionar ou remover marcas de suas conexões selecionadas:

* Para adicionar uma nova marca para suas conexões selecionadas, selecione **Adicionar marca** e insira o nome de marca que você deseja usar.

* Para marcar as conexões selecionadas com um nome de marca existente, marque a caixa ao lado do nome de marca que você deseja aplicar.

* Para remover uma marca de selecionado todas as conexões, desmarque a caixa ao lado da marca que você deseja remover.

* Se uma marca for aplicada a um subconjunto das conexões selecionadas, a caixa de seleção é mostrada em um estado intermediário. Você pode clicar na caixa para verificá-lo e aplicar a marca para todas as conexões selecionadas ou clique novamente para desmarcá-la e remova a marcação de todas as conexões selecionadas.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Conexões de filtro por marca

Depois que as marcas foram adicionadas a um ou mais conexões de servidor, você pode exibir as marcas na lista de conexão e filtrar a lista de conexão por marcas.

* Para filtrar por uma marca, selecione o ícone de filtro ao lado da caixa de pesquisa.
![](../media/launch/tags-7.png)
* Você pode selecionar "ou", "e" ou "não" para modificar o comportamento do filtro de marcas selecionadas.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Usar o PowerShell para importar ou exportar suas conexões (com marcas)

> Aplica-se a: Windows Admin Center Preview

Visualização do Windows Admin Center inclui um módulo do PowerShell para importar ou exportar a lista de conexão.

>[!IMPORTANT]
>Importação e exportação de conexões com o módulo do PowerShell tem suporte apenas ao Windows Admin Center é implantado como um serviço de gateway no Windows Server.

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

O formato do arquivo CSV começa com os três títulos: ```"name","type","tags"```, seguido por cada conexão em uma nova linha.

**nome** é o FQDN do que a conexão

**tipo** é o tipo de conexão. Para as conexões padrão incluídas com o Windows Admin Center, você usará um destes procedimentos:

| Tipo de conexão | Cadeia de caracteres de Conexão |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| Windows 10 PC | msft.sme.connection-type.windows-client |
| Cluster de failover | msft.sme.connection-type.cluster |
| Cluster Hiperconvergente | msft.sme.connection-type.hyper-converged-cluster |

**marcas** são separados por pipe.

### <a name="example-csv-file-for-importing-connections"></a>Exemplo de arquivo CSV para importar conexões

```
"name","type","tags"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"mycluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"myclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importar conexões RDCman

Use o script a seguir para exportar as conexões salvas no [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) para um arquivo. Você pode, em seguida, importar o arquivo de Windows Admin Center, mantendo sua hierarquia de agrupamento RDCMan usando marcas. Experimente!

1. Copie e cole o código a seguir em sua sessão do PowerShell:

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

3. Importe resultante. Arquivo CSV no Windows Admin Center e a sua hierarquia de agrupamento RDCMan serão representados por marcas na lista de conexão. Para obter detalhes, consulte [usar o PowerShell para importar ou exportar suas conexões (com marcas)](#use-powershell-to-import-or-export-your-connections-(with-tags)).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Exibir os scripts do PowerShell usados no Windows Admin Center

Depois de se conectar a um servidor, um cluster ou um PC, você pode examinar os scripts do PowerShell que ativam as ações de interface do usuário disponíveis no Windows Admin Center. De dentro de uma ferramenta, clique no ícone do PowerShell na barra superior do aplicativo. Selecione um comando de seu interesse na lista suspensa para navegar até o script do PowerShell correspondente.

![](../media/launch/showscript.png)