---
title: gpfixup
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6df3b2fabcbfad29576a8e8f48d6a1a87f0da2ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724967"
---
# <a name="gpfixup"></a>gpfixup



Corrija dependências de nome de domínio em objetos Política de Grupo e Política de Grupo links após uma operação de renomeação de domínio.

## <a name="syntax"></a>Sintaxe

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

#### <a name="parameters"></a>Parâmetros

|       Parâmetro       |                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      Exibe mensagens de status detalhadas.</br>Se esse parâmetro não for usado, somente mensagens de erro ou uma mensagem de status de Resumo de **êxito** ou **falha** serão exibidas.                                                                                                                                                       |
| /olddns:\<OLDDNSNAME> |                                                                                                           Especifica o nome DNS antigo do domínio renomeado como * \<OLDDNSNAME>* quando a operação de renomeação de domínio altera o nome DNS de um domínio. Você pode usar esse parâmetro somente se também usar o parâmetro **/newdns** para especificar um novo nome DNS de domínio.                                                                                                            |
| /newdns:\<NEWDNSNAME> |                                                                                                          Especifica o novo nome DNS do domínio renomeado como * \<NEWDNSNAME>* quando a operação de renomeação de domínio altera o nome DNS de um domínio. Você pode usar esse parâmetro somente se também usar o parâmetro **/olddns** para especificar o nome DNS do domínio antigo.                                                                                                           |
| /oldnb:\<OLDFLATNAME> |                                                                                                        Especifica o antigo nome NetBIOS do domínio renomeado como * \<OLDFLATNAME>* quando a operação de renomeação de domínio altera o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se usar o parâmetro **/newnb** para especificar um novo nome NetBIOS de domínio.                                                                                                        |
| /newnb:\<NEWFLATNAME> |                                                                                                       Especifica o novo nome NetBIOS do domínio renomeado como * \<NEWFLATNAME>* quando a operação de renomeação de domínio altera o nome NetBIOS de um domínio. Você pode usar esse parâmetro somente se usar o parâmetro **/oldnb** para especificar o nome NetBIOS do domínio antigo.                                                                                                       |
|     /DC:\<DCNAME>     | Conecte-se ao controlador de domínio chamado * \<DCNAME>* (um nome DNS ou um nome NetBIOS). DCNAME>deve hospedar uma réplica gravável da partição de diretório de domínio, conforme indicado por um dos seguintes: * \<*</br>-O nome * \<DNS NEWDNSNAME>* usando **/newdns**</br>-O nome * \<NetBIOS NEWFLATNAME>* usando **/newnb**</br>Se esse parâmetro não for usado, conecte-se a qualquer controlador de domínio no domínio renomeado indicado por * \<NEWDNSNAME>* ou * \<NEWFLATNAME>*. |
|        /sionly        |                                                                                                                           Executa apenas a correção de Política de Grupo relacionada à instalação de software gerenciado (a extensão de instalação de software para Política de Grupo). Ignore as ações que corrigem os links Política de Grupo e os caminhos do SYSVOL em GPOs.                                                                                                                           |
|   /User:\<username>   |                                                                                                                                   Executa esse comando no contexto de segurança do nome de usuário * \<>*, em que * \<username>* está no formato Domain\User.</br>Se esse parâmetro não for usado, o executará esse comando como o usuário conectado.                                                                                                                                    |
|   /pwd: {\<senha>   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentários

-   O comando **Gpfixup** está disponível no windows Server 2008 R2 e no windows Server 2008, exceto nas instalações do Server Core.
-   Embora o Console de Gerenciamento de Política de Grupo (GPMC) seja distribuído com o Windows Server 2008 R2 e o Windows Server 2008, você deve instalar o Política de Grupo Management como um recurso por meio de Gerenciador do Servidor.

## <a name="examples"></a>Exemplos

Este exemplo pressupõe que você já executou uma operação de renomeação de domínio em que você alterou o nome DNS de **MyOldDnsName** para **MyNewDnsName**e o nome NetBIOS de **MyOldNetBIOSName** para **MyNewNetBIOSName**. Neste exemplo, você usa o comando **Gpfixup** para se conectar ao controlador de domínio chamado **MyDcDnsName** e reparar GPOs e política de grupo links atualizando o nome de domínio antigo inserido nos GPOs e links. O status e a saída de erro são salvos em um arquivo chamado **Gpfixup. log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Este exemplo é o mesmo que o anterior, exceto que ele assume que o nome NetBIOS do domínio não foi alterado durante a operação de renomeação de domínio.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>Referências adicionais

-   [Administrando Domínio do Active Directory renomear](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [TechCenter de Política de Grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)