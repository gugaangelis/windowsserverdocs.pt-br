---
title: Visão geral da tecnologia VPN do AlwaysOn
description: 'Esta página fornece um uma breve visão geral das tecnologias VPN Always On com links para documentos detalhados. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821257"
---
# <a name="always-on-vpn-technology-overview"></a>Visão geral da tecnologia VPN Always On
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Saiba mais sobre os aprimoramentos de VPN Always On](always-on-vpn-enhancements.md)<br>
&#187;  [**Next:** Saiba mais sobre os recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

Para essa implantação, você deve instalar um novo servidor de acesso remoto que esteja executando o Windows Server 2016, bem como modificar algumas de sua infraestrutura existente para a implantação.

A ilustração a seguir mostra a infraestrutura que é necessário para implantar a VPN Always On.

![Sempre na infraestrutura VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

O processo de conexão descrito nesta ilustração é composto das seguintes etapas:

1. Usando servidores DNS públicos, o cliente de VPN do Windows 10 executa uma consulta de resolução de nome para o endereço IP do gateway de VPN.

2. Usando o endereço IP retornado pelo DNS, o cliente VPN envia uma solicitação de conexão para o gateway de VPN.

3. O gateway de VPN também está configurado como um Remote Authentication Dial-In User Service \(RADIUS\) cliente; o cliente RADIUS de VPN envia a solicitação de conexão para o servidor NPS/corporativas de organização para o processamento de solicitação de conexão.

4. O servidor NPS processa a solicitação de conexão, incluindo a realização de autorização e autenticação e determina se deseja permitir ou negar a solicitação de conexão.

5. O servidor NPS encaminha uma resposta de aceitação de acesso ou negar o acesso ao gateway de VPN.

6. A conexão seja iniciada ou encerrada com base na resposta que o servidor VPN recebido do servidor NPS.

Para obter mais informações sobre cada componente de infraestrutura representada na ilustração acima, consulte as seções a seguir.

>[!NOTE]
>Se você já tiver algumas dessas tecnologias implantadas em sua rede, você pode usar as instruções neste guia de implantação para executar uma configuração adicional das tecnologias para essa finalidade da implantação.

## <a name="domain-name-system-dns"></a>Sistema de nome de domínio (DNS)

Zonas de sistema de nome de domínio (DNS) internos e externos são necessárias, que presume que a zona interna é um subdomínio delegado da zona externo (por exemplo, "corp.contoso.com" e "contoso.com").

Saiba mais sobre [sistema de nome de domínio (DNS)](../../../../networking/dns/dns-top.md) ou [guia da rede principal](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Outro DNS projeta, como o DNS com partição de rede (usando o mesmo nome de domínio interna e externamente em zonas DNS separadas) ou não relacionado interno e domínios externos (por exemplo, "contoso. local" e "contoso.com") também são possíveis. Para obter mais informações sobre a implantação de DNS com partição de rede, consulte [usar a política de DNS para a implantação de DNS/cérebro divisão](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewalls

Verifique se os firewalls permitem o tráfego que é necessário para comunicações de VPN e RADIUS funcionar corretamente.

Para obter mais informações, consulte [configurar Firewalls para o tráfego RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Acesso remoto como um servidor VPN de Gateway RAS

No Windows Server 2016, a função de servidor de acesso remoto é criada para executar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos. Para este guia de implantação, você precisa apenas um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de rede local.

O IKEv2 está descrito na solicitação de Internet Engineering Task Force para comentários 7296 de protocolo de túnel VPN. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for perdida temporariamente ou se um usuário move um computador cliente de uma rede para outra, IKEv2 restaura automaticamente a conexão VPN quando a conexão de rede for restabelecida — tudo sem a intervenção do usuário.

Usando o Gateway de RAS, você pode implantar conexões VPN para fornecer aos usuários finais acesso remoto à rede e recursos de sua organização. Implantação de VPN Always On mantém uma conexão persistente entre clientes e a rede da sua organização, sempre que os computadores remotos são conectados à Internet. Com o Gateway de RAS, você também pode criar uma conexão de VPN site a site entre dois servidores em locais diferentes, como entre seu escritório principal e uma filial e usar conversão de endereços de rede \(NAT\) , de modo que os usuários dentro do rede possa acessar recursos externos, como a Internet. Além disso, o Gateway de RAS dá suporte a Border Gateway Protocol (BGP), que fornece serviços de roteamento dinâmicos quando seus locais de escritório remoto também possuem gateways de borda que dão suporte a BGP.

Você pode gerenciar Gateways de serviço (acesso remoto), usando comandos do Windows PowerShell e o Remote Access Microsoft Management Console (MMC).



## <a name="network-policy-server-nps"></a>NPS (Servidor de Políticas de Rede)

O NPS permite que você crie e imponha políticas de acesso de rede em toda a organização para autenticação de solicitação de conexão e autorização. Quando você usa o NPS como servidor RADIUS Remote Authentication Dial-In usuário Service (), você configura os servidores de acesso de rede, como servidores VPN, como clientes RADIUS no NPS.

Você também configura as políticas de rede que o NPS usa para autorizar solicitações de conexão. Você pode configurar a contabilização RADIUS para que o NPS registre as informações de contabilização em arquivos de log no disco rígido local ou em um banco de dados do Microsoft SQL Server.

Para obter mais informações, consulte [servidor de diretivas de rede (NPS)](../../../../networking/technologies/nps/nps-top.md).


## <a name="active-directory-certificate-services"></a>Serviços de Certificados do Active Directory

O servidor de autoridade de certificação (CA) é uma autoridade de certificação que está executando serviços de certificados do Active Directory. A configuração de VPN exige uma Active Directory com base em infraestrutura de chave pública (PKI).

As organizações podem usar o AD CS para aumentar a segurança associando a identidade de uma pessoa, dispositivo ou serviço a uma chave pública correspondente. AD CS também inclui recursos que permitem que você gerencie o registro de certificado e a revogação em uma variedade de ambientes escalonáveis. Para obter mais informações, consulte [visão geral serviços de certificados do Active Directory](https://technet.microsoft.com/library/hh831740.aspx) e [diretrizes de Design de infraestrutura de chave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante a conclusão da implantação, você configurará os seguintes modelos de certificado na autoridade de certificação.

-   O modelo de certificado de autenticação de usuário

-   O modelo de certificado de autenticação do servidor VPN

-   O modelo de certificado de autenticação do servidor NPS

### <a name="certificate-templates"></a>Modelos de certificado

Modelos de certificado podem simplificar muito a tarefa de administração de uma autoridade de certificação (CA), permitindo a você emitir certificados que são pré-configurados para tarefas selecionadas. O snap-in MMC de modelos de certificado permite que você execute as seguintes tarefas.

-   Exiba as propriedades para cada modelo de certificado.

-   Copiar e modificar modelos de certificado.

-   Controlar quais usuários e computadores podem ler modelos e registrar certificados.

-   Execute outras tarefas administrativas relacionadas aos modelos de certificado.

Modelos de certificado são parte integrante de uma autoridade de certificação corporativa (CA). Eles são um elemento importante da política de certificação para um ambiente, que é o conjunto de regras e formatos de registro de certificado, uso e gerenciamento.

Para obter mais informações, consulte [modelos de certificado](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificados de servidor digital

Neste guia de implantação fornece instruções sobre como usar os serviços de certificados do Active Directory (AD CS) para registrar e registrar automaticamente certificados para acesso remoto e servidores de infraestrutura do NPS. AD CS permite que você criar uma infraestrutura de chave pública (PKI) e fornecer recursos de assinatura digital, certificados digitais e criptografia de chave pública para sua organização.

Quando você usa certificados digitais de servidor para autenticação entre computadores na sua rede, os certificados fornecem:

1.  Confidencialidade por meio da criptografia.

2.  Integridade através de assinaturas digitais.

3.  Autenticação pela associação de chaves de certificado com contas de computador, usuário ou dispositivo em uma rede do computador.

Para obter mais informações, consulte [guia passo a passo do AD CS: Implantação de hierarquia PKI de duas camadas](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Serviços de Domínio do Active Directory (AD DS)

O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos da rede e dados específicos de aplicativos habilitados por diretório. Os administradores podem usar AD DS para organizar elementos de uma rede, como usuários, computadores e outros dispositivos, em uma estrutura de confinamento hierárquica. A estrutura de confinamento hierárquica inclui a floresta do Active Directory, domínios na floresta e unidades organizacionais (OUs) em cada domínio. Um servidor com AD DS é chamado de controlador de domínio.

AD DS contém as contas de usuário, contas de computador e as propriedades de conta são necessárias pelo Protected Extensible Authentication Protocol (PEAP) para autenticar as credenciais do usuário e para avaliar a autorização para solicitações de conexão de VPN. Para obter informações sobre como implantar o AD DS, consulte o Windows Server 2016 [guia da rede principal](../../../../networking/core-network-guide/Core-Network-Guide.md).



Durante a conclusão das etapas nesta implantação, você configurará os itens a seguir no controlador de domínio.

-   Habilitar o registro automático de certificados na diretiva de grupo para computadores e usuários

-   Criar o grupo de usuários de VPN

-   Criar o grupo de servidores de VPN

-   Criar o grupo de servidores NPS

### <a name="active-directory-users-and-computers"></a>Usuários e computadores do Active Directory

Active Directory Users and Computers é um componente do AD DS que contém as contas que representam entidades físicas, como um computador, uma pessoa ou um grupo de segurança. Um grupo de segurança é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. Contas de usuário e computador que pertencem a um determinado grupo são denominadas membros do grupo.

Contas de usuário no Active Directory Users and Computers tem propriedades de discagem que durante o processo de autorização - o NPS avalia a menos que o **permissão de acesso de rede** propriedade da conta de usuário é definida como **controle acesso por meio da diretiva de rede do NPS**. Isso é a configuração padrão para todas as contas de usuário. Em alguns casos, no entanto, essa configuração pode ter uma configuração diferente que bloqueia o usuário se conecte usando VPN. Para proteger contra essa possibilidade, você pode configurar o servidor NPS para ignorar as propriedades de discagem da conta de usuário.

Para obter mais informações, consulte [configurar o NPS para ignorar a conta de usuário Dial-in propriedades](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### <a name="group-policy-management"></a>Gerenciamento de Política de Grupo

Gerenciamento de diretiva de grupo permite o gerenciamento de alterações e configuração baseada no Active de configurações de usuário e computador, incluindo informações de segurança e do usuário. Usar política de grupo para definir configurações para grupos de usuários e computadores.

Com a diretiva de grupo, você pode especificar configurações para as entradas do registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. As configurações de diretiva de grupo que você cria estão contidas em um objeto de diretiva de grupo (GPO). Associando um GPO selecionados contêineres de sistema do Active Directory — sites, domínios e OUs — você pode aplicar as configurações do GPO aos usuários e computadores nesses contêineres do Active Directory. Para gerenciar objetos de diretiva de grupo em toda a empresa, você pode usar o grupo de política de gerenciamento Editor Microsoft Management Console (MMC).


## <a name="windows-10-vpn-clients"></a>Clientes VPN do Windows 10

Juntamente com os componentes de servidor, certifique-se de que os computadores cliente que você configura para usar a VPN estiver executando a atualização de aniversário do Windows 10 (versão 1607). Os clientes de VPN do Windows 10 devem estar ingressados no domínio do Active Directory.


O cliente de VPN do Windows 10 é altamente configurável e oferece muitas opções. Para melhor ilustrar os recursos específicos que esse cenário usa, a tabela 1 mostra as categorias de recurso VPN e as configurações específicas que faz referência a essa implantação. Você vai configurar as configurações individuais para esses recursos usando o provedor de serviços de configuração (CSP) VPNv2 discutidos mais adiante nesta implantação. 

Tabela 1. Recursos de VPN e configurações discutidas nesta implantação

| **Recurso de VPN** | **Configuração de cenário de implantação**         |
|-----------------|-----------------------------------------------|
| Tipo de conexão | Native IKEv2                                  |
| Roteamento         | Túnel dividido                               |
| Resolução de nome | Sufixo DNS e a lista de informações de nome de domínio   |
| Disparar      | Detecção de rede sempre ativado e confiável       |
| Autenticação  | PEAP-TLS com certificados de usuário protegido – TPM |
---

>[!NOTE] 
>PEAP-TLS e do TPM são "Protegido Extensible Authentication Protocol com Transport Layer Security" e "Trusted Platform Module", respectivamente.

### <a name="vpnv2-csp-nodes"></a>Nós CSP de VPNv2

Nessa implantação, você pode usar o nó do ProfileXML VPNv2 CSP para criar o perfil VPN que é fornecido para computadores cliente com Windows 10. Provedores de serviço de configuração (CSPs) são interfaces que expõem diversos recursos de gerenciamento dentro do cliente Windows; Conceitualmente, os CSPs funcionam semelhante a como funciona a diretiva de grupo. Cada CSP conta conosco de configuração que representam as configurações individuais. Também, como configurações de diretiva de grupo, você pode vincular as configurações do CSP para chaves do registro, arquivos, permissões e assim por diante. Semelhante a como usar o Editor de gerenciamento de diretiva de grupo para configurar objetos de diretiva de grupo (GPOs), você configurar nós CSP usando uma solução MDM (gerenciamento) do dispositivo móvel, como o Microsoft Intune. Produtos MDM como Intune oferecem uma opção de configuração fácil de usar que configura o CSP no sistema operacional.

![Gerenciamento de dispositivos móveis para configuração do CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

No entanto, é possível configurar alguns nós CSP diretamente por meio de uma interface do usuário (IU) como o Console de administração do Intune. Nesses casos, você deve definir as configurações de Open Mobile Alliance Uniform Resource Identifier (OMA-URI) manualmente. Você pode configurar OMA URIs por meio do protocolo de gerenciamento de dispositivo do OMA (OMA-DM), uma especificação de gerenciamento de dispositivo universal que dão suporte a mais modernos dispositivos da Apple, Android e Windows. Desde que eles de acordo com a especificação de OMA-DM, todos os produtos MDM devem interagir com esses sistemas operacionais da mesma maneira.

O Windows 10 oferece muitos CSPs, mas essa implantação concentra-se sobre como usar o CSP de VPNv2 para configurar o cliente VPN. O CSP de VPNv2 permite a configuração de cada configuração de perfil VPN no Windows 10 por meio de um nó CSP exclusivo. Também contida em VPNv2 CSP é um nó chamado *ProfileXML*, que permite que você defina todas as configurações em um nó em vez de individualmente. Para obter mais informações sobre ProfileXML, consulte a seção "Visão geral do ProfileXML" mais adiante nessa implantação. Para obter detalhes sobre cada nó VPNv2 CSP, consulte o [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Comece a planejar sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## <a name="related-topics"></a>Tópicos relacionados
- [Suporte de software de servidor da Microsoft para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Este artigo descreve a política de suporte para a execução de software de servidor Microsoft no ambiente de máquina virtual do Microsoft Azure (uma infraestrutura como serviço).

- [Acesso remoto](../../Remote-Access.md): Este tópico fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016.

- [Guia técnico de VPN do Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Este guia o orienta as decisões que serão feitas para clientes do Windows 10 em sua solução de VPN da empresa e como configurar sua implantação. Este guia referencia o provedor de serviço de configuração de VPNv2 (CSP) e fornece gerenciamento de dispositivo móvel usando o Microsoft Intune e o modelo de perfil de VPN para Windows 10 de instruções de configuração (MDM).

- [Guia da rede principal](../../../../networking/core-network-guide/Core-Network-Guide.md): Este guia fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente operacional e um novo domínio do Active Directory em uma nova floresta.

- [O sistema de nomes de domínio (DNS)](../../../../networking/dns/dns-top.md): Este tópico fornece uma visão geral de sistemas de nome de domínio (DNS). No Windows Server 2016, o DNS é uma função de servidor que você pode instalar usando comandos do Gerenciador do servidor ou o Windows PowerShell. Se você estiver instalando uma nova floresta do Active Directory e o domínio, DNS é instalado automaticamente com o Active Directory como o servidor de Catálogo Global para a floresta e domínio. 

- [Visão geral dos serviços de certificados do Active Directory](https://technet.microsoft.com/library/hh831740.aspx): Este documento fornece uma visão geral dos serviços de certificados do Active Directory (AD CS) no Windows Server® 2012. O AD CS é a Função de Servidor que permite construir uma infraestrutura de chave pública (PKI) e fornecer recursos de criptografia de chave pública, certificados digitais e assinaturas digitais para a sua organização.

- [Diretrizes de Design de infraestrutura de chave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):  O wiki oferece orientação sobre criando infra-estruturas de chave pública (PKI). Antes de configurar uma hierarquia PKI e certificação autoridade (CA), você deve estar ciente da segurança certificado e política prática instrução sua organização (CPS).

- [Guia passo a passo do AD CS: Implantação de hierarquia PKI de duas camadas](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): Este guia passo a passo descreve as etapas necessárias para configurar uma configuração básica dos serviços de certificados do Active Directory® (AD CS) em um ambiente de laboratório. O AD CS no Windows Server® 2008 R2 fornece serviços personalizáveis para criar e gerenciar certificados de chaves públicas usados em sistemas de segurança de software empregam tecnologias de chave pública.

- [Rede (NPS) do servidor de política](../../../../networking/technologies/nps/nps-top.md): Este tópico fornece uma visão geral do servidor de políticas de rede no Windows Server 2016. O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão. 

---
