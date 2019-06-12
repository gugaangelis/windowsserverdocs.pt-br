---
title: attach vdisk
description: Tópico de comandos do Windows para **attach vdisk** -anexa (às vezes chamado de montagens ou trazer à tona) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fb33d040ce0b2a7a9d06951a7e80251a0d0da614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435225"
---
# <a name="attach-vdisk"></a>attach vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa (às vezes chamado de montagens ou trazer à tona) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. Se o VHD já tiver uma partição de disco e um volume de sistema de arquivos quando você anexá-lo, o volume interno do VHD será atribuído a uma letra de unidade
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     somente leitura     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             anexa o VHD como somente leitura. Qualquer gravação operação retornará um erro.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sd=<SDDL string> | Define o filtro de usuário no VHD. A cadeia de caracteres de filtro deve ser no formato de linguagem de definição de descritor de segurança (SDDL). Por padrão o filtro de usuário permite o acesso, como em um disco físico.<br /><br />Cadeias de caracteres SDDL podem ser complexas, mas em sua forma mais simples, um descritor de segurança que protege o acesso é conhecido como uma lista de controle de acesso discricionário (DACL). Ele está no formato: Unidade d: < dacl_flags >< string_ace1 >< string_ace2 >... < string_acen ><br /><br />Sinalizadores de DACL comuns são:<br /><br />-   **Um** permitir o acesso<br />-   **1!d** negar acesso<br /><br />Direitos comuns são:<br /><br />-   **GA** todo o acesso<br />-   **GR** acesso de leitura<br />-   **GW** acesso de gravação<br /><br />Contas de usuário comuns são:<br /><br />-   **BA** criados em administradores<br />-   **AU** usuários autenticados<br />-   **CO** proprietário criador<br />-   **WD** -todos<br /><br />Exemplos:<br /><br />**D:P:(A;; GR;; AU** concede acesso de leitura para todos os usuários autenticados<br /><br />**D:P:(A;; GA;; WD** fornece a todos total acesso |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Especifica que o descritor de segurança no arquivo. vhd deve ser usado no VHD. Se o **Usefilesd** parâmetro não for especificado, o VHD não terá um descritor de segurança explícito, a menos que ele é especificado com o **Sd** parâmetro.                                                                                                                                                                                                                                                                                                                                                                                          |
|      noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentários
- Um VHD deve ser selecionado e desanexado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um VHD e mudar o foco a ele.
  ## <a name="BKMK_Examples"></a>Exemplos
  Para anexar o VHD selecionado como somente leitura, digite:
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [Compactar vdisk](compact-vdisk.md)

- [Detalhar vdisk](detail-vdisk.md)
- [Desanexar vdisk](detach-vdisk.md)
- [Expandir vdisk](expand-vdisk.md)
- [Mesclar vdisk](merge-vdisk.md)
- [Selecionar o vdisk](select-vdisk.md)
- [list_1](list_1.md)
