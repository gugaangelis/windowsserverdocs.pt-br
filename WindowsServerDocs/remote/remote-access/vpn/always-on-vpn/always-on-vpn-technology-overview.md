---
title: Visão geral da tecnologia de VPN Always On
description: 'Esta página fornece uma breve visão geral das tecnologias de VPN Always On com links para documentos detalhados. '
ms.topic: article
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.openlocfilehash: 155d06657811878464d905a51f249cbc8ad3cfe2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958284"
---
# <a name="always-on-vpn-technology-overview"></a>Visão geral da tecnologia de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre os aprimoramentos de VPN Always On](always-on-vpn-enhancements.md)
- [**Em seguida:** Saiba mais sobre os recursos avançados do Always On VPN](deploy/always-on-vpn-adv-options.md)

Para essa implantação, você deve instalar um novo servidor de acesso remoto que esteja executando o Windows Server 2016, bem como modificar parte de sua infraestrutura existente para a implantação.

A ilustração a seguir mostra a infraestrutura necessária para implantar Always On VPN.

![Infraestrutura de VPN Always On](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

O processo de conexão descrito nesta ilustração é composto pelas seguintes etapas:

1. Usando servidores DNS públicos, o cliente de VPN do Windows 10 executa uma consulta de resolução de nomes para o endereço IP do gateway de VPN.

2. Usando o endereço IP retornado pelo DNS, o cliente VPN envia uma solicitação de conexão para o gateway de VPN.

3. O gateway de VPN também é configurado como um cliente serviço RADIUS (RADIUS); o cliente RADIUS de VPN envia a solicitação de conexão para o servidor NPS corporativo/de empresa para o processamento de solicitação de conexão.

4. O servidor NPS processa a solicitação de conexão, incluindo a execução de autorização e autenticação, e determina se a solicitação de conexão deve ser permitida ou negada.

5. O servidor NPS encaminha uma resposta Access-Accept ou Access-Deny para o gateway de VPN.

6. A conexão é iniciada ou encerrada com base na resposta que o servidor VPN recebeu do servidor NPS.

Para obter mais informações sobre cada componente de infraestrutura descrito na ilustração acima, consulte as seções a seguir.

>[!NOTE]
>Se você já tiver algumas dessas tecnologias implantadas em sua rede, poderá usar as instruções neste guia de implantação para executar uma configuração adicional das tecnologias para essa finalidade de implantação.

## <a name="domain-name-system-dns"></a>Sistema de nome de domínio (DNS)

As zonas de DNS (sistema de nomes de domínio) internas e externas são necessárias, o que pressupõe que a zona interna é um subdomínio delegado da zona externa (por exemplo, corp.contoso.com e contoso.com).

Saiba mais sobre o [DNS (sistema de nomes de domínio)](../../../../networking/dns/dns-top.md) ou o [Guia de rede principal](../../../../networking/core-network-guide/core-network-guide.md).

>[!NOTE]
>Outros designs de DNS, como o DNS de divisão-cérebro (usando o mesmo nome de domínio interna e externamente em zonas DNS separadas) ou domínios internos e externos não relacionados (por exemplo, contoso. local e contoso.com) também são possíveis. Para obter mais informações sobre como implantar o DNS de divisão-Brain, consulte [usar a política DNS para implantação de DNS de divisão-Brain](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewalls

Verifique se os firewalls permitem o tráfego necessário para que as comunicações VPN e RADIUS funcionem corretamente.

Para obter mais informações, consulte [Configurar firewalls para tráfego RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Acesso remoto como um servidor VPN de gateway RAS

No Windows Server 2016, a função de servidor de acesso remoto foi projetada para funcionar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos. Para essas diretrizes de implantação, você precisa apenas de um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de LAN.

IKEv2 é um protocolo de encapsulamento de VPN descrito em solicitação de força de tarefa de engenharia da Internet para comentários 7296. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for temporariamente perdida ou se um usuário mover um computador cliente de uma rede para outra, o IKEv2 restaurará automaticamente a conexão VPN quando a conexão de rede for restabelecida, tudo isso sem a intervenção do usuário.

Usando o gateway RAS, você pode implantar conexões VPN para fornecer aos usuários finais acesso remoto à rede e aos recursos da sua organização. A implantação de Always On VPN mantém uma conexão persistente entre os clientes e a rede da sua organização sempre que os computadores remotos estiverem conectados à Internet. Com o gateway RAS, você também pode criar uma conexão VPN site a site entre dois servidores em locais diferentes, como entre o escritório primário e uma filial, e usar NAT (conversão de endereços de rede) para que os usuários dentro da rede possam acessar recursos externos, como a Internet. Além disso, o gateway RAS dá suporte a Border Gateway Protocol (BGP), que fornece serviços de roteamento dinâmico quando seus locais de escritório remotos também têm gateways de borda com suporte para BGP.

Você pode gerenciar gateways do serviço de acesso remoto (RAS) usando comandos do Windows PowerShell e o MMC (console de gerenciamento Microsoft) de acesso remoto.

## <a name="network-policy-server-nps"></a>NPS (Servidor de Políticas de Rede)

O NPS permite que você crie e aplique políticas de acesso à rede em toda a organização para autenticação e autorização de solicitação de conexão. Ao usar o NPS como um servidor serviço RADIUS (RADIUS), você configura servidores de acesso à rede, como servidores VPN, como clientes RADIUS no NPS.

Você também configura as políticas de rede que o NPS usa para autorizar solicitações de conexão. Você pode configurar a contabilização RADIUS para que o NPS registre as informações de contabilização em arquivos de log no disco rígido local ou em um banco de dados do Microsoft SQL Server.

Para obter mais informações, consulte [Servidor de Políticas de Rede (NPS)](../../../../networking/technologies/nps/nps-top.md).

## <a name="active-directory-certificate-services"></a>Serviços de Certificados do Active Directory

O servidor de autoridade de certificação (CA) é uma autoridade de certificação que está executando Active Directory serviços de certificados. A configuração de VPN requer uma PKI (infraestrutura de chave pública) baseada em Active Directory.

As organizações podem usar o AD CS para aumentar a segurança ligando a identidade de uma pessoa, dispositivo ou serviço a uma chave pública correspondente. O AD CS também inclui recursos que permitem que você gerencie o registro e a revogação de certificados em uma variedade de ambientes escalonáveis. Para obter mais informações, consulte [visão geral dos serviços de certificados Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11)) e [diretrizes de design de infraestrutura de chave pública](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953).

Durante a conclusão da implantação, você configurará os seguintes modelos de certificado na autoridade de certificação.

- O modelo de certificado de autenticação de usuário

- O modelo de certificado de autenticação do servidor VPN

- O modelo de certificado de autenticação do servidor NPS

### <a name="certificate-templates"></a>Modelos de certificado

Os modelos de certificado podem simplificar muito a tarefa de administrar uma autoridade de certificação (CA), permitindo que você emita certificados pré-configurados para as tarefas selecionadas. O snap-in MMC de modelos de certificado permite que você execute as seguintes tarefas.

- Exiba as propriedades de cada modelo de certificado.

- Copiar e modificar modelos de certificado.

- Controlar quais usuários e computadores podem ler modelos e se registrar para certificados.

- Execute outras tarefas administrativas relacionadas aos modelos de certificado.

Os modelos de certificado são parte integrante de uma autoridade de certificação (CA) corporativa. Eles são um elemento importante da política de certificado para um ambiente, que é o conjunto de regras e formatos para registro, uso e gerenciamento de certificados.

Para obter mais informações, consulte [modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11)).

### <a name="digital-server-certificates"></a>Certificados do servidor digital

Esta orientação de implantação fornece instruções para usar o AD CS (serviços de certificados Active Directory) para registrar e registrar automaticamente certificados para servidores de acesso remoto e de infraestrutura de NPS. O AD CS permite criar uma PKI (infraestrutura de chave pública) e fornecer criptografia de chave pública, certificados digitais e recursos de assinatura digital para sua organização.

Quando você usa certificados de servidor digital para autenticação entre computadores em sua rede, os certificados fornecem:

1. Confidencialidade por meio de criptografia.

2. Integridade por meio de assinaturas digitais.

3. Autenticação associando chaves de certificado a um computador, usuário ou contas de dispositivo em uma rede de computador.

Para obter mais informações, consulte [visão geral dos serviços de certificados Active Directory](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11)).

