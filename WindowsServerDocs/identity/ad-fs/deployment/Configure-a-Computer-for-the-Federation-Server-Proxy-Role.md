---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: "Configurar um computador para a função de Proxy do servidor de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar um computador para a função de Proxy do servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de configurar um computador com os certificados necessários e tiver instalado o serviço de função Proxy de serviço de federação, você estará pronto para configurar o computador para se tornar um proxy de servidor de Federação. Você pode usar o procedimento a seguir para que o computador funciona na função de proxy do servidor de Federação.  
  
> [!IMPORTANT]  
> Antes de usar este procedimento para configurar o computador de proxy do servidor de federação, certifique-se de que você seguiu todas as etapas em [lista de verificação: configuração de backup de Proxy de servidor de Federação um](Checklist--Setting-Up-a-Federation-Server-Proxy.md) na ordem em que estão listados. Certifique-se de que esse servidor de Federação pelo menos um é implantado e credenciais de todos os a necessária para autorizar uma configuração de proxy do servidor de Federação são implementadas. Você também deverá definir associações de Secure Sockets Layer \(SSL\) no Site da Web padrão ou esse assistente não será iniciado. Todas essas tarefas devem ser concluídas antes esse proxy do servidor de Federação pode funcionar.  
  
Depois de concluir a configuração do computador, verifique se que o proxy do servidor de Federação está funcionando conforme o esperado. Para obter mais informações, consulte [verificar se uma federação servidor Proxy é operacional](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Para configurar um computador para a função de proxy do servidor de Federação  
  
1.  Há duas maneiras de iniciar o Assistente para configuração do servidor de Federação do AD FS. Para iniciá-lo, siga um destes procedimentos:  
  
    -   Sobre o **iniciar** de tela, digite**Assistente de configuração do AD FS federação servidor Proxy**, e pressione ENTER.  
  
    -   A qualquer momento depois que o Assistente para instalação for concluída, abra o Windows Explorer, navegue até o **C:\\Windows\\ADFS** pasta e, em seguida, clique em double\ **FspConfigWizard.exe**.  
  
2.  Com um dos métodos, iniciá-lo e na **boas-vindas** página, clique em **próxima**.  
  
3.  Sobre o **especificar nome do serviço de Federação** página, em **nome do serviço de Federação**, digite o nome que representa o serviço de federação para o qual este computador vai atuar na função de proxy.  
  
4.  Com base nas necessidades de rede específica, determine se você precisa usar um servidor proxy HTTP para encaminhar as solicitações para o serviço de Federação. Em caso afirmativo, selecione o **usar um servidor proxy HTTP ao enviar solicitações para esse serviço de Federação** caixa de seleção em **endereço do servidor proxy HTTP** digite o endereço do servidor proxy, clique em **Conexão de teste** para verificar a conectividade e, em seguida, clique em **próxima**.  
  
5.  Quando você for solicitado, especifique as credenciais que são necessárias para estabelecer uma relação de confiança entre esse proxy do servidor de Federação e o serviço de Federação.  
  
    Por padrão, apenas a conta de serviço usada pelo serviço de Federação ou um membro do grupo local BUILTIN\\Administrators pode autorizar um proxy de servidor de Federação.  
  
6.  Sobre o **pronto para aplicar configurações** página, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar este computador com essas configurações de proxy.  
  
7.  Sobre o **resultados de configuração** página, examinar os resultados. Quando terminarem de todas as etapas de configuração, clique em **fechar** para sair do assistente.  
  
    Não há nenhum \(MMC\) snap\ Console de gerenciamento Microsoft-na ser usado para administrar proxys de servidor de Federação. Para definir as configurações para cada um do proxys de servidor de Federação em sua organização, use cmdlets do Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurando uma porta TCP\/IP alternativa para operações de Proxy  
Por padrão, o serviço de proxy do servidor de Federação está configurado para usar a porta TCP 443 para tráfego HTTPS e a porta 80 para o tráfego HTTP para comunicação com o servidor de Federação. Para configurar as portas diferentes, como a porta TCP 444 para HTTPS e porta 81 para HTTP, as seguintes tarefas devem ser concluídas.  
  
> [!NOTE]  
> Se você pretende implantar inicialmente AD FS para operar em portas TCP\/IP alternativas, primeiro você deve modificar portas em suas associações de protocolo IIS para HTTP e HTTPS no servidor de Federação e computadores de proxy do servidor de Federação. Isso deve ocorrer antes de executar os assistentes de configuração do AD FS para configuração inicial. Se você definir o Internet Information Services \(IIS\) pela primeira vez, as configurações de porta TCP\/IP alternativas são descobertas quando a configuração com base no assistente ocorre dentro do AD FS, e o procedimento a seguir não é necessário. Se você quiser alterar as configurações de porta posteriormente, atualize as associações de protocolo IIS primeiro e, em seguida, use o procedimento a seguir para atualizar as configurações de porta adequadamente. Para obter mais informações sobre como editar ligações IIS, consulte [149605 do artigo](https://go.microsoft.com/fwlink/?LinkId=190275) na Base de Conhecimento Microsoft.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar alternativas portas TCP\/IP para o proxy do servidor de Federação usar  
  
1.  Configure o servidor de federação para usar as portas não padrão.  
  
    Para fazer isso, especifique o número da porta não padrão, incluindo-o com o *HttpsPort* e *HttpPort* opções como parte do **Set \ ADFSProperties** cmdlet. Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configure o proxy de servidor de federação para usar a porta não predefinida.  
  
    Para fazer isso, especifique o número da porta não padrão, incluindo-o com o *HttpsPort* e *HttpPort* opções como parte do **Set \ ADFSProxyProperties** cmdlet. Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URLs de ponto de extremidade não estão habilitados por padrão para o serviço de proxy do servidor de Federação. Se você estiver configurando uma nova instalação do servidor de federação, você deve habilitar a pontos de extremidade serviço proxy de servidor de Federação pela primeira vez. Por exemplo, presume-se que para todos os pontos de extremidade que o exemplo neste procedimento refere-se a você ativou-los para proxy selecionando-los no AD FS gerenciamento snap\-in e, em seguida, selecionando **Ativar proxy**.  
  
3.  Atualize a instalação do IIS do proxy do servidor de federação para que a linguagem de marcação de asserção de segurança \(SAML\) e pontos de extremidade de confiança WS\ estão configurados para refletir o número da porta atualizado. Para fazer isso, você pode usar o bloco de notas para modificar o seguinte no arquivo Web. config, que está localizado em systemdrive%\\inetpub\\adfs\\ls\\ no computador de proxy do servidor de Federação. Por exemplo, supondo que você tem um servidor de Federação chamado sts1.contoso.com e o novo número da porta é 444, procure e abra o arquivo de Web. config no bloco de notas no computador de proxy do servidor de federação, localize a seção a seguir, modificar o número da porta conforme realçado abaixo e, em seguida, salvar e sair do bloco de notas.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Adicionar a conta de usuário do serviço de proxy do servidor de federação para o controle de acesso listar \(ACL\) para as URLs de ponto de extremidade relacionados. Por exemplo, se o número da porta é 1234 e a conta de usuário que é usada para executar o serviço de proxy do servidor AD FSfederation em é a conta de serviço de rede built\-in, digite o seguinte comando em um prompt de comando:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Os comandos anteriores devem ser executados no servidor de Federação e os computadores de proxy do servidor de Federação.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

