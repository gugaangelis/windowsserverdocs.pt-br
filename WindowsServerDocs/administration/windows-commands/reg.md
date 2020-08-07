---
title: reg
description: Artigo de referência para os comandos reg, que executam operações em valores e informações da subchave do registro em entradas do registro.
ms.topic: article
ms.assetid: c97496b2-d1ff-4887-b5d2-6e1524be465a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18a9f243001758393597f6cc5803dd42cdc32dae
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883969"
---
# <a name="reg"></a>reg

Executa operações em valores e informações da subchave do registro em entradas do registro.

Algumas operações permitem que você exiba ou configure entradas de registro em computadores locais ou remotos, enquanto outros permitem que você configure apenas computadores locais. Usar **reg** para configurar o registro de computadores remotos limita os parâmetros que você pode usar em algumas operações. Verifique a sintaxe e os parâmetros de cada operação para verificar se eles podem ser usados em computadores remotos.

> [!CAUTION]
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do registro ignora as proteções padrão, permitindo que as configurações possam prejudicar o desempenho, danificar o sistema ou até mesmo exigir que você reinstale o Windows. Você pode alterar com segurança a maioria das configurações do registro usando os programas no painel de controle ou no console de gerenciamento Microsoft (MMC). Se você precisar editar o registro diretamente, faça o backup primeiro.

## <a name="syntax"></a>Sintaxe

```
reg add
reg compare
reg copy
reg delete
reg export
reg import
reg load
reg query
reg restore
reg save
reg unload
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [reg add](reg-add.md) | Adiciona uma nova subchave ou entrada ao registro. |
| [reg compare](reg-compare.md) | Compara as entradas ou subchaves do registro especificadas. |
| [reg copy](reg-copy.md) | Copia uma entrada de registro para um local especificado no computador local ou remoto. |
| [reg delete](reg-delete.md) | Exclui uma subchave ou entradas do registro. |
| [reg export](reg-export.md) | Copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores. |
| [reg import](reg-import.md) | Copia o conteúdo de um arquivo que contém subchaves de registro exportadas, entradas e valores no registro do computador local. |
| [reg load](reg-load.md) | Grava subchaves e entradas salvas em uma subchave diferente no registro. |
| [reg query](reg-query.md) | Retorna uma lista da próxima camada de subchaves e entradas localizadas em uma subchave especificada no registro. |
| [reg restore](reg-restore.md) | Grava subchaves e entradas salvas de volta no registro. |
| [reg save](reg-save.md) | Salva uma cópia de subchaves, entradas e valores especificados do registro em um arquivo especificado. |
| [reg unload](reg-unload.md) | Remove uma seção do registro que foi carregado usando a operação **reg Load** . |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
