---
title: Visão geral da migração de acesso VPN Always On remoto
description: VPN Always On aborda as lacunas anteriores entre Windows VPNs e o DirectAccess e como migrar do DirectAccess para VPN Always On.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845977"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Visão geral do DirectAccess a migração de VPN Always On 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

&#187; [**Next:** Planejar o DirectAccess para a migração de VPN Always On](da-always-on-migration-planning.md)

Nas versões anteriores da arquitetura de VPN do Windows, limitações da plataforma tornou difícil fornecer recursos essenciais necessários para substituir o DirectAccess, como conexões automáticas iniciadas antes que os usuários entrar. VPN Always On, no entanto, tiver atenuado a maioria dessas limitações ou expandida a funcionalidade VPN, além dos recursos do DirectAccess. VPN Always On aborda as lacunas anteriores entre Windows VPNs e o DirectAccess.

O DirectAccess – ao – sempre no processo de migração de VPN consiste em quatro componentes principais e os processos de alto nível:


1.  **Planeje a migração de VPN Always On.** Planejamento ajuda a identificar clientes de destino para a separação da fase de usuário, bem como a infraestrutura e a funcionalidade.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Implante uma infraestrutura VPN de lado a lado.** Depois de ter determinado suas fases de migração e os recursos que você deseja incluir em sua implantação, você pode implantar a infraestrutura de VPN Always On lado a lado com a infraestrutura existente do DirectAccess.  

3.  **Implante certificados e a configuração para os clientes.**  Depois que a infraestrutura VPN estiver pronta, criar e publicar os certificados necessários para o cliente. Quando os clientes receberam os certificados, você pode implantar o script de configuração VPN_Profile.ps1. Como alternativa, você pode usar o Intune para configurar o cliente VPN. Use o Microsoft System Center Configuration Manager ou o Microsoft Intune para monitorar implantações bem-sucedidas de configuração de VPN.

4.  **Remover e encerrar.** Remova o ambiente após você ter migrado todos off DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Cenário de implantação do DirectAccess

Nesse cenário de implantação, você usa um cenário simples de implantação do DirectAccess como um ponto de partida para a migração, que este guia apresenta. Você não precisa corresponder a esse cenário de implantação antes de migrar para VPN Always On, mas para muitas organizações, essa configuração simples é uma representação precisa de sua implantação atual do DirectAccess. A tabela a seguir fornece uma lista dos recursos básicos para que esta instalação.

Muitas opções e cenários de implantação do DirectAccess existem, portanto, sua implementação é provavelmente será diferente daquele descrito aqui. Nesse caso, consulte [mapeamento de recurso entre o DirectAccess e VPN Always On](../vpn/vpn-map-da.md) para determinar o recurso de VPN Always On definir o mapeamento para suas adições atuais e, em seguida, adicione esses recursos à sua configuração. Além disso, você pode consultar a [aprimoramentos de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md) adicionar opções à sua implantação de VPN Always On.

>[!NOTE] 
>Para dispositivos ingressados em fora do domínio, há considerações adicionais, como o registro de certificado. Para obter detalhes, consulte [Always On VPN implantação para o Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Lista de recursos do cenário de implantação

| Recurso DirectAccess | Cenário típico |
|-----|----|
| Cenário de implantação                   | Implantar o DirectAccess completo para acesso para cliente e o gerenciamento remoto                                               |
| Adaptadores de Rede                      | 2                                                                                                              |
| Autenticação de usuário                   | Credenciais do Active Directory                                                                                   |
| Usar certificados de computador             | Sim                                                                                                            |
| Grupos de segurança                       | Sim                                                                                                            |
| Único servidor DirectAccess            | Sim                                                                                                            |
| Topologia de rede                      | NAT (NAT) atrás de um firewall de borda com dois adaptadores de rede                            |
| Modo de acesso                           | End à borda                                                                                                    |
| Túnel                             | Túnel dividido                                                                                                   |
| Autenticação                        | Autenticação padrão infraestrutura de chave pública (PKI) com certificado de computador além de Kerberos (não KerbProxy) |
| Protocolos                             | IP sobre HTTPS (IP-HTTPS)                                                                                       |
| Rede local NLS (servidor) prontas para uso | Sim                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Cenário de implantação de VPN Always On

Nesse cenário de implantação, você se concentrar em migrar de um ambiente simples do DirectAccess para um ambiente de VPN Always On simples, que é a solução de substituição do DirectAccess. A tabela a seguir fornece os recursos usados nesta solução simple. Para obter mais informações sobre os aprimoramentos adicionais para o cliente VPN Always On, consulte [aprimoramentos de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Recursos de VPN Always On usados no ambiente simple

| Recurso de VPN | Configuração de cenário de implantação |
|-----|-----|
| Tipo de conexão | Nativo, o protocolo IKE versão 2 (IKEv2) |
| Adaptadores de Rede   | 2        |
| Autenticação de usuário  | Credenciais do Active Directory            |
| Usar certificados de computador        | Sim                          |
| Roteamento | Túnel dividido |
| Resolução de nome | Lista de informações de nome de domínio e o sufixo do sistema de nome de domínio (DNS) |
| Disparar | Detecção de rede sempre ativado e confiável |
| Autenticação  | Protegido Extensible Authentication Protocol-Transport Layer Security (PEAP-TLS) com certificados de usuário protegido – Trusted Platform Module |

## <a name="next-step"></a>Próximas etapas

[Planejar o DirectAccess para a migração de VPN Always On](da-always-on-migration-planning.md). É o principal objetivo da migração para os usuários a manter a conectividade remota ao escritório durante todo o processo.

---