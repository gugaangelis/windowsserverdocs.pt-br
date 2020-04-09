---
title: pnpunattend
description: Saiba como auditar os drivers de dispositivo em um computador, bem como executar instalações de driver silenciosas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c4836665946b39acdacf4c204c6e79fc2d8507bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837529"
---
# <a name="pnpunattend"></a>pnpunattend

Audita um computador para drivers de dispositivo e executa instalações autônomas de driver ou pesquisa de drivers sem instalar e, opcionalmente, relatar os resultados para a linha de comando. Use esse comando para especificar a instalação de drivers específicos para dispositivos de hardware específicos. Veja os comentários.

## <a name="syntax"></a>Sintaxe

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|auditSystem|Especifica a instalação do driver online.</br>Obrigatório, exceto quando **pnpunattend** é executado com a **/Help** ou **/?** .|
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

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)