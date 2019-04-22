---
title: msdt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781e83615d44a4adb20fd85fe5ce6ebc7c2d4966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818827"
---
# <a name="msdt"></a>msdt



Invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado e permite que as opções adicionais sem entrada do usuário.

## <a name="syntax"></a>Sintaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parâmetros

A tabela a seguir inclui os parâmetros e opções com suporte pelo msdt.exe.

|Parâmetro|Descrição|
|---------|-----------|
|/ID \<nome do pacote >|Especifica qual pacote de diagnóstico será executado. Para obter uma lista de pacotes disponíveis, consulte a identificação do pacote de solução de problemas na "disponível pacotes de solução de problemas? seção mais adiante neste tópico.|
|/Path \<diretório | arquivo .diagpkg | arquivo .diagcfg >|Especifica o caminho completo para um pacote de diagnóstico. Se você especificar um diretório, o diretório deve conter um pacote de diagnóstico. Você não pode usar o parâmetro /path em conjunto com o */id*, */dci*, ou */cab* parâmetro.|
|/dci \<passkey>|Predefine o campo de chave de acesso no msdt. Esse parâmetro é usado somente quando um provedor de suporte tiver fornecido uma chave de acesso.|
|/dt \<directory>|Exibe o histórico de solução de problemas no diretório especificado. Resultados do diagnóstico são armazenados do usuário **%LOCALAPPDATA%\Diagnostics** ou **%LOCALAPPDATA%\ElevatedDiagnostics** diretórios.|
|/af \<answer file>|Especifica um arquivo de resposta no formato XML que contém as respostas às interações do diagnóstico de um ou mais.|
