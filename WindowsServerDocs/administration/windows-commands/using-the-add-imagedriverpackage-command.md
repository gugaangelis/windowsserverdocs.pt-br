---
title: Add-ImageDriverPackage
description: Artigo de referência para Add-ImageDriverPackage, que adiciona um pacote de driver que está no repositório de driver a uma imagem de inicialização existente no servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 450b06c2c935f83a0851fb887f34d7403061fea8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922060"
---
# <a name="add-imagedriverpackage"></a>Add-ImageDriverPackage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um pacote de driver que está no repositório de drivers a uma imagem de inicialização existente no servidor. A versão da imagem deve ser Windows 7 ou Windows Server 2008 R2 ou posterior.

## <a name="syntax"></a>Sintaxe
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64}
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parâmetros

|                 Parâmetro                  |                                                                                                                                                                                                            Descrição                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /Server<Server name>           |                                                                                                                                               Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                |
|             meio<Image name>             |                                                                                                                                                                                       Especifica o nome da imagem à qual adicionar o driver.                                                                                                                                                                                        |
|               MediaType: inicialização               |                                                                                                                                                                Especifica o tipo de imagem ao qual adicionar o driver. Os pacotes de driver só podem ser adicionados a imagens de inicialização.                                                                                                                                                                 |
| /Architecture: {x86 &#124; IA64 &#124; x64} |                                                                                                       Especifica a arquitetura da imagem de inicialização. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada.                                                                                                        |
|           /Filename: <File name> ]           |                                                                                                                                                        Especifica o nome do arquivo. Se a imagem não puder ser identificada exclusivamente pelo nome, o nome do arquivo deverá ser especificado.                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   Especifica o nome do pacote de driver a ser adicionado à imagem.                                                                                                                                                                                    |
|             [/PackageId: <ID> ]              | Especifica a ID dos serviços de implantação do Windows do pacote de driver. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome. Para localizar a ID do pacote, clique no grupo de drivers no qual o pacote está (ou no nó **todos os pacotes** ), clique com o botão direito do mouse no pacote e clique em **Propriedades**. A ID do pacote é listada na guia **geral** . Por exemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="examples"></a>Exemplos
Para adicionar um pacote de driver a uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando Add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
