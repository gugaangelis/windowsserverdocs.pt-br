---
title: attach vdisk
description: Artigo de referência para o comando attach VDISK, que anexa (às vezes, chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local.
ms.topic: reference
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf2b5bb435be441410eeb39ca84ad9a2bd2e0736
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029254"
---
# <a name="attach-vdisk"></a>attach vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Anexa (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. Se o VHD já tiver uma partição de disco e um volume de sistema de arquivos quando você anexá-lo, o volume interno do VHD será atribuído a uma letra de unidade

> [!IMPORTANT]
> Você deve escolher e desanexar um VHD para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| readonly | Anexa o VHD como somente leitura. Qualquer operação de gravação retorna um erro. |
| `sd=<SDDL string>` | Define o filtro de usuário no VHD. A cadeia de caracteres de filtro deve estar no formato SDDL (Security Descriptor Definition Language). Por padrão, o filtro de usuário permite acesso como em um disco físico. As cadeias de caracteres SDDL podem ser complexas, mas em sua forma mais simples, um descritor de segurança que protege o acesso é conhecido como DACL (lista de controle de acesso discricionário). Ele usa o formulário: `D:<dacl_flags><string_ace1><string_ace2>` ... `<string_acen>`<p>Os sinalizadores comuns de DACL são:<ul><li>**A**. Permitir o acesso</li><li>**D**. Negar acesso</li></ul>Os direitos comuns são:<ul><li>**GA**. Todo o acesso</li><li>**Gr**. Acesso de leitura</li><li> **GW**. Acesso de gravação</li></ul>As contas de usuário comuns são:<ul><li>**BA**. Administradores internos</li><li>**Au**. usuários autenticados</li><li>**CO**. Proprietário criador</li><li>**WD**. Todos</li></ul>Exemplos:<ul><li>**D:P: (A;; GR;;; AU**. Fornece acesso de leitura a todos os usuários autenticados.</li><li>**D:P: (A;; GA;;; WD**. Concede a todos acesso completo.</li></ul> |
| usefilesd | Especifica que o descritor de segurança no arquivo. vhd deve ser usado no VHD. Se o parâmetro **Usefilesd** não for especificado, o VHD não terá um descritor de segurança explícito, a menos que seja especificado com o parâmetro **SD** . |
| NOERR | Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para anexar o VHD selecionado como somente leitura, digite:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [select vdisk](select-vdisk.md)

- [compact vdisk](compact-vdisk.md)

- [detail vdisk](detail-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [expand vdisk](expand-vdisk.md)

- [merge vdisk](merge-vdisk.md)

- [list](./list.md)