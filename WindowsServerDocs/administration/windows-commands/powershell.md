---
title: PowerShell
description: Saiba como abrir o console do PowerShell em um prompt de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 327ac844bec0e4c89ee1443c193aa628de038dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837399"
---
# <a name="powershell"></a>PowerShell

O Windows PowerShell é um shell de linha de comando baseado em tarefa e uma linguagem de script desenvolvida especialmente para a administração do sistema. Baseado no .NET Framework, o Windows PowerShell ajuda os profissionais de TI e os usuários avançados a controlar e automatizar a administração de sistemas operacionais Windows e de aplicativos que são executados no Windows.

A ferramenta de linha de comando **PowerShell. exe** inicia uma sessão do Windows PowerShell em uma janela de prompt de comando. Ao usar o **PowerShell. exe**, você pode usar seus parâmetros opcionais para personalizar a sessão. Por exemplo, você pode iniciar uma sessão que usa uma política de execução específica ou uma que exclua um perfil do Windows PowerShell. Caso contrário, a sessão será a mesma que qualquer sessão iniciada no console do Windows PowerShell.

## <a name="using-powershellexe"></a>Usando o PowerShell. exe

Você pode usar a ferramenta de linha de comando **PowerShell. exe** para iniciar uma sessão do Windows PowerShell em uma janela de prompt de comando.

- Para iniciar uma sessão do Windows PowerShell em uma janela de prompt de comando, digite `PowerShell`. Um prefixo de **PS** é adicionado ao prompt de comando para indicar que você está em uma sessão do Windows PowerShell.

- Para iniciar uma sessão com uma política de execução específica, use o parâmetro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para iniciar uma sessão do Windows PowerShell sem seus perfis do Windows PowerShell, use o parâmetro **NoProfile** .

    ```
    PowerShell.exe -NoProfile
    ```
  
- Para iniciar uma sessão, use o parâmetro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- Para ver o arquivo de ajuda do PowerShell. exe, use o seguinte formato de comando.  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- Para encerrar uma sessão do Windows PowerShell em uma janela de prompt de comando, digite `exit`. O prompt de comando típico retorna.

Para obter uma lista completa dos parâmetros de linha de comando do **PowerShell. exe** , consulte [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Outras maneiras de iniciar o Windows PowerShell

Para obter informações sobre outras maneiras de iniciar o Windows PowerShell, consulte [iniciando o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentários

O Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, os recursos que exigem uma interface gráfica do usuário, como o [ambiente de script integrado do Windows PowerShell (ISE)](https://technet.microsoft.com/library/hh849182)e os cmdlets [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) e [show-Command](https://go.microsoft.com/fwlink/?LinkID=217448) , não são executados em instalações do Server Core.

## <a name="additional-references"></a>Referências adicionais

[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512)
script do [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[com o Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) consulte também