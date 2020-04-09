---
title: Processos de credenciais na autenticação do Windows
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2de8383ce6a946dfdd80cfc027478c495037169b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857579"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processos de credenciais na autenticação do Windows

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para o profissional de ti descreve como a autenticação do Windows processa as credenciais.

O gerenciamento de credenciais do Windows é o processo pelo qual o sistema operacional recebe as credenciais do serviço ou do usuário e protege essas informações para futuras apresentações no destino de autenticação. No caso de um computador ingressado no domínio, o destino de autenticação é o controlador de domínio. As credenciais usadas na autenticação são documentos digitais que associam a identidade do usuário a alguma forma de verificação de autenticidade, como um certificado, uma senha ou um PIN.

Por padrão, as credenciais do Windows são validadas no banco de dados SAM (Gerenciador de contas de segurança) no computador local ou em relação a Active Directory em um computador ingressado no domínio, por meio do serviço Winlogon. As credenciais são coletadas por meio da entrada do usuário na interface do usuário de logon ou programaticamente por meio da API (interface de programação de aplicativo) a ser apresentada ao destino de autenticação.

As informações de segurança local são armazenadas no registro em **HKEY_LOCAL_MACHINE \Security**. As informações armazenadas incluem configurações de política, valores de segurança padrão e informações de conta, como credenciais de logon armazenadas em cache. Uma cópia do banco de dados SAM também é armazenada aqui, embora esteja protegida contra gravação.

O diagrama a seguir mostra os componentes necessários e os caminhos que as credenciais assumem pelo sistema para autenticar o usuário ou o processo para um logon bem-sucedido.

