---
title: ID do conjunto
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b48cc701716412c4a79cedddb4458c57ba25ad5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384067"
---
# <a name="set-id"></a>ID do conjunto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Set ID do DiskPart altera o campo de tipo de partição para a partição com foco.  
  
> [!IMPORTANT]  
> Este comando destina-se ao uso por fabricantes de equipamentos originais \(OEMs @ no__t-1 somente. A alteração de campos de tipo de partição com esse parâmetro pode fazer com que o computador falhe ou não possa ser inicializado. A menos que você seja um OEM ou tenha experiência com discos GPT, não altere os campos de tipo de partição em discos GPT usando esse parâmetro. Em vez disso, sempre use o comando [Create Partition EFI](create-partition-efi.md) para criar partições de sistema EFI, o comando [Create Partition MSR](create-partition-msr.md) para criar partições reservadas da Microsoft e o comando [Create Partition Primary](create-partition-primary.md) sem o parâmetro ID para Crie partições primárias em discos GPT.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
| Parâmetro |                                                                                                                                                                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       para o registro de inicialização mestre \(MBR @ no__t-1, especifica o novo valor para o campo de tipo, em formato hexadecimal, para a partição. Qualquer byte de tipo de partição pode ser especificado com esse parâmetro, exceto para o tipo 0x42, que especifica uma partição LDM. Observe que o 0x à esquerda é omitido ao especificar o tipo de partição hexadecimal.                                                                                                                                                                                                       |
|  <GUID>   | para tabela de partição GUID \(gpt @ no__t-1 discos, especifica o novo valor de GUID para o campo de tipo da partição. Os GUIDs reconhecidos incluem:<br /><br />-Partição do sistema EFI: c12a7328 @ no__t-0f81f @ no__t-111d2 @ no__t-2ba4b @ no__t-300a0c93ec93b<br />-Partição de dados básica: ebd0a0a2 @ no__t-0b9e5 @ no__t-14433 @ no__t-287c0 @ no__t-368b6b72699c7<br /><br />Qualquer GUID de tipo de partição pode ser especificado com esse parâmetro, exceto o seguinte:<br /><br />-Partição reservada da Microsoft: e3c9e316 @ no__t-00b5c @ no__t-14db8 @ no__t-2817d @ no__t-3f92df00215ae<br />-Partição de metadados LDM em um disco dinâmico: 5808c8aa @ no__t-07e8f @ no__t-142e0 @ no__t-285d2 @ no__t-3e1e90434cfb3<br />-Partição de dados LDM em um disco dinâmico: AF9B60A0 @ no__t-01431 @ no__t-14f62 @ no__t-2bc68 @ no__t-33311714a69ad<br />-Partição de metadados do cluster: db97dba9 @ no__t-00840 @ no__t-14bae @ no__t-297f0 @ no__t-3ffb9a327c7e1 |
| substituição  |                                                                força o sistema de arquivos no volume a ser desmontado antes de alterar o tipo de partição. Quando você executa o comando **set ID** , o DiskPart tenta bloquear e desmontar o sistema de arquivos no volume. Se a **substituição** não for especificada, e a chamada para bloquear o sistema de arquivos falhar \(Para exemplo, porque há um identificador aberto @ no__t-2, a operação falhará. Quando **override** é especificado, o DiskPart força a desmontagem mesmo se a chamada para bloquear o sistema de arquivos falhar e quaisquer identificadores abertos para o volume se tornarão inválidos.<br /><br />Este comando só está disponível para o Windows 7 e o Windows Server 2008 R2.                                                                 |
|   NOERR   |                                                                                                                                                                                                                                                                    Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>Comentários  
  
-   Além das limitações mencionadas anteriormente, o DiskPart não verifica a validade do valor especificado \(except para garantir que seja um byte em formato hexadecimal ou um GUID @ no__t-1.  
  
-   Esse comando não funciona em discos dinâmicos ou em partições reservadas da Microsoft.  
  
## <a name="BKMK_examples"></a>Disso  
Para definir o campo de tipo como 0x07 e forçar o sistema de arquivos a ser desmontado, digite:  
  
```  
set id=0x07 override  
```  
  
Para definir o campo de tipo como uma partição de dados básica, digite:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

