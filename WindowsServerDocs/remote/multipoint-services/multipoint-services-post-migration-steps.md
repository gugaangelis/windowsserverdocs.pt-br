---
title: Serviços do MultiPoint – tarefas de pós-implantação
description: Saiba como validar e fechar sua migração para os serviços do MultiPoint
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: ed3937cd6830de642c21616071e86eca3ad6cd5b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969243"
---
# <a name="multipoint-services---post-migration-tasks"></a>Serviços do MultiPoint – tarefas de pós-implantação

>Aplica-se a: Windows Server 2016

Depois de migrar para os serviços do MultiPoint no Windows Server 2016, use as informações a seguir para validar a migração e executar etapas de limpeza.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Validar a migração executando um programa piloto

Você pode validar sua migração de serviços do MultiPoint criando um projeto piloto no ambiente de produção. Execute o projeto piloto nos servidores antes de colocar os serviços de função migrados em produção para verificar se a implantação funciona conforme o esperado. Considere limitar o número de conexões primeiro, aumentando lentamente o número de usuários que acessam os serviços do MultiPoint.

> [!NOTE]
> Sempre use contas de teste para testar a migração. Use uma conta com privilégios administrativos e uma conta para um usuário válido.

## <a name="retire-the-source-server"></a>Desativar o servidor de origem
Depois de validar a migração, você pode desligar ou desconectar o servidor de origem da rede. Se o servidor tiver ingressado no domínio, remova-o do domínio antes de desconectá-lo.