## <a name="active-directory-domain-services-ad-ds"></a>Active Directory Domain Services (AD DS)

O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos da rede e dados específicos de aplicativos habilitados por diretório. Os administradores podem usar AD DS para organizar elementos de uma rede, como usuários, computadores e outros dispositivos, em uma estrutura de confinamento hierárquica. A estrutura de confinamento hierárquica inclui a floresta do Active Directory, domínios na floresta e unidades organizacionais (OUs) em cada domínio. Um servidor com AD DS é chamado de controlador de domínio.

AD DS contém as contas de usuário, contas de computador e propriedades de conta que são exigidas pelo protocolo PEAP para autenticar as credenciais do usuário e avaliar a autorização para solicitações de conexão VPN. Para obter informações sobre como implantar AD DS, consulte o guia de [rede](../../../../networking/core-network-guide/Core-Network-Guide.md)do Windows Server 2016 Core.

Durante a conclusão das etapas nesta implantação, você configurará os itens a seguir no controlador de domínio.

- Habilitar o registro automático de certificado no Política de Grupo para computadores e usuários

- Criar o grupo de usuários VPN

- Criar o grupo de servidores VPN

- Criar o grupo de servidores NPS

### <a name="active-directory-users-and-computers"></a>Usuários e computadores do Active Directory

Active Directory usuários e computadores é um componente do AD DS que contém contas que representam entidades físicas, como um computador, uma pessoa ou um grupo de segurança. Um grupo de segurança é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. As contas de usuário e computador que pertencem a um grupo específico são chamadas de membros do grupo.

As contas de usuário no Active Directory usuários e computadores têm propriedades de discagem que o NPS avalia durante o processo de autorização, a menos que a propriedade de **permissão de acesso à rede** da conta de usuário esteja definida para controlar o **acesso por meio da política de rede do NPS**. Essa é a configuração padrão para todas as contas de usuário. Em alguns casos, no entanto, essa configuração pode ter uma configuração diferente que impede que o usuário se conecte usando VPN. Para se proteger contra essa possibilidade, você pode configurar o servidor NPS para ignorar as propriedades de discagem da conta de usuário.

Para obter mais informações, consulte [Configurar o NPS para ignorar as propriedades de discagem da conta de usuário](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).

### <a name="group-policy-management"></a>Gerenciamento de Política de Grupo

O gerenciamento de Política de Grupo permite a alteração baseada em diretório e o gerenciamento de configurações de usuário e computador, incluindo informações de segurança e de usuário. Você usa Política de Grupo para definir configurações para grupos de usuários e computadores.

Com Política de Grupo, você pode especificar configurações para entradas de registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. As configurações de Política de Grupo que você cria estão contidas em um objeto de Política de Grupo (GPO). Ao associar um GPO a Active Directory contêineres de sistema selecionados — sites, domínios e UOs — você pode aplicar as configurações do GPO aos usuários e computadores nesses Active Directory contêineres. Para gerenciar objetos do Política de Grupo em uma empresa, você pode usar o Editor de Gerenciamento de Política de Grupo MMC (console de gerenciamento Microsoft).

## <a name="windows-10-vpn-clients"></a>Clientes de VPN do Windows 10

Além dos componentes do servidor, verifique se os computadores cliente que você configura para usar a VPN estão executando a atualização de aniversário do Windows 10 (versão 1607). Os clientes de VPN do Windows 10 devem estar ingressados no domínio do seu Active Directory domínio.


O cliente de VPN do Windows 10 é altamente configurável e oferece muitas opções. Para ilustrar melhor os recursos específicos que esse cenário usa, a tabela 1 identifica as categorias de recursos de VPN e as configurações específicas às quais essa implantação faz referência. Você definirá as configurações individuais para esses recursos usando o CSP (provedor de serviços de configuração) do VPNv2 discutido posteriormente nesta implantação.

Tabela 1. Recursos e configurações de VPN discutidos nesta implantação

| Recurso de VPN     |     Configuração do cenário de implantação         |
|-----------------|-----------------------------------------------|
| Tipo de conexão |                 IKEv2 nativo                  |
|     Roteamento     |                Túnel dividido                |
| Resolução de nomes |  Lista de informações de nome de domínio e sufixo DNS  |
|   Disparar    |    Detecção de Always On e de rede confiável    |
| Autenticação  | PEAP-TLS com certificados de usuário protegidos por TPM |

>[!NOTE]
>PEAP-TLS e TPM são "protocolo de autenticação extensível protegida com segurança da camada de transporte" e "Trusted Platform Module", respectivamente.

### <a name="vpnv2-csp-nodes"></a>Nós do CSP VPNv2

Nessa implantação, você usa o nó ProfileXML VPNv2 CSP para criar o perfil VPN que é entregue a computadores cliente com Windows 10. Os CSPs (provedores de serviços de configuração) são interfaces que expõem vários recursos de gerenciamento dentro do cliente do Windows; Conceitualmente, os CSPs funcionam de maneira semelhante ao funcionamento do Política de Grupo. Cada CSP tem nós de configuração que representam configurações individuais. Também como Política de Grupo configurações, você pode ligar as configurações do CSP às chaves do registro, arquivos, permissões e assim por diante. Semelhante a como você usa o Editor de Gerenciamento de Política de Grupo para configurar objetos de Política de Grupo (GPOs), você configura os nós CSP usando uma solução de MDM (gerenciamento de dispositivo móvel), como Microsoft Intune. Os produtos de MDM, como o Intune, oferecem uma opção de configuração amigável que configura o CSP no sistema operacional.

