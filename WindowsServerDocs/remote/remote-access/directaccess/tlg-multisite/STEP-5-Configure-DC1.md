---
title: ETAPA 5 configurar o DC1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108e517923c75f685d817cdf9fad9b14132e3bb0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281441"
---
# <a name="step-5-configure-dc1"></a>ETAPA 5 configurar o DC1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

DC1 atua como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com.  
  
Para configurar o acesso remoto para usar uma topologia de multissite, é necessário para adicionar um site de serviços de domínio Active Directory (AD DS) adicional para o segundo domínio controlador 2-DC1 e configurar o roteamento entre as sub-redes.  
  
1. Para configurar o gateway padrão no controlador de domínio. Configure o gateway padrão em DC1.  
  
2. Crie grupos de segurança para clientes Windows 7 DirectAccess no DC1. Quando o DirectAccess é configurado, ele cria automaticamente os objetos de diretiva de grupo (GPOs) e as configurações de GPO são aplicadas aos clientes DirectAccess e servidores. GPO do cliente do DirectAccess é aplicada a grupos de segurança específicos do Active Directory.  
  
3. Para adicionar um novo site do AD DS. Crie um segundo site AD DS.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Para configurar o gateway padrão no controlador de domínio  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  Na janela Conexões de rede, clique com botão direito **Conexão de Ethernet com fio**e, em seguida, clique em **propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Na **gateway padrão**, digite **10.0.0.254**e, na **servidor DNS alternativo**, tipo **10.2.0.1**e, em seguida, clique em **Okey** .  
  
5.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
6.  Na **gateway padrão**, digite **2001:db8:1::fe**e, na **servidor DNS alternativo**, tipo **2001:db8:2::1**e, em seguida, clique em **Okey**.  
  
7.  Sobre o **propriedades da Conexão Ethernet com fio** caixa de diálogo, clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede** .  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Criar grupos de segurança para clientes Windows 7 DirectAccess no DC1  
Crie os grupos de segurança do DirectAccess para o Windows 7 com o procedimento a seguir.  
  
 Computadores cliente do Windows 7 devem ser membros de grupos de segurança separados porque eles são capazes de se conectar aos recursos internos por meio de um único ponto de entrada apenas. Quando habilitar o suporte a multissite ou adicionando a entrada de pontos, se o suporte do Windows 7 for solicitado, em seguida, um GPO separado será criado automaticamente por clientes DirectAccess para Windows 7 para cada ponto de entrada.  
  
### <a name="create-security-groups"></a>Criar grupos de segurança  
  
1.  Sobre o **inicie** tela, digite**DSA. msc**, e pressione ENTER.  
  
2.  No painel esquerdo, expanda **corp.contoso.com**, clique em **usuários**, em seguida, clique com botão direito **usuários**, aponte para **novo**e, em seguida, clique em **Grupo**.  
  
3.  Sobre o **novo objeto - grupo** caixa de diálogo **nome do grupo**, insira **Win7_Clients_Site1**.  
  
4.  Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
5.  Clique duas vezes o **Win7_Clients_Site1** grupo de segurança e, na **propriedades Win7_Clients_Site1** caixa de diálogo, clique no **membros** guia.  
  
6.  Na guia **Membros** , clique em **Adicionar**.  
  
7.  Sobre o **selecionar usuários, contatos, computadores ou contas de serviço** caixa de diálogo, clique em **tipos de objeto**. Sobre o **tipos de objetos** caixa de diálogo, selecione **computadores**e, em seguida, clique em **Okey**.  
  
8.  Na **digite os nomes de objeto para selecionar**, tipo **client2**e, em seguida, clique em **Okey**e, em seguida, no **Win7_Clients_Site1 propriedades** caixa de diálogo caixa de clicar **Okey**.  
  
9. No **Active Directory Users and Computers** console, no painel esquerdo, clique com botão direito **usuários**, aponte para **New**e, em seguida, clique em **grupo** .  
  
10. Sobre o **novo objeto - grupo** caixa de diálogo **nome do grupo**, insira **Win7_Clients_Site2**.  
  
11. Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
12. Feche o console **Usuários e Computadores do Active Directory** .  
  
## <a name="to-add-a-new-ad-ds-site"></a>Para adicionar um novo site do AD DS  
  
1.  Sobre o **inicie** tela, digite**Dssite. msc**, e, em seguida, pressione ENTER.  
  
2.  No console Serviços e Sites do Active Directory, na árvore de console, clique com botão direito **Sites**e, em seguida, clique em **novo Site**.  
  
3.  Sobre o **novo objeto - Site** na caixa de **nome** , digite **segundo Site**.  
  
4.  Na caixa de listagem, clique em **DEFAULTIPSITELINK**e, em seguida, clique em **Okey** duas vezes.  
  
5.  Na árvore de console, expanda **Sites**, clique com botão direito **sub-redes**e, em seguida, clique em **nova sub-rede**.  
  
6.  No **novo objeto - subrede** caixa de diálogo **prefixo**, tipo **10.0.0.0/24**, no **selecione um objeto de site para esse prefixo** , clique em **Default-First-Site-Name**e, em seguida, clique em **Okey**.  
  
7.  Na árvore de console, clique com botão direito **subredes**e, em seguida, clique em **nova sub-rede**.  
  
8.  No **novo objeto - subrede** caixa de diálogo **prefixo**, tipo **2001:db8:1:: 64**, no **selecione um objeto de site para esse prefixo** lista, Clique em **Default-First-Site-Name**e, em seguida, clique em **Okey**.  
  
9. Na árvore de console, clique com botão direito **subredes**e, em seguida, clique em **nova sub-rede**.  
  
10. No **novo objeto - subrede** caixa de diálogo **prefixo**, tipo **10.2.0.0/24**, no **selecione um objeto de site para esse prefixo** , clique em **Segundo Site**e, em seguida, clique em **Okey**.  
  
11. Na árvore de console, clique com botão direito **subredes**e, em seguida, clique em **nova sub-rede**.  
  
12. No **novo objeto - subrede** caixa de diálogo **prefixo**, tipo **2001:db8:2:: 64**, no **selecione um objeto de site para esse prefixo** lista, Clique em **segundo Site**e, em seguida, clique em **Okey**.  
  
13. Feche os serviços e Sites do Active Directory.  
  


