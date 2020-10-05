---
title: WDSUTIL Get-Server
description: Artigo de referência para o WDSUTIL Get-Server, que recupera informações do servidor de serviços de implantação do Windows especificado.
ms.topic: reference
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e855724833a90c64dfb3692ccd82fad67a572873
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729636"
---
# <a name="wdsutil-get-server"></a>WDSUTIL Get-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações do servidor de serviços de implantação do Windows especificado.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {config &#124; imagens &#124; todos}|Especifica o tipo de informações a serem retornadas.<p>-   **Config** retorna informações de configuração.<br />-   As **imagens** retornam informações sobre grupos de imagens, imagens de inicialização e imagens de instalação.<br />-   **Todos** retorna informações de configuração e informações de imagem.|
|[/detailed]|Você pode usar essa opção com **/show: images** ou **/show: ALL** para indicar que todos os metadados de imagem de cada imagem devem ser retornados. Se a opção **/detailed** não for usada, o comportamento padrão será retornar o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre o servidor, digite:
```
wdsutil /Get-Server /Show:Config
```
Para exibir informações detalhadas sobre o servidor, digite:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [WDSUTIL Disable – comando do servidor](wdsutil-disable-server.md)
- [WDSUTIL Enable-Server comando](wdsutil-enable-server.md)
- [comando do servidor de inicialização WDSUTIL](wdsutil-initialize-server.md)
- [WDSUTIL Set-Server Command](wdsutil-set-server.md)
- [comando Start-Server do WDSUTIL](wdsutil-start-server.md)
- [comando WDSUTIL Stop-Server](wdsutil-stop-server.md)
- [WDSUTIL não inicializar-comando de servidor](wdsutil-uninitialize-server.md)
