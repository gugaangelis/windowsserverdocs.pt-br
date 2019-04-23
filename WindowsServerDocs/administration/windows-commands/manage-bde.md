---
title: manage-bde
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874497"
---
# <a name="manage-bde"></a>manage-bde



Usado para ativar ou desativar o BitLocker, especificar mecanismos de desbloqueio, atualizar métodos de recuperação e desbloquear unidades de dados protegidas pelo BitLocker. Essa ferramenta de linha de comando pode ser usada em vez do **BitLocker Drive Encryption** item do painel de controle. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Gerenciar-bde: status](manage-bde-status.md)|Fornece informações sobre todas as unidades no computador, se elas são protegidas pelo BitLocker.|
|[Gerenciar-bde: no](manage-bde-on.md)|Criptografa a unidade e ativa o BitLocker.|
|[Gerenciar-bde: desativado](manage-bde-off.md)|Descriptografa a unidade e desativa o BitLocker. Todos os protetores de chave são removidos quando a descriptografia for concluída.|
|[Gerenciar-bde: pause](manage-bde-pause.md)|Pausa a criptografia ou descriptografia.|
|[Gerenciar-bde: retomar](manage-bde-resume.md)|Retoma a criptografia ou descriptografia.|
|[Gerenciar-bde: bloqueio](manage-bde-lock.md)|Impede o acesso aos dados protegidos pelo BitLocker.|
|[Gerenciar-bde: desbloquear](manage-bde-unlock.md)|Permite o acesso aos dados protegidos pelo BitLocker com uma senha de recuperação ou uma chave de recuperação.|
|[Gerenciar-bde: desbloqueio automático](manage-bde-autounlock.md)|Gerencia o desbloqueio automático de unidades de dados.|
|[Gerenciar-bde: protetores](manage-bde-protectors.md)|Gerencia os métodos de proteção da chave de criptografia.|
|[Gerenciar-bde: tpm](manage-bde-tpm.md)|Configura o Trusted Platform Module (TPM) do computador. Esse comando não tem suporte em computadores que executam o Windows 8 ou **win8_server_2**. Para gerenciar o TPM nesses computadores, use o snap-in do MMC de gerenciamento do TPM ou os cmdlets de gerenciamento do TPM para o Windows PowerShell.|
|[Gerenciar-bde: setidentifier](manage-bde-setidentifier.md)|Define o campo de identificador de unidade na unidade para o valor especificado na **fornecem os identificadores exclusivos para sua organização** configuração de diretiva de grupo.|
|[Gerenciar-bde: ForceRecovery](manage-bde-forcerecovery.md)|Força uma unidade protegida pelo BitLocker no modo de recuperação na reinicialização. Esse comando exclui todos os protetores de chave relacionadas ao TPM da unidade. Quando o computador for reiniciado, somente uma senha ou chave de recuperação pode ser usada para desbloquear a unidade.|
|[Gerenciar-bde: changepassword](manage-bde-changepassword.md)|Modifica a senha para uma unidade de dados.|
|[Gerenciar-bde: changepin](manage-bde-changepin.md)|Modifica o PIN para uma unidade do sistema operacional.|
|[Gerenciar-bde: changekey](manage-bde-changekey.md)|Modifica a chave de inicialização para uma unidade do sistema operacional.|
|[Gerenciar-bde: KeyPackage](manage-bde-keypackage.md)|Gera um pacote de chaves para uma unidade.|
|[Gerenciar-bde: atualizar](manage-bde-upgrade.md)|Atualiza a versão do BitLocker.|
|[Gerenciar-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Apaga o espaço livre em uma unidade.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir exibe as unidades no computador e identifica se elas são protegidas pelo BitLocker e o status atual de criptografia.
```
manage-bde -status
```
O exemplo a seguir ilustra a habilitação de BitLocker na unidade C com a opção de uma senha de recuperação. A senha de recuperação será gerada pelo BitLocker e exibida na tela para que você pode gravá-la.
```
manage-bde –on C: -recoverypassword
```
O exemplo a seguir ilustra como desbloquear uma unidade protegida pelo BitLocker usando uma senha de recuperação.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Habilitar o BitLocker usando a linha de comando](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
