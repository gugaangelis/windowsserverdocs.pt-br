---
title: Gerenciar VPN no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Gerenciar VPN no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Conexões de rede virtual privada (VPN) permitem que os usuários trabalham em casa ou viajando acessar um servidor em uma rede privada usando a infraestrutura fornecida por uma rede pública, como a Internet. Para usar VPN para acessar os recursos de servidor, você deve concluir o seguinte:  
  
-   [Permitir VPN para acesso remoto no servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Definir permissões de VPN para usuários de rede](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Conectar computadores cliente ao servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Usar a VPN para se conectar ao Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>Permitir VPN para acesso remoto no servidor  
 Conclua o procedimento a seguir para configurar a VPN no Windows Server Essentials para permitir o acesso remoto.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Para habilitar a VPN no Windows Server Essentials  
  
1.  Abra o painel.  
  
2.  Clique em **configurações**e, em seguida, clique no **acesso em qualquer local** guia.  
  
3.  Clique em **configurar**. Assistente de configuração de qualquer lugar acesso aparece.  
  
4.  Sobre o **escolher qualquer lugar acessar os recursos para permitir que** página, selecione o **rede Virtual privada** caixa de seleção.  
  
5.  Siga as instruções para concluir o assistente.  
  
##  <a name="BKMK_2"></a>Definir permissões de VPN para usuários de rede  
 Você pode usar VPN para se conectar ao Windows Server Essentials e acessar todos os recursos que são armazenados no servidor. Isso é especialmente útil se você tiver um computador cliente que é configurado com contas de rede que podem ser usadas para se conectar a um servidor Windows Server Essentials hospedado por meio de uma conexão VPN. Todas as contas de usuário recém criada no servidor Windows Server Essentials hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Para definir as permissões de VPN para usuários de rede  
  
1.  Abra o painel.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja conceder permissões para acessar a área de trabalho remota.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **propriedades**.  
  
5.  No **< usuário Account\ > propriedades**, clique no **acesso em qualquer local** guia.  
  
6.  Sobre o **acesso em qualquer local** tab, para permitir que um usuário se conectar ao servidor usando VPN, selecione o **permitir rede Virtual privada (VPN)** caixa de seleção.  
  
7.  Clique em **aplicar**e clique em **Okey**.  
  
##  <a name="BKMK_Connect"></a>Conectar computadores cliente ao servidor  
 Após a VPN está habilitada em um servidor que executa o Windows Server Essentials para acesso remoto, você pode usar uma conexão VPN para se conectar a e acessar todos os recursos que são armazenados no servidor. No entanto, primeiro você deve conectar o computador para o servidor. Quando você conectar um computador para o servidor usando o conectar meu computador para o Assistente de servidor, uma conexão de rede VPN é gerado automaticamente no computador cliente e pode ser usado para acessar recursos do servidor enquanto trabalham em casa ou viajando. Para obter instruções passo a passo sobre como conectar seu computador para o servidor, veja [conectar computadores para o servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_3"></a>Usar a VPN para se conectar ao Windows Server Essentials  
 Se você tiver um computador cliente que é configurado com contas de rede que podem ser usadas para se conectar a um servidor hospedado executando o Windows Server Essentials por meio de uma conexão de VPN, todas as contas de usuário recém criada servidor hospedado deve usar VPN para fazer logon no computador cliente pela primeira vez. Conclua o procedimento a seguir do computador cliente que esteja conectado ao servidor.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Usar VPN para acessar recursos do servidor remotamente  
  
1.  Pressione Ctrl + Alt + Delete no computador cliente.  
  
2.  Clique em **Alternar usuário** na tela de logon.  
  
3.  Clique no ícone de logon de rede no canto inferior direito da tela.  
  
4.  Faça logon na rede do Windows Server Essentials usando seu nome de usuário e senha.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Trabalhar remotamente](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso em qualquer lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
