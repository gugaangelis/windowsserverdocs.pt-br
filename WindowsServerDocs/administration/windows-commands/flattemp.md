---
title: flattemp
description: Tópico de referência para o comando flattemp, que habilita ou desabilita pastas temporárias simples.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a30a3f7eb6ec56a499864116debfbb6c09756d34
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437221"
---
# <a name="flattemp"></a>flattemp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita ou desabilita pastas temporárias simples. Você deve ter credenciais administrativas para executar este comando.

> [!NOTE]
> Esse comando só estará disponível se você tiver instalado o serviço de função Host da Sessão da Área de Trabalho Remota.

## <a name="syntax"></a>Sintaxe

```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Query | Consulta a configuração atual. |
| /Enable | Habilita pastas temporárias simples. Os usuários irão compartilhar a pasta temporária, a menos que a pasta temporária resida na pasta base do usuário. |
| /Disable | Desabilita as pastas temporárias simples. A pasta temporária de cada usuário residirá em uma pasta separada (determinada pela ID de sessão do usuário). |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Depois que cada usuário tiver uma pasta temporária exclusiva, use `flattemp /enable` para habilitar pastas temporárias simples.

- O método padrão para criar pastas temporárias para vários usuários (geralmente apontados pelas variáveis de ambiente TEMP e TMP) é criar subpastas na pasta **\temp** , usando o LogonId como o nome da subpasta. Por exemplo, se a variável de ambiente TEMP aponta para C:\Temp, a pasta temporária atribuída ao usuário LogonId 4 é C:\Temp\4.

    Usando **flattemp**, você pode apontar diretamente para a pasta \temp e impedir que as subpastas sejam formadas. Isso é útil quando você deseja que as pastas temporárias do usuário estejam contidas em pastas base, seja em uma unidade local do Host da Sessão da Área de Trabalho Remota Server ou em uma unidade de rede compartilhada. Você deve usar o `flattemp /enable*` comando somente quando cada usuário tiver uma pasta temporária separada.

- Você poderá encontrar erros de aplicativo se a pasta temporária do usuário estiver em uma unidade de rede. Isso ocorre quando a unidade de rede compartilhada torna-se momentaneamente inacessível na rede. Como os arquivos temporários do aplicativo estão inacessíveis ou fora de sincronização, ele responde como se o disco fosse interrompido. Não é recomendável mover a pasta temporária para uma unidade de rede. O padrão é manter as pastas temporárias no disco rígido local. Se você tiver um comportamento inesperado ou erros de corrupção de disco com determinados aplicativos, estabilize sua rede ou mova as pastas temporárias de volta para o disco rígido local.

- Se você desabilitar o uso de pastas temporárias separadas por sessão, as configurações de **flattemp** serão ignoradas. Essa opção é definida na ferramenta de configuração do Serviços de Área de Trabalho Remota.

### <a name="examples"></a>Exemplos

Para exibir a configuração atual de pastas temporárias simples, digite:

```
flattemp /query
```

Para habilitar pastas temporárias simples, digite:

```
flattemp /enable
```

Para desabilitar pastas temporárias simples, digite:

```
flattemp /disable
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

