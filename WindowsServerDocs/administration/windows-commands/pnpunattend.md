---
title: pnpunattend
description: Saiba como auditar os drivers de dispositivo em um computador, bem como executar instalações de driver silenciosas.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 77a6ab1ea45322e3c53e8b095c412cf8838be60d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372275"
---
# <a name="pnpunattend"></a>pnpunattend

Audita um computador para drivers de dispositivo e executa instalações autônomas de driver ou pesquisa de drivers sem instalar e, opcionalmente, relatar os resultados para a linha de comando. Use esse comando para especificar a instalação de drivers específicos para dispositivos de hardware específicos. Veja os comentários.

## <a name="syntax"></a>Sintaxe

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|auditSystem|Especifica a instalação do driver online.</br>Obrigatório, exceto quando **pnpunattend** é executado com a **/Help** ou **/?** parâmetro.|
|/s|Opcional. Especifica a pesquisa de drivers sem instalar o.|
|/L|Opcional. Especifica para exibir as informações de log para este comando no prompt de comando.|
|/?|Opcional. Exibe a ajuda para este comando no prompt de comando.|

## <a name="remarks"></a>Comentários

A preparação preliminar é necessária. Antes de usar esse comando, você deve concluir as seguintes tarefas:

1. Crie um diretório para os drivers que você deseja instalar. Por exemplo, crie uma pasta em **C:\Drivers\Video** para drivers de adaptador de vídeo.
2. Baixe e extraia o pacote de driver para seu dispositivo. Copie o conteúdo da subpasta que contém o arquivo INF para sua versão do sistema operacional e quaisquer subpastas na pasta de vídeo que você criou. Por exemplo, copie os arquivos de driver de vídeo para C:\Drivers\Video.
3. Adicione uma variável de caminho do ambiente do sistema à pasta que você criou na etapa 1. por exemplo, **C:\Drivers\Video**.
4. Crie a seguinte chave do registro e, em seguida, para a chave **DriverPaths** que você criar, defina os **dados do valor** como **1**.
5. Para o Windows® 7, navegue pelo caminho do registro: **HKEY_LOCAL_Machine \Software\microsoft\windows NT\CurrentVersion\\** e, em seguida, crie as chaves: **UnattendSettings\PnPUnattend\DriverPaths\\**
6. Para o Windows Vista, navegue até o caminho do registro: **HK_LM \Software\microsoft\windows NT\CurrentVersion\\** e, em seguida, crie as chaves = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Exemplos

O comando de exemplo a seguir mostra como usar o **PNPUnattend. exe** para auditar um computador quanto a possíveis atualizações de driver e, em seguida, relatar as descobertas para o prompt de comando.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)