---
title: Personalizar o Backup do Servidor
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838537"
---
# <a name="customize-server-backup"></a>Personalizar o Backup do Servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Desligar o Backup do Servidor por Padrão  
 É possível escolher desligar o Backup do Servidor por padrão. É preciso definir o valor de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** para 1 para habilitar essa opção.  
  
 Quando essa chave é definida, a interface do Usuário de Backup do Servidor não será exposta através do Dashboard ou Barra Inicial. Isso permite utilizar aplicativos de terceiros para Backup do Servidor.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Para adicionar ServerBackup\ProviderDisabled? chave do registro e defina o valor para 1  
  
1.  No servidor, clique em **Iniciar** e em **Executar**, digite **regedit** na caixa de texto **Abrir** e clique em **OK**.  
  
2.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server** e então expanda **ServerBackup**.  
  
3.  Clique com o botão direito em **ServerBackup**, clique em **Novo** e clique em **Valor de DWARD**.  
  
4.  Para o nome, digite **ProviderDisabled**.  
  
5.  Clique com o botão direito do mouse no nome, selecione **Modificar**, digite **1** para os dados de valor e, em seguida, clique em **OK**.  
  
## <a name="turn-on-server-backup"></a>Ativar o Backup do Servidor  
 Você pode ativar o Backup do Servidor se ele estiver desativado, criando a chave de registro **ProviderDisabled** (como descrito anteriormente neste documento).  
  
 É preciso excluir a chave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** para habilitar o backup de servidor padrão, alterar o tipo de início de serviço do Serviço de Backup de Servidores do Windows Server e reiniciar o servidor.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Para excluir ServerBackup\ProviderDisabled? chave do registro  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.  
  
2.  Na caixa de Pesquisa, digite **regedit**e, em seguida, clique no aplicativo **Regedit** .  
  
3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server** e então expanda **ServerBackup**.  
  
4.  Clique com o botão direito em **ProviderDisabled**e clique em **Excluir**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Altere o tipo de início do Serviço de Backup de Servidores do Windows Server  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.  
  
2.  Na caixa Pesquisar, digite **services.msc**e clique no aplicativo **Services** .  
  
3.  No painel de serviços, clique com o botão direito do mouse em **Serviço de Backup de Servidores do Windows Server** e clique em **Propriedades**.  
  
4.  Na guia **Geral** , selecione **Automático** para o **tipo de Inicialização**.  
  
5.  Clique em **OK** para fechar o diálogo.  
  
#### <a name="restart-the-server"></a>Reinicie o servidor  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Configurações**, clique em **Potência** e depois clique em Reiniciar.