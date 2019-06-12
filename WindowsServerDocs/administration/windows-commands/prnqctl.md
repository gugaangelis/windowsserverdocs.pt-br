---
title: prnqctl
description: Imprimir uma página de teste, pausar ou retomar uma impressora.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ba58970e76497f6e91c53c73a429eb65a275b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442099"
---
# <a name="prnqctl"></a>prnqctl

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime uma página de teste, faz uma pausa ou retoma uma impressora e limpa uma fila da impressora.  

## <a name="syntax"></a>Sintaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|-z|pausa a impressão da impressora especificada com o **-p** parâmetro.|  
|-m|Retoma a impressão especificado com o **-p** parâmetro.|  
|-e|uma página de teste é impresso na impressora especificada com o **-p** parâmetro.|  
|-x|Cancela todos os trabalhos de impressão da impressora especificada com o **-p** parâmetro.|  
|-s \<ServerName>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|  
|-p \<printerName>|Especifica o nome da impressora que você deseja gerenciar. Obrigatório.|  
|-u \<nome de usuário > -w \<senha >|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
- O **prnqctl** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo prnqctl ou altere os diretórios para a pasta apropriada. Por exemplo:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).  

## <a name="BKMK_examples"></a>Exemplos  
Para imprimir uma página de teste na impressora Laserprinter1 compartilhada pelo \\\Server1 computador, digite:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Para pausar a impressão Laserprinter1 no computador local, digite:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Para cancelar todos os trabalhos de impressão da impressora Laserprinter1 no computador local, digite:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Referência do comando Imprimir](print-command-reference.md)  
