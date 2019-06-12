---
title: gpfixup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efb30e243d9c165fdcf13943225eb90d38235070
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438211"
---
# <a name="gpfixup"></a>gpfixup



Corrigi as dependências de nome de domínio em links de objetos de diretiva de grupo e a diretiva de grupo após a operação de renomeação de um domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Parâmetros

|       Parâmetro       |                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      Exibe mensagens de status detalhado.</br>Se esse parâmetro não for usado, somente mensagens de erro ou uma mensagem de resumo de status de **sucesso** ou **falha** é exibida.                                                                                                                                                       |
| /olddns:\<OLDDNSNAME> |                                                                                                           Especifica o nome antigo do DNS do domínio renomeado como  *\<OLDDNSNAME >* quando a operação de renomeação de domínio muda o nome DNS de um domínio. Você pode usar esse parâmetro somente se você também usar o **/newdns** parâmetro para especificar um novo nome de domínio DNS.                                                                                                            |
| /newdns:\<NEWDNSNAME> |                                                                                                          Especifica o novo nome DNS do domínio renomeado como  *\<NEWDNSNAME >* quando a operação de renomeação de domínio muda o nome DNS de um domínio. Você pode usar esse parâmetro somente se você também usar o **/olddns** parâmetro para especificar o nome DNS do domínio antigo.                                                                                                           |
| /oldnb:\<OLDFLATNAME> |                                                                                                        Especifica o nome antigo do NetBIOS do domínio renomeado como  *\<OLDFLATNAME >* quando a operação de renomeação de domínio muda o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se você usar o **/newnb** parâmetro para especificar um novo nome de NetBIOS do domínio.                                                                                                        |
| /newnb:\<NEWFLATNAME> |                                                                                                       Especifica o novo nome NetBIOS do domínio renomeado como  *\<NEWFLATNAME >* quando a operação de renomeação de domínio muda o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se você usar o **/oldnb** parâmetro para especificar o nome de NetBIOS do domínio antigo.                                                                                                       |
|     /dc:\<DCNAME>     | Conectar-se ao controlador de domínio denominado  *\<DCNAME >* (um nome DNS ou um nome NetBIOS). *\<DCNAME >* deve hospedar uma réplica gravável da partição de diretório do domínio, conforme indicado por um dos seguintes:</br>-O nome DNS  *\<NEWDNSNAME >* usando **/newdns**</br>-O nome NetBIOS  *\<NEWFLATNAME >* usando **/newnb**</br>Se esse parâmetro não for usado, se conectar a qualquer controlador de domínio no domínio renomeado indicado por  *\<NEWDNSNAME >* ou  *\<NEWFLATNAME >* . |
|        /sionly        |                                                                                                                           Executa somente a correção de diretiva de grupo relacionadas à instalação de softwares gerenciados (a extensão de instalação do Software de diretiva de grupo). Ignore as ações que corrigem links de diretiva de grupo e os caminhos SYSVOL nos GPOs.                                                                                                                           |
|   /User:\<USERNAME >   |                                                                                                                                   Executa este comando no contexto de segurança do usuário  *\<nome de usuário >* , onde  *\<USERNAME >* está no formato domínio \ usuário.</br>Se esse parâmetro não for usado, executa este comando como o usuário conectado.                                                                                                                                    |
|   /pwd: {\<senha >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentários

-   O **gpfixup** comando está disponível no Windows Server 2008 R2 e Windows Server 2008, exceto em instalações Server Core.
-   Embora o grupo de política de gerenciamento GPMC (Console) é distribuída com o Windows Server 2008 R2 e Windows Server 2008, você deve instalar o gerenciamento de diretiva de grupo como um recurso via Gerenciador do servidor.

## <a name="BKMK_Examples"></a>Exemplos

Este exemplo supõe que você já executou uma operação de renomeação de domínio no qual você alterou o nome DNS de **MyOldDnsName** à **MyNewDnsName**e o nome NetBIOS do  **MyOldNetBIOSName** à **MyNewNetBIOSName**. Neste exemplo, você deve usar o **gpfixup** comando para se conectar ao controlador de domínio denominado **MyDcDnsName** e reparar GPOs e diretiva de grupo links atualizando o nome do domínio antigo incorporados nas GPOs e links. Saída de status e o erro é salva em um arquivo chamado **gpfixup.log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Este exemplo é o mesmo que o anterior, exceto que ele assume que o nome NetBIOS do domínio não foi alterado durante o domínio de renomear a operação.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>Referências adicionais

-   [Administrando a renomeação de domínio do Active Directory](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [Group Policy TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)