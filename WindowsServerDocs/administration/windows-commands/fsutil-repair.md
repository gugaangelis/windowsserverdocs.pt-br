---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: Fsutil Repair
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6de102daed2bedfeb41da16e06b7db483af70da0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844189"
---
# <a name="fsutil-repair"></a>Fsutil Repair
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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|enumera|Enumera os inteiros do log de corrupção de um volume.|
|\<VolumePath >|Especifica o volume como o nome da unidade seguido por dois-pontos.|
|\<LogName >|$Corrupt-o conjunto de corrupções confirmadas no volume.<br />$Verify-um conjunto de possíveis danos não verificados no volume.|
|início|Inicia a auto-recuperação do NTFS.|
|\<FileReference >|Especifica a ID de arquivo específica do volume NTFS (número de referência do arquivo). A referência de arquivo inclui o número de segmento do arquivo.|
|query|Consulta o estado de auto-recuperação do volume NTFS.|
|set|Define o estado de auto-recuperação do volume.|
|Sinalizadores de \<>|Especifica o método de reparo a ser usado ao definir o estado de auto-recuperação do volume.<p>O parâmetro **flags** pode ser definido como três valores:<p>-   **0x01**: habilita o reparo geral.<br />-   **0x09**: avisa sobre potencial perda de dados sem reparo.<br />-   **0x00**: desabilita as operações de reparo de auto-recuperação do NTFS.|
|state|Consulta o estado de corrupção do sistema ou de um determinado volume.|
|esperado|Aguarda a conclusão de reparo (s). Se o NTFS tiver detectado um problema em um volume no qual está executando reparos, essa opção permitirá que o sistema aguarde até que o reparo seja concluído antes de executar qualquer script pendente.|
|[Waittype {0&#124;1}]|Indica se deve aguardar a conclusão do reparo atual ou aguardar a conclusão de todos os reparos. *Waittype* pode ser definido com os seguintes valores:<p>-   **0**: aguarda a conclusão de todos os reparos. (valor padrão)<br />-   **1**: aguarda a conclusão do reparo atual.|

## <a name="remarks"></a>Comentários

-   O NTFS de auto-recuperação tenta corrigir as corrupções do sistema de arquivos NTFS online, sem a necessidade de executar **chkdsk. exe** . Esse recurso foi introduzido no Windows Server 2008. Para obter mais informações, consulte auto-recuperação do [NTFS](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para enumerar os danos confirmados de um volume, digite:

```
fsutil repair enumerate C: $Corrupt 
```

Para habilitar o reparo de auto-recuperação na unidade C, digite:

```
fsutil repair set c: 1
```

Para desabilitar o reparo de auto-recuperação na unidade C, digite:

```
fsutil repair set c: 0
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[Auto-recuperação do NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


