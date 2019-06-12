---
title: Usando o comando add-ImageDriverPackage
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38d6c032f347f9945701f17b9289f3e3ff474031
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440627"
---
# <a name="using-the-add-imagedriverpackage-command"></a>Usando o comando add-ImageDriverPackage

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um pacote de driver que está no repositório de drivers a uma imagem de inicialização existente no servidor. A versão da imagem deve ser Windows 7 ou Windows Server 2008 R2 ou posterior.
## <a name="syntax"></a>Sintaxe
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parâmetros

|                 Parâmetro                  |                                                                                                                                                                                                            Descrição                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server:<Server name>           |                                                                                                                                               Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                |
|             mídia:<Image name>             |                                                                                                                                                                                       Especifica o nome da imagem para adicionar o driver.                                                                                                                                                                                        |
|               mediatype:Boot               |                                                                                                                                                                Especifica o tipo de imagem para adicionar o driver. Só podem ser adicionados a pacotes de driver a imagens de inicialização.                                                                                                                                                                 |
| / Arquitetura: {x86 &#124; ia64 &#124; x64} |                                                                                                       Especifica a arquitetura da imagem de inicialização. Como é possível ter o mesmo nome de imagem para imagens de inicialização em arquiteturas diferentes, você deve especificar a arquitetura para garantir que a imagem correta é usada.                                                                                                        |
|           /Filename:<File name>]           |                                                                                                                                                        Especifica o nome do arquivo. Se a imagem não pode ser identificada exclusivamente pelo nome, o nome do arquivo deve ser especificado.                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   Especifica o nome do pacote de driver para adicionar à imagem.                                                                                                                                                                                    |
|             [/PackageId:<ID>]              | Especifica a ID de serviços de implantação do Windows do pacote de driver. Você deve especificar essa opção se o pacote de driver não pode ser identificado exclusivamente pelo nome. Para localizar a ID do pacote, clique no grupo de driver que o pacote está em (ou o **todos os pacotes** nó), o pacote com o botão direito e, em seguida, clique em **propriedades**. A ID do pacote está listada na **geral** guia. Por exemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="BKMK_examples"></a>Exemplos
Para adicionar um pacote de driver a uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
