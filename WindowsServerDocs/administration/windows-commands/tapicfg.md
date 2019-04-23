---
title: tapicfg
description: Saiba como gerenciar uma partição de diretório de aplicativos TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886507"
---
# <a name="tapicfg"></a>tapicfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, remove, exibe uma partição de diretório de aplicativo TAPI ou define uma partição de diretório de aplicativo TAPI padrão. Os clientes TAPI 3.1 podem usar as informações nessa partição de diretório de aplicativo com o serviço de localizador de serviço de diretório para localizar e se comunicar com os diretórios TAPI. Você também pode usar **tapicfg** para criar ou remover pontos de conexão de serviço, que permitem que os clientes TAPI localizar com eficiência as partições de diretório de aplicativo TAPI em um domínio. Para obter mais informações, consulte comentários. Para exibir a sintaxe de comando, clique em um comando. 
-   [tapicfg install](#BKMK_install)
-   [tapicfg remove](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [Mostrar tapicfg](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>tapicfg install
Cria uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|instalar /directory:\<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI a ser criado. Esse nome deve ser um nome de domínio totalmente qualificado.|
|/server: \<DCName>|Especifica o nome DNS do controlador de domínio no qual a partição de diretório de aplicativo TAPI é criada. Se o nome do controlador de domínio não for especificado, o nome do computador local será usado.|
|/forcedefault|Especifica que esse diretório é a partição de diretório de aplicativos TAPI padrão para o domínio. Pode haver várias partições de diretório de aplicativo TAPI em um domínio.<br /><br />Se esse diretório é a primeira partição de diretório de aplicativo TAPI criada no domínio, ele é definido automaticamente como padrão, independentemente de você usar o **/forcedefault** opção.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_remove"></a>Remover tapicfg
Remove uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|remove /directory:\<PartitionName>|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI a ser removido. Observe que esse nome deve ser um nome de domínio totalmente qualificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
Cria um ponto de conexão de serviço para publicar uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|publishscp /directory:\<PartitionName>|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI do ponto de conexão de serviço será publicado.|
|/Domain:\<DomainName >|Especifica o nome DNS do domínio no qual ponto de conexão de serviço é criado. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/forcedefault|Especifica que esse diretório é a partição de diretório de aplicativos TAPI padrão para o domínio. Pode haver várias partições de diretório de aplicativo TAPI em um domínio.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_removescp"></a>tapicfg removescp
Remove um ponto de conexão de serviço para uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|removescp /directory:\<PartitionName>|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI para o qual um ponto de conexão de serviço é removido.|
|/domain: \<DomainName>|Especifica o nome DNS do domínio do qual o ponto de conexão de serviço é removido. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_show"></a>tapicfg show
Exibe os nomes e locais das partições de diretório de aplicativo TAPI no domínio.

### <a name="syntax"></a>Sintaxe
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/defaultonly|Exibe os nomes e locais de apenas a partição de diretório do padrão TAPI aplicativo no domínio.|
|/domain: \<DomainName>|Especifica o nome DNS do domínio para o qual as partições de diretório de aplicativo TAPI são exibidas. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
Define a partição de diretório de aplicativos TAPI padrão para o domínio.

### <a name="syntax"></a>Sintaxe
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|makedefault /directory:\<PartitionName>|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI definida como a partição padrão para o domínio. Observe que esse nome deve ser um nome de domínio totalmente qualificado. Especifica o nome DNS do domínio para o qual a partição de diretório de aplicativos TAPI está definida como o padrão. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
Você deve ser um membro do grupo Administradores de empresa no active directory para executar um **instalar tapicfg** (para criar uma partição de diretório de aplicativos TAPI) ou **tapicfg remover** (para remover um aplicativo de TAPI partição de diretório).

Essa ferramenta de linha de comando pode ser executada em qualquer computador que seja membro do domínio.

Texto fornecido pelo usuário (como os nomes das partições de diretório de aplicativos, servidores e domínios TAPI) com caracteres internacionais ou Unicode só será exibido corretamente se as fontes apropriadas e suporte de idioma estiverem instalados.

Você ainda pode usar servidores de serviço ILS (Internet Locator) em sua organização, se ILS for necessário para dar suporte a determinados aplicativos, porque os clientes TAPI executando o sistema operacional Windows Server 2003 ou Windows XP podem consultar servidores ILS ou aplicativo TAPI partições de diretório.

Você pode usar **tapicfg** para criar ou remover pontos de conexão de serviço. Se a partição de diretório de aplicativos TAPI for renomeada por qualquer motivo (por exemplo, se você renomear o domínio no qual ela reside), você deve remover o ponto de conexão de serviço existente e criar um novo que contém o novo nome DNS do diretório do aplicativo TAPI partição a ser publicado. Caso contrário, os clientes TAPI não conseguem localizar e acessar a partição de diretório de aplicativos TAPI. Você também pode remover um ponto de conexão de serviço para fins de manutenção ou de segurança (por exemplo, se você não deseja expor dados TAPI em uma partição de diretório de aplicativo TAPI específica).

## <a name="examples"></a>Exemplos
Para criar uma partição de diretório de aplicativo TAPI tapifiction.testdom.microsoft.com nomeada em um servidor chamado testdc e, em seguida, defini-lo como a partição de diretório de aplicativos TAPI padrão para o novo domínio, digite:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Para exibir o nome da partição de diretório de aplicativo TAPI padrão para o novo domínio, digite:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
