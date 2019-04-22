---
title: Implantar Acesso Remoto com autenticação OTP
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811987"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Implantar Acesso Remoto com autenticação OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinam o DirectAccess e o roteamento e serviço de acesso remoto \(RRAS\) VPN em uma única função de acesso remoto.   

## <a name="BKMK_OVER"></a>Descrição do cenário  
Nesse cenário, um acesso remoto servidor com o DirectAccess habilitado está configurado para autenticar os usuários de cliente do DirectAccess com dois\-senha de uso único fator \(OTP\) autenticação, além do padrão ativo Credenciais de diretório.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  
  
-   [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deve ser implantado antes de implantar a OTP.  
  
-   Os clientes do Windows 7 deve usar DCA 2.0 para dar suporte a OTP.  
  
-   OTP não oferece suporte à alteração de PIN.  
  
-   Uma infraestrutura de chave pública deve ser implantada.  
  
    Para saber mais, confira: [Minimódulo do guia de laboratório de teste: PKI básico para Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou os cmdlets do Windows PowerShell.  
  
## <a name="in-this-scenario"></a>Neste cenário  
O cenário de autenticação OTP inclui várias etapas:  
  
1.  [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único servidor de acesso remoto deve ser implantado antes de configurar a OTP. O planejamento e a implantação de um servidor único incluem projeto e configuração de uma topologia de rede, planejamento e implantação de certificados, configuração de DNS e Active Directory, configuração das definições do servidor de Acesso Remoto, implantação de clientes do DirectAccess e preparação de servidores da intranet.  
  
2.  [Planejar o acesso remoto com autenticação OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Além do planejamento necessário para um único servidor, a OTP exige o planejamento de uma autoridade de certificação da Microsoft \(autoridade de certificação\) e modelos de certificado da OTP; e um RAIO\-servidor OTP habilitado. Planejamento pode também incluir um requisito para grupos de segurança isentar usuários específicos a strong \(OTP ou cartão inteligente\) autenticação. Para obter informações sobre a configuração de OTP em uma de várias\-ambiente de floresta, consulte [configurar uma implantação de várias florestas](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurar o DirectAccess com autenticação OTP](/configure/Configure-RA-with-OTP-Authentication.md). Implantação da OTP consiste em um número de etapas de configuração, incluindo a preparação da infra-estrutura de autenticação de OTP, configuração do servidor OTP, definir as configurações de OTP no servidor de acesso remoto e atualização das configurações de cliente do DirectAccess.  
  
4.  [Troubleshoot an OTP Deployment] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Esta seção solução de problemas descreve vários dos erros mais comuns que podem ocorrer ao implantar o acesso remoto com autenticação OTP.  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Aumente a segurança usando OTP aumenta a segurança da implantação do DirectAccess. Um usuário exige as credenciais OTP para obter acesso à rede interna. Um usuário fornece credenciais OTP por meio das conexões de área de trabalho disponíveis em conexões de rede no computador cliente Windows 10 ou Windows 8, ou usando o Assistente de conectividade do DirectAccess \(DCA\) nos computadores cliente em execução Windows 7. O processo de autenticação OTP funciona da seguinte maneira:  
  
1.  O cliente DirectAccess insere as credenciais de domínio para acessar os servidores de infraestrutura do DirectAccess \(por meio do túnel de infraestrutura\).  Se nenhuma conexão com a rede interna estiver disponível, devido a uma falha de IKE específica, a Conexão do Local de Trabalho no computador cliente notifica o usuário informando que as credenciais são necessárias. Em computadores cliente que executam o Windows 7, um pop\-solicitar credenciais de cartão inteligente é exibida.  
  
2.  Depois que as credenciais OTP são inseridas, elas são enviadas por SSL para o servidor de acesso remoto, juntamente com uma solicitação por um breve\-certificado de logon de cartão inteligente do termo.  
  
3.  O servidor de acesso remoto inicia a validação das credenciais OTP com o RADIUS\-com base em servidor OTP.  
  
4.  Se for bem-sucedido, o servidor de Acesso Remoto faz a solicitação do certificado usando seu certificado de autoridade de registro e envia de volta para o computador cliente DirectAccess  
  
5.  O computador cliente DirectAccess encaminha a solicitação de certificado à autoridade de certificação e armazena o certificado registrado para uso pelo Kerberos SSP\/AP.  
  
6.  Usando este certificado, o computador cliente executa de forma transparente a autenticação Kerberos de cartão inteligente padrão.  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos necessários para o cenário:  
  
|Função\/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|*Função de gerenciamento de acesso remoto*|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e o roteamento e os serviços de acesso remoto que era anteriormente um serviço de função sob a política de rede e serviços de acesso \(NPAS\) server função. A função Acesso Remoto consiste em dois componentes:<br /><br />1.  DirectAccess e o roteamento e serviços de acesso remoto \(RRAS\) VPN do DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />2.  Recursos de roteamento de RRAS-roteamento de RRAS são gerenciados no console de roteamento e acesso remoto legado.<br /><br />A função de Acesso Remoto é dependente dos seguintes recursos de servidor:<br /><br />-Serviços de informações da Internet \(IIS\) servidor Web - esse recurso é necessário para configurar o servidor de local de rede, utilizar autenticação OTP e configurar a investigação da web padrão.<br />-Windows Database-Used interno para contabilidade local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />– Ele é instalado por padrão em um servidor de acesso remoto quando a função acesso remoto está instalada e oferece suporte a interface de usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não executa a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-Ferramentas de linha de comando e GUI de acesso remoto<br />-Módulo de acesso remoto para o Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de gerenciamento de diretiva de grupo<br />-Kit de administração do Gerenciador de Conexão de RAS \(CMAK\)<br />-   Windows PowerShell 3.0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
-   Um computador que atenda aos requisitos de hardware do Windows Server 2016 ou Windows Server 2012.  
  
-   Para testar o cenário, é necessário pelo menos um computador executando o Windows 10, Windows 8 ou 7 do Windows configurado como um cliente DirectAccess.  
  
-   Um servidor OTP que oferece suporte a PAP sobre RADIUS.  
  
-   Um token de hardware ou software de OTP.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  
  
1.  Requisitos de software para implantação de servidor único. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Além dos requisitos de software para um único servidor, há uma série de OTP\-requisitos específicos:  
  
    1.  Autoridade de certificação para o IPsec na autenticação de uma implantação OTP que DirectAccess deve ser implantado usando o IPsec máquinas certificados emitidos por uma autoridade de certificação. A autenticação IPsec usando o servidor de Acesso Remoto como um proxy Kerberos não é aceita em uma implantação OTP. Uma CA interna é necessária.  
  
    2.  Autoridade de certificação para A autenticação de OTP Microsoft autoridade de certificação corporativa \(executado no Windows 2003 Server ou posterior\) é necessária para emitir o certificado de cliente OTP. A mesma autoridade de certificação usada para emitir certificados para a autenticação IPsec pode ser usada. Os servidores de AC devem ser acessíveis no primeiro túnel de infraestrutura.  
  
    3.  Um grupo de segurança do Active Directory que contém esses usuários de segurança ao grupo usuários isentos da autenticação forte, é necessário.  
  
    4.  Cliente\-requisitos de lado-para Windows 10 e Windows 8 computadores cliente, o Assistente de conectividade de rede \(NCA\) serviço é usado para detectar se as credenciais OTP são necessárias. Se elas forem, o Gerenciador de mídia do DirectAccess solicitará credenciais.  O NCA está incluído no sistema operacional, e nenhuma instalação nem implantação é necessária. Para computadores do cliente do Windows 7, o Assistente de conectividade do DirectAccess \(DCA\) 2.0 é necessário. Ele está disponível como um download no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Observe o seguinte:  
  
        1.  Autenticação de OTP pode ser usada em paralelo com o cartão inteligente e o Trusted Platform Module \(TPM\)\-com base em autenticação. A habilitação de autenticação OTP no console de Gerenciamento de Acesso Remoto também permite o uso da autenticação de cartão inteligente.  
  
        2.  Durante os usuários de configuração de acesso remoto em uma segurança especificada grupo pode ser isento de duas\-autenticação de fatores e, portanto, autenticar com o nome de usuário\/somente de senha.  
  
        3.  Não há suporte para os modos de novo PIN e de próximo código de token  
  
        4.  Em uma implantação de vários sites de Acesso Remoto, as configurações OTP são globais e identificam todos os pontos de entrada. Se vários servidores RADIUS ou CA são configurados para a OTP, eles são classificados por cada servidor de Acesso Remoto de acordo com a disponibilidade e a proximidade.  
  
        5.  Ao configurar a OTP em um múltiplo de acesso remoto\-ambiente de floresta, CAs da OTP devem ser da floresta de recursos somente e registro de certificado deve ser configurado entre relações de confiança de floresta. Para obter mais informações, consulte [AD CS: Registro de certificado entre florestas com o Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Os usuários que estão usando um token KEY FOB OTP devem inserir o PIN seguido do código de token \(sem nenhum separador\) na caixa de diálogo OTP do DirectAccess. Os usuários que estão usando o token PIN PAD OTP devem inserir somente o código de token na caixa de diálogo.  
  
        7.  Quando o WEBDAV está habilitado, a OTP não deve ser habilitada.  
  
## <a name="KnownIssues"></a>Problemas conhecidos  
Os problemas a seguir são conhecidos quando se configura um cenário de OTP:  
  
-   Acesso remoto usa um mecanismo de teste para verificar a conectividade com o RADIUS\-com base em servidores OTP. Em alguns casos, isso pode causar um erro a ser emitido no servidor de OTP. Para evitar esse problema, faça o seguinte no servidor OTP:  
  
    -   Crie uma conta de usuário que corresponda ao nome de usuário e senha configurados no servidor de Acesso Remoto para o mecanismo de teste. O nome de usuário não deve definir um usuário do Active Directory.  
  
        Por padrão, o nome de usuário no servidor de Acesso Remoto é DAProbeUser e a senha é DAProbePass. Essas configurações padrão podem ser modificadas usando os seguintes valores de Registro no servidor de Acesso Remoto:  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Se você alterar o certificado raiz do IPsec em uma implantação configurada e executando o DirectAccess, o OTP para de funcionar. Para resolver esse problema, em cada servidor do DirectAccess, em um prompt do Windows PowerShell, execute o comando: `iisreset`  
  
