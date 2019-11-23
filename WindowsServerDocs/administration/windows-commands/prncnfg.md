---
title: prncnfg
description: Saiba como configurar uma impressora usando o comando prncfg.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5cbbf82e832c50d168e0bef06b2b7c3022dd90e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372143"
---
# <a name="prncnfg"></a>prncnfg

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura ou exibe informações de configuração sobre uma impressora.

## <a name="syntax"></a>Sintaxe
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|-g|Exibe informações de configuração sobre uma impressora.|
|-t|Configura uma impressora.|
|-x|Renomeia uma impressora.|
|-S \<ServerName\>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|
|-P \<PrinterName\>|Especifica o nome da impressora que você deseja gerenciar. Necessário.|
|-z \<NewprinterName\>|Especifica o novo nome da impressora. Requer os parâmetros **-x** e **-P** .|
|-u \<nome de usuário\>-w \<senha\>|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione.|
|-r \<PortName\>|Especifica a porta onde a impressora está conectada. Se essa for uma porta paralela ou serial, use a ID da porta (por exemplo, LPT1 ou COM1). Se esta for uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada.|
|-l \<local\>|Especifica o local da impressora, como "copiar sala".|
|-h \<ShareName\>|Especifica o nome de compartilhamento da impressora.|
|-m \<comentário\>|Especifica a cadeia de caracteres de comentário da impressora.|
|-f \<SeparatorFileName\>|Especifica um arquivo que contém o texto que aparece na página separadora.|
|-y \<tipo de dados\>|Especifica os tipos de dados que a impressora pode aceitar.|
|-St \<StartTime\>|Configura a impressora para a disponibilidade limitada. Especifica a hora do dia em que a impressora está disponível. Se você enviar um documento para uma impressora quando ela não estiver disponível, o documento será mantido (colocado em spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**.|
|-UT \<EndTime\>|Configura a impressora para a disponibilidade limitada. Especifica a hora do dia em que a impressora não está mais disponível. Se você enviar um documento para uma impressora quando ela não estiver disponível, o documento será mantido (colocado em spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**.|
|-o \<prioridade\>|Especifica uma prioridade que o spooler usa para rotear trabalhos de impressão na fila de impressão. Uma fila de impressão com prioridade mais alta recebe todos os seus trabalhos antes de qualquer fila com prioridade mais baixa.|
|-i \<default\>|Especifica a prioridade padrão atribuída a cada trabalho de impressão.|
|{+&#124;-} compartilhado|Especifica se esta impressora é compartilhada na rede.|
|{+&#124;-} direto|Especifica se o documento deve ser enviado diretamente para a impressora sem ser colocado no spool.|
|{+&#124;-} publicado|Especifica se esta impressora deve ser publicada no Active Directory. Se você publicar a impressora, outros usuários poderão procurá-la com base em sua localização e recursos (como impressão de cores e grampeamento).|
|{+&#124;-} ocultos|Função reservada.|
|{+&#124;-} rawonly|Especifica se somente trabalhos de impressão de dados brutos podem ser colocados em spool nessa fila.|
|{+ &#124; -} na fila|Especifica que a impressora não deve começar a imprimir até que a última página do documento seja colocada no spool. O programa de impressão não está disponível até que o documento termine de ser impresso. No entanto, o uso desse parâmetro garante que todo o documento esteja disponível para a impressora.|
|{+ &#124; -} KeepPrintedJobs|Especifica se o spooler deve reter os documentos depois de serem impressos. Habilitar essa opção permite que um usuário reenvie um documento para a impressora da fila de impressão em vez do programa de impressão.|
|{+ &#124; -} WorkOffline|Especifica se um usuário é capaz de enviar trabalhos de impressão para a fila de impressão se o computador não estiver conectado à rede.|
|{+ &#124; -} EnableDevq|Especifica se os trabalhos de impressão que não correspondem à configuração da impressora (por exemplo, arquivos PostScript em spool em impressoras não-PostScript) devem ser mantidos na fila em vez de serem impressos.|
|{+ &#124; -} DoCompleteFirst|Especifica se o spooler deve enviar trabalhos de impressão com uma prioridade mais baixa que concluiu o spooling antes de enviar trabalhos de impressão com uma prioridade mais alta que não concluiu o spooling. Se essa opção estiver habilitada e nenhum documento tiver concluído o spool, o spooler enviará documentos maiores antes dos menores. Você deve habilitar essa opção se quiser maximizar a eficiência da impressora no custo da prioridade do trabalho. Se essa opção estiver desabilitada, o spooler sempre enviará trabalhos de prioridade mais alta para suas respectivas filas primeiro.|
|{+ &#124; -} EnableBIDI|Especifica se a impressora envia informações de status para o spooler.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O comando **prncnfg** é um script Visual Basic localizado no diretório <language> do printing_Admin_Scripts\\do%windir%\system32\. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prncnfg ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Disso
Para exibir informações de configuração para a impressora chamada colorprinter_2 com uma fila de impressão hospedada pelo computador remoto chamado HRServer, digite:
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Para configurar uma impressora chamada colorprinter_2 para que o spooler no computador remoto chamado HRServer Mantenha os trabalhos de impressão depois que eles forem impressos, digite:
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Para alterar o nome de uma impressora no computador remoto chamado HRServer de colorprinter_2 para colorprinter 3, digite:
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando de impressão](print-command-reference.md)
