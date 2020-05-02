---
title: Manage-bde em
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55f9dc446b17b8e61655686b9f4b6259b12dafc9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724131"
---
# <a name="manage-bde-on"></a>Manage-bde: on



Criptografa a unidade e ativa o BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> da unidade|Representa uma letra de unidade seguida de dois-pontos.|
|-recoverypassword|Adiciona um protetor de senha numérica. Você também pode usar **-RP** como uma versão abreviada desse comando.|
|\<> NumericalPassword|Representa a senha de recuperação.|
|-recoverykey|Adiciona um protetor de chave externa para recuperação. Você também pode usar **-r** como uma versão abreviada deste comando.|
|\<> PathToExternalDirectory|Representa o caminho do diretório para a chave de recuperação.|
|-startupkey|Adiciona um protetor de chave externa para inicialização. Você também pode usar **-SK** como uma versão abreviada desse comando.|
|\<> PathToExternalKeyDirectory|Representa o caminho do diretório para a chave de inicialização.|
|-certificado|Adiciona um protetor de chave pública para uma unidade de dados. Você também pode usar **-CERT** como uma versão abreviada desse comando.|
|-tpmandpin|Adiciona um protetor de Trusted Platform Module (TPM) e número de identificação pessoal (PIN) para a unidade do sistema operacional. Você também pode usar **-TP** como uma versão abreviada desse comando.|
|-tpmandstartupkey|Adiciona um protetor de chave de inicialização e TPM para a unidade do sistema operacional. Você também pode usar **-tsk** como uma versão abreviada deste comando.|
|-tpmandpinandstartupkey|Adiciona um protetor de TPM, PIN e chave de inicialização para a unidade do sistema operacional. Você também pode usar **-tpsk** como uma versão abreviada deste comando.|
|-password|Adiciona um protetor de chave de senha para a unidade de dados. Você também pode usar **-PW** como uma versão abreviada desse comando.|
|-ADAccountOrGroup|Adiciona um protetor de identidade baseado em SID para o volume. O volume será desbloqueado automaticamente se o usuário ou o computador tiver as credenciais apropriadas. Ao especificar uma conta de computador, acrescente **$** a ao nome do computador e especifique **– Service** para indicar que o desbloqueio deve ocorrer no conteúdo do servidor BitLocker em vez do usuário. Você também pode usar **-Sid** como uma versão abreviada deste comando.|
|-UsedSpaceOnly|Define o modo de criptografia para criptografia somente de espaço usado. As seções do volume que contém o espaço usado serão criptografadas, mas o espaço livre não será. Se essa opção não for especificada, todo o espaço usado e o espaço livre no volume serão criptografados. Você também pode usar **-usado** como uma versão abreviada deste comando.|
|-encryptionMethod|Configura o algoritmo de criptografia e o tamanho da chave. Você também pode usar **-em** como uma versão abreviada deste comando.|
|-skiphardwaretest|Inicia a criptografia sem um teste de hardware. Você também pode usar **-s** como uma versão abreviada deste comando.|
|-discoveryvolumetype|Especifica o sistema de arquivos a ser usado para a unidade de dados de descoberta. A unidade de dados de descoberta é uma unidade oculta adicionada a uma unidade de dados removível com formato FAT protegido pelo BitLocker que contém o Leitor BitLocker To Go para que os sistemas operacionais Windows Vista ou Windows XP possam ser usados para exibir unidades protegidas pelo BitLocker.|
|-ForceEncryptionType|Força o BitLocker a usar a criptografia de software ou hardware. Você pode especificar o **hardware** ou o **software** como o tipo de criptografia. Se o parâmetro de **hardware** for selecionado, mas a unidade não oferecer suporte à criptografia de hardware, o Manage-bde retornará um erro. Se Política de Grupo configurações proibir o tipo de criptografia especificado, o Manage-bde retornará um erro. Você também pode usar **-FET** como uma versão abreviada deste comando.|
|-RemoveVolumeShadowCopies|Forçar deletikon de cópias de sombra de volume para o volume. Você não poderá restaurar esse volume usando pontos de restauração do sistema anteriores depois de executar esse comando. Você também pode usar **-rvsc** como uma versão abreviada deste comando.|
|\<> FileSystemType|Especifica quais sistemas de arquivos podem ser usados com unidades de dados de descoberta: FAT32, padrão ou nenhum.|
|-ComputerName|Especifica que o Manage-bde está sendo usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrar o uso do comando **-on** para ativar o BitLocker para a unidade C e adicionar uma senha de recuperação à unidade.
```
manage-bde –on C: -recoverypassword
```
Para ilustrar o uso do comando **-on** para ativar o BitLocker para a unidade C, adicione uma senha de recuperação à unidade e salve uma chave de recuperação na unidade E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
Para ilustrar o uso do comando **-on** para ativar o BitLocker para a unidade C usando um protetor de chave externa (como uma chave USB) para desbloquear a unidade do sistema operacional. Esse método será necessário se você estiver usando o BitLocker com computadores que não têm um TPM.
```
manage-bde -on C: -startupkey E:\
```
Para ilustrações usando o comando **-on** para ativar o BitLocker para a unidade de dados e e adicionar um protetor de chave de senha. Manage-bde solicitará que você insira a senha depois que esse comando for inserido.
```
manage-bde –on E: -pw
```
Para ilustrar o uso do comando **-on** para ativar o BitLocker para a unidade de sistema operacional C e usar a criptografia baseada em hardware.
```
manage-bde –on C: -fet Hardware
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)