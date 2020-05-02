---
title: PowerShell_ise
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fb143c3d365b47f66aee5c64bfdc7dc26e5794f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723287"
---
# <a name="powershell_ise"></a>PowerShell_ise



O Ambiente de Script Integrado do Windows PowerShell (ISE) é um aplicativo de host gráfico que permite ler, gravar, executar, depurar e testar scripts e módulos em um ambiente assistido por gráfico. Os principais recursos, como IntelliSense, show-Command, trechos de código, preenchimento com Tab, coloração de sintaxe, depuração Visual e ajuda contextual fornecem uma experiência de script rica.

A ferramenta **PowerShell_ISE. exe** inicia uma sessão de ISE do Windows PowerShell. Ao usar o **PowerShell_ISE. exe**, você pode usar seus parâmetros opcionais para abrir arquivos no ISE do Windows PowerShell ou para iniciar uma sessão de ISE do Windows PowerShell sem perfil ou com um apartamento multi-threaded.

O **PowerShell_ISE. exe** foi introduzido no windows PowerShell 2,0 e expandido significativamente no windows PowerShell 3,0.

## <a name="using-powershell_iseexe"></a>Usando PowerShell_ISE. exe

Você pode usar **PowerShell_ISE. exe** para iniciar e encerrar uma sessão do Windows PowerShell da seguinte maneira:
- Para iniciar uma sessão de ISE do Windows PowerShell, em uma janela de prompt de comando, no Windows PowerShell ou no menu Iniciar, digite:  
  ```
  PowerShell_Ise
  ```  
- Para abrir um script (. ps1), módulo de script (. psm1), manifesto de módulo (. psd1), arquivo XML ou qualquer outro arquivo com suporte no ISE do Windows PowerShell, use o seguinte formato de comando:  
  ```
  PowerShell_Ise <FilePath>
  ```  
  No Windows PowerShell 3,0, você pode usar o parâmetro de **arquivo** opcional da seguinte maneira:  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- Para iniciar uma sessão de ISE do Windows PowerShell sem seus perfis do Windows PowerShell, use o parâmetro **NoProfile** . (O parâmetro **NoProfile** é introduzido no Windows PowerShell 3,0.)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- Para ver o arquivo de ajuda **PowerShell_ISE. exe** em uma janela de prompt de comando, use o seguinte formato de comando:  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  Para obter uma lista completa dos parâmetros de linha de comando do **PowerShell_ISE. exe** , consulte [about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Iniciar ISE do Windows PowerShell de outras maneiras

Para obter informações sobre outras maneiras de iniciar ISE do Windows PowerShell, consulte [iniciando o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentários

O Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, como ISE do Windows PowerShell requer uma interface gráfica do usuário, ela não é executada em instalações do Server Core.

## <a name="additional-references"></a>Referências adicionais

[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439)
script do[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[com o Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) consulte também