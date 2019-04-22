---
title: Gerenciar-bde desbloquear
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814597"
---
# <a name="manage-bde-unlock"></a>Gerenciar-bde: desbloquear



Desbloqueia uma unidade protegida pelo BitLocker usando uma senha de recuperação ou uma chave de recuperação. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|
|---------|-----|-----------|
|-recoverypassword||Especifica que uma senha de recuperação será usada para desbloquear a unidade. Abreviação: - rp|
||\<Senha >|Representa a senha de recuperação que pode ser usada para desbloquear a unidade.|
|-recoverykey||Especifica que um arquivo de chave de recuperação externo será usado para desbloquear a unidade. Abreviação: - rk|
||\<PathToExternalKeyFile>|Representa o arquivo de chave de recuperação externo que pode ser usado para desbloquear a unidade.|
||\<Drive>|Representa uma letra de unidade seguida de dois-pontos.|
|-certificate||O certificado de usuário local para um certificado de BitLocker para unclock o volume está localizado no repositório de certificados de usuário local. Abbreviation: -cert|
||<-PathToCertificateFile cf >|Caminho para o arquivo cerficate|
||<-ct CertificateThumbprint >|Impressão digital do certificado que pode, opcionalmente, incluir o PIN (-pin).|
|-senha||Apresenta um prompt para a senha desbloquear o volume. Abreviação: - pw|
|-computername||Especifica que gerenciar bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Abreviação: - cn|
||\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?||Exibe uma ajuda breve no prompt de comando.|
|-help ou -h||Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **-desbloquear** comando para desbloquear a unidade E com um arquivo de chave de recuperação que foi salvo em uma pasta de backup em outra unidade.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)