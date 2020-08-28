---
title: fsutil repair
description: Artigo de referência do comando fsutil Repair, que administra e monitora operações de reparo de auto-recuperação do NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 9c26f2be8e586205271ba1d150bb2378cc8b2045
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037324"
---
# <a name="fsutil-repair"></a>fsutil repair

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra e monitora operações de reparo de auto-recuperação do NTFS. O NTFS de auto-recuperação tenta corrigir as corrupções do sistema de arquivos NTFS online, sem a necessidade de executar **Chkdsk.exe** . Para obter mais informações, consulte [recuperação de NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10))de auto-recuperação.

## <a name="syntax"></a>Sintaxe

```
fsutil repair [enumerate] <volumepath> [<logname>]
fsutil repair [initiate] <volumepath> <filereference>
fsutil repair [query] <volumepath>
fsutil repair [set] <volumepath> <flags>
fsutil repair [wait][<waittype>] <volumepath>

```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| enumera | Enumera os inteiros do log de corrupção de um volume. |
| `<logname>` | Pode ser `$corrupt` , o conjunto de corrupções confirmadas no volume ou `$verify` um conjunto de danos potenciais não verificados no volume. |
| início | Inicia a auto-recuperação do NTFS. |
| `<filereference>` | Especifica a ID de arquivo específica do volume NTFS (número de referência do arquivo). A referência de arquivo inclui o número de segmento do arquivo. |
| Consulta | Consulta o estado de auto-recuperação do volume NTFS. |
| set | Define o estado de auto-recuperação do volume. |
| `<flags>` | Especifica o método de reparo a ser usado ao definir o estado de auto-recuperação do volume.<p>Esse parâmetro pode ser definido como três valores:<ul><li>**0x01** – habilita o reparo geral.</li><li>**0x09** -avisa sobre potencial perda de dados sem reparo.</li><li>**0x00** -desabilita operações de reparo de auto-recuperação do NTFS.</li></ul> |
| state | Consulta o estado de corrupção do sistema ou de um determinado volume. |
| wait | Aguarda a conclusão de reparo (s). Se o NTFS tiver detectado um problema em um volume no qual está executando reparos, essa opção permitirá que o sistema aguarde até que o reparo seja concluído antes de executar qualquer script pendente. |
| `[waittype {0|1}]` | Indica se deve aguardar a conclusão do reparo atual ou aguardar a conclusão de todos os reparos. O parâmetro *waittype* pode ser definido com os seguintes valores:<ul><li>**0** -aguarda a conclusão de todos os reparos. (valor padrão)</li><li>**1** -aguarda a conclusão do reparo atual.</li></ul> |

### <a name="examples"></a>Exemplos

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

- [fsutil](fsutil.md)

- [Recuperação automática de NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10))
