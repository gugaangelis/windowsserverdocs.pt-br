---
title: dcgpofix
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 179d540371870075906bbcbf8ff912e1b883915d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433929"
---
# <a name="dcgpofix"></a>dcgpofix



Recria os objetos de diretiva de grupo (GPOs) padrão para um domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                                 Descrição                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | Ignora a versão do esquema mc Active Directory®</br>Quando você executar esse comando. Caso contrário, o comando só funciona da mesma versão de esquema, como a versão do Windows no qual o comando foi enviado. |
| /target {Domain |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    Exibe a ajuda no prompt de comando.                                                                                     |

## <a name="remarks"></a>Comentários

-   O **dcgpofix** comando está disponível no Windows Server 2008 R2 e Windows Server 2008, exceto em instalações Server Core.
-   Embora o grupo de política de gerenciamento GPMC (Console) é distribuída com o Windows Server 2008 R2 e Windows Server 2008, você deve instalar o gerenciamento de diretiva de grupo como um recurso via Gerenciador do servidor.

## <a name="BKMK_Examples"></a>Exemplos

Restaure o GPO de diretiva de domínio padrão para seu estado original. Você perderá as alterações feitas a esse GPO. Como prática recomendada, você deve configurar o GPO de diretiva de domínio padrão apenas para gerenciar as configurações de diretivas de conta padrão, política de senha, diretiva de bloqueio de conta e política do Kerberos. Neste exemplo, você ignorar a versão do esquema do Active Directory para que o **dcgpofix** comando não é limitado ao mesmo esquema que a versão do Windows no qual o comando foi enviado.
```
dcgpofix /ignoreschema /target:Domain
```
Restaure o GPO de política de controladores de domínio padrão para seu estado original. Você perderá as alterações feitas a esse GPO. Como prática recomendada, você deve configurar os controladores de política de GPO de domínio padrão apenas para definir direitos de usuário e as políticas de auditoria. Neste exemplo, você ignorar a versão do esquema do Active Directory para que o **dcgpofix** comando não é limitado ao mesmo esquema que a versão do Windows no qual o comando foi enviado.
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>Referências adicionais

-   [Group Policy TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)