---
title: Comandos comuns do git bash para uso com o GitHub
description: Uma lista de alguns dos comandos usados com mais frequência no git bash ao trabalhar com o GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865002"
---
# <a name="common-git-bash-commands"></a>Comandos comuns do git bash

Esses são alguns dos comandos mais usados no git bash, com base em quando você os usará em seu processo de criação e edição de conteúdo.

## <a name="master-branch-related"></a>Relacionado à ramificação mestre

Você sempre deve usar o mestre como sua base para qualquer nova ramificação.

| Comando | Descrição |
|---------|-------------|
| `git checkout master` | Alternar para o mestre de qualquer outra ramificação |
| `git pull upstream master` | Atualizar a cópia local do mestre por meio do repositório de produção |

## <a name="branch-related"></a>Relacionado à ramificação

| Comando | Descrição |
|---------|-------------|
| `git branch` | Consulte seus branches existentes |
| `git checkout -B <name-of-branch>` | Criar uma nova ramificação |
| `git checkout <name-of-branch>` | Alterar para outra ramificação |
| `git status` | Verificar o que está acontecendo em seu Branch |
| `git branch -D <name-of-branch>` | Excluir uma ramificação existente (certificando-se de que você não está nela) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Relacionado ao check-in (feito como um grupo, em ordem)

| Comando | Descrição |
|---------|-------------|
| `git add --all` | Depois de salvar seu trabalho, adicione-o a uma ramificação |
| `git commit -m “public comment, including quotes”` | Confirmar suas alterações em seu Branch |
| `git pull upstream master` | Atualizar a cópia local do mestre por meio do repositório de produção |
| `git push origin <name-of-branch>` | Enviar por push para a versão remota do seu Branch local |