---
title: gerenciar/desbloquear o BDE
description: Artigo de referência para o comando de desbloqueio Manage-bde, que desbloqueia uma unidade protegida pelo BitLocker usando uma senha de recuperação ou uma chave de recuperação.
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9b13e4ac27ab77a522d223749cd84e3b1bb11a6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886640"
---
# <a name="manage-bde-unlock"></a>gerenciar/desbloquear o BDE

Desbloqueia uma unidade protegida pelo BitLocker usando uma senha de recuperação ou uma chave de recuperação.

## <a name="syntax"></a>Sintaxe

```
manage-bde -unlock {-recoverypassword <password>|-recoverykey <pathtoexternalkeyfile>} <drive> [-certificate {-cf pathtocertificatefile | -ct certificatethumbprint} {-pin}] [-password] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -recoverypassword | Especifica que uma senha de recuperação será usada para desbloquear a unidade. Você também pode usar **-RP** como uma versão abreviada desse comando. |
| `<password>` | Representa a senha de recuperação que pode ser usada para desbloquear a unidade. |
| -recoverykey | Especifica que um arquivo de chave de recuperação externa será usado para desbloquear a unidade. Você também pode usar **-r** como uma versão abreviada deste comando. |
| `<pathtoexternalkeyfile>` | Representa o arquivo de chave de recuperação externa que pode ser usado para desbloquear a unidade. |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -certificado | O certificado de usuário local para um certificado do BitLocker para desbloquear o volume está localizado no repositório de certificados do usuário local. Você também pode usar **-CERT** como uma versão abreviada desse comando. |
| -CF`<pathtocertificatefile>` | Caminho para o arquivo de certificado. |
| -CT`<certificatethumbprint>` | Impressão digital do certificado que pode, opcionalmente, incluir o PIN (-PIN). |
| -password | Apresenta um prompt para a senha para desbloquear o volume. Você também pode usar **-PW** como uma versão abreviada desse comando. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para desbloquear a unidade E com um arquivo de chave de recuperação que foi salvo em uma pasta de backup em outra unidade, digite:

```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
