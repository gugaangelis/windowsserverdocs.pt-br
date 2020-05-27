---
title: cacls
description: Tópico de referência para o comando Cacls. Este comando foi preterido e não tem garantia de suporte em versões futuras do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8602157bf87e523d6d842d5636031c61b52e8ef4
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819246"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> Este comando foi preterido. Em vez disso, use o [icacls](icacls.md) .

Exibe ou modifica as listas de controle de acesso discricional (DACL) nos arquivos especificados.

## <a name="syntax"></a>Sintaxe

```
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<filename>` | Obrigatórios. Exibe ACLs de arquivos especificados. |
| /t | Altera as ACLs de arquivos especificados no diretório atual e em todos os subdiretórios. |
| /m | Altera as ACLs de volumes montados em um diretório. |
| /l | Funciona no próprio link simbólico, em vez do destino. |
| /s: SDDL | Substitui as ACLs pelas especificadas na cadeia de caracteres SDDL. Esse parâmetro não é válido para uso com os parâmetros **/e**, **/g**, **/r**, **/p**ou **/d** . |
| /e | Edite uma ACL em vez de substituí-la. |
| /c | Continuar após os erros de acesso negado. |
| `/g user:<perm>` | Concede direitos de acesso de usuário especificados, incluindo estes valores válidos para a permissão:<ul><li>**n** -nenhum</li><li>**r** -ler</li><li>**w** -gravar</li><li>**c** -alterar (gravação)</li><li>**f** -controle total</li></ul> |
| /r usuário [...] | Revogar os direitos de acesso do usuário especificado. Válido somente quando usado com o parâmetro **/e** . |
| `[/p user:<perm> [...]` | Substitua os direitos de acesso do usuário especificado, incluindo estes valores válidos para a permissão:<ul><li>**n** -nenhum</li><li>**r** -ler</li><li>**w** -gravar</li><li>**c** -alterar (gravação)</li><li>**f** -controle total</li></ul> |
| [/d usuário [...] | Negar acesso de usuário especificado. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="sample-output"></a>Saída de exemplo

| Saída | A ACE (entrada de controle de acesso) aplica-se a |
-------- | ------------------------------------- |
| OI | Herança de objeto. Esta pasta e arquivos. |
| CI | Herança de contêiner. Esta pasta e subpastas. |
| IO | Herdar somente. A ACE não se aplica ao arquivo/diretório atual. |
| Nenhuma mensagem de saída | Somente esta pasta. |
| Oi CIS | Esta pasta, subpastas e arquivos. |
| Oi CIS I | Somente subpastas e arquivos. |
| CIS I | Somente subpastas. |
| Oi I | Somente arquivos. |

#### <a name="remarks"></a>Comentários

- Você pode usar curingas (**?** e **&#42;**) para especificar vários arquivos.

- Você pode especificar mais de um usuário.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [icacls](icacls.md)
