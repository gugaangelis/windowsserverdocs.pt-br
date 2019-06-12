---
title: PowerShell
description: Saiba como abrir o console do PowerShell em um prompt de comando.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1e2ccf6187e4480f94b30632b6f8f9f092052541
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811082"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell é um shell de linha de comando baseado em tarefa e linguagem de script criado especialmente para administração do sistema. Baseado no .NET Framework, o Windows PowerShell ajuda os profissionais de TI e os usuários avançados a controlar e automatizar a administração de sistemas operacionais Windows e de aplicativos que são executados no Windows.

O **PowerShell.exe** ferramenta de linha de comando inicia uma sessão do Windows PowerShell em uma janela de Prompt de comando. Quando você usa **PowerShell.exe**, você pode usar seus parâmetros opcionais para personalizar a sessão. Por exemplo, você pode iniciar uma sessão que usa uma política de execução específico ou uma que exclui um perfil do Windows PowerShell. Caso contrário, a sessão é o mesmo que qualquer sessão que é iniciada no console do Windows PowerShell.

## <a name="using-powershellexe"></a>Usando PowerShell.exe

Você pode usar o **PowerShell.exe** ferramenta de linha de comando para iniciar uma sessão do Windows PowerShell em uma janela de Prompt de comando.

- Para iniciar uma sessão do Windows PowerShell em uma janela de prompt de comando, digite `PowerShell`. Um **PS** prefixo é adicionado ao prompt de comando para indicar que você está em uma sessão do Windows PowerShell.

- Para iniciar uma sessão com uma política de execução específico, use o **ExecutionPolicy** parâmetro.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para iniciar uma sessão do Windows PowerShell sem os perfis do Windows PowerShell, use o **NoProfile** parâmetro.

    ```
    PowerShell.exe -NoProfile
    ```
  
- Para iniciar uma sessão, use o **ExecutionPolicy** parâmetro.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- Para ver o PowerShell.exe no arquivo de Ajuda, use o seguinte formato de comando.  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- Para encerrar uma sessão do Windows PowerShell em uma janela de Prompt de comando, digite `exit`. Retorna o prompt de comando típico.

Para obter uma lista completa da **PowerShell.exe** parâmetros de linha de comando, consulte [about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Outras maneiras de iniciar o Windows PowerShell

Para obter informações sobre outras maneiras de iniciar o Windows PowerShell, consulte [iniciando o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentários

Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, os recursos que exigem que um usuário de gráfico de interface, como o [Windows PowerShell Integrated Scripting ISE (ambiente)](https://technet.microsoft.com/library/hh849182)e o [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) e [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448)cmdlets, não são executados em instalações Server Core.

## <a name="additional-references"></a>Referências adicionais

[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[scripts com o Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Consulte também