---
title: PowerShell_ise
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5619396e29b446dbc6804ece7444f355dae4c0a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436305"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell Integrated Scripting ISE (ambiente) é um aplicativo host gráfica que permite que você ler, gravar, executar, depurar e testar scripts e módulos em um ambiente assistido por gráfico. Principais recursos como IntelliSense, Show-Command, trechos de código, o preenchimento com tab, coloração de sintaxe, o visual de depuração e ajuda contextual proporcionam uma experiência avançada de script.

O **PowerShell_ISE.exe** ferramenta inicia uma sessão do Windows PowerShell ISE. Quando você usa **PowerShell_ISE.exe**, você pode usar seus parâmetros opcionais para abrir arquivos no Windows PowerShell ISE ou iniciar uma sessão do Windows PowerShell ISE sem o perfil ou com um multi-threaded apartment.

**PowerShell_ISE.exe** foi introduzido no Windows PowerShell 2.0 e expandido significativamente no Windows PowerShell 3.0.

## <a name="using-powershelliseexe"></a>Usando PowerShell_ISE.exe

Você pode usar **PowerShell_ISE.exe** para iniciar e encerrar uma sessão do Windows PowerShell da seguinte maneira:
- Para iniciar uma sessão do Windows PowerShell ISE, em uma janela de Prompt de comando, no Windows PowerShell, ou no menu Iniciar, digite:  
  ```
  PowerShell_Ise
  ```  
- Para abrir um script (. ps1), o módulo de script (. psm1), o manifesto de módulo (. psd1), o arquivo XML ou qualquer outro arquivo com suporte no Windows PowerShell ISE, use o seguinte formato de comando:  
  ```
  PowerShell_Ise <FilePath>
  ```  
  No Windows PowerShell 3.0, você pode usar a opção **arquivo** parâmetro da seguinte maneira:  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- Para iniciar uma sessão do Windows PowerShell ISE sem os perfis do Windows PowerShell, use o **NoProfile** parâmetro. (O **NoProfile** parâmetro é introduzido no Windows PowerShell 3.0.)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- Para ver os **PowerShell_ISE.exe** ajuda de arquivos em uma janela de Prompt de comando, use o seguinte formato de comando:  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  Para obter uma lista completa da **PowerShell_ISE.exe** parâmetros de linha de comando, consulte [about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Iniciar o Windows PowerShell ISE de outras maneiras

Para obter informações sobre outras maneiras de iniciar o Windows PowerShell ISE, consulte [iniciando o Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentários

Windows PowerShell é executado na opção de instalação Server Core dos sistemas operacionais Windows Server. No entanto, como o Windows PowerShell ISE requer uma interface gráfica do usuário, ele não é executado em instalações Server Core.

## <a name="additional-references"></a>Referências adicionais

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[scripts com o Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Consulte também