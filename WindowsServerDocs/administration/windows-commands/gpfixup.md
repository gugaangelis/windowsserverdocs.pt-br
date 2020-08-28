---
title: gpfixup
description: Artigo de referência para o comando Gpfixup, que corrige dependências de nome de domínio em objetos Política de Grupo e Política de Grupo links após uma operação de renomeação de domínio.
ms.topic: reference
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b11ae09bcc345c9fb656a8945e0febe5f4098ac
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025690"
---
# <a name="gpfixup"></a>gpfixup

Corrige dependências de nome de domínio em objetos Política de Grupo e Política de Grupo links após uma operação de renomeação de domínio. Para usar esse comando, você deve instalar Política de Grupo gerenciamento como um recurso por meio de Gerenciador do Servidor.

## <a name="syntax"></a>Sintaxe

```
gpfixup [/v]
[/olddns:<olddnsname> /newdns:<newdnsname>]
[/oldnb:<oldflatname> /newnb:<newflatname>]
[/dc:<dcname>] [/sionly]
[/user:<username> [/pwd:{<password>|*}]] [/?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| /v | Exibe mensagens de status detalhadas. Se esse parâmetro não for usado, somente mensagens de erro ou uma mensagem de status de resumo informando, **êxito** ou **falha** aparecerão. |
| /olddns:`<olddnsname>` | Especifica o nome DNS antigo do domínio renomeado como `<olddnsname>` quando a operação de renomeação de domínio altera o nome DNS de um domínio. Você pode usar esse parâmetro somente se também usar o parâmetro **/newdns** para especificar um novo nome DNS de domínio. |
| /newdns:`<newdnsname>` | Especifica o novo nome DNS do domínio renomeado como `<newdnsname>` quando a operação de renomeação de domínio altera o nome DNS de um domínio. Você pode usar esse parâmetro somente se também usar o parâmetro **/olddns** para especificar o nome DNS do domínio antigo. |
| /oldnb:`<oldflatname>` | Especifica o antigo nome NetBIOS do domínio renomeado como `<oldflatname>` quando a operação de renomeação de domínio altera o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se usar o parâmetro **/newnb** para especificar um novo nome NetBIOS de domínio. |
| /newnb:`<newflatname>` | Especifica o novo nome NetBIOS do domínio renomeado como `<newflatname>` quando a operação de renomeação de domínio altera o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se usar o parâmetro **/oldnb** para especificar o nome NetBIOS do domínio antigo. |
| origem`<dcname>` | Conecte-se ao controlador de domínio chamado `<dcname>` (um nome DNS ou um nome NetBIOS). `<dcname>` deve hospedar uma réplica gravável da partição de diretório de domínio, conforme indicado por um dos seguintes:<ul><li>O nome DNS `<newdnsname>` usando **/newdns**</li><li>O nome NetBIOS `<newflatname>` usando **/newnb**</br>Se esse parâmetro não for usado, você poderá se conectar a qualquer controlador de domínio no domínio renomeado indicado por `<newdnsname>` ou `<newflatname>` .</li></ul> |
| /sionly | Executa apenas a correção de Política de Grupo relacionada à instalação de software gerenciado (a extensão de instalação de software para Política de Grupo). Ignore as ações que corrigem os links Política de Grupo e os caminhos do SYSVOL em GPOs. |
| /`<username>` |Executa esse comando no contexto de segurança do usuário `<username>` , onde `<username>` está no formato Domain\User. Se esse parâmetro não for usado, esse comando será executado como o usuário conectado. |
| /pwd`{<password> | *}` | Especifica a senha do usuário. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Este exemplo pressupõe que você já executou uma operação de renomeação de domínio em que você alterou o nome DNS de **MyOldDnsName** para **MyNewDnsName**e o nome NetBIOS de **MyOldNetBIOSName** para **MyNewNetBIOSName**.

Neste exemplo, você usa o comando **Gpfixup** para se conectar ao controlador de domínio chamado **MyDcDnsName** e reparar GPOs e política de grupo links atualizando o nome de domínio antigo inserido nos GPOs e links. O status e a saída de erro são salvos em um arquivo chamado **Gpfixup. log**.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

Este exemplo é o mesmo que o anterior, exceto que ele assume que o nome NetBIOS do domínio não foi alterado durante a operação de renomeação de domínio.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Administrando Domínio do Active Directory renomear](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794869(v=ws.10))
