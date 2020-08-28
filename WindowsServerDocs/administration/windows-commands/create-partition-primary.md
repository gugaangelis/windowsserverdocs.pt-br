---
title: create partition primary
description: Artigo de referência para o comando criar partição primário, que cria uma partição primária no disco básico com foco.
ms.topic: reference
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 236d2b824e712f50d518468a50a359b8670dfbef
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033194"
---
# <a name="create-partition-primary"></a>create partition primary

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição primária no disco básico com foco. Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição.

> [!IMPORTANT]
> Um disco básico deve ser selecionado para que essa operação tenha sucesso. Você deve usar o comando [selecionar disco](select-disk.md) para selecionar um disco básico e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | Especifica o tamanho da partição em megabytes (MB). Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço não alocado na região atual. |
| deslocamento =`<n>` | O deslocamento em kilobytes (KB), no qual a partição é criada. Se nenhum deslocamento for fornecido, a partição será iniciada no início da maior extensão de disco grande o suficiente para contê-la. |
| align =`<n>` | Alinha todas as extensões de partição com o limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN (número de unidade lógica) RAID de hardware para melhorar o desempenho. `<n>` é o número de kilobytes (KB) desde o início do disco até o limite de alinhamento mais próximo. |
| ID = { `<byte>  | <guid>` } | Especifica o tipo de partição. Este parâmetro destina-se somente ao uso do OEM (fabricante original do equipamento). Qualquer byte de tipo de partição ou GUID pode ser especificado com esse parâmetro. O DiskPart não verifica o tipo de partição para fins de validade, exceto para garantir que seja um byte em formato hexadecimal ou GUID. **Cuidado:** A criação de partições com esse parâmetro pode fazer com que o computador falhe ou não possa ser inicializado. A menos que você seja um OEM ou um profissional de ti experiente em discos GPT, não crie partições em discos GPT usando esse parâmetro. Em vez disso, sempre use o comando [Create Partition EFI](create-partition-efi.md) para criar partições de sistema EFI, o comando [Create Partition MSR](create-partition-msr.md) para criar partições reservadas da Microsoft e o comando [criar partição primário](create-partition-primary.md)) (sem o `id={ <byte>  | <guid>` parâmetro) para criar partições primárias em discos GPT.<p>**Para discos MBR (registro mestre de inicialização)**, você deve especificar um byte de tipo de partição, em formato hexadecimal, para a partição. Se esse parâmetro não for especificado, o comando criará uma partição do tipo `0x06` , que especifica que um sistema de arquivos não está instalado. Os exemplos incluem:<ul><li>**Partição de dados LDM:** 0x42</li><li>**Partição de recuperação:** 0x27</li><li>**Partição OEM reconhecida:** 0x12, 0X84, 0XDE, 0XFE, 0XA0</li></ul><p>**Para discos GPT (tabela de partição GUID)**, você pode especificar um GUID de tipo de partição para a partição que deseja criar. Os GUIDs reconhecidos incluem:<ul><li>**Partição do sistema EFI:** C12A7328-F81F-11D2-BA4B-00A0C93EC93B</li><li>**Partição reservada da Microsoft:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partição de dados básica:** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li><li>**Partição de metadados LDM (disco dinâmico):** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partição de dados LDM (disco dinâmico):** AF9B60A0-1431-4F62-BC68-3311714A69AD</li><li>**Partição de recuperação:** de94bba4-06d1-4d40-a16a-bfd50179d6ac<p>Se esse parâmetro não for especificado para um disco GPT, o comando criará uma partição de dados básica.</li></ul> |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem o parâmetro noerr, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para criar uma partição primária de 1000 megabytes de tamanho, digite:

```
create partition primary size=1000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [atribuir comando](assign.md)

- [comando Create](create.md)

- [select disk](select-disk.md)
