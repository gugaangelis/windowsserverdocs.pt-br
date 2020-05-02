---
title: gerenciar/desbloquear o BDE
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fd303116e101c8c8503d220ba382d6cdd994ad3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724062"
---
# <a name="manage-bde-unlock"></a>Manage-bde: desbloquear



Desbloqueia uma unidade protegida pelo BitLocker usando uma senha de recuperação ou uma chave de recuperação.

## <a name="syntax"></a>Sintaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|
|---------|-----|-----------|
|-recoverypassword||Especifica que uma senha de recuperação será usada para desbloquear a unidade. Abreviação:-RP|
||\<> de senha|Representa a senha de recuperação que pode ser usada para desbloquear a unidade.|
|-recoverykey||Especifica que um arquivo de chave de recuperação externa será usado para desbloquear a unidade. Abreviação:-r|
||\<> PathToExternalKeyFile|Representa o arquivo de chave de recuperação externa que pode ser usado para desbloquear a unidade.|
||\<> da unidade|Representa uma letra de unidade seguida de dois-pontos.|
|-certificado||O certificado de usuário local para um certificado do BitLocker para desregistrar o volume está localizado no repositório de certificados do usuário localizável. Abreviação:-CERT|
||<-CF PathToCertificateFile>|Caminho para o arquivo cerficate|
||< de CertificateThumbprint de CT>|Impressão digital do certificado que pode, opcionalmente, incluir o PIN (-PIN).|
|-password||Apresenta um prompt para a senha para desbloquear o volume. Abreviação:-PW|
|-ComputerName||Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Abreviação:-CN|
||\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?||Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h||Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-Unlock** para desbloquear a unidade E com um arquivo de chave de recuperação que foi salvo em uma pasta de backup em outra unidade.
```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)