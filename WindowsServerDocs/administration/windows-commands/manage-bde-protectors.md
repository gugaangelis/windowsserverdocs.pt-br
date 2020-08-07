---
title: Manage-bde protetores
description: Artigo de referência para o comando Manage-bde protectors, que gerencia os métodos de proteção usados para a chave de criptografia do BitLocker.
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 445f87435a97173192976f195478f9c92e109032
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886753"
---
# <a name="manage-bde-protectors"></a>Manage-bde protetores

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Gerencia os métodos de proteção usados para a chave de criptografia do BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ----------- | ----------- |
| -obter | Exibe todos os métodos de proteção de chave habilitados na unidade e fornece seu tipo e identificador (ID). |
| -Adicionar | Adiciona os métodos de proteção de chave conforme especificado usando parâmetros adicionais **-Adicionar** . |
| -excluir | Exclui os métodos de proteção de chave usados pelo BitLocker. Todos os protetores de chave serão removidos de uma unidade, a menos que os parâmetros opcionais **-delete** sejam usados para especificar quais protetores devem ser excluídos. Quando o último protetor em uma unidade é excluído, a proteção do BitLocker da unidade é desabilitada para garantir que o acesso aos dados não seja perdido inadvertidamente. |
| -desabilitar | Desabilita a proteção, que permitirá que qualquer pessoa acesse dados criptografados tornando a chave de criptografia disponível sem segurança na unidade. Nenhum protetor de chave foi removido. A proteção será retomada na próxima vez em que o Windows for inicializado, a menos que os parâmetros opcionais **-Disable** sejam usados para especificar a contagem de reinicialização. |
| -habilitar | Habilita a proteção removendo a chave de criptografia não segura da unidade. Todos os protetores de chave configurados na unidade serão aplicados. |
| -adbackup | Faz backup de todas as informações de recuperação da unidade especificada para Active Directory Domain Services (AD DS). Para fazer backup de uma única chave de recuperação para AD DS, acrescente o parâmetro **-ID** e ESPECIFIQUE a ID de uma chave de recuperação específica para fazer backup. |
| -aadbackup | Faz backup de todas as informações de recuperação da unidade especificada para Azure Active Directory (Azure AD). Para fazer backup de apenas uma única chave de recuperação para o Azure AD, acrescente o parâmetro **-ID** e ESPECIFIQUE a ID de uma chave de recuperação específica para fazer backup. |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

#### <a name="additional--add-parameters"></a>Adicional-adicionar parâmetros

O parâmetro-Add também pode usar esses parâmetros adicionais válidos.

```
manage-bde -protectors -add [<drive>] [-forceupgrade] [-recoverypassword <numericalpassword>] [-recoverykey <pathtoexternalkeydirectory>]
[-startupkey <pathtoexternalkeydirectory>] [-certificate {-cf <pathtocertificatefile>|-ct <certificatethumbprint>}] [-tpm] [-tpmandpin]
[-tpmandstartupkey <pathtoexternalkeydirectory>] [-tpmandpinandstartupkey <pathtoexternalkeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <name>]
[{-?|/?}] [{-help|-h}]
```

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -recoverypassword | Adiciona um protetor de senha numérica. Você também pode usar **-RP** como uma versão abreviada desse comando. |
| `<numericalpassword>` | Representa a senha de recuperação. |
| -recoverykey | Adiciona um protetor de chave externa para recuperação. Você também pode usar **-r** como uma versão abreviada deste comando. |
| `<pathtoexternalkeydirectory>` | Representa o caminho do diretório para a chave de recuperação. |
| -startupkey | Adiciona um protetor de chave externa para inicialização. Você também pode usar **-SK** como uma versão abreviada desse comando. |
| `<pathtoexternalkeydirectory>` | Representa o caminho do diretório para a chave de inicialização. |
| -certificado | Adiciona um protetor de chave pública para uma unidade de dados. Você também pode usar **-CERT** como uma versão abreviada desse comando. |
| -cf | Especifica que um arquivo de certificado será usado para fornecer o certificado de chave pública. |
| <pathtocertificatefile> | Representa o caminho do diretório para o arquivo de certificado. |
| -CT | Especifica que uma impressão digital do certificado será usada para identificar o certificado de chave pública |
| `<certificatethumbprint>` | Especifica o valor da propriedade de impressão digital do certificado que você deseja usar. Por exemplo, um valor de impressão digital do certificado de a9 09 50 2D D8 2a E4 14 33 E6 F8 38 86 B0 0d 42 77 a3 2a 7B deve ser especificado como a909502dd82ae41433e6f83886b00d4277a32a7b. |
| -tpmandpin | Adiciona um protetor de Trusted Platform Module (TPM) e número de identificação pessoal (PIN) para a unidade do sistema operacional. Você também pode usar **-TP** como uma versão abreviada desse comando. |
| -tpmandstartupkey | Adiciona um protetor de chave de inicialização e TPM para a unidade do sistema operacional. Você também pode usar **-tsk** como uma versão abreviada deste comando. |
| -tpmandpinandstartupkey | Adiciona um protetor de TPM, PIN e chave de inicialização para a unidade do sistema operacional. Você também pode usar **-tpsk** como uma versão abreviada deste comando. |
| -password | Adiciona um protetor de chave de senha para a unidade de dados. Você também pode usar **-PW** como uma versão abreviada desse comando. |
| -adaccountorgroup | Adiciona um protetor de identidade baseado em SID (identificador de segurança) para o volume. Você também pode usar **-Sid** como uma versão abreviada deste comando. **Importante:** Por padrão, você não pode adicionar um protetor ADaccountorgroup remotamente usando o WMI ou o Manage-bde. Se sua implantação exigir a capacidade de adicionar esse protetor remotamente, você deverá habilitar a delegação restrita. |
| -ComputerName | Especifica que o Manage-bde está sendo usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

