---
title: bootcfg addsw
description: Tópico de referência para o comando Bootcfg addsw, que adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17abdc1ba28afad173ea6486519277916f08ad3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709941"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona opções de carregamento do sistema operacional para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe

```
bootcfg /addsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm <maximumram>] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parâmetros

| Termo | Definição |
| ---- | ---------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>`. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `/mm <maximumram>` | Especifica a quantidade máxima de RAM, em megabytes, que o sistema operacional pode usar. O valor deve ser igual ou maior que 32 megabytes. |
| /bv | Adiciona a opção **/basevideo** ao especificado `<osentrylinenum>`, direcionando o sistema operacional para usar o modo VGA padrão para o driver de vídeo instalado. |
| /so | Adiciona a opção **/SOS** ao especificado `<osentrylinenum>`, direcionando o sistema operacional para exibir nomes de driver de dispositivo enquanto eles estiverem sendo carregados. |
| /ng | Adiciona a opção **/noguiboot** ao especificado `<osentrylinenum>`, desabilitando a barra de progresso que aparece antes do prompt de logon Ctrl + Alt + Del. |
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/addsw** :

```
bootcfg /addsw /mm 64 /id 2
bootcfg /addsw /so /id 3
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /addsw /ng /id 2
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
