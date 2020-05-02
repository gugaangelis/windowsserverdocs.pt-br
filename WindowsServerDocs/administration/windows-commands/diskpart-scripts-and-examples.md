---
title: Scripts e exemplos do DiskPart
description: Tópico de referência para scripts do DiskPart e exemplos sobre como automatizar tarefas relacionadas ao disco, como a criação de volumes ou a conversão de discos em discos dinâmicos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74cefe87fbbeaa1f2bacccbe4addeff952b2c432
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719481"
---
# <a name="diskpart-scripts-and-examples"></a>Scripts e exemplos do DiskPart

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use o `/s` Diskpart para executar scripts que automatizam tarefas relacionadas a disco, como a criação de volumes ou a conversão de discos em discos dinâmicos. O script dessas tarefas será útil se você implantar o Windows usando a instalação autônoma ou a ferramenta Sysprep, que não oferece suporte à criação de volumes diferentes do volume de inicialização.  
  
-   Para criar um script do DiskPart, crie um arquivo de texto que contenha os comandos do DiskPart que você deseja executar, com um comando por linha e nenhuma linha vazia. Você pode iniciar uma linha com `rem` para transformar a linha em um comentário.  
  
    por exemplo, aqui está um script que apaga um disco e, em seguida, cria uma partição de 300 MB para o ambiente de recuperação do Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label=Windows RE tools  
    assign letter=T  
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
> Ao usar o comando **DiskPart** como parte de um script, recomendamos que você conclua todas as operações do DiskPart juntas como parte de um único script do DiskPart. Você pode executar scripts do DiskPart consecutivos, mas deve permitir pelo menos 15 segundos entre cada script para um desligamento completo da execução anterior antes de executar o comando **DiskPart** novamente em scripts sucessivos. Caso contrário, os scripts sucessivos podem falhar. Você pode adicionar uma pausa entre scripts do DiskPart consecutivos adicionando `timeout /t 15` o comando ao arquivo em lotes junto com seus scripts do DiskPart.  
  
Quando o DiskPart é iniciado, a versão do DiskPart e o nome do computador são exibidos no prompt de comando. Por padrão, se o DiskPart encontrar um erro ao tentar executar uma tarefa com script, o DiskPart interromperá o processamento do script e exibirá \(um código de erro, a\)menos que você tenha especificado o parâmetro **noerr** . No entanto, o DiskPart sempre retorna erros quando encontra erros de sintaxe, independentemente de você ter usado o parâmetro **noerr** . O parâmetro **noerr** permite que você execute tarefas úteis, como usar um único script para excluir todas as partições em todos os discos, independentemente do número total de discos.  
  
## <a name="additional-references"></a>Referências adicionais
  
- [Exemplo: configurar partições\/de\-disco rígido baseado em UEFI com base em GPT usando o Windows PE e o DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
- [Exemplo: configurar partições\/de\-disco rígido com base em MBR do BIOS usando o Windows PE e o DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
- [Cmdlets de armazenamento no Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

