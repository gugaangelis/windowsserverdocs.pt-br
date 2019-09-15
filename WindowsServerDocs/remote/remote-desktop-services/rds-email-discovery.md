---
title: Configurar a descoberta de email para assinar seu feed de RDS
description: Saiba como integrar o Azure AD Domain Services à sua implantação do RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: ca9484cc8abffcc21b4ed11756fb009b55046a0c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870966"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar a descoberta de email para assinar seu feed de RDS

Já teve dificuldades para conectar seus usuários finais a seu feed RDS publicado, seja devido a um único caractere ausente na URL do feed ou porque eles perderem o email com a URL? Praticamente todos os aplicativos de cliente da Área de Trabalho Remota dão suporte à localização de sua assinatura digitando seu endereço de email, tornando mais fácil do que nunca conectar seus usuários a seus RemoteApps e áreas de trabalho.

>[!IMPORTANT]
>O aplicativo da Área de Trabalho Remota da Microsoft na Microsoft Store não dá suporte a assinatura com endereço de email no momento.

Antes de configurar a descoberta de email, faça o seguinte:

- Verifique se você tem permissão para adicionar um registro TXT ao domínio associado ao seu email (por exemplo, se os usuários tivessem endereços de email @contoso.com, você precisaria de permissões para o domínio contoso.com)
- Crie uma URL do Web Feed de RD (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx, como https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Agora, use estas etapas para configurar a descoberta de email:

1. Em seu navegador, conecte-se ao site do registrador de nomes de domínio em que seu domínio está registrado.
2. Navegue até a página adequada para seu domínio registrado, onde você pode exibir, adicionar e editar registros DNS.
3. Insira um novo registro DNS com as seguintes propriedades:
   - **Host:** _msradc
   - **Texto:** \<URL do Web Feed da RD\>
   - **TTL:** 300

   Os nomes dos campos dos registros DNS variam de acordo com o registrador de nomes de domínio, mas esse processo resultará em um registro TXT denominado msradc.\<nome_de_domínio\> (por exemplo, _msradc.contoso.com) que tem um valor do Web Feed de RD completo.

Pronto! Agora, inicie o aplicativo da Área de Trabalho Remota em seu dispositivo e assine!