---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil quota
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 8079bacaa54282a1dd1091ffacd427ddaf74cc59
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725477"
---
# <a name="fsutil-quota"></a>Fsutil quota
> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gerencia cotas de disco em volumes NTFS para fornecer um controle mais preciso do armazenamento baseado em rede.



## <a name="syntax"></a>Sintaxe

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                    Descrição                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    disable    |                                                         Desabilita o rastreamento de cota e a imposição no volume especificado.                                                          |
|    fazer    |                                                                   Impõe o uso de cota no volume especificado.                                                                   |
|    modify     |                                                              Modifica uma cota de disco existente ou cria uma nova cota.                                                              |
|     Consulta     |                                                                            Lista as cotas de disco existentes.                                                                            |
|     rastrear     |                                                                    Controla o uso do disco no volume especificado.                                                                     |
|  violações   | Pesquisa os logs do sistema e do aplicativo e exibe uma mensagem para indicar que as violações de cota foram detectadas ou que um usuário atingiu um limite de cota ou de cota. |
| \<> VolumePath |                                  Obrigatórios. Especifica o nome da unidade seguido por dois-pontos ou pelo GUID no formato **volume {**<em>GUID</em>**}**.                                  |
| \<> de limite  |                            Define o limite (em bytes) no qual os avisos são emitidos. Esse parâmetro é necessário para o comando **fsutil quota modify** .                            |
|   \<> de limite    |                                Define o uso máximo de disco permitido (em bytes). Esse parâmetro é necessário para o comando **fsutil quota modify** .                                |
|  \<Nome de usuário>  |                                      Especifica o nome de usuário ou domínio. Esse parâmetro é necessário para o comando **fsutil quota modify** .                                       |

## <a name="remarks"></a>Comentários

-   As cotas de disco são implementadas por volume e permitem que os limites de armazenamento físico e flexível sejam implementados por usuário.

-   Você pode usar scripts de gravação que usam a **cota fsutil** para definir os limites de cota sempre que adicionar um novo usuário ou para controlar automaticamente os limites de cota, compilá-los em um relatório e enviá-los automaticamente ao administrador do sistema por email.

### <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para listar as cotas de disco existentes para um volume de disco especificado com o GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digite:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Para listar as cotas de disco existentes para um volume de disco especificado com a letra da unidade, **C:**, digite:

```
Fsutil quota query C:
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


