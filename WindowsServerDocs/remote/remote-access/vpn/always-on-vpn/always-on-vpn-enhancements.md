---
title: Aprimoramentos de VPN Always On
description: VPN Always On traz muitos benefícios em relação às soluções de VPN do Windows do passado. Principais aprimoramentos na integração, segurança, conectividade, controle de rede e compatibilidade alinham VPN Always On com visão de nuvem e dispositivos móveis da Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: e7cbe64609e30b042df70020896aebd85b7d22b9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280783"
---
# <a name="always-on-vpn-enhancements"></a>Aprimoramentos de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

- [**Anterior:** Saiba mais sobre os recursos de VPN Always On](../vpn-map-da.md)
- [**Avançar:** Saiba mais sobre a tecnologia VPN Always On](always-on-vpn-technology-overview.md)

VPN Always On traz muitos benefícios em relação às soluções de VPN do Windows do passado. Os seguintes aprimoramentos principais alinham VPN Always On com visão de nuvem e dispositivos móveis da Microsoft:

- **Integração de plataforma:** VPN Always On tem integração aprimorada com o sistema de operacional Windows e soluções de terceiros para fornecer uma plataforma robusta para inúmeros cenários de conexão avançada.

- **Segurança:** VPN Always On tem recursos de segurança novos e avançados para restringir o tipo de tráfego, quais aplicativos podem usar a conexão VPN, e quais métodos de autenticação que você pode usar para iniciar a conexão. Quando a conexão está ativa na maioria das vezes, é especialmente importante proteger a conexão. Para obter mais detalhes, consulte [opções de autenticação VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-authentication).

- **Conectividade VPN:** VPN Always On, com ou sem o túnel de dispositivo fornece a capacidade de disparo automático. Antes de VPN Always On, a capacidade de disparar uma conexão automática por meio do usuário ou autenticação de dispositivo não foi possível.  

- **Controle de rede:** VPN Always On permite aos administradores especificar políticas de roteamento em um nível mais granular — até mesmo o aplicativos individuais — que é perfeito para aplicativos de linha de negócios (LOB) que requerem acesso remoto especial.  VPN Always On também é totalmente compatível com ambos os protocolo IP versão 4 (IPv4) e versão 6 (IPv6). Ao contrário do DirectAccess, não há nenhuma dependência específica no IPv6.

  >[!NOTE]
  >Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro.

- **Configuração e compatibilidade:** VPN Always On podem ser implantado e gerenciado de diversas maneiras, que oferece várias vantagens em relação a outros softwares de cliente VPN de VPN Always On.

## <a name="platform-integration"></a>Integração de plataforma

Microsoft tem introduzidos ou aprimorados os seguintes recursos de integração em VPN Always On:


| Melhoria-chave   |  Descrição  |
|----------------|---|
| **[Proteção de informações do Windows (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integração com WIP permite a imposição de política de rede determinar se o tráfego é permitido para falar sobre a VPN. Se o perfil do usuário está ativo e as políticas de WIP são aplicadas, VPN Always On é automaticamente disparada para se conectar. Além disso, quando você usa o WIP, não é necessário para especificar regras AppTriggerList e TrafficFilterList separadamente no perfil de VPN (a menos que você deseja a configuração mais avançada) porque as listas de aplicativo e as políticas de WIP entram em vigor automaticamente. |
|**[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** |VPN Always On nativamente dá suporte ao Windows Hello para empresas (no modo de autenticação baseada em certificado) para fornecer uma experiência perfeita único logon para ambos os entrar na máquina e conexão à VPN. Portanto, nenhuma autenticação secundária (credenciais de usuário) é necessário para a conexão VPN, tornando possível usar uma conexão sempre ativa com o Windows Hello para autenticação de negócios. |
| **[Acesso condicional do Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)**  |O cliente VPN Always On pode integrar com a plataforma de acesso condicional do Azure para impor a autenticação multifator (MFA), a conformidade do dispositivo ou uma combinação dos dois. Quando estiver em conformidade com políticas de acesso condicional, o Azure Active Directory (Azure AD) emite um certificado de autenticação de segurança de IP (IPsec) e de curta duração (por padrão, 60 minutos) que, em seguida, pode ser usado para autenticar para o gateway de VPN. Conformidade do dispositivo usa políticas de conformidade do sistema de Center Configuration Manager/Intune, que podem incluir o estado de atestado de integridade do dispositivo como parte da verificação de conformidade de conexão.|
|  **Azure MFA** |Quando combinado com os serviços de RADIUS Remote Authentication Dial-In usuário Service () e a extensão do servidor de diretivas de rede (NPS) para o Azure MFA, autenticação de VPN pode usar a MFA forte. | **Plug-in de VPN de terceiros**  | Com a Universal Windows Platform (UWP), provedores VPN de terceiros podem criar um aplicativo único para o intervalo completo do Windows 10 dispositivos. A UWP fornece uma camada de API do núcleo garantida entre dispositivos, eliminando a complexidade do e os problemas geralmente associados ao escrever drivers de nível de kernel. Atualmente, os plug-ins de VPN do Windows 10 UWP existentes [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [acesso F5](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [ SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz), e [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sem dúvida, outras pessoas aparecerá no futuro. |

## <a name="security"></a>Segurança

As principais melhorias na segurança estão nas seguintes áreas:

| Melhoria-chave | Descrição  |
|---|---|
| **Filtros de tráfego** | Por meio de filtros de tráfego, você pode especificar as políticas do lado do cliente que determinam qual tráfego é permitido na rede corporativa. Dessa forma, os administradores podem aplicar restrições de aplicativo ou o tráfego na interface de VPN, limitando seu uso para fontes específicas, as portas de destino e endereços IP. Há dois tipos de regras de filtragem:<ul><li>**Regras com base no aplicativo.** Regras de firewall baseado em aplicativo baseiam-se em uma lista de aplicativos especificados para que somente o tráfego originado desses aplicativos têm permissão para falar sobre a interface VPN.</li><li>**Regras de tráfego.** Regras de firewall com base no tráfego são baseadas nos requisitos de rede, como protocolos, portas e endereços. Use essas regras apenas para o tráfego que corresponder a essas condições específicas são permitidas para falar sobre a interface VPN.<p><p>***Observação:***<br>Essas regras se aplicam apenas ao tráfego de saída do dispositivo. Uso de tráfego filtra o tráfego de entrada blocos da rede corporativa para o cliente. </li></ul> |**VPN por aplicativo**|VPN por aplicativo é como ter um filtro de tráfego baseado em aplicativo, mas ele vai além disso, para combinar os gatilhos de aplicativo com um filtro de tráfego baseado em aplicativo para que a conectividade VPN é restrito a um aplicativo específico em vez de todos os aplicativos no cliente VPN. O recurso iniciará automaticamente quando o aplicativo é iniciado.|
|  **Suporte para algoritmos de criptografia IPsec personalizados**   |  VPN Always On suporta o uso de RSA e da curva elíptica baseados em criptografia personalizado algoritmos criptográficos para atender aos rigorosos governamentais ou políticas de segurança organizacional.|
| **Suporte nativo do protocolo EAP (Extensible Authentication)** |VPN Always On nativamente dá suporte a EAP, que permite que você use um conjunto diversificado de Microsoft e os tipos de EAP de terceiros como parte do fluxo de trabalho de autenticação. O EAP fornece autenticação segura com base nos seguintes tipos de autenticação:<ul><li>Nome de usuário e senha</li><li>Cartão inteligente (física e virtual)</li><li>Certificados do usuário</li><li>Windows Hello para Empresas</li><li>Suporte a MFA por meio da integração do EAP RADIUS</li></ul>O fornecedor do aplicativo controla os métodos de autenticação de plug-in de UWP VPN de terceiros, apesar de terem uma matriz de opções disponíveis, incluindo tipos de credenciais personalizado e o suporte OTP.|

## <a name="vpn-connectivity"></a>Conectividade VPN

Estes são os aprimoramentos principais na conectividade de VPN Always On:

|  Melhoria-chave  | Descrição |
|---|---|
|                    **Sempre ativo**                    |                                                                                          Always On é um recurso do Windows 10 que permite que o perfil VPN ativo para se conectar automaticamente e permanecer conectado com base nos gatilhos — ou seja, entrada do usuário, alteração de estado da rede ou Active Directory de tela do dispositivo. Always On também é integrado com a experiência de espera conectada para maximizar a vida útil da bateria.                                                                                           |
|             **Disparo de aplicativo**              |                                                                                                                                          Você pode configurar perfis de VPN no Windows 10 para conectar-se automaticamente na inicialização de um conjunto especificado de aplicativos. Você pode configurar a área de trabalho e aplicativos de UWP para disparar uma conexão VPN.                                                                                                                                          |
|              **Com base no nome de disparo**              |                                                                                                            Com VPN Always On, você pode definir regras para que as consultas de nome de domínio específico disparam a conexão VPN. Windows 10 agora oferece suporte com base no nome do gatilho de computadores ingressados no domínio e ingressados fora do domínio (anteriormente, havia suporte para somente computadores que ingressaram em fora do domínio).                                                                                                            |
|            **Detecção de rede confiável**            |                                                                                    VPN Always On inclui esse recurso para garantir a conectividade VPN não será disparada se um usuário estiver conectado a uma rede confiável dentro do limite corporativo. Você pode combinar esse recurso com qualquer um dos métodos disparo mencionados anteriormente, para fornecer uma experiência de usuário perfeita "se conectar somente quando necessário".                                                                                     |
| **[Dispositivo de túnel](../vpn-device-tunnel-config.md)** | VPN Always On fornece a capacidade de criar um perfil VPN dedicado para o computador ou dispositivo. Diferentemente *usuário túnel*, que se conecta somente depois de um usuário faz logon no dispositivo ou computador, *túnel do dispositivo* permite que a VPN estabelecer a conectividade antes de entrada do usuário. Dispositivo de túnel e o túnel do usuário operam de forma independente com seus perfis VPN, podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras definições de configuração de VPN conforme apropriado. |

## <a name="networking"></a>Rede

A seguir está alguns dos aprimoramentos de rede em VPN Always On:


|              Melhoria-chave              |                                                                                                                                                                                                                Descrição                                                                                                                                                                                                                |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Suporte de pilha dupla para IPv4 e IPv6**  | VPN Always On nativamente suporta o uso de IPv4 e IPv6 em uma abordagem de pilha dupla. Ele tem nenhuma dependência específica em um protocolo em detrimento do outro, que permite a máxima compatibilidade de aplicativos IPv4/IPv6 combinada com o suporte para IPv6 futuras necessidades de rede.<p>***Observação:*** Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro. |
| **Políticas de roteamento específico do aplicativo** |                            Além de definir global VPN conexão políticas de roteamento para separação de tráfego de internet e intranet, é possível adicionar políticas de roteamento para controlar o uso de túnel dividido ou forçar as configurações de túnel em uma base por aplicativo. Essa opção fornece um controle mais granular sobre quais aplicativos têm permissão para interagir com os quais recursos por meio do túnel VPN.                             |
|           **Rotas de exclusão**            |                 VPN Always On dá suporte à capacidade de especificar as rotas de exclusão que especificamente controlam o comportamento de roteamento para definir qual o tráfego deve percorrer a VPN apenas e não passam pela interface de rede física.<p><p>***Observações:***<br>-Rotas exclusão funcionam atualmente para o tráfego dentro da mesma sub-rede que o cliente, por exemplo, LinkLocal.<br>-Rotas exclusão só funcionam em uma configuração de túnel dividido.                  |

## <a name="configuration-and-compatibility"></a>Configuração e compatibilidade 

Seguem algumas das melhorias de configuração e compatibilidade em VPN Always On:


|                 Melhoria-chave                  |                                                                                                                                                                                                                                                                                                                       Descrição                                                                                                                                                                                                                                                                                                                       |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    **Compatibilidade de gateway VPN de terceiros**     | O cliente VPN Always On não exige o uso de um gateway VPN com base em Microsoft para operar. Por meio do suporte do protocolo IKEv2, o cliente facilita a interoperabilidade com gateways de VPN de terceiros que dão suporte a esse tipo de túnel de padrão da indústria. Você também pode obter interoperabilidade com gateways de VPN de terceiros usando um plug-in de VPN de UWP combinado com um tipo personalizado de túnel sem sacrificar os benefícios e recursos da plataforma de VPN Always On.<p><p>***Observação:***<br>Consulte seu fornecedor de dispositivo do back-end de gateway ou de terceiros em configurações e compatibilidade com o túnel de dispositivo usando o IKEv2 e VPN Always On. |
| **Suporte de protocolo VPN IKEv2 padrão do setor** |                                                                                                                                                                                                                              O cliente VPN Always On dá suporte a IKEv2, um de hoje mais amplamente usado protocolos de encapsulamento de padrão da indústria. Essa compatibilidade maximiza a interoperabilidade com gateways de VPN de terceiros.                                                                                                                                                                                                                               |
|               **Suporte de plataforma**               |                                                                                                                                                                                                           Suporta AAlways em VPN ingressado no domínio, ingressado fora do domínio (grupo de trabalho) ou dispositivos de ingressados ao AD do Azure para permitir as duas empresas e traga seu próprio dispositivo) cenários de BYOD. Além disso, a VPN Always On está disponível em todas as edições do Windows.                                                                                                                                                                                                           |
| **Diversos mecanismos de implantação e gerenciamento** |                                                                                                                                 Você pode usar vários mecanismos de implantação e gerenciamento para gerenciar as configurações de VPN (chamado de um *perfil de VPN*), incluindo o Windows PowerShell, o System Center Configuration Manager, o Intune ou ferramenta de gerenciamento (MDM) do dispositivo móvel de terceiros, e o Designer de configuração do Windows. Essas opções simplificam a configuração de VPN Always On, independentemente das ferramentas de gerenciamento de cliente que você usar.                                                                                                                                 |
|     **Definição de perfil VPN padronizada**      |                                                                                                                                                                                                                                  VPN Always On dá suporte à configuração usando um perfil padrão de XML (ProfileXML), fornecendo um formato de modelo de configuração padrão e a maioria dos conjuntos de ferramentas de implantação usam.                                                                                                                                                                                                                                   |

## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia VPN Always On](always-on-vpn-technology-overview.md)

- [Comece a planejar sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)