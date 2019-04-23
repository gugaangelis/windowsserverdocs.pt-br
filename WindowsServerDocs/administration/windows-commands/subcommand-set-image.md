---
title: Subcomando definir imagem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e63c67210764de76edae18a1897a68d763f9d695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856477"
---
# <a name="subcommand-set-image"></a>Subcommand: set-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de uma imagem.
## <a name="syntax"></a>Sintaxe
para imagens de inicialização:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
Para instalar imagens:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
tipo de mídia: {inicialização &#124; instalar}|Especifica o tipo de imagem.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem. Como você pode ter o mesmo nome de imagem para imagens de inicialização diferente em diferentes arquiteturas, especificando a arquitetura garante que a imagem correta será modificada.|
|[/Filename:<File name>]|Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|[/ Nome]|Especifica o nome da imagem.|
|[/Description:<Description>]|Define a descrição da imagem.|
|[/ Habilitado: {Sim &#124; não}]|Habilita ou desabilita a imagem.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo será usado. Se houver mais de um grupo de imagens no servidor, você deve usar essa opção para especificar o grupo de imagens.|
|[/UserFilter:<SDDL>]|Define o filtro de usuário na imagem. A cadeia de caracteres de filtro deve estar no formato de linguagem de definição de descritor de segurança (SDDL). Observe que, ao contrário do **/segurança** opção para grupos de imagens, essa opção só restringe quem pode ver a definição da imagem e não os recursos de arquivo de imagem real. Para restringir o acesso aos recursos de arquivo e, portanto, acesso a todas as imagens dentro de um grupo de imagens, você precisará definir a segurança para o grupo de imagens.|
|[/UnattendFile:<Unattend file path>]|Define o caminho completo para o arquivo autônomo a ser associado com a imagem. Por exemplo:  **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend:{Yes &#124; No}]|Você pode especificar **/overwrite** para substituir o arquivo autônomo se já houver um arquivo autônomo associado à imagem. Observe que a configuração padrão é **não**.|
## <a name="BKMK_examples"></a>Exemplos
Para definir valores para uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
Para definir valores para uma imagem de instalação, digite o seguinte:
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando a comando de copiar imagem](using-the-copy-image-command.md)
[usando o Comando de Export-Image](using-the-export-image-command.md)
[usando o comando get-Image](using-the-get-image-command.md)
[usando o comando de remove-Image](using-the-remove-image-command.md) 
 [ Usando a comando de substituir imagem](using-the-replace-image-command.md)
