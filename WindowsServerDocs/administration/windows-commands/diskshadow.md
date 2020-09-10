---
title: Diskshadow
description: Artigo de referência para o comando DiskShadow, que é uma ferramenta que expõe a funcionalidade oferecida pelo VSS (serviço de cópias de sombra de volume).
ms.topic: reference
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 69a8eae18261085f697c92b138fce7ad3cff55ca
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635386"
---
# <a name="diskshadow"></a>Diskshadow

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diskshadow.exe é uma ferramenta que expõe a funcionalidade oferecida pelo VSS (serviço de cópias de sombra de volume). Por padrão, o DiskShadow usa um interpretador de comando interativo semelhante ao do DiskRAID ou DiskPart. O DiskShadow também inclui um modo programável.

> [!NOTE]
> A associação no grupo local de administradores, ou equivalente, é o requisito mínimo necessário para executar o DiskShadow.

## <a name="syntax"></a>Sintaxe

Para o modo interativo, digite o seguinte no prompt de comando para iniciar o interpretador de comando do DiskShadow:

```
diskshadow
```

Em modo de script, digite o seguinte, em que *script.txt* é um arquivo de script que contém comandos DiskShadow:

```
diskshadow -s script.txt
```

### <a name="parameters"></a>Parâmetros

Você pode executar os seguintes comandos no interpretador de comandos do DiskShadow ou por meio de um arquivo de script. No mínimo, apenas **Adicionar** e **criar** são necessários para criar uma cópia de sombra. No entanto, isso perde as configurações de contexto e opção, será um backup de cópia e cria uma cópia de sombra sem nenhum script de execução de backup.

| Comando | Descrição |
| --------- | ----------- |
| [comando Set](set_2.md) | Define o contexto, as opções, o modo detalhado e o arquivo de metadados para criar cópias de sombra. |
| [comando carregar metadados](load-metadata.md) | Carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração. |
| [comando do gravador](writer.md) | verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração. |
| [Adicionar comando](add.md) | Adiciona volumes ao conjunto de volumes que devem ser copiados em sombra ou adiciona aliases ao ambiente de alias. |
| [comando Create](create.md) | Inicia o processo de criação de cópia de sombra usando as configurações de contexto e opção atuais. |
| [comando exec](exec.md) | Executa um arquivo no computador local. |
| [comando Iniciar backup](begin-backup.md) | Inicia uma sessão de backup completo. |
| [comando end backup](end-backup.md) | Encerra uma sessão de backup completo e emite um evento **backupcomplete** com o estado de gravador apropriado, se necessário. |
| [comando Iniciar restauração](begin-restore.md) | Inicia uma sessão de restauração e emite um evento de **prerestore** para gravadores envolvidos. |
| [comando end Restore](end-restore.md) | Encerra uma sessão de restauração e emite um evento de **restauração** para gravadores envolvidos. |
| [redefinir comando](reset.md) | Redefine o DiskShadow para o estado padrão. |
| [comando de lista](list.md) | Lista gravadores, cópias de sombra ou provedores de cópia de sombra atualmente registrados que estão no sistema. |
| [comando delete Shadows](delete-shadows.md) | Exclui cópias de sombra. |
| [comando de importação](import.md) | Importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema. |
| [comando Mask](mask.md) | Remove cópias de sombra de hardware que foram importadas usando o comando de **importação** . |
| [expor comando](expose.md) | Expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem. |
| [comando unexpote](unexpose.md) | Não expõe uma cópia de sombra que foi exposta usando o comando **Expo** . |
| [comando de quebra](break_2.md) | Desassocia um volume de cópia de sombra do VSS. |
| [reverter comando](revert.md) | Reverte um volume de volta para uma cópia de sombra especificada. |
| [comando exit](exit.md) | Sai do interpretador de comandos ou script. |

## <a name="examples"></a>Exemplos

Esta é uma sequência de exemplo de comandos que criará uma cópia de sombra para backup. Ele pode ser salvo em arquivo como script. DSH e executado usando `diskshadow /s script.dsh` .

Suponha o seguinte:

- Você tem um diretório existente chamado c: \\ diskshadowdata.

- O volume do sistema é C: e seu volume de dados é D:.

- Você tem um arquivo backupscript. cmd em c: \\ diskshadowdata.

- O arquivo backupscript. cmd executará a cópia dos dados de sombra p: e q: em sua unidade de backup.

Você pode inserir esses comandos manualmente ou fazer o script deles:

```
#Diskshadow script file
set context persistent nowriters
set metadata c:\diskshadowdata\example.cab
set verbose on
begin backup
add volume c: alias systemvolumeshadow
add volume d: alias datavolumeshadow

create

expose %systemvolumeshadow% p:
expose %datavolumeshadow% q:
exec c:\diskshadowdata\backupscript.cmd
end backup
#End of script
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
