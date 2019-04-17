---
title: Sempre na visão geral da tecnologia VPN
description: 'Esta página provies uma breve visão geral das tecnologias VPN sempre ativa com links para documentos detalhados. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031320"
---
# Visão geral da tecnologia VPN Always On
>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** Saiba mais sobre os aprimoramentos de VPN Always On](always-on-vpn-enhancements.md)<br>
& #187;  [ **Próximo:** Saiba mais sobre os recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

Para essa implantação, você deve instalar um novo servidor de acesso remoto que está executando o Windows Server 2016, bem como modificar algumas das sua infraestrutura existente para a implantação.

A ilustração a seguir mostra a infraestrutura necessária para implantar VPN Always On.

![Sempre na infraestrutura de VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

O processo de conexão descrito nesta ilustração é composto pelas seguintes etapas:

1. Usando servidores DNS públicos, o cliente VPN do Windows 10 executa uma consulta de resolução de nome para o endereço IP do gateway VPN.

2. O cliente VPN usando o endereço IP retornado pelo DNS, envia uma solicitação de conexão ao gateway VPN.

3. O gateway VPN também está configurado como um \(RADIUS\) Remote Authentication Dial-In User Service cliente; Cliente VPN RADIUS envia a solicitação de conexão ao servidor NPS organização/corporativo para processamento de solicitação de conexão.

4. O servidor NPS processa a solicitação de conexão, incluindo realizando autorização e autenticação e determina se é permitir ou negar a solicitação de conexão.

5. O servidor NPS encaminha uma resposta de aceitação de acesso ou negar o acesso ao gateway VPN.

6. A conexão é iniciada ou encerrado com base na resposta que o servidor VPN recebido do servidor NPS.

Para obter mais informações sobre cada componente de infraestrutura representado na ilustração acima, consulte as seções a seguir.

>[!NOTE]
>Se você já tiver algumas dessas tecnologias implantadas em sua rede, você pode usar as instruções neste guia de implantação para executar configurações adicionais das tecnologias para essa finalidade de implantação.

## Sistema de nome de domínio (DNS)

Zonas de sistema de nome de domínio (DNS) internos e externos são necessárias, que assume que a zona interna é um subdomínio delegado da zona externo (por exemplo, corp.contoso.com e contoso.com).

Saiba mais sobre o [Sistema de nome de domínio (DNS)](../../../../networking/dns/dns-top.md) ou o [Guia da rede principal](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Outro DNS designs, como DNS com partição de rede (usando o mesmo nome de domínio interna e externamente em separado zonas DNS) ou não relacionados interno e domínios externos (por exemplo, contoso. local e contoso.com) também são possíveis. Para obter mais informações sobre como implantar o DNS com partição de rede, consulte [Usar política de DNS para a implantação de DNS cérebro/divisão](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## Firewalls

Certifique-se de que os firewalls permitem o tráfego que é necessário para comunicações VPN e RAIO de funcionar corretamente.

Para obter mais informações, consulte [Configurar Firewalls para tráfego RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Acesso remoto como um servidor VPN de Gateway RAS

No Windows Server 2016, a função de servidor de acesso remoto foi projetada para executar bem como um roteador e um servidor de acesso remoto; Portanto, ele dá suporte a uma ampla gama de recursos. Para este guia de implantação, você precisa apenas um pequeno subconjunto desses recursos: suporte para conexões VPN IKEv2 e roteamento de rede local.

IKEv2 é uma VPN descrito na Internet Engineering Task Force solicitar para 7296 comentários de protocolo de encapsulamento. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se a conexão for temporariamente perdida ou se um usuário move um computador cliente de uma rede para outra, IKEv2 restaura automaticamente a conexão VPN quando a conexão de rede for restabelecida — tudo isso sem a intervenção do usuário.

Usando o Gateway de RAS, você pode implantar conexões VPN para fornecer aos usuários finais acesso remoto à rede e recursos de sua organização. Implantação de VPN Always On mantém uma conexão persistente entre clientes e a rede da organização, sempre que os computadores remotos estão conectados à Internet. Com o Gateway de RAS, você também pode criar uma conexão de VPN site a site entre dois servidores em locais diferentes, como entre seu escritório principal e uma filial e usar \(NAT\) conversão de endereços de rede para que os usuários dentro da rede podem acessar externo recursos, como a Internet. Além disso, o Gateway de RAS dá suporte a Border Gateway Protocol (BGP), que fornece serviços de roteamento dinâmicos quando seus locais de escritório remoto também têm gateways de borda que dão suporte ao BGP.

Você pode gerenciar Gateways de serviço de acesso remoto (RAS) usando comandos do Windows PowerShell e o remoto acesso Microsoft Management Console (MMC).



## NPS (Servidor de Políticas de Rede)

NPS permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação de solicitação de conexão e autorização. Quando você usa o NPS como um servidor remoto Authentication Dial-In User Service (RADIUS), você configurar servidores de acesso de rede, como servidores VPN, como clientes RADIUS no NPS.

Você também configurar políticas de rede que o NPS usa para autorizar solicitações de conexão, e você pode configurar estatísticas RADIUS para que o NPS registra informações de estatísticas para registrar os arquivos no disco rígido local ou em um banco de dados do Microsoft SQL Server.

Para obter mais informações, consulte o [Servidor de política de rede (NPS)](../../../../networking/technologies/nps/nps-top.md).


## Serviços de Certificados do Active Directory

O servidor de autoridade de certificação (CA) é uma autoridade de certificação que está executando os serviços de certificados do Active Directory. A configuração de VPN requer uma baseada no Active Directory infraestrutura de chave pública (PKI).

As organizações podem usar o AD CS para aprimorar a segurança, associando a identidade de uma pessoa, dispositivo ou serviço a uma chave pública correspondente. AD CS também inclui recursos que permitem que você gerencie o registro de certificado e revogação em uma variedade de ambientes escalonáveis. Para obter mais informações, consulte [Visão geral do Active Directory certificado serviços](https://technet.microsoft.com/library/hh831740.aspx) e [Diretrizes de Design de infraestrutura de chave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante a conclusão da implantação, você irá configurar os seguintes modelos de certificado na autoridade de certificação.

-   O modelo de certificado de autenticação do usuário

-   O modelo de certificado de autenticação de servidor VPN

-   O modelo de certificado de autenticação de servidor NPS

### Modelos de certificado

Modelos de certificado podem simplificar bastante a tarefa de administrar uma autoridade de certificação (CA), permitindo que você para emitir certificados que são pré-configurados para tarefas selecionadas. O snap-in do MMC modelos de certificado permite que você execute as seguintes tarefas.

-   Propriedades de exibição para cada modelo de certificado.

-   Copiar e modificar modelos de certificado.

-   Controlar quais usuários e computadores podem ler modelos e registrar certificados.

-   Execute outras tarefas administrativas relacionadas aos modelos de certificado.

Modelos de certificado são parte integrante de uma autoridade de certificação (CA). Eles são um elemento importante da diretiva de certificado de um ambiente, que é o conjunto de regras e formatos para registro de certificado, uso e gerenciamento.

Para obter mais informações, consulte [Modelos de certificado](https://technet.microsoft.com/library/cc730705.aspx).

### Certificados de servidor digital

Este guia de implantação fornece instruções para usar os serviços de certificados do Active Directory (AD CS) para registrar e registrar automaticamente os certificados para acesso remoto e servidores de infraestrutura do NPS. AD CS permite que você compilar uma infraestrutura de chave pública (PKI) e fornecer recursos de assinatura digital, certificados digitais e criptografia de chave pública para sua organização.

Quando você usa certificados de servidor digital para autenticação entre computadores em sua rede, os certificados fornecem:

1.  Confidencialidade por meio de criptografia.

2.  Integridade por meio de assinaturas digitais.

3.  Autenticação associando chaves de certificado com um contas de usuário, computador ou dispositivo em uma rede do computador.

Para obter mais informações, consulte [guia passo a passo do AD CS: implantação de hierarquia de PKI camadas duas](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## Serviços de Domínio do Active Directory (AD DS)

O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede e dados específicos do aplicativo de aplicativos habilitados por diretório. Os administradores podem usar o AD DS para organizar elementos de uma rede, como usuários, computadores e outros dispositivos, em uma estrutura hierárquica contenção. A estrutura hierárquica contenção inclui floresta do Active Directory, domínios na floresta e unidades organizacionais (UOs) em cada domínio. Um servidor que está executando o AD DS é chamado de um controlador de domínio.

AD DS contém as contas de usuário, contas de computador e propriedades da conta que são necessárias pelo Protected Extensible Authentication Protocol (PEAP) para autenticar as credenciais do usuário e para avaliar a autorização para solicitações de conexão VPN. Para obter informações sobre como implantar o AD DS, consulte o [Guia da rede principal](../../../../networking/core-network-guide/Core-Network-Guide.md)do Windows Server 2016.



Durante a conclusão das etapas nesta implantação, você irá configurar os itens a seguir no controlador de domínio.

-   Habilitar o registro automático de certificado na política de grupo para usuários e computadores

-   Criar o grupo de usuários de VPN

-   Criar o grupo de servidores VPN

-   Criar o grupo de servidores NPS

### Computadores e usuários do active Directory

Active Directory Users and Computers é um componente do AD DS que contém as contas que representam entidades físicas, como um computador, uma pessoa ou um grupo de segurança. Um grupo de segurança é uma coleção de contas de usuário ou computador que os administradores podem gerenciar como uma única unidade. Contas de usuário e computador que pertencem a um determinado grupo são chamadas de membros do grupo.

Contas de usuário no Active Directory Users and Computers têm propriedades de discagem que NPS avalia durante o processo de autorização -, a menos que a propriedade de **Permissão de acesso à rede** da conta de usuário é definida como controlar o acesso por meio da política de rede NPS ** **. Isso é a configuração padrão para todas as contas de usuário. Em alguns casos, no entanto, essa configuração pode ter uma configuração diferente que impede o usuário de se conectar usando a VPN. Para evitar essa possibilidade, você pode configurar o servidor NPS para ignorar as propriedades de discagem da conta de usuário.

Para obter mais informações, consulte [Configurar o NPS para propriedades de discagem ignorar a conta de usuário](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### Gerenciamento de Política de Grupo

Gerenciamento de política de grupo permite que o gerenciamento baseado em diretório de configuração e alteração de configurações de usuário e de computador, incluindo informações de segurança e do usuário. Use a política de grupo para definir configurações para grupos de usuários e computadores.

Com a política de grupo, você pode especificar configurações para entradas do registro, segurança, instalação de software, scripts, redirecionamento de pasta, serviços de instalação remota e manutenção do Internet Explorer. As configurações de política de grupo que você cria estão contidas em um objeto de política de grupo (GPO). Associando um GPO a recipientes de sistema selecionados do Active Directory — sites, domínios e UOs — você pode aplicar as configurações do GPO aos usuários e computadores nesses recipientes do Active Directory. Para gerenciar objetos de política de grupo em uma empresa, você pode usar o grupo de política de gerenciamento Editor Microsoft Management Console (MMC).


## Clientes VPN do Windows 10

Além dos componentes do servidor, certifique-se de que os computadores cliente que você configurar para usar VPN estão executando a atualização de aniversário do Windows 10 (version1607). Os clientes VPN do Windows 10 devem ser ingressados em domínio ao seu domínio do Active Directory.


O cliente VPN do Windows 10 é altamente configurável e oferece muitas opções. Para ilustrar melhor os recursos específicos que usa esse cenário, a tabela 1 identifica as categorias de recursos VPN e configurações específicas que faz referência a essa implantação. Você vai configurar as configurações individuais para que esses recursos usando o provedor de serviços de configuração (CSP) VPNv2 explicada posteriormente essa implantação. 

Tabela 1. Recursos de VPN e configurações discutidas nesta implantação

| **Recurso VPN** | **Configuração do cenário de implantação**         |
|-----------------|-----------------------------------------------|
| Tipo de conexão | IKEv2 nativo                                  |
| Roteamento         | Encapsulamento de divisão                               |
| Resolução de nomes | Sufixo DNS e a lista de informações de nome de domínio   |
| Disparo      | Detecção de rede sempre ativado e confiável       |
| Authentication  | PEAP-TLS com certificados de usuário – protegida por TPM |
---

>[!NOTE] 
>PEAP-TLS e TPM são "Protegido Extensible Authentication Protocol com Transport Layer Security" e "Trusted Platform Module", respectivamente.

### Nós do CSP VPNv2

Nessa implantação, você pode usar o nó ProfileXML CSP de VPNv2 para criar o perfil VPN é entregues aos computadores de cliente do Windows 10. Provedores de serviço de configuração (CSPs) são interfaces que expõem vários recursos de gerenciamento no cliente do Windows; Conceitualmente, CSPs trabalho semelhante à maneira como funciona a política de grupo. Cada CSP tem nós de configuração que representam as configurações individuais. Também como configurações de política de grupo, você pode vincular configurações do CSP para chaves do registro, arquivos, permissões e assim por diante. Semelhante a como você usar o Editor de gerenciamento de política de grupo para configurar objetos de política de grupo (GPOs), você configurar nós do CSP usando uma solução de gerenciamento (MDM) de dispositivo móvel, como o Microsoft Intune. Produtos MDM como o Intune oferecem uma opção de configuração amigáveis que configura o CSP no sistema operacional.

![Gerenciamento de dispositivos móveis para configuração do CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

No entanto, você não pode configurar alguns nós CSP diretamente por meio de uma interface do usuário (UI) como o Console de administração do Intune. Nesses casos, você deve definir as configurações de Open Mobile Alliance Uniform Resource Identifier (OMA-URI) manualmente. Você pode configurar OMA-URIs usando o protocolo de gerenciamento de dispositivo OMA (OMA-DM), uma especificação de gerenciamento de dispositivos universal que dão suporte a mais modernos dispositivos Apple, Android e Windows. Desde que seguem a especificação de OMA DM, todos os produtos do MDM devem interagir com esses sistemas operacionais da mesma maneira.

Windows 10 oferece muitos CSPs, mas essa implantação se concentra no uso do CSP VPNv2 para configurar o cliente VPN. O CSP de VPNv2 permite a configuração de cada configuração de perfil VPN no Windows 10 por meio de um nó CSP exclusivo. Também contidos no CSP VPNv2 é um nó chamado *ProfileXML*, que permite que você configure todas as configurações em um nó em vez de individualmente. Para obter mais informações sobre o ProfileXML, consulte a seção "Visão geral do ProfileXML" mais tarde nessa implantação. Para obter detalhes sobre cada nó CSP VPNv2, consulte o [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Começar a planejar sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## Tópicos relacionados
- [Suporte de software de servidor da Microsoft para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Este artigo aborda a política de suporte para a execução de software de servidor da Microsoft no ambiente de máquina virtual do Microsoft Azure (uma infraestrutura como serviço).

- [Acesso remoto](../../Remote-Access.md): Este tópico fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016.

- [Guia de técnico de VPN do Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): este guia explica as decisões que você deverá tomar para clientes do Windows 10 em sua solução VPN empresarial e como configurar sua implantação. Este guia faz referência ao Provedor de Serviços de Configuração (CSP) VPNv2 e fornece instruções de configuração de gerenciamento de dispositivos móveis (MDM) usando o Microsoft Intune e o modelo de Perfil de VPN para o Windows 10.

- [Guia da rede principal](../../../../networking/core-network-guide/Core-Network-Guide.md): este guia fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo domínio do Active Directory em uma nova floresta.

- [Sistema de nome de domínio (DNS)](../../../../networking/dns/dns-top.md): Este tópico fornece uma visão geral dos sistemas de nomes de domínio (DNS). No Windows Server 2016, o DNS é uma função de servidor que você pode instalar usando comandos do Gerenciador do servidor ou do Windows PowerShell. Se você estiver instalando uma nova floresta do Active Directory e o domínio, DNS é instalado automaticamente com o Active Directory como o servidor de Catálogo Global para a floresta e domínio. 

- [Visão geral do Active Directory certificado serviços](https://technet.microsoft.com/library/hh831740.aspx): este documento fornece uma visão geral dos serviços de certificado Active Directory (AD CS) no Windows Server® 2012. AD CS é a função de servidor que permite que você crie uma infraestrutura de chave pública (PKI) e fornecer recursos de assinatura digital, certificados digitais e criptografia de chave pública para sua organização.

- [Diretrizes de Design de infraestrutura de chave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): este wiki fornece diretrizes sobre como projetar infraestruturas de chave pública (PKI). Antes de configurar uma hierarquia PKI e certificação (autoridade), você deve estar ciente da segurança política e certificado prática instrução sua organização (CPS).

- [Guia passo a passo do AD CS: implantação de hierarquia de PKI camadas dois](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): este guia passo a passo descreve as etapas necessárias para configurar uma configuração básica de serviços de certificados do Active Directory® (AD CS) em um ambiente de laboratório. AD CS no Windows Server® 2008 R2 fornecem serviços personalizáveis para criar e gerenciar certificados de chave pública usados em sistemas de segurança de software que emprega a tecnologia de chave pública.

- [Servidor de política de rede (NPS)](../../../../networking/technologies/nps/nps-top.md): Este tópico fornece uma visão geral do servidor de políticas de rede no Windows Server 2016. O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão. 

---
