---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: cota do fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2daa2f6253a406ccad68677e1877215a1c610328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835287"
---
# <a name="fsutil-quota"></a>cota do fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gerencia as cotas de disco em volumes NTFS para fornecer um controle mais preciso do armazenamento baseado em rede.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|desabilitar|Desabilita o controle de cotas e imposição no volume especificado.|
|enforce|Impõe o uso de cota no volume especificado.|
|modify|Modifica uma cota de disco existente ou cria uma nova cota.|
|consulta|Lista os existentes cotas de disco.|
|Faixa|Faixas de uso do disco no volume especificado.|
|Violações|Os logs de sistema e do aplicativo de pesquisa e exibe uma mensagem para indicar que as violações de cota foram detectadas ou que um usuário tiver atingido um limite de cota ou limite de cota.|
|\<VolumePath>|Obrigatório. Especifica o nome da unidade seguido por dois-pontos ou o GUID no formato **Volume {***GUID***}**.|
|\<Limite >|Define o limite (em bytes) em que os avisos serão emitidos. Esse parâmetro é necessário para o **cota fsutil modificar** comando.|
|\<Limite de >|Define o uso máximo permitido em disco (em bytes). Esse parâmetro é necessário para o **cota fsutil modificar** comando.|
|\<UserName>|Especifica o nome de domínio ou usuário. Esse parâmetro é necessário para o **cota fsutil modificar** comando.|

## <a name="remarks"></a>Comentários

-   As cotas de disco são implementadas em uma base por volume, e eles permitem que os limites de armazenamento de hardware e software ser implementado em uma base por usuário.

-   Você pode usar scripts de gravação que usam **cota fsutil** para definir os limites de cota, sempre que você adicionar um novo usuário ou para controlar automaticamente os limites de cota, compilá-los em um relatório e automaticamente enviá-los para o administrador do sistema no email.

### <a name="BKMK_examples"></a>Exemplos
Para listar as cotas de disco existente para um volume de disco que é especificado com o GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digite:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Para listar as cotas de disco existente para um volume de disco que é especificado com a letra da unidade **c:**, tipo:

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


