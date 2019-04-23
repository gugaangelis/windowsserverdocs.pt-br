---
title: MultiPoint Services – tarefas pós-migração
description: Saiba como validar e fechar sua migração para o MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863497"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint Services – tarefas pós-migração

>Aplica-se a: Windows Server 2016

Depois de migrar para o MultiPoint Services no Windows Server 2016, use as informações a seguir para validar a migração e execute as etapas de limpeza.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Validar a migração executando um programa piloto

Você pode validar a migração do MultiPoint Services, criando um projeto-piloto no ambiente de produção. Execute o projeto-piloto nos servidores antes de colocar os serviços de função migrados em produção para verificar se a implantação funciona conforme o esperado. Considere limitar o número de conexões a princípio, aumentando lentamente o número de usuários que acessam o MultiPoint Services.

> [!NOTE] 
> Sempre use contas de teste para testar a migração. Use uma conta com privilégios administrativos e uma conta de usuário válido.

## <a name="retire-the-source-server"></a>Desativar o servidor de origem
Depois de validar sua migração, você pode desligar ou desconecte o servidor de origem da sua rede. Se o servidor foi ingressado no domínio, remova-o do domínio antes de você desconectá-lo.

