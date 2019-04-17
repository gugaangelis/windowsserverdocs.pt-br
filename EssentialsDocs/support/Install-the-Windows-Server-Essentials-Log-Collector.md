---
title: Instalar o coletor de Log do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ade18cec590392f35e7ad6b30d9a22ccdce44dcd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Instalar o coletor de Log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O Assistente de instalação do coletor de Log do Windows Server Essentials instala o coletor de Log como um Launchpad Add-in. Você pode instalar e usar o coletor de Log em computadores de rede ou o servidor ou ambos. Após a instalação, o coletor de Log será exibido no painel.  
  
###  <a name="BKMK_ToInstall"></a>Para instalar o coletor de Log  
  
1.  Baixe o pacote de instalação do coletor de Log para qualquer servidor ou computador na rede.  
  
    > [!NOTE]
    >  Você pode [baixar o pacote de instalação do coletor de Log](https://go.microsoft.com/fwlink/p/?LinkId=255470) da Microsoft.  
  
2.  Clique duas vezes no ícone do coletor de Log.  
  
3.  Se você estiver executando a instalação de um computador de rede, insira suas credenciais de administrador de servidor quando solicitado.  
  
4.  Optar por aceitar os termos de licença de Software da Microsoft.  
  
5.  Para instalar o coletor de Log somente no servidor, selecione o **somente no servidor** caixa de seleção. Para instalar o coletor de Log em todos os computadores de rede, selecione o **no servidor e em todos os computadores na rede** caixa de seleção.  
  
6.  Clique em **instalar o suplemento**.  
  
###  <a name="BKMK_Reinstall"></a>Reinstalar o coletor de Log  
 Se for necessário reinstalar o coletor de Log, você deve desinstalar e reinstalar o coletor de Log no servidor e os computadores da rede na rede. Desinstalando o coletor de Log no servidor do painel, todos os computadores de rede desinstalará automaticamente o coletor de Log.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Desinstalar e reinstalar o coletor de Log  
  
1.  Abra o painel.  
  
2.  Clique no **suplemento** , selecione **coletor de Log** na lista e, em seguida, clique **desinstalar**.  
  

3.  Baixe e instale o coletor de Log realizando as etapas no procedimento anterior, [para instalar o coletor de Log](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Baixe e instale o coletor de Log realizando as etapas no procedimento anterior, [para instalar o coletor de Log](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Instalar manualmente o coletor de Log  
 Se o Assistente de instalação falha ao instalar o coletor de Log, você pode instalar o coletor de Log em um único computador usando o procedimento a seguir.  
  
##### <a name="to-manually-install-the-log-collector"></a>Para instalar manualmente o coletor de Log  
  
1.  Renomeie a extensão do arquivo baixado de instalação da. wssx para. cab.  
  
2.  Clique duas vezes em nome do arquivo de instalação.  
  
3.  Clique em **Okey** se você for solicitado.  
  
4.  Clique duas vezes em nome do arquivo termina com ˜.msi e selecione uma pasta na qual extraí-lo.  
  
5.  Navegue até a pasta com o arquivo extraído e clique duas vezes no arquivo de instalação para usar o Assistente para concluir a instalação.
