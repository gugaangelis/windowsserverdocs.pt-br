---
title: AD FS de solução de problemas - Fiddler
description: Este documento descreve o Fiddler é e como instalar e configurar o Fiddler para solucionar problemas de declarações do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865627"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS de solução de problemas - Fiddler
Fiddler é uma ferramenta que pode ser usada para capturar o tráfego de web HTTP/HTTPS.  Essa ferramenta pode ser usada para auxiliar na solução o processo de emissão de declaração.  Ao examinar o tráfego, podemos obter um melhor entendimento de onde a interação é decompor.  Este documento descreverá como instalar e configurar o Fiddler para capturar o tráfego do AD FS.  Para um rastreamento do fiddler exemplo usando o WS-Federation consulte [AD FS de solução de problemas - Fiddler - WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Baixe e instale o Fiddler
Você pode baixar o Fiddler [aqui](https://www.telerik.com/download/fiddler).  Depois que você baixou ele vá em frente e instalá-lo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configure o Fiddler para capturar o tráfego do AD FS
Para capturar o tráfego do AD FS, precisamos configurar o Fiddler para descriptografar o tráfego SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurar o certificado SSL do Fiddler
 Use o procedimento a seguir para configurar o Fiddler para descriptografar o tráfego SSL.

1.  Abra o Fiddler
2.  Na parte superior, sob **ferramentas**, selecione **opções do Fiddler**.
3.  Clique na guia HTTPS.
4.  Coloque uma marca de seleção **descriptografar tráfego HTTPS** e selecione **de navegadores apenas** na lista suspensa.
5.  Coloque uma marca de seleção **ignoram os erros de certificado de servidor**.
6.  Clique em **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)