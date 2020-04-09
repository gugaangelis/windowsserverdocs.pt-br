---
title: ETAPA 5 configurar DC1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d7bf3fff3100f866b16d4932cf9df05d16c50d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861569"
---
# <a name="step-5-configure-dc1"></a>ETAPA 5 configurar DC1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

DC1 atua como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com.  
  
Para configurar o acesso remoto para usar uma topologia multissite, é necessário adicionar um site Active Directory Domain Services (AD DS) adicional para o segundo controlador de domínio 2-DC1 e configurar o roteamento entre as sub-redes.  
  
1. Para configurar o gateway padrão no controlador de domínio. Configure o gateway padrão no DC1.  
  
2. Crie grupos de segurança para clientes do Windows 7 DirectAccess no DC1. Quando o DirectAccess é configurado, ele cria automaticamente objetos de Política de Grupo (GPOs) e configurações de GPO que são aplicados aos clientes e servidores DirectAccess. O GPO do cliente do DirectAccess é aplicado a grupos de segurança Active Directory específicos.  
  
3. Para adicionar um novo site AD DS. Crie um segundo site AD DS.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Para configurar o gateway padrão no controlador de domínio  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.  
  
2.  Na janela conexões de rede, clique com o botão direito do mouse em **conexão Ethernet com fio**e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Em **gateway padrão**, digite **10.0.0.254**e, em **servidor DNS alternativo**, digite **10.2.0.1**e clique em **OK**.  
  
5.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
6.  Em **gateway padrão**, digite **2001: DB8:1:: Fe**e, em **servidor DNS alternativo**, digite **2001: DB8:2:: 1**e clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades da conexão Ethernet com fio** , clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Criar grupos de segurança para clientes do Windows 7 DirectAccess no DC1  
Crie os grupos de segurança do DirectAccess para o Windows 7 com o procedimento a seguir.  
  
 Os computadores cliente do Windows 7 devem ser membros de grupos de segurança separados porque eles são capazes de se conectar a recursos internos por meio de apenas um único ponto de entrada. Ao habilitar o suporte multissite ou adicionar pontos de entrada, se o suporte do Windows 7 for solicitado, um GPO separado será criado automaticamente pelo DirectAccess para clientes do Windows 7 para cada ponto de entrada.  
  
### <a name="create-security-groups"></a>Criar grupos de segurança  
  
1.  Na tela **Iniciar** , digite**DSA. msc**e pressione Enter.  
  
2.  No painel esquerdo, expanda **Corp.contoso.com**, clique em **usuários**, clique com o botão direito do mouse em **usuários**, aponte para **novo**e clique em **grupo**.  
  
3.  Na caixa de diálogo **novo grupo de objetos** , em **nome do grupo**, digite **Win7_Clients_Site1**.  
  
4.  Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
5.  Clique duas vezes no grupo de segurança **Win7_Clients_Site1** e, na caixa de diálogo **Propriedades do Win7_Clients_Site1** , clique na guia **Membros** .  
  
6.  Na guia **Membros**, clique em **Adicionar**.  
  
7.  Na caixa de diálogo **Selecionar usuários, contatos, computadores ou contas de serviço** , clique em **tipos de objeto**. Na caixa de diálogo **tipos de objeto** , selecione **computadores**e clique em **OK**.  
  
8.  Em **Inserir os nomes de objeto a serem selecionados**, digite **CLIENT2**e clique em **OK**e, na caixa de diálogo **Propriedades do Win7_Clients_Site1** , clique em **OK**.  
  
9. No console **Active Directory usuários e computadores** , no painel esquerdo, clique com o botão direito do mouse em **usuários**, aponte para **novo**e clique em **grupo**.  
  
10. Na caixa de diálogo **novo grupo de objetos** , em **nome do grupo**, digite **Win7_Clients_Site2**.  
  
11. Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
12. Feche o console **Usuários e Computadores do Active Directory**.  
  
## <a name="to-add-a-new-ad-ds-site"></a>Para adicionar um novo site AD DS  
  
1.  Na tela **Iniciar** , digite**dssite. msc**e pressione Enter.  
  
2.  No console Active Directory sites e serviços, na árvore de console, clique com o botão direito do mouse em **sites**e clique em **novo site**.  
  
3.  Na caixa de diálogo **novo objeto – site** , na caixa **nome** , digite **segundo site**.  
  
4.  Na caixa de listagem, clique em **DEFAULTIPSITELINK**e em **OK** duas vezes.  
  
5.  Na árvore de console, expanda **sites**, clique com o botão direito do mouse em **sub-redes**e clique em **nova sub-rede**.  
  
6.  Na caixa de diálogo **novo objeto-sub-rede** , em **prefixo**, digite **10.0.0.0/24**, na lista **selecionar um objeto do site para este prefixo** , clique em **primeiro-site-padrão**e, em seguida, clique em **OK**.  
  
7.  Na árvore de console, clique com o botão direito do mouse em **sub-redes**e clique em **nova sub-rede**.  
  
8.  Na caixa de diálogo **novo objeto-sub-rede** , em **prefixo**, digite **2001: DB8:1::/64**, na lista **selecionar um objeto de site para este prefixo** , clique em **nome-primeiro-site-padrão**e em **OK**.  
  
9. Na árvore de console, clique com o botão direito do mouse em **sub-redes**e clique em **nova sub-rede**.  
  
10. Na caixa de diálogo **novo objeto-sub-rede** , em **prefixo**, digite **10.2.0.0/24**, na lista **selecionar um objeto de site para este prefixo** , clique em **segundo site**e, em seguida, clique em **OK**.  
  
11. Na árvore de console, clique com o botão direito do mouse em **sub-redes**e clique em **nova sub-rede**.  
  
12. Na caixa de diálogo **novo objeto-sub-rede** , em **prefixo**, digite **2001: DB8:2::/64**, na lista **selecionar um objeto do site para este prefixo** , clique em **segundo site**e, em seguida, clique em **OK**.  
  
13. Feche Active Directory sites e serviços.  
  


