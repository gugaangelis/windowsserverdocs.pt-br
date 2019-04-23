---
title: ETAPA 6 instalar e configurar o DC1 2
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
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f31c0e1d36ff458fb4807ab6856a56498f449ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838177"
---
# <a name="step-6-install-and-configure-2-dc1"></a>ETAPA 6 instalar e configurar o DC1 2

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

2-DC1 fornece os seguintes serviços:  
  
-   Um controlador de domínio para o domínio de serviços de domínio Active Directory (AD DS) corp2.corp.contoso.com.  
  
-   Um servidor DNS para o domínio do DNS corp2.corp.contoso.com.  
  
A configuração de 2-DC1 consiste das seguintes opções:  
  
- Instalar o sistema operacional no DC1 2
  
- Configurar as propriedades de TCP/IP

- Configurar 2-DC1 como um controlador de domínio e servidor DNS

- Fornecer permissões de política de grupo para CORP\User1
  
- Permitir que os computadores CORP2 obter certificados de computador
  
- Forçar a replicação entre o DC1 e 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Instalar o sistema operacional no DC1 2  
Primeiro, instale o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Para instalar o sistema operacional no DC1 2  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta local de administrador.  
  
3.  Conectar-se o DC1 de 2 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte-se 2-DC1 à sub-rede Corpnet 2.  
  
## <a name="configure-tcpip-properties"></a>Configurar as propriedades de TCP/IP  
Configure o protocolo TCP/IP com endereços IP estáticos.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Para configurar TCP/IP em DC1 2  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  Em **Conexões de Rede**, clique com o botão direito do mouse em **Conexão Ethernet com Fio** e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.2.0.1**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Na **gateway padrão**, digite **10.2.0.254**. Clique em **usar os seguintes endereços de servidor DNS**, na **servidor DNS preferencial**, digite **10.2.0.1**e, na **servidor DNS alternativo**, tipo **10.0.0.1**.  
  
5.  Clique em **Avançado** e clique na guia **DNS**.  
  
6.  Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
7.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
8.  Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:2::1**. Na **comprimento do prefixo de sub-rede**, digite **64**. Na **gateway padrão**, digite **2001:db8:2::fe**. Clique em **usar os seguintes endereços de servidor DNS**, na **servidor DNS preferencial**, digite **2001:db8:2::1**e, na **servidor DNS alternativo**, tipo de **2001:db8:1::1**.  
  
9. Clique em **Avançado** e clique na guia **DNS**.  
  
10. Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
11. Sobre o **propriedades da Conexão Ethernet com fio** caixa de diálogo, clique em **fechar**.  
  
12. Feche a janela **Conexões de Rede** .  
  
13. No console do Gerenciador do servidor, no **servidor Local**, no **Properties** área, ao lado **nome do computador**, clique no link.  
  
14. Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
15. Sobre o **alterações de nome/domínio do computador** na caixa **nome do computador**, tipo **2-DC1**e, em seguida, clique em **Okey**.  
  
16. Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
17. Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
18. Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
19. Depois de reiniciar, faça logon usando a conta de administrador local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurar 2-DC1 como um controlador de domínio e servidor DNS  
Configure 2-DC1 como controlador de domínio para o domínio corp2.corp.contoso.com e como um servidor DNS para o domínio do DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Para configurar o DC1 2 como um controlador de domínio e servidor DNS  
  
1.  No console do Gerenciador do servidor, sobre o **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **próxima** três vezes para chegar à tela de seleção de função de servidor  
  
3.  Sobre o **selecionar funções de servidor** , selecione **Active Directory Domain Services**. Clique em **adicionar recursos** quando solicitado e, em seguida, clique em **próxima** três vezes.  
  
4.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  
  
5.  Quando a instalação for concluída com êxito, clique em **promover este servidor a um controlador de domínio**.  
  
6.  No Active Directory domínio serviços de Assistente de configuração, sobre o **configuração de implantação** , clique em **adicionar um novo domínio a uma floresta existente**.  
  
7.  Na **nome do domínio pai**, digite **corp.contoso.com**, na **novo nome de domínio**, tipo **corp2**.  
  
8.  Sob **fornecer as credenciais para executar essa operação**, clique em **alteração**. Sobre o **segurança do Windows** na caixa **nome de usuário**, tipo **corp.contoso.com\Administrator**e, na **senha**, insira o corp \ Senha do administrador, clique em **Okey**e, em seguida, clique em **próxima**.  
  
9. Sobre o **opções do controlador de domínio** página, certifique-se de que o **nome do Site** é **segundo Site**. Sob **digite a senha do modo de restauração dos serviços de diretório (DSRM)**, na **senha** e **Confirmar senha**, digite uma senha forte duas vezes e, em seguida, clique em  **Próxima** cinco vezes.  
  
10. Sobre o **verificação de pré-requisitos** página, depois que os pré-requisitos forem validados, clique em **instalar**.  
  
11. Aguarde até que o assistente conclui a configuração dos serviços do Active Directory e DNS e, em seguida, clique em **fechar**.  
  
12. Depois que o computador for reiniciado, faça logon no domínio CORP2 usando a conta de administrador.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Fornecer permissões de política de grupo para CORP\User1  
Use este procedimento para fornecer ao usuário CORP\User1 permissões completas para criar e alterar objetos de diretiva de grupo corp2.  
  
### <a name="to-provide-group-policy-permissions"></a>Para fornecer permissões de política de grupo  
  
1.  Sobre o **inicie** tela, digite**GPMC. msc**, e pressione ENTER.  
  
