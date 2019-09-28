---
title: bootcfg
description: O tópico de comandos do Windows para **Bootcfg** – configura, consulta ou altera as configurações do arquivo boot. ini.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d66296327a2221093e5434f69e15e7c55df1f6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379847"
---
# <a name="bootcfg"></a>bootcfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura, consulta ou altera as configurações do arquivo Boot.ini.  
## <a name="syntax"></a>Sintaxe  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.|  
|[bootcfg copy](bootcfg-copy.md)|Faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando.|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|Configura a depuração de porta 1394 para uma entrada de sistema operacional especificada.|  
|[bootcfg debug](bootcfg-debug.md)|Adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada.|  
|[bootcfg default](bootcfg-default.md)|Especifica a entrada do sistema operacional a ser designada como padrão.|  
|[bootcfg delete](bootcfg-delete.md)|exclui uma entrada do sistema operacional na seção **[Operating Systems]** do arquivo boot. ini.|  
|[bootcfg ems](bootcfg-ems.md)|Permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto.|  
|[bootcfg query](bootcfg-query.md)|Consulta e exibe as entradas da seção [boot loader] e **[Operating Systems]** do boot. ini.|  
|[bootcfg raw](bootcfg-raw.md)|adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção **[Operating Systems]** do arquivo boot. ini.|  
|[bootcfg rmsw](bootcfg-rmsw.md)|Remove as opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.|  
|[bootcfg timeout](bootcfg-timeout.md)|altera o valor de tempo limite do sistema operacional.|  
