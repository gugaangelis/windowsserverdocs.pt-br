---
title: Serviços do MultiPoint – tarefas de pós-implantação
description: Saiba como validar e fechar sua migração para os serviços do MultiPoint
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: a1d304e95037ad67a8d4f02e1dc17ec3e0485ed8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858889"
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