2.  No console de gerenciamento de diretiva de grupo, abra **floresta: corp.contoso.com/Domains/corp2.corp.contoso.com**.  
  
3.  No painel de detalhes, clique no **delegação** guia. No **permissão** lista suspensa, clique em **vincular GPOs**.  
  
4.  Clique em **Add**e na nova **Selecionar usuário, computador ou grupo** caixa de diálogo, clique em **locais**.  
  
5.  Sobre o **locais** na caixa a **local** de árvore, clique em **corp.contoso.com**e clique em **Okey**.  
  
6.  Na **insira o nome do objeto para selecionar** tipo **User1**, clique em **Okey**e, no **adicionar grupo ou usuário** caixa de diálogo, clique em **Okey** .  
  
7.  No console de gerenciamento de diretiva de grupo, na árvore, clique **Group Policy Objects**, e nos detalhes do clique em painel a **delegação** guia.  
  
8.  Clique em **Add**e na nova **Selecionar usuário, computador ou grupo** caixa de diálogo, clique em **locais**.  
  
9. Sobre o **locais** na caixa a **local** de árvore, clique em **corp.contoso.com**e clique em **Okey**.  
  
10. Na **insira o nome do objeto para selecionar** tipo **User1**, clique em **Okey**.  
  
11. No console de gerenciamento de diretiva de grupo, na árvore, clique **filtros WMI**, e nos detalhes do clique em Painel de **delegação** guia.  
  
12. Clique em **Add**e na nova **Selecionar usuário, computador ou grupo** caixa de diálogo, clique em **locais**.  
  
13. Sobre o **locais** na caixa a **local** de árvore, clique em **corp.contoso.com**e clique em **Okey**.  
  
14. Na **insira o nome do objeto para selecionar** tipo **User1**, clique em **Okey**. Sobre o **adicionar grupo ou usuário** diálogo caixa, certifique-se de que **permissões** são definidas como **controle total**e, em seguida, clique em **Okey**.  
  
15. Feche o console Gerenciamento de Política de Grupo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Permitir que os computadores CORP2 obter certificados de computador 

Computadores no domínio CORP2 devem obter certificados de computador da autoridade de certificação no APP1. Execute este procedimento no APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Para permitir que computadores CORP2 automaticamente obter certificados de computador  
  
1.  No APP1, clique em **inicie**, digite **certtmpl**, e pressione ENTER.  
  
2.  No **Console do modelo de certificados**, no painel central, clique duas vezes **autenticação cliente-servidor**.  
  
3.  Sobre o **as propriedades de autenticação de cliente-servidor** caixa de diálogo, clique o **segurança** guia.  
  
4.  Clique em **Add**e, na **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, clique em **locais**.  
  
5.  Sobre o **locais** na caixa **local**, expanda **corp.contoso.com**, clique em **corp2.corp.contoso.com**e, em seguida, clique em  **Okey**.  
  
6.  Na **digite os nomes de objeto para selecionar**, tipo **Admins. do domínio; Computadores do domínio** e, em seguida, clique em **Okey**.  
  
7.  Sobre o **as propriedades de autenticação de cliente-servidor** na caixa **nomes de grupo ou usuário**, clique em **Admins. do domínio (CORP2\Domain administradores)** e, em  **Permissões para administradores de domínio**, no **Allow** coluna, selecione **escrever** e **registrar**.  
  
8.  Na **nomes de usuário ou grupo**, clique em **computadores do domínio (computadores CORP2\Domain)** e, na **permissões para computadores do domínio**, no **permitir**coluna, selecione **registrar** e **registrar automaticamente**e, em seguida, clique em **Okey**.  
  
9. Feche o Console de Modelos de Certificado.  
  
## <a name="replication"></a>Forçar a replicação entre o DC1 e 2-DC1  
Antes de poder registrar para certificados em EDGE1 2, você deve forçar a replicação das configurações do DC1 para 2-DC1. Esta operação deve ser feita no DC1.  
  
### <a name="to-force-replication"></a>Para forçar a replicação  
  
1.  No DC1, clique em **inicie**e, em seguida, clique em **serviços e Sites do Active Directory**.  
  
2.  No console Serviços e Sites do Active Directory, na árvore, expanda **transportes entre sites**e, em seguida, clique em **IP**.  
  
3.  No painel de detalhes, clique duas vezes **DEFAULTIPSITELINK**.  
  
4.  Sobre o **DEFAULTIPSITELINK propriedades** na caixa **custo**, tipo **1**, no **replicar cada**, tipo **15**e, em seguida, clique em **Okey**. Aguarde de 15 minutos para a conclusão da replicação.  
  
5.  Para forçar a replicação agora na árvore de console, expanda **configurações Sites\Site padrão-First-Site-name\Servers\DC1\NTDS**, no painel de detalhes, clique com botão direito **<automatically generated>**, clique em  **Replicate Now**e, em seguida, o **Replicate Now** caixa de diálogo, clique em **Okey**.  
  
6.  Para garantir que a replicação foi concluída com êxito, faça o seguinte:  
  
    1.  Sobre o **inicie** tela, digite**cmd.exe**, e pressione ENTER.  
  
    2.  Digite o comando a seguir e pressione ENTER.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Certifique-se de que todas as partições são sincronizadas sem erros. Caso contrário, em seguida, execute o comando novamente até que nenhum erro for relatado antes de continuar.  
  
7.  Feche a janela do Prompt de Comando.  
  


