---
title: Id do conjunto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da870c4a9676a08070e22f5391164af0bffd4df0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441345"
---
# <a name="set-id"></a>Id do conjunto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de Diskpart definir ID altera o campo de tipo de partição para a partição com foco.  
  
> [!IMPORTANT]  
> Esse comando é destinado ao uso por fabricantes de equipamento original \(OEMs\) apenas. Alterar campos de tipo de partição com esse parâmetro pode causar o computador falhar ou ser impossível a inicialização. A menos que você for um OEM ou experiência com discos gpt, você não deve alterar os campos de tipo de partição em discos gpt por meio desse parâmetro. Em vez disso, sempre use a [criar partição efi](create-partition-efi.md) comando para criar partições de sistema EFI, a [criar partição msr](create-partition-msr.md) comando para criar partições Microsoft Reserved e o [criar partição primária](create-partition-primary.md) comando sem o parâmetro de ID para criar partições primárias em discos gpt.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro |                                                                                                                                                                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       para o registro mestre de inicialização \(MBR\) discos, especifica o novo valor para o campo de tipo, em formato hexadecimal, para a partição. O byte de qualquer tipo de partição pode ser especificado com esse parâmetro, exceto pelo tipo 0x42, que especifica uma partição LDM. Observe que o 0x à esquerda for omitida, ao especificar o tipo de partição hexadecimal.                                                                                                                                                                                                       |
|  <GUID>   | para a tabela de partição GUID \(gpt\) discos, especifica o novo valor GUID para o campo de tipo para a partição. GUIDs reconhecidos incluem:<br /><br />-Partição do sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partição de dados básica: ebd0a0a2\-b9e5\-4433\-87c 0\-68b6b72699c7<br /><br />Qualquer tipo de partição GUID pode ser especificado com esse parâmetro, exceto o seguinte:<br /><br />-Partição Microsoft Reserved: e3c9e316\-0b5c\-4db8\-1!d 817\-f92df00215ae<br />-Partição de metadados de LDM em um disco: 5808c8aa dinâmico\-7e8f\-42e0\-1!d 85 2\-e1e90434cfb3<br />-Partição de dados LDM em um disco dinâmico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Partição de metadados do cluster: db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1 |
| Substituir  |                                                                força o sistema de arquivos no volume ser desmontado antes de alterar o tipo de partição. Quando você executa o **id do conjunto de** de comando, o DiskPart tenta bloquear e desmontar o sistema de arquivos no volume. Se **substituir** não for especificado, e a chamada para bloquear o sistema de arquivos falhar \(por exemplo, porque não há um identificador aberto\), a operação falhará. Quando **substituir** for especificado, o DiskPart força a desmontagem, mesmo se a chamada para bloquear o sistema de arquivos falhar, e os identificadores abertos para o volume se tornarão inválidos.<br /><br />Esse comando só está disponível para Windows 7 e Windows Server 2008 R2.                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>Comentários  
  
-   Além das restrições mencionadas anteriormente, o DiskPart não verifica a validade do valor que você especifique \(exceto garantir que ele é um byte em formato hexadecimal ou um GUID\).  
  
-   Esse comando não funciona em discos dinâmicos ou em partições Microsoft Reserved.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para definir o campo de tipo para 0x07 e forçar o sistema de arquivos para desmontar, digite:  
  
```  
set id=0x07 override  
```  
  
Para definir o campo de tipo para ser uma partição de dados básica, digite:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

