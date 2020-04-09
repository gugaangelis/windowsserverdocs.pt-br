---
title: unlodctr
description: Tópico de comandos do Windows para Unlodctr, que remove nomes de contadores de desempenho e explica o texto de um serviço ou driver de dispositivo do registro do sistema
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe7fc3c9eafefd59a5daab625e3af06b6addd292
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832253"
---
# <a name="unlodctr"></a>unlodctr

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove os nomes de **contadores de desempenho** e o texto **explicativo** para um serviço ou driver de dispositivo do registro do sistema.   

## <a name="syntax"></a>Sintaxe  
```  
Unlodctr <DriverName>   
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|\<> de driver|Remove as configurações do nome do contador de desempenho e o texto explicativo para driver ou serviço <DriverName> do registro do Windows Server 2003.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
> [!WARNING]  
> A edição correta do registro pode danificar gravemente o seu sistema. Antes de fazer mudanças no registro, você deve fazer o backup de quaisquer dados importantes no computador.  

Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, <DriverName>).  

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Para remover as configurações atuais do registro de desempenho e o texto explicativo do contador para o serviço SMTP:  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
