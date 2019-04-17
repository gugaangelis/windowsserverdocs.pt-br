---
title: Aprimoramentos de VPN Always On
description: VPN Always On tem muitos benefícios sobre as soluções de VPN do Windows do passado. Principais melhorias na integração, segurança, controle de rede, conectividade e compatibilidade alinhem VPN Always On a visão de prioriza a nuvem, prioriza da Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066974"
---
# Aprimoramentos de VPN Always On

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

& #171; [ **Anterior:** Saiba mais sobre os recursos de VPN Always On](../vpn-map-da.md)<br>
& #187; [ **Próximo:** Saiba mais sobre a tecnologia de VPN Always On](always-on-vpn-technology-overview.md)

VPN Always On tem muitos benefícios sobre as soluções de VPN do Windows do passado. As seguintes melhorias chaves alinhem VPN Always On a visão de prioriza a nuvem, prioriza da Microsoft:

- **Integração de plataforma:** VPN Always On tem melhor integração com o sistema operacional Windows e soluções de terceiros para fornecer uma plataforma robusta para inúmeros cenários avançados de conexão.

- **Segurança:** VPN Always On tem recursos de segurança novos e avançados para restringir o tipo de tráfego, quais aplicativos podem usar a conexão VPN, e quais métodos de autenticação que você pode usar para iniciar a conexão. Quando a conexão está ativo na maioria das vezes, é especialmente importante proteger a conexão. Para obter mais detalhes, consulte as [Opções de autenticação de VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **Conectividade VPN:** VPN Always On, com ou sem o encapsulamento do dispositivo fornece funcionalidade de disparo automático. Antes de VPN Always On, a capacidade de disparar uma conexão automática por meio do usuário ou autenticação de dispositivo não foi possível.  

- **Controle de rede:** VPN Always On permite que os administradores especifiquem diretivas de roteamento em um nível mais granular — até mesmo para o aplicativo individual — que é perfeito para aplicativos de linha de negócios (LOB) que exigem acesso remoto especial.  VPN Always On também é totalmente compatível com ambos os protocolo IP versão 4 (IPv4) e versão 6 (IPv6). Ao contrário do DirectAccess, não há nenhuma dependência específica em IPv6.

  >[!Note]
  >Antes de começar, certifique-se de habilitar IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro.
  
- **Configuração e compatibilidade:** VPN Always On podem ser implantado e gerenciado de várias maneiras, que dá a VPN Always On várias vantagens em relação a outros softwares de cliente VPN.

## Integração de plataforma

Microsoft tem introduzidos ou aprimorados os seguintes recursos de integração em VPN Always On:

| Melhoria-chave | Descrição |
| ---- | ---- |
| **[Proteção de Informações do Windows (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integração com a WIP permite que a aplicação de políticas de rede determinar se o tráfego é permitido para passar pela VPN. Se o perfil do usuário está ativo e políticas WIP são aplicadas, VPN Always On é disparada automaticamente para se conectar. Além disso, quando você usa o WIP, não é necessário especificar regras AppTriggerList e TrafficFilterList separadamente no perfil da VPN (a menos que você queira configuração mais avançada) porque as políticas WIP e listas de aplicativos automaticamente entrem em vigor. |
| **[Windows Hello para Empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | VPN Always On nativamente compatível com o Windows Hello para empresas (no modo de autenticação baseada em certificado) para fornecer uma única logon experiência perfeita para ambos os entrar no computador e conexão à VPN. Portanto, nenhuma autenticação secundária (credenciais de usuário) é necessária para a conexão VPN, tornando possível usar uma conexão sempre ativa com o Windows Hello para empresas a autenticação. |
| **[Acesso condicional do Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | O cliente VPN Always On pode se integrar com a plataforma de acesso condicional do Azure para reforçar a autenticação multifator (MFA), conformidade do dispositivo ou uma combinação dos dois. Quando estiver em conformidade com políticas de acesso condicional, o Azure Active Directory (Azure AD) emite um certificado de autenticação de segurança de IP (IPsec) curta (por padrão, 60 minutos) que pode ser usado para autenticar para o gateway VPN. Conformidade do dispositivo usa as políticas de conformidade do System Center Configuration Manager/Intune, que podem incluir o estado de atestado de integridade do dispositivo como parte da verificação de conformidade de conexão. |
| **Azure MFA** | Quando combinado com serviços remoto Authentication Dial-In User Service (RADIUS) e a extensão do servidor de política de rede (NPS) para o Azure MFA, autenticação VPN pode usar a MFA forte. |
| **Plug-in de VPN de terceiros** | Com o Universal Windows Platform (UWP), provedores VPN de terceiros podem criar um único aplicativo para o intervalo completo do Windows 10 dispositivos. A UWP fornece uma camada de API básica garantida entre dispositivos, eliminando a complexidade da e os problemas geralmente associados à gravação drivers de nível de kernel. Atualmente, plug-ins de VPN do Windows 10 UWP existentes [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)e [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sem dúvida, outros aparecerá no futuro. |
---

## Segurança

São as principais melhorias na segurança nas seguintes áreas:

| Melhoria-chave | Descrição |
| ---- | ---- |
| **Filtros de tráfego** | Por meio de filtros de tráfego, você pode especificar políticas no lado do cliente para determinar qual tráfego é permitido na rede corporativa. Dessa forma, os administradores podem aplicar restrições de aplicativo ou tráfego na interface VPN, limitando seu uso fontes específicas, portas de destino e endereços IP. Dois tipos de regras de filtragem estão disponíveis:<ul><li>**Regras baseadas em aplicativo.** Regras de firewall baseadas em aplicativo são baseadas em uma lista de aplicativos especificados para que apenas o tráfego originário desses aplicativos têm permissão para passar pela interface VPN.</li><li>**Regras baseadas em tráfego.** Regras de firewall baseadas em tráfego são com base nos requisitos de rede como portas, endereços e protocolos. Use essas regras somente para o tráfego que corresponde a essas condições específicas têm permissão para passar pela interface VPN.<p><p>_**Observação.**_<br>Essas regras se aplicam apenas ao tráfego de saída do dispositivo. Uso de tráfego filtros de tráfego de entrada blocos da rede corporativa para o cliente. </li></ul> |
| **VPN por aplicativo** | VPN por aplicativo é como ter um filtro de tráfego baseado em aplicativo, mas ele vai além para combinar disparadores de aplicativo com um filtro de tráfego baseado em aplicativo para que a conectividade VPN é restrito a um aplicativo específico em vez de todos os aplicativos no cliente VPN. O recurso inicia automaticamente quando o aplicativo é iniciado. |
| **Suporte para algoritmos de criptografia IPsec personalizados** | VPN Always On oferece suporte ao uso de RSA e curva elíptica baseada em criptografia personalizado algoritmos criptográficos para atender aos rígidos governo dos Estados Unidos ou políticas de segurança organizacional. |
| **Suporte nativo do protocolo EAP (Extensible Authentication)** | VPN Always On suporta nativamente EAP, que permite que você use um conjunto variado de Microsoft e tipos de EAP de terceiros como parte do fluxo de trabalho de autenticação. EAP fornece autenticação segura com base nos seguintes tipos de autenticação:<ul><li>Nome de usuário e senha</li><li>Cartão inteligente (físico e virtual)</li><li>Certificados de usuário</li><li>Windows Hello para Empresas</li><li>Suporte MFA por meio de integração de EAP RADIUS</li></ul>O fornecedor do aplicativo controla os métodos de autenticação de plug-in de VPN UWP de terceiros, embora eles têm uma matriz de opções disponíveis, incluindo os tipos de credencial personalizada e suporte OTP. |
---

## Conectividade VPN

Estas são as principais melhorias na conectividade de VPN Always On:

| Melhoria-chave | Descrição |
| ---- | ---- |
| **Sempre Ativo** | Sempre ativado é um recurso do Windows 10 que permite que o perfil ativo de VPN para se conectar automaticamente e permanecer conectado com base em acionadores — ou seja, usuário entrar, alteração de estado de rede ou ativo de tela do dispositivo. Sempre ativado também está integrado à experiência de modo de espera conectada para maximizar a duração da bateria. |
| **Disparo de aplicativo** | Você pode configurar perfis VPN no Windows 10 para se conectar automaticamente durante a inicialização de um conjunto especificado de aplicativos. Você pode configurar a área de trabalho e aplicativos UWP para disparar uma conexão VPN. |
| **Disparo com base em nome** | Com VPN Always On, você pode definir regras para que as consultas de nomes de domínio específico disparam a conexão VPN. Windows 10 agora oferece suporte com base em nome de disparo para computadores ingressados em domínio e fora do domínio associado (anteriormente, somente fora computadores tinham suporte). |
| **Detecção de rede confiável** | VPN Always On inclui esse recurso para garantir que a conectividade VPN não será acionada se um usuário estiver conectado a uma rede confiável dentro dos limites corporativos. Você pode combinar esse recurso com qualquer um dos métodos que disparou mencionados anteriormente para fornecer uma experiência de usuário perfeita "conectar somente quando necessário". |
| **[Encapsulamento do dispositivo](../vpn-device-tunnel-config.md)** | VPN Always On oferece a capacidade de criar um perfil de VPN dedicado para o dispositivo ou computador. Ao contrário de _Encapsulamento do usuário_, que só se conecta após um usuário faz logon no dispositivo ou computador, _Encapsulamento do dispositivo_ permite que a VPN estabelecer a conectividade antes de usuário entrar. Encapsulamento do dispositivo e usuário encapsulamento operam de forma independente com seus perfis VPN, podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras configurações de VPN conforme apropriado. |
---

## Rede

Estes são alguns das melhorias de rede VPN Always On:

| Melhoria-chave | Descrição |
| ---- | ---- |
| **Suporte de pilha dupla para IPv4 e IPv6** | VPN Always On nativamente oferece suporte ao uso de IPv4 e IPv6 em uma abordagem de pilha dupla. Ele tem nenhuma dependência específica em um protocolo sobre o outro, que permite a compatibilidade de aplicativos de IPv4/IPv6 máxima combinada com suporte a IPv6 futuras necessidades de rede.<p>**_Observação._** Antes de começar, certifique-se de habilitar IPv6 no servidor VPN. Caso contrário, não é possível estabelecer uma conexão e exibe uma mensagem de erro.|
| **Políticas de roteamento específicas do aplicativo** | Além de definir políticas roteamento do globais conexão de VPN para separação de tráfego de internet e intranet, é possível adicionar políticas de roteamento para controlar o uso do encapsulamento de divisão ou forçar configurações de túnel em uma base por aplicativo. Essa opção oferece um controle mais granular sobre quais aplicativos têm permissão para interagir com os quais recursos através do túnel VPN. |
| **Rotas de exclusão** | VPN Always On oferece suporte a capacidade de especificar rotas de exclusão que especificamente controlam o comportamento de roteamento para definir qual tráfego deve percorrer a VPN apenas e não passar pela interface de rede física.<p><p>_**Notas.**_<br>-Rotas de exclusão atualmente funcionam para tráfego dentro da mesma sub-rede que o cliente, por exemplo, LinkLocal.<br>-Rotas de exclusão funcionam somente em uma configuração de encapsulamento de divisão. |
---

## Configuração e compatibilidade 

A seguir estão algumas das melhorias de configuração e compatibilidade no VPN Always On:

| Melhoria-chave | Descrição |
| ---- | ---- |
| **Compatibilidade de gateway VPN de terceiros** | O cliente VPN Always On não exige o uso de um gateway VPN com base em Microsoft para operar. Por meio do suporte do protocolo IKEv2, o cliente facilita a interoperabilidade com gateways VPN de terceiros que dão suporte a esse tipo de encapsulamento padrão do setor. Você também pode obter interoperabilidade com gateways VPN de terceiros usando um plug-in de VPN de UWP combinado com um tipo personalizado de encapsulamento sem sacrificar benefícios e os recursos de plataforma de VPN Always On.<p><p>_**Observação.**_<br>Consulte seu fornecedor de back-end appliance gateway ou de terceiros em configurações e compatibilidade com VPN Always On e encapsulamento do dispositivo usando IKEv2. |
| **Suporte a protocolos VPN IKEv2 padrão do setor** | O cliente VPN Always On oferece suporte a IKEv2, um dos hoje mais amplamente usado protocolos de encapsulamento padrão do setor. Compatibilidade maximiza a interoperabilidade com gateways VPN de terceiros. |
| **Suporte de plataforma** | Suporta AAlways em VPN ingressado no domínio, fora do domínio associado (grupo de trabalho) ou dispositivos Azure – ingressado no AD para permitir para ambas as empresas cenários e traga seu próprio dispositivo (BYOD). Além disso, a VPN Always On está disponível em todas as edições do Windows. |
| **Diversos mecanismos de gerenciamento e implantação** | Você pode usar vários mecanismos de implantação e gerenciamento para gerenciar configurações de VPN (chamadas de um _perfil de VPN_), incluindo o Windows PowerShell, System Center Configuration Manager, o Intune (ou dispositivo móvel de terceiros [] ferramenta de gerenciamento MDM) e Windows Designer de configuração. Essas opções simplificam a configuração de VPN Always On independentemente as ferramentas de gerenciamento de cliente que você usar. |
| **Definição de perfil VPN padronizada** | VPN Always On oferece suporte a configuração usando um perfil XML padrão (ProfileXML), fornecendo um formato de modelo de configuração padrão que a maioria dos conjuntos de ferramentas de implantação e o gerenciamento usam. |
---


## Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia de VPN Always On](always-on-vpn-technology-overview.md)

- [Começar a planejar sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)



---
