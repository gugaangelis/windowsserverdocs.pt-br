---
title: repair-bde
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 817e5fb5cf032376ddfddb3a54f73411ac175def
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384548"
---
# <a name="repair-bde"></a>repair-bde



Acessa dados criptografados em um disco rígido danificado severamente se a unidade foi criptografada usando o BitLocker. Repair-bde pode reconstruir partes críticas da unidade e recuperar dados recuperáveis desde que uma senha ou chave de recuperação válida seja usada para descriptografar os dados. Se os dados de metadados do BitLocker na unidade estiverem corrompidos, você deverá fornecer um pacote de chaves de backup, além da senha ou da chave de recuperação. O backup desse pacote de chaves é feito nos Serviços de Domínio do Active Directory (AD DS) caso você tenha usado a configuração padrão para o backup do AD DS. Com esse pacote de chaves e a senha ou a chave de recuperação, você poderá descriptografar partes de uma unidade protegida pelo BitLocker, se o disco for danificado. Cada pacote de chaves funcionará apenas para uma unidade que tenha o identificador de unidade correspondente. Você pode usar o [Visualizador de senha de recuperação do BitLocker para Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) para obter esse pacote de chaves do AD DS.

> [!NOTE]
> O Visualizador de senha de recuperação do BitLocker é incluído como um dos recursos de gerenciamento opcionais instaláveis usando o gerenciamento do servidor no Windows Server 2012.

Existem as seguintes limitações para a ferramenta de linha de comando Repair-bde:
-   O Repair-bde não pode reparar uma unidade que falhou durante o processo de criptografia ou descriptografia.
-   Repair-bde pressupõe que, se a unidade tiver alguma criptografia, a unidade terá sido totalmente criptografada.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<InputVolume >|Identifica a letra da unidade da unidade criptografada pelo BitLocker que você deseja reparar. A letra da unidade deve incluir dois-pontos; por exemplo: **C:** .|
|\<OutputVolumeorImage >|Identifica a unidade na qual armazenar o conteúdo da unidade reparada. Todas as informações na unidade de saída serão substituídas.|
|-r|Identifica o local da chave de recuperação que deve ser usada para desbloquear o volume. Esse comando também pode ser especificado como **-RecoveryKey**.|
|-RP|Identifica a senha de recuperação numérica que deve ser usada para desbloquear o volume. Esse comando também pode ser especificado como **-RecoveryPassword**.|
|-PW|Identifica a senha que deve ser usada para desbloquear o volume. Este comando também pode ser especificado como **senha**|
|-KP|Identifica o pacote de chave de recuperação que pode ser usado para desbloquear o volume. Esse comando também pode ser especificado como **-KeyPackage**.|
|-LF|Especifica o caminho para o arquivo que armazenará mensagens de erro, aviso e informações de reparo do bde. Esse comando também pode ser especificado como **-logfile**.|
|-f|Força a desmontagem de um volume mesmo que ele não possa ser bloqueado. Esse comando também pode ser especificado como **-Force**.|
|-? ou/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Se o caminho para um pacote de chaves não for especificado, o **Repair-bde** pesquisará a unidade em busca de um pacote de chaves. No entanto, se o disco rígido tiver sido danificado, o **Repair-bde** poderá não conseguir localizar o pacote e solicitará que você forneça o caminho.

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo da unidade C na unidade D usando o arquivo de chave de recuperação (RecoveryKey. Bek) armazenado na unidade F e grava os resultados dessa tentativa no arquivo de log (log. txt) na unidade Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo na unidade C na unidade D usando a senha de recuperação de 48 dígitos especificada. A senha de recuperação deve ser digitada em oito blocos de seis dígitos com um hífen separando cada bloco.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
O exemplo a seguir força a desmontagem da unidade C e tenta reparar a unidade C e gravar o conteúdo na unidade C na unidade D usando o pacote de chave de recuperação e o arquivo de chave de recuperação (RecoveryKey. Bek) armazenados na unidade F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
O exemplo a seguir tenta reparar a unidade C e gravar o conteúdo da unidade C na unidade D e você deve digitar uma senha para desbloquear a unidade C: quando solicitado:
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)