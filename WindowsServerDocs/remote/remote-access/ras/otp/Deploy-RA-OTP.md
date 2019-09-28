---
title: Implantar Acesso Remoto com autenticação OTP
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0de5f459e31e1dfac40e49cd6cc83de8722df4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404431"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Implantar Acesso Remoto com autenticação OTP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

 O Windows Server 2016 e o Windows Server 2012 combinam o DirectAccess e o serviço de roteamento e acesso remoto \(RRAS @ no__t-1 VPN em uma única função de acesso remoto.   

## <a name="BKMK_OVER"></a>Descrição do cenário  
Nesse cenário, um servidor de acesso remoto com o DirectAccess habilitado é configurado para autenticar os usuários do cliente do DirectAccess com duas no__t de @no__t senha de uso único do 0factor-1OTP @ no__t-2, além das credenciais de Active Directory padrão.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  
  
-   [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deve ser implantado antes de implantar a OTP.  
  
-   Os clientes do Windows 7 devem usar o DCA 2,0 para dar suporte à OTP.  
  
-   OTP não oferece suporte à alteração de PIN.  
  
-   Uma infraestrutura de chave pública deve ser implantada.  
  
    Para saber mais, confira: Mini-módulo do guia do laboratório de [Test: PKI básica para Windows Server 2012. ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   Não há suporte para a alteração de políticas fora do console de gerenciamento do DirectAccess ou de cmdlets do Windows PowerShell.  
  
## <a name="in-this-scenario"></a>Neste cenário  
O cenário de autenticação OTP inclui várias etapas:  
  
1.  [Implante um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único servidor de acesso remoto deve ser implantado antes de configurar A OTP. O planejamento e a implantação de um servidor único incluem projeto e configuração de uma topologia de rede, planejamento e implantação de certificados, configuração de DNS e Active Directory, configuração das definições do servidor de Acesso Remoto, implantação de clientes do DirectAccess e preparação de servidores da intranet.  
  
2.  [Planeje o acesso remoto com autenticação OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Além do planejamento necessário para um único servidor, a OTP requer o planejamento de uma autoridade de certificação da Microsoft \(CA @ no__t-1 e modelos de certificado para OTP; e um servidor de OTP RADIUS @ no__t-2enabled. O planejamento também pode incluir um requisito de grupos de segurança para isentar usuários específicos de autenticação forte de \(OTP ou cartão inteligente @ no__t-1. Para obter informações sobre a configuração de OTP em um ambiente multi @ no__t-0forest, consulte [Configurar uma implantação de várias florestas](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configure o DirectAccess com a autenticação OTP](/configure/Configure-RA-with-OTP-Authentication.md). A implantação de OTP consiste em várias etapas de configuração, incluindo a preparação da infraestrutura para autenticação OTP, a configuração do servidor de OTP, a definição de configurações de OTP no servidor de acesso remoto e a atualização das configurações do cliente do DirectAccess.  
  
4.  [Solucionar problemas de implantação de OTP] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Esta seção de solução de problemas descreve uma série de erros mais comuns que podem ocorrer ao implantar o acesso remoto com autenticação OTP.  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Aumentar a segurança-usar a OTP aumenta a segurança de sua implantação do DirectAccess. Um usuário exige as credenciais OTP para obter acesso à rede interna. Um usuário fornece credenciais de OTP por meio das conexões de local de trabalho disponíveis nas conexões de rede no computador cliente Windows 10 ou Windows 8 ou usando o assistente de conectividade do DirectAccess \(DCA @ no__t-1 em computadores cliente que executam o Windows 7. O processo de autenticação OTP funciona da seguinte maneira:  
  
1.  O cliente do DirectAccess insere as credenciais de domínio para acessar os servidores de infraestrutura do DirectAccess \(over o túnel de infraestrutura @ no__t-1.  Se nenhuma conexão com a rede interna estiver disponível, devido a uma falha de IKE específica, a Conexão do Local de Trabalho no computador cliente notifica o usuário informando que as credenciais são necessárias. Em computadores cliente que executam o Windows 7, um pop @ no__t-0up solicitando credenciais de cartão inteligente é exibido.  
  
2.  Depois que as credenciais de OTP forem inseridas, elas serão enviadas por SSL para o servidor de acesso remoto, junto com uma solicitação para um certificado de logon de cartão inteligente de @ no__t-0term curto.  
  
3.  O servidor de acesso remoto inicia a validação das credenciais de OTP com o servidor de OTP RADIUS @ no__t-0based.  
  
4.  Se for bem-sucedido, o servidor de Acesso Remoto faz a solicitação do certificado usando seu certificado de autoridade de registro e envia de volta para o computador cliente DirectAccess  
  
5.  O computador cliente do DirectAccess encaminha a solicitação de certificado assinado para a AC e armazena o certificado registrado para uso pelo SSP do Kerberos @ no__t-0AP.  
  
6.  Usando este certificado, o computador cliente executa de forma transparente a autenticação Kerberos de cartão inteligente padrão.  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos necessários para o cenário:  
  
|Função @ no__t-0feature|Como este cenário tem suporte|  
|---------|-----------------|  
|*Função de gerenciamento de acesso remoto*|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função abrange ambos o DirectAccess, que anteriormente era um recurso no Windows Server 2008 R2, e serviços de roteamento e acesso remoto que anteriormente era um serviço de função na função de servidor e serviços de acesso \(NPAS @ no__t-1. A função Acesso Remoto consiste em dois componentes:<br /><br />1.  DirectAccess e serviços de roteamento e acesso remoto \(RRAS @ no__t-1 VPN-DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />2.  Roteamento RRAS-os recursos de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<br /><br />A função de Acesso Remoto é dependente dos seguintes recursos de servidor:<br /><br />-Serviços de Informações da Internet \(IIS @ no__t-1 servidor Web-esse recurso é necessário para configurar o servidor de local de rede, utilizar a autenticação OTP e configurar a investigação da Web padrão.<br />-Banco de dados interno do Windows-usado para contabilização local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-GUI de acesso remoto e ferramentas de linha de comando<br />-Módulo de acesso remoto para Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS \(CMAK @ no__t-1<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
-   Um computador que atenda aos requisitos de hardware para o Windows Server 2016 ou o Windows Server 2012.  
  
-   Para testar o cenário, pelo menos um computador com Windows 10, Windows 8 ou Windows 7 configurado como um cliente do DirectAccess é necessário.  
  
-   Um servidor OTP que oferece suporte a PAP sobre RADIUS.  
  
-   Um token de hardware ou software de OTP.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  
  
1.  Requisitos de software para implantação de servidor único. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Além dos requisitos de software para um único servidor, há vários requisitos de OTP @ no__t-0specific:  
  
    1.  AC para autenticação IPsec-em uma implantação de OTP, o DirectAccess deve ser implantado usando certificados de computadores IPsec emitidos por uma autoridade de certificação. A autenticação IPsec usando o servidor de Acesso Remoto como um proxy Kerberos não é aceita em uma implantação OTP. Uma CA interna é necessária.  
  
    2.  AC para autenticação de OTP-um Microsoft Enterprise CA \(running no Windows 2003 Server ou posterior @ no__t-1 é necessário para emitir o certificado de cliente de OTP. A mesma autoridade de certificação usada para emitir certificados para a autenticação IPsec pode ser usada. Os servidores de AC devem ser acessíveis no primeiro túnel de infraestrutura.  
  
    3.  Grupo de segurança – para isentar usuários da autenticação forte, um grupo de segurança Active Directory que contém esses usuários é necessário.  
  
    4.  Requisitos de cliente @ no__t-0side-para computadores cliente com Windows 10 e Windows 8, o serviço Network Connectivity Assistant \(NCA @ no__t-2 é usado para detectar se as credenciais de OTP são necessárias. Se estiverem, o Gerenciador de mídia do DirectAccess solicitará as credenciais.  O NCA está incluído no sistema operacional e nenhuma instalação ou implantação é necessária. Para computadores cliente do Windows 7, o assistente de conectividade do DirectAccess \(DCA @ no__t-1 2,0 é necessário. Ele está disponível como um download no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Observe o seguinte:  
  
        1.  A autenticação OTP pode ser usada em paralelo com o cartão inteligente e Trusted Platform Module \(TPM @ no__t-1 @ no__t-2based Authentication. A habilitação de autenticação OTP no console de Gerenciamento de Acesso Remoto também permite o uso da autenticação de cartão inteligente.  
  
        2.  Durante a configuração de acesso remoto, os usuários em um grupo de segurança especificado podem ser isentos de duas autenticações @ no__t-0factor e, portanto, autenticar somente com o nome de usuário @ no__t-1Password.  
  
        3.  Não há suporte para os modos de novo PIN e de próximo código de token  
  
        4.  Em uma implantação de vários sites de Acesso Remoto, as configurações OTP são globais e identificam todos os pontos de entrada. Se vários servidores RADIUS ou CA são configurados para a OTP, eles são classificados por cada servidor de Acesso Remoto de acordo com a disponibilidade e a proximidade.  
  
        5.  Ao configurar a OTP em um ambiente de acesso remoto multi @ no__t-0forest, as autoridades de certificação OTP devem ser apenas da floresta de recursos e o registro de certificado deve ser configurado entre relações de confiança de floresta. Para obter mais informações, consulte [AD CS: Registro de certificado entre florestas com o Windows Server 2008 R2 @ no__t-0.  
  
        6.  Os usuários que estão usando um token de OTP de chave FOB devem inserir o PIN seguido pelo tokencode \(without quaisquer separadores @ no__t-1 na caixa de diálogo de OTP do DirectAccess. Os usuários que estão usando o token PIN PAD OTP devem inserir somente o código de token na caixa de diálogo.  
  
        7.  Quando o WEBDAV está habilitado, a OTP não deve ser habilitada.  
  
## <a name="KnownIssues"></a>Problemas conhecidos  
Os problemas a seguir são conhecidos quando se configura um cenário de OTP:  
  
-   O acesso remoto usa um mecanismo de investigação para verificar a conectividade com os servidores de OTP do RADIUS @ no__t-0based. Em alguns casos, isso pode causar um erro a ser emitido no servidor de OTP. Para evitar esse problema, faça o seguinte no servidor OTP:  
  
    -   Crie uma conta de usuário que corresponda ao nome de usuário e senha configurados no servidor de Acesso Remoto para o mecanismo de teste. O nome de usuário não deve definir um usuário do Active Directory.  
  
        Por padrão, o nome de usuário no servidor de Acesso Remoto é DAProbeUser e a senha é DAProbePass. Essas configurações padrão podem ser modificadas usando os seguintes valores de Registro no servidor de Acesso Remoto:  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6RadiusProbeUser  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6 RadiusProbePass  
  
-   Se você alterar o certificado raiz do IPsec em uma implantação configurada e executando o DirectAccess, o OTP para de funcionar. Para resolver esse problema, em cada servidor DirectAccess, em um prompt do Windows PowerShell, execute o comando: `iisreset`  
  
