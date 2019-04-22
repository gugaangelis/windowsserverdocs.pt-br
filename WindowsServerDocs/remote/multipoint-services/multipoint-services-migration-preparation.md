---
title: Prepare-se para migrar para MultiPoint Services
description: Descreve as informações a serem reunidas antes de migrar para o MultiPoint Services no Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824377"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Preparar para migrar para o MultiPoint Services no Windows Server 2016

>Aplica-se a: Windows Server 2016

Use as informações a seguir para coletar as informações necessárias migrar o serviço de função do MultiPoint Services de um servidor de origem que executa uma versão anterior do Windows Server 2016 para um servidor de destino executando o Windows Server 2016 RTM.

No mínimo, você deve ser um membro do grupo Administradores no servidor de origem e o servidor de destino para instalar, remover ou configurar os serviços do MultiPoint.

>[!NOTE]
> As etapas descritas aqui não fornecem diretrizes para migração de dados salvas para pasta do usuário ou pastas compartilhadas. Certifique-se de que os usuários fazer backup de seus dados antes de começar a migração.

Use o Gerenciador do MultiPoint para recuperar as informações necessárias para a migração. Você precisará de permissão de administrador de servidor para usar o Gerenciador do MultiPoint.

Registre as configurações de servidor, o usuário e o ambiente de vários pontos na [planilha de coleta de dados de migração](multipoint-services-migration-worksheet.md). Use as etapas a seguir para coletar essas informações.

## <a name="multipoint-server-settings-for-the-local-server"></a>Configurações do multiPoint Server para o servidor local
1. Inicie o Gerenciador do MultiPoint.
2. Sobre o **página inicial** guia, selecione o servidor local e, em seguida, clique em **editar configurações do servidor.**
3. Registre as configurações na planilha de dados.
4. Feche a janela de configurações.

## <a name="managed-servers-and-computers"></a>Computadores e servidores gerenciados

Você pode localizar os nomes dos servidores gerenciados e computadores na **Home** guia no Gerenciador do MultiPoint.

## <a name="station-settings"></a>Configurações da estação
Se a orientação logon automático ou exibição é configurada para a estação, use as seguintes etapas para recuperar essas informações. Caso contrário, você pode ignorar esta etapa.

Para recuperar as configurações de estação:

1. Vá para o **estações** guia no Gerenciador do MultiPoint.
2. Localizar uma estação com "Sim" no **logon automático** coluna.
3. Selecione essa estação e, em seguida, clique em **configurar estação**.
4. Registro de usuário que é usado para logon automático.

Para recuperar as configurações de orientação de exibição, exibir o **as configurações da estação** para cada estação.

## <a name="list-of-users"></a>Lista de usuários
1. Clique o **usuários** guia no Gerenciador do MultiPoint.
2. Registro a **Administrator** e **usuário do MultiPoint Dashboard** accoutns.
3. Registre os usuários padrão.

## <a name="vdi-template-location"></a>Local do modelo de VDI
 Se você habilitou o recurso de modelo VDI, registre o local do modelo do VDI. Desde que os servidores de origem e destino estiverem na mesma rede, você pode importar o modelo usando o Gerenciador do MultiPoint.
 
## <a name="next-step"></a>Próximas etapas
Agora você está pronto para [migrar para o MultiPoint Services](multipoint-services-migration-steps.md) na versão RTM do Windows Server 2016.