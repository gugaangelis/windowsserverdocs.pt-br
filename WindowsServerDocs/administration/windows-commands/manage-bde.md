---
title: manage-bde
description: Artigo de referência para o comando Manage-bde, que ativa ou desativa o BitLocker, especifica mecanismos de desbloqueio, atualiza métodos de recuperação e desbloqueia unidades de dados protegidas pelo BitLocker.
ms.topic: reference
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d29b123e6087106e3697c3343b023ed19967664b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634085"
---
# <a name="manage-bde"></a>manage-bde

Ativa ou desativa o BitLocker, especifica mecanismos de desbloqueio, atualiza métodos de recuperação e desbloqueia unidades de dados protegidas pelo BitLocker.

> [!NOTE]
> Essa ferramenta de linha de comando pode ser usada no lugar do **criptografia de unidade de disco BitLocker** item do painel de controle.

## <a name="syntax"></a>Sintaxe

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm]
[–setidentifier] [-forcerecovery] [–changepassword] [–changepin] [–changekey] [-keypackage] [–upgrade] [-wipefreespace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| [gerenciar-status do bde](manage-bde-status.md) | Fornece informações sobre todas as unidades no computador, se elas são protegidas pelo BitLocker ou não. |
| [Manage-bde em](manage-bde-on.md) | Criptografa a unidade e ativa o BitLocker. |
| [Manage-bde desativado](manage-bde-off.md) | Descriptografa a unidade e desativa o BitLocker. Todos os protetores de chave são removidos quando a descriptografia é concluída. |
| [Manage-bde pausar](manage-bde-pause.md) | Pausa a criptografia ou descriptografia. |
| [gerenciar – retomar o BDE](manage-bde-resume.md) | Retoma a criptografia ou descriptografia. |
| [gerenciar o bloqueio do bde](manage-bde-lock.md) | Impede o acesso a dados protegidos pelo BitLocker. |
| [gerenciar/desbloquear o BDE](manage-bde-unlock.md) | Permite o acesso a dados protegidos pelo BitLocker com uma senha de recuperação ou uma chave de recuperação. |
| [gerenciar o desbloqueio automático do bde](manage-bde-autounlock.md) | Gerencia o desbloqueio automático de unidades de dados. |
| [Manage-bde protetores](manage-bde-protectors.md) | Gerencia métodos de proteção para a chave de criptografia. |
| [gerenciar o TPM do bde](manage-bde-tpm.md) | Configura o Trusted Platform Module do computador (TPM). Não há suporte para esse comando em computadores que executam o Windows 8 ou o **win8_server_2**. Para gerenciar o TPM nesses computadores, use o snap-in MMC de gerenciamento do TPM ou os cmdlets de gerenciamento do TPM para Windows PowerShell. |
| [gerenciar-bde setidentifier](manage-bde-setidentifier.md)   | Define o campo identificador da unidade na unidade para o valor especificado na configuração **fornecer os identificadores exclusivos para sua organização** política de grupo. |
| [Manage-bde ForceRecovery](manage-bde-forcerecovery.md) | Força uma unidade protegida pelo BitLocker no modo de recuperação na reinicialização. Esse comando exclui todos os protetores de chave relacionados ao TPM da unidade. Quando o computador é reiniciado, apenas uma senha de recuperação ou chave de recuperação pode ser usada para desbloquear a unidade. |
| [ChangePassword de Manage-bde](manage-bde-changepassword.md) | Modifica a senha de uma unidade de dados. |
| [Manage-bde changepin](manage-bde-changepin.md) | Modifica o PIN de uma unidade do sistema operacional. |
| [Manage-bde ChangeKey](manage-bde-changekey.md) | Modifica a chave de inicialização para uma unidade do sistema operacional. |
| [Manage-bde pacote de pacotes](manage-bde-keypackage.md) | Gera um pacote de chaves para uma unidade. |
| [atualização do Manage-bde](manage-bde-upgrade.md) | Atualiza a versão do BitLocker. |
| [Manage-bde WipeFreeSpace](manage-bde-wipefreespace.md) | Apaga o espaço livre em uma unidade. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Habilitando o BitLocker usando a linha de comando](/previous-versions/windows/it-pro/windows-7/dd894351(v=ws.10))
