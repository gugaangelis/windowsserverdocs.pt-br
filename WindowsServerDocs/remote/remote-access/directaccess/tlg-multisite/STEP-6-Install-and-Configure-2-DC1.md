---
title: 'ETAPA 6: instalar e configurar o 2-DC1'
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 558c99c187ab01f3084621410964f3a01c0dace8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308660"
---
# <a name="step-6-install-and-configure-2-dc1"></a>ETAPA 6: instalar e configurar o 2-DC1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

o 2-DC1 fornece os seguintes serviços:  
  
-   Um controlador de domínio para o domínio de Active Directory Domain Services corp2.corp.contoso.com (AD DS).  
  
-   Um servidor DNS para o domínio DNS corp2.corp.contoso.com.  
  
a configuração de 2-DC1 consiste no seguinte:  
  
- Instalar o sistema operacional em 2-DC1
  
- Configurar as propriedades de TCP/IP

- Configurar 2-DC1 como um controlador de domínio e um servidor DNS

- Fornecer permissões de Política de Grupo para CORP\User1
  
- Permitir que computadores CORP2 obtenham certificados de computador
  
- Forçar a replicação entre DC1 e 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Instalar o sistema operacional em 2-DC1  
Primeiro, instale o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Para instalar o sistema operacional em 2-DC1  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta do Administrador local.  
  
3.  Conecte-se a 2-DC1 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e desconecte-se da Internet.  
  
4.  Conecte 2-DC1 à sub-rede 2-corpnet.  
  
## <a name="configure-tcpip-properties"></a>Configurar as propriedades de TCP/IP  
Configure o protocolo TCP/IP com endereços IP estáticos.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Para configurar TCP/IP em 2-DC1  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.  
  
2.  Em **Conexões de Rede**, clique com o botão direito do mouse em **Conexão Ethernet com Fio** e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.2.0.1**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Em **gateway padrão**, digite **10.2.0.254**. Clique em **usar os seguintes endereços de servidor DNS**, em **servidor DNS preferencial**, digite **10.2.0.1**e, em **servidor DNS alternativo**, digite **10.0.0.1**.  
  
5.  Clique em **Avançado** e clique na guia **DNS**.  
  
6.  Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
7.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
8.  Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:2:: 1**. Em **comprimento do prefixo da sub-rede**, digite **64**. Em **gateway padrão**, digite **2001: DB8:2:: Fe**. Clique em **usar os seguintes endereços de servidor DNS**, em **servidor DNS preferencial**, digite **2001: DB8:2:: 1**e, em **servidor DNS alternativo**, digite **2001: DB8:1:: 1**.  
  
9. Clique em **Avançado** e clique na guia **DNS**.  
  
10. Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
11. Na caixa de diálogo **Propriedades da conexão Ethernet com fio** , clique em **fechar**.  
  
12. Feche a janela **Conexões de Rede**.  
  
13. No console do Gerenciador do Servidor, no **servidor local**, na área **Propriedades** , ao lado de **nome do computador**, clique no link.  
  
14. Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
15. Na caixa de diálogo **alterações no nome do computador/domínio** , em **nome do computador**, digite **2-DC1**e clique em **OK**.  
  
16. Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
17. Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
18. Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
19. Após a reinicialização, faça logon usando a conta de administrador local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurar 2-DC1 como um controlador de domínio e um servidor DNS  
Configure 2-DC1 como um controlador de domínio para o domínio corp2.corp.contoso.com e como um servidor DNS para o domínio DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Para configurar 2-DC1 como um controlador de domínio e um servidor DNS  
  
1.  No console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para acessar a tela de seleção de função de servidor  
  
3.  Na página **selecionar funções de servidor** , selecione **Active Directory Domain Services**. Clique em **Adicionar recursos** quando solicitado e, em seguida, clique em **Avançar** três vezes.  
  
4.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  
  
5.  Quando a instalação for concluída com êxito, clique em **promover este servidor a um controlador de domínio**.  
  
6.  No assistente de configuração do Active Directory Domain Services, na página **configuração de implantação** , clique em **Adicionar um novo domínio a uma floresta existente**.  
  
7.  Em **nome de domínio pai**, digite **Corp.contoso.com**, em **novo nome de domínio**, digite **corp2**.  
  
8.  Em **fornecer as credenciais para executar esta operação**, clique em **alterar**. Na caixa de diálogo **segurança do Windows** , em **nome de usuário**, digite **Corp. contoso. com\Administrator**e, em **senha**, digite a senha do corp\Administrator, clique em **OK**e, em seguida, clique em **Avançar**.  
  
9. Na página **Opções do controlador de domínio** , verifique se o **nome do site** é **segundo site**. Em **digite a senha do modo de restauração dos serviços de diretório (DSRM)** , em **senha** e **Confirmar senha**, digite uma senha forte duas vezes e clique em **Avançar** cinco vezes.  
  
10. Na página **verificação de pré-requisitos** , depois que os pré-requisitos forem validados, clique em **instalar**.  
  
11. Aguarde até que o assistente conclua a configuração dos serviços de Active Directory e DNS e, em seguida, clique em **fechar**.  
  
12. Depois que o computador for reiniciado, faça logon no domínio CORP2 usando a conta de administrador.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Fornecer permissões de Política de Grupo para CORP\User1  
Use este procedimento para fornecer ao usuário CORP\User1 permissões totais para criar e alterar objetos do corp2 Política de Grupo.  
  
### <a name="to-provide-group-policy-permissions"></a>Para fornecer permissões de Política de Grupo  
  
