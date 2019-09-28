---
title: Scripts e exemplos do DiskPart
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c40ce79664795297af4369e35cbda7422617e6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377847"
---
# <a name="diskpart-scripts-and-examples"></a>Scripts e exemplos do DiskPart

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use o DiskPart `/s` para executar scripts que automatizam tarefas de disco @ no__t-1related, como a criação de volumes ou a conversão de discos em discos dinâmicos. O script dessas tarefas será útil se você implantar o Windows usando a instalação autônoma ou a ferramenta Sysprep, que não oferece suporte à criação de volumes diferentes do volume de inicialização.  
  
-   Para criar um script do DiskPart, crie um arquivo de texto que contenha os comandos do DiskPart que você deseja executar, com um comando por linha e nenhuma linha vazia. Você pode iniciar uma linha com `rem` para transformar a linha em um comentário.  
  
    por exemplo, aqui está um script que apaga um disco e, em seguida, cria uma partição de 300 MB para o ambiente de recuperação do Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Para executar um script do DiskPart, no prompt de comando, digite o seguinte comando, em que *scriptname* é o nome do arquivo de texto que contém o script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Para redirecionar a saída de script do DiskPart para um arquivo, digite o seguinte comando, em que *logfile* é o nome do arquivo de texto em que o DiskPart grava sua saída.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Ao usar o comando **DiskPart** como parte de um script, recomendamos que você conclua todas as operações do DiskPart juntas como parte de um único script do DiskPart. Você pode executar scripts do DiskPart consecutivos, mas deve permitir pelo menos 15 segundos entre cada script para um desligamento completo da execução anterior antes de executar o comando **DiskPart** novamente em scripts sucessivos. Caso contrário, os scripts sucessivos podem falhar. Você pode adicionar uma pausa entre scripts do DiskPart consecutivos adicionando o comando `timeout /t 15` ao arquivo em lotes junto com os scripts do DiskPart.  
  
Quando o DiskPart é iniciado, a versão do DiskPart e o nome do computador são exibidos no prompt de comando. Por padrão, se o DiskPart encontrar um erro ao tentar executar uma tarefa com script, o DiskPart interromperá o processamento do script e exibirá um código de erro \(unless você especificou o parâmetro **noerr** @ no__t-2. No entanto, o DiskPart sempre retorna erros quando encontra erros de sintaxe, independentemente de você ter usado o parâmetro **noerr** . O parâmetro **noerr** permite que você execute tarefas úteis, como usar um único script para excluir todas as partições em todos os discos, independentemente do número total de discos.  
  
## <a name="see-also"></a>Consulte também  
[Nova Configurar partições de disco rígido UEFI @ no__t-0gpt @ no__t-1Based usando o Windows PE e o DiskPart @ no__t-2  
[Nova Configurar partições de disco rígido do BIOS @ no__t-0MBR @ no__t-1Based usando o Windows PE e o DiskPart @ no__t-2  
[Cmdlets de armazenamento no Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

