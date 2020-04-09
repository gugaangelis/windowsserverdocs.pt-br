---
title: manage-bde
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816e20152ec40ce54c1192f3075c6f4556aed3db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839689"
---
# <a name="manage-bde"></a>manage-bde



Usado para ativar ou desativar o BitLocker, especificar mecanismos de desbloqueio, atualizar métodos de recuperação e desbloquear unidades de dados protegidas pelo BitLocker. Essa ferramenta de linha de comando pode ser usada no lugar do **criptografia de unidade de disco BitLocker** item do painel de controle. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|Fornece informações sobre todas as unidades no computador, se elas são protegidas pelo BitLocker ou não.|
|[Manage-bde: on](manage-bde-on.md)|Criptografa a unidade e ativa o BitLocker.|
|[Manage-bde: off](manage-bde-off.md)|Descriptografa a unidade e desativa o BitLocker. Todos os protetores de chave são removidos quando a descriptografia é concluída.|
|[Manage-bde: pause](manage-bde-pause.md)|Pausa a criptografia ou descriptografia.|
|[Manage-bde: resume](manage-bde-resume.md)|Retoma a criptografia ou descriptografia.|
|[Manage-bde: lock](manage-bde-lock.md)|Impede o acesso a dados protegidos pelo BitLocker.|
|[Manage-bde: unlock](manage-bde-unlock.md)|Permite o acesso a dados protegidos pelo BitLocker com uma senha de recuperação ou uma chave de recuperação.|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|Gerencia o desbloqueio automático de unidades de dados.|
|[Manage-bde: protectors](manage-bde-protectors.md)|Gerencia métodos de proteção para a chave de criptografia.|
|[Manage-bde: tpm](manage-bde-tpm.md)|Configura o Trusted Platform Module do computador (TPM). Não há suporte para esse comando em computadores que executam o Windows 8 ou o **win8_server_2**. Para gerenciar o TPM nesses computadores, use o snap-in MMC de gerenciamento do TPM ou os cmdlets de gerenciamento do TPM para Windows PowerShell.|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|Define o campo identificador da unidade na unidade para o valor especificado na configuração **fornecer os identificadores exclusivos para sua organização** política de grupo.|
|[Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)|Força uma unidade protegida pelo BitLocker no modo de recuperação na reinicialização. Esse comando exclui todos os protetores de chave relacionados ao TPM da unidade. Quando o computador é reiniciado, apenas uma senha de recuperação ou chave de recuperação pode ser usada para desbloquear a unidade.|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|Modifica a senha de uma unidade de dados.|
|[Manage-bde: changepin](manage-bde-changepin.md)|Modifica o PIN de uma unidade do sistema operacional.|
|[Manage-bde: changekey](manage-bde-changekey.md)|Modifica a chave de inicialização para uma unidade do sistema operacional.|
|[Manage-bde: KeyPackage](manage-bde-keypackage.md)|Gera um pacote de chaves para uma unidade.|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|Atualiza a versão do BitLocker.|
|[Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Apaga o espaço livre em uma unidade.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir exibe as unidades no computador e identifica se elas são protegidas pelo BitLocker e o status de criptografia atual.
```
manage-bde -status
```
O exemplo a seguir ilustra como habilitar o BitLocker na unidade C com a opção de uma senha de recuperação. A senha de recuperação será gerada pelo BitLocker e exibida na tela para que você possa gravá-la.
```
manage-bde –on C: -recoverypassword
```
O exemplo a seguir ilustra o desbloqueio de uma unidade protegida pelo BitLocker usando uma senha de recuperação.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Habilitando o BitLocker usando a linha de comando](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