1.  Na tela **Iniciar** , digite**GPMC. msc**e pressione Enter.  
  
2.  No console de gerenciamento do Política de Grupo, abra **floresta: Corp.contoso.com/Domains/corp2.Corp.contoso.com**.  
  
3.  No painel de detalhes, clique na guia **delegação** . Na lista suspensa **permissão** , clique em **vincular GPOs**.  
  
4.  Clique em **Adicionar**e, na caixa de diálogo novo **Selecionar usuário, computador ou grupo** , clique em **locais**.  
  
5.  Na caixa de diálogo **locais** , na árvore **local** , clique em **Corp.contoso.com**e em **OK**.  
  
6.  Em **Inserir o nome do objeto para selecionar o** tipo **Usuário1**, clique em **OK**e, na caixa de diálogo **Adicionar grupo ou usuário** , clique em **OK**.  
  
7.  No console de gerenciamento do Política de Grupo, na árvore, clique em **política de grupo objetos**e, no painel de detalhes, clique na guia **delegação** .  
  
8.  Clique em **Adicionar**e, na caixa de diálogo novo **Selecionar usuário, computador ou grupo** , clique em **locais**.  
  
9. Na caixa de diálogo **locais** , na árvore **local** , clique em **Corp.contoso.com**e em **OK**.  
  
10. Em **Inserir o nome do objeto para selecionar o** tipo **Usuário1**, clique em **OK**.  
  
11. No console de gerenciamento do Política de Grupo, na árvore, clique em **filtros WMI**e, no painel de detalhes, clique na guia **delegação** .  
  
12. Clique em **Adicionar**e, na caixa de diálogo novo **Selecionar usuário, computador ou grupo** , clique em **locais**.  
  
13. Na caixa de diálogo **locais** , na árvore **local** , clique em **Corp.contoso.com**e em **OK**.  
  
14. Em **Inserir o nome do objeto para selecionar o** tipo **Usuário1**, clique em **OK**. Na caixa de diálogo **Adicionar grupo ou usuário** , verifique se **as permissões** estão definidas como **controle total**e clique em **OK**.  
  
15. Feche o console Gerenciamento de Política de Grupo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Permitir que computadores CORP2 obtenham certificados de computador 

Os computadores no domínio CORP2 devem obter certificados de computador da autoridade de certificação no APP1. Execute este procedimento no APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Para permitir que computadores CORP2 obtenham automaticamente certificados de computador  
  
1.  No APP1, clique em **Iniciar**, digite **certtmpl. msc**e pressione Enter.  
  
2.  No **console de modelos de certificados**, no painel central, clique duas vezes em **autenticação cliente-servidor**.  
  
3.  Na caixa de diálogo **Propriedades de autenticação de cliente-servidor** , clique na guia **segurança** .  
  
4.  Clique em **Adicionar**e, na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , clique em **locais**.  
  
5.  Na caixa de diálogo **locais** , em **local**, expanda **Corp.contoso.com**, clique em **corp2.Corp.contoso.com**e, em seguida, clique em **OK**.  
  
6.  Em **Inserir os nomes de objeto a serem selecionados**, digite **Admins. do domínio; Computadores de domínio** e clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades de autenticação de cliente-servidor** , em **nomes de grupo ou de usuário**, clique em admins. do **domínio (CORP2\Domain admins)** e em **permissões para administradores de domínio**, na coluna **permitir** , selecione **gravar** e **registrar**.  
  
8.  Em **nomes de grupo ou de usuário**, clique em **computadores de domínio (computadores CORP2\Domain)** e em **permissões para computadores de domínio**, na coluna **permitir** , selecione **registrar** e **registrar automaticamente**e clique em **OK**.  
  
9. Feche o Console de Modelos de Certificado.  
  
## <a name="force-replication-between-dc1-and-2-dc1"></a><a name="replication"></a>Forçar a replicação entre DC1 e 2-DC1  
Antes de poder se registrar para certificados em 2-EDGE1, você deve forçar a replicação de configurações de DC1 para 2-DC1. Esta operação deve ser feita no DC1.  
  
### <a name="to-force-replication"></a>Para forçar a replicação  
  
1.  No DC1, clique em **Iniciar**e em **Active Directory sites e serviços**.  
  
2.  No console Active Directory sites e serviços, na árvore, expanda transportes **entre sites**e clique em **IP**.  
  
3.  No painel de detalhes, clique duas vezes em **DEFAULTIPSITELINK**.  
  
4.  Na caixa de diálogo **Propriedades de DEFAULTIPSITELINK** , em **custo**, digite **1**, em **replicar a cada**, digite **15**e clique em **OK**. Aguarde 15 minutos para a replicação ser concluída.  
  
5.  Para forçar a replicação agora na árvore de console, expanda **configurações de Sites\Default-First-site-name\Servers\DC1\NTDS**, no painel de detalhes, clique com o botão direito do mouse em **<automatically generated>** , clique em **replicar agora**e, na caixa de diálogo **replicar agora** , clique em **OK**.  
  
6.  Para garantir que a replicação seja concluída com êxito, faça o seguinte:  
  
    1.  Na tela **Iniciar** , digite**cmd. exe**e pressione Enter.  
  
    2.  Digite o comando a seguir e pressione ENTER.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Verifique se todas as partições estão sincronizadas sem erros. Caso contrário, execute novamente o comando até que nenhum erro seja relatado antes de continuar.  
  
7.  Feche a janela do Prompt de Comando.  
  


