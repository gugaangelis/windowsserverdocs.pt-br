---
title: attach vdisk
description: O tópico de comandos do Windows para **Attach VDISK**, que anexa (às vezes, chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3a903ed231e34ac902ce10b5342f27e772ac89f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851259"
---
# <a name="attach-vdisk"></a>attach vdisk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Anexa (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. Se o VHD já tiver uma partição de disco e um volume de sistema de arquivos quando você anexá-lo, o volume interno do VHD será atribuído a uma letra de unidade

> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| readonly | Anexa o VHD como somente leitura. Qualquer operação de gravação retorna um erro. |
| `sd=<SDDL string>` | Define o filtro de usuário no VHD. A cadeia de caracteres de filtro deve estar no formato SDDL (Security Descriptor Definition Language). Por padrão, o filtro de usuário permite acesso como em um disco físico. As cadeias de caracteres SDDL podem ser complexas, mas em sua forma mais simples, um descritor de segurança que protege o acesso é conhecido como DACL (lista de controle de acesso discricionário). Ele está no formato: `D:<dacl_flags><string_ace1><string_ace2>`... `<string_acen>`<p>Os sinalizadores comuns de DACL são:<p>- **um**. Permitir acesso<p>- **D**. Negar acesso<p>Os direitos comuns são:<p>- **GA**. Todo o acesso<p>- **gr**. Acesso de leitura<p>- **GW**. Acesso de gravação<p>As contas de usuário comuns são:<p>**BA**- . Administradores internos<p>- **au**. Usuários autenticados<p>- **co**. Proprietário criador<p>- **WD**. Todos<p>Exemplos:<p>**D:P: (A;; GR;;; AU** fornece acesso de leitura a todos os usuários autenticados.<p>**D:P: (A;; GA;;; O WD** dá a todos acesso completo. |
| usefilesd | Especifica que o descritor de segurança no arquivo. vhd deve ser usado no VHD. Se o parâmetro **Usefilesd** não for especificado, o VHD não terá um descritor de segurança explícito, a menos que seja especificado com o parâmetro **SD** . |
| NOERR | Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

- Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Para anexar o VHD selecionado como somente leitura, digite:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Compact vdisk](compact-vdisk.md)

- [detalhes do VDISK](detail-vdisk.md)

- [desanexar vdisk](detach-vdisk.md)

- [expandir vdisk](expand-vdisk.md)

- [Mesclar vdisk](merge-vdisk.md)

- [selecionar vdisk](select-vdisk.md)

- [lista](list_1.md)
