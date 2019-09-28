---
title: dcgpofix
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2592210ae688f47dcf2d32c7bef560d52223141c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378760"
---
# <a name="dcgpofix"></a>dcgpofix



Recria os objetos de Política de Grupo padrão (GPOs) para um domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                                 Descrição                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | Ignora a versão do Active Directory® do esquema MC</br>Quando você executar esse comando. Caso contrário, o comando só funcionará na mesma versão do esquema que a versão do Windows na qual o comando foi enviado. |
| /Target {domínio |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    Exibe a ajuda no prompt de comando.                                                                                     |

## <a name="remarks"></a>Comentários

-   O comando **Dcgpofix** está disponível no windows Server 2008 R2 e no windows Server 2008, exceto nas instalações do Server Core.
-   Embora o Console de Gerenciamento de Política de Grupo (GPMC) seja distribuído com o Windows Server 2008 R2 e o Windows Server 2008, você deve instalar o Política de Grupo Management como um recurso por meio de Gerenciador do Servidor.

## <a name="BKMK_Examples"></a>Disso

Restaure o GPO de política de domínio padrão para seu estado original. Você perderá todas as alterações feitas nesse GPO. Como prática recomendada, você deve configurar o GPO de política de domínio padrão somente para gerenciar as configurações de políticas de conta padrão, a política de senha, a política de bloqueio de conta e a política Kerberos. Neste exemplo, você ignora a versão do esquema de Active Directory para que o comando **Dcgpofix** não esteja limitado ao mesmo esquema que a versão do Windows na qual o comando foi enviado.
```
dcgpofix /ignoreschema /target:Domain
```
Restaure o GPO de política de controladores de domínio padrão para seu estado original. Você perderá todas as alterações feitas nesse GPO. Como prática recomendada, você deve configurar o GPO de política de controladores de domínio padrão somente para definir direitos de usuário e políticas de auditoria. Neste exemplo, você ignora a versão do esquema de Active Directory para que o comando **Dcgpofix** não esteja limitado ao mesmo esquema que a versão do Windows na qual o comando foi enviado.
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>Referências adicionais

-   [TechCenter do Política de Grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)