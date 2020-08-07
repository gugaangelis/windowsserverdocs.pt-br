---
title: Acesso remoto Always On planejamento de migração de VPN
description: Migrar do DirectAccess para Always On VPN requer um planejamento adequado para determinar suas fases de migração, o que ajuda a identificar quaisquer problemas antes que eles afetem toda a organização.
manager: dougkim
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: 57b44b90213ca756666b83aa934bc6fd42fa609b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951421"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planejar o migração de VPN Always On do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

&#171; [ **anterior:** visão geral do directaccess para Always on migração de VPN](da-always-on-migration-overview.md)<br>
&#187; [ **Avançar:** migrar para Always on VPN e encerrar o DirectAccess](da-always-on-migration-deploy.md)


Migrar do DirectAccess para Always On VPN requer um planejamento adequado para determinar suas fases de migração, o que ajuda a identificar quaisquer problemas antes que eles afetem toda a organização. O objetivo principal da migração é que os usuários mantenham a conectividade remota com o escritório durante todo o processo. Se você executar tarefas fora de ordem, poderá ocorrer uma condição de corrida, deixando os usuários remotos sem como acessar os recursos da empresa. Portanto, a Microsoft recomenda executar uma migração planejada lado a lado do DirectAccess para Always On VPN. Para obter detalhes, consulte a seção [Always on implantação de migração de VPN](da-always-on-migration-deploy.md) .

A seção descreve os benefícios de separar usuários para a migração, considerações de configuração padrão e Always On aprimoramentos de recursos de VPN. A fase de planejamento de migração inclui:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)]

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)]

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Criar anéis de migração
Os anéis de migração são usados para dividir o Always On esforço de migração de cliente VPN em várias fases. No momento que chegar à última fase, seu processo deverá ser bem testado e consistente.

Esta seção fornece uma abordagem para separar usuários em fases de migração e, em seguida, gerenciar essas fases. Independentemente do método de separação de fase de usuário que você escolher, mantenha um único grupo de usuários VPN para facilitar o gerenciamento quando a migração for concluída.

>[!NOTE]
>A palavra _fase_ não se destina a indicar que esse é um processo longo. Se você passar por cada fase em alguns dias ou alguns meses, a Microsoft recomenda que você aproveite a migração lado a lado e use uma abordagem em fases.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Benefícios de dividir o esforço de migração em várias fases

-   **Proteção de interrupção em massa.** Dividindo uma migração em fases, o número de pessoas que um problema gerado pela migração pode afetar é muito menor.

-   **Melhoria no processo ou comunicação de comentários.** O ideal é que os usuários nem mesmo percebam que a migração ocorreu. No entanto, se sua experiência fosse menor do que o ideal, os comentários desses usos oferecerão a oportunidade de melhorar o planejamento e evitar problemas no futuro.

### <a name="tips-for-building-your-migration-ring"></a>Dicas para criar seu anel de migração

-   **Identificar usuários remotos.** Comece separando os usuários em dois buckets: aqueles que freqüentemente entram no escritório e aqueles que não o fazem. O processo de migração é o mesmo para ambos os grupos, mas provavelmente levará mais tempo para que os clientes remotos recebam a atualização do que para aqueles que se conectam com mais frequência. Cada fase de migração, idealmente, deve incluir membros de cada Bucket.

-  **Priorize os usuários.** A liderança e outros usuários de alto impacto geralmente estão entre os últimos usuários migrados. No entanto, ao priorizar os usuários, considere seu impacto na produtividade comercial se a migração do computador cliente falhar. Por exemplo, se você tivesse uma classificação de 1 a 3, com 1 significando que o funcionário não seria capaz de trabalhar e 3 significando que não há nenhuma interrupção de trabalho imediata, um analista de negócios que usa apenas aplicativos internos de linha de negócios (LOB) seria um 1, enquanto um vendedor usando um aplicativo de nuvem seria um 3.

-   **Migre cada departamento ou unidade de negócios em várias fases.** A Microsoft recomenda enfaticamente que você não migre um departamento inteiro ao mesmo tempo. Se ocorrer um problema, você não deseja que ele atrapalhe o trabalho remoto de todo o departamento. Em vez disso, migre cada departamento ou unidade de negócios em pelo menos duas fases.

-   **Aumentar gradualmente as contagens de usuários.** Os cenários de migração mais comuns começam com membros da organização de ti e, em seguida, mudam para os usuários empresariais seguidos por liderança e outros usuários de alto impacto. Cada fase de migração geralmente envolve progressivamente mais pessoas. Por exemplo, a primeira fase pode incluir dez usuários e o grupo final pode incluir 5.000 usuários. Para simplificar a implantação, crie um grupo de segurança de usuários de VPN único e adicione usuários a ele à medida que sua fase chega. Dessa forma, você acaba com um único grupo de usuários VPN ao qual você pode adicionar membros no futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Próxima etapa

|Se desejar...  |Em seguida, consulte...  |
|---------|---------|
|Iniciar a migração para a VPN Always On     |[Migre para Always on VPN e descomissionar o DirectAccess](da-always-on-migration-deploy.md). Migrar do DirectAccess para Always On VPN requer um processo específico para migrar clientes, o que ajuda a minimizar as condições de corrida que surgem da execução de etapas de migração fora de ordem.         |
|Saiba mais sobre os recursos de Always On VPN e DirectAccess    |[Comparação de recursos de Always on VPN e DirectAccess](../vpn/vpn-map-da.md). Nas versões anteriores da arquitetura de VPN do Windows, as limitações de plataforma tornaram difícil fornecer a funcionalidade crítica necessária para substituir o DirectAccess (como conexões automáticas iniciadas antes que os usuários entrem). O Always On VPN, no entanto, reduziu a maioria dessas limitações ou expandiu a funcionalidade de VPN além dos recursos do DirectAccess.         |



---