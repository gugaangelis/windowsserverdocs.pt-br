---
title: Configurar a descoberta de email para se inscrever seu feed de RDS
description: Saiba como integrar serviços de domínio do Windows Azure AD na sua implantação do RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691665"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar a descoberta de email para se inscrever seu feed de RDS

Você já tinha problemas para os usuários finais conectados a seus RDS publicado feed, devido a um único caractere ausente no feed de inserir URL ou porque eles perderem o e-mail com o URL? Quase todos os aplicativos de cliente de área de trabalho remota suportam Localizando sua assinatura digitando seu endereço de email, tornando mais fácil do que nunca fazer seus usuários conectados aos seus RemoteApps e áreas de trabalho.

>[!IMPORTANT]
>O aplicativo de área de trabalho remota Microsoft nos Store a Microsoft não dá suporte a assinatura de endereço de email no momento.

Antes de configurar a descoberta de email, faça o seguinte:

- Verifique se você tem permissão para adicionar um registro TXT ao domínio associado ao seu email (por exemplo, se os usuários tiverem @contoso.com endereços de email você precisaria permissões para o domínio contoso.com)
- Criar um feed URL da Web RD (https://\ < rdweb-dns-name\ >.domain/RDWeb/Feed/webfeed.aspx, tais comohttps://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Agora, execute estas etapas para configurar a descoberta de email:

1. No seu navegador, conecte-se ao site do registrador de nome de domínio onde o seu domínio está registrado.
2. Navegue até a página apropriada para seu domínio registrado, onde você pode exibir, adicionar e editar registros DNS.
3. Insira um novo registro DNS com as seguintes propriedades:
   - **Host:** _msradc
   - **Texto:** \ < RD Web Feed URL\ >
   - **TTL:** 300

   Variam de acordo com os nomes dos campos de registros de DNS com o registrador de nome de domínio, mas esse processo resultará em um registro TXT denominado _msradc. \ < nome_do_domínio > (por exemplo, _msradc.contoso.com) que tem o valor do feed RD Web completo.

Isso é tudo! Agora, iniciar o aplicativo de área de trabalho remota no seu dispositivo e se inscrever!