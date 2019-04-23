---
title: Processos de credenciais na autenticação do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 52d50a1bb6bfbe9d35146f362a2a5184df0a6504
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839857"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processos de credenciais na autenticação do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para profissionais de TI descreve como a autenticação do Windows processa as credenciais.

Gerenciamento de credenciais do Windows é o processo pelo qual o sistema operacional recebe as credenciais do usuário ou serviço e protege essas informações para apresentação futuras para o destino de autenticação. No caso de um computador ingressado no domínio, o destino de autenticação é o controlador de domínio. As credenciais usadas na autenticação são documentos digitais que associam a identidade do usuário em alguma forma de prova de autenticidade, como um certificado, uma senha ou um PIN.

Por padrão, as credenciais do Windows são validadas em relação o banco de dados do Gerenciador de contas de segurança (SAM) no computador local ou no Active Directory em um computador ingressado no domínio, por meio do serviço Winlogon. As credenciais são coletadas por meio de entrada do usuário na interface do usuário de logon ou programaticamente por meio da interface de programação de aplicativo (API) para ser apresentado para o destino de autenticação.

Informações de segurança local são armazenadas no registro em **HKEY_LOCAL_MACHINE\SECURITY**. Informações armazenadas incluem configurações de política, os valores padrão de segurança e informações de conta, como credenciais de logon armazenado em cache. Uma cópia do banco de dados SAM também é armazenada aqui, embora seja protegido contra gravação.

O diagrama a seguir mostra os componentes que são necessários e os caminhos que credenciais levar através do sistema para autenticar o usuário ou processo para um logon bem-sucedido.

