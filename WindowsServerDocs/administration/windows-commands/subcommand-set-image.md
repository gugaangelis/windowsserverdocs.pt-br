---
title: Conjunto de subcomando-imagem
description: Artigo de referência para subcomando set-Image, que altera os atributos de uma imagem.
ms.topic: reference
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecb35deed9f938a7498aa282701034b5d9455233
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036814"
---
# <a name="subcommand-set-image"></a>Subcomando: Set-Image

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de uma imagem.

## <a name="syntax"></a>Sintaxe
para imagens de inicialização:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>]
[/Description:<Description>] [/Enabled:{Yes | No}]
```
para imagens de instalação:
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
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
meio<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {boot &#124; install}|Especifica o tipo de imagem.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem. Como você pode ter o mesmo nome de imagem para diferentes imagens de inicialização em diferentes arquiteturas, especificar a arquitetura garante que a imagem correta seja modificada.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/Name|Especifica o nome da imagem.|
|[/Description:<Description>]|Define a descrição da imagem.|
|[/Enabled: {Sim &#124; não}]|Habilita ou desabilita a imagem.|
|\mediaGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar o grupo de imagens.|
|[/UserFilter: <SDDL> ]|Define o filtro de usuário na imagem. A cadeia de caracteres de filtro deve estar no formato SDDL (Security Descriptor Definition Language). Observe que, diferentemente da opção **/Security** para grupos de imagens, essa opção apenas restringe quem pode ver a definição de imagem, e não os recursos de arquivo de imagem real. Para restringir o acesso aos recursos de arquivo e, portanto, acessar todas as imagens em um grupo de imagens, você precisará definir a segurança para o próprio grupo de imagens.|
|[/UnattendFile:<Unattend file path>]|Define o caminho completo para o arquivo autônomo a ser associado à imagem. Por exemplo: **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend: {Sim &#124; não}]|Você pode especificar **/overwrite** para substituir o arquivo Unattend se já houver um arquivo autônomo associado à imagem. Observe que a configuração padrão é **não**.|
## <a name="examples"></a>Exemplos
Para definir valores para uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Set-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /Description:New description
wdsutil /verbose /Set-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
/Name:New Name /Description:New Description /Enabled:Yes
```
Para definir valores para uma imagem de instalação, digite um dos seguintes:
```
wdsutil /Set-Imagmedia:Windows Vista with Officemediatype:Install /Description:New description
wdsutil /verbose /Set-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /Name:New name /Description:New description /UserFilter:O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU) /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-image-command.md) 
 Add-Image [Usando o comando](using-the-copy-image-command.md) 
 Copy-Image [Usando o comando](using-the-export-image-command.md) 
 Export-Image [Usando o comando](using-the-get-image-command.md) 
 Get-Image [Usando o comando](using-the-remove-image-command.md) 
 Remove-Image [Usando o comando Replace-Image](using-the-replace-image-command.md)
