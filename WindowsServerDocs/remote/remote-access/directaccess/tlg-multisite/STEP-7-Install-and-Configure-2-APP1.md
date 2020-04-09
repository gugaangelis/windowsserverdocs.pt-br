---
title: ETAPA 7 instalar e configurar o 2-APP1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8b6f5438498349e7d02fd6c9122d300e55fe2517
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861549"
---
# <a name="step-7-install-and-configure-2-app1"></a>ETAPA 7 instalar e configurar o 2-APP1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

2-APP1 fornece serviços de compartilhamento de arquivos e Web. a configuração 2-APP1 consiste no seguinte:  
  
- Instalar o sistema operacional em 2-APP1  
  
- Configurar as propriedades de TCP/IP  
  
- Junção 2-APP1 ao domínio CORP2  
  
- Instalar a função do servidor Web (IIS) em 2-APP1  
  
- Criar uma pasta compartilhada em 2-APP1 
  
## <a name="install-the-operating-system-on-2-app1"></a><a name="bkmk_InstallOS"></a>Instalar o sistema operacional em 2-APP1  
Primeiro, instale o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Para instalar o sistema operacional em 2-APP1  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa).  
  
2.  Siga as instruções para concluir a instalação, especificando uma senha forte para a conta local de administrador. Faça logon usando a conta do Administrador local.  
  
3.  Conecte o 2-APP1 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012 e desconecte-se da Internet.  
  
4.  Conecte o 2-APP1 à sub-rede 2-corpnet.  
  
## <a name="configure-tcpip-properties"></a><a name="bkmk_TCP"></a>Configurar as propriedades de TCP/IP  
Configure as propriedades de TCP/IP em 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Para configurar as propriedades de TCP/IP  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.  
  
2.  Na janela **conexões de rede** , clique com o botão direito do mouse em **conexão Ethernet com fio**e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.2.0.3**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Em **gateway padrão**, digite **10.2.0.254**.  
  
5.  Clique em **Usar os seguintes endereços de servidor DNS**. Em **servidor DNS preferencial**, digite **10.2.0.1**.  
  
6.  Clique em **avançado**e, em seguida, clique na guia **DNS** . Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
7.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
8.  Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:2:: 3**. Em **comprimento do prefixo da sub-rede**, digite **64**. Em **gateway padrão**, digite **2001: DB8:2:: Fe**. Clique em **usar os seguintes endereços de servidor DNS**e, em **servidor DNS preferencial**, digite **2001: DB8:2:: 1**.  
  
9. Clique em **Avançado** e clique na guia **DNS**.  
  
10. Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
11. Na caixa de diálogo **Propriedades da conexão Ethernet com fio** , clique em **fechar**.  
  
12. Feche a janela **Conexões de Rede**.  
  
## <a name="join-2-app1-to-the-corp2-domain"></a><a name="bkmk_JoinDomain"></a>Junção 2-APP1 ao domínio CORP2  
Junção 2-APP1 ao domínio corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Para unir 2-APP1 ao domínio CORP2  
  
1.  No console do Gerenciador do Servidor, no **servidor local**, na área **Propriedades** , ao lado de **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Em **nome do computador**, digite **2-App1**. Em **membro de**, clique em **domínio**, digite **corp2.Corp.contoso.com**e clique em **OK**.  
  
4.  Quando for solicitado um nome de usuário e uma senha, digite **administrador** e sua senha e clique em **OK**.  
  
5.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio corp2.corp.contoso.com, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, clique em **Alternar usuário**e, em seguida, clique em **outro usuário** e faça logon no domínio CORP2 com a conta de administrador.  
  
## <a name="install-the-web-server-iis-role-on-2-app1"></a><a name="bkmk_IIS"></a>Instalar a função do servidor Web (IIS) em 2-APP1  
Instale a função do servidor Web (IIS) para tornar o 2-APP1 um servidor Web.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Para instalar a função do servidor Web (IIS)  
  
1.  No console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para acessar a tela de seleção de função de servidor  
  
3.  Na página **selecionar funções de servidor** , selecione **servidor Web (IIS)** e clique em **Avançar** quatro vezes.  
  
4.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  
  
5.  Verifique se a instalação foi bem-sucedida e clique em **fechar**.  
  
## <a name="create-a-shared-folder-on-2-app1"></a><a name="bkmk_Share"></a>Criar uma pasta compartilhada em 2-APP1  
Crie uma pasta compartilhada e um arquivo de texto dentro da pasta em 2-APP1.  
  
#### <a name="to-create-a-shared-folder"></a>Para criar uma pasta compartilhada  
  
1.  Na tela **Iniciar** , digite**Explorer. exe**e pressione Enter.  
  
2.  Clique em **computador**e clique duas vezes em **disco local (C:)** .  
  
3.  Clique em **nova pasta**, digite **arquivos**e pressione Enter. Deixe a janela **disco local** aberta.  
  
4.  Na tela **Iniciar** , digite**Notepad. exe**, clique com o botão direito do mouse em **bloco de notas**, clique em **avançado**e, em seguida, clique em **Executar como administrador**.  
  
5.  Na janela sem **título-bloco de notas** , digite **este é um arquivo compartilhado em 2-App1**.  
  
6.  Clique em **arquivo**, em **salvar**, em **computador**, clique duas vezes em **disco local (C:)** e, em seguida, clique duas vezes na pasta **arquivos** .  
  
7.  Em **nome do arquivo**, digite **example. txt**e clique em **salvar**. Feche o Bloco de Notas.  
  
8.  Na janela **disco local** , clique com o botão direito do mouse na pasta **arquivos** , aponte para **compartilhar com**e clique em **pessoas específicas**.  
  
9. Na caixa de diálogo **compartilhamento de arquivos** , na lista suspensa, clique em **todos**e, em seguida, clique em **Adicionar**. Em **nível de permissão** para **todos**, clique em **leitura/gravação**.  
  
10. Clique em **compartilhar**e, em seguida, clique em **concluído**.  
  
11. Feche a janela **disco local** .  
  


