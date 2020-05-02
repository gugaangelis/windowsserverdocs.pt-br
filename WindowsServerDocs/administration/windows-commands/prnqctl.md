---
title: prnqctl
description: Imprima uma página de teste, pause ou retome uma impressora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 377b448eb74f0a27228d8502ce149fb04869895b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722796"
---
# <a name="prnqctl"></a>prnqctl

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime uma página de teste, pausa ou retoma uma impressora e limpa uma fila de impressora.  

## <a name="syntax"></a>Sintaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|-Z|pausa a impressão na impressora especificada com o parâmetro **-p** .|  
|-M|Retoma a impressão na impressora especificada com o parâmetro **-p** .|  
|-E|imprime uma página de teste na impressora especificada com o parâmetro **-p** .|  
|-X|Cancela todos os trabalhos de impressão na impressora especificada com o parâmetro **-p** .|  
|-s \<servername>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|  
|-p \<printername>|Especifica o nome da impressora que você deseja gerenciar. Obrigatórios.|  
|-u \<nome de usuário> \<-w senha>|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
- O comando **prnqctl** é um script Visual Basic localizado no diretório%windir%\system32\ printing_Admin_Scripts\\ <language> . Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnqctl ou altere os diretórios para a pasta apropriada. Por exemplo:   
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Se as informações fornecidas contiverem espaços, use aspas em volta do texto (por exemplo, `"computer Name"`).  

## <a name="examples"></a><a name="BKMK_examples"></a>Disso  
Para imprimir uma página de teste na impressora Laserprinter1 compartilhada pelo computador \\\Server1, digite:  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Para pausar a impressão na impressora Laserprinter1 no computador local, digite:  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Para cancelar todos os trabalhos de impressão na impressora Laserprinter1 no computador local, digite:  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Referência de comando de impressão](print-command-reference.md)  
