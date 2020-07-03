---
title: Manage-bde em
description: Artigo de referência para o comando Manage-bde on, que criptografa a unidade e ativa o BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b06c8a37524544201bf9f37a446a8d227f878ee4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935448"
---
# <a name="manage-bde-on"></a>Manage-bde em

Criptografa a unidade e ativa o BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde –on <drive> {[-recoverypassword <numericalpassword>]|[-recoverykey <pathtoexternaldirectory>]|[-startupkey <pathtoexternalkeydirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <pathtoexternalkeydirectory>]|[-tpmandstartupkey <pathtoexternalkeydirectory>]|[-password]|[-ADaccountorgroup <domain\account>]}
[-usedspaceonly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <filesystemtype>] [-forceencryptiontype <type>] [-removevolumeshadowcopies][-computername <name>]
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -recoverypassword | Adiciona um protetor de senha numérica. Você também pode usar **-RP** como uma versão abreviada desse comando. |
| `<numericalpassword>` | Representa a senha de recuperação. |
| -recoverykey | Adiciona um protetor de chave externa para recuperação. Você também pode usar **-r** como uma versão abreviada deste comando. |
| `<pathtoexternaldirectory>` | Representa o caminho do diretório para a chave de recuperação. |
| -startupkey | Adiciona um protetor de chave externa para inicialização. Você também pode usar **-SK** como uma versão abreviada desse comando. |
| `<pathtoexternalkeydirectory>` | Representa o caminho do diretório para a chave de inicialização. |
| -certificado | Adiciona um protetor de chave pública para uma unidade de dados. Você também pode usar **-CERT** como uma versão abreviada desse comando. |
| -tpmandpin | Adiciona um protetor de Trusted Platform Module (TPM) e número de identificação pessoal (PIN) para a unidade do sistema operacional. Você também pode usar **-TP** como uma versão abreviada desse comando. |
| -tpmandstartupkey | Adiciona um protetor de chave de inicialização e TPM para a unidade do sistema operacional. Você também pode usar **-tsk** como uma versão abreviada deste comando. |
| -tpmandpinandstartupkey | Adiciona um protetor de TPM, PIN e chave de inicialização para a unidade do sistema operacional. Você também pode usar **-tpsk** como uma versão abreviada deste comando. |
| -password | Adiciona um protetor de chave de senha para a unidade de dados. Você também pode usar **-PW** como uma versão abreviada desse comando. |
| -ADaccountorgroup | Adiciona um protetor de identidade baseado em SID para o volume. O volume será desbloqueado automaticamente se o usuário ou o computador tiver as credenciais apropriadas. Ao especificar uma conta de computador, acrescente a **$** ao nome do computador e especifique **– Service** para indicar que o desbloqueio deve ocorrer no conteúdo do servidor BitLocker em vez do usuário. Você também pode usar **-Sid** como uma versão abreviada deste comando. |
| -usedspaceonly | Define o modo de criptografia para criptografia somente de espaço usado. As seções do volume que contém o espaço usado serão criptografadas, mas o espaço livre não será. Se essa opção não for especificada, todo o espaço usado e o espaço livre no volume serão criptografados. Você também pode usar **-usado** como uma versão abreviada deste comando. |
| -encryptionMethod | Configura o algoritmo de criptografia e o tamanho da chave. Você também pode usar **-em** como uma versão abreviada deste comando. |
| -skiphardwaretest | Inicia a criptografia sem um teste de hardware. Você também pode usar **-s** como uma versão abreviada deste comando. |
| -discoveryvolumetype | Especifica o sistema de arquivos a ser usado para a unidade de dados de descoberta. A unidade de dados de descoberta é uma unidade oculta adicionada a uma unidade de dados removível com formato FAT protegido pelo BitLocker que contém o Leitor BitLocker To Go. |
| -forceencryptiontype | Força o BitLocker a usar a criptografia de software ou hardware. Você pode especificar o **hardware** ou o **software** como o tipo de criptografia. Se o parâmetro de **hardware** for selecionado, mas a unidade não oferecer suporte à criptografia de hardware, o Manage-bde retornará um erro. Se Política de Grupo configurações proibir o tipo de criptografia especificado, o Manage-bde retornará um erro. Você também pode usar **-FET** como uma versão abreviada deste comando. |
| -removevolumeshadowcopies | Forçar a exclusão de cópias de sombra de volume para o volume. Você não conseguirá restaurar esse volume usando pontos de restauração do sistema anteriores depois de executar esse comando. Você também pode usar **-rvsc** como uma versão abreviada deste comando. |
| `<filesystemtype>` | Especifica quais sistemas de arquivos podem ser usados com unidades de dados de descoberta: FAT32, padrão ou nenhum. |
| -ComputerName | Especifica que o Manage-bde está sendo usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para ativar o BitLocker para a unidade C e adicionar uma senha de recuperação à unidade, digite:

```
manage-bde –on C: -recoverypassword
```

Para ativar o BitLocker para a unidade C, adicione uma senha de recuperação à unidade e salve uma chave de recuperação na unidade E, digite:

```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```

Para ativar o BitLocker para a unidade C, usando um protetor de chave externa (como uma chave USB) para desbloquear a unidade do sistema operacional, digite:

```
manage-bde -on C: -startupkey E:\
```

> [!IMPORTANT]
> Esse método será necessário se você estiver usando o BitLocker com computadores que não têm um TPM.

Para ativar o BitLocker para a unidade de dados E para adicionar um protetor de chave de senha, digite:

```
manage-bde –on E: -pw
```

Para ativar o BitLocker para a unidade de sistema operacional C e usar a criptografia baseada em hardware, digite:

```
manage-bde –on C: -fet hardware
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde off](manage-bde-off.md)

- [comando de pausa Manage-bde](manage-bde-pause.md)

- [comando de retomada Manage-bde](manage-bde-resume.md)

- [comando Manage-bde](manage-bde.md)
