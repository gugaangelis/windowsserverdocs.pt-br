---
title: WDSUTIL Add-ImageDriverPackage
description: Artigo de referência para o comando WDSUTIL Add-ImageDriverPackage, que adiciona um pacote de driver que está no repositório de drivers a uma imagem de inicialização existente no servidor.
ms.topic: reference
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5d441007f673a194799e245bb704d31add81a483
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524501"
---
# <a name="wdsutil-add-imagedriverpackage"></a>WDSUTIL Add-ImageDriverPackage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um pacote de driver que está no repositório de drivers a uma imagem de inicialização existente no servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-ImageDriverPackage [/Server:<Servername>] [media:<Imagename>] [mediatype:Boot] [/Architecture:{x86 | ia64 | x64}] [/Filename:<Filename>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome de servidor não for especificado, o servidor local será usado. |
| [mídia: `<Imagename>` ] | Especifica o nome da imagem à qual adicionar o driver. |
| [MediaType: inicialização] | Especifica o tipo de imagem ao qual adicionar o driver. Os pacotes de driver só podem ser adicionados a imagens de inicialização. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica a arquitetura da imagem de inicialização. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada. |
| [/Filename:`<Filename>`] | Especifica o nome do arquivo. Se a imagem não puder ser identificada exclusivamente pelo nome, o nome do arquivo deverá ser especificado. |
| [/DriverPackage:`<Name>` | Especifica o nome do pacote de driver a ser adicionado à imagem. |
| [/PackageId: `<ID>` ] | Especifica a ID dos serviços de implantação do Windows do pacote de driver. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome. Para localizar a ID do pacote, selecione o grupo de drivers no qual o pacote está (ou o nó **todos os pacotes** ), clique com o botão direito do mouse no pacote e selecione **Propriedades**. A ID do pacote é listada na guia **geral** . Por exemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="examples"></a>Exemplos

Para adicionar um pacote de driver a uma imagem de inicialização, digite:

```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```

```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-imagedriverpackages](wdsutil-add-imagedriverpackages.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