![Gerenciamento de dispositivo móvel para configuração do CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

No entanto, você não pode configurar alguns nós do CSP diretamente por meio de uma interface do usuário (IU), como o console de administração do Intune. Nesses casos, você deve configurar manualmente as configurações de OMA-URI (Open Mobile Alliance Uniform Resource Identifier). Você configura o OMA-URIs usando o OMA-DM (OMA Device Management Protocol), uma especificação de gerenciamento de dispositivo universal que oferece suporte aos dispositivos Apple, Android e Windows mais modernos. Desde que eles sigam a especificação OMA-DM, todos os produtos MDM devem interagir com esses sistemas operacionais da mesma maneira.

O Windows 10 oferece muitos CSPs, mas essa implantação se concentra no uso do CSP VPNv2 para configurar o cliente VPN. O CSP VPNv2 permite a configuração de cada configuração de perfil de VPN no Windows 10 por meio de um nó CSP exclusivo. Também contida no CSP VPNv2 é um nó chamado *ProfileXML*, que permite que você defina todas as configurações em um nó em vez de individualmente. Para obter mais informações sobre o ProfileXML, consulte a seção "visão geral do ProfileXML" mais adiante nesta implantação. Para obter detalhes sobre cada nó do CSP do VPNv2, consulte o [CSP do VPNv2](/windows/client-management/mdm/vpnv2-csp).

## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Iniciar o planejamento de sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>Tópicos relacionados

- [Suporte de software de servidor da Microsoft para máquinas virtuais Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Este artigo aborda a política de suporte para executar o software de servidor da Microsoft no ambiente de Microsoft Azure máquina virtual (infraestrutura como serviço).

- [Acesso remoto](../../Remote-Access.md): Este tópico fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016.

- [Guia técnico de VPN do Windows 10](/windows/access-protection/vpn/vpn-guide): este guia orienta você pelas decisões que você fará para clientes do Windows 10 em sua solução de VPN corporativa e como configurar sua implantação. Este guia faz referência ao Provedor de Serviços de Configuração (CSP) VPNv2 e fornece instruções de configuração de gerenciamento de dispositivos móveis (MDM) usando o Microsoft Intune e o modelo de Perfil de VPN para o Windows 10.

- [Guia de rede principal](../../../../networking/core-network-guide/Core-Network-Guide.md): este guia fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo domínio de Active Directory em uma nova floresta.

- [DNS (sistema de nomes de domínio)](../../../../networking/dns/dns-top.md): Este tópico fornece uma visão geral dos sistemas de nomes de domínio (DNS). No Windows Server 2016, o DNS é uma função de servidor que você pode instalar usando Gerenciador do Servidor ou comandos do Windows PowerShell. Se você estiver instalando um novo Active Directory floresta e domínio, o DNS será instalado automaticamente com Active Directory como o servidor de catálogo global para a floresta e o domínio.

- [Active Directory visão geral dos serviços de certificados](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11)): Este documento fornece uma visão geral dos serviços de certificados Active Directory (AD CS) no Windows Server &reg; 2012. O AD CS é a Função de Servidor que permite construir uma infraestrutura de chave pública (PKI) e fornecer recursos de criptografia de chave pública, certificados digitais e assinaturas digitais para a sua organização.

- [Diretrizes de design de infraestrutura de chave pública](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953): este fórum fornece orientação sobre como criar infraestruturas de chave pública (PKIs). Antes de configurar uma hierarquia de PKI e autoridade de certificação (CA), você deve estar ciente da política de segurança da sua organização e do CPS (declaração de prática de certificado).

- [Active Directory visão geral dos serviços de certificados](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11)): este guia passo a passo descreve as etapas necessárias para configurar uma configuração básica de &reg; serviços de certificados Active Directory (AD CS) em um ambiente de laboratório. O AD CS no Windows Server &reg; 2008 R2 fornece serviços personalizáveis para criar e gerenciar certificados de chave pública usados em sistemas de segurança de software que empregam tecnologias de chave pública.

- [Servidor de políticas de rede (NPS)](../../../../networking/technologies/nps/nps-top.md): Este tópico fornece uma visão geral do servidor de políticas de rede no Windows Server 2016. O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão.
