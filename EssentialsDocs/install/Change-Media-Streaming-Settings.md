---
title: Alterar Configurações de Streaming de Mídia
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823397"
---
# <a name="change-media-streaming-settings"></a>Alterar Configurações de Streaming de Mídia

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Várias opções estão disponíveis para você alterar as configurações de streaming de mídia. As seguintes opções estão disponíveis:  
  
-   [Ocultar suplemento de streaming de mídia remota](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Defina o nome da biblioteca de mídia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Definir a qualidade de streaming de vídeo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Habilitar ou desabilitar o streaming de mídia de forma programática](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a> Ocultar suplemento de streaming de mídia remota  
 Você pode ocultar o suplemento de streaming de mídia remota adicionando uma entrada no registro.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Para ocultar suplemento de streaming de mídia remota  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.  
  
2.  Na caixa **Pesquisar**, digite **regedit** e clique no aplicativo **Regedit**.  
  
3.  No painel esquerdo, expanda a seguinte entrada do registro:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Clique com o botão direito do mouse em **DisabledAddIns**, aponte para **Novo**e clique em **Valor DWORD**.  
  
5.  Para o novo valor, digite **497796c6-9cc7-43be-aa26-4e6b5695370d**, que é o identificador para o suplemento de streaming de mídia remota, e pressione **Enter**.  
  
6.  Clique com o botão direito do mouse e clique em **Modificar**.  
  
7.  Digite **1** nos dados de valor e clique em **OK**.  
  
##  <a name="BKMK_LibraryName"></a> Defina o nome da biblioteca de mídia  
 Você pode usar uma classe do Windows Server Solutions SDK para definir o nome da biblioteca de mídia. Para definir o nome da biblioteca de mídia, você pode usar o método **SetMediaLibraryName** da classe **MediaStreamingManager** no namespace **Microsoft.WindowsServerSolutions.MediaStreaming** . O exemplo a seguir mostra como definir o nome da biblioteca de mídia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Para obter mais informações, consulte o [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a> Definir a qualidade de streaming de vídeo  
 Defina a qualidade de streaming de vídeo obtendo a pontuação da CPU do WinSAT e criando e instalando o arquivo .xml que contém as informações de pontuação do WinSAT. Se o arquivo .xml que contém as informações de pontuação do WinSAT estiver instalado antes de executar a Configuração Inicial, a interface do usuário para definir a qualidade de vídeo não será exibida para o cliente. Para obter mais informações, consulte [Set the WinSAT Score on the Server](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a> Habilitar ou desabilitar o streaming de mídia de forma programática  
 Você pode usar uma classe do Windows Server Solutions SDK para habilitar ou desabilitar o streaming de mídia de forma programática. Para habilitar ou desabilitar o streaming de mídia, você pode usar o método **SetMediaStreamingEnabled** da classe **MediaStreamingManager** no namespace **Microsoft.WindowsServerSolutions.MediaStreaming** . O exemplo de código a seguir mostra como habilitar o streaming de mídia:  
  
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
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)