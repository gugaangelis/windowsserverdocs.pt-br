---
title: Exemplos e Scripts do Diskpart
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862787"
---
# <a name="diskpart-scripts-and-examples"></a>Exemplos e Scripts do Diskpart

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use o Diskpart `/s` para executar scripts que automatizam o disco\-relacionadas a tarefas, como criar volumes ou converter discos para discos dinâmicos. Essas tarefas de script é útil se você implantar o Windows usando a instalação autônoma ou a ferramenta Sysprep, que têm suporte para a criação de volumes que não seja o volume de inicialização.  
  
-   Para criar um script de Diskpart, crie um arquivo de texto que contém os comandos de Diskpart que você deseja executar, com um comando por linha e não linhas vazias. Você pode iniciar uma linha com `rem` para que a linha em um comentário.  
  
    Por exemplo, aqui s um script que apaga um disco e, em seguida, cria uma partição de 300 MB para o ambiente de recuperação do Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Para executar um script de DiskPart, no prompt de comando, digite o comando a seguir, onde *scriptname* é o nome do arquivo de texto que contém o script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Para redirecionar a saída do script do DiskPart para um arquivo, digite o seguinte comando, onde *arquivo de log* é o nome do arquivo de texto onde o DiskPart grava sua saída.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Ao usar o **DiskPart** de comando como parte de um script, recomendamos que você conclua todas as operações do DiskPart juntas como parte de um único script de DiskPart. Você pode executar scripts DiskPart consecutivos, mas você deve permitir pelo menos 15 segundos entre cada script para um desligamento completo da execução anterior antes de executar o **DiskPart** comando novamente em scripts sucessivos. Caso contrário, os scripts sucessivos podem falhar. Você pode adicionar uma pausa entre scripts DiskPart consecutivos, adicionando o `timeout /t 15` comando para o arquivo em lotes, juntamente com seus scripts de DiskPart.  
  
Quando o DiskPart é iniciado, o DiskPart versão e o computador de exibição do nome no prompt de comando. Por padrão, se o DiskPart encontrar um erro ao tentar executar uma tarefa script, DiskPart interrompe o processamento do script e exibe um código de erro \(, a menos que você especificou o **noerr** parâmetro\). No entanto, o DiskPart sempre retorna erros quando encontra erros de sintaxe, independentemente se você tiver usado o **noerr** parâmetro. O **noerr** parâmetro permite que você execute tarefas úteis, como o uso de um único script para excluir todas as partições em todos os discos, independentemente do número total de discos.  
  
## <a name="see-also"></a>Consulte também  
[Exemplo: Configurar a UEFI\/gpt\-partições com base no disco rígido de unidade usando o Windows PE e DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[Exemplo: Configurar o BIOS\/MBR\-com base em partições de disco rígido usando o Windows PE e DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
[Cmdlets de armazenamento no Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

