---
title: Configurar a descoberta de email para assinar seu feed de RDS
description: Saiba como configurar a descoberta de email em sua implantação do RDS.
ms.author: chrimo
ms.date: 8/28/2020
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 71b892d95b15f02445ec7898a6c57f931bc4b501
ms.sourcegitcommit: 2b1a12c85acff137e5ac84cd0e62d8353fcdde31
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087459"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar a descoberta de email para assinar seu feed de RDS

Já teve dificuldades para conectar seus usuários finais a seu feed RDS publicado, seja devido a um único caractere ausente na URL do feed ou porque eles perderem o email com a URL? Praticamente todos os aplicativos de cliente da Área de Trabalho Remota dão suporte à localização de sua assinatura digitando seu endereço de email, tornando mais fácil do que nunca conectar seus usuários a seus RemoteApps e áreas de trabalho.

Antes de configurar a descoberta de email, faça o seguinte:

- Verifique se você tem permissão para adicionar um registro TXT ao domínio associado ao seu email (por exemplo, se os usuários tivessem endereços de email @contoso.com, você precisaria de permissões para o domínio contoso.com)
- Crie uma URL do Web Feed de RD (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx, como https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

>[!NOTE]
>Se você estiver usando a Área de Trabalho Virtual do Windows em vez da Área de Trabalho Remota, convém usar estas URLs:
>
>- Se você estiver usando a Área de Trabalho Virtual do Windows (clássica): <https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfedddiscovery.aspx>
>- Se você estiver usando a Área de Trabalho Virtual do Windows: <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>

Agora, siga estas etapas para configurar a descoberta de email:

1. Em seu navegador, conecte-se ao site do registrador de nomes de domínio em que seu domínio está registrado.
2. Navegue até a página adequada para seu domínio registrado, onde você pode exibir, adicionar e editar registros DNS.
3. Insira um novo registro DNS com as seguintes propriedades:
   - **Host:** _msradc
   - **Texto:** \<RD Web Feed URL\>
   - **TTL:** 300

   Os nomes dos campos dos registros DNS variam de acordo com o registrador de nomes de domínio, mas esse processo resultará em um registro TXT denominado _msradc.\<domain_name\> (por exemplo, _msradc.contoso.com) que tem um valor do Web Feed de RD completo.

Pronto! Agora, inicie o aplicativo da Área de Trabalho Remota em seu dispositivo e assine!