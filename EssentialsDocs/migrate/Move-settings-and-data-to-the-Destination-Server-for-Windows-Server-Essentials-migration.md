---
title: Mover configurações e dados para o servidor de destino para migração para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4f4ba08c17429f70ef754b0861553e38ba116e5d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318836"
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover configurações e dados para o servidor de destino para migração para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e os dados para o servidor de destino da seguinte maneira:

1. [Copiar dados para o servidor de destino](#copy-data-to-the-destination-server)

2. [Configurar a rede](#configure-the-network) 

3. [Mapear computadores permitidos para contas de usuário](#map-permitted-computers-to-user-accounts)
 
## <a name="copy-data-to-the-destination-server"></a>Copie os dados para o servidor de destino
 Antes de copiar os dados do servidor de origem para o servidor de destino, execute as seguintes tarefas: 
 
- Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Crie ou personalize as pastas no servidor de destino para corresponderem à estrutura de pasta que você está migrando do servidor de origem. 
 
- Revise o tamanho de cada pasta e certifique-se de que o servidor de destino tenha espaço de armazenamento suficiente. 
 
- Torne as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma gravação possa ocorrer na unidade enquanto você estiver copiando arquivos para o servidor de destino. 
 
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar os dados do servidor de origem para o servidor de destino 
 
1. Faça logon no servidor de destino como administrador de domínio e, em seguida, abra uma janela Comando. 
 
2. No prompt de comando, digite o seguinte comando e pressione ENTER: 
 
 `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 
 
 Onde:
 - \<SourceServerName\> é o nome do servidor de origem
 - \<SharedSourceFolderName\> é o nome da pasta compartilhada no servidor de origem
 - \<DestinationServerName\> é o nome do servidor de destino,
 - \<SharedDestinationFolderName\> é a pasta compartilhada no servidor de destino para a qual os dados serão copiados. 
 
3. Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem. 
 
## <a name="configure-the-network"></a>Configurar a rede
 Depois que você mover a função DHCP no roteador, defina as configurações de rede no servidor de destino. 
 
#### <a name="to-configure-the-network"></a>Para configurar a rede 
 
1. No servidor de destino, abra o painel. 
 
2. Na página **Home** do painel, clique em **INSTALAÇÃO**, clique em **Configurar Acesso em Qualquer Local** e escolha a opção **Clique para configurar o Acesso em Qualquer Local**. 
 
3. Siga as instruções no assistente para configurar seu roteador e nomes de domínio. 
 
 Se o roteador não oferecer suporte para a estrutura UPnP, ou se a estrutura UPnP estiver desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que sejam direcionadas para o endereço IP do servidor de destino: 
 
- Porta 80: tráfego HTTP da Web 
 
- Porta 443: tráfego HTTPS da Web 
 
## <a name="map-permitted-computers-to-user-accounts"></a>Mapeie os computadores permitidos para as contas de usuário
 Cada conta de usuário que for migrada do servidor de origem deve ser mapeada para um ou mais computadores. 
 
#### <a name="to-map-user-accounts-to-computers"></a>Para mapear contas de usuário para computadores
 
1. Abra o Painel do Windows Server Essentials. 
 
2. Na barra de navegação, clique em **Usuários**. 
 
3. Na lista de contas de usuário, clique em uma conta de usuário e clique em **Exibir Propriedades da Conta**. 
 
4. Clique na guia **Acesso em Qualquer Local** e, em seguida, clique em **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web**. 
 
5. Selecione **Pastas Compartilhadas**, selecione **Computadores**, selecione **Links da Home Page** e, em seguida, clique em **Aplicar**. 
 
6. Clique na guia **Acesso ao Computador** e, em seguida, clique no nome do computador ao qual deseja permitir o acesso. 
 
7. Repita as etapas 3, 4, 5 e 6 para cada conta de usuário. 
 
> [!NOTE]
> Você não precisará alterar a configuração do computador cliente. Ela é definida automaticamente. 
>
> Depois de concluir a migração, se encontrar um problema ao criar a primeira nova conta de usuário no servidor de destino, remova a conta de usuário adicionada e crie a conta novamente.
