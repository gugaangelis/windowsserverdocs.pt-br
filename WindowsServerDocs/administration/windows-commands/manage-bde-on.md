---
title: Gerenciar-bde em
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b50cad64025e85824a8f0a27d773ffb614491fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841177"
---
# <a name="manage-bde-on"></a>Gerenciar-bde: no



Criptografa a unidade e ativa o BitLocker. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>|Representa uma letra de unidade seguida de dois-pontos.|
|-recoverypassword|Adiciona um protetor de senha numérica. Você também pode usar **- rp** como uma versão abreviada desse comando.|
|\<NumericalPassword >|Representa a senha de recuperação.|
|-recoverykey|Adiciona um protetor de chave externa para recuperação. Você também pode usar **- rk** como uma versão abreviada desse comando.|
|\<PathToExternalDirectory>|Representa o caminho do diretório para a chave de recuperação.|
|-startupkey|Adiciona um protetor de chave externa para inicialização. Você também pode usar **sk -** como uma versão abreviada desse comando.|
|\<PathToExternalKeyDirectory>|Representa o caminho do diretório para a chave de inicialização.|
|-certificate|Adiciona um protetor de chave público para uma unidade de dados. Você também pode usar **-cert** como uma versão abreviada desse comando.|
|-tpmandpin|Adiciona um Trusted Platform Module (TPM) e um protetor (PIN) número de identificação pessoal para a unidade do sistema operacional. Você também pode usar **- tp** como uma versão abreviada desse comando.|
|-tpmandstartupkey|Adiciona um protetor de chave TPM e inicialização na unidade do sistema operacional. Você também pode usar **-tsk** como uma versão abreviada desse comando.|
|-tpmandpinandstartupkey|Adiciona um TPM, o PIN e o protetor de chave de inicialização na unidade do sistema operacional. Você também pode usar **- tpsk** como uma versão abreviada desse comando.|
|-senha|Adiciona um protetor de chave de senha para a unidade de dados. Você também pode usar **- pw** como uma versão abreviada desse comando.|
|-ADAccountOrGroup|Adiciona um protetor de identidade baseada em SID para o volume. O volume irá desbloquear automaticamente se o usuário ou computador tiver as credenciais apropriadas. Ao especificar uma conta de computador, acrescente uma **$** para o computador nomear e especificar **– serviço** para indicar que o desbloqueio deve acontecer no conteúdo do servidor, em vez de disco BitLocker a usuário. Você também pode usar **-sid** como uma versão abreviada desse comando.|
|-UsedSpaceOnly|Define o modo de criptografia para criptografia apenas espaço usado. As seções do volume que contém o espaço usado serão criptografadas, mas o espaço livre não será. Se essa opção não for especificada, todos os espaço usado e o espaço livre no volume será criptografado... Você também pode usar **-usado** como uma versão abreviada desse comando.|
|-encryptionMethod|Configura o tamanho de chave e o algoritmo de criptografia. Você também pode usar **-em** como uma versão abreviada desse comando.|
|-skiphardwaretest|Inicia a criptografia sem um teste de hardware. Você também pode usar **-s** como uma versão abreviada desse comando.|
|-discoveryvolumetype|Especifica o sistema de arquivos a ser usado para a unidade de dados de descoberta. A unidade de dados de descoberta é uma unidade oculta adicionada a uma unidade de dados removíveis formatadas por FAT, protegidas pelo BitLocker que contém o leitor sobre o BitLocker To Go para que os sistemas operacionais Windows Vista ou Windows XP pode ser usados para exibir as unidades protegidas pelo BitLocker.|
|-ForceEncryptionType|Força o BitLocker para usar a criptografia de software ou hardware. Você pode especificar **Hardware** ou **Software** como o tipo de criptografia. Se o **hardware** parâmetro for selecionado, mas a unidade não dá suporte à criptografia de hardware, gerencie-bde retornará um erro. Se as configurações de diretiva de grupo proíbe o tipo de criptografia, gerenciar-bde retornará um erro. Você também pode usar **-fet** como uma versão abreviada desse comando.|
|-RemoveVolumeShadowCopies|Força deletikon de cópias de sombra de Volume para o volume. Você não poderá restaurar este volume usando pontos de restauração do sistema anterior após a execução desse comando. Você também pode usar **- rvsc** como uma versão abreviada desse comando.|
|\<FileSystemType>|Especifica quais sistemas de arquivos podem ser usados com unidades de dados de descoberta: FAT32, padrão ou none.|
|-computername|Especifica que Manage-bde está sendo usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **-no** comando para ativar o BitLocker para a unidade C e adicione uma senha de recuperação para a unidade.
```
manage-bde –on C: -recoverypassword
```
O exemplo a seguir ilustra o uso de **-no** comando para ativar o BitLocker para a unidade C, adicionar uma senha de recuperação para a unidade e salvar uma chave de recuperação para a unidade E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
O exemplo a seguir ilustra o uso de **-no** comando para ativar o BitLocker para a unidade C usando um protetor de chave externa (como uma chave USB) para desbloquear a unidade do sistema operacional. Esse método é necessário se você estiver usando o BitLocker com computadores que não têm um TPM.
```
manage-bde -on C: -startupkey E:\
```
O exemplo a seguir ilustra o uso de **-no** comando para ativar o BitLocker para a unidade de dados E e adicionar um protetor de chave de senha. Gerenciar-bde solicitará que você insira a senha depois que esse comando foi inserido.
```
manage-bde –on E: -pw
```
O exemplo a seguir ilustra o uso de **-no** comando para ativar o BitLocker para a unidade C do sistema operacional e usar a criptografia baseada em hardware.
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)