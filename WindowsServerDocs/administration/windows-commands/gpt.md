---
title: gpt
description: Artigo de referência para o comando GPT, que atribui os atributos GPT à partição com foco.
ms.topic: reference
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 72260c00830d2d85fbe4324203dcfefe90154c7e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634720"
---
# <a name="gpt"></a>gpt

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em discos básicos da tabela de partição GUID (GPT), esse comando atribui os atributos GPT à partição com foco. Os atributos de partição GPT fornecem informações adicionais sobre o uso da partição. Alguns atributos são específicos para o GUID do tipo de partição.

Você deve escolher uma partição GPT básica para que essa operação tenha sucesso. Use o [comando selecionar partição](select-partition.md) para selecionar uma partição GPT básica e deslocar o foco para ela.

> [!CAUTION]
> Alterar os atributos de GPT pode fazer com que os volumes de dados básicos não recebam letras de unidade ou impeçam que o sistema de arquivos seja montado. É altamente recomendável que você não altere os atributos de GPT, a menos que seja um fabricante de equipamento original (OEM) ou um profissional de ti que tenha experiência com discos GPT.

## <a name="syntax"></a>Sintaxe

```
gpt attributes=<n>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| atributos =`<n>` | Especifica o valor do atributo que você deseja aplicar à partição com foco. O campo atributo GPT é um campo de 64 bits que contém dois subcampos. O campo superior é interpretado apenas no contexto da ID da partição, enquanto o campo inferior é comum a todas as IDs de partição. Os valores aceitos incluem:<ul><li>**0x0000000000000001** -especifica que a partição é exigida pelo computador para funcionar corretamente.</li><li>**0x8000000000000000** -especifica que a partição não receberá uma letra de unidade por padrão quando o disco for movido para outro computador ou quando o disco for visto pela primeira vez por um computador.</li><li>**0x4000000000000000** – oculta o volume de uma partição para que ela não seja detectada pelo Gerenciador de montagem.</li><li>**0x2000000000000000** -especifica que a partição é uma cópia de sombra de outra partição.</li><li>**0x1000000000000000** -especifica que a partição é somente leitura. Esse atributo impede que o volume seja gravado no.</li></ul><p>Para obter mais informações sobre esses atributos, consulte a seção atributos em [Create_PARTITION_PARAMETERS estrutura](/windows/win32/api/vds/ns-vds-create_partition_parameters). |

#### <a name="remarks"></a>Comentários

- A partição do sistema EFI contém somente os binários necessários para iniciar o sistema operacional. Isso torna mais fácil para os binários OEM ou binários específicos de um sistema operacional serem colocados em outras partições.

### <a name="examples"></a>Exemplos

Para impedir que o computador atribua automaticamente uma letra de unidade à partição com foco, ao mover um disco GPT, digite:

```
gpt attributes=0x8000000000000000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de partição](select-partition.md)

- [Estrutura de create_PARTITION_PARAMETERS](/windows/win32/api/vds/ns-vds-create_partition_parameters)
