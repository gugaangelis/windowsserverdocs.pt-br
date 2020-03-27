---
title: Limpar dados de utilização
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a8542e643a9c4d33acad18523fd34eed8926413d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316770"
---
# <a name="purge-utilization-data"></a>Limpar dados de utilização

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber como excluir dados de utilização do banco de dado do IPAM.  

Você deve ser membro de **Administradores do IPAM**, do grupo **Administradores** do computador local ou equivalente, para executar esse procedimento.

## <a name="to-purge-the-ipam-database"></a>Para limpar o banco de dados IPAM  
1. Abra Gerenciador do Servidor e, em seguida, navegue até a interface do cliente IPAM.
2. Navegue até um dos seguintes locais: **blocos de endereço IP**, **inventário de endereço IP**ou **grupos de intervalos de endereços IP**.  
3. Clique em **tarefas**e, em seguida, clique em **limpar dados de utilização**. A caixa de diálogo **limpar dados de utilização** é aberta.
4. Em **limpar todos os dados de utilização em ou antes**, clique em **selecionar uma data**.
5. Escolha a data para a qual você deseja excluir todos os registros de banco de dados e antes dessa data.
6. Clique em **OK**. IPAM exclui todos os registros que você especificou.
