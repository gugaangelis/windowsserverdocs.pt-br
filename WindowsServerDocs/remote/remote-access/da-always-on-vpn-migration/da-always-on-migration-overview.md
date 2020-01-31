---
title: Visão geral da migração de VPN Always On acesso remoto
description: Always On VPN aborda as lacunas anteriores entre as VPNs do Windows e o DirectAccess e como migrar do DirectAccess para Always On VPN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: d3ea6f0e29803b8a709f31811f77678bf03201a8
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822570"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Visão geral do DirectAccess a migração de VPN Always On 

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

&#187;[ **Em seguida:** planejar o directaccess para Always on migração de VPN](da-always-on-migration-planning.md)

Nas versões anteriores da arquitetura de VPN do Windows, as limitações de plataforma tornaram difícil fornecer a funcionalidade crítica necessária para substituir o DirectAccess, como conexões automáticas iniciadas antes que os usuários entrem. O Always On VPN, no entanto, reduziu a maioria dessas limitações ou expandiu a funcionalidade de VPN além dos recursos do DirectAccess. Always On VPN atende às lacunas anteriores entre as VPNs do Windows e o DirectAccess.

O processo de migração de VPN do DirectAccess para o Always On consiste em quatro componentes principais e processos de alto nível:


1.  **Planeje a migração de VPN Always On.** O planejamento ajuda a identificar clientes de destino para a separação da fase do usuário, bem como a infraestrutura e a funcionalidade.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Implante uma infraestrutura de VPN lado a lado.** Depois de determinar suas fases de migração e os recursos que você deseja incluir em sua implantação, implante a infraestrutura de VPN Always On lado a lado com a infraestrutura de DirectAccess existente.  

3.  **Implantar certificados e configuração para os clientes.**  Quando a infraestrutura de VPN estiver pronta, você criará e publicará os certificados necessários no cliente. Quando os clientes tiverem recebido os certificados, você implantará o script de configuração VPN_Profile. ps1. Como alternativa, você pode usar o Intune para configurar o cliente VPN. Use o Microsoft Endpoint Configuration Manager ou Microsoft Intune para monitorar implantações de configuração de VPN bem-sucedidas.

4.  **Remover e desativar.** Encerre o ambiente corretamente depois de migrar todos fora do DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Cenário de implantação do DirectAccess

Nesse cenário de implantação, você usa um cenário de implantação simples do DirectAccess como um ponto de partida para a migração que este guia apresenta. Você não precisa corresponder a esse cenário de implantação antes de migrar para Always On VPN, mas para muitas organizações, essa configuração simples é uma representação precisa da sua implantação atual do DirectAccess. A tabela a seguir fornece uma lista de recursos básicos para essa configuração.

Há muitas opções e cenários de implantação do DirectAccess, portanto, sua implementação provavelmente será diferente da descrita aqui. Nesse caso, consulte [mapeamento de recursos entre o DirectAccess e Always on VPN](../vpn/vpn-map-da.md) para determinar o mapeamento do conjunto de recursos de VPN Always on para suas adições atuais e, em seguida, adicione esses recursos à sua configuração. Além disso, você pode consultar o [Always on aprimoramentos de VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md) para adicionar opções à sua implantação de VPN Always on.

>[!NOTE] 
>Para dispositivos não ingressados no domínio, há considerações adicionais, como registro de certificado. Para obter detalhes, consulte [implantação de VPN Always on para Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Lista de recursos do cenário de implantação

| Recurso do DirectAccess | Cenário típico |
|-----|----|
| Cenário de implantação                   | Implantar o DirectAccess completo para acesso de cliente e gerenciamento remoto                                               |
| Adaptadores de Rede                      | 2                                                                                                              |
| Autenticação de usuário                   | Credenciais do Active Directory                                                                                   |
| Usar certificados de computador             | Sim                                                                                                            |
| Grupos de segurança                       | Sim                                                                                                            |
| Servidor DirectAccess único            | Sim                                                                                                            |
| Topologia de rede                      | NAT (conversão de endereços de rede) por trás de um firewall de borda com dois adaptadores de rede                            |
| Modo de acesso                           | Ponta a borda                                                                                                    |
| Túnel                             | Túnel dividido                                                                                                   |
| Authentication                        | Autenticação de PKI (infraestrutura de chave pública) padrão com certificado de computador mais Kerberos (não KerbProxy) |
| Protocolos                             | IP sobre HTTPS (IP-HTTPS)                                                                                       |
| NLS (servidor de local de rede) fora da caixa | Sim                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Cenário de implantação de VPN Always On

Nesse cenário de implantação, você se concentra em migrar um ambiente simples do DirectAccess para um ambiente de VPN Always On simples, que é a solução de substituição do DirectAccess. A tabela a seguir fornece os recursos usados nesta solução simples. Para obter informações mais detalhadas sobre os aprimoramentos adicionais para o cliente VPN Always On, consulte [Always on aprimoramentos de VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Always On recursos de VPN usados no ambiente simples

| Recurso de VPN | Configuração do cenário de implantação |
|-----|-----|
| Tipo de conexão | Native protocolo IKE versão 2 (IKEv2) |
| Adaptadores de Rede   | 2        |
| Autenticação de usuário  | Credenciais do Active Directory            |
| Usar certificados de computador        | Sim                          |
| Roteamento | Túnel dividido |
| Resolução de nomes | Lista de informações de nome de domínio e sufixo DNS (sistema de nomes de domínio) |
| Disparar | Detecção de rede confiável e AlwaysOn |
| Authentication  | Protocolo de autenticação extensível protegido – PEAP-TLS (segurança de camada de transporte) com Trusted Platform Module – certificados de usuário protegidos |

## <a name="next-step"></a>Próximas etapas

[Planeje o DirectAccess para Always on migração de VPN](da-always-on-migration-planning.md). O objetivo principal da migração é que os usuários mantenham a conectividade remota com o escritório durante todo o processo.

---