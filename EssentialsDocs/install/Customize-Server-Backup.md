---
title: Personalizar o Backup do servidor
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>Personalizar o Backup do servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Desativar o Backup do servidor por padrão  
 Você pode optar por desativar o Backup do servidor por padrão. Você precisa definir o valor de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** como 1 para habilitar essa opção.  
  
 Quando essa chave é definida, a interface do usuário de Backup do servidor não será exposta por meio do painel ou na barra inicial. Isso permite que você utilize a aplicativos de terceiros para o Backup do servidor.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Para adicionar ServerBackup\ProviderDisabled? chave do registro e defina o valor como 1  
  
1.  No servidor, clique em **iniciar**, clique em **executar**, tipo **regedit** no **abrir** textbox e, em seguida, clique em **Okey**.  
  
2.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**e, em seguida, expanda **ServerBackup**.  
  
3.  Clique com botão direito **ServerBackup**, clique em **nova**e clique em **valor DWARD**.  
  
4.  Para o nome, digite **ProviderDisabled**.  
  
5.  Clique com botão direito no nome, selecione **modificar**, insira **1** de dados do valor e clique em **Okey**.  
  
## <a name="turn-on-server-backup"></a>Ativar o Backup do servidor  
 Você pode ativar Backup do servidor se ele foi desativado, criando **ProviderDisabled** chave do registro (conforme descrito anteriormente neste documento).  
  
 Você precisará excluir a chave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** para habilitar o backup do servidor padrão, alterar o tipo de inicialização do serviço de serviço de Backup de servidor do Windows Server e reiniciar o servidor.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Para excluir ServerBackup\ProviderDisabled? chave do registro  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  Na caixa de pesquisa, digite **regedit**e, em seguida, clique no **Regedit** aplicativo.  
  
3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**e, em seguida, expanda **ServerBackup**.  
  
4.  Clique com botão direito **ProviderDisabled**e clique em **excluir**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Alterar o tipo de inicialização do serviço de Backup de servidor Windows Server  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  Na caixa de pesquisa, digite **services.msc**e clique em **serviços** aplicativo.  
  
3.  No painel de serviços, clique com botão direito do **serviço de Backup de servidor Windows Server**e clique em **propriedades**.  
  
4.  Em **geral**, selecione **automática** para **tipo de inicialização**.  
  
5.  Clique em **Okey** para fechar a caixa de diálogo.  
  
#### <a name="restart-the-server"></a>Reiniciar o servidor  
  
1.  No servidor, mova o mouse para o canto superior direito da tela, clique em **configurações**, clique em **energia**e clique em Reiniciar.