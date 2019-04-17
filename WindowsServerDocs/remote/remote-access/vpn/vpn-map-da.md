---
title: Recursos de VPN Always On
description: Neste tópico, você saberá mais sobre os recursos e funcionalidades de VPN Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066850"
---
# Funcionalidades e recursos de VPN always On

>Aplicável a: Windows Server \(Semi-Annual Channel\), Windows Server 2016, Windows 10

& #171;  [ **Anterior:** sempre na implantação de VPN para o Windows Server e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
& #187; [ **Próximo:** Saiba mais sobre os aprimoramentos de VPN sempre ativado](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

Neste tópico, você saberá mais sobre os recursos e funcionalidades de VPN Always On.  A tabela a seguir não é uma lista completa, no entanto, ele inclui alguns dos recursos e funcionalidades usadas em soluções de acesso remoto mais comuns. 

>[!TIP]
>Se você usar o DirectAccess no momento, é recomendável que você investigue a funcionalidade de VPN Always On cuidadosamente para determinar se ele aborda que todas as suas necessidades de acesso remoto antes de migrar formam o DirectAccess para VPN Always On.  

| Área funcional | VPN Always On  |
| ---- | ---- |
| Conectividade perfeita e transparente à rede corporativa. | Você pode configurar a VPN Always On para dar suporte a disparo automático com base em solicitações de resolução de namespace ou de inicialização do aplicativo.<p><p>Defina usando:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName//domainnameinformationlist/AutoTrigger** |
| Uso de um encapsulamento de infraestrutura dedicada para fornecer conectividade para usuários não assinados na rede corporativa. | Você pode obter essa funcionalidade, usando o recurso de encapsulamento do dispositivo no perfil da VPN.<p><p>_**Observação.**_<br>Encapsulamento do dispositivo só pode ser configurado em dispositivos que ingressaram no domínio usando IKEv2 com autenticação de certificado de computador.<p><p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Uso de gerenciar-out para permitir que a conectividade remota para clientes de sistemas de gerenciamento localizados na rede corporativa. | Você pode obter essa funcionalidade, usando o recurso de encapsulamento do dispositivo no perfil da VPN, combinado com a configuração a conexão VPN para registrar dinamicamente os endereços IP atribuídos à interface da VPN com serviços internos de DNS.<p><p>_**Observação.**_<br>Se você ativar filtros de tráfego no perfil de encapsulamento do dispositivo, o encapsulamento do dispositivo nega o tráfego de entrada (a partir da rede corporativa para o cliente).<p><p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Se enquadram novamente quando os clientes estão por trás de firewalls ou servidores proxy. | Você pode configurar para retornar ao SSTP (de IKEv2) usando o tipo de protocolo/encapsulamento automático dentro do perfil VPN.<p><p>_**Observação.**_<br>Túnel de usuário é compatível com o SSTP e IKEv2 e encapsulamento do dispositivo suporta IKEv2 somente com nenhum suporte para fallback SSTP.<p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Suporte para o modo de acesso de end-to-edge. | VPN Always On fornece conectividade aos recursos corporativos usando as diretivas de túnel que exigem autenticação e criptografia até que eles cheguem gateway VPN. Por padrão, as sessões de túnel encerrar no gateway VPN, que também funciona como o gateway IKEv2, fornecendo segurança end-to-edge. |
| Suporte para autenticação de certificado de máquina. | O tipo de protocolo IKEv2 disponível como parte da plataforma VPN Always On oferece suporte especificamente para o uso de certificados de computador ou o computador para autenticação de VPN.<p><p>_**Observação.**_<br>IKEv2 é o único protocolo com suporte para encapsulamento do dispositivo e não há nenhuma opção de suporte para fallback SSTP. <p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/autenticação/MachineMethod** |
| Use grupos de segurança para limitar a funcionalidade de acesso remoto para clientes específicos. | Você pode configurar a VPN Always On para dar suporte a autorização granular ao usar RADIUS, que inclui o uso de grupos de segurança para controlar o acesso VPN. |
| Suporte para servidores por trás de um dispositivo NAT ou firewall de borda. | VPN Always On oferece a possibilidade de usar protocolos como IKEv2 e SSTP que suportam totalmente o uso de um gateway VPN que fica atrás de um firewall de dispositivo ou edge NAT.<p><p>_**Observação.**_<br>Túnel de usuário é compatível com o SSTP e IKEv2 e encapsulamento do dispositivo suporta IKEv2 somente com nenhum suporte para fallback SSTP. |
| Capacidade para determinar a conectividade de intranet quando conectado à rede corporativa. | Detecção de rede confiável fornece a capacidade de detectar conexões de rede corporativa, e ele se baseia em uma avaliação do sufixo DNS específico da conexão atribuído a interfaces de rede e o perfil de rede.<p><p>Defina usando:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Conformidade usando a proteção de acesso à rede (NAP). | O cliente VPN Always On pode se integrar com acesso condicional do Azure para impor MFA, conformidade do dispositivo ou uma combinação de ambos. Quando estiver em conformidade com políticas de acesso condicional, o Azure AD emite um certificado de autenticação IPsec curta (por padrão, 60 minutos) que o cliente pode usar para autenticar para o gateway VPN. Conformidade do dispositivo tira proveito das políticas de conformidade do System Center Configuration Manager/Intune, que pode incluir o estado de atestado de integridade do dispositivo. Neste momento, o acesso condicional do Azure VPN fornece a substituição mais próxima à solução NAP existente, embora não haja nenhuma forma de correção recursos de rede de quarentena ou serviço. Para obter mais detalhes, consulte [VPN e acesso condicional](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Defina usando:<br>**VPNv2/ProfileName/DeviceCompliance** |
| Capacidade de definir quais servidores de gerenciamento são acessíveis antes da entrada do usuário. | Você pode conseguir essa funcionalidade em VPN Always On usando o recurso de encapsulamento do dispositivo (disponível na versão 1709 – para IKEv2 somente) no perfil da VPN, combinado com filtros de tráfego para controlar quais sistemas de gerenciamento na rede corporativa estão acessíveis por meio do Encapsulamento do dispositivo.<p><p>_**Observação.**_<br>Se você ativar filtros de tráfego no perfil de encapsulamento do dispositivo, o encapsulamento do dispositivo nega o tráfego de entrada (a partir da rede corporativa para o cliente).<p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## Funcionalidades adicionais

Cada item nesta seção é um cenário de caso de uso ou a funcionalidade de acesso remoto comumente usados para o qual a VPN Always On melhorou funcionalidade — por meio de uma expansão de funcionalidade ou eliminação de uma limitação anterior.


| Área funcional | VPN Always On  |
| ---- | ---- |
| Dispositivos ingressados em domínio com SKUs Enterprise requisito. | Suporta VPN Always On ingressado no domínio, fora do domínio associado (grupo de trabalho) ou dispositivos Azure – ingressado no AD para permitir cenários BYOD e corporativos. VPN Always On está disponível em todas as edições do Windows, e os recursos de plataforma estão disponíveis a terceiros por meio de suporte de plug-in de VPN UWP.<p><p>_**Observação.**_<br>Encapsulamento do dispositivo só pode ser configurado em dispositivos ingressados em domínio executando o Windows 10 Enterprise ou Education versão 1709 ou posterior. Não há nenhum suporte para o controle de terceiros do túnel de dispositivo. |
| Suporte para IPv4 e IPv6. | Com a VPN Always On, os usuários podem acessar recursos de IPv4 e IPv6 na rede corporativa. O cliente VPN Always On usa uma abordagem de pilha dupla que não especificamente depende IPv6 ou a necessidade de gateway VPN para fornecer serviços de tradução NAT64 ou DNS64. |
| Suporte a dois fatores ou autenticação OTP. |A plataforma de VPN Always On suporta nativamente EAP, o que possibilita o uso de Microsoft diversificada e tipos de EAP de terceiros como parte do fluxo de trabalho de autenticação. Especificamente, o VPN Always On oferece suporte a cartão inteligente (física e virtual) e o Windows Hello para certificados de negócios para atender às exigências de autenticação de dois fatores. Além disso, VPN Always On oferece suporte a OTP por meio de MFA (não há suportado nativo, tem suporte apenas em plug-ins de terceiros) por meio de integração de EAP RADIUS.<p><p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/autenticação** |
| Suporte para vários domínios e florestas. | A plataforma de VPN Always On não tem nenhuma dependência em topologia de florestas ou domínio do Active Directory Domain Services (AD DS) (ou níveis funcionais/esquema associado) porque ele não requer que o cliente VPN ser associado a função de domínio. Política de grupo, portanto, não é uma dependência para definir configurações de perfil VPN, pois você não usá-lo durante a configuração do cliente. Quando a integração de autorização do Active Directory é necessária, você pode consegui-lo por meio de RAIO como parte do processo de autenticação e autorização de EAP. |
| Suporte para ambos os divisão e forçar encapsulamento para separação de tráfego de internet/intranet. | Você pode configurar a VPN Always On para dar suporte a ambas as encapsulamento de força (o modo de operação de padrão) e encapsulamento de divisão nativamente. VPN Always On fornece granularidade adicional para políticas de roteamento específicas do aplicativo.<p><p>_**Observação.**_<br>Compatível com apenas o encapsulamento do usuário.<p><p>Defina usando:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Suporte a vários protocolos. | VPN Always On pode ser configurado para dar suporte ao SSTP nativamente se Secure Sockets Layer fallback de IKEv2 é necessária.<p><p>_**Observação.**_<br>Túnel de usuário é compatível com o SSTP e IKEv2 e encapsulamento do dispositivo suporta IKEv2 somente com nenhum suporte para fallback SSTP.  |
| Assistente de conectividade para informar o status de conectividade corporativa. | VPN Always On está totalmente integrada com o Assistente de conectividade de rede nativo e fornece o status da conectividade da interface do modo de exibição todas as redes. Com o advento do Windows 10 para criadores (versão 1703), o status de conexão de VPN e o controle de conexão de VPN para o encapsulamento de usuário agora estão disponíveis por meio do submenu de rede (para o Windows VPN cliente interno), também. |
| Resolução de nomes de recursos corporativos usando nome curto, o nome de domínio totalmente qualificado (FQDN) e o sufixo DNS. | VPN Always On nativamente pode definir um ou mais sufixos DNS como parte do processo de atribuição de endereço IP, incluindo a resolução de nomes de recursos corporativos para nomes curtos, FQDNs ou toda namespaces DNS e conexão VPN. VPN Always On também permitem o uso de tabelas de diretiva de resolução de nome para fornecer granularidade de resolução de namespace específicas.<p><p>_**Observação.**_<br>Evite o uso de sufixos Global conforme eles interferem na resolução shortname ao usar tabelas de políticas de resolução de nome.<p><p>Defina usando:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName//domainnameinformationlist** |
---



## Próximas etapas

- [Saiba mais sobre os aperfeiçoamentos de VPN Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia de VPN Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Começar a planejar sua implantação de VPN Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
