---
title: "Alterar configurações de Streaming de mídia"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>Alterar configurações de Streaming de mídia

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Várias opções estão disponíveis para alterar as configurações de streaming de mídia. As seguintes opções estão disponíveis:  
  
-   [Ocultar suplemento de streaming de mídia remota](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Defina o nome da biblioteca de mídia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Defina a qualidade de streaming de vídeo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Habilitar ou desabilitar o streaming de mídia de forma programática](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>Ocultar suplemento de streaming de mídia remota  
 Você pode ocultar a suplemento de streaming, adicionando uma entrada no registro de mídia remoto.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Para ocultar a suplemento de streaming de mídia remota  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  No **pesquisa** , digite **regedit**e, em seguida, clique no **Regedit** aplicativo.  
  
3.  No painel esquerdo, expanda até a seguinte entrada do registro:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Clique com botão direito **DisabledAddIns**, aponte para **nova**e clique em **valor DWORD**.  
  
5.  Para o novo valor, digite **497796c6-9cc7-43be-aa26-4e6b5695370d**, que é o identificador para o suplemento de streaming de mídia remoto e pressione **Enter**.  
  
6.  Clique com botão direito do valor e clique em **modificar**.  
  
7.  Tipo **1** de dados do valor e clique em **Okey**.  
  
##  <a name="BKMK_LibraryName"></a>Defina o nome da biblioteca de mídia  
 Você pode usar uma classe no SDK do Windows Server Solutions para definir o nome da biblioteca de mídia. Para definir o nome da biblioteca de mídia, você pode usar o **SetMediaLibraryName** método do **MediaStreamingManager** de classe no **Microsoft.WindowsServerSolutions.MediaStreaming** namespace. O exemplo a seguir mostra como definir o nome da biblioteca de mídia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Para obter mais informações, consulte [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a>Defina a qualidade de streaming de vídeo  
 Defina o vídeo de qualidade de streaming Obtendo a pontuação WinSAT CPU e, em seguida, criando e instalando o arquivo. XML que contém as informações de pontuações do WinSAT. Se o arquivo. XML que contém as informações de pontuações do WinSAT é instalado antes da configuração inicial é executado, a interface do usuário de qualidade do vídeo configuração não aparecerá ao cliente. Para obter mais informações, consulte [definir a pontuação WinSAT no servidor](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a>Habilitar ou desabilitar o streaming de mídia de forma programática  
 Você pode usar uma classe no SDK do Windows Server Solutions para habilitar ou desabilitar o streaming de mídia de forma programática. Para habilitar ou desabilitar o streaming de mídia, você pode usar o **SetMediaStreamingEnabled** método do **MediaStreamingManager** de classe no **Microsoft.WindowsServerSolutions.MediaStreaming** namespace. O exemplo de código a seguir mostra como habilitar o streaming de mídia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 O exemplo de código a seguir mostra como desabilitar o streaming de mídia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)