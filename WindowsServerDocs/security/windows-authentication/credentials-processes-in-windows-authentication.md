---
title: "Processos de credenciais de autenticação do Windows"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-processes-in-windows-authentication"></a>Processos de credenciais de autenticação do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de referência para o profissional de TI descreve como a autenticação do Windows processa as credenciais.

Gerenciamento de credenciais do Windows é o processo pelo qual o sistema operacional recebe as credenciais do usuário ou serviço e protege essa informação para apresentação futura para o destino de autenticação. No caso de um computador ingressado no domínio, o destino de autenticação é o controlador de domínio. As credenciais usadas na autenticação são documentos digitais que associam a identidade do usuário para alguma forma de comprovação de autenticidade, como um certificado, uma senha ou um PIN.

Por padrão, as credenciais do Windows são validadas no banco de dados do Gerenciador de contas de segurança (SAM) no computador local ou no Active Directory em um computador ingressado no domínio, por meio do serviço do Winlogon. As credenciais são coletadas por meio de entrada do usuário na interface do usuário de logon ou programaticamente via a interface de programação de aplicativo (API) seja apresentado para o destino de autenticação.

Informações de segurança local são armazenadas no registro em **HKEY_LOCAL_MACHINE\SECURITY**. As informações armazenadas incluem configurações de política, valores de segurança padrão e informações de conta, como credenciais de logon em cache. Uma cópia do banco de dados SAM também é armazenada aqui, embora seja protegido contra gravação.

O diagrama a seguir mostra os componentes que são necessários e os caminhos que as credenciais levar por meio do sistema para autenticar o usuário ou processo para um logon bem-sucedido.

![Diagrama que mostra os componentes que são necessários e os caminhos que as credenciais levar por meio do sistema para autenticar o usuário ou processo para um logon bem-sucedido.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

A tabela a seguir descreve cada componente que gerencia credenciais no processo de autenticação no ponto de logon.

**Componentes de autenticação para todos os sistemas**

|Componente|Descrição|
|-------|--------|
|Logon do usuário|Winlogon.exe é o arquivo executável responsável pelo gerenciamento de interações de usuário segura. O serviço do Winlogon inicia o processo de logon para sistemas operacionais Windows, passando as credenciais coletadas por ação do usuário na área de trabalho segura (Logon da interface do usuário) a autoridade de segurança Local (LSA) por meio de Secur32.dll.|
|Logon de aplicativo|Aplicativo ou serviço logons que não necessitam de logon interativo. A maioria dos processos iniciados pelo usuário executado no modo de usuário usando Secur32.dll enquanto processos iniciados no momento da inicialização, como serviços, executados no modo kernel usando Ksecdd.sys.<br /><br />Para obter mais informações sobre o modo de usuário e modo kernel, veja aplicativos e o modo de usuário ou serviços e modo Kernel neste tópico.|
|Secur32.dll|Vários provedores de autenticação que processam da Base de autenticação.|
|Lsasrv.dll|O serviço LSA servidor, que tanto impõe políticas de segurança e age como o Gerenciador de pacotes de segurança para a LSA. A LSA contém a função negociar, que seleciona o NTLM ou Kerberos protocolo após determinar qual protocolo é seja bem-sucedido.|
|Provedores de suporte de segurança|Um conjunto de provedores que individualmente pode invocar um ou mais protocolos de autenticação. O conjunto padrão de provedores pode alterar a cada versão do sistema operacional Windows e provedores personalizados podem ser escritos.|
|Netlogon.dll|Os serviços que executa o serviço de Logon de rede são da seguinte maneira:<br /><br />-Mantém o canal seguro do computador (não deve ser confundido com Schannel) para um controlador de domínio.<br />-Passa as credenciais do usuário por meio de um canal seguro para o controlador de domínio e retorna os identificadores de segurança (SIDs) do domínio e direitos de usuário para o usuário.<br />-Publicar os registros de recurso de serviço no sistema de nome de domínio (DNS) e usa o DNS para resolver nomes para os endereços IP (Internet Protocol) dos controladores de domínio.<br />-Implementa o protocolo de replicação com base em chamada de procedimento remoto (RPC) para sincronizar os controladores de domínio primário (PDCs) e controladores de domínio de backup (BDCs).|
|Samsrv.dll|O Gerenciador de segurança contas (SAM), que armazena contas de segurança local, impõe políticas armazenadas localmente e dá suporte a APIs.|
|Registro|O registro contém uma cópia do banco de dados SAM, configurações de política de segurança local, valores de segurança padrão e informações de conta que só sejam acessíveis para o sistema.|

Este tópico contém as seguintes seções:

-   [Entrada de credenciais de logon do usuário](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credenciais para o aplicativo e logon de serviço](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridade de segurança local](#BKMK_LSA)

-   [Validação e credenciais armazenadas em cache](#BKMK_CachedCredentialsAndValidation)

-   [Armazenamento de credenciais e validação](#BKMK_CredentialStorageAndValidation)

-   [Banco de dados de Gerenciador de contas de segurança](#BKMK_SAM)

-   [Domínios do locais e confiáveis](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados na autenticação do Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credenciais de logon do usuário
No Windows Server 2008 e Windows Vista, a arquitetura gráfica GINA Identification and Authentication () foi substituída por um modelo de provedor de credenciais, que possibilitados enumerar tipos diferentes de logon por meio do uso de blocos de logon. Ambos os modelos estão descritos abaixo.

**Arquitetura gráfica identificação e autenticação**

A arquitetura de autenticação (GINA) Graphical Identification and se aplica aos sistemas operacionais Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. Nesses sistemas, cada sessão de logon interativo cria uma instância separada do serviço do Winlogon. A arquitetura GINA é carregada no espaço de processo usado pelo Winlogon, recebe e processa as credenciais e faz as chamadas para as interfaces de autenticação por meio de LSALogonUser.

As instâncias do Winlogon para um logon interativo executados na sessão 0. Serviços do sistema hosts sessão 0 e outros processos críticos, incluindo o processo de autoridade de segurança Local (LSA).

O diagrama a seguir mostra o processo de credenciais para Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional.

![Diagrama mostrando o processo de credenciais para Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitetura de provedor de credenciais**

A arquitetura do provedor de credenciais se aplica a essas versões designadas no **aplica-se a** lista no início deste tópico. Nesses sistemas, a arquitetura de entrada de credenciais alterado para um design extensible usando os provedores de credenciais. Esses provedores são representados pelos blocos de logon diferentes na área de trabalho segura que permitirem qualquer número de cenários de logon - contas diferentes para o mesmo usuário e métodos de autenticação diferentes, como biometria, senha e cartão inteligente.

Com a arquitetura de provedor de credenciais, o Winlogon sempre começa da interface do usuário de Logon após ele recebe um evento de sequência segura. Interface do usuário de logon consulta cada provedor de credenciais para o número de diferentes tipos de credencial que o provedor está configurado para enumerar. Provedores de credenciais tem a opção de especificar um desses blocos como padrão. Depois que todos os provedores têm enumerados seus blocos, interface do usuário de Logon exibe-los para o usuário. O usuário interage com um bloco para fornecer suas credenciais. Interface do usuário de logon envia essas credenciais para autenticação.

Provedores de credenciais não são mecanismos de imposição. Eles são usados para coletar e serialização de credenciais. Os pacotes de autoridade de segurança Local e autenticação reforçar a segurança.

Provedores de credenciais são registrados no computador e serão responsáveis pelo seguinte:

-   Descreve as informações de credenciais necessárias para autenticação.

-   Manipulação de comunicação e lógica de autoridades de autenticação externa.

-   Credenciais de empacotamento de interativos e logon de rede.

Credenciais de empacotamento de interativos e logon de rede inclui o processo de serialização. Serializando credenciais vários blocos de logon podem ser exibidos na interface do usuário de logon. Portanto, sua organização pode controlar a tela de logon, como os usuários, sistemas de destino para logon, pré-login acessem as rede e estação de trabalho bloqueio/desbloqueio políticas - através do uso de provedores de credenciais personalizado. Vários provedores de credenciais podem coexistir no mesmo computador.

Únicos provedores logon (SSO) podem ser desenvolvidos como um provedor de credenciais padrão ou como um provedor de login-acesso.

Cada versão do Windows contém o provedor de credenciais de um padrão e um padrão pré-login acesso provedor (PLAP), também conhecido como o SSO. O provedor SSO permite que os usuários façam uma conexão a uma rede antes de fazer logon no computador local. Quando esse provedor é implementado, o provedor não enumerar blocos na interface do usuário de Logon.

Um provedor SSO se destina a ser usado nos seguintes cenários:

-   **Logon de autenticação e o computador da rede são manipuladas pelo provedores de credenciais diferentes.** Variações para esse cenário incluem:

    -   Um usuário tem a opção de se conectar a uma rede, como se conectar a uma rede virtual privada (VPN), antes de fazer logon no computador, mas não é necessário para tornar esta conexão.

    -   Autenticação de rede é necessária para recuperar informações usadas durante a autenticação interativa no computador local.

    -   Várias autenticações de rede são seguidas por um dos outros cenários. Por exemplo, um usuário se autentica um provedor de serviços de Internet (ISP), autentica a uma VPN e usa suas credenciais de conta de usuário para fazer logon localmente.

    -   Credenciais armazenadas em cache são desabilitadas, e uma conexão de serviços de acesso remoto por meio de VPN é necessária antes do logon local para autenticar o usuário.

    -   Um usuário de domínio não tem uma conta local configurar em um computador ingressado no domínio e deve estabelecer uma conexão de serviços de acesso remoto por meio de conexão de VPN antes de concluir o logon interativo.

-   **Logon de autenticação e o computador da rede são manipuladas pelo mesmo provedor de credenciais.** Nesse cenário, o usuário é necessário para se conectar à rede antes do logon no computador.

**Enumeração de bloco de logon**

O provedor de credenciais enumera blocos de logon nas seguintes instâncias:

-   Para esses sistemas operacionais designados no **se aplica a** lista no início deste tópico.

-   O provedor de credenciais enumera os blocos de logon de estação de trabalho. Normalmente, o provedor de credenciais serializa credenciais de autenticação para a autoridade de segurança local. Esse processo exibe blocos específicos para cada usuário e específicos para sistemas de destino de cada usuário.

-   A arquitetura de logon e autenticação permite que um usuário use blocos enumerados pelo provedor de credenciais para desbloquear uma estação de trabalho. Normalmente, o usuário conectado no momento é o bloco padrão, mas se mais de um usuário fizer logon, vários blocos são exibidos.

-   O provedor de credenciais enumera blocos em resposta a uma solicitação do usuário para alterar sua senha ou outras informações particulares, como um PIN. Normalmente, o usuário conectado no momento é o bloco padrão; No entanto, se mais de um usuário fizer logon, vários blocos são exibidos.

-   O provedor de credenciais enumera blocos de acordo com as credenciais serializadas para ser usada para autenticação em computadores remotos. Interface do usuário de credenciais não usam a mesma instância do provedor de como a interface do usuário de Logon, desbloquear estação de trabalho ou alterar a senha. Portanto, informações de estado não podem ser mantidas no provedor entre as instâncias da interface do usuário de credenciais. Essa estrutura resulta em um bloco para cada logon no computador remoto, pressupondo que as credenciais tiverem sido serializadas corretamente. Esse cenário também é usado em User Account Control (UAC), que pode ajudar a impedir alterações não autorizadas em um computador por solicitar ao usuário permissão ou uma senha de administrador antes de permitir ações capazes de afetar potencialmente a operação do computador ou que podem alterar as configurações que afetam outros usuários do computador.

O diagrama a seguir mostra o processo de credenciais para os sistemas operacionais designados no **aplica-se a** lista no início deste tópico.

![Diagrama que mostra o processo de credenciais para os sistemas operacionais designados no * * aplica-se a * * lista no início deste tópico](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credenciais para o aplicativo e logon de serviço
Autenticação do Windows foi projetada para gerenciar credenciais para aplicativos ou serviços que não exigem interação do usuário. Aplicativos no modo de usuário são limitados em termos de quais recursos do sistema que eles têm acesso, enquanto os serviços podem ter acesso irrestrito para a memória do sistema e dispositivos externos.

Serviços do sistema e aplicativos de nível de transporte acessar um Security Support Provider (SSP) por meio do suporte SSPI Security Provider Interface () no Windows, que fornece funções para enumerar os pacotes de segurança disponíveis em um sistema, selecionando um pacote e usar esse pacote para obter uma conexão autenticado.

Quando uma conexão de cliente/servidor é autenticado:

-   O aplicativo no lado do cliente da conexão envia as credenciais para o servidor usando a função SSPI `InitializeSecurityContext (General)`.

-   Responde a aplicação no lado do servidor da conexão com a função SSPI `AcceptSecurityContext (General)`.

-   As funções SSPI `InitializeSecurityContext (General)`e `AcceptSecurityContext (General)`são repetidas até que todas as mensagens de autenticação necessárias foram trocadas para ter sucesso ou falha na autenticação.

-   Depois que a conexão foi autenticado, a LSA no servidor usa informações do cliente para criar o contexto de segurança, que contém um token de acesso.

-   O servidor, em seguida, pode chamar a função SSPI `ImpersonateSecurityContext`para anexar o token de acesso a um segmento de representação para o serviço.

**Aplicativos e o modo de usuário**

Modo de usuário no Windows é composto de dois sistemas capazes de passagem de solicitações de e/s para os drivers de modo kernel apropriado: o sistema de ambiente, que é executado aplicativos escritos para muitos tipos diferentes de sistemas operacionais, e o sistema integral, que opera funções específicas do sistema em nome do sistema de ambiente.

O sistema integral gerencia operacional funções system'specific em nome do sistema de ambiente e consiste em um processo do sistema de segurança (o LSA), um serviço de estação de trabalho e um serviço de servidor. O processo do sistema de segurança lida com tokens de segurança, concede ou nega permissões para acessar contas de usuário com base nas permissões de recurso, manipula solicitações de logon e inicia a autenticação de logon e determina quais recursos do sistema que o sistema operacional necessita de auditoria.

Aplicativos podem ser executados no modo de usuário em que o aplicativo pode ser executado como qualquer entidade de segurança, incluindo no contexto de segurança do sistema Local (sistema). Aplicativos também podem ser executados no modo de kernel onde o aplicativo pode ser executado no contexto de segurança do sistema Local (sistema).

SSPI está disponível por meio do módulo Secur32.dll, que é uma API usada para obtenção de serviços de segurança integrada para autenticação, a integridade da mensagem e a privacidade de mensagem. Ele fornece uma camada de abstração entre protocolos de nível de aplicativo e protocolos de segurança. Como diferentes aplicativos exigem diferentes formas de identificação ou autenticação de usuários e maneiras diferentes de criptografar dados à medida que ele transita através de uma rede, SSPI oferece uma maneira de acessar bibliotecas de vínculo dinâmico (DLLs) que contêm funções criptográficas e autenticação diferentes. Essas DLLs são chamadas de provedores de suporte de segurança (SSPs).

Serviço gerenciado contas e virtual foram introduzidas no Windows Server 2008 R2 e Windows 7 para fornecer aplicativos essenciais, como o Microsoft SQL Server e o Internet Information Services (IIS), com o isolamento de suas próprias contas de domínio, enquanto eliminando a necessidade de um administrador manualmente administrar o nome de entidade de serviço (SPN) e credenciais para essas contas. Para saber mais sobre esses recursos e sua função na autenticação, consulte [gerenciado serviço contas documentação para o Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) e [visão geral de contas de serviço do grupo gerenciado ](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Modo de kernel e serviços**

Embora a maioria dos aplicativos do Windows executados no contexto de segurança do usuário que inicia, isso não é verdade dos serviços. Muitos serviços do Windows, como redes e serviços de impressão, são iniciados com o controlador de serviço quando o usuário inicia o computador. Esses serviços talvez executados como sistema Local ou de serviço Local e podem continuar a ser executado depois do último usuário humano faz logoff.

> [!NOTE]
> Normalmente, os serviços executados em contextos de segurança conhecidos como sistema Local (sistema), serviço de rede ou serviço Local.  Windows Server 2008 R2 introduziu serviços que são executados em uma conta de serviço gerenciado, que são entidades de domínio.

Antes de iniciar um serviço, o controlador de serviço faz logon usando a conta que é designada para o serviço e, em seguida, apresenta as credenciais do serviço de autenticação, a LSA. O serviço do Windows implementa uma interface de programação que pode usar o Gerenciador de controlador de serviço para controlar o serviço. Um serviço do Windows pode ser iniciado automaticamente quando o sistema é iniciado ou manualmente com um programa de controle de serviço. Por exemplo, quando um computador cliente com Windows ingressa em um domínio, o serviço do messenger no computador se conecta a um controlador de domínio e abre um canal seguro a ele. Para obter uma conexão autenticado, o serviço deve ter credenciais relações de confiança da autoridade de segurança Local (LSA) do computador remoto. Ao se comunicar com outros computadores na rede, a LSA usa as credenciais da conta de domínio do computador local, assim como todos os outros serviços em execução no contexto de segurança do sistema Local e serviço de rede. No computador local, os serviços executados como sistema credenciais não é necessário ser apresentado a LSA.

O arquivo Ksecdd.sys gerencia e criptografa essas credenciais e usa uma chamada de procedimento local para a LSA. O tipo de arquivo é DRV (driver) e é conhecido como o modo de kernel Security Support Provider (SSP) e, nessas versões designadas no **aplica-se a** listar no início deste tópico, é o padrão FIPS 140-2 nível 1 compatível.

O modo de kernel tem acesso completo aos recursos de hardware e sistema do computador. O modo de kernel interrompe o modo de usuário serviços e aplicativos acessem áreas críticas do sistema operacional que eles não devem ter acesso a.

## <a name="BKMK_LSA"></a>Autoridade de segurança local
A autoridade de segurança Local (LSA) é um processo de sistema protegido que autentica e faz logon de usuários logon no computador local. Além disso, a LSA mantém informações sobre todos os aspectos de segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local) e fornece vários serviços para conversão entre nomes e identificadores de segurança (SIDs). O processo de sistema de segurança, segurança autoridade servidor serviço LSASS (Local), mantém o controle das políticas de segurança e as contas que estão em vigor em um sistema de computador.

A LSA valida a identidade do usuário com base no qual as seguintes duas entidades emitidas a conta do usuário:

-   **Autoridade de segurança local.** A LSA pode validar informações do usuário, verificando o banco de dados do Gerenciador de contas de segurança (SAM) localizado no mesmo computador. Qualquer servidor membro ou estação de trabalho pode armazenar informações sobre grupos locais e contas de usuário local. No entanto, essas contas podem ser usadas para acessar apenas essa estação de trabalho ou computador.

-   **Autoridade de segurança para o domínio local ou para um domínio confiável.** A LSA entra em contato com a entidade que emitiu a conta e a verificação de solicitações que a conta é válida e se a solicitação originada do titular da conta.

O Local LSASS Security Authority Subsystem Service () armazena credenciais na memória em nome dos usuários com sessões ativas do Windows. As credenciais armazenadas permitem que os usuários perfeitamente acessar recursos de rede, como compartilhamentos de arquivos, caixas de correio do Exchange Server e sites do SharePoint sem inserir novamente suas credenciais para cada serviço remoto.

LSASS podem armazenar credenciais em várias formas, incluindo:

-   Texto não criptografado reversível

-   Tíquetes Kerberos (tíquetes de concessão (TGTs), tíquetes de serviço)

-   Hash do NT

-   Hash do LAN Manager (LM)

Se o usuário fizer logon Windows usando um cartão inteligente, LSASS não armazenar uma senha de texto sem formatação, mas ele armazena o valor de hash NT correspondente para a conta e o texto não criptografado PIN para o cartão inteligente. Se o atributo de conta for habilitado em um cartão inteligente é necessário para logon interativo, um valor de hash NT aleatório é gerado automaticamente para a conta em vez do hash da senha original. Não altere o hash da senha que é gerado automaticamente quando o atributo é definido.

Se um usuário faz logon em um computador baseado no Windows com uma senha que seja compatível com hashes do LAN Manager (LM), esse autenticador está presente na memória.

O armazenamento de credenciais de texto sem formatação na memória não pode ser desabilitado, mesmo se os provedores de credenciais que precisem deles são desabilitados.

As credenciais armazenadas são associadas diretamente as sessões de logon Local LSASS Security Authority Subsystem Service () que foram iniciadas depois que a última reiniciar e não tiver sido fechadas. Por exemplo, as sessões de LSA com as credenciais armazenadas da LSA são criadas quando um usuário faz um destes procedimentos:

-   Faz logon em um local ou uma sessão remota protocolo RDP (Desktop) no computador

-   Executa uma tarefa usando o **RunAs** opção

-   Executa um serviço Windows ativo no computador

-   Executa um trabalho em lotes ou de tarefa agendado

-   Executa uma tarefa no computador local usando uma ferramenta de administração remota

Em algumas circunstâncias, os segredos LSA, que são secretas partes dos dados que são acessíveis apenas a processos de conta do sistema, são armazenados na unidade de disco rígido. Alguns desses segredos são as credenciais que devem persistir após a reinicialização, e eles são armazenados em formato criptografado na unidade de disco rígido. As credenciais armazenadas como segredos LSA podem incluir:

-   Senha de conta para a conta do computador os serviços de domínio do Active Directory (AD DS)

-   Senhas de conta para serviços do Windows que estão configurados no computador

-   Senhas de conta configurado tarefas agendadas

-   Senhas de conta para pools de aplicativos IIS e sites

-   Senhas de contas da Microsoft

Introduzida no Windows 8.1, o sistema operacional cliente fornece proteção adicional para a LSA impedir que a memória de leitura e injeção de código por processos não protegidos. Essa proteção aumenta a segurança para as credenciais que a LSA armazena e gerencia.

Para saber mais sobre essas proteções adicionais, consulte [configurando proteção adicional de LSA](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validação e credenciais armazenadas em cache
Mecanismos de validação dependem da apresentação de credenciais no momento do logon. No entanto, quando o computador for desconectado de um controlador de domínio e o usuário é apresentar as credenciais de domínio, o Windows usa o processo de credenciais armazenadas em cache no mecanismo de validação.

Cada vez que um usuário faz logon em um domínio, Windows armazena em cache as credenciais fornecidas e os armazena na seção segurança no registro do sistema operacional.

Com credenciais armazenadas em cache, o usuário pode fazer logon em um membro do domínio sem estar conectado a um controlador de domínio nesse domínio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Armazenamento de credenciais e validação
Nem sempre é desejável para usar um conjunto de credenciais para acessar recursos diferentes. Por exemplo, um administrador pode querer usar administrativas em vez de credenciais do usuário ao acessar um servidor remoto. Da mesma forma, se um usuário acessa recursos externos, como uma conta bancária, ele ou ela só pode usar as credenciais que são diferentes das suas credenciais de domínio. As seções a seguir descrevem as diferenças no gerenciamento de credenciais entre as versões atuais dos sistemas operacionais Windows e os sistemas operacionais Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processos de credenciais de logon remoto
O protocolo de área de trabalho remota (RDP) gerencia as credenciais do usuário que se conecta a um computador remoto usando o cliente de área de trabalho remota, que foi introduzido no Windows 8. As credenciais em forma de texto sem formatação são enviadas para o host de destino em que o host tenta executar o processo de autenticação e, se bem-sucedido, conecta o usuário a recursos permitidos. RDP não armazena as credenciais no cliente, mas as credenciais de domínio do usuário são armazenadas no LSASS.

Introduzida no Windows Server 2012 R2 e Windows 8.1, o modo administrador restrito oferece segurança adicional para cenários de logon remoto. Esse modo de área de trabalho remota faz com que o aplicativo cliente realizar uma desafio / resposta de logon de rede com a função unidirecional NT (NTOWF) ou usar um tíquete de serviço Kerberos durante a autenticação para o host remoto. Depois que o administrador seja autenticado, o administrador não tem as credenciais da conta respectivos em LSASS porque eles não foram fornecidos com o host remoto. Em vez disso, o administrador tenha as credenciais da conta de computador para a sessão. Credenciais de administrador não são fornecidas para o host remoto, portanto, ações são executadas como a conta de computador. Recursos também são limitados a conta do computador e o administrador não pode acessar os recursos com sua própria conta.

### <a name="automatic-restart-sign-on-credential-process"></a>Processo de credenciais de logon de reinicialização automática
Quando um usuário entra em um dispositivo Windows 8.1, a LSA salva as credenciais do usuário na memória criptografada que são acessíveis apenas por LSASS.exe. Quando o Windows Update inicia uma reinicialização automática sem a presença do usuário, essas credenciais são usadas para configurar o logon automático para o usuário.

Ao reiniciar, o usuário é conectado automaticamente por meio do mecanismo de Autologon e, em seguida, o computador Além disso é bloqueado para proteger a sessão do usuário. O bloqueio é iniciada por meio do Winlogon, enquanto o gerenciamento de credenciais é feito ao LSA. Ao fazer logon automaticamente no e bloqueando a sessão do usuário no console, aplicativos de tela de bloqueio do usuário é reiniciado e disponível.

Para saber mais sobre ARSO, consulte [Winlogon automática reiniciar logon & #40; ARSO & #41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Nomes de usuário e senhas no Windows Vista e Windows XP
No Windows Server 2008, Windows Server 2003, Windows Vista e Windows XP, **armazenados nomes de usuário e senhas** no painel de controle simplifica o gerenciamento e o uso de vários conjuntos de credenciais de logon, incluindo certificados x. 509 usados com cartões inteligentes e credenciais do Windows Live (agora chamadas de conta da Microsoft). As credenciais - parte do perfil do usuário - são armazenadas até que o necessário. Essa ação pode aumentar a segurança em uma base por recurso, garantindo que, se uma senha for comprometida, ele não comprometa toda a segurança.

Depois que um usuário faz logon e tenta acessar recursos adicionais de protegidas por senha, como um compartilhamento em um servidor, e se as credenciais de logon do usuário padrão não forem suficientes para obter acesso, **armazenados nomes de usuário e senhas** é consultada. Se as credenciais alternativas com as informações de logon corretas tiverem sido salvas no **armazenados nomes de usuário e senhas**, as credenciais são usadas para obter acesso. Caso contrário, o usuário é solicitado a fornecer novas credenciais, que, em seguida, podem ser salvos para reutilização, mais tarde na sessão de logon ou durante uma sessão subsequente.

As seguintes restrições se aplicam:

-   Se **armazenados nomes de usuário e senhas** contém credenciais incorretas ou inválidas para um recurso específico, acesso ao recurso é negado e o **armazenados nomes de usuário e senhas** caixa de diálogo não aparece.

-   **Nomes de usuário e senhas armazenados** armazena credenciais somente para NTLM, o protocolo Kerberos, conta da Microsoft (anteriormente Windows Live ID) e autenticação Secure Sockets Layer (SSL). Algumas versões do Internet Explorer mantêm seu próprios cache para autenticação básica.

Essas credenciais se tornam uma parte de um perfil de usuário local no diretório do usuário \ Application aplicativos\Microsoft\Credenciais e \Documents criptografada. Como resultado, essas credenciais podem fazer roaming com o usuário se a política de rede do usuário dá suporte a perfis de usuário móvel. No entanto, se o usuário tiver cópias de **armazenados nomes de usuário e senhas** em dois computadores diferentes e alterações as credenciais que são associadas com o recurso em um desses computadores, a alteração não é propagada para **armazenados nomes de usuário e senhas** no segundo computador.

### <a name="windows-vault-and-credential-manager"></a>Cofre Windows e o Gerenciador de credenciais
O Gerenciador de credenciais foi introduzido no Windows Server 2008 R2 e Windows 7 como um recurso de painel de controle para armazenar e gerenciar nomes de usuário e senhas. O Gerenciador de credenciais permite que os usuários armazenar credenciais relevantes para outros sistemas e sites no Windows cofre. Algumas versões do Internet Explorer usam esse recurso para autenticação para sites.

Gerenciamento de credenciais usando o Gerenciador de credenciais é controlado pelo usuário no computador local. Os usuários podem salvar e armazenar credenciais do suporte a navegadores e aplicativos do Windows para torná-la conveniente quando eles precisam fazer logon nesses recursos. As credenciais são salvos em pastas criptografadas especiais no computador com o perfil do usuário. Aplicativos que dão suporte a esse recurso (por meio do uso das APIs do Gerenciador de credenciais), como navegadores da web e aplicativos, podem apresentar as credenciais corretas para outros computadores e sites durante o processo de logon.

Quando um site, um aplicativo ou outra autenticação de solicitações do computador por meio de NTLM ou o protocolo Kerberos, uma caixa de diálogo aparece no que você selecionar o **credenciais padrão do Update** ou **salvar senha** caixa de seleção. Essa caixa de diálogo que permite ao usuário salvar credenciais localmente é gerada por um aplicativo que dá suporte a APIs do Gerenciador de credenciais. Se o usuário seleciona o **salvar senha** caixa de seleção, o Gerenciador de credenciais mantém o controle do nome de usuário, senha, e informações relacionadas para o serviço de autenticação que está em uso.

Na próxima vez que o serviço é usado, o Gerenciador de credenciais fornece automaticamente as credenciais armazenadas no cofre do Windows. Se não for aceita, o usuário é solicitado para obter as informações corretas de acesso. Se o acesso é concedido com as novas credenciais, o Gerenciador de credenciais substitui a credencial anterior pelo novo e, em seguida, armazena as novas credenciais no cofre do Windows.

## <a name="BKMK_SAM"></a>Banco de dados de Gerenciador de contas de segurança
O Gerenciador de contas de segurança (SAM) é um banco de dados que armazena contas de usuário local e grupos. Está presente em todos os sistemas operacionais Windows; No entanto, quando um computador ingressa em um domínio, o Active Directory gerencia contas de domínio de domínios do Active Directory.

Por exemplo, computadores cliente que executam um sistema operacional Windows participarem de um domínio de rede se comunicando com um controlador de domínio, mesmo quando nenhum usuário humano é conectado. Para iniciar as comunicações, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a LSA no controlador de domínio autentica a identidade do computador e, em seguida, constrói o contexto de segurança do computador, assim como faz para uma entidade de segurança humana. Esse contexto de segurança define a identidade e os recursos de um usuário ou um serviço em um computador específico ou um usuário, o serviço ou o computador em uma rede. Por exemplo, o token de acesso contido dentro do contexto de segurança define os recursos (como um compartilhamento de arquivos ou impressora) que podem ser acessados e as ações (como leitura, gravação ou modificação) que podem ser executadas por essa entidade de segurança - um usuário, computador ou serviço em que recurso.

O contexto de segurança de um usuário ou computador pode variar de um computador para outro, como quando um usuário faz logon em um servidor ou uma estação de trabalho que não seja a estação de trabalho do usuário principal. Ele também pode variar de uma sessão para outro, como quando um administrador modifica direitos e permissões do usuário. Além disso, o contexto de segurança é normalmente diferente quando um usuário ou computador estiver funcionando de forma independente, em uma rede ou como parte de um domínio do Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Domínios do locais e confiáveis
Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio dependem a validade das autenticações provenientes de outro domínio. Confie ajudam a fornecer acesso controlado a recursos compartilhados em um domínio de recurso (o domínio confiante) Verificando que solicita que a autenticação de entrada provenientes de uma autoridade confiável (o domínio confiável). Dessa forma, relações de confiança atuam como pontes que permitem apenas validado viagem de solicitações de autenticação entre domínios.

Como uma relação de confiança específica passa solicitações de autenticação depende de como ele está configurado. Relações de confiança podem ser, fornecendo acesso do domínio confiável aos recursos do domínio confiante unidirecional ou bidirecional, fornecendo acesso de cada domínio aos recursos em outro domínio. Relações de confiança também são relação nesse caso, uma relação de confiança existe apenas entre os domínios de parceiros de duas confiança ou transitivas, caso em que uma relação de confiança automaticamente estende a outros domínios que qualquer um dos parceiros confie.

Para obter informações sobre relações de confiança de floresta e domínio em relação a autenticação, consulte [autenticação recebido e relações de confiança](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados na autenticação do Windows
Uma infraestrutura de chave pública (PKI) é a combinação de software, tecnologias de criptografia, processos e serviços que permitem que uma organização proteger suas comunicações e transações comerciais. A capacidade de uma PKI para proteger as comunicações e transações comerciais se baseia a troca de certificados digitais entre usuários autenticados e recursos confiáveis.

Um certificado digital é um documento eletrônico que contém informações sobre a entidade que ela pertence a, a entidade que ele foi emitido por, um número de série exclusivo ou alguma outra identificação exclusiva, emissão e as datas de validade e uma impressão digital.

Autenticação é o processo de determinar se um host remoto pode ser confiável. Para estabelecer sua confiabilidade, o host remoto deve fornecer um certificado de autenticação aceitável.

Hosts remotos estabelecem sua confiabilidade obtendo um certificado de uma autoridade de certificação (CA). A CA pode, por sua vez, ter certificação de uma autoridade superior, que cria uma cadeia de confiança. Para determinar se um certificado é confiável, um aplicativo deve determinar a identidade da CA raiz e, em seguida, determine se é confiável.

Da mesma forma, o host remoto ou o computador local deve determinar se o certificado apresentado pelo usuário ou aplicativo é autêntico. O certificado apresentado pelo usuário por meio da LSA e SSPI é avaliado autenticidade no computador local para logon local, na rede ou no domínio através das lojas de certificado no Active Directory.

Para produzir um certificado, dados de autenticação passa por meio de algoritmos de hash, como Secure Hash Algorithm 1 (SHA1), para produzir um resumo da mensagem. Resumo da mensagem é assinado digitalmente, usando a chave privada do remetente para provar que o resumo de mensagem foi produzido pelo remetente.

> [!NOTE]
> SHA1 é o padrão no Windows 7 e Windows Vista, mas foi alterado para SHA2 no Windows 8.

**Autenticação de cartão inteligente**

Tecnologia de cartão inteligente é um exemplo de autenticação baseada em certificado. Fazer logon em uma rede com um cartão inteligente fornece uma forma eficiente de autenticação porque usa identificação baseada em criptografia e prova de posse ao autenticar um usuário em um domínio. Os serviços de certificados do Active Directory (AD CS) fornece a identificação baseadas em criptografia por meio da emissão de um certificado de logon para cada cartão inteligente.

Para obter informações sobre autenticação com cartão inteligente, consulte o [referência técnica do Windows cartão inteligente](https://technet.microsoft.com/library/ff404297.aspx).

Tecnologia de cartão inteligente virtual foi introduzida no Windows 8. Ele armazena o certificado do cartão inteligente no computador e protege usando chip de segurança do dispositivo à prova de adulteração Trusted Platform Module (TPM). Dessa forma, o computador, na verdade, torna-se o cartão inteligente que deve receber o PIN do usuário para ser autenticado.

**Autenticação remota e sem fio**

Autenticação de rede remoto e sem fio é outra tecnologia que usa certificados para autenticação. O serviço de autenticação da Internet (IAS) e os servidores de rede virtual privada usam Extensible Authentication Protocol-Transport Level Security (EAP-TLS), Protected Extensible Authentication Protocol (PEAP) ou protocolo IPSec (IPsec) para realizar autenticação baseada em certificado para vários tipos de acesso à rede, incluindo rede virtual privada (VPN) e conexões sem fio.

Para obter informações sobre a autenticação baseada em certificado em rede, consulte [certificados e autenticação de acesso de rede](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Consulte também
[Conceitos de autenticação do Windows](https://technet.microsoft.com/library/d169018.aspx)


