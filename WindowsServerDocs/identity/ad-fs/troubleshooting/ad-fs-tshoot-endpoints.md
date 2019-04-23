---
title: AD FS de solução de problemas - pontos de extremidade do AD FS
description: Este documento descreve como solucionar problemas de pontos de extremidade do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857557"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS de solução de problemas - pontos de extremidade de metadados do AD FS
Pontos de extremidade fornecem acesso para a funcionalidade do servidor de Federação do AD FS, como publicar metadados de Federação.  Para verificar que o servidor do AD FS está respondendo às solicitações da web, podemos verificar vários pontos de extremidade.


## <a name="federation-metadata-test"></a>Teste de metadados de Federação
Federação passiva refere-se a cenários em que o seu navegador é novamente direcionado para a página de entrada do AD FS.  Testando o ponto de extremidade de metadados, podemos determinar se o servidor do AD FS está respondendo a solicitações de web nesses cenários passivos.  Use o procedimento a seguir para testar o ponto de extremidade.

1.  Usando um navegador da web, navegue até seu ponto de extremidade de metadados de Federação do AD FS.  Por exemplo:  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. O arquivo xml deve baixar localmente em seu computador.
3. Abri-lo e verificar se ela contém informações semelhantes para as informações abaixo: ![Passivo](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>Teste de WS-MEX (teste ativo)
WS-MetaDataExchange é um protocolo de serviços da web e faz parte do roteiro do WS-Federation.  Ele usa uma mensagem SOAP para solicitação de metadados.  Testando o ponto de extremidade, podemos determinar se o servidor do AD FS está respondendo às solicitações da web para WS-MetaDataExchange.  Use o procedimento a seguir para testar o ponto de extremidade.
1.  Usando um navegador da web, navegue até seu ponto de extremidade de metadados de Federação do AD FS.  Por exemplo:  https://sts.contoso.com/adfs/services/trust/mex
2. O arquivo xml deve ser exibido automaticamente no navegador.  Deve se parecer com a imagem a seguir:

![Ativo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)