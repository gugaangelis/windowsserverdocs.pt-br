---
title: Instalar o coletor de log do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4ea922142b43e35d6f17207d448c48b2da52ebc8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852279"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Instalar o coletor de log do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O assistente de instalação do coletor de logs do Windows Server Essentials instala o coletor de logs como um suplemento de Launchpad. Você pode instalar e usar o Coletor de Log em computadores da rede ou servidor, ou ambos. Após a instalação, o coletor de log será exibido no painel.  
  
###  <a name="to-install-the-log-collector"></a><a name="BKMK_ToInstall"></a>Para instalar o coletor de logs  
  
1.  Baixe o pacote de instalação do coletor de log para qualquer servidor ou computador na rede.  
  
    > [!NOTE]
    > [Baixe o pacote de instalação do coletor de logs do Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).  
  
2.  Clique duas vezes no ícone do coletor de log.  
  
3.  Se você estiver executando a instalação de um computador de rede, insira suas credenciais de administrador do servidor quando solicitado.  
  
4.  Escolha aceitar os termos de licença de software da Microsoft.  
  
5.  Para instalar o coletor de log somente no servidor, marque a caixa de seleção **Somente no servidor**. Para instalar o coletor de log em todos os computadores de rede, marque a caixa de seleção **No servidor e em todos os computadores na rede**.  
  
6.  Clique em **Instalar o suplemento**.  
  
###  <a name="reinstalling-the-log-collector"></a><a name="BKMK_Reinstall"></a>Reinstalando o coletor de logs  
 Se for necessário reinstalar o coletor de log, desinstale e reinstale o coletor de log no servidor e os computadores de rede na rede. Ao desinstalar o coletor de log no servidor do painel, todos os computadores de rede irão automaticamente desinstalar o coletor de log.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Para desinstalar e reinstalar o coletor de log  
  
1.  Abra o Dashboard.  
  
2.  Clique na guia **Suplemento**, selecione **Coletor de Log** na lista e clique em **Desinstalar**.  
  

3.  Baixe e instale o coletor de log executando as etapas no procedimento anterior, [Para instalar o coletor de log](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Baixe e instale o coletor de log executando as etapas no procedimento anterior, [Para instalar o coletor de log](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Instalar o coletor de log manualmente  
 Se o assistente de instalação falhar ao instalar o coletor de log, você pode instalar o coletor de log em um único computador usando o procedimento a seguir.  
  
##### <a name="to-manually-install-the-log-collector"></a>Para instalar o coletor de log manualmente  
  
1.  Renomeie a extensão do arquivo de instalação baixado de. WSSX para. cab.  
  
2.  Clique duas vezes no nome do arquivo de instalação.  
  
3.  Clique em **OK** se você for solicitado.  
  
4.  Clique duas vezes no nome do arquivo que termina com '. msi ' e selecione uma pasta na qual ele será extraído.  
  
5.  Navegue até a pasta com os arquivos extraídos e clique duas vezes no arquivo de instalação para usar o assistente para concluir a instalação.