![Diagrama que mostra os componentes necessários e os caminhos que as credenciais assumem pelo sistema para autenticar o usuário ou o processo para um logon bem-sucedido.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

A tabela a seguir descreve cada componente que gerencia as credenciais no processo de autenticação no ponto de logon.

**Componentes de autenticação para todos os sistemas**

|Componente|Descrição|
|-------|--------|
|Logon do usuário|O Winlogon. exe é o arquivo executável responsável pelo gerenciamento de interações de usuário seguras. O serviço Winlogon inicia o processo de logon para sistemas operacionais Windows, passando as credenciais coletadas por ação do usuário na área de trabalho segura (IU de logon) para a autoridade de segurança local (LSA) por meio de Secur32. dll.|
|Logon do aplicativo|Logons de aplicativo ou serviço que não exigem logon interativo. A maioria dos processos iniciados pelo usuário é executada no modo de usuário usando Secur32. dll, enquanto os processos iniciados na inicialização, como serviços, são executados no modo kernel usando Ksecdd. sys.<p>Para obter mais informações sobre o modo de usuário e o modo kernel, consulte aplicativos e modo de usuário ou serviços e modo kernel neste tópico.|
|Secur32.dll|Os vários provedores de autenticação que formam a base do processo de autenticação.|
|Lsasrv.dll|O serviço do servidor LSA, que impõe políticas de segurança e atua como o Gerenciador de pacotes de segurança para a LSA. A LSA contém a função Negotiate, que seleciona o protocolo NTLM ou Kerberos depois de determinar qual protocolo deve ser bem-sucedido.|
|Provedores de suporte de segurança|Um conjunto de provedores que pode invocar individualmente um ou mais protocolos de autenticação. O conjunto padrão de provedores pode ser alterado em cada versão do sistema operacional Windows, e os provedores personalizados podem ser gravados.|
|Netlogon.dll|Os serviços que o serviço de logon de rede executa são os seguintes:<p>-Mantém o canal seguro do computador (não deve ser confundido com Schannel) para um controlador de domínio.<br />– Passa as credenciais do usuário por meio de um canal seguro para o controlador de domínio e retorna os identificadores de segurança do domínio (SIDs) e os direitos de usuário para o usuário.<br />– Publica registros de recursos de serviço no DNS (sistema de nomes de domínio) e usa o DNS para resolver nomes para endereços IP (Internet Protocol) de controladores de domínio.<br />-Implementa o protocolo de replicação com base na chamada de procedimento remoto (RPC) para sincronizar controladores de domínio primários (PDCs) e controladores de domínio de backup (BDCs).|
|Samsrv. dll|O SAM (Gerenciador de contas de segurança), que armazena contas de segurança local, impõe políticas armazenadas localmente e dá suporte a APIs.|
|Registro|O registro contém uma cópia do banco de dados SAM, as configurações da política de segurança local, os valores de segurança padrão e as informações de conta que só podem ser acessadas pelo sistema.|

Esse tópico contém as seguintes seções:

-   [Entrada de credencial para logon do usuário](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credenciais para logon de aplicativo e serviço](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridade de segurança local](#BKMK_LSA)

-   [Validação e credenciais em cache](#BKMK_CachedCredentialsAndValidation)

-   [Armazenamento e validação de credenciais](#BKMK_CredentialStorageAndValidation)

-   [Banco de dados do Gerenciador de contas de segurança](#BKMK_SAM)

-   [Domínios locais e domínios confiáveis](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados na autenticação do Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="credential-input-for-user-logon"></a><a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credencial para logon do usuário
No Windows Server 2008 e no Windows Vista, a arquitetura de identificação gráfica e autenticação (GINA) foi substituída por um modelo de provedor de credenciais, o que tornou possível enumerar diferentes tipos de logon por meio do uso de blocos de logon. Ambos os modelos são descritos abaixo.

**Arquitetura de identificação gráfica e autenticação**

A arquitetura de identificação gráfica e autenticação (GINA) aplica-se aos sistemas operacionais Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. Nesses sistemas, cada sessão de logon interativa cria uma instância separada do serviço Winlogon. A arquitetura GINA é carregada no espaço de processo usado pelo Winlogon, recebe e processa as credenciais e faz as chamadas para as interfaces de autenticação por meio de LSALogonUser.

As instâncias do Winlogon para um logon interativo são executadas na sessão 0. A sessão 0 hospeda serviços do sistema e outros processos críticos, incluindo o processo da autoridade de segurança local (LSA).

O diagrama a seguir mostra o processo de credencial para o Windows Server 2003, o Microsoft Windows 2000 Server, o Windows XP e o Microsoft Windows 2000 Professional.

![Diagrama mostrando o processo de credencial para o Windows Server 2003, o Microsoft Windows 2000 Server, o Windows XP e o Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitetura do provedor de credenciais**

A arquitetura do provedor de credenciais se aplica a essas versões designadas na lista **aplica-se a** ao início deste tópico. Nesses sistemas, a arquitetura de entrada de credenciais mudou para um design extensível usando provedores de credenciais. Esses provedores são representados pelos diferentes blocos de logon na área de trabalho segura que permitem qualquer número de cenários de logon – contas diferentes para o mesmo usuário e métodos de autenticação diferentes, como senha, cartão inteligente e biometria.

Com a arquitetura do provedor de credenciais, o Winlogon sempre inicia a interface do usuário de logon depois de receber um evento de sequência de atenção segura. A interface do usuário de logon consulta cada provedor de credenciais para o número de tipos de credenciais diferentes que o provedor está configurado para enumerar. Os provedores de credenciais têm a opção de especificar um desses blocos como o padrão. Depois que todos os provedores tiverem enumerado seus blocos, a interface do usuário de logon os exibirá para o usuário. O usuário interage com um bloco para fornecer suas credenciais. A interface do usuário de logon envia essas credenciais para autenticação.

Os provedores de credenciais não são mecanismos de imposição. Eles são usados para reunir e serializar credenciais. A autoridade de segurança local e os pacotes de autenticação impõem a segurança.

Os provedores de credenciais são registrados no computador e são responsáveis pelo seguinte:

-   Descrever as informações de credenciais necessárias para a autenticação.

-   Tratamento de comunicação e lógica com autoridades de autenticação externas.

-   Pacotes de credenciais para logon interativo e de rede.

As credenciais de empacotamento para logon interativo e de rede incluem o processo de serialização. Ao serializar credenciais, vários blocos de logon podem ser exibidos na interface do usuário de logon. Portanto, sua organização pode controlar a exibição de logon, como usuários, sistemas de destino para logon, acesso pré-login à rede e políticas de bloqueio/desbloqueio de estação de trabalho-por meio do uso de provedores de credenciais personalizados. Vários provedores de credenciais podem coexistir no mesmo computador.

Provedores de logon único (SSO) podem ser desenvolvidos como um provedor de credenciais padrão ou como um provedor de acesso pré-logon.

Cada versão do Windows contém um provedor de credenciais padrão e um provedor de acesso pré-logon padrão (PLAP), também conhecido como provedor de SSO. O provedor de SSO permite que os usuários façam uma conexão com uma rede antes de fazer logon no computador local. Quando esse provedor é implementado, o provedor não enumera blocos na interface do usuário de logon.

Um provedor de SSO deve ser usado nos seguintes cenários:

-   **A autenticação de rede e o logon do computador são tratados por diferentes provedores de credenciais.** As variações para esse cenário incluem:

    -   Um usuário tem a opção de se conectar a uma rede, como conectar-se a uma VPN (rede virtual privada), antes de fazer logon no computador, mas não é necessário para fazer essa conexão.

    -   A autenticação de rede é necessária para recuperar informações usadas durante a autenticação interativa no computador local.

    -   Várias autenticações de rede são seguidas por um dos outros cenários. Por exemplo, um usuário é autenticado em um provedor de serviços de Internet (ISP), é autenticado em uma VPN e, em seguida, usa suas credenciais de conta de usuário para fazer logon localmente.

    -   As credenciais armazenadas em cache são desabilitadas e uma conexão de serviços de acesso remoto por meio de VPN é necessária antes do logon local para autenticar o usuário.

    -   Um usuário de domínio não tem uma conta local configurada em um computador ingressado no domínio e deve estabelecer uma conexão de serviços de acesso remoto por meio da conexão VPN antes de concluir o logon interativo.

-   **A autenticação de rede e o logon do computador são tratados pelo mesmo provedor de credenciais.** Nesse cenário, o usuário é solicitado a se conectar à rede antes de fazer logon no computador.

**Enumeração de bloco de logon**

O provedor de credenciais enumera os blocos de logon nas seguintes instâncias:

-   Para os sistemas operacionais designados na lista **aplica-se a** no início deste tópico.

-   O provedor de credenciais enumera os blocos para o logon da estação de trabalho. O provedor de credenciais normalmente serializa as credenciais de autenticação para a autoridade de segurança local. Esse processo exibe blocos específicos para cada usuário e específicos para os sistemas de destino de cada usuário.

-   A arquitetura de logon e autenticação permite que um usuário use blocos enumerados pelo provedor de credenciais para desbloquear uma estação de trabalho. Normalmente, o usuário conectado no momento é o bloco padrão, mas se mais de um usuário estiver conectado, vários blocos serão exibidos.

-   O provedor de credenciais enumera blocos em resposta a uma solicitação de usuário para alterar sua senha ou outras informações particulares, como um PIN. Normalmente, o usuário conectado no momento é o bloco padrão; no entanto, se mais de um usuário estiver conectado, vários blocos serão exibidos.

-   O provedor de credenciais enumera blocos com base nas credenciais serializadas a serem usadas para autenticação em computadores remotos. A interface do usuário da credencial não usa a mesma instância do provedor que a interface do usuário de logon, desbloquear a estação de trabalho ou alterar a senha. Portanto, as informações de estado não podem ser mantidas no provedor entre instâncias da interface do usuário da credencial. Essa estrutura resulta em um bloco para cada logon de computador remoto, supondo que as credenciais tenham sido corretamente serializadas. Esse cenário também é usado no UAC (controle de conta de usuário), que pode ajudar a impedir alterações não autorizadas em um computador solicitando permissão do usuário ou uma senha de administrador antes de permitir ações que possam afetar a operação do computador ou que podem alterar as configurações que afetam outros usuários do computador.

O diagrama a seguir mostra o processo de credencial para os sistemas operacionais designados na lista **aplica-se a** no início deste tópico.

![Diagrama que mostra o processo de credencial para os sistemas operacionais designados na lista * * aplica-se a * * no início deste tópico](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="credential-input-for-application-and-service-logon"></a><a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credenciais para logon de aplicativo e serviço
A autenticação do Windows foi projetada para gerenciar credenciais para aplicativos ou serviços que não exigem interação do usuário. Os aplicativos no modo de usuário são limitados em termos de quais recursos do sistema eles têm acesso, enquanto os serviços podem ter acesso irrestrito à memória do sistema e aos dispositivos externos.

Os serviços do sistema e os aplicativos de nível de transporte acessam um SSP (provedor de suporte de segurança) por meio da SSPI (Security Support Provider interface) no Windows, que fornece funções para enumerar os pacotes de segurança disponíveis em um sistema, selecionar um pacote e usar esse pacote para obter uma conexão autenticada.

Quando uma conexão de cliente/servidor é autenticada:

-   O aplicativo no lado do cliente da conexão envia credenciais para o servidor usando a função SSPI `InitializeSecurityContext (General)`.

-   O aplicativo no lado do servidor da conexão responde com a função SSPI `AcceptSecurityContext (General)`.

-   As funções SSPI `InitializeSecurityContext (General)` e `AcceptSecurityContext (General)` são repetidas até que todas as mensagens de autenticação necessárias tenham sido trocadas para autenticação bem-sucedida ou falha.

-   Depois que a conexão for autenticada, a LSA no servidor usará as informações do cliente para criar o contexto de segurança, que contém um token de acesso.

-   O servidor pode então chamar a função SSPI `ImpersonateSecurityContext` para anexar o token de acesso a um thread de representação para o serviço.

**Aplicativos e modo de usuário**

O modo de usuário no Windows é composto por dois sistemas capazes de passar solicitações de e/s para os drivers de modo kernel apropriados: o sistema de ambiente, que executa aplicativos escritos para muitos tipos diferentes de sistemas operacionais, e o sistema integral, que opera funções específicas do sistema em nome do sistema de ambiente.

O sistema integral gerencia as funções system'specific operacionais em nome do sistema de ambiente e consiste em um processo do sistema de segurança (a LSA), um serviço de estação de trabalho e um serviço de servidor. O processo do sistema de segurança lida com tokens de segurança, concede ou nega permissões para acessar contas de usuário com base em permissões de recurso, lida com solicitações de logon e inicia a autenticação de logon e determina quais recursos do sistema o sistema operacional precisa auditar.

Os aplicativos podem ser executados no modo de usuário em que o aplicativo pode ser executado como qualquer entidade, incluindo no contexto de segurança do sistema local (sistema). Os aplicativos também podem ser executados no modo kernel, no qual o aplicativo pode ser executado no contexto de segurança do sistema local (sistema).

O SSPI está disponível por meio do módulo Secur32. dll, que é uma API usada para obter serviços de segurança integrados para autenticação, integridade da mensagem e privacidade da mensagem. Ele fornece uma camada de abstração entre protocolos de nível de aplicativo e protocolos de segurança. Como aplicativos diferentes exigem diferentes maneiras de identificar ou autenticar usuários e diferentes maneiras de criptografar dados conforme eles trafegam em uma rede, o SSPI fornece uma maneira de acessar DLLs (bibliotecas de vínculo dinâmico) que contêm funções de autenticação e criptografia diferentes. Essas DLLs são chamadas de SSPs (provedores de suporte de segurança).

Contas de serviço gerenciado e contas virtuais foram introduzidas no Windows Server 2008 R2 e no Windows 7 para fornecer aplicativos cruciais, como Microsoft SQL Server e Serviços de Informações da Internet (IIS), com o isolamento de suas próprias contas de domínio, ao mesmo tempo que elimina a necessidade de um administrador administrar manualmente o SPN (nome da entidade de serviço) e as credenciais para essas contas. Para obter mais informações sobre esses recursos e sua função na autenticação, consulte a [documentação contas de serviço gerenciado para o Windows 7 e o Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) e [visão geral das contas de serviço gerenciado de grupo](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Serviços e modo kernel**

Embora a maioria dos aplicativos do Windows seja executada no contexto de segurança do usuário que os inicia, isso não é verdade para os serviços. Muitos serviços do Windows, como serviços de rede e impressão, são iniciados pelo controlador de serviço quando o usuário inicia o computador. Esses serviços podem ser executados como serviço local ou sistema local e podem continuar sendo executados depois que o último usuário humano fizer logoff.

> [!NOTE]
> Normalmente, os serviços são executados em contextos de segurança conhecidos como sistema local (sistema), serviço de rede ou serviço local.  O Windows Server 2008 R2 introduziu serviços que são executados em uma conta de serviço gerenciado, que são entidades de domínio.

Antes de iniciar um serviço, o controlador de serviço faz logon usando a conta designada para o serviço e, em seguida, apresenta as credenciais do serviço para autenticação pelo LSA. O serviço do Windows implementa uma interface programática que o Gerenciador do controlador de serviço pode usar para controlar o serviço. Um serviço do Windows pode ser iniciado automaticamente quando o sistema é iniciado ou manualmente com um programa de controle de serviço. Por exemplo, quando um computador cliente Windows ingressa em um domínio, o serviço de mensageiro no computador se conecta a um controlador de domínio e abre um canal seguro para ele. Para obter uma conexão autenticada, o serviço deve ter credenciais que a LSA (autoridade de segurança local) do computador remoto confie. Ao se comunicar com outros computadores na rede, a LSA usa as credenciais da conta de domínio do computador local, assim como todos os outros serviços em execução no contexto de segurança do sistema local e do serviço de rede. Os serviços no computador local são executados como sistema, portanto, as credenciais não precisam ser apresentadas à LSA.

O arquivo Ksecdd. sys gerencia e criptografa essas credenciais e usa uma chamada de procedimento local para o LSA. O tipo de arquivo é DRV (driver) e é conhecido como o SSP (provedor de suporte de segurança) no modo kernel e, nessas versões designadas na lista **aplica-se a ao** início deste tópico, é compatível com o FIPS 140-2 nível 1.

O modo kernel tem acesso completo aos recursos de hardware e sistema do computador. O modo kernel interrompe os serviços de modo de usuário e os aplicativos de acessar áreas críticas do sistema operacional para as quais eles não devem ter acesso.

## <a name="local-security-authority"></a><a name="BKMK_LSA"></a>Autoridade de segurança local
A autoridade de segurança local (LSA) é um processo do sistema protegido que autentica e registra os usuários no computador local. Além disso, a LSA mantém informações sobre todos os aspectos da segurança local em um computador (esses aspectos são conhecidos coletivamente como a política de segurança local) e fornece vários serviços para conversão entre nomes e SIDs (identificadores de segurança). O processo do sistema de segurança, o serviço de servidor de autoridade de segurança local (LSASs), controla as políticas de segurança e as contas que estão em vigor em um sistema de computador.

A LSA valida a identidade de um usuário com base em quais das duas entidades a seguir emitiram a conta do usuário:

-   **Autoridade de segurança local.** A LSA pode validar as informações do usuário verificando o banco de dados SAM (Gerenciador de contas de segurança) localizado no mesmo computador. Qualquer estação de trabalho ou servidor membro pode armazenar contas de usuário local e informações sobre grupos locais. No entanto, essas contas podem ser usadas para acessar apenas a estação de trabalho ou o computador.

-   **Autoridade de segurança para o domínio local ou para um domínio confiável.** A LSA entra em contato com a entidade que emitiu a conta e solicita a verificação de que a conta é válida e que a solicitação foi originada do titular da conta.

O serviço LSASS armazena credenciais na memória em nome de usuários com sessões ativas do Windows. As credenciais armazenadas permitem que os usuários acessem diretamente os recursos da rede, como compartilhamentos de arquivos, caixas de correio do Exchange Server e sites do SharePoint, sem digitar novamente suas credenciais para cada serviço remoto.

O LSASS pode armazenar credenciais de várias formas, incluindo:

-   Texto não criptografado de criptografia reversível

-   Tíquetes Kerberos (tíquetes de concessão de tíquetes (TGTs), tíquetes de serviço)

-   Hash de NT

-   Hash do LM (LAN Manager)

Se o usuário fizer logon no Windows usando um cartão inteligente, o LSASs não armazenará uma senha de texto não criptografado, mas armazenará o valor de hash NT correspondente para a conta e o PIN de texto não criptografado para o cartão inteligente. Se o atributo da conta estiver habilitado para um cartão inteligente exigido no logon interativo, um valor de hash de NT aleatório será gerado automaticamente para a conta, e não o hash da senha original. O hash de senha gerado automaticamente quando o atributo é definido não muda.

Se um usuário fizer logon em um computador baseado no Windows com uma senha compatível com os hashes do LM (LAN Manager), esse autenticador estará presente na memória.

O armazenamento de credenciais em texto não criptografado na memória não pode ser desabilitado, mesmo que os provedores de credenciais que necessitam delas estejam desabilitados.

As credenciais armazenadas são diretamente associadas às sessões de logon do serviço LSASS (LSASs) que foram iniciadas após a última reinicialização e não foram fechadas. Por exemplo, as sessões de LSA com credenciais LSA armazenadas são criadas quando um usuário:

-   Faz logon em uma sessão local ou sessão de protocolo RDP (RDP) no computador

-   Executa uma tarefa usando a opção **RunAs**

-   Executa um serviço Windows ativo no computador

-   Executa uma tarefa ou trabalho em lotes programado

-   Executa uma tarefa no computador local usando uma ferramenta de administração remota

Em algumas circunstâncias, os segredos de LSA, que são partes secretas de dados que são acessíveis apenas aos processos de conta do sistema, são armazenados na unidade de disco rígido. Alguns desses segredos são credenciais que devem persistir após a reinicialização, e são armazenadas de forma criptografada no disco rígido. Credenciais armazenadas como segredos de LSA podem incluir:

-   Senha da conta para a conta de Active Directory Domain Services do computador (AD DS)

-   Senhas da conta de serviços Windows que estão configurados no computador

-   Senhas da conta para tarefas agendadas configuradas

-   Senhas da conta de pools de aplicativos IIS e sites da Web

-   Senhas para contas da Microsoft

Introduzido no Windows 8.1, o sistema operacional do cliente fornece proteção adicional para a LSA para evitar a leitura de memória e injeção de código por processos não protegidos. Essa proteção aumenta a segurança das credenciais que o LSA armazena e gerencia.

Para obter mais informações sobre essas proteções adicionais, consulte [Configurando a proteção de LSA adicional](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="cached-credentials-and-validation"></a><a name="BKMK_CachedCredentialsAndValidation"></a>Validação e credenciais em cache
Os mecanismos de validação dependem da apresentação das credenciais no momento do logon. No entanto, quando o computador está desconectado de um controlador de domínio e o usuário está apresentando credenciais de domínio, o Windows usa o processo de credenciais armazenadas em cache no mecanismo de validação.

Cada vez que um usuário faz logon em um domínio, o Windows armazena em cache as credenciais fornecidas e as armazena no hive de segurança no registro do sistema operacional.

Com as credenciais armazenadas em cache, o usuário pode fazer logon em um membro do domínio sem estar conectado a um controlador de domínio dentro desse domínio.


## <a name="credential-storage-and-validation"></a><a name="BKMK_CredentialStorageAndValidation"></a>Armazenamento e validação de credenciais
Nem sempre é desejável usar um conjunto de credenciais para acesso a diferentes recursos. Por exemplo, um administrador pode querer usar credenciais administrativas em vez de usuário ao acessar um servidor remoto. Da mesma forma, se um usuário acessar recursos externos, como uma conta bancária, ele poderá usar apenas as credenciais que forem diferentes das suas credenciais de domínio. As seções a seguir descrevem as diferenças no gerenciamento de credenciais entre as versões atuais dos sistemas operacionais Windows e os sistemas operacionais Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processos de credenciais de logon remoto
O protocolo RDP (RDP) gerencia as credenciais do usuário que se conecta a um computador remoto usando o cliente Área de Trabalho Remota, que foi introduzido no Windows 8. As credenciais em formato de texto sem formatação são enviadas para o host de destino onde o host tenta executar o processo de autenticação e, se for bem-sucedido, conecta o usuário aos recursos permitidos. O RDP não armazena as credenciais no cliente, mas as credenciais de domínio do usuário são armazenadas no LSASs.

Introduzido no Windows Server 2012 R2 e Windows 8.1, o modo de administrador restrito fornece segurança adicional para cenários de logon remoto. Esse modo de Área de Trabalho Remota faz com que o aplicativo cliente execute um desafio de logon de rede – resposta com a função unidirecional do NT (NTOWF) ou use um tíquete de serviço Kerberos ao autenticar no host remoto. Depois que o administrador é autenticado, o administrador não tem as credenciais de conta respectivas no LSASs porque elas não foram fornecidas ao host remoto. Em vez disso, o administrador tem as credenciais de conta de computador para a sessão. As credenciais de administrador não são fornecidas ao host remoto, portanto, as ações são executadas como a conta do computador. Os recursos também são limitados à conta do computador, e o administrador não pode acessar recursos com sua própria conta.

### <a name="automatic-restart-sign-on-credential-process"></a>Processo de credencial de logon automático de reinicialização
Quando um usuário entra em um dispositivo Windows 8.1, o LSA salva as credenciais do usuário na memória criptografada que são acessíveis somente pelo LSASs. exe. Quando Windows Update inicia uma reinicialização automática sem a presença do usuário, essas credenciais são usadas para configurar o logon automático para o usuário.

Ao reiniciar, o usuário é conectado automaticamente por meio do mecanismo de logon automático e, em seguida, o computador é bloqueado adicionalmente para proteger a sessão do usuário. O bloqueio é iniciado por meio do Winlogon, enquanto o gerenciamento de credenciais é feito pela LSA. Ao entrar automaticamente e bloquear a sessão do usuário no console do, os aplicativos da tela de bloqueio do usuário são reiniciados e disponibilizados.

Para obter mais informações sobre ARSO, consulte [ &#40;ARSO&#41;de logon automático de reinício do Winlogon](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Nomes de usuário e senhas armazenados no Windows Vista e no Windows XP
No Windows Server 2008, no Windows Server 2003, no Windows Vista e no Windows XP, os **nomes de usuário e as senhas armazenados** no painel de controle simplificam o gerenciamento e o uso de vários conjuntos de credenciais de logon, incluindo certificados X. 509 usados com cartões inteligentes e credenciais do Windows Live (agora chamado de conta Microsoft). As credenciais-parte do perfil do usuário – são armazenadas até que sejam necessárias. Essa ação pode aumentar a segurança por recurso, garantindo que, se uma senha for comprometida, ela não compromete toda a segurança.

Depois que um usuário faz logon e tenta acessar recursos adicionais protegidos por senha, como um compartilhamento em um servidor, e se as credenciais de logon padrão do usuário não forem suficientes para obter acesso, os **nomes de usuário e as senhas armazenados** serão consultados. Se credenciais alternativas com as informações de logon corretas tiverem sido salvas em **nomes de usuário e senhas armazenados**, essas credenciais serão usadas para obter acesso. Caso contrário, o usuário receberá uma solicitação para fornecer novas credenciais, que podem ser salvas para reutilização, posteriormente na sessão de logon ou durante uma sessão subsequente.

As seguintes restrições se aplicam:

-   Se os **nomes de usuário e as senhas armazenados** contiverem credenciais inválidas ou incorretas para um recurso específico, o acesso ao recurso será negado e a caixa de diálogo **nomes de usuário e senhas armazenados** não será exibida.

-   **As senhas e nomes de usuário armazenados** armazenam credenciais somente para NTLM, protocolo Kerberos, conta Microsoft (anteriormente chamado de Windows Live ID) e autenticação de protocolo SSL (SSL). Algumas versões do Internet Explorer mantêm seu próprio cache para autenticação básica.

Essas credenciais se tornam uma parte criptografada do perfil local de um usuário no diretório \Documents and Settings\Username\Application Data\Microsoft\Credentials. Como resultado, essas credenciais podem fazer roaming com o usuário se a política de rede do usuário oferecer suporte a perfis de usuário móveis. No entanto, se o usuário tiver cópias de **nomes de usuário e senhas armazenados** em dois computadores diferentes e alterar as credenciais associadas ao recurso em um desses computadores, a alteração não será propagada para **nomes de usuário e senhas armazenados** no segundo computador.

### <a name="windows-vault-and-credential-manager"></a>Cofre do Windows e Gerenciador de credenciais
O Credential Manager foi introduzido no Windows Server 2008 R2 e no Windows 7 como um recurso do painel de controle para armazenar e gerenciar nomes de usuário e senhas. O Gerenciador de credenciais permite que os usuários armazenem credenciais relevantes para outros sistemas e sites no cofre seguro do Windows. Algumas versões do Internet Explorer usam esse recurso para autenticação em sites.

O gerenciamento de credenciais executado pelo Gerenciador de Credenciais é controlado pelo usuário no computador local. Os usuários podem salvar e armazenar credenciais de navegadores e aplicativos do Windows com suporte para ter conveniência ao entrar nesses recursos. As credenciais são salvas em pastas criptografadas especiais no computador sob o perfil do usuário. Os aplicativos que dão suporte a esse recurso (com o uso das APIs do Gerenciador de credenciais), como navegadores da Web e aplicativos, podem apresentar as credenciais corretas a outros computadores e sites durante o processo de logon.

Quando um site, um aplicativo ou outro computador solicita autenticação por meio de NTLM ou do protocolo Kerberos, uma caixa de diálogo é exibida na qual você marca a caixa de seleção **Atualizar credenciais padrão** ou **salvar senha** . Essa caixa de diálogo que permite que um usuário salve as credenciais localmente é gerada por um aplicativo que dá suporte às APIs do Credential Manager. Se o usuário marcar a caixa de seleção **salvar senha** , o Gerenciador de credenciais acompanhará o nome de usuário, a senha e as informações relacionadas do usuário para o serviço de autenticação que está em uso.

Na próxima vez em que o serviço for usado, o Gerenciador de credenciais fornecerá automaticamente a credencial armazenada no cofre do Windows. Se a credencial não for aceita, o usuário será solicitado a fornecer as informações de acesso corretas. Se o acesso for concedido com as novas credenciais, o Gerenciador de credenciais substituirá a credencial anterior por uma nova e, em seguida, armazenará a nova credencial no cofre do Windows.

## <a name="security-accounts-manager-database"></a><a name="BKMK_SAM"></a>Banco de dados do Gerenciador de contas de segurança
O SAM (Gerenciador de contas de segurança) é um banco de dados que armazena contas de usuário e grupos locais. Ele está presente em todos os sistemas operacionais Windows; no entanto, quando um computador é ingressado em um domínio, o Active Directory gerencia contas de domínio em domínios Active Directory.

Por exemplo, os computadores cliente que executam um sistema operacional Windows participam de um domínio de rede comunicando-se com um controlador de domínio, mesmo quando nenhum usuário humano está conectado. Para iniciar as comunicações, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a LSA no controlador de domínio autentica a identidade do computador e, em seguida, constrói o contexto de segurança do computador da mesma forma que faz para uma entidade de segurança humana. Esse contexto de segurança define a identidade e os recursos de um usuário ou serviço em um computador específico ou um usuário, serviço ou computador em uma rede. Por exemplo, o token de acesso contido no contexto de segurança define os recursos (como um compartilhamento de arquivos ou impressora) que podem ser acessados e as ações (como leitura, gravação ou modificação) que podem ser executadas por essa entidade de segurança – um usuário, computador ou serviço nesse recurso.

O contexto de segurança de um usuário ou computador pode variar de um computador para outro, como quando um usuário faz logon em um servidor ou em uma estação de trabalho que não seja a própria estação de trabalho principal do usuário. Ele também pode variar de uma sessão para outra, como quando um administrador modifica os direitos e permissões do usuário. Além disso, o contexto de segurança geralmente é diferente quando um usuário ou computador está operando em uma base autônoma, em uma rede ou como parte de um domínio de Active Directory.

## <a name="local-domains-and-trusted-domains"></a><a name="BKMK_LocalDomainsAndTrustedDomains"></a>Domínios locais e domínios confiáveis
Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio dependem da validade das autenticações provenientes do outro domínio. As relações de confiança ajudam a fornecer acesso controlado a recursos compartilhados em um domínio de recurso (o domínio confiante), verificando se as solicitações de autenticação de entrada são provenientes de uma autoridade confiável (o domínio confiável). Dessa forma, as relações de confiança agem como pontes que permitem que apenas solicitações de autenticação validadas percorram entre domínios.

Como uma relação de confiança específica passa solicitações de autenticação depende de como ela está configurada. As relações de confiança podem ser unidirecionais, fornecendo acesso do domínio confiável a recursos no domínio confiante, ou bidirecional, fornecendo acesso de cada domínio aos recursos no outro domínio. As relações de confiança também são intransitivas; nesse caso, uma relação de confiança existe somente entre os dois domínios de parceiro confiável, ou transitiva, caso em que uma relação de confiança se estende automaticamente para quaisquer outros domínios nos quais os parceiros confiam.

Para obter informações sobre relações de confiança de domínio e floresta referentes à autenticação, consulte [autenticação delegada e relações de confiança](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="certificates-in-windows-authentication"></a><a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados na autenticação do Windows
Uma PKI (infraestrutura de chave pública) é a combinação de software, tecnologias de criptografia, processos e serviços que permitem que uma organização proteja suas comunicações e transações de negócios. A capacidade de uma PKI de proteger comunicações e transações de negócios é baseada na troca de certificados digitais entre usuários autenticados e recursos confiáveis.

Um certificado digital é um documento eletrônico que contém informações sobre a entidade à qual ele pertence, a entidade que foi emitida, um número de série exclusivo ou alguma outra identificação exclusiva, datas de emissão e de validade e uma impressão digital.

A autenticação é o processo de determinar se um host remoto pode ser confiável. Para estabelecer sua confiabilidade, o host remoto deve fornecer um certificado de autenticação aceitável.

Os hosts remotos estabelecem sua confiabilidade obtendo um certificado de uma autoridade de certificação (CA). A CA pode, por sua vez, ter certificação de uma autoridade superior, que cria uma cadeia de confiança. Para determinar se um certificado é confiável, um aplicativo deve determinar a identidade da autoridade de certificação raiz e, em seguida, determinar se é confiável.

Da mesma forma, o host remoto ou o computador local deve determinar se o certificado apresentado pelo usuário ou aplicativo é autêntico. O certificado apresentado pelo usuário por meio de LSA e SSPI é avaliado quanto à autenticidade no computador local para logon local, na rede ou no domínio por meio dos repositórios de certificados no Active Directory.

Para produzir um certificado, os dados de autenticação passam por algoritmos de hash, como Secure Hash Algorithm 1 (SHA1), para produzir um resumo de mensagem. O resumo da mensagem é assinado digitalmente usando a chave privada do remetente para provar que o resumo da mensagem foi produzido pelo remetente.

> [!NOTE]
> SHA1 é o padrão no Windows 7 e no Windows Vista, mas foi alterado para SHA2 no Windows 8.

**Autenticação de cartão inteligente**

A tecnologia de cartão inteligente é um exemplo de autenticação baseada em certificado. O logon em uma rede com um cartão inteligente fornece uma forma forte de autenticação, pois usa a identificação baseada em criptografia e a prova de posse ao autenticar um usuário em um domínio. Os serviços de certificado Active Directory (AD CS) fornecem a identificação baseada em criptografia por meio da emissão de um certificado de logon para cada cartão inteligente.

Para obter informações sobre a autenticação de cartão inteligente, consulte a [referência técnica do cartão inteligente do Windows](https://technet.microsoft.com/library/ff404297.aspx).

A tecnologia de cartão inteligente virtual foi introduzida no Windows 8. Ele armazena o certificado do cartão inteligente no PC e o protege usando o chip de segurança do TPM (prova de adulteração Trusted Platform Module) do dispositivo. Dessa forma, o PC realmente se torna o cartão inteligente que deve receber o PIN do usuário para ser autenticado.

**Autenticação remota e sem fio**

A autenticação de rede remota e sem fio é outra tecnologia que usa certificados para autenticação. O IAS (serviço de autenticação da Internet) e os servidores de rede virtual privada usam EAP-TLS (protocolo de autenticação extensível), PEAP (protocolo de autenticação extensível protegida) ou IPsec (segurança de protocolo IP) para executar a autenticação baseada em certificado para muitos tipos de acesso à rede, incluindo VPN (rede virtual privada) e conexões sem fio.

Para obter informações sobre a autenticação baseada em certificado em rede, consulte [autenticação e certificados de acesso à rede](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="see-also"></a><a name="BKMK_SeeAlso"></a>Consulte também
[Conceitos de autenticação do Windows](https://docs.microsoft.com/windows-server/security/windows-authentication/windows-authentication-concepts)


