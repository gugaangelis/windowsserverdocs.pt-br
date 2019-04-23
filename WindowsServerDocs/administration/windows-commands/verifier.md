---
title: verifier
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865867"
---
# <a name="verifier"></a>verifier

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerenciador de verificação de driver.  

## <a name="syntax"></a>Sintaxe  
```  
verifier /standard /driver <name> [<name> ...]  
verifier /standard /all  
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]  
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all  
verifier /querysettings  
verifier /volatile /flags <flags>  
verifier /volatile /adddriver <name> [<name>...]  
verifier /volatile /removedriver <name> [<name>...]  
verifier /volatile /faults [<probability> [<tags> [<applications>]]  
verifier /reset  
verifier /query  
verifier /log <LogFileName> [/interval <seconds>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|\<flags>|Deve ser um número em decimal ou hexadecimal, combinação de bits:<br /><br />-   **Valor: descrição**<br />-   **bit 0:** verificação de pool especial<br />-   **bit 1:** forçar verificação irql<br />-   **bit 2:** baixa a simulação de recursos<br />-   **bit 3:** rastreamento de pool<br />-   **4 de bits:** Verificação de e/s<br />-   **bit 5:** detecção de deadlock<br />-   **bit 6:** não utilizados<br />-   **7 de bits:** Verificação de DMA<br />-   **8 de bits:** verificações de segurança<br />-   **bit 9:** forçar solicitações de e/s pendentes<br />-   **10 de bits:** Registro em log o IRP<br />-   **11 de bits:** verificações diversas<br /><br />Por exemplo, **/flags 27** é equivalente a **/flags 0x1B**|  
|/volatile|Usado para alterar as configurações do verificador dinamicamente sem reiniciar o sistema. Todas as novas configurações serão perdidas quando o sistema for reiniciado.|  
|\<probabilidade >|Número entre 1 e 10.000 que especifica a probabilidade de injeção de falha. Por exemplo, a especificação de 100 significa uma probabilidade de injeção de falha de % 1 (100/10.000).<br /><br />Se esse parâmetro não for especificado, em seguida, a probabilidade de padrão de 6% será usada.|  
|\<tags>|Especifica as marcas de pool que serão injetadas com falha, separados por caracteres de espaço. Se esse parâmetro não for especificado, em seguida, qualquer alocação de pool pode ser injetada com falhas.|  
|\<applications>|Especifica o nome do arquivo de imagem dos aplicativos que serão injetados com falha, separados por caracteres de espaço. Se esse parâmetro não for especificado, em seguida, simulação de recursos baixos pode ocorrer em qualquer aplicativo.|  
|\<minutes>|Um número positivo que especifica o comprimento do período após a reinicialização, em minutos, durante o qual nenhuma falha de injeção ocorrerá. Se esse parâmetro não for especificado, em seguida, o tamanho padrão de 8 minutos será usado.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  