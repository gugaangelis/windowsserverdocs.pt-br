---
title: gerenciar/desbloquear o BDE
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e26952ca8c2b20cb0cb8efa167fca81e27b692a0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839732"
---
# <a name="manage-bde-unlock"></a>Manage-bde: desbloquear



Desbloqueia uma unidade protegida pelo BitLocker usando uma senha de recuperação ou uma chave de recuperação. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|{1&gt;Valor&lt;1}|Descrição|
|---------|-----|-----------|
|-recoverypassword||Especifica que uma senha de recuperação será usada para desbloquear a unidade. Abreviação:-RP|
||\<senha >|Representa a senha de recuperação que pode ser usada para desbloquear a unidade.|
|-recoverykey||Especifica que um arquivo de chave de recuperação externa será usado para desbloquear a unidade. Abreviação:-r|
||\<PathToExternalKeyFile >|Representa o arquivo de chave de recuperação externa que pode ser usado para desbloquear a unidade.|
||Unidade de \<>|Representa uma letra de unidade seguida de dois-pontos.|
|-certificado||O certificado de usuário local para um certificado do BitLocker para desregistrar o volume está localizado no repositório de certificados do usuário localizável. Abreviação:-CERT|
||<-CF PathToCertificateFile >|Caminho para o arquivo cerficate|
||< de CertificateThumbprint de CT >|Impressão digital do certificado que pode, opcionalmente, incluir o PIN (-PIN).|
|-password||Apresenta um prompt para a senha para desbloquear o volume. Abreviação:-PW|
|-ComputerName||Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Abreviação:-CN|
||Nome do \<>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?||Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h||Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir ilustra o uso do comando **-Unlock** para desbloquear a unidade E com um arquivo de chave de recuperação que foi salvo em uma pasta de backup em outra unidade.
```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)