---
title: ReFSUtil
description: Artigo de referência para a ferramenta ReFSUtil, que tenta diagnosticar volumes ReFS altamente danificados, identificar arquivos restantes e copiar esses arquivos para outro volume.
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: storage-file-systems
ms.topic: article
ms.openlocfilehash: c84aaed5b34c535221247dcdd6ab0a5462fd4aad
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931106"
---
# <a name="refsutil"></a>ReFSUtil

>Aplica-se a: Windows Server 2019, Windows 10

ReFSUtil é uma ferramenta incluída no Windows e no Windows Server que tenta diagnosticar volumes ReFS altamente danificados, identificar arquivos restantes e copiar esses arquivos para outro volume. Isso é fornecido no Windows 10 na `%SystemRoot%\Windows\System32` pasta ou no Windows Server na `%SystemRoot%\\System32` pasta.

O ReFS Salvage é a função principal de ReFSUtil e é útil para recuperar dados de volumes que são exibidos como BRUTOs no gerenciamento de disco. O ReFS Salvage tem duas fases: fase de verificação e uma fase de cópia. No modo automático, a fase de verificação e a fase de cópia serão executadas em sequência. No modo manual, cada fase pode ser executada separadamente. O progresso e os logs são salvos em um diretório de trabalho para permitir que as fases sejam executadas separadamente, bem como a fase de verificação para ser pausada e retomada. Você não precisará usar a ferramenta ReFSutil, a menos que o volume seja bruto. Se for somente leitura, os dados ainda estarão acessíveis.

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<source volume>` | Especifica o volume ReFS a ser processado. A letra da unidade deve ser formatada como "L:", ou você deve fornecer um caminho para o ponto de montagem do volume. |
| `<working directory>` | Especifica o local para armazenar informações e logs temporários. Ele **não** deve estar localizado no `<source volume>` . |
| `<target directory>` | Especifica o local onde os arquivos identificados são copiados. Ele **não** deve estar localizado no `<source volume>` . |
| \-m | Recupera todos os arquivos possíveis, incluindo aqueles excluídos.<p>**AVISO:** Esse parâmetro não apenas faz com que o processo demore mais para ser executado, mas também pode levar a resultados inesperados. |
| \-l | Especifica o uso do modo detalhado. |
| \-w.x.y. | Força o volume a ser desmontado primeiro, se necessário. Todos os identificadores abertos para o volume são, então, inválidos. Por exemplo, `refsutil salvage -QA R: N:\\WORKING N:\\DATA -x`. |

## <a name="usage-and-available-options"></a>Opções de uso e disponíveis

### <a name="quick-automatic-mode-command-line-usage"></a>Uso rápido de linha de comando no modo automático

Executa uma fase de verificação rápida seguida de uma fase de cópia. Esse modo é executado mais rapidamente, pois ele assume que algumas estruturas críticas do volume não estão corrompidas e, portanto, não há necessidade de verificar o volume inteiro para localizá-los. Isso também reduz a recuperação de arquivos/diretórios/volumes obsoletos.

```
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```

### <a name="full-automatic-mode-command-line-usage"></a>Uso completo de linha de comando no modo automático

Executa uma fase de verificação completa seguida por uma fase de cópia. Esse modo pode levar muito tempo, pois ele examinará todo o volume em busca de arquivos/diretórios/volumes recuperáveis.

```
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

### <a name="diagnose-phase-command-line-usage-manual-mode"></a>Diagnosticar o uso da linha de comando da fase (modo manual)

Primeiro, tente determinar se o `<source volume>` é um volume ReFS e determinar se o volume está montável. Se um volume não estiver montável, os motivos serão fornecidos. Esta é uma fase autônoma.

```
refsutil salvage -D <source volume> <working directory> <options>
```

### <a name="quick-scan-phase-command-line-usage"></a>Uso da linha de comando da fase de verificação rápida

Executa uma verificação rápida do `<source volume>` para qualquer arquivo recuperável. Esse modo é executado mais rapidamente, pois ele assume que algumas estruturas críticas do volume não estão corrompidas e, portanto, não há necessidade de verificar o volume inteiro para localizá-los. Isso também reduz a recuperação de arquivos/diretórios/volumes obsoletos. Os arquivos descobertos são registrados `foundfiles.<volume signature>.txt` no arquivo, localizado no seu `<working directory>` . Se a fase de verificação foi interrompida anteriormente, a execução com o sinalizador **-QS** retomará a verificação de onde parou.

```
refsutil salvage -QS <source volume> <working directory> <options>
```

### <a name="full-scan-phase-command-line-usage"></a>Uso da linha de comando da fase de verificação completa

Examina todo o `<source volume>` para todos os arquivos recuperáveis. Esse modo pode levar muito tempo, pois ele examinará todo o volume em busca de arquivos recuperáveis. Os arquivos descobertos serão registrados no `foundfiles.<volume signature>.txt` arquivo, localizado no seu `<working directory>` . Se a fase de verificação foi interrompida anteriormente, a execução com o sinalizador **-FS** retomará a verificação de onde parou.

```
refsutil salvage -FS <source volume> <working directory> <options>
```

### <a name="copy-phase-command-line-usage"></a>Uso da linha de comando da fase de cópia

Copia todos os arquivos descritos no `foundfiles.<volume signature>.txt` arquivo para o `<target directory>` . Se você parar a fase de verificação muito cedo, é possível que o `foundfiles.<volume signature>.txt` arquivo ainda não exista, portanto, nenhum arquivo é copiado para o `<target directory>` .

```
refsutil salvage -C <source volume> <working directory> <target directory> <options>
```

### <a name="copy-phase-with-list-command-line-usage"></a>Fase de cópia com uso de linha de comando de lista

Copia todos os arquivos do `<file list>` do para o `<source volume>` `<target directory>` . Os arquivos no `<file list>` devem ter sido identificados primeiro pela fase de verificação, embora a verificação não precise ser executada até a conclusão. O `<file list>` pode ser gerado copiando- `foundfiles.<volume signature>.txt` se para um novo arquivo, removendo as linhas que fazem referência a arquivos que não devem ser restaurados e preservando os arquivos que devem ser restaurados. O cmdlet **Select-String** do PowerShell pode ser útil na filtragem `foundfiles.<volume signature>.txt` para incluir apenas caminhos, extensões ou nomes de arquivos desejados.

```
refsutil salvage -SL <source volume> <working directory> <target directory> <file list> <options>
```

### <a name="copy-phase-with-interactive-console"></a>Fase de cópia com Console interativo

Os usuários avançados podem recuperar arquivos usando um console interativo. Esse modo também requer arquivos gerados a partir de uma das fases de verificação.

```
refsutil salvage -IC <source volume> <working directory> <options>
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
