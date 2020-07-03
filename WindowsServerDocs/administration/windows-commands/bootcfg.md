---
title: bootcfg
description: Artigo de referência para o comando Bootcfg, que configura, consulta ou altera Boot.ini configurações de arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4c64ab33e8026606072cbb1d509eb3c787f76c0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924942"
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
