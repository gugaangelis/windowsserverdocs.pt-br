---
title: Arquivo em lotes de exemplo do Netsh (shell de rede)
description: Você pode usar este tópico para aprender a criar um arquivo em lotes que executa várias tarefas usando o Netsh no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f0cc6818b589c6ee2ac64115fe9e7fb71c3d20b1
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517901"
---
# <a name="network-shell-netsh-example-batch-file"></a>Arquivo em lotes de exemplo do Netsh (shell de rede)

Aplica-se a: Windows Server 2016

Você pode usar este tópico para aprender a criar um arquivo em lotes que executa várias tarefas usando o Netsh no Windows Server 2016. Este exemplo de arquivo em lotes usa o contexto **netsh wins**.

## <a name="example-batch-file-overview"></a>Visão geral do arquivo em lote de exemplo

Você pode usar comandos Netsh para o \(WINS\) (Serviço de Nome da Internet do Windows) em arquivos de lotes e outros scripts para automatizar tarefas. O exemplo de arquivo em lotes a seguir demonstra como usar comandos Netsh para o WINS para executar uma variedade de tarefas relacionadas.

Neste exemplo de arquivo em lotes, WINS\-A é um servidor WINS com o endereço IP 192.168.125.30 e WINS\-B é um servidor WINS com o endereço IP 192.168.0.189.

O arquivo em lotes de exemplo realiza as tarefas a seguir.

- Adiciona um registro de nome dinâmico com o endereço IP 192.168.0.205, MY\_RECORD \[04h\] a WINS\-A
- Define o WINS\-B como um parceiro de replicação push/pull do WINS\-A
- Conecta-se ao WINS\-B e, em seguida, define o WINS\-A como um parceiro de replicação push/pull do WINS\-B
- Inicia uma replicação push do WINS\-A para o WINS\-B
- Conecta-se ao WINS\-B para verificar se o novo registro, MEU\_REGISTRO, foi replicado com êxito

## <a name="netsh-example-batch-file"></a>Arquivo em lotes de exemplo Netsh

No exemplo de arquivo em lotes a seguir, as linhas que contêm comentários são precedidas por "rem", de remark (comentário, em inglês). O Netsh ignora os comentários.

```
rem: Begin example batch file.
rem two WINS servers:
rem (WINS-A) 192.168.125.30
rem (WINS-B) 192.168.0.189

rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}

rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2

rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2

rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0

rem 5. Connect to (WINS-B), and check that the record MY_RECORD [04h] was replicated successfully.
netsh wins server 192.168.0.189 show name Name=MY_RECORD EndChar=04

rem 6. End example batch file.
```

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos WINS do Netsh usados no exemplo de arquivo em lotes

A seção a seguir lista os comandos **netsh wins** usados neste procedimento de exemplo.

- **server**. Desloca o contexto atual da linha de\-comando do WINS para o servidor especificado pelo seu nome ou endereço IP.
- **add name**. Registra um nome no servidor WINS.
- **add partner**. Adiciona um parceiro de replicação no servidor WINS.
- **init push**. Inicia e envia um gatilho de envio para um servidor WINS.
- **show name**. Exibe informações detalhadas de um registro específico no banco de dados do servidor WINS.

## <a name="additional-references"></a>Referências adicionais

- [Shell de Rede (Netsh)](netsh.md)
