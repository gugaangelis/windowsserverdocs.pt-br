---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurar um computador para a função de proxy do servidor de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b01e2ae567155cd3d53d6d7972bfd0b9ec0cf51b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192285"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar um computador para a função de proxy do servidor de federação

Depois de configurar um computador com os certificados necessários e tiver instalado o serviço de função Proxy do serviço de federação, você está pronto para configurar o computador para se tornar um proxy do servidor de Federação. Use o procedimento a seguir para que o computador atue na função proxy de servidor de federação.  
  
> [!IMPORTANT]  
> Antes de usar este procedimento para configurar o computador de proxy do servidor de federação, certifique-se de que você tiver seguido todas as etapas [lista de verificação: Configuração de backup a Federation Server Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md) na ordem em que estão listados. Verifique se pelo menos um servidor de federação foi implantado e se todas as credenciais necessárias para autorização de uma configuração proxy de servidor de federação foram implementadas. Você também deve configurar o Secure Sockets Layer \(SSL\) associações no Site da Web padrão ou esse assistente não serão iniciado. Todas essas tarefas devem ser concluídas antes do funcionamento desse servidor de federação.  
  
Depois que você terminar de configurar o computador, verifique se o proxy de servidor de federação está funcionando como esperado. Para mais informações, consulte [Verificar se um Proxy do Servidor de Federação está operacional](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Para configurar um computador para a função de proxy do servidor de federação  
  
1.  Há duas maneiras de iniciar o Assistente de configuração do servidor de Federação do AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Sobre o **inicie** tela, digite**Assistente Configuração do Proxy de servidor de Federação do AD FS**, e pressione ENTER.  
  
    -   A qualquer momento depois que o Assistente de instalação for concluída, abra o Windows Explorer, navegue até a **c:\\Windows\\ADFS** pasta e, em seguida, double\-clique **FspConfigWizard.exe**.  
  
2.  Usando qualquer método, inicie o assistente e, na página **Bem-vindo**, clique em **Avançar**.  
  
3.  Na página **Especificar Nome do Serviço de Federação** em **Nome do Serviço de Federação**, digite o nome que representa o Serviço de Federação para o qual este computador agirá na função de proxy.  
  
4.  Com base em seus requisitos de rede específicos, determine se você precisará usar um servidor proxy HTTP para encaminhar solicitações ao Serviço de Federação. Em caso positivo, marque a caixa de seleção **Usar um servidor proxy HTTP ao enviar solicitações a este Serviço de Federação**, em **Endereço do servidor proxy HTTP** digite o endereço do servidor proxy, clique em **Testar Conexão** para verificar a conectividade e clique em **Avançar**.  
  
5.  Quando for solicitado, especifique as credenciais necessárias para estabelecer uma relação de confiança entre o proxy do servidor de federação e o Serviço de Federação.  
  
    Por padrão, somente a conta de serviço usada pelo serviço de Federação ou um membro de um local interno\\grupo Administradores pode autorizar um proxy do servidor de Federação.  
  
6.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **Avançar** para começar a configurar este computador com as configurações de proxy.  
  
7.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração estiverem concluídas, clique em **fechar** para sair do assistente.  
  
    Não há nenhum Console de gerenciamento Microsoft \(MMC\) encaixar\-para usar para administrar os proxys do servidor de Federação. Para definir configurações para cada um dos proxys do servidor de federação na sua organização, use cmdlets do Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurando uma alternativa TCP\/porta IP para operações de Proxy  
Por padrão, o serviço de proxy do servidor de Federação está configurado para usar a porta TCP 443 para tráfego HTTPS e a porta 80 para tráfego HTTP para comunicação com o servidor de Federação. Para configurar portas diferentes, como a porta TCP 444 para HTTPS e a porta 81 para HTTP, as seguintes tarefas devem ser concluídas.  
  
> [!NOTE]  
> Se você pretende implantar inicialmente o AD FS para operar TCP alternativo como\/portas IP, primeiro você deve modificar as portas em suas associações de protocolo do IIS para HTTP e HTTPS no servidor de Federação e computadores de proxy do servidor de Federação. Isso deve ocorrer antes de executar os assistentes de configuração do AD FS para a configuração inicial. Se você configurar os serviços de informações da Internet \(IIS\) primeiro, o TCP alternativa\/configurações de porta IP descobertas quando assistente\-configuração com base ocorre dentro do AD FS, e o procedimento a seguir não é necessário. Para alterar as configurações de porta depois, atualize as associações do protocolo IIS primeiro e, depois, use o seguinte procedimento para atualizar as configurações da porta de forma adequada. Para obter mais informações sobre como editar as associações do IIS, consulte [artigo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) na Base de dados de Conhecimento Microsoft.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar o TCP alternativo\/portas IP para o proxy do servidor de Federação usar  
  
1.  Configure o servidor de federação para usar as portas não padrão.  
  
    Para fazer isso, especifique o número da porta não padrão incluindo-o com o *HttpsPort* e *HttpPort* opções como parte do **definir\-ADFSProperties** cmdlet. Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configure o proxy do servidor de federação para usar a porta não padrão.  
  
    Para fazer isso, especifique o número da porta não padrão incluindo-o com o *HttpsPort* e *HttpPort* opções como parte do **definir\-ADFSProxyProperties** cmdlet. Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URLs de ponto de extremidade não estão habilitadas por padrão para o serviço de proxy do servidor de Federação. Se você estiver configurando uma nova instalação do servidor de federação, você deve habilitar pontos de extremidade serviço proxy de servidor de Federação pela primeira vez. Por exemplo, supõe-se que para todos os pontos de extremidade que o exemplo deste procedimento se refere ao foram habilitados para proxy selecionando-os no snap do gerenciamento do AD FS\-em e, em seguida, selecionando **habilitar no proxy**.  
  
3.  Atualizar a instalação do IIS no proxy de servidor de Federação até que Security Assertion Markup Language \(SAML\) e WS\-confiar em pontos de extremidade são configurados para refletir o número de porta atualizado. Para fazer isso, você pode usar o bloco de notas para modificar o seguinte no arquivo Web. config, que está localizado em % systemdrive\\inetpub\\adfs\\ls\\ no computador de proxy do servidor de Federação. Por exemplo, supondo que você tiver um servidor de Federação chamado sts1.contoso.com e o novo número da porta seja 444, navegue até e abra o arquivo Web. config no bloco de notas no computador de proxy do servidor de federação, localize a seção a seguir, modifique o número da porta como realçada abaixo e, em seguida, salvar e sair do bloco de notas.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Adicione a conta de usuário do serviço de proxy do servidor de federação para a lista de controle de acesso \(ACL\) para as URLs de ponto de extremidade relacionado. Por exemplo, se o número da porta for 1234 e a conta de usuário que é usada para executar o AD FSfederation proxy do servidor de serviço sob é interna\-na conta de serviço de rede, digite o seguinte comando no prompt de comando:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Os comandos anteriores devem ser executados no servidor de Federação e os computadores de proxy do servidor de Federação.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

