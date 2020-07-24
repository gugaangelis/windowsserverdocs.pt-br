---
title: PowerShell_ise
description: Artigo de referência para o comando PowerShell_ise, que inicia uma sessão de Ambiente de Script Integrado do Windows PowerShell (ISE).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24fc3c6dca5ba3fea872f625b2ef81f1c78f59fb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956568"
---
# <a name="powershell_ise"></a>PowerShell_ise

O Ambiente de Script Integrado do Windows PowerShell (ISE) é um aplicativo de host gráfico que permite ler, gravar, executar, depurar e testar scripts e módulos em um ambiente assistido por gráfico. Os principais recursos, como IntelliSense, show-Command, trechos de código, preenchimento com Tab, coloração de sintaxe, depuração Visual e ajuda contextual fornecem uma experiência de script rica.

## <a name="using-powershellexe"></a>Usando PowerShell.exe

A ferramenta de **PowerShell_ISE.exe** inicia uma sessão de ISE do Windows PowerShell. Ao usar **PowerShell_ISE.exe**, você pode usar seus parâmetros opcionais para abrir arquivos no ISE do Windows PowerShell ou para iniciar uma sessão de ISE do Windows PowerShell sem perfil ou com um apartamento multi-threaded.

- Para iniciar uma sessão de ISE do Windows PowerShell em uma janela de prompt de comando, no Windows PowerShell ou no menu **Iniciar** , digite:

  ```powershell
  PowerShell_Ise.exe
  ```

- Para abrir um script (. ps1), módulo de script (. psm1), manifesto de módulo (. psd1), arquivo XML ou qualquer outro arquivo com suporte no ISE do Windows PowerShell, digite:

  ```powershell
  PowerShell_Ise.exe <filepath>
  ```

  No Windows PowerShell 3,0, você pode usar o parâmetro de **arquivo** opcional da seguinte maneira:

  ```powershell
  PowerShell_Ise.exe -file <filepath>
  ```

- Para iniciar uma sessão de ISE do Windows PowerShell sem seus perfis do Windows PowerShell, use o parâmetro **NoProfile** . (O parâmetro **NoProfile** é introduzido no Windows PowerShell 3,0.), digite:

  ```powershell
  PowerShell_Ise.exe -NoProfile
  ```

- Para ver o arquivo de ajuda PowerShell_ISE.exe, digite:

    ```powershell
    PowerShell_Ise.exe -help
    PowerShell_Ise.exe -?
    PowerShell_Ise.exe /?
    ```

### <a name="remarks"></a>Comentários

- Para obter uma lista completa dos parâmetros de linha de comando **PowerShell_ISE.exe** , consulte [about_PowerShell_Ise.Exe](/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe).

- Para obter informações sobre outras maneiras de iniciar o Windows PowerShell, consulte [iniciando o Windows PowerShell](/powershell/scripting/windows-powershell/starting-windows-powershell).

- O Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, como ISE do Windows PowerShell requer uma interface gráfica do usuário, ela não é executada em instalações do Server Core.

## <a name="additional-references"></a>Referências adicionais

- [about_PowerShell_Ise.exe](/powershell/module/microsoft.powershell.core/about/about_powershell_exe)
