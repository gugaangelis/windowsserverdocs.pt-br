---
title: Limpar dados de utilização
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4454131c6a73626b0668b3ca3ab4eefb3e3334f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860619"
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
