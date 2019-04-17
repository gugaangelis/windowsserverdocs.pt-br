---
title: Importar pacotes de dados no servidor do Cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0bd8f12ab76c8e2bf03ba79ce46a4cbea2f4dc5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importar pacotes de dados em hospedado Cache \(Optional\) Server

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar este procedimento para importar pacotes de dados e pré-carregar conteúdo em seus servidores de cache hospedado.

Esse procedimento é opcional, porque não é necessário para conteúdo prehash e pré-carregamento em seus servidores de cache hospedado.

Se você fizer não pre\ carregar conteúdo, dados são adicionados para o cache hospedado automaticamente como clientes baixá-lo sobre a conexão WAN.

Você deve ser um membro do grupo Administradores para executar este procedimento.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Para importar pacotes de dados no servidor do cache hospedado  

1. No computador servidor, abra o Windows PowerShell com privilégios de administrador.

2. Digite o comando a seguir, substituindo o valor para o caminho parâmetro – com o local da pasta onde você armazenou seus pacotes de dados e pressione ENTER.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Se você tiver mais de um servidor de cache hospedado onde deseja pré-carregar conteúdo, execute este procedimento em cada servidor de cache hospedado.

Para continuar com este guia, consulte [configurar cliente hospedado Cache descoberta automática pelo serviço de Conexão ponto](10-Bc-Client-By-Scp.md).
