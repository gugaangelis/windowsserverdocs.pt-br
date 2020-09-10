---
title: 'Etapa 3: Fazer computadores ingressarem no novo servidor do Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 7495831c6f593b65261fda8f50d4ef9d1000d9b4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625479"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Etapa 3: Fazer computadores ingressarem no novo servidor do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

A próxima etapa do processo de migração é conectar computadores cliente ao novo servidor que executa o Windows Server Essentials.

> [!NOTE]
>  Você pode pular esta etapa para computadores que executam os sistemas operacionais Windows Vista ou Windows XP. O software Connector do Windows Server não dá suporte a computadores que executam o Windows XP ou Windows Vista.

 Para poder ingressar um computador cliente no novo servidor do Windows Server Essentials, você deve desconectá-lo do servidor de origem desinstalando o software do conector do Windows Server no computador cliente.

### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Para desinstalar o Windows Server Connector de um computador cliente

1.  Em um computador cliente, abra o painel de controle e, em seguida, abra **Programas e recursos**.

2.  Na lista de programas, clique no aplicativo Connector que está em execução no seu computador.

    > [!NOTE]
    >  O aplicativo do conector pode ser o **conector do Windows Small Business Server 2011 Essentials**ou o **conector do Windows Server Essentials**, dependendo de qual versão do Windows Server Essentials o computador cliente estava conectado.

3.  Clique em **Desinstalar**.

### <a name="to-reconnect-a-client-computer-to-the-server"></a>Para reconectar um computador cliente ao servidor

1.  Autentique-se no computador que você deseja conectar ao servidor.

    > [!NOTE]
    >  Caso esse computador tenha várias contas de usuário, autentique-se com a conta de usuário cujos documentos, imagens e preferências pessoais você deseja manter depois de conectar o computador ao servidor.

2.  Abra um navegador de Internet, como o Internet Explorer.

3.  Na barra de endereços, digite **http://<ServerName \> /Connect**e pressione Enter.

4.  Siga as instruções na tela para ingressar o computador cliente no novo servidor do Windows Server Essentials.

## <a name="next-steps"></a>Próximas etapas
 Você ingressou seus computadores cliente no novo servidor que executa o Windows Server Essentials. Agora vá para a [etapa 4: mover as configurações e os dados para o servidor de destino para a migração do Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).


Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

