---
title: Um laptop remoto se desconecta da rede sem fio
description: Solução de um problema no qual o laptop remoto se desconecta da rede sem fio.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: df43a69fa4777a9286cbe27cfa2d241111f7edf6
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265868"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>Um laptop remoto se desconecta da rede sem fio

Esse problema pode ocorrer quando um cliente da Área de Trabalho Remota se conecta a um computador laptop usando uma rede sem fio 802.1x. O laptop se desconecta intermitentemente da rede sem fio e não se reconecta automaticamente.

Esse é um problema conhecido que ocorre quando a configuração de autenticação de rede para a conexão de rede sem fio é **Autenticação de usuário**.

Para contornar esse problema, defina a configuração de autenticação de rede como **Autenticação do usuário ou do computador** ou **Autenticação do computador**.

 > [!NOTE]  
> Para alterar as configurações de autenticação de rede em um único computador, talvez você precise usar o painel de controle da Central de Rede e Compartilhamento para criar uma conexão sem fio com as novas configurações.

Para obter uma descrição completa de como definir as configurações de rede sem fio usando GPOs, confira [Configurar Políticas de Rede Sem Fio (IEEE 802.11)](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).
