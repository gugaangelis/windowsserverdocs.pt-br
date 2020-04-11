---
title: Um laptop remoto se desconecta da rede sem fio
description: Solução de um problema no qual o laptop remoto se desconecta da rede sem fio.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 72bf482512ff3bb0a678ae59cd6ac20b947a54d9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857149"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>Um laptop remoto se desconecta da rede sem fio

Esse problema pode ocorrer quando um cliente da Área de Trabalho Remota se conecta a um computador laptop usando uma rede sem fio 802.1x. O laptop se desconecta intermitentemente da rede sem fio e não se reconecta automaticamente.

Esse é um problema conhecido que ocorre quando a configuração de autenticação de rede para a conexão de rede sem fio é **Autenticação de usuário**.

Para contornar esse problema, defina a configuração de autenticação de rede como **Autenticação do usuário ou do computador** ou **Autenticação do computador**.

 > [!NOTE]  
> Para alterar as configurações de autenticação de rede em um único computador, talvez você precise usar o painel de controle da Central de Rede e Compartilhamento para criar uma conexão sem fio com as novas configurações.

Para obter uma descrição completa de como definir as configurações de rede sem fio usando GPOs, confira [Configurar Políticas de Rede Sem Fio (IEEE 802.11)](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).
