---
title: Exemplo de Shell (Netsh) de rede em arquivo de lote
description: Você pode usar este tópico para aprender a criar um arquivo em lotes que executa várias tarefas usando o netsh no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 86fbe66978f7c09a332bba16a27a13fa029cb5a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401914"
---
# <a name="network-shell-netsh-example-batch-file"></a>Arquivo em lotes de exemplo \(Netsh @ no__t-1 do Shell de rede

Aplica-se a: Windows Server 2016

Você pode usar este tópico para aprender a criar um arquivo em lotes que executa várias tarefas usando netsh no Windows Server 2016. Neste exemplo de arquivo em lotes, o contexto **netsh WINS** é usado.

## <a name="example-batch-file-overview"></a>Visão geral do arquivo em lotes de exemplo

Você pode usar comandos netsh para o serviço de nome da Internet do Windows \(WINS @ no__t-1 em arquivos em lotes e outros scripts para automatizar tarefas. O exemplo de arquivo em lotes a seguir demonstra como usar comandos netsh para o WINS para executar uma variedade de tarefas relacionadas.

Neste exemplo de arquivo em lotes, o WINS @ no__t-0A é um servidor WINS com o endereço IP 192.168.125.30 e o WINS @ no__t-1B é um servidor WINS com o endereço IP 192.168.0.189.

O arquivo em lotes de exemplo realiza as seguintes tarefas.

- Adiciona um registro de nome dinâmico com o endereço IP 192.168.0.205, meu @ no__t-0RECORD \[04h @ no__t-2, ao WINS @ no__t-3A
- Define o WINS @ no__t-0B como um parceiro de replicação push/pull do WINS @ no__t-1A
- Conecta-se ao WINS @ no__t-0B e, em seguida, define o WINS @ no__t-1A como um parceiro de replicação push/pull do WINS @ no__t-2B
- Inicia uma replicação de envio por push do WINS @ no__t-0A para o WINS @ no__t-1B
- Conecta-se ao WINS @ no__t-0B para verificar se o novo registro, meu @ no__t-1RECORD, foi replicado com êxito

## <a name="netsh-example-batch-file"></a>Arquivo em lotes de exemplo netsh

No arquivo em lotes de exemplo a seguir, as linhas que contêm comentários são precedidas por "REM", para o comentário. O netsh ignora os comentários.

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos netsh WINS usados no exemplo de arquivo em lotes

A seção a seguir lista os comandos **netsh WINS** que são usados neste procedimento de exemplo.

- **servidor**. Altera o contexto do comando do WINS atual @ no__t-0line para o servidor especificado por seu nome ou endereço IP.
- **adicionar nome**. Registra um nome no servidor WINS.
- **Adicionar parceiro**. Adiciona um parceiro de replicação no servidor WINS.
- **init push**. Inicia e envia um gatilho de envio por push para um servidor WINS.
- **Mostrar nome**. Exibe informações detalhadas de um registro específico no banco de dados do servidor WINS.  

Para obter mais informações, consulte [Network Shell (netsh)](netsh.md).