#### <a name="additional--delete-parameters"></a>Parâmetros de exclusão adicionais

```
manage-bde -protectors -delete <drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}]
[-id <keyprotectorID>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -tipo | Identifica o protetor de chave a ser excluído. Você também pode usar **-t** como uma versão abreviada desse comando. |
| recoverypassword | Especifica que qualquer protetor de chave de senha de recuperação deve ser excluído. |
| externalkey | Especifica que qualquer protetor de chave externa associado à unidade deve ser excluído. |
| certificado | Especifica que qualquer protetor de chave de certificado associado à unidade deve ser excluído. |
| TPM | Especifica que qualquer protetor de chave somente TPM associado à unidade deve ser excluído. |
| tpmandstartupkey | Especifica que qualquer TPM e protetores de chave baseados em chave de inicialização associados à unidade devem ser excluídos. |
| tpmandpin | Especifica que qualquer protetor de chave com base em TPM e PIN associado à unidade deve ser excluído. |
| tpmandpinandstartupkey | Especifica que qualquer TPM, PIN e chave de inicialização com base em protetores de chave associados à unidade devem ser excluídos. |
| password | Especifica que qualquer protetor de chave de senha associado à unidade deve ser excluído. |
| identidade | Especifica que qualquer protetor de chave de identidade associado à unidade deve ser excluído. |
| -ID | Identifica o protetor de chave a ser excluído usando o identificador de chave. Esse parâmetro é uma opção alternativa para o parâmetro **-Type** . |
| `<keyprotectorID>` | Identifica um protetor de chave individual na unidade a ser excluída. As IDs de protetor de chave podem ser exibidas usando o comando **Manage-bde-protectors-get** . |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

#### <a name="additional--disable-parameters"></a>Parâmetros de desabilitação adicionais

```
manage-bde -protectors -disable <drive> [-rebootcount <integer 0 - 15>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| rebootcount | Especifica que a proteção do volume do sistema operacional foi suspensa e será retomada depois que o Windows tiver sido reiniciado o número de vezes especificado no parâmetro **rebootcount** . Especifique **0** para suspender a proteção indefinidamente. Se esse parâmetro não for especificado, a proteção do BitLocker será retomada automaticamente após a reinicialização do Windows. Você também pode usar **-RC** como uma versão abreviada deste comando. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para adicionar um protetor de chave de certificado, identificado por um arquivo de certificado, na unidade E, digite:

```
manage-bde -protectors -add E: -certificate -cf c:\File Folder\Filename.cer
```

Para adicionar um protetor de chave **adaccountorgroup** , identificado pelo nome de usuário e domínio, na unidade E, digite:

```
manage-bde -protectors -add E: -sid DOMAIN\user
```

Para desabilitar a proteção até que o computador tenha sido reinicializado 3 vezes, digite:

```
manage-bde -protectors -disable C: -rc 3
```

Para excluir todos os protetores de chave baseados em TPM e chaves de inicialização na unidade C, digite:

```
manage-bde -protectors -delete C: -type tpmandstartupkey
```

Para fazer backup de todas as informações de recuperação da unidade C para AD DS, digite:

```
manage-bde -protectors -adbackup C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
