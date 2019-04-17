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
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296648"
---
# Introdução ao Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## Windows Admin Center está instalado no Windows 10

> [!IMPORTANT]
> Você deve ser um membro do grupo do administrador local para usar o Windows Admin Center no Windows 10

### Selecionar um certificado de cliente

Na primeira vez que você abrir o Windows Admin Center no Windows 10, certifique-se de selecionar o certificado de *Cliente do Windows Admin Center* (caso contrário, você receberá um erro de HTTP 403 informando "não é possível obter a essa página").

No Microsoft Edge, quando você for solicitado com essa caixa de diálogo:
 
1. Clique em **mais opções**

    ![](../media/launch-cert-1.png)

2. Selecione o certificado rotulado **Cliente do Windows Admin Center** e clique em **Okey**

    ![](../media/launch-cert-2.png)

3. Certifique-se de **Que permitir acesso sempre** é selecionado e clique em **Permitir**

    ![](../media/launch-cert-3.png)

## Conectando-se a nós gerenciados e clusters

Depois de concluir a instalação do Windows Admin Center, você pode adicionar servidores ou clusters para gerenciar da página de visão geral principal.

 **Adicionar um servidor único ou um cluster como um nó gerenciado**

 1. Clique em **+ Adicionar** em **todas as conexões**.

    ![](../media/launch/addserver0.png)

 2. Optar por adicionar uma conexão de servidor, Cluster de Failover ou Cluster hiperconvergente:
    
    ![](../media/launch/addserver1.png)

 3. Digite o nome do servidor ou cluster para gerenciar e clique em **Enviar**. O servidor ou cluster será adicionado à sua lista de conexão na página de visão geral.

    ![](../media/launch/addserver2.png)

   **-- OU --**

**Vários servidores de importação em massa**

 1. Na página **Adicionar Conexão de servidor** , escolha a guia **Servidores de importação** .

    ![](../media/launch/import-servers.png)

 2. Clique em **Procurar** e selecione um arquivo de texto que contém uma vírgula ou nova linha separados, lista de FQDNs para os servidores que você deseja adicionar.

    **-- OU --**

**Adicionar servidores pesquisando o Active Directory**

 1. Na página **Adicionar Conexão de servidor** , escolha a guia **Pesquisar no Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Insira os critérios de pesquisa e clique em **Pesquisar**. Caracteres curinga (*) é compatíveis.

 3. Após a pesquisa - selecione um ou mais dos resultados, opcionalmente, adicionar marcas e clique em **Adicionar**.

## Autenticar com o nó gerenciado ##

Windows Admin Center oferece suporte a vários mecanismos para autenticar com um nó gerenciado. Logon único é o padrão.

**Logon único**

Você pode usar suas credenciais atuais do Windows para autenticar com o nó gerenciado. Este é o padrão, e o Windows Admin Center tenta o logon quando você adiciona um servidor. 

**Logon único quando implantado como um serviço no Windows Server**

Se você instalou o Windows Admin Center no Windows Server, configuração adicional é necessária para logon único.  [Configurar seu ambiente para delegação](..\configure\user-access-control.md)

**-- OU --**

**Use *Gerenciar como* para especificar credenciais**

Em **Todas as conexões**, selecione um servidor na lista e escolha **Gerenciar como** especificar as credenciais que você usará para se autenticar ao nó gerenciado:

![](../media/launch-use-6.png)

Se o Windows Admin Center está em execução no modo de serviço no Windows Server, mas você não tiver configurada a delegação Kerberos, você deve inserir novamente suas credenciais do Windows:

![](../media/launch-use-7.png)

Você pode aplicar as credenciais para todas as conexões, que serão o cache-los para essa sessão de navegador específico. Se você recarregar o navegador, você deve inserir novamente suas credenciais **Gerenciar como** .

**Solução de senha de administrador local (LAPS)**

