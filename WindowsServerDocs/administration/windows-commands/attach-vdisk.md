---
title: attach vdisk
description: Tópico de comandos do Windows para **Attach VDISK** – anexações (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382644"
---
# <a name="attach-vdisk"></a>attach vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. Se o VHD já tiver uma partição de disco e um volume de sistema de arquivos quando você anexá-lo, o volume interno do VHD será atribuído a uma letra de unidade
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     readonly     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             anexa o VHD como somente leitura. Qualquer operação de gravação retorna um erro.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SD = <SDDL string> | Define o filtro de usuário no VHD. A cadeia de caracteres de filtro deve estar no formato SDDL (Security Descriptor Definition Language). Por padrão, o filtro de usuário permite acesso como em um disco físico.<br /><br />As cadeias de caracteres SDDL podem ser complexas, mas em sua forma mais simples, um descritor de segurança que protege o acesso é conhecido como DACL (lista de controle de acesso discricionário). Ele está no formato: D: < dacl_flags > < string_ace1 > < string_ace2 >... < string_acen ><br /><br />Os sinalizadores comuns de DACL são:<br /><br />-   **A** permitir acesso<br />-   **D** negar acesso<br /><br />Os direitos comuns são:<br /><br />-   **GA** todos os acessos<br />acesso de leitura de -   **gr**<br />acesso de gravação de -   **GW**<br /><br />As contas de usuário comuns são:<br /><br />-   **BA** administradores internos<br />usuários autenticados do -   **au**<br />proprietário do**cocreator @no__t** -0<br />-   **WD** -Everyone<br /><br />Exemplos:<br /><br />**D:P: (A;; GR;;; AU** fornece acesso de leitura a todos os usuários autenticados<br /><br />**D:P: (A;; GA;;; O WD** dá a todos o acessador completo |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Especifica que o descritor de segurança no arquivo. vhd deve ser usado no VHD. Se o parâmetro **Usefilesd** não for especificado, o VHD não terá um descritor de segurança explícito, a menos que seja especificado com o parâmetro **SD** .                                                                                                                                                                                                                                                                                                                                                                                          |
|      NOERR       |                                                                                                                                                                                                                                                                                                                                                                                                           Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentários
- Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.
  ## <a name="BKMK_Examples"></a>Disso
  Para anexar o VHD selecionado como somente leitura, digite:
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [Compact vdisk](compact-vdisk.md)

- [detalhes do VDISK](detail-vdisk.md)
- [Desanexar vdisk](detach-vdisk.md)
- [expandir vdisk](expand-vdisk.md)
- [Mesclar vdisk](merge-vdisk.md)
- [selecionar vdisk](select-vdisk.md)
- [list_1](list_1.md)
