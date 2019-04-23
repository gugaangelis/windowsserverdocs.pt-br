---
title: ETAPA 7 instalar e configurar o APP1 2
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b0f91b4d2b876cb7b22dc8614e7ea5dcce6da2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833557"
---
# <a name="step-7-install-and-configure-2-app1"></a>ETAPA 7 instalar e configurar o APP1 2

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

2-APP1 fornece web e serviços de compartilhamento de arquivo. Configuração do APP1 2 consiste no seguinte:  
  
- Instalar o sistema operacional no APP1 2  
  
- Configurar as propriedades de TCP/IP  
  
- Ingressar no domínio de CORP2 2-APP1  
  
- Instalar a função de servidor Web (IIS) no APP1 2  
  
- Crie uma pasta compartilhada no APP1 2 
  
## <a name="bkmk_InstallOS"></a>Instalar o sistema operacional no APP1 2  
Primeiro, instale o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Para instalar o sistema operacional em APP1 2  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa).  
  
2.  Siga as instruções para concluir a instalação, especificando uma senha forte para a conta local de administrador. Faça logon usando a conta local de administrador.  
  
3.  Conectar-se 2-APP1 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte-se 2-APP1 na sub-rede Corpnet 2.  
  
## <a name="bkmk_TCP"></a>Configurar propriedades de TCP/IP  
Configure propriedades de TCP/IP em 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Para configurar as propriedades de TCP/IP  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  No **conexões de rede** janela, clique com botão direito **Conexão de Ethernet com fio**e, em seguida, clique em **propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.2.0.3**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Na **gateway padrão**, digite **10.2.0.254**.  
  
5.  Clique em **Usar os seguintes endereços de servidor DNS**. Na **servidor DNS preferencial**, digite **10.2.0.1**.  
  
6.  Clique em **Avançado** e clique na guia **DNS**. Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e clique em **Okey** duas vezes.  
  
7.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
8.  Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:2::3**. Na **comprimento do prefixo de sub-rede**, digite **64**. Na **gateway padrão**, digite **2001:db8:2::fe**. Clique em **usar os seguintes endereços de servidor DNS**e, na **servidor DNS preferencial**, digite **2001:db8:2::1**.  
  
9. Clique em **Avançado** e clique na guia **DNS**.  
  
10. Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
11. Sobre o **propriedades de Conexão Ethernet com fio** caixa de diálogo clicar **fechar**.  
  
12. Feche a janela **Conexões de Rede** .  
  
## <a name="bkmk_JoinDomain"></a>Ingressar no domínio de CORP2 2-APP1  
Ingresse no domínio de corp2.corp.contoso.com 2-APP1.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Para ingressar no domínio de CORP2 2-APP1  
  
1.  No console do Gerenciador do servidor, no **servidor Local**, no **Properties** área, ao lado **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Na **nome do computador**, digite **2-APP1**. No **membro**, clique em **domínio**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey**.  
  
4.  Quando você for solicitado um nome de usuário e senha, digite **Administrator** e a senha e clique **Okey**.  
  
5.  Quando você vir uma caixa de diálogo boas-vindas ao domínio corp2.corp.contoso.com, clique em **Okey**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, clique em **trocar de usuário**e, em seguida, clique em **outro usuário** e faça logon no domínio CORP2 com a conta de administrador.  
  
## <a name="bkmk_IIS"></a>Instalar a função de servidor Web (IIS) no APP1 2  
Instale a função de servidor Web (IIS) para tornar 2-APP1 um servidor web.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Para instalar a função de servidor Web (IIS)  
  
1.  No console do Gerenciador do servidor, sobre o **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **próxima** três vezes para chegar à tela de seleção de função de servidor  
  
3.  Sobre o **selecionar funções de servidor** , selecione **servidor Web (IIS)** e, em seguida, clique em **próxima** quatro vezes.  
  
4.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  
  
5.  Verificar se a instalação foi bem-sucedida e, em seguida, clique em **fechar**.  
  
## <a name="bkmk_Share"></a>Crie uma pasta compartilhada no APP1 2  
Crie uma pasta compartilhada e um arquivo de texto dentro da pasta em APP1 2.  
  
#### <a name="to-create-a-shared-folder"></a>Para criar uma pasta compartilhada  
  
1.  Sobre o **inicie** tela, digite**explorer.exe**, e pressione ENTER.  
  
2.  Clique em **computador**, em seguida, clique duas vezes em **disco Local (c)**.  
  
3.  Clique em **nova pasta**, digite **arquivos**, e pressione ENTER. Deixe o **disco Local** janela aberta.  
  
4.  No **inicie** tela, digite**notepad.exe**, clique com botão direito **o bloco de notas**, clique em **avançado**e, em seguida, clique em **executar como administrador**.  
  
5.  No **sem título - bloco de notas** , digite **esse é um arquivo compartilhado no APP1 2**.  
  
6.  Clique em **arquivo**, clique em **salve**, clique em **computador**, clique duas vezes em **disco Local (c)** e, em seguida, clique duas vezes o **arquivos**  pasta.  
  
7.  Na **nome do arquivo**, digite **example**e, em seguida, clique em **salvar**. Feche o Bloco de Notas.  
  
8.  No **disco Local** janela, clique com botão direito do **arquivos** pasta, aponte para **compartilhar com**e, em seguida, clique em **pessoas específicas**.  
  
9. Sobre o **compartilhamento de arquivos** caixa de diálogo, na lista suspensa, clique em **todas as pessoas**e, em seguida, clique em **adicionar**. Na **nível de permissão** para **Everyone**, clique em **leitura/gravação**.  
  
10. Clique em **compartilhamento**e, em seguida, clique em **feito**.  
  
11. Fechar o **disco Local** janela.  
  


