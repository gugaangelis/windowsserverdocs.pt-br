---
title: Recursos de VPN Always On
description: Neste tópico, você aprenderá sobre os recursos e funcionalidade de VPN Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874807"
---
# <a name="always-on-vpn-features-and-functionalities"></a>As funcionalidades e recursos de VPN always On

>Aplica-se a: Windows Server \(canal semestral\), Windows Server 2016, Windows 10

&#171;  [**Anterior:** Sempre na implantação de VPN para Windows Server e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
&#187; [ **Next:** Saiba mais sobre os aprimoramentos de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

Neste tópico, você aprenderá sobre os recursos e funcionalidades de VPN Always On.  A tabela a seguir não é uma lista completa, no entanto, ele inclui alguns dos recursos mais comuns e as funcionalidades usadas em soluções de acesso remoto. 

>[!TIP]
>Se você usar o DirectAccess no momento, é recomendável que você investigue a funcionalidade de VPN Always On com cuidado para determinar se ele tratar de que todas as suas necessidades de acesso remoto antes de migrar formam o DirectAccess para VPN Always On.  

| Área funcional | VPN Always On  |
| ---- | ---- |
| Conectividade perfeita e transparente para a rede corporativa. | Você pode configurar a VPN Always On para dar suporte a disparo automático com base em solicitações de resolução de namespace ou de inicialização do aplicativo.<p><p>Defina usando:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Uso de um túnel de infraestrutura dedicada para fornecer conectividade para os usuários que não está conectados à rede corporativa. | Você pode obter essa funcionalidade, usando o recurso de encapsulamento de dispositivo no perfil de VPN.<p><p>_**Observação.**_<br>Túnel de dispositivo pode ser configurado somente em dispositivos ingressados no domínio usando o IKEv2 com a autenticação de certificado do computador.<p><p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Uso de gerenciamento remoto para permitir a conectividade remota para clientes de sistemas de gerenciamento, localizados na rede corporativa. | Você pode obter essa funcionalidade usando o recurso de encapsulamento de dispositivo no perfil de VPN, combinado com a configuração de conexão de VPN registrem dinamicamente os endereços IP atribuídos à interface de VPN com os serviços DNS internos.<p><p>_**Observação.**_<br>Se você ativar os filtros de tráfego no perfil de dispositivo de túnel, o dispositivo de túnel nega o tráfego de entrada (da rede corporativa para o cliente).<p><p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Se enquadram quando os clientes estiverem protegidos por firewalls ou servidores proxy. | Você pode configurar para retornarão ao SSTP (de IKEv2) usando o tipo de protocolo/encapsulamento automático dentro do perfil VPN.<p><p>_**Observação.**_<br>Túnel de usuário dá suporte a SSTP e IKEv2 e túnel do dispositivo dá suporte a IKEv2 somente com não há suporte para o fallback SSTP.<p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Suporte para o modo de acesso de borda no final. | VPN Always On fornece conectividade aos recursos corporativos por meio de políticas de túnel que exigem autenticação e criptografia até atingirem o gateway de VPN. Por padrão, as sessões de túnel encerrar no gateway de VPN, também funciona como o gateway IKEv2, fornecendo segurança de borda no final. |
| Suporte para autenticação de certificado do computador. | O tipo de protocolo IKEv2 disponível como parte da plataforma de VPN Always On especificamente suporta o uso de certificados de máquina ou computador para autenticação de VPN.<p><p>_**Observação.**_<br>IKEv2 é o único protocolo com suporte para túnel do dispositivo e não há nenhuma opção de suporte para o fallback SSTP. <p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/autenticação/MachineMethod** |
| Use grupos de segurança para limitar a funcionalidade de acesso remoto para clientes específicos. | Você pode configurar a VPN Always On para dar suporte à autorização granular quando usando RADIUS, que inclui o uso de grupos de segurança para controle de acesso à VPN. |
| Suporte para servidores atrás de um firewall de borda ou dispositivo NAT. | VPN Always On fornece a capacidade de usar protocolos como IKEv2 e SSTP que suporte totalmente o uso de um gateway VPN que está por trás de um firewall de borda ou dispositivo NAT.<p><p>_**Observação.**_<br>Túnel de usuário dá suporte a SSTP e IKEv2 e túnel do dispositivo dá suporte a IKEv2 somente com não há suporte para o fallback SSTP. |
| Capacidade de determinar a conectividade de intranet quando conectado à rede corporativa. | Detecção de rede confiável fornece a capacidade de detectar conexões de rede corporativa, e ele se baseia em uma avaliação do sufixo DNS específico da conexão atribuído a interfaces de rede e o perfil de rede.<p><p>Defina usando:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Conformidade usando a proteção de acesso de rede (NAP). | O cliente VPN Always On pode integrar com acesso condicional do Azure para impor o MFA, a conformidade do dispositivo ou uma combinação de ambos. Quando estiver em conformidade com políticas de acesso condicional, o Azure AD emite um certificado de autenticação IPsec e de curta duração (por padrão, 60 minutos) que o cliente pode usar para autenticar para o gateway VPN. Conformidade do dispositivo tira proveito das políticas de conformidade do sistema de Center Configuration Manager/Intune, que pode incluir o estado de atestado de integridade do dispositivo. Neste momento, o acesso condicional do VPN do Azure fornece a substituição mais próximo à solução existente de NAP, embora não haja nenhuma forma de atualização de recursos de serviço ou colocar em quarentena de rede. Para obter mais detalhes, consulte [VPN e acesso condicional](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Defina usando:<br>**VPNv2/ProfileName/DeviceCompliance** |
| Capacidade de definir quais servidores de gerenciamento são acessíveis antes da entrada do usuário. | Você pode obter essa funcionalidade de VPN Always On usando o recurso de dispositivo de túnel (disponível na versão 1709 – para IKEv2 somente) no perfil de VPN, combinado com filtros de tráfego para controlar quais sistemas de gerenciamento na rede corporativa são acessíveis por meio de Dispositivo de túnel.<p><p>_**Observação.**_<br>Se você ativar os filtros de tráfego no perfil de dispositivo de túnel, o dispositivo de túnel nega o tráfego de entrada (da rede corporativa para o cliente).<p>Defina usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## <a name="additional-functionalities"></a>Funcionalidades adicionais

Cada item desta seção é um cenário de caso de uso ou a funcionalidade de acesso remoto comumente usadas para a qual VPN Always On melhorou a funcionalidade — por meio de uma expansão de funcionalidade ou eliminação de uma limitação anterior.


| Área funcional | VPN Always On  |
| ---- | ---- |
| Dispositivos ingressados no domínio com o requisito de SKUs do Enterprise. | Suporta VPN Always On ingressado no domínio, ingressado fora do domínio (grupo de trabalho) ou dispositivos de ingressados ao AD do Azure para permitir o enterprise e cenários de BYOD. VPN Always On está disponível em todas as edições do Windows e os recursos da plataforma estão disponíveis para terceiros por meio do suporte a plug-in VPN de UWP.<p><p>_**Observação.**_<br>Túnel de dispositivo pode ser configurado somente em dispositivos ingressados no domínio executando o Windows 10 Enterprise ou educação versão 1709 ou posterior. Não há nenhum suporte para controle de terceiros do dispositivo de túnel. |
| Suporte para IPv4 e IPv6. | Com VPN Always On, os usuários podem acessar recursos de IPv4 e IPv6 na rede corporativa. O cliente de VPN Always On usa uma abordagem de pilha dupla não especificamente depende de IPv6 ou a necessidade do gateway de VPN para fornecer serviços de tradução NAT64 ou DNS64. |
| Suporte para dois fatores ou autenticação de OTP. |A plataforma de VPN Always On nativamente dá suporte a EAP, que permite o uso dos tipos de EAP de terceiros e Microsoft diversificado como parte do fluxo de trabalho de autenticação. Especificamente, o VPN Always On dá suporte a cartão inteligente (física e virtual) e o Windows Hello para certificados de negócios para atender aos requisitos de autenticação de dois fatores. Além disso, sempre VPN dá suporte a OTP por meio de MFA (sem suporte nativamente, só tem suporte em plug-ins de terceiros) por meio da integração do EAP RADIUS.<p><p>Defina usando:<br>**VPNv2/ProfileName/NativeProfile/Authentication** |
| Suporte para vários domínios e florestas. | A plataforma de VPN Always On não tem dependência topologia florestas ou domínio de serviços de domínio Active Directory (AD DS) (ou níveis funcionais/esquema associado) porque ele não requer o cliente VPN para ser unido a função de domínio. Política de grupo, portanto, não é uma dependência para definir as configurações do perfil VPN porque você não usá-lo durante a configuração do cliente. Onde a integração de autorização do Active Directory é necessária, pode ser feito por meio do RADIUS como parte do processo de autenticação e autorização de EAP. |
| Suporte para ambos os dividir e forçar túnel para separação de tráfego de internet/intranet. | Você pode configurar a VPN Always On para dar suporte a ambos os túnel à força (o modo operacional padrão) e dividir túnel nativamente. VPN Always On fornece granularidade adicional para políticas de roteamento específico do aplicativo.<p><p>_**Observação.**_<br>Túnel do usuário só tem suporte.<p><p>Defina usando:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Suporte a vários protocolos. | VPN Always On pode ser configurado para dar suporte a SSTP nativamente se Secure Sockets Layer fallback de IKEv2 é necessária.<p><p>_**Observação.**_<br>Túnel de usuário dá suporte a SSTP e IKEv2 e túnel do dispositivo dá suporte a IKEv2 somente com não há suporte para o fallback SSTP.  |
| Assistente de conectividade para fornecer o status de conectividade corporativa. | VPN Always On está totalmente integrado com o Assistente de conectividade de rede nativa e fornece o status de conectividade da interface do modo de exibição de todas as redes. Com o advento do Windows 10 Creators Update (versão 1703), o status de conexão de VPN e o controle de conexão de VPN para túnel do usuário estão agora disponíveis por meio do submenu de rede (para o Windows interno cliente VPN), também. |
| Resolução de nomes de recursos corporativos usando o nome abreviado, o nome de domínio totalmente qualificado (FQDN) e o sufixo DNS. | VPN Always On nativamente pode definir um ou mais sufixos DNS como parte do processo de atribuição de endereço IP, incluindo a resolução de nome de recurso corporativo para nomes curtos, FQDNs ou espaços para nome DNS de inteiros e conexão de VPN. VPN Always On também suporta o uso de tabelas de diretiva de resolução de nome para fornecer a granularidade de resolução de namespace específicos.<p><p>_**Observação.**_<br>Evite o uso de sufixos Global conforme eles interferem na resolução de nome curto ao usar tabelas de política de resolução de nome.<p><p>Defina usando:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre os aprimoramentos de VPN Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia VPN Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Comece a planejar sua implantação de VPN Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
