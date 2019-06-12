---
title: Gerenciar-bde protetores
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 78c0b338aa34684cc3c5da5ae4493f3526537ee6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437502"
---
# <a name="manage-bde-protectors"></a>Gerenciar-bde: protetores

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Gerencia os métodos de proteção usados para a chave de criptografia BitLocker. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxe
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                                                                                           Descrição                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -get      |                                                                                                                                            Exibe todos os métodos de proteção de chave habilitados na unidade e oferece seu tipo e identificador (ID).                                                                                                                                             |
|     -add      |                                                                                                                                   Adiciona métodos de proteção de chave conforme especificado pelo uso adicionais [adicionar parâmetros](manage-bde-protectors.md#BKMK_addprotectors).                                                                                                                                    |
|    -delete    | Exclui os métodos de proteção de chave usados pelo BitLocker. Todos os protetores de chave serão removidos de uma unidade, a menos que opcional [-excluir parâmetros](manage-bde-protectors.md#BKMK_deleteprotectors) são usados para especificar quais protetores para excluir. Quando o protetor de último em uma unidade é excluído, a proteção do BitLocker da unidade está desabilitada para garantir que o acesso a dados não seja perdido inadvertidamente. |
|   -desabilitar    |                      Desabilita a proteção, o que permitirá que qualquer pessoa acesse os dados criptografados, disponibilizando a chave de criptografia não segura na unidade. Nenhum protetor de chave é removidas. Proteção será retomada na próxima vez em que o Windows é inicializado, a menos que opcional [-desabilitar parâmetros](manage-bde-protectors.md#BKMK_disableprot) são usados para especificar a contagem de reinicialização.                       |
|    -Habilitar    |                                                                                                                             Habilita a proteção, removendo a chave de criptografia não seguros da unidade. Todos os protetores de chave configurados na unidade serão impostos.                                                                                                                             |
|   -adbackup   |                                                                          Faz backup de todas as informações de recuperação para a unidade especificada para os serviços de domínio do Active Directory (AD DS). Para fazer backup somente uma chave de recuperação único para o AD DS, acrescente a **-id** parâmetro e especifique a ID de uma chave de recuperação específico para fazer backup.                                                                           |
|  -aadbackup   |                                                                            Faz backup de todas as informações de recuperação para a unidade especificada para o Azure Ad (Active Directory do Azure). Para fazer backup somente uma chave de recuperação único para o Azure AD, acrescente a **-id** parâmetro e especifique a ID de uma chave de recuperação específico para fazer backup.                                                                             |
|    <Drive>    |                                                                                                                                                                          Representa uma letra de unidade seguida de dois-pontos.                                                                                                                                                                          |
| -computername |                                                                                                              Especifica que bde.exe gerenciar será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.                                                                                                              |
|    <Name>     |                                                                                                                 Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.                                                                                                                  |
|   -? ou /?    |                                                                                                                                                                            Exibe uma breve ajuda no prompt de comando.                                                                                                                                                                            |
|  -help ou -h  |                                                                                                                                                                          Exibe concluir ajuda no prompt de comando.                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-Adicionar a sintaxe e parâmetros
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          Parâmetro           |                                                                                                                                                                                   Descrição                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 Representa uma letra de unidade seguida de dois-pontos.                                                                                                                                                                  |
|      -recoverypassword       |                                                                                                                                    Adiciona um protetor de senha numérica. Você também pode usar **- rp** como uma versão abreviada desse comando.                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        Representa a senha de recuperação.                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                Adiciona um protetor de chave externa para recuperação. Você também pode usar **- rk** como uma versão abreviada desse comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               Representa o caminho do diretório para a chave de recuperação.                                                                                                                                                                |
|         -startupkey          |                                                                                                                                 Adiciona um protetor de chave externa para inicialização. Você também pode usar **sk -** como uma versão abreviada desse comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                Representa o caminho do diretório para a chave de inicialização.                                                                                                                                                                |
|         -certificate         |                                                                                                                               Adiciona um protetor de chave público para uma unidade de dados. Você também pode usar **-cert** como uma versão abreviada desse comando.                                                                                                                               |
|             -cf              |                                                                                                                                              Especifica que um arquivo de certificado será usado para fornecer o certificado de chave pública.                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             Representa o caminho do diretório para o arquivo de certificado.                                                                                                                                                              |
|             -ct              |                                                                                                                                           Especifica que uma impressão digital do certificado será usada para identificar o certificado de chave pública                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       Especifica o valor da propriedade impressão digital do certificado que você deseja usar. Por exemplo, um valor de impressão digital de certificado do "a9 09 50 2d d8 2a e4 14 f8 de 33 e6 38 86 7b de 2a de a3 77 do b0 0d 42" deve ser especificado como "a909502dd82ae41433e6f83886b00d4277a32a7b".                                                        |
|          -tpmandpin          |                                                                                           Adiciona um Trusted Platform Module (TPM) e um protetor (PIN) número de identificação pessoal para a unidade do sistema operacional. Você também pode usar **- tp** como uma versão abreviada desse comando.                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    Adiciona um protetor de chave TPM e inicialização na unidade do sistema operacional. Você também pode usar **-tsk** como uma versão abreviada desse comando.                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                Adiciona um TPM, o PIN e o protetor de chave de inicialização na unidade do sistema operacional. Você também pode usar **- tpsk** como uma versão abreviada desse comando.                                                                                                                 |
|          -senha           |                                                                                                                              Adiciona um protetor de chave de senha para a unidade de dados. Você também pode usar **- pw** como uma versão abreviada desse comando.                                                                                                                              |
|      -adaccountorgroup       | Adiciona um identificador de segurança (SID)-com base em protetor de identidade para o volume.  Você também pode usar **-sid** como uma versão abreviada desse comando. **IMPORTANTE:** Por padrão, é possível adicionar um protetor de ADAccountOrGroup remotamente usando o WMI ou gerenciar-bde.  Se sua implantação exigir a capacidade de adicionar essa protetor remotamente, você deve habilitar a delegação restrita. |
|        -computername         |                                                                                                       Especifica que gerenciar-bde está sendo usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.                                                                                                       |
|            <Name>            |                                                                                                         Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-Excluir sintaxe e parâmetros
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       Parâmetro        |                                                                              Descrição                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             Representa uma letra de unidade seguida de dois-pontos.                                                             |
|         -type          |                               Identifica o protetor de chave para excluir. Você também pode usar **-t** como uma versão abreviada desse comando.                               |
|    RecoveryPassword    |                                                 Especifica que qualquer protetores de chave de senha de recuperação devem ser excluídos.                                                 |
|      ExternalKey       |                                        Especifica que qualquer protetores de chave externas associadas com a unidade devem ser excluídos.                                         |
|      Certificado       |                                       Especifica que qualquer protetores de chave do certificado associados à unidade devem ser excluídos.                                       |
|          tpm           |                                        Especifica que qualquer somente TPM protetores de chave associados à unidade devem ser excluídos.                                         |
|    tpmandstartupkey    |                                Especifica que qualquer TPM e inicialização com base chave protetores de chave associados à unidade devem ser excluídos.                                |
|       tpmandpin        |                                    Especifica que qualquer TPM e PIN baseado protetores de chave associados à unidade deve ser excluído.                                    |
| tpmandpinandstartupkey |                             Especifica que qualquer inicialização TPM e PIN baseado em chave protetores de chave associados à unidade devem ser excluídos.                             |
|        password        |                                        Especifica que qualquer protetores de chave de senha associados com a unidade devem ser excluídos.                                         |
|        de autenticação        |                                        Especifica que qualquer protetores de chave de identidade associados à unidade devem ser excluídos.                                         |
|          -id           |                Identifica o protetor de chave para excluir, usando o identificador de chave. Esse parâmetro é uma opção alternativa para o **-tipo** parâmetro.                 |
|    <KeyProtectorID>    |        Identifica um protetor de chave individual na unidade para excluir. O protetor de chave IDs pode ser exibido usando o **gerenciar-bde-protetores-obter** comando.         |
|     -computername      | Especifica que bde.exe gerenciar será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando. |
|         <Name>         |    Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.     |
|        -? ou /?        |                                                               Exibe uma breve ajuda no prompt de comando.                                                               |
|      -help ou -h       |                                                             Exibe concluir ajuda no prompt de comando.                                                              |

### <a name="BKMK_disableprot"></a>-desabilitar a sintaxe e parâmetros
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   Parâmetro   |                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  Representa uma letra de unidade seguida de dois-pontos.                                                                                                                                                                                                  |
|  RebootCount  | Começando com o Windows 8, especifica que a proteção do volume do sistema operacional foi suspenso e será retomado quando o Windows tem sido reiniciado o número de vezes especificado no parâmetro RebootCount. Especifique 0 para suspender proteção indefinidamente. Se esse parâmetro não for especificado, a proteção do BitLocker será automaticamente continuar quando o Windows é reiniciado. Você também pode usar **-rc** como uma versão abreviada desse comando. |
| -computername |                                                                                                                                      Especifica que bde.exe gerenciar será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.                                                                                                                                      |
|    <Name>     |                                                                                                                                         Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.                                                                                                                                          |
|   -? ou /?    |                                                                                                                                                                                                    Exibe uma breve ajuda no prompt de comando.                                                                                                                                                                                                    |
|  -help ou -h  |                                                                                                                                                                                                  Exibe concluir ajuda no prompt de comando.                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>Exemplos
O exemplo a seguir ilustra o uso de **-protetores** comando para adicionar um protetor de chave de certificado identificado por um arquivo de certificado para a unidade E.
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
O exemplo a seguir ilustra o uso de **-protetores** comando para adicionar um **adaccountorgroup** protetor de chave identificado pelo nome de usuário e domínio para a unidade E.
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
O exemplo a seguir ilustra o uso de **protetores** comando para desabilitar a proteção até que o computador foi reinicializado a 3 vezes.
```
manage-bde  protectors  disable C: -rc 3
```
O exemplo a seguir ilustra o uso de **-protetores** protetores de chave com base em comando para excluir todos os TPM e chave de inicialização na unidade C.
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
O exemplo a seguir ilustra o uso de **-protetores** comando para fazer backup de todas as informações de recuperação para a unidade C para o AD DS.
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
