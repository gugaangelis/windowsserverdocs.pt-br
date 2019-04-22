---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Objectid do fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813737"
---
# <a name="fsutil-objectid"></a>Objectid do fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gerencia os identificadores de objeto (OIDs), que são objetos internos usados pelo serviço de cliente de controle de Link distribuído (DLT) e replicação FRS (serviço) para controlar outros objetos como arquivos, diretórios e links. Identificadores de objeto são invisíveis para a maioria dos programas e nunca devem ser modificados.

> [!CAUTION]
> Não excluir, definir ou modificar um identificador de objeto. Excluir ou definir um identificador de objeto pode resultar na perda de dados de partes de um arquivo, até e incluindo volumes inteiros de dados. Além disso, você pode causar comportamento adverso no serviço de cliente de controle de Link distribuído (DLT) e replicação FRS (serviço).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|criar|Cria um identificador de objeto se o arquivo especificado não tiver uma. Se o arquivo já tiver um identificador de objeto, este subcomando é equivalente à **consulta** subcomando.|
|excluir|Exclui um identificador de objeto.|
|consulta|Consulta de um identificador de objeto.|
|set|Define um identificador de objeto.|
|\<ObjectID>|Define um identificador hexadecimal de 16 bytes específico de arquivo que é garantido que seja exclusivo dentro de um volume. O identificador de objeto é usado pelo serviço de cliente de controle de Link distribuído (DLT) e a replicação FRS (serviço) para identificar os arquivos.|
|\<BirthVolumeID>|Indica o volume no qual o arquivo estava localizado quando ele tiver obtido pela primeira vez um identificador de objeto. Esse valor é um identificador hexadecimal de 16 bytes que é usado pelo serviço de cliente DLT.|
|\<BirthObjectID>|Indica o identificador de objeto do arquivo original (a *ObjectID* pode ser alterado quando um arquivo é movido). Esse valor é um identificador hexadecimal de 16 bytes que é usado pelo serviço de cliente DLT.|
|\<DomainID >|Identificador de domínio hexadecimal de 16 bytes. Esse valor não é usado atualmente e deve ser definido como todos os zeros.|
|\<FileName>|Especifica o caminho completo para o arquivo, incluindo o nome de arquivo e extensão, por exemplo C:\documents\filename.txt.|

## <a name="remarks"></a>Comentários

-   Qualquer arquivo que tenha um identificador de objeto também tem um identificador de volume de nascimento, um identificador de objeto de nascimento e um identificador de domínio. Quando você move um arquivo, o identificador de objeto pode ser alterado, mas o volume de nascimento e identificadores de objeto de nascimento permanecem os mesmos. Esse comportamento permite que o sistema operacional do Windows sempre localizar um arquivo, não importa onde ele foi movido.

## <a name="BKMK_examples"></a>Exemplos
Para criar um identificador de objeto, digite:

`fsutil objectid create c:\temp\sample.txt`

Para excluir um identificador de objeto, digite:

`fsutil objectid delete c:\temp\sample.txt`

Para consultar um identificador de objeto, digite:

`fsutil objectid query c:\temp\sample.txt`

Para definir um identificador de objeto, digite:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