Se o ambiente usa [LAPS](https://technet.microsoft.com/mt227395.aspx), você pode usar credenciais LAPS para autenticar com o nó gerenciado. **Se você usar esse cenário, por favor** [fornecer comentários](http://aka.ms/WACFeedback).

## Usando marcas para organizar as conexões

Você pode usar marcas para identificar e filtrar servidores relacionados na sua lista de conexão.  Isso permite que você veja um subconjunto de seus servidores na lista de conexão.  Isso é especialmente útil se você tiver muitas conexões.

### Editar marcas

* Selecione um servidor ou vários servidores na lista de todas as conexões
* Em **Todas as conexões**, clique em **Editar marcas**

![](../media/launch/tags-5.png)

O painel **Editar marcas de Conexão** permite que você modificar, adicionar ou remover marcas de seu conexões selecionadas:

* Para adicionar uma nova marca ao seu conexões selecionadas, selecione **Adicionar marca** e insira o nome de marca que você gostaria de usar.

* Para marcar as conexões selecionadas com um nome de marca existente, marque a caixa ao lado do nome de marca que você deseja aplicar.

* Para remover uma marca de conexões todos selecionadas, desmarque a caixa ao lado da marca que deseja remover.

* Se uma marca é aplicada a um subconjunto das conexões selecionadas, a caixa de seleção é mostrada em um estado intermediário. Você pode clicar na caixa para verificá-lo e aplicar a marca a todas as conexões selecionadas ou clique novamente para desmarcá-la e remover a marca de todas as conexões selecionadas.

![](../media/launch/tags-6.png)

### Filtrar conexões por marca

Depois de marcas foram adicionadas a uma ou mais conexões de servidor, você pode exibir as marcas na lista de conexão e filtrar a lista de conexão por marcas.

* Para filtrar por uma marca, selecione o ícone de filtro ao lado da caixa de pesquisa.
![](../media/launch/tags-7.png)
* Você pode selecionar "ou", "e" ou "não" para modificar o comportamento de filtro das marcas selecionados.
![](../media/launch/tags-8.png)

## Usar o PowerShell para importar ou exportar as conexões (com marcas)

> Aplicável a: Windows Admin Center Preview

Windows Admin Center Preview inclui um módulo do PowerShell para importar ou exportar sua lista de conexão.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### Formato de arquivo CSV para importar conexões

O formato do arquivo CSV começa com quatro títulos ```"name","type","tags","groupId"```, seguido por cada conexão em uma nova linha.

**nome** é o FQDN da conexão

**tipo** é o tipo de conexão. Para as conexões padrão incluídas com o Windows Admin Center, você usará um destes procedimentos:

| Tipo de conexão | Cadeia de caracteres de Conexão |
|------|-------------------------------|
| Windows Server | msft.SME.Connection type.server |
| Computador Windows 10 | msft.SME.Connection-type.windows-client |
| Cluster de failover | msft.SME.Connection type.cluster |
| Cluster Hiperconvergente | msft.SME.Connection-type.hyper-convergiu-cluster |

**as marcas** são separados por pipe.

**groupId** é usado para conexões compartilhadas. Use o valor ```global``` nessa coluna para torná-lo uma conexão compartilhada.

### Exemplo de arquivo CSV para importar conexões

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## Importar conexões RDCman

Use o script a seguir para exportar conexões salvas no [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) para um arquivo. Em seguida, você pode importar o arquivo no Windows Admin Center, mantendo sua hierarquia de agrupamento RDCMan usando marcas. Experimente!

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

3. Importe o resultante. Arquivo CSV no Windows Admin Center e sua hierarquia de agrupamento RDCMan serão representados por marcas na lista de conexão. Para obter detalhes, consulte [Usar o PowerShell para importar ou exportar as conexões (com marcas)](#use-powershell-to-import-or-export-your-connections-with-tags).

## Exibir scripts do PowerShell usados no Windows Admin Center

Depois de se conectar a um servidor, cluster ou PC, você pode examinar os scripts do PowerShell que a energia as ações de interface do usuário disponíveis no Centro de administração do Windows. De dentro de uma ferramenta, clique no ícone de PowerShell na barra superior do aplicativo. Selecione um comando de interesse na lista suspensa para navegar até o script do PowerShell correspondente.

![](../media/launch/showscript.png)