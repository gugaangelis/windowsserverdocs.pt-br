---
title: Comandos comuns do Git Bash para uso com o GitHub
description: Uma lista de alguns dos comandos usados com mais frequência no Git Bash ao trabalhar com o GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461694"
---
# <a name="common-git-bash-commands"></a>Comandos comuns do Git Bash

Estes são alguns dos comandos mais usados no Git Bash, com base em quando você os usará na criação de conteúdo e a edição do processo.

## <a name="master-branch-related"></a>Mestre relacionados à ramificação

Você sempre deve usar o mestre como sua base para qualquer nova ramificação.

| Comando | Descrição |
|---------|-------------|
| `git checkout master` | Alterne para o mestre de qualquer outro branch |
| `git pull upstream master` | Atualizar sua cópia local do mestre do repositório de produção |

## <a name="branch-related"></a>Relacionado a ramificação

| Comando | Descrição |
|---------|-------------|
| `git branch` | Ver suas ramificações existentes |
| `git checkout -B <name-of-branch>` | Criar um novo branch |
| `git checkout <name-of-branch>` | Alterar para outra ramificação |
| `git status` | Verifique o que está acontecendo em sua ramificação |
| `git branch -D <name-of-branch>` | Excluir uma ramificação existente (tornando-se de que você não estiver nela) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Seleção em relacionados (feito como um grupo, na ordem)

| Comando | Descrição |
|---------|-------------|
| `git add --all` | Depois de salvar seu trabalho, adicione-o para uma ramificação |
| `git commit -m “public comment, including quotes”` | Confirme suas alterações para a ramificação |
| `git pull upstream master` | Atualizar sua cópia local do mestre do repositório de produção |
| `git push origin <name-of-branch>` | Enviar por push para a versão remota do seu branch local |