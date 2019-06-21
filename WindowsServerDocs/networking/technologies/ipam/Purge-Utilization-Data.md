---
title: Limpar dados de utilização
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab2bd3ad1ef8965400e09fa74c6eb89ffc5ebcef
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283838"
---
# <a name="purge-utilization-data"></a>Limpar dados de utilização

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber como excluir dados de utilização do banco de dados IPAM.  

Você deve ser um membro da **os administradores do IPAM**, o computador local **administradores** grupo, ou equivalente, para executar este procedimento.

## <a name="to-purge-the-ipam-database"></a>Limpar o banco de dados do IPAM  
1. Abra o Gerenciador do servidor e, em seguida, navegue até a interface do cliente IPAM.
2. Navegue até um dos seguintes locais: **Blocos de endereço IP**, **inventário de endereço IP**, ou **grupos de intervalos de endereço IP**.  
3. Clique em **tarefas**e, em seguida, clique em **limpar dados de utilização**. O **limpar dados de utilização** caixa de diálogo é aberta.
4. Na **limpar a utilização de todos os dados em ou antes**, clique em **selecionar uma data**.
5. Escolha a data para o qual você deseja excluir todos os registros de banco de dados em e antes da data.
6. Clique em **OK**. IPAM exclui todos os registros que você especificou.
