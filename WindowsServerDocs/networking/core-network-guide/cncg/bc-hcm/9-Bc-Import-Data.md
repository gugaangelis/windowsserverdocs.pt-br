---
title: Importar pacotes de dados no servidor de cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887997"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importar pacotes de dados no servidor de Cache hospedado \(opcional\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para importar pacotes de dados e pré-carregar conteúdo em seus servidores de cache hospedado.

Esse procedimento é opcional, porque você não é necessários para prehash e pré-carregamento de conteúdo em seus servidores de cache hospedado.

Se você fizer não pré\-carregar conteúdo, dados são adicionados ao cache hospedado automaticamente como clientes baixá-lo sobre a conexão de WAN.

Você deve ser membro do grupo Administradores para executar esse procedimento.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Para importar pacotes de dados no servidor de cache hospedado  

1. No computador do servidor, abra o Windows PowerShell com privilégios de administrador.

2. Digite o seguinte comando, substituindo o valor para o parâmetro-Path com o local da pasta onde você armazenou seus pacotes de dados e, em seguida, pressione ENTER.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Se você tiver mais de um servidor de cache hospedado, onde você deseja pré-carregar conteúdo, execute este procedimento em cada servidor de cache hospedado.

Para continuar com este guia, consulte [configurar o cliente Hosted Cache a descoberta automática pelo ponto de Conexão de serviço](10-Bc-Client-By-Scp.md).
