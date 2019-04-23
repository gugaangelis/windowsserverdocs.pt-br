---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: reparação fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846677"
---
# <a name="fsutil-repair"></a>reparação fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra e monitora operações de reparo de auto-recuperação do NTFS.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|Enumerar|Enumera os inteiros de log de corrupção do volume.|
|\<volumepath>|Especifica o volume como o nome da unidade seguido por dois-pontos.|
|\<LogName>|$Corromper - o conjunto de corrupções confirmadas no volume.<br />Verifique se $ – um conjunto de verificados, potenciais danos no volume.|
|Iniciar|Inicia a auto-recuperação do NTFS.|
|\<FileReference>|Especifica a ID de arquivo específico de volume NTFS (número de referência de arquivo). A referência de arquivo inclui o número de segmento do arquivo.|
|consulta|Consulta o estado de auto-recuperação do volume NTFS.|
|set|Define o estado de auto-recuperação do volume.|
|\<Sinalizadores >|Especifica o método de reparo a ser usado ao definir o estado de auto-recuperação do volume.<br /><br />O **sinalizadores** parâmetro pode ser definido como três valores:<br /><br />-   **0x01**: Permite que o reparo geral.<br />-   **0x09**: Avisa sobre uma possível perda de dados sem o reparo.<br />-   **0x00**: Desabilita e operações de reparo de auto-recuperação do NTFS.|
|Estado|Consulta o estado de corrupção do sistema ou de um determinado volume.|
|Aguarde|Espera aguarda a conclusão. Se NTFS detectou um problema em um volume no qual ele está desempenhando reparos, essa opção permite que o sistema a aguardar até que o reparo seja concluído antes de executar qualquer script pendente.|
|[WaitType {0&#124;1}]|Indica se deve aguardar o reparo atual para concluir ou para aguardar todos os reparos ser concluída. *WaitType* podem ser definidas com os seguintes valores:<br /><br />-   **0**: Aguarda até que todos os reparos ser concluída. (valor padrão)<br />-   **1**: Aguarda até que o reparo atual ser concluída.|

## <a name="remarks"></a>Comentários

-   Autorrecuperação NTFS tenta corrigir corrupções do sistema de arquivos NTFS online, sem exigir **Chkdsk.exe** para ser executado. Esse recurso foi introduzido no Windows Server 2008. Para obter mais informações, consulte [NTFS de recuperação de autoatendimento](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Exemplos

Para enumerar as corrupções confirmadas de um volume, digite:

```
fsutil repair enumerate C: $Corrupt 
```

Para habilitar a auto-recuperação reparo da unidade C, digite:

```
fsutil repair set c: 1
```

Para desabilitar a auto-recuperação reparo da unidade C, digite:

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Autorecuperação do NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


