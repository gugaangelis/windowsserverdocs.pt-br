---
title: ReFSUtil
description: ReFSUtil é uma ferramenta incluída no Windows e no Windows Server que tenta diagnosticar volumes ReFS altamente danificados, identificar arquivos restantes e copiar esses arquivos para outro volume.
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: storage-file-systems
ms.topic: article
ms.openlocfilehash: 0d7c1bf79695c2ddd03943744ebf1df4827d365c
ms.sourcegitcommit: 457e88e5aa6be13a2bffdb8e434a8efc3698678f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549136"
---
# <a name="refsutil"></a>ReFSUtil

>Aplica-se a: Windows Server 2019, Windows 10

ReFSUtil é uma ferramenta incluída no Windows e no Windows Server que tenta diagnosticar volumes ReFS altamente danificados, identificar arquivos restantes e copiar esses arquivos para outro volume. Isso é fornecido no Windows 10 na pasta%SystemRoot%\Windows\System32 ou no Windows Server na pasta% SystemRoot% \\ System32.

O ReFS Salvage é a principal função de ReFSUtil no momento e é útil para recuperar dados de volumes que mostram como BRUTOs no gerenciamento de disco. O ReFS Salvage tem duas fases: fase de verificação e uma fase de cópia. No modo automático, a fase de verificação e a fase de cópia serão executadas em sequência. No modo manual, cada fase pode ser executada separadamente. O progresso e os logs são salvos em um diretório de trabalho para permitir que as fases sejam executadas separadamente, bem como a fase de verificação para ser pausada e retomada. ReFSutil não precisará ser usado, a menos que o volume seja RAW. Se for somente leitura, os dados ainda estarão acessíveis.

## <a name="automatic-mode-command-line-usage"></a>Uso de linha de comando no modo automático

### <a name="quick-automatic-mode-command-line-usage"></a>Uso rápido de linha de comando no modo automático

```dos
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```
Ele executará uma fase de verificação rápida seguida de uma fase de cópia. Esse modo é executado mais rapidamente, pois ele assume que algumas estruturas críticas do volume não estão corrompidas e, portanto, não há necessidade de verificar o volume inteiro para localizá-los. Isso também reduz a recuperação de arquivos/diretórios/volumes obsoletos.

### <a name="full-automatic-mode-command-line-usage"></a>Uso completo de linha de comando no modo automático

```dos
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

Ele executará uma fase de verificação completa seguida de uma fase de cópia. Esse modo pode levar muito tempo, pois ele examinará todo o volume em busca de arquivos/diretórios/volumes recuperáveis.

## <a name="manual-mode-command-line-usage"></a>Uso de linha de comando no modo manual

### <a name="diagnose-phase-command-line-usage"></a>Diagnóstico de uso da linha de comando da fase

```dos
refsutil salvage -D <source volume> <working directory> <options>
```

Primeiro, tente determinar se \<source volume\> é um volume ReFS e determine se o volume é montável. Quando um volume não é montável, o motivo (s) será determinado. Esta é uma fase autônoma.

### <a name="quick-scan-phase-command-line-usage"></a>Uso da linha de comando da fase de verificação rápida

```dos
refsutil salvage -QS <source volume> <working directory> <options>
```

Verificação rápida \<source volume\> de quaisquer arquivos recuperáveis. Esse modo é executado mais rapidamente, pois ele assume que algumas estruturas críticas do volume não estão corrompidas e, portanto, não há necessidade de verificar o volume inteiro para localizá-los. Isso também reduz a recuperação de arquivos/diretórios/volumes obsoletos. Os arquivos descobertos serão registrados em "FoundFiles. \<volume signature\> . txt "em \<working directory\> . Se a fase de verificação foi interrompida anteriormente, a execução com o sinalizador-QS novamente retomará a verificação de onde parou.

### <a name="full-scan-phase-command-line-usage"></a>Uso da linha de comando da fase de verificação completa

```dos
refsutil salvage -FS <source volume> <working directory> <options>
```

Verificar todo os \<source volume\> arquivos recuperáveis. Esse modo pode levar muito tempo, pois ele examinará todo o volume em busca de arquivos recuperáveis.
Os arquivos descobertos serão registrados em "FoundFiles. \<volume signature\> . txt "em \<working directory\> . Se a fase de verificação foi interrompida anteriormente, a execução com o sinalizador-FS novamente retomará a verificação de onde parou.

### <a name="copy-phase-command-line-usage"></a>Uso da linha de comando da fase de cópia

```dos
refsutil salvage -C <source volume> <working directory> <target directory>
<options>
```

Copie todos os arquivos descritos em "FoundFiles. \<volume signature\> . txt "para \<target
directory\> . Se a fase de verificação for interrompida muito cedo, "FoundFiles. \<volume
signature\> . txt "Talvez ainda não tenha sido gravado e, portanto, nenhum arquivo será copiado para \<target directory\> .

### <a name="copy-phase-with-list-command-line-usage"></a>Fase de cópia com uso de linha de comando de lista

```dos
refsutil salvage -SL <source volume> <working directory> <target
directory> <file list> <options>
```

Copie todos os arquivos de \<file list\> de \<source volume\> para \<target
directory\> . Os arquivos em \<file list\> devem ter sido identificados primeiro pela fase de verificação, embora a verificação não precise ter sido executada até a conclusão. \<file list\>pode ser gerado copiando "FoundFiles. \<volume signature\> . txt "para um novo arquivo, removendo as linhas que fazem referência a arquivos que não devem ser restaurados e preservando os arquivos que devem ser restaurados. O cmdlet Select-String do PowerShell pode ser útil na filtragem de " \<volume signature\> FoundFiles. txt "para incluir apenas caminhos, extensões ou nomes de arquivos desejados.

### <a name="copy-phase-with-interactive-console"></a>Fase de cópia com Console interativo

```dos
refsutil salvage -IC <source volume> <working directory> <options>
```

Arquivos de recuperação em um console interativo para usuários avançados. Esse modo também requer arquivos gerados a partir de uma das fases de verificação.

## <a name="parameters"></a>Parâmetros

| Parâmetro             | Descrição                                                                     |
| --------------------- | --------------------------------------------------------------------------------------- |
| \<source volume\>     | Volume ReFS para processar. Letra da unidade no formato "L:" ou um caminho para o ponto de montagem do volume.           |
| \<working directory\> | Local para armazenar informações e logs temporários. Ele **não** deve estar localizado em \<source volume\> .  |
| \<target directory\>  | Local onde os arquivos identificados serão copiados. Ele **não** deve estar localizado em \<source volume\> . |
| \-m         | Recuperar todos os arquivos possíveis, incluindo aqueles excluídos. Essa opção será ignorada em refsutil Salvage v1. Aviso: não apenas isso levará mais tempo para ser executado, pode levar a um resultado inesperado. |
| \-l         | Modo detalhado                                                                                           |
| \-w.x.y.         | Force o volume a ser desmontado primeiro, se necessário. Todos os identificadores abertos para o volume seriam então inválidos. Por exemplo: refsutil Salvage-QA R: N: \\ Working N: \\ Data-x                                  |
