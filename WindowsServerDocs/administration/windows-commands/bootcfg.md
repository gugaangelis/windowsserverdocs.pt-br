---
title: bootcfg
description: Tópico de referência para o comando Bootcfg, que configura, consulta ou altera as configurações do arquivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aca24cfbf47586ae1d7d4262c232be47a056f7ae
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708862"
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
| [bootcfg delete](bootcfg-delete.md) | Exclui uma entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini. |
| [bootcfg ems](bootcfg-ems.md) | Permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto. |
| [bootcfg query](bootcfg-query.md) | Consulta e exibe as entradas da seção [boot loader] e [Operating Systems] do boot. ini. |
| [bootcfg raw](bootcfg-raw.md) | Adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini. |
| [bootcfg rmsw](bootcfg-rmsw.md) | Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada. |
| [bootcfg timeout](bootcfg-timeout.md) | Altera o valor de tempo limite do sistema operacional. |
