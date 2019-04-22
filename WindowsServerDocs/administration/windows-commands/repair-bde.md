---
title: repair-bde
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e2bce0c0a0f12a3a171c161a669c903044e8b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813537"
---
# <a name="repair-bde"></a>repair-bde



Acessos criptografado dados em um disco rígido gravemente danificado se a unidade foi criptografada usando o BitLocker. Repair-bde pode reconstruir partes críticas da unidade e recuperar dados recuperáveis desde que uma senha ou chave de recuperação válida seja usada para descriptografar os dados. Se os dados de metadados do BitLocker na unidade estiverem corrompidos, você deverá fornecer um pacote de chaves de backup, além da senha ou da chave de recuperação. O backup desse pacote de chaves é feito nos Serviços de Domínio do Active Directory (AD DS) caso você tenha usado a configuração padrão para o backup do AD DS. Com esse pacote de chaves e a senha ou a chave de recuperação, você poderá descriptografar partes de uma unidade protegida pelo BitLocker, se o disco for danificado. Cada pacote de chaves funcionará apenas para uma unidade que tenha o identificador de unidade correspondente. Você pode usar o [Visualizador de senha de recuperação do BitLocker para o Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) para obter esse pacote de chaves do AD DS.

> [!NOTE]
> O Visualizador de senha de recuperação do BitLocker é incluído como um dos recursos opcionais de gerenciamento instalados usando o servidor gerenciar no Windows Server 2012.

As limitações a seguir existem para a ferramenta de linha de comando Repair-bde:
-   Repair-bde não pode reparar uma unidade que falhou durante o processo de criptografia ou descriptografia.
-   Repair-bde presume que se a unidade tiver alguma criptografia, em seguida, a unidade foi totalmente criptografada.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<InputVolume>|Identifica a letra da unidade da unidade criptografada pelo BitLocker que você deseja reparar. A letra da unidade deve incluir dois-pontos; Por exemplo: **C:**.|
|\<OutputVolumeorImage>|Identifica a unidade na qual armazenar o conteúdo da unidade reparado. Todas as informações sobre a unidade de saída serão substituídas.|
|-rk|Identifica o local da chave de recuperação deve ser usado para desbloquear o volume. Esse comando também pode ser especificado como **- recoverykey**.|
|-rp|Identifica a senha de recuperação numérica deve ser usada para desbloquear o volume. Esse comando também pode ser especificado como **- recoverypassword**.|
|-pw|Identifica a senha que deve ser usada para desbloquear o volume. Esse comando também pode ser especificado como **-senha**|
|-kp|Identifica o pacote de chaves de recuperação que pode ser usado para desbloquear o volume. Esse comando também pode ser especificado como **- keypackage**.|
|-lf|Especifica o caminho para o arquivo que armazenará o erro, aviso e mensagens de informações de Repair-bde. Esse comando também pode ser especificado como **- logfile**.|
|-f|Força um volume ser desmontado, mesmo se ele não pode ser bloqueado. Esse comando também pode ser especificado como **-forçar**.|
|-? ou /?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Se o caminho para um pacote de chaves não for especificado, **repair-bde** procurará a unidade para um pacote de chaves. No entanto, se o disco rígido tiver sido danificado, **repair-bde** pode não ser capaz de encontrar o pacote e solicitará que você forneça o caminho.

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo da unidade C para a unidade D, usando o arquivo de chave de recuperação (RecoveryKey.bek) armazenado na unidade F e grava os resultados dessa tentativa ao arquivo de log (log. txt) na unidade Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo na unidade C para a unidade D usando a senha de recuperação de 48 dígitos especificada. A senha de recuperação deve ser digitada em oito blocos de seis dígitos com um hífen, separando cada bloco.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
O exemplo a seguir força a unidade C a ser desmontado e, em seguida, tenta reparar a unidade C e gravar o conteúdo na unidade C para a unidade D usando o pacote de chaves de recuperação e o arquivo de chave de recuperação (RecoveryKey.bek) armazenados na unidade F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo da unidade C para a unidade D, e você deve digitar uma senha para desbloquear a unidade c: quando solicitado:
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)