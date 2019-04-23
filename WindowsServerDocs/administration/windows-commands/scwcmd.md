---
title: Scwcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19d631f97c194a78819491f32955e391d3be5a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883877"
---
# <a name="scwcmd"></a>Scwcmd

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

A ferramenta de linha de comando Scwcmd.exe incluída com o Assistente de configuração de segurança (ACS) pode ser usada para executar as seguintes tarefas:
-   Configure um ou vários servidores com uma diretiva gerados pelo ACS.
-   Analise um ou vários servidores com uma política gerado pelo ACS.
-   Exibir resultados da análise em formato HTML.
-   Reverta as políticas do ACS.
-   Transforme uma diretiva gerados pelo ACS em arquivos nativos que têm suporte pela diretiva de grupo.
-   Registre uma extensão de banco de dados de configuração de segurança com o ACS.

Quando você usa **scwcmd** para configurar, analisar ou reverter uma política em um servidor remoto, o ACS deve ser instalada no servidor remoto.

## <a name="syntax"></a>Sintaxe

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Parâmetros

|Subcomando|Descrição|
|----------|-----------|
|/ANALYZE|Determina se um computador está em conformidade com uma política.</br>Ver [Scwcmd: analisar](scwcmd-analyze.md) para sintaxe e opções.|
|/configure|Aplica uma política de segurança gerados pelo ACS a um computador.</br>Ver [Scwcmd: configurar](scwcmd-configure.md) para sintaxe e opções.|
|/register|Estende ou personaliza o banco de dados de configuração de segurança do ACS, registrando um arquivo de banco de dados de configuração de segurança que contém a função, tarefa, serviço ou definições de porta.</br>Ver [Scwcmd: registrar](scwcmd-register.md) para sintaxe e opções.|
|/rollback|Aplica-se a política de reversão mais recente disponível e, em seguida, exclui essa diretiva de reversão.</br>Ver [Scwcmd: reversão](scwcmd-rollback.md) para sintaxe e opções.|
|/transform|Transforma um arquivo de política de segurança gerado usando o ACS em um novo objeto de diretiva de grupo (GPO) nos serviços de domínio do Active Directory.</br>Ver [Scwcmd: transformar](scwcmd-transform.md) sintaxe e opções.|
|/View|Renderiza um arquivo. XML usando uma transformação XSL especificado.</br>Ver [Scwcmd: exibição](scwcmd-view.md) para sintaxe e opções.|
|/?|Exibe a ajuda no prompt de comando.|

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
