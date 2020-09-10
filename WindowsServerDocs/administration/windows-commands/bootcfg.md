---
title: bootcfg
description: Artigo de referência para o comando Bootcfg, que configura, consulta ou altera Boot.ini configurações de arquivo.
ms.topic: reference
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f187015ab3c8cba18e34601faebc1637ecf7896
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630091"
---
# <a name="bootcfg"></a>bootcfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura, consulta ou altera as configurações do arquivo Boot.ini.

## <a name="syntax"></a>Sintaxe

```
bootcfg <parameter> [arguments...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [bootcfg addsw](bootcfg-addsw.md) | Adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada. |
| [bootcfg copy](bootcfg-copy.md) | Faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando. |
| [bootcfg dbg1394](bootcfg-dbg1394.md) | Configura a depuração de porta 1394 para uma entrada de sistema operacional especificada. |
| [bootcfg debug](bootcfg-debug.md) | Adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada. |
| [bootcfg default](bootcfg-default.md) | Especifica a entrada do sistema operacional a ser designada como padrão. |
| [bootcfg delete](bootcfg-delete.md) | Exclui uma entrada do sistema operacional na seção [Operating Systems] do arquivo Boot.ini. |
| [bootcfg ems](bootcfg-ems.md) | Permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto. |
| [bootcfg query](bootcfg-query.md) | Consulta e exibe as entradas da seção [boot loader] e [Operating Systems] de Boot.ini. |
| [bootcfg raw](bootcfg-raw.md) | Adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini. |
| [bootcfg rmsw](bootcfg-rmsw.md) | Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada. |
| [bootcfg timeout](bootcfg-timeout.md) | Altera o valor de tempo limite do sistema operacional. |
