---
title: Solução de problemas AD FS-Fiddler
description: Este documento descreve o que é o Fiddler e como instalar e configurar o Fiddler para solucionar problemas de declarações de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366150"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>Solução de problemas AD FS-Fiddler
O Fiddler é uma ferramenta que pode ser usada para capturar o tráfego da Web HTTP/HTTPS.  Essa ferramenta pode ser usada para auxiliar na solução de problemas do processo de emissão de declaração.  Ao examinar o tráfego, podemos entender melhor onde a interação está se dividindo.  Este documento descreverá como instalar e configurar o Fiddler para capturar AD FS tráfego.  Para obter um exemplo de rastreamento Fiddler usando o WS-Federation, consulte [solução de problemas AD FS-Fiddler-WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Baixar e instalar o Fiddler
Você pode baixar o Fiddler [aqui](https://www.telerik.com/download/fiddler).  Depois de baixá-lo, vá em frente e instale-o.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configurar o Fiddler para capturar o tráfego de AD FS
Para capturar o tráfego de AD FS, precisamos configurar o Fiddler para descriptografar o tráfego SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurar o certificado SSL Fiddler
 Use o procedimento a seguir para configurar o Fiddler para descriptografar o tráfego SSL.

1.  Abrir o Fiddler
2.  Na parte superior, em **ferramentas**, selecione **Opções do Fiddler**.
3.  Clique na guia HTTPS.
4.  Faça um check-in para **descriptografar o tráfego HTTPS** e selecione **somente de navegadores** na lista suspensa.
5.  Faça um check-in **ignorar erros de certificado do servidor**.
6.  Clique em **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)