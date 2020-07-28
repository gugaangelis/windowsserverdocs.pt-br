---
title: Use pastas compartilhadas no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: cb7f3d7d-4225-409a-9f6b-34a106e8dd24
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c7f042a47dd6f350b79ec17f69ffb87ab7b99259
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87179452"
---
# <a name="use-shared-folders-in-windows-server-essentials"></a>Use pastas compartilhadas no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 O Windows Server Essentials fornece um local central para todos os seus dados e arquivos através de pastas compartilhadas localizadas no seu servidor.

 Há maneiras diferentes de acessar suas pastas compartilhadas no Windows Server Essentials a partir de um dispositivo que está conectado ao servidor:


-   [Uso da Barra Inicial do Windows Server Essentials](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)

-   [Uso do Acesso via Web Remoto](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)

-   [Uso do aplicativo My Server para Windows Phone](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)

-   [Uso do aplicativo My Server para Windows 8](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)

##  <a name="using-the-windows-server-essentials-launchpad"></a><a name="BKMK_UsingLaunchpad"></a>Usando o Launchpad do Windows Server Essentials
 Você pode usar a Barra Inicial de qualquer computador que esteja conectado ao servidor usando o Assistente Conectar Meu Computador ao Servidor. Para obter mais informações sobre como conectar o computador ao servidor, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

-   [Uso da Barra Inicial do Windows Server Essentials](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)

-   [Uso do Acesso via Web Remoto](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)

-   [Uso do aplicativo My Server para Windows Phone](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)

-   [Uso do aplicativo My Server para Windows 8](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)

##  <a name="using-the-windows-server-essentials-launchpad"></a><a name="BKMK_UsingLaunchpad"></a>Usando o Launchpad do Windows Server Essentials
 Você pode usar a Barra Inicial de qualquer computador que esteja conectado ao servidor usando o Assistente Conectar Meu Computador ao Servidor. Para obter mais informações sobre como conectar o computador ao servidor, consulte [conectar computadores ao servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).


 Depois de conectar seu computador ao servidor, um atalho da Barra Inicial é adicionado à área de notificação da área de trabalho. Clique duas vezes no ícone da Barra Inicial e insira suas credenciais de rede para acessar pastas compartilhadas usando a Barra Inicial. Usando o link de pastas compartilhadas na Barra Inicial, você pode fazer upload ou download de arquivos para qualquer uma das pastas compartilhadas que estão listadas por arrastar e soltar arquivos entre o computador local e as pastas compartilhadas. Pastas compartilhadas habilitam você a transmitir músicas e vídeos, reproduzir apresentações de slides ou gravar programas de TV em qualquer computador que esteja conectado ao servidor, ou você pode reproduzir uma apresentação de slides para exibir imagens.

 Para obter mais informações sobre o Launchpad, consulte [visão geral da Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).

###  <a name="copy-or-move-shared-files-or-folders-using-the-launchpad"></a><a name="BKMK_Launchpad"></a>Copiar ou mover arquivos ou pastas compartilhadas usando o Launchpad
 Quando você deseja copiar ou mover arquivos compartilhados no Windows Server Essentials usando a Barra Inicial, clique na guia **Pastas compartilhadas** na Barra Inicial.

 Se você deseja mover um arquivo ou pasta de um local para outro em **Pastas compartilhadas**, você pode usar o método de arrastar e soltar da mesma maneira que você moveria arquivos e pastas no seu computador. Abra a pasta que contém o arquivo ou pasta que você deseja mover. Em seguida, abra a pasta aonde você deseja movê-lo em uma janela diferente. Posicione as janelas lado a lado na área de trabalho para que você possa ver o conteúdo de ambas e em seguida, arraste o arquivo ou pasta da primeira pasta para a segunda.

> [!NOTE]
>  Ao usar o método de arrastar e soltar, você poderá notar que algumas vezes o arquivo ou pasta é **copiado** e outras vezes é **movido**. Se você estiver arrastando um item entre duas pastas que estão armazenadas no mesmo disco rígido, o item é movido para que as duas cópias do mesmo arquivo ou pasta não sejam criadas no mesmo local. Se você arrastar o item para uma pasta que está em um local diferente (como outro computador) ou uma mídia removível como uma unidade flash USB, o item é copiado.

 Se você deseja copiar arquivos ou pastas de um local para outro em **Pastas compartilhadas**, você pode usar o método de copiar e colar da mesma maneira que você copiaria os arquivos no computador. Abra a pasta que contém os arquivos que você deseja copiar. Clique com botão direito nos arquivos que você deseja copiar e em seguida, clique em **Copiar**. Clique na pasta aonde você deseja colar os arquivos copiados e em seguida, clique em **Colar**.

