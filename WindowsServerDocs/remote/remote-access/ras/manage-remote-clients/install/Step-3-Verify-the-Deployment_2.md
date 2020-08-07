---
title: Etapa 3 verificar a implantação
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7e8b7e037c0c98c62daf9abdf743ae4c1153f054
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950841"
---
# <a name="step-3-verify-the-deployment"></a>Etapa 3 verificar a implantação

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação para o gerenciamento remoto de clientes DirectAccess.

### <a name="to-verify-proper-deployment"></a>Para verificar a implantação adequada

1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha o objeto Política de Grupo.

2.  No computador cliente, clique no ícone **conexões de rede** na área de notificação para acessar o Gerenciador de mídia do DirectAccess.

3.  Clique em **conexão DirectAccess**e você verá que o status é **conectado localmente**.

4.  Remova o computador da rede corporativa e conecte-o a uma rede pública.

5.  Em um prompt de comando, digite **nltest/dsgetdc: [nome de domínio totalmente qualificado]**. Esse comando verificará se a rede corporativa está acessível ao cliente. Se o controlador de domínio não estiver acessível, a seguinte mensagem de erro exibirá o relatório de que o domínio não existe: ERROR_NO_SUCH_DOMAIN.



