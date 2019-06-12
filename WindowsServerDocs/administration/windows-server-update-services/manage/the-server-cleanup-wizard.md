---
title: O Assistente para limpeza de servidor
description: Tópico do Windows Server Update Service (WSUS) - como usar a Assistente de limpeza do servidor para gerenciar o espaço em disco
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d9287bb8bfc0fd51c53c598ccbc1f0498942e2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439809"
---
# <a name="the-server-cleanup-wizard"></a>O Assistente para limpeza de servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A Assistente de limpeza do servidor é integrada à interface do usuário e pode ser usada para ajudar você a gerenciar o espaço em disco. Este assistente pode fazer as seguintes operações:

- Remover as atualizações não utilizadas e revisões de atualização de remover todas as atualizações mais antigas e as revisões de atualização que não foram aprovadas.

- Exclua computadores não entrar em contato com a exclusão do servidor em todos os computadores cliente que não contataram o servidor de trinta dias ou mais.

- Exclusão de arquivos de atualização desnecessários excluir que todos os arquivos que não são necessários de atualização por atualizações ou os servidores downstream.

- Recuse atualizações expiradas recusa todas as atualizações expiradas pela Microsoft.

- Recusa atualizações substituídas recusar todas as atualizações que atendem aos seguintes critérios:

  -   A atualização substituída não é obrigatória

  -   A atualização substituída foi no servidor por 30 dias ou mais

  -   A atualização substituída não é relatada no momento, conforme necessário por qualquer cliente

  -   A atualização substituída não foi explicitamente implantada para um grupo de computadores por 90 dias ou mais

  -   A atualização substituta deve ser aprovada para instalação para um grupo de computadores

  > [!WARNING]
  >  Em uma hierarquia do WSUS, é altamente recomendável que você executar o processo de limpeza no servidor do WSUS de réplica de downstream/mais inferior, primeiro e, em seguida, mova a hierarquia. Executar a limpeza em qualquer servidor upstream antes de executar a limpeza em todos os servidores downstream incorretamente pode causar uma incompatibilidade entre os dados que está presentes em bancos de dados upstream e downstream bancos de dados. A incompatibilidade de dados pode levar a falhas de sincronização entre os servidores upstream e downstream. 
  > 
  > [!IMPORTANT]
  >  Se você remover o conteúdo desnecessário usando o Assistente para limpeza de servidor, todos os arquivos de atualização privada que você baixou do site do catálogo do Microsoft Update também serão removidos. Você deve reimportar esses arquivos depois de executar o Assistente para limpeza de servidor. 

Se houver atualizações aprovadas usando uma regra de aprovação automática, eles ainda podem estar no estado "Aprovado" e não serão removidos pelo Assistente de limpeza do servidor. Para remover as atualizações aprovadas automaticamente que estão no estado "aprovado", o administrador do WSUS - pelo menos - defina manualmente o status de aprovação de atualizações substituídas para "Aprovado" para que eles estarão qualificados para a recusa de Assistente de limpeza do servidor. A limpeza de servidor Assistente garantirá que uma atualização mais recente for aprovada e que nenhum sistema de cliente ainda está se comunicando que atualize conforme necessário antes de marcar a atualização como "Recusada".




