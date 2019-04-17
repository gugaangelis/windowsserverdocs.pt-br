---
title: Limpar dados de utilização
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>Limpar dados de utilização

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como excluir dados de utilização do banco de dados IPAM.  

Você deve ser um membro do **IPAM administradores**, o computador local **administradores** grupo, ou equivalente, para executar este procedimento.

## <a name="to-purge-the-ipam-database"></a>Para limpar o banco de dados IPAM  
1. Abra o Gerenciador do servidor e, em seguida, navegue até a interface do cliente IPAM.
2. Navegue até um dos seguintes locais: **blocos de endereço IP**, **inventário de endereço IP**, ou **grupos de intervalo de endereços IP**.  
3. Clique em **tarefas**e clique em **limpar dados de utilização da**. O **limpar dados de utilização da** caixa de diálogo é aberta.
4. Em **limpar a utilização de todos os dados na data ou antes**, clique em **selecionar uma data**.
5. Escolha a data para o qual você deseja excluir todos os registros do banco de dados em e antes dessa data.
6. Clique em **Okey**. IPAM exclui todos os registros que você especificou.