![Diagrama que mostra os componentes que são necessários e os caminhos que credenciais levar através do sistema para autenticar o usuário ou processo para um logon bem-sucedido.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

A tabela a seguir descreve cada componente que gerencia as credenciais no processo de autenticação no ponto de logon.

**Componentes de autenticação para todos os sistemas**

|Componente|Descrição|
|-------|--------|
|Logon do usuário|Winlogon.exe é o responsável por gerenciar interações do usuário segura do arquivo executável. O serviço Winlogon inicia o processo de logon para os sistemas operacionais Windows, passando as credenciais coletadas por ação do usuário na área de trabalho protegida (interface de usuário de Logon) para autoridade de segurança Local (LSA) por meio de Secur32.dll.|
|Logon do aplicativo|Aplicativo ou serviço logons que não exigem logon interativo. A maioria dos processos iniciada pelo usuário são executados no modo de usuário usando Secur32.dll, enquanto os processos iniciados na inicialização, como os serviços, executado no modo kernel usando Ksecdd. sys.<br /><br />Para obter mais informações sobre o modo de usuário e o modo de kernel, consulte aplicativos e o modo de usuário ou serviços e o modo de Kernel neste tópico.|
|Secur32.dll|Vários provedores de autenticação que formam a base do processo de autenticação.|
|Lsasrv.dll|O serviço servidor de LSA, que impõe políticas de segurança e atua como o Gerenciador de pacotes de segurança para a LSA. A LSA contém a função Negotiate, que seleciona o protocolo NTLM ou Kerberos depois de determinar qual protocolo deve ser bem-sucedida.|
|Provedores de suporte de segurança|Um conjunto de provedores que individualmente pode invocar um ou mais protocolos de autenticação. O conjunto de provedores padrão pode ser alterado a cada versão do sistema operacional Windows e provedores personalizados podem ser gravados.|
|Netlogon.dll|Os serviços que executa o serviço de Logon de rede são da seguinte maneira:<br /><br />-Mantém o canal seguro do computador (não deve ser confundido com Schannel) para um controlador de domínio.<br />-Passa as credenciais do usuário por meio de um canal seguro para o controlador de domínio e retorna os identificadores de segurança (SIDs) do domínio e direitos de usuário para o usuário.<br />-Publica registros de recursos de serviço no sistema de nome de domínio (DNS) e usa o DNS para resolver nomes para os endereços IP (Internet Protocol) dos controladores de domínio.<br />-Implementa o protocolo de replicação com base na chamada de procedimento remoto (RPC) para a sincronização de controladores de domínio primário (PDCs) e controladores de domínio de backup (BDCs).|
|Samsrv.dll|O Gerenciador de segurança contas (SAM), que armazena as contas de segurança local, impõe políticas armazenadas localmente e dá suporte a APIs.|
|Registro|O registro contém uma cópia do banco de dados SAM, configurações de política de segurança local, os valores padrão de segurança e informações de conta que só estão acessíveis no sistema.|

Esse tópico contém as seguintes seções:

-   [Entrada de credencial para logon do usuário](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credencial para logon de serviço e aplicativo](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridade de segurança local](#BKMK_LSA)

-   [Validação e credenciais armazenadas em cache](#BKMK_CachedCredentialsAndValidation)

-   [Armazenamento de credenciais e validação](#BKMK_CredentialStorageAndValidation)

-   [Banco de dados de Gerenciador de contas de segurança](#BKMK_SAM)

-   [Domínios locais e domínios confiáveis](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados de autenticação do Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credencial para logon do usuário
No Windows Server 2008 e Windows Vista, a arquitetura de autenticação (GINA) Graphical Identification and foi substituída por um modelo de provedor de credenciais, que agora é possível enumerar os tipos de logon diferentes com o uso de blocos de logon. Ambos os modelos são descritos abaixo.

**Arquitetura de identificação e autenticação gráfica**

A arquitetura de autenticação (GINA) Graphical Identification and se aplica aos sistemas operacionais Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. Nesses sistemas, cada sessão de logon interativo cria uma instância separada do serviço Winlogon. A arquitetura da GINA é carregada no espaço de processo usado pelo Winlogon, recebe e processa as credenciais e faz as chamadas para as interfaces de autenticação por meio de LSALogonUser.

As instâncias do Winlogon para um logon interativo são executados na sessão 0. Serviços do sistema de hosts de sessão 0 e outros processos críticos, incluindo o processo de autoridade de segurança Local (LSA).

O diagrama a seguir mostra o processo de credencial para o Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional.

![Diagrama mostrando o processo de credencial para o Microsoft Windows 2000 Professional, Microsoft Windows 2000 Server, Windows XP e Windows Server 2003](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitetura do provedor de credenciais**

A arquitetura do provedor de credenciais se aplica a essas versões designadas na **aplica-se a** no início deste tópico. Nesses sistemas, a arquitetura de entrada de credenciais será alterado para um design extensível por meio de provedores de credenciais. Esses provedores são representados por blocos diferentes de logon na área de trabalho segura que permitem a qualquer número de cenários de logon - contas diferentes para o mesmo usuário e os diferentes métodos de autenticação, como senha, cartão inteligente e biometria.

Com a arquitetura de provedor de credenciais, o Winlogon inicia sempre da interface do usuário de Logon depois de receber um evento de sequência segura. Interface do usuário de logon consulta cada provedor de credenciais para o número de diferentes tipos de credencial que do provedor está configurado para enumerar. Provedores de credenciais tem a opção de especificar um desses blocos, como o padrão. Depois que todos os provedores têm enumerada seus blocos, interface do usuário de Logon exibe-os para o usuário. O usuário interage com um bloco para fornecer suas credenciais. Interface do usuário de logon envia essas credenciais para autenticação.

Provedores de credenciais não são mecanismos de imposição. Eles são usados para coletar e serializar as credenciais. Os pacotes de autenticação e a autoridade de segurança Local impõem a segurança.

Provedores de credenciais são registrados no computador e são responsáveis pelo seguinte:

-   Descrever as informações de credenciais necessárias para a autenticação.

-   Tratamento de comunicação e a lógica com autoridades de autenticação externa.

-   As credenciais de empacotamento para interativo e logon de rede.

As credenciais de empacotamento para interativo e de logon de rede inclui o processo de serialização. Serializando credenciais vários blocos de logon podem ser exibidos no logon da interface do usuário. Portanto, sua organização pode controlar a exibição de logon, como usuários, sistemas de destino para o logon, acesso de Pre-logon para as rede e a estação de trabalho Bloquear/desbloquear políticas - com o uso de provedores de credenciais personalizado. Vários provedores de credenciais podem coexistir no mesmo computador.

Únicos provedores sign-on (SSO) podem ser desenvolvidos como um provedor de credenciais padrão ou como um provedor de Logon-Access.

Cada versão do Windows contém o provedor de credenciais de um padrão e o Pre-Logon de um padrão-Access Provider (PLAP), também conhecido como o provedor de SSO. O provedor SSO permite que os usuários a fazer uma conexão a uma rede antes de fazer logon no computador local. Quando esse provedor é implementado, o provedor não enumerar os blocos na interface do usuário de Logon.

Um provedor SSO destina-se a ser usada nos seguintes cenários:

-   **Logon de autenticação e o computador de rede são manipulados pelos provedores de credenciais diferentes.** Variações para este cenário incluem:

    -   Um usuário tem a opção de se conectar a uma rede, como se conectar a uma rede virtual privada (VPN), antes de fazer logon no computador, mas não é necessária para fazer essa conexão.

    -   Autenticação de rede é necessária para recuperar as informações usadas durante a autenticação interativa no computador local.

    -   Várias autenticações de rede são seguidas por um dos outros cenários. Por exemplo, um usuário se autentica em um provedor de serviços de Internet (ISP), autentica a uma VPN e, em seguida, usa suas credenciais de conta de usuário para fazer logon localmente.

    -   Credenciais armazenadas em cache são desabilitadas, e uma conexão de serviços de acesso remoto por meio de VPN é necessária antes do logon local para autenticar o usuário.

    -   Um usuário de domínio não tem uma conta local configurado em um computador ingressado no domínio e deve estabelecer uma conexão de serviços de acesso remoto por meio da conexão VPN antes de concluir o logon interativo.

-   **Logon de autenticação e o computador de rede são manipuladas pelo mesmo provedor de credenciais.** Nesse cenário, o usuário é necessário para se conectar à rede antes de fazer logon no computador.

**Enumeração de bloco de logon**

O provedor de credenciais enumera os blocos de logon nas seguintes instâncias:

-   Para esses sistemas operacionais designados a **aplica-se a** no início deste tópico.

-   O provedor de credenciais enumera os blocos para logon da estação de trabalho. O provedor de credenciais normalmente serializa as credenciais para autenticação para a autoridade de segurança local. Esse processo exibe blocos específicos para cada usuário e específicos para sistemas de destino de cada usuário.

-   A arquitetura de logon e autenticação permite que um usuário use blocos enumerados pelo provedor de credenciais para desbloquear uma estação de trabalho. Normalmente, o usuário conectado no momento é o bloco padrão, mas se mais de um usuário fizer logon, vários blocos são exibidos.

-   O provedor de credenciais enumera os blocos em resposta a uma solicitação de usuário para alterar sua senha ou outras informações privadas, como um PIN. Normalmente, o usuário conectado no momento é o bloco padrão; No entanto, se mais de um usuário fizer logon, vários blocos são exibidos.

-   O provedor de credenciais enumera blocos com base nas credenciais serializadas a ser usado para autenticação em computadores remotos. Credencial da interface do usuário não usa a mesma instância do provedor como da interface do usuário de Logon, o desbloqueio da estação de trabalho ou alterar a senha. Portanto, as informações de estado não podem ser mantidas no provedor entre instâncias de credencial da interface do usuário. Essa estrutura resulta em um bloco para cada logon no computador remoto, supondo que as credenciais tiverem sido serializadas corretamente. Esse cenário também é usado no User Account Control (UAC), que pode ajudar a impedir alterações não autorizadas a um computador, solicitar ao usuário permissão ou uma senha de administrador antes de permitir que as ações que podem afetar a operação do computador ou que poderia alterar as configurações que afetam outros usuários do computador.

O diagrama a seguir mostra o processo de credencial para os sistemas operacionais designado a **aplica-se a** no início deste tópico.

![Diagrama que mostra o processo de credencial para os sistemas operacionais designado a * * aplica-se a * * no início deste tópico](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credencial para logon de serviço e aplicativo
Autenticação do Windows foi projetada para gerenciar as credenciais para aplicativos ou serviços que não exigem interação do usuário. Aplicativos no modo de usuário são limitados em termos de quais recursos do sistema que eles têm acesso, enquanto serviços poderá ter acesso irrestrito aos dispositivos externos e memória do sistema.

Serviços do sistema e os aplicativos de nível de transporte acessar um provedor de suporte de segurança (SSP) por meio de suporte de segurança provedor Interface (SSPI) no Windows, que fornece funções para enumerar os pacotes de segurança disponíveis em um sistema, selecionando um pacote e usando esse pacote para obter uma conexão autenticada.

Quando uma conexão de cliente/servidor é autenticado:

-   O aplicativo no lado do cliente da conexão envia as credenciais para o servidor usando a função SSPI `InitializeSecurityContext (General)`.

-   O aplicativo no lado do servidor da conexão responde com a função SSPI `AcceptSecurityContext (General)`.

-   As funções SSPI `InitializeSecurityContext (General)` e `AcceptSecurityContext (General)` são repetidas até que todas as mensagens de autenticação necessárias foram trocadas para ter êxito ou falha de autenticação.

-   Após a conexão ser autenticada, a LSA no servidor usa informações do cliente para criar o contexto de segurança, que contém um token de acesso.

-   O servidor pode, em seguida, chamar a função SSPI `ImpersonateSecurityContext` para anexar o token de acesso a um thread de representação para o serviço.

**Aplicativos e o modo de usuário**

Modo de usuário no Windows é composto de dois sistemas capazes de passar as solicitações de e/s para os drivers do modo de kernel apropriado: o sistema de ambiente, que executa aplicativos escritos para muitos tipos diferentes de sistemas operacionais, e o sistema integral, que opera funções específicas do sistema em nome do sistema do ambiente.

O sistema integral gerencia as funções operacionais de system'specific em nome do sistema do ambiente e consiste em um processo do sistema de segurança (LSA), um serviço de estação de trabalho e um serviço do servidor. O processo de sistema de segurança lida com tokens de segurança, concede ou nega permissões para acessar as contas de usuário com base nas permissões de recurso, lida com solicitações de logon e inicia a autenticação de logon e determina quais recursos do sistema que o sistema operacional precisa de auditoria.

Aplicativos podem ser executados no modo de usuário em que o aplicativo pode ser executado como qualquer entidade de segurança, incluindo no contexto de segurança do sistema Local (SYSTEM). Aplicativos também podem ser executados no modo kernel em que o aplicativo pode ser executado no contexto de segurança do sistema Local (SYSTEM).

SSPI está disponível por meio do módulo Secur32.dll, que é uma API usada para a obtenção de serviços de segurança integrada para autenticação, a integridade da mensagem e a privacidade de mensagem. Ele fornece uma camada de abstração entre protocolos de nível de aplicativo e de segurança. Como aplicativos diferentes exigem diferentes maneiras de identificação ou autenticação de usuários e as diferentes maneiras de criptografar dados enquanto trafegam por uma rede, o SSPI fornece uma maneira de acessar bibliotecas de vínculo dinâmico (DLLs) que contêm diferentes de autenticação e funções de criptografia. Essas DLLs são chamadas de provedores de suporte de segurança (SSPs).

Contas de serviço gerenciadas e contas virtuais foram introduzidas no Windows Server 2008 R2 e Windows 7 para fornecer aplicativos cruciais, como o Microsoft SQL Server e serviços de informações da Internet (IIS), com o isolamento de suas próprias contas de domínio, enquanto elimina a necessidade de um administrador para administrar manualmente o nome da entidade de serviço (SPN) e as credenciais dessas contas. Para obter mais informações sobre esses recursos e sua função na autenticação, consulte [Managed Service contas documentação para Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) e [grupo gerenciados serviço de visão geral das contas](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Modo de kernel e serviços**

Embora a maioria dos aplicativos do Windows executado no contexto de segurança do usuário que inicia-os, isso não é verdade de serviços. Muitos serviços do Windows, como rede e serviços de impressão são iniciados pelo controlador de serviço quando o usuário inicia o computador. Esses serviços pode ser executado como serviço Local ou sistema Local e podem continuar a executar após o logoff do último usuário humano.

> [!NOTE]
> Os serviços normalmente são executados em contextos de segurança, conhecidos como sistema Local (SYSTEM), o serviço de rede ou serviço Local.  Windows Server 2008 R2 introduziu os serviços que são executados em uma conta de serviço gerenciado, que são entidades de domínio.

Antes de iniciar um serviço, o controlador de serviço faz logon usando a conta que é designada para o serviço e, em seguida, apresenta as credenciais do serviço para autenticação por LSA. O serviço do Windows implementa uma interface programática que o Gerenciador do controlador de serviço pode usar para controlar o serviço. Um serviço do Windows pode ser iniciado automaticamente quando o sistema é iniciado ou manualmente com um programa de controle de serviço. Por exemplo, quando um domínio ingressa em um computador de cliente do Windows, o serviço de Mensageiro no computador se conecta a um controlador de domínio e abre um canal seguro para ele. Para obter uma conexão autenticada, o serviço deve ter credenciais de confiança da autoridade de segurança Local (LSA) do computador remoto. Ao se comunicar com outros computadores na rede, a LSA usa as credenciais de conta de domínio do computador local, assim como todos os outros serviços em execução no contexto de segurança do sistema Local e do serviço de rede. Serviços no computador local são executados como sistema para que as credenciais não precisam ser apresentado para a LSA.

O arquivo Ksecdd. sys gerencia e criptografa essas credenciais e usa uma chamada de procedimento local na LSA. O tipo de arquivo está DRV (driver) e é conhecido como modo de kernel segurança suporte Provider (SSP) e, em que essas versões designadas na **aplica-se a** lista no início deste tópico, é FIPS 140-2 nível compatível com 1.

Modo kernel tem acesso completo aos recursos de hardware e sistema do computador. O modo de kernel interrompe serviços de modo de usuário e aplicativos acessem as áreas críticas do sistema operacional que eles não deveriam ter acesso ao.

## <a name="BKMK_LSA"></a>Autoridade de segurança local
A autoridade de segurança Local (LSA) é um processo de sistema protegido que autentica e registra os usuários logon no computador local. Além disso, a LSA mantém informações sobre todos os aspectos de segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local) e ele fornece vários serviços para conversão entre os nomes e identificadores de segurança (SIDs). O processo de sistema de segurança, segurança autoridade servidor serviço LSASS (Local), mantém o controle de políticas de segurança e as contas que estão em vigor em um computador.

A LSA valida a identidade do usuário com base no qual das seguintes duas entidades emitidas a conta do usuário:

-   **Autoridade de segurança local.** A LSA pode validar as informações de usuário, verificando o banco de dados do Gerenciador de contas de segurança (SAM) localizado no mesmo computador. Qualquer estação de trabalho ou servidor membro pode armazenar contas de usuário local e informações sobre grupos locais. No entanto, essas contas podem ser usadas para acessar apenas essa estação de trabalho ou o computador.

-   **Autoridade de segurança para o domínio local ou para um domínio confiável.** A LSA entra em contato com a entidade que emitiu a conta e a verificação de solicitações que a conta é válida e que originou a solicitação do titular da conta.

O serviço LSASS armazena credenciais na memória em nome de usuários com sessões ativas do Windows. As credenciais armazenadas permitem aos usuários acessar diretamente os recursos de rede, como compartilhamentos de arquivos, caixas de correio do Exchange Server e sites do SharePoint, sem precisar reinserir suas credenciais para cada serviço remoto.

O LSASS pode armazenar credenciais de várias formas, incluindo:

-   Texto não criptografado de criptografia reversível

-   Tíquetes Kerberos (ticket-granting tickets (TGTs), tíquetes de serviço)

-   Hash de NT

-   Hash do LM (LAN Manager)

Se o usuário fizer logon no Windows usando um cartão inteligente, o LSASS não armazenará uma senha de texto sem formatação, mas ele armazena o valor de hash de NT correspondente para a conta e o texto não criptografado PIN do cartão inteligente. Se o atributo da conta estiver habilitado para um cartão inteligente exigido no logon interativo, um valor de hash de NT aleatório será gerado automaticamente para a conta, e não o hash da senha original. O hash de senha gerado automaticamente quando o atributo é definido não muda.

Se um usuário faz logon um computador baseado em Windows com uma senha que é compatível com hashes de LM (LAN Manager), esse autenticador está presente na memória.

O armazenamento de credenciais em texto não criptografado na memória não pode ser desabilitado, mesmo que os provedores de credenciais que necessitam delas estejam desabilitados.

As credenciais armazenadas são diretamente associadas com as sessões de logon do serviço de subsistema de autoridade de segurança Local (LSASS) que foram iniciadas após a última reinicialização e não foram fechadas. Por exemplo, as sessões de LSA com credenciais LSA armazenadas são criadas quando um usuário:

-   Faz logon em uma sessão local ou sessão de protocolo de área de trabalho remota (RDP) no computador

-   Executa uma tarefa usando a opção **RunAs**

-   Executa um serviço Windows ativo no computador

-   Executa uma tarefa ou trabalho em lotes programado

-   Executa uma tarefa no computador local usando uma ferramenta de administração remota

Em algumas circunstâncias, os segredos LSA, que são partes secretas dos dados que são acessíveis somente aos processos de conta do sistema, são armazenados na unidade de disco rígido. Alguns desses segredos são credenciais que devem persistir após a reinicialização, e são armazenadas de forma criptografada no disco rígido. Credenciais armazenadas como segredos de LSA podem incluir:

-   Senha da conta para a conta do computador os serviços de domínio do Active Directory (AD DS)

-   Senhas da conta de serviços Windows que estão configurados no computador

-   Senhas da conta para tarefas agendadas configuradas

-   Senhas da conta de pools de aplicativos IIS e sites da Web

-   Senhas para contas da Microsoft

Introduzido no Windows 8.1, o sistema operacional cliente fornece proteção adicional ao LSA impedir a leitura da memória e injeção de código por processos não protegidos. Essa proteção aumenta a segurança para as credenciais que a LSA armazena e gerencia.

Para obter mais informações sobre essas proteções adicionais, consulte [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validação e credenciais armazenadas em cache
Mecanismos de validação contam com a apresentação de credenciais no momento do logon. No entanto, quando o computador é desconectado de um controlador de domínio e o usuário é apresentar as credenciais de domínio, o Windows usa o processo de credenciais armazenadas em cache no mecanismo de validação.

Cada vez que um usuário faz logon em um domínio, o Windows armazena em cache as credenciais fornecidas e armazena-os na seção de segurança no registro do sistema operacional.

Com credenciais armazenadas em cache, o usuário pode fazer logon um membro do domínio sem estar conectado a um controlador de domínio dentro desse domínio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Armazenamento de credenciais e validação
Nem sempre é desejável usar um conjunto de credenciais para acesso a recursos diferentes. Por exemplo, um administrador poderá usar administrativa em vez de credenciais do usuário ao acessar um servidor remoto. Da mesma forma, se um usuário acessa recursos externos, como uma conta bancária, ele ou ela só poderá usar as credenciais que são diferentes de suas credenciais de domínio. As seções a seguir descrevem as diferenças no gerenciamento de credenciais entre as versões atuais dos sistemas de operacionais do Windows e os sistemas operacionais Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processos de credencial de logon remoto
O protocolo de área de trabalho remota (RDP) gerencia as credenciais do usuário que se conecta a um computador remoto usando o cliente de área de trabalho remoto, que foi introduzida no Windows 8. As credenciais na forma de texto sem formatação são enviadas para o host de destino em que o host tenta executar o processo de autenticação e, se for bem-sucedido, conecta o usuário aos recursos permitidos. RDP não armazena as credenciais no cliente, mas as credenciais de domínio do usuário são armazenadas do LSASS.

Introduzido no Windows Server 2012 R2 e Windows 8.1, o modo administrador restrito fornece segurança adicional para cenários de logon remoto. Este modo de área de trabalho remota faz com que o aplicativo cliente executar uma desafio / resposta de logon de rede com a função unidirecional do NT (NTOWF) ou usar um tíquete de serviço Kerberos para autenticar para o host remoto. Depois que o administrador é autenticado, o administrador não tem as credenciais da conta respectiva no LSASS porque eles não foram fornecidos para o host remoto. Em vez disso, o administrador tem as credenciais da conta de computador para a sessão. As credenciais de administrador não são fornecidas para o host remoto, portanto, as ações são executadas como a conta de computador. Recursos também são limitados à conta de computador e o administrador não pode acessar os recursos com sua própria conta.

### <a name="automatic-restart-sign-on-credential-process"></a>Processo de credencial de logon de reinicialização automática
Quando um usuário faz logon em um dispositivo Windows 8.1, a LSA salva as credenciais do usuário na memória criptografada que são acessíveis somente pela LSASS.exe. Quando o Windows Update inicia uma reinicialização automática sem a presença do usuário, essas credenciais são usadas para configurar o logon automático para o usuário.

Na reinicialização, o usuário é conectado automaticamente por meio do mecanismo de logon automático e, em seguida, o computador adicionalmente está bloqueado para proteger a sessão do usuário. O bloqueio é iniciado por meio do Winlogon, enquanto o gerenciamento de credenciais é feito pela LSA. Ao entrar automaticamente e bloqueio de sessão do usuário no console, aplicativos de tela de bloqueio do usuário é reiniciado e está disponível.

Para obter mais informações sobre ARSO, consulte [Winlogon automática reiniciar Sign-On &#40;ARSO&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Nomes de usuário e senhas no Windows Vista e Windows XP
No Windows Server 2008, Windows Server 2003, Windows Vista e Windows XP, **nomes de usuários e senhas** no painel de controle simplifica o gerenciamento e o uso de vários conjuntos de credenciais de logon, incluindo certificados x. 509 usados com cartões inteligentes e credenciais em tempo real do Windows (agora chamadas de conta da Microsoft). As credenciais — parte do perfil do usuário — são armazenadas até que seja necessário. Essa ação pode aumentar a segurança em uma base por recurso, garantindo que se uma senha estiver comprometida, ele não comprometa toda a segurança.

Depois que um usuário faz logon e tenta acessar recursos protegidos por senha adicionais, como um compartilhamento em um servidor, e se as credenciais de logon do usuário padrão não são suficientes para obter acesso, **nomes de usuários e senhas** é consultado . Se as credenciais alternativas com as informações de logon correto tem sido salvo em **nomes de usuários e senhas**, essas credenciais são usadas para obter acesso. Caso contrário, o usuário é solicitado a fornecer credenciais de novo, poderá ser salvo para reutilização, mais tarde na sessão de logon ou durante uma sessão subsequente.

As seguintes restrições se aplicam:

-   Se **nomes de usuários e senhas** contém credenciais inválidas ou incorretas para um recurso específico, o acesso ao recurso é negado e o **usuário nomes e senhas armazenados** caixa de diálogo não são exibidos.

-   **Nomes de usuário e senhas armazenados** armazena as credenciais somente para NTLM, protocolo Kerberos, conta da Microsoft (anteriormente Windows Live ID) e autenticação de Secure Sockets Layer (SSL). Algumas versões do Internet Explorer mantém seu próprio cache para a autenticação básica.

Essas credenciais se tornam uma parte criptografada de um perfil de usuário local em \Documents and aplicativos\Microsoft\Credenciais \ nome_do_usuário \ dados de diretório. Como resultado, essas credenciais possam usar roaming com o usuário se a diretiva de rede do usuário oferece suporte a perfis de usuário móvel. No entanto, se o usuário tiver cópias dos **nomes de usuários e senhas** em dois computadores diferentes e alterações as credenciais que estão associadas com o recurso em um desses computadores, a alteração não será propagada para  **Nomes de usuário e senhas armazenados** no segundo computador.

### <a name="windows-vault-and-credential-manager"></a>Cofre do Windows e o Gerenciador de credenciais
O Gerenciador de credenciais foi introduzido no Windows Server 2008 R2 e Windows 7 como um recurso do painel de controle para armazenar e gerenciar senhas e nomes de usuário. O Gerenciador de credenciais permite aos usuários armazenar credenciais relevantes para outros sistemas e sites no cofre do Windows. Algumas versões do Internet Explorer usam esse recurso para a autenticação em sites.

O gerenciamento de credenciais executado pelo Gerenciador de Credenciais é controlado pelo usuário no computador local. Os usuários podem salvar e armazenar credenciais de navegadores e aplicativos do Windows com suporte para ter conveniência ao entrar nesses recursos. As credenciais são salvas em pastas criptografadas especiais no computador sob o perfil do usuário. Aplicativos que dão suporte a esse recurso (usando as APIs do Gerenciador de credenciais), como navegadores da web e aplicativos, podem apresentar as credenciais corretas a outros computadores e sites durante o processo de logon.

Quando um site, um aplicativo ou outro computador solicita autenticação por meio do protocolo Kerberos ou NTLM, uma caixa de diálogo aparece na qual você seleciona os **atualizar credenciais padrão** ou **salvar senha**caixa de seleção. Essa caixa de diálogo que permite ao usuário salvar credenciais localmente é gerada por um aplicativo compatível com as APIs do Gerenciador de credenciais. Se o usuário seleciona o **salvar senha** mantém controle de caixa de seleção, o Gerenciador de credenciais do nome de usuário do usuário, senha e informações relacionadas para o serviço de autenticação que está em uso.

Na próxima vez em que o serviço é usado, o Gerenciador de credenciais fornecerá automaticamente a credencial armazenada no cofre do Windows. Se a credencial não for aceita, o usuário será solicitado a fornecer as informações de acesso corretas. Se o acesso é concedido com as novas credenciais, o Gerenciador de credenciais substituirá a credencial anterior pela nova e, em seguida, armazena a nova credencial no cofre do Windows.

## <a name="BKMK_SAM"></a>Banco de dados de Gerenciador de contas de segurança
O Gerenciador de contas de segurança (SAM) é um banco de dados que armazena grupos e contas de usuário local. Ele está presente em todos os sistemas operacionais Windows; No entanto, quando um computador tiver ingressado em um domínio, o Active Directory gerencia contas de domínio nos domínios do Active Directory.

Por exemplo, computadores cliente executando o sistema operacional Windows participem de um domínio de rede comunicando-se com um controlador de domínio, mesmo quando nenhum usuário humano é conectado. Para iniciar a comunicação, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a LSA no controlador de domínio autentica a identidade do computador e, em seguida, constrói o contexto de segurança do computador, assim como faz para uma entidade de segurança humana. Este contexto de segurança define a identidade e recursos de um usuário ou serviço em um computador específico ou um usuário, serviço ou computador em uma rede. Por exemplo, o token de acesso contido dentro do contexto de segurança define os recursos (como um compartilhamento de arquivos ou impressora) que podem ser acessados e as ações que podem ser executadas por essa entidade de segurança – um usuário, computador ou serviço em que (como leitura, gravação ou modificação) recurso.

O contexto de segurança de um usuário ou computador pode variar de um computador para outro, como quando um usuário faz logon em um servidor ou uma estação de trabalho que não seja a estação de trabalho primário do usuário. Ele também pode variar de uma sessão para outra, como quando um administrador modifica os direitos e permissões do usuário. Além disso, o contexto de segurança é normalmente diferente quando um usuário ou computador está funcionando de forma independente, em uma rede, ou como parte de um domínio do Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Domínios locais e domínios confiáveis
Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio contam com a validade das autenticações provenientes de outro domínio. Relações de confiança ajudam a fornecer acesso controlado aos recursos compartilhados em um domínio de recurso (o domínio confiante), verificando que as solicitações que autenticação de entrada provenientes de uma autoridade confiável (o domínio confiável). Dessa forma, relações de confiança agem como pontes que permitem apenas validado viagem de solicitações de autenticação entre domínios.

Como uma relação de confiança específica passa as solicitações de autenticação depende de como ela é configurada. Relações de confiança podem ser unidirecional, fornecendo acesso do domínio confiável aos recursos no domínio confiante ou bidirecional, fornecendo acesso de cada domínio aos recursos em outro domínio. Relações de confiança são também intransitiva, caso em que uma relação de confiança existe somente entre os domínios de parceiro de confiança de duas ou transitiva, caso em que uma relação de confiança estende automaticamente para outros domínios que confie em qualquer um dos parceiros.

Para obter informações sobre relações de confiança de floresta e domínio referentes à autenticação, consulte [a autenticação delegada e relações de confiança](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados de autenticação do Windows
Uma infraestrutura de chave pública (PKI) é a combinação de software, as tecnologias de criptografia, processos e serviços que permitem que uma organização proteger suas comunicações e transações comerciais. A capacidade de uma PKI para proteger as comunicações e transações comerciais baseia-se a troca de certificados digitais entre recursos confiáveis e de usuários autenticados.

Um certificado digital é um documento eletrônico que contém informações sobre a entidade pertence, a entidade que ele foi emitido por, um número de série exclusivo ou alguma outra identificação exclusiva, emissão e datas de validade e uma impressão digital.

A autenticação é o processo de determinar se um host remoto pode ser confiável. Para estabelecer sua confiabilidade, o host remoto deve fornecer um certificado de autenticação aceitáveis.

Hosts remotos estabelecem sua credibilidade, obtendo um certificado de uma autoridade de certificação (CA). A autoridade de certificação pode, por sua vez, ter a certificação de uma autoridade superior, que cria uma cadeia de confiança. Para determinar se um certificado é confiável, um aplicativo deve determinar a identidade da AC raiz e, em seguida, determinar se ele é confiável.

Da mesma forma, o computador local ou um host remoto deve determinar se o certificado apresentado pelo usuário ou aplicativo é autêntico. O certificado apresentado pelo usuário por meio da LSA e SSPI é avaliado para autenticidade no computador local para logon local, na rede ou no domínio por meio de armazenamentos de certificados no Active Directory.

Para gerar um certificado, os dados de autenticação passa por meio de algoritmos de hash, como o Secure Hash Algorithm 1 (SHA1) para produzir um resumo da mensagem. O resumo da mensagem, em seguida, é assinado digitalmente, usando a chave particular do remetente para provar que o resumo da mensagem foi produzido pelo remetente.

> [!NOTE]
> SHA1 é o padrão no Windows 7 e Windows Vista, mas foi alterado para SHA2 no Windows 8.

**Autenticação de cartão inteligente**

Tecnologia de cartões inteligentes é um exemplo de autenticação baseada em certificado. Fazendo logon em uma rede com um cartão inteligente fornece uma forma eficiente de autenticação porque ele usa baseados em criptografia identificação e prova de posse ao autenticar um usuário em um domínio. Os serviços de certificados do Active Directory (AD CS) fornece a identificação baseada em criptografia por meio de emissão de um certificado de logon para cada cartão inteligente.

Para obter informações sobre a autenticação de cartão inteligente, consulte o [referência técnica do cartão inteligente Windows](https://technet.microsoft.com/library/ff404297.aspx).

Tecnologia de cartão inteligente virtual foi introduzida no Windows 8. Ele armazena o certificado do cartão inteligente no PC e, em seguida, protege-o usando o chip de segurança do dispositivo à prova de adulteração Trusted Platform Module (TPM). Dessa forma, o PC se torna, na verdade, o cartão inteligente que deve receber o PIN do usuário para ser autenticado.

**Autenticação remota e sem fio**

Autenticação de rede sem fio e remoto é outra tecnologia que usa certificados para autenticação. O serviço de autenticação da Internet (IAS) e servidores de rede virtual privada usam Extensible Authentication Protocol-Transport Level Security (EAP-TLS), Protected Extensible Authentication Protocol (PEAP) ou Internet Protocol security (IPsec) para Execute a autenticação baseada em certificado para vários tipos de acesso à rede, incluindo a rede virtual privada (VPN) e conexões sem fio.

Para obter informações sobre a autenticação baseada em certificado no sistema de rede, consulte [certificados e autenticação de acesso de rede](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Consulte também
[Conceitos de autenticação do Windows](https://technet.microsoft.com/library/d169018.aspx)


