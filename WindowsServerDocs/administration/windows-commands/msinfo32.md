---
title: msinfo32
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843457"
---
# <a name="msinfo32"></a>msinfo32

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abre a ferramenta de informações do sistema para exibir uma visão abrangente de hardware, componentes do sistema e ambiente de software no computador local. 
## <a name="syntax"></a>Sintaxe
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<path>|Especifica o arquivo a ser aberto no formato *C:\Folder1\File1.XXX*, onde *C* é a letra da unidade *Pasta1* é a pasta *File1*é o nome de arquivo, e *XXX* é a extensão de nome de arquivo.<br /><br />Esse arquivo pode ser um **nfo**, **. XML**, **. txt**, ou **. cab** arquivo.|
|<computerName>|Especifica o nome do computador local ou de destino. Isso pode ser um nome UNC, um endereço IP ou um nome completo do computador.|
|<CategoryID>|Especifica a ID do item de categoria. Você pode obter a ID da categoria usando **/showcategories**.|
|/pch|Exibe o modo de exibição do histórico do sistema na ferramenta de informações do sistema.|
|/nfo|Salva o arquivo exportado como um **nfo** arquivo. Se o nome do arquivo que é especificado na *caminho* não termina com um **nfo** extensão, o **nfo** extensão é acrescentada automaticamente ao nome do arquivo.|
|/report|Salva o arquivo no *caminho* como um arquivo de texto. O nome do arquivo é salvo, exatamente como ele aparece no *caminho*. A extensão. txt não é acrescentada ao arquivo, a menos que ele é especificado no caminho.|
|/computer|Inicia a ferramenta de informações do sistema para o computador remoto especificado. Você deve ter as permissões apropriadas para acessar o computador remoto.|
|/showcategories|Inicia a ferramenta de informações do sistema com todas as identificações de categoria, em vez de exibir os nomes amigáveis ou traduzidos. Por exemplo, a categoria do ambiente de Software é exibida como o **SWEnv** categoria.|
|/category|Inicia as informações do sistema com a categoria especificada. Use **/showcategories** para exibir uma lista de IDs de categoria disponíveis.|
|/categories|Inicia as informações do sistema com apenas a categoria especificada ou as categorias. Ela também limita a saída para a categoria selecionada ou categorias. Use **/showcategories** para exibir uma lista de IDs de categoria disponíveis.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
Algumas categorias de informações do sistema contêm grandes quantidades de dados. Você pode usar o **start /wait** comando para otimizar o desempenho de relatório para essas categorias. Para obter mais informações, consulte [informações do sistema](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="BKMK_Examples"></a>Exemplos
Para listar as IDs de categoria disponíveis, digite:
```
msinfo32 /showcategories
```
Para iniciar a ferramenta de informações do sistema com todas as informações disponíveis exibidos, exceto os módulos carregados, digite:
```
msinfo32 /categories +all -loadedmodules
```
Para exibir somente as informações de resumo do sistema e criar um arquivo. nfo chamado res_sis contém informações de categoria de resumo do sistema, digite:
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Para exibir informações de conflito de recursos e criar um arquivo. nfo chamado nfo que contém informações sobre conflitos de recursos, digite:
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)

