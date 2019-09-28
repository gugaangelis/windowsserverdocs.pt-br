---
title: Solução de problemas de AD FS – AD FS pontos de extremidade
description: Este documento descreve como solucionar problemas de AD FS pontos de extremidade
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 807b5c5de14bf6a43419d0b9d2d3a4e6953d0075
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366218"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>Solução de problemas de AD FS – AD FS pontos de extremidade de metadados
Os pontos de extremidade fornecem acesso à funcionalidade do servidor de Federação de AD FS, como a publicação de metadados de Federação.  Para verificar se o servidor de AD FS está respondendo às solicitações da Web, podemos verificar os vários pontos de extremidade.


## <a name="federation-metadata-test"></a>Teste de metadados de Federação
A Federação passiva refere-se a cenários em que seu navegador é redirecionado para a página de entrada AD FS.  Ao testar o ponto de extremidade de metadados, podemos determinar se o servidor de AD FS está respondendo a solicitações da Web nesses cenários passivos.  Use o procedimento a seguir para testar o ponto de extremidade.

1.  Usando um navegador da Web, navegue até o ponto de extremidade de metadados de Federação do AD FS.  Por exemplo: https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. O arquivo XML deve ser baixado localmente em seu computador.
3. Abra-o e verifique se ele contém informações semelhantes à informações abaixo: ![Passive @ no__t-1

## <a name="ws-mex-test-active-test"></a>Teste de WS-MEX (teste ativo)
O WS-MetaDataExchange é um protocolo de serviços Web e faz parte do roteiro do WS-Federation.  Ele usa uma mensagem SOAP para solicitar metadados.  Testando o ponto de extremidade, podemos determinar se o servidor de AD FS está respondendo às solicitações da Web para WS-MetaDataExchange.  Use o procedimento a seguir para testar o ponto de extremidade.
1.  Usando um navegador da Web, navegue até o ponto de extremidade de metadados de Federação do AD FS.  Por exemplo: https://sts.contoso.com/adfs/services/trust/mex
2. O arquivo XML deve ser exibido no navegador automaticamente.  Ele deve ser semelhante à imagem abaixo:

![Ativo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)