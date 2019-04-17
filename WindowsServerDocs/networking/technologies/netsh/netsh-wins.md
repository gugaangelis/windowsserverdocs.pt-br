---
title: Arquivo de lote de exemplo de Shell (Netsh) de rede
description: Você pode usar este tópico para saber como criar um arquivo em lotes que executa várias tarefas usando Netsh no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh-example-batch-file"></a>Exemplo de rede Shell \(Netsh\) lote arquivo

Aplica-se a: Windows Server 2016

Você pode usar este tópico para saber como criar um arquivo em lotes que executa várias tarefas usando Netsh no Windows Server 2016. No arquivo de lote neste exemplo, o **netsh wins** contexto é usado.

## <a name="example-batch-file-overview"></a>Visão geral do arquivo de lote de exemplo

Você pode usar comandos Netsh para o Windows Internet Name Service \(WINS\) em arquivos em lotes e outros scripts para automatizar tarefas. O exemplo de arquivo em lotes a seguir demonstra como usar comandos Netsh para WINS para executar uma variedade de tarefas relacionadas.

No arquivo de lote neste exemplo, WINS\-A é um servidor WINS com o endereço IP 192.168.125.30 e WINS\ B é um servidor WINS com o endereço IP 192.168.0.189.

O arquivo em lotes de exemplo realiza as seguintes tarefas.

- Adiciona um registro de nome dinâmico com endereço IP 192.168.0.205, MY\_RECORD \[04h\], WINS\-A para
- Define WINS\ B como um parceiro de replicação de envio/recepção de WINS\-A
- Se conecta a WINS\ B e define WINS\-A como um parceiro de replicação de envio/recepção de WINS\ B
- Inicia uma replicação de envio de WINS\-A para WINS\ B
- Se conecta a WINS\-B para verificar se o novo registro, MY\_RECORD, foi replicado com êxito

## <a name="netsh-example-batch-file"></a>Arquivo de lote de exemplo netsh

No arquivo de lote exemplo a seguir, linhas que contêm comentários são precedidas por "rem", de comentário. Netsh ignora os comentários.

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
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos Netsh WINS usados no arquivo em lotes de exemplo

A seguinte seção listas a **netsh wins** comandos que são usados neste procedimento de exemplo.

- **servidor**. Desloca o contexto de linha de command\ WINS atual para o servidor especificado pelo seu nome ou endereço IP.
- **Adicionar nome**. Registra um nome no servidor WINS.
- **Adicionar parceiro**. Adiciona um parceiro de replicação no servidor WINS.
- **init push**. Inicia e envia um disparador de envio para um servidor WINS.
- **Mostrar o nome**. Exibe informações detalhadas de um registro específico no banco de dados de servidor WINS.  

Para obter mais informações, consulte [Shell de rede (Netsh)](netsh.md).
