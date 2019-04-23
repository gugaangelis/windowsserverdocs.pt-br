---
title: Configurar a descoberta de email para se inscrever em seu feed de RDS
description: Saiba como integrar o Azure AD Domain Services em sua implantação do RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878317"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar a descoberta de email para se inscrever em seu feed de RDS

Você já teve dificuldades para seus usuários finais conectados ao seu RDS publicado feed, devido à de um único caractere ausente no feed de URL ou porque eles perderem o email com a URL? Dar suporte a praticamente todos os aplicativos de cliente de área de trabalho remota, localizando sua assinatura digitando seu endereço de email, tornando mais fácil do que nunca fazer os usuários conectados a seus aplicativos remotos e áreas de trabalho.

>[!IMPORTANT]
>O aplicativo de área de trabalho remota Microsoft em que a Microsoft Store não dá suporte a assinatura de endereço de email no momento.

Antes de configurar a descoberta de email, faça o seguinte:

- Verifique se você tem permissão para adicionar um registro TXT para o domínio associado com seu email (por exemplo, se os usuários tiverem @contoso.com endereços de email, você precisaria de permissões para o domínio contoso.com)
- Criar uma Web de área de trabalho remota a URL do feed (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx, tais como https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Agora, use estas etapas para configurar a descoberta de email:

1. Em seu navegador, conecte-se ao site do registrador de nome de domínio em que o seu domínio está registrado.
2. Navegue até a página apropriada para seu domínio registrado em que você pode exibir, adicionar e editar os registros DNS.
3. Insira um novo registro DNS com as seguintes propriedades:
   - **Host:** _msradc
   - **Texto:** \<URL de Feed de Web de área de trabalho remota\>
   - **TTL:** 300

   Variam de acordo com os nomes dos campos de registros de DNS com o registrador de nome de domínio, mas esse processo resultará em um registro TXT denominado msradc. \<domain_name\> (como _msradc.contoso.com) que tem um valor de feed da Web da área de trabalho remota completo.

Isso é tudo! Agora, inicie o aplicativo de área de trabalho remota no seu dispositivo e se inscrever!