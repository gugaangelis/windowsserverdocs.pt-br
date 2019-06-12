---
title: pnpunattend
description: Saiba como os drivers de dispositivo em um computador de auditoria, bem como executar instalações de driver sem confirmação.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 53b72459d497ac5d079336c2a00ba65634b2e3a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436329"
---
# <a name="pnpunattend"></a>pnpunattend

Auditorias de um computador para drivers de dispositivo e executar instalações autônomas do driver, ou procurar drivers sem instalar e, opcionalmente, os resultados de relatório para a linha de comando. Use este comando para especificar a instalação de drivers específicos para dispositivos de hardware específico. Veja os comentários.

## <a name="syntax"></a>Sintaxe

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|auditSystem|Especifica a instalação do driver online.</br>Obrigatório, exceto quando **pnpunattend** é executado com qualquer um os **/Help** ou **/?** parâmetros.|
|/s|Opcional. Especifica a procura por drivers sem instalar.|
|/L|Opcional. Especifica para exibir as informações de log para este comando no prompt de comando.|
|/?|Opcional. Exibe a Ajuda para este comando no prompt de comando.|

## <a name="remarks"></a>Comentários

Preparação preliminar é necessária. Antes de usar esse comando, você deve concluir as seguintes tarefas:

1. Crie um diretório para os drivers que você deseja instalar. Por exemplo, crie uma pasta no **C:\Drivers\Video** para drivers de adaptador de vídeo.
2. Baixe e extraia o pacote de driver para seu dispositivo. Copie o conteúdo da subpasta que contém o arquivo INF para a sua versão do sistema operacional e todas as subpastas para a pasta de vídeo que você criou. Por exemplo, copie os arquivos de driver de vídeo para C:\Drivers\Video.
3. Adicionar uma variável de caminho de ambiente do sistema para a pasta que você criou na etapa 1, por exemplo, **C:\Drivers\Video**.
4. Crie a seguinte chave do registro e, em seguida, para o **DriverPaths** chave que você cria, defina as **dados de valor** para **1**.
5. Para o Windows® 7 navegue até o caminho do registro: **HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion.\\** e, em seguida, criar as chaves: **UnattendSettings\PnPUnattend\DriverPaths\\**
6. Para o Windows Vista, navegue até o caminho do registro: **HK_LM\Software\Microsoft\Windows NT\CurrentVersion.\\** e, em seguida, criar as chaves = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Exemplos

O comando de exemplo a seguir mostra como usar o **PNPUnattend.exe** para um computador para possíveis atualizações de driver de auditoria e, em seguida, relatar as descobertas ao prompt de comando.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)