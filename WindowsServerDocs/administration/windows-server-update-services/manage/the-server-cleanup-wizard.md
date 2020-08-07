---
title: O Assistente para limpeza de servidor
description: Tópico Windows Server Update Service (WSUS)-como usar o assistente de limpeza do servidor para gerenciar o espaço em disco
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85713dc245e8e812fa0c715d037738feaeb1ca0e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891672"
---
# <a name="the-server-cleanup-wizard"></a>O Assistente para limpeza de servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O assistente de limpeza do servidor é integrado à interface do usuário e pode ser usado para ajudá-lo a gerenciar o espaço em disco. Este assistente pode realizar as seguintes operações:

- Remova atualizações não usadas e revisões de atualização remova todas as atualizações mais antigas e revisões de atualização que não foram aprovadas.

- Excluir computadores que não estão contatando o servidor exclua todos os computadores cliente que não contataram o servidor em trinta dias ou mais.

- Excluir arquivos de atualização desnecessários exclua todos os arquivos de atualização que não são necessários para atualizações ou servidores downstream.

- Recusar atualizações expiradas recusam todas as atualizações expiradas pela Microsoft.

- Recusar atualizações substituídas recusam todas as atualizações que atendem a todos os seguintes critérios:

  -   A atualização substituída não é obrigatória

  -   A atualização substituída esteve no servidor por trinta dias ou mais

  -   A atualização substituída não é atualmente relatada conforme a necessidade de qualquer cliente

  -   A atualização substituída não foi implantada explicitamente em um grupo de computadores por 90 dias ou mais

  -   A atualização substituta deve ser aprovada para instalação em um grupo de computadores

  > [!WARNING]
  >  Em uma hierarquia do WSUS, é altamente recomendável que você execute o processo de limpeza no servidor do WSUS mais baixo, downstream/réplica primeiro e, em seguida, mova para cima a hierarquia. A execução incorreta de limpeza em qualquer servidor upstream antes de executar a limpeza em cada servidor downstream pode causar uma incompatibilidade entre os dados que estão presentes em bancos de dados upstream e bancos de dados downstream. A incompatibilidade de dados pode levar a falhas de sincronização entre os servidores upstream e downstream.
  >
  > [!IMPORTANT]
  >  Se você remover o conteúdo desnecessário usando o assistente para limpeza do servidor, todos os arquivos de atualização particulares que você baixou do site do catálogo Microsoft Update também serão removidos. Você deve reimportar esses arquivos depois de executar o assistente de limpeza do servidor.

Se as atualizações forem aprovadas usando uma regra de aprovação automática, elas ainda poderão estar no estado aprovado e não serão removidas pelo assistente de limpeza do servidor. Para remover atualizações aprovadas automaticamente que estão em um Estado aprovado, o administrador do WSUS deve, no mínimo, definir manualmente o status de aprovação das atualizações substituídas como não aprovadas para que elas fiquem qualificadas para a recusa pelo assistente de limpeza do servidor. O assistente de limpeza do servidor garantirá que uma atualização mais recente seja aprovada e que nenhum sistema cliente ainda esteja relatando a atualização conforme necessário antes de marcar a atualização como recusada.




