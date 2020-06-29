---
title: PowerShell
description: Tópico de referência para o comando do PowerShell, que abre o console do PowerShell em um prompt de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e38684943c6c0c9a4371803d7e473c14cbef7a91
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472341"
---
# <a name="powershell"></a>PowerShell

O Windows PowerShell é um shell de linha de comando baseado em tarefa e uma linguagem de script desenvolvida especialmente para a administração do sistema. Baseado no .NET Framework, o Windows PowerShell ajuda os profissionais de TI e os usuários avançados a controlar e automatizar a administração de sistemas operacionais Windows e de aplicativos que são executados no Windows.

## <a name="using-powershellexe"></a>Usando PowerShell.exe

A ferramenta de linha de comando **PowerShell.exe** inicia uma sessão do Windows PowerShell em uma janela de prompt de comando. Ao usar **PowerShell.exe**, você pode usar seus parâmetros opcionais para personalizar a sessão. Por exemplo, você pode iniciar uma sessão que usa uma política de execução específica ou uma que exclua um perfil do Windows PowerShell. Caso contrário, a sessão será a mesma que qualquer sessão iniciada no console do Windows PowerShell.

- Para iniciar uma sessão do Windows PowerShell em uma janela de prompt de comando, digite `PowerShell` . Um prefixo de **PS** é adicionado ao prompt de comando para indicar que você está em uma sessão do Windows PowerShell.

- Para iniciar uma sessão com uma política de execução específica, use o parâmetro **ExecutionPolicy** e digite:

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para iniciar uma sessão do Windows PowerShell sem seus perfis do Windows PowerShell, use o parâmetro **NoProfile** e digite:

    ```powershell
    PowerShell.exe -NoProfile
    ```

- Para iniciar uma sessão, use o parâmetro **ExecutionPolicy** e digite:

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para ver o arquivo de ajuda PowerShell.exe, digite:

    ```powershell
    PowerShell.exe -help
    PowerShell.exe -?
    PowerShell.exe /?
    ```

- Para encerrar uma sessão do Windows PowerShell em uma janela de prompt de comando, digite `exit` . O prompt de comando típico retorna.

### <a name="remarks"></a>Comentários

- Para obter uma lista completa dos parâmetros de linha de comando **PowerShell.exe** , consulte [about_PowerShell.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe).

- Para obter informações sobre outras maneiras de iniciar o Windows PowerShell, consulte [iniciando o Windows PowerShell](https://docs.microsoft.com/powershell/scripting/windows-powershell/starting-windows-powershell).

- O Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, os recursos que exigem uma interface gráfica do usuário, como o [ambiente de script integrado do Windows PowerShell (ISE)](https://docs.microsoft.com/previous-versions//hh849182(v=technet.10))e os cmdlets [Out-GridView](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-gridview) e [show-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Show-Command) , não são executados em instalações do Server Core.

## <a name="additional-references"></a>Referências adicionais

- [about_PowerShell.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe)

- [about_PowerShell_Ise.exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe)

- [Windows PowerShell](https://docs.microsoft.com/powershell/)