##  <a name="using-remote-web-access"></a><a name="BKMK_UsingRWA"></a>Usando o Acesso via Web remoto

 Você pode acessar arquivos e pastas compartilhados de qualquer computador remoto usando o site de Acesso via Web Remoto. Em um computador na rede de servidor, para acessar o site Acesso via Web remoto, abra o navegador da Internet e digite https://<ServerName \> /Remote. Usando o Acesso via Web Remoto, você pode exibir e gerenciar arquivos em pastas compartilhadas. Para obter as instruções passo a passo, consulte [usar o acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

 Você pode acessar arquivos e pastas compartilhados de qualquer computador remoto usando o site de Acesso via Web Remoto. Em um computador na rede de servidor, para acessar o site Acesso via Web remoto, abra o navegador da Internet e digite https://<ServerName \> /Remote. Usando o Acesso via Web Remoto, você pode exibir e gerenciar arquivos em pastas compartilhadas. Para obter as instruções passo a passo, consulte [usar o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).


> [!NOTE]
>  O Acesso via Web Remoto no servidor deve ser ativado para acessar o site de Acesso via Web remoto. Para obter informações sobre como gerenciar Acesso via Web remotos, consulte [gerenciar acesso via Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).

###  <a name="create-rename-move-delete-or-copy-files-and-folders-in-remote-web-access"></a><a name="BKMK_2"></a>Criar, renomear, mover, excluir ou copiar arquivos e pastas no Acesso via Web remoto

 Você pode usar o Acesso via Web Remoto para criar novas pastas em uma pasta compartilhada existente para renomear arquivos e pastas, mover ou copiar arquivos e pastas e excluir arquivos e pastas no servidor. Para obter mais informações, consulte a seção criar, renomear, mover, excluir ou copiar arquivos e pastas no Acesso via Web remoto? no tópico [usar acesso via Web remotos](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_3"></a>Carregar e baixar arquivos no Acesso via Web remoto
 Na guia **Pastas compartilhadas** do Acesso via Web Remoto, você pode fazer upload e download de arquivos. Para obter mais informações, consulte a seção carregar e baixar arquivos no Acesso via Web remoto? no tópico [usar acesso via Web remotos](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

 Você pode usar o Acesso via Web Remoto para criar novas pastas em uma pasta compartilhada existente para renomear arquivos e pastas, mover ou copiar arquivos e pastas e excluir arquivos e pastas no servidor. Para obter mais informações, consulte a seção criar, renomear, mover, excluir ou copiar arquivos e pastas no Acesso via Web remoto? no tópico [usar acesso via Web remotos](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_3"></a>Carregar e baixar arquivos no Acesso via Web remoto
 Na guia **Pastas compartilhadas** do Acesso via Web Remoto, você pode fazer upload e download de arquivos. Para obter mais informações, consulte a seção carregar e baixar arquivos no Acesso via Web remoto? no tópico [usar acesso via Web remotos](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).


##  <a name="using-my-server-app-for-windows-phone"></a><a name="BKMK_Phone"></a>Usando meu aplicativo de servidor para Windows Phone
 Você pode acessar as pastas compartilhadas por meio de seu Windows Phone usando o aplicativo My Server para Windows Phone. Você pode baixar este aplicativo do [Marketplace para Windows Phone](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a).

##  <a name="using-my-server-app-for-windows-8"></a><a name="BKMK_App"></a>Usando meu aplicativo de servidor para Windows 8
 Você pode acessar as pastas compartilhadas por meio do Windows 8 usando o aplicativo My Server para o Windows 8. Você pode baixar o aplicativo da e armazenamento da [Loja de Aplicativos do Windows 8](https://windows.microsoft.com/windows-8/apps).

## <a name="see-also"></a>Confira também

-   [Gerenciar pastas do servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md)

-   [Gerenciar o armazenamento do servidor](../manage/Manage-Server-Storage-in-Windows-Server-Essentials.md)

-   [Conecte-se](Get-Connected-in-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Reproduzir mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md)

