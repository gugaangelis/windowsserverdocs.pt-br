---
title: unlodctr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886647"
---
# <a name="unlodctr"></a>unlodctr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove os nomes de contador de desempenho e texto explicativo para um serviço ou driver de dispositivo do registro do sistema.   

## <a name="syntax"></a>Sintaxe  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|\<DriverName>|Remove o desempenho, as configurações de nome de contador e texto explicativo para o driver ou serviço <DriverName> do registro do Windows Server 2003.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
> [!WARNING]  
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.  

Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, "<DriverName>").  

## <a name="BKMK_Examples"></a>Exemplos  
Para remover as configurações de registro de desempenho atuais e texto explicativo para o serviço SMTP Simple Mail Transfer Protocol () do contador:  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
