---
title: tapicfg
description: Saiba como gerenciar uma partição de diretório de aplicativos TAPI.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e9e113f0679034a4cd135cad6e7c546dc59c5c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383674"
---
# <a name="tapicfg"></a>tapicfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, remove ou exibe uma partição de diretório de aplicativo TAPI ou define uma partição de diretório de aplicativo TAPI padrão. Os clientes TAPI 3,1 podem usar as informações nessa partição de diretório de aplicativo com o serviço localizador de serviço de diretório para localizar e se comunicar com diretórios TAPI. Você também pode usar **Tapicfg** para criar ou remover pontos de conexão de serviço, que permitem que clientes TAPI localizem partições de diretório de aplicativos TAPI com eficiência em um domínio. Para obter mais informações, consulte comentários. Para exibir a sintaxe do comando, clique em um comando. 
-   [instalação de Tapicfg](#BKMK_install)
-   [remover Tapicfg](#BKMK_remove)
-   [Tapicfg publishscp](#BKMK_publishscp)
-   [Tapicfg removescp](#BKMK_removescp)
-   [Tapicfg mostrar](#BKMK_show)
-   [Tapicfg MakeDefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>instalação de Tapicfg
Cria uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|instalar o/diretório: \<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório do aplicativo TAPI a ser criada. Esse nome deve ser um nome de domínio totalmente qualificado.|
|/Server \<DCName >|Especifica o nome DNS do controlador de domínio no qual a partição de diretório do aplicativo TAPI é criada. Se o nome do controlador de domínio não for especificado, o nome do computador local será usado.|
|/forcedefault|Especifica que esse diretório é a partição de diretório de aplicativo TAPI padrão para o domínio. Pode haver várias partições de diretório de aplicativos TAPI em um domínio.<br /><br />Se esse diretório for a primeira partição de diretório de aplicativo TAPI criada no domínio, ele será automaticamente definido como o padrão, independentemente de você usar a opção **/forcedefault** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_remove"></a>remover Tapicfg
Remove uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|remover/diretório: \<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório do aplicativo TAPI a ser removida. Observe que esse nome deve ser um nome de domínio totalmente qualificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_publishscp"></a>Tapicfg publishscp
Cria um ponto de conexão de serviço para publicar uma partição de diretório de aplicativo TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|publishscp/diretório: \<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI que o ponto de conexão de serviço publicará.|
|/domain: \<DomainName >|Especifica o nome DNS do domínio no qual o ponto de conexão de serviço é criado. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/forcedefault|Especifica que esse diretório é a partição de diretório de aplicativo TAPI padrão para o domínio. Pode haver várias partições de diretório de aplicativos TAPI em um domínio.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_removescp"></a>Tapicfg removescp
Remove um ponto de conexão de serviço para uma partição de diretório de aplicativos TAPI.

### <a name="syntax"></a>Sintaxe
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|removescp/diretório: \<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório de aplicativo TAPI para a qual um ponto de conexão de serviço é removido.|
|/Domain \<DomainName >|Especifica o nome DNS do domínio do qual o ponto de conexão de serviço é removido. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_show"></a>Tapicfg mostrar
Exibe os nomes e locais das partições de diretório do aplicativo TAPI no domínio.

### <a name="syntax"></a>Sintaxe
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/defaultonly|Exibe os nomes e locais apenas da partição de diretório de aplicativo TAPI padrão no domínio.|
|/Domain \<DomainName >|Especifica o nome DNS do domínio para o qual as partições de diretório do aplicativo TAPI são exibidas. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_makedefault"></a>Tapicfg MakeDefault
Define a partição de diretório de aplicativo TAPI padrão para o domínio.

### <a name="syntax"></a>Sintaxe
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|MakeDefault/diretório: \<PartitionName >|Obrigatório. Especifica o nome DNS da partição de diretório do aplicativo TAPI definida como a partição padrão para o domínio. Observe que esse nome deve ser um nome de domínio totalmente qualificado. Especifica o nome DNS do domínio para o qual a partição de diretório do aplicativo TAPI está definida como padrão. Se o nome de domínio não for especificado, o nome do domínio local será usado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
Você deve ser membro do grupo Administradores de empresa no Active Directory para executar o **Tapicfg install** (para criar uma partição de diretório de aplicativos TAPI) ou **Tapicfg remove** (para remover uma partição de diretório de aplicativo TAPI).

Essa ferramenta de linha de comando pode ser executada em qualquer computador que seja membro do domínio.

O texto fornecido pelo usuário (como os nomes das partições de diretório do aplicativo TAPI, servidores e domínios) com caracteres internacionais ou Unicode só será exibido corretamente se as fontes apropriadas e o suporte a idioma estiverem instalados.

Você ainda pode usar servidores ILS (serviço de localização da Internet) em sua organização, se o ILS for necessário para dar suporte a determinados aplicativos, porque os clientes TAPI que executam o Windows XP ou um sistema operacional Windows Server 2003 podem consultar os servidores ILS ou o aplicativo TAPI partições de diretório.

Você pode usar **Tapicfg** para criar ou remover pontos de conexão de serviço. Se a partição de diretório do aplicativo TAPI for renomeada por qualquer motivo (por exemplo, se você renomear o domínio no qual ele reside), será necessário remover o ponto de conexão de serviço existente e criar um novo que contenha o novo nome DNS do diretório de aplicativo TAPI partição a ser publicada. Caso contrário, os clientes TAPI não poderão localizar e acessar a partição de diretório de aplicativos TAPI. Você também pode remover um ponto de conexão de serviço para fins de manutenção ou segurança (por exemplo, se não quiser expor dados TAPI em uma partição de diretório de aplicativo TAPI específica).

## <a name="examples"></a>Exemplos
Para criar uma partição de diretório de aplicativo TAPI chamada tapifiction.testdom.microsoft.com em um servidor chamado testdc.testdom.microsoft.com e, em seguida, defini-la como a partição de diretório de aplicativo TAPI padrão para o novo domínio, digite:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Para exibir o nome da partição de diretório de aplicativo TAPI padrão para o novo domínio, digite:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
