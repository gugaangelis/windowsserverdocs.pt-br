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
ms.openlocfilehash: d07d8caa0568b26f5edc16258085a59ecdafcf4e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837199"
---
# <a name="prnqctl"></a>prnqctl

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

imprime uma página de teste, pausa ou retoma uma impressora e limpa uma fila de impressora.  

## <a name="syntax"></a>Sintaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>Parâmetros  

|Parâmetro|Descrição|  
|-------|--------|  
|-z|pausa a impressão na impressora especificada com o parâmetro **-p** .|  
|-m|Retoma a impressão na impressora especificada com o parâmetro **-p** .|  
|{1&gt;-e&lt;1}|imprime uma página de teste na impressora especificada com o parâmetro **-p** .|  
|{1&gt;-&lt;1}x|Cancela todos os trabalhos de impressão na impressora especificada com o parâmetro **-p** .|  
|-s \<ServerName >|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|  
|-p \<PrinterName >|Especifica o nome da impressora que você deseja gerenciar. Obrigatório.|  
|-u \<nome de usuário >-w \<senha >|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
- O comando **prnqctl** é um script Visual Basic localizado no printing_Admin_Scripts diretório <language>\\do%windir%\system32\. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnqctl ou altere os diretórios para a pasta apropriada. Por exemplo:  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).  

## <a name="examples"></a><a name="BKMK_examples"></a>Disso  
Para imprimir uma página de teste na impressora Laserprinter1 compartilhada pelo computador do \\\Server1, digite:  
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
