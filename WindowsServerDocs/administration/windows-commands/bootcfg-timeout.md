---
title: bootcfg timeout
description: Tópico de comandos do Windows para Bootcfg timeout, que altera o valor de tempo limite do sistema operacional.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56e609d5e3b7c92a887a98ae5d02bfbfb7a78e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848489"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o valor de tempo limite do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```

### <a name="parameters"></a>Parâmetros


|        Parâmetro        |                                                                                                                                                                                  Descrição                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Timeout <timeOutValue> | Especifica o valor de tempo limite na seção [carregador de inicialização]. A <timeOutValue> é o número de segundos que o usuário tem para selecionar um sistema operacional na tela do carregador de inicialização antes de o NTLDR carregar o padrão. O intervalo válido para <timeOutValue> é 0-999. Se o valor for 0, o NTLDR iniciará imediatamente o sistema operacional padrão sem exibir a tela do carregador de inicialização. |
|      /s <computer>      |                                                                                                                               Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                                                               |
|    /u < domínio \ usuário >     |                                                                                       Executa o comando com as permissões de conta do usuário especificado por <User> ou < domínio \ usuário >. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.                                                                                        |
|      /p <Password>      |                                                                                                                                            Especifica o <Password> da conta de usuário que é especificada no parâmetro **/u** .                                                                                                                                             |
|           /?            |                                                                                                                                                                      Exibe a ajuda no prompt de comando.                                                                                                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Timeout** :
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
