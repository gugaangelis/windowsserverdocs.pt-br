---
title: Planejamento de migração de acesso VPN Always On remoto
description: Migrando do DirectAccess para sempre VPN requer planejamento adequado determinar suas fases de migração, que ajuda a identificar problemas antes que eles afetam toda a organização.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835667"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planejar o migração de VPN Always On do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

&#171;[ **Anterior:** Visão geral do DirectAccess para a migração de VPN Always On](da-always-on-migration-overview.md)<br>
&#187; [**Next:** Migrar para VPN Always On e encerrar o DirectAccess](da-always-on-migration-deploy.md)


Migrando do DirectAccess para sempre VPN requer planejamento adequado determinar suas fases de migração, que ajuda a identificar problemas antes que eles afetam toda a organização. É o principal objetivo da migração para os usuários a manter a conectividade remota ao escritório durante todo o processo. Se você executar tarefas fora de ordem, uma condição de corrida poderá ocorrer, deixando os usuários remotos com nenhuma maneira de acessar recursos da empresa. Portanto, a Microsoft recomenda a execução de uma migração planejada e lado a lado do DirectAccess para VPN Always On. Para obter detalhes, consulte o [implantação de migração de VPN Always On](da-always-on-migration-deploy.md) seção.

A seção descreve os benefícios da separação de usuários para a migração, considerações sobre a configuração padrão e os aprimoramentos de recursos de VPN Always On. A fase de planejamento de migração inclui:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Criar anéis de migração
Anéis de migração são usados para dividir o esforço de migração de cliente VPN Always On em várias fases. Quando que chegar à última fase, o processo deve ser bem testado e consistente.

Esta seção fornece uma abordagem para a separação de usuários em fases de migração e, em seguida, gerenciar essas fases. Independentemente do método de separação de fase do usuário escolhido, manter um único grupo de usuários de VPN para facilitar o gerenciamento quando a migração for concluída.

>[!NOTE] 
>A palavra _fase_ não se destina para indicar que este é um processo longo. Se você percorrer cada fase em alguns dias ou alguns meses, a Microsoft recomenda que você aproveite a migração lado a lado e usa uma abordagem em fases.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Benefícios de dividir o esforço de migração em várias fases

-   **Proteção de interrupção em massa.** Dividindo uma migração em fases, o número de pessoas que pode afetar a um problema de migração gerado é muito menor.

-   **Melhoria no processo ou a comunicação de comentários.** O ideal é que os usuários não mesmo observou que a migração ocorreu. No entanto, se a sua experiência foi aquém do ideal, comentários desses usos lhe dá a oportunidade de melhorar o seu planejamento e evitar problemas no futuro.

### <a name="tips-for-building-your-migration-ring"></a>Dicas para criar o anel de migração

-   **Identifique os usuários remotos.** Comece separando os usuários em dois buckets: aqueles que vêm com frequência no escritório e aquelas que não. O processo de migração é o mesmo para ambos os grupos, mas é provável que levar mais tempo para os clientes remotos receber a atualização, que, para aqueles que se conectam com mais frequência. O ideal é que cada fase da migração, deve incluir os membros de cada bucket.

-  **Priorize os usuários.** Liderança e outros usuários de alto impacto são normalmente entre os último usuários migrados. Quando a priorizar os usuários, no entanto, considere o impacto de produtividade nos negócios se a migração de seus computadores cliente falharem. Por exemplo, se você tivesse uma classificação de 1 a 3, com que 1 significa que o funcionário não seria capaz de trabalho e 3, ou seja, sem interrupção do trabalho imediato, um analista de negócios usando somente interno linha de negócios (LOB) aplicativos remotamente seria um 1, enquanto um vendedor usando uma nuvem  aplicativo seria um 3.

-   **Migre cada departamento ou unidade de negócios em várias fases.** Microsoft recomenda enfaticamente que você não migrar todo o departamento ao mesmo tempo. Se surgir um problema, você não deseja prejudicar o trabalho remoto para o departamento de inteiro. Em vez disso, migre cada departamento ou unidade de negócios em pelo menos duas fases.

-   **Aumente gradualmente a contagem de usuários.** Cenários mais comuns de migração começam com os membros do departamento de TI e, em seguida, mova para usuários de negócios, seguidos de liderança e outros usuários de alto impacto. Normalmente, cada fase da migração envolve progressivamente mais pessoas. Por exemplo, a primeira fase pode incluir dez usuários, e o grupo final pode incluir a 5.000 usuários. Para simplificar a implantação, crie um único grupo de segurança de usuários da VPN e adicionar usuários a ele como sua fase é recebido. Dessa forma, você acaba com um único grupo de usuários da VPN ao qual você pode adicionar membros no futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Próximas etapas

|Se você quiser...  |Em seguida, consulte...  |
|---------|---------|
|Iniciar a migração para VPN Always On     |[Migrar para VPN Always On e encerrar o DirectAccess](da-always-on-migration-deploy.md). Migrando do DirectAccess para sempre VPN requer um processo específico, a migração de clientes, que ajuda a minimizar as condições de corrida que podem surgir de executar as etapas de migração fora de ordem.         |
|Saiba mais sobre os recursos de VPN Always On e o DirectAccess    |[Comparação de recursos do Always On DirectAccess e VPN](../vpn/vpn-map-da.md). Nas versões anteriores da arquitetura de VPN do Windows, limitações da plataforma tornou difícil fornecer recursos essenciais necessários para substituir o DirectAccess (como conexões automáticas iniciadas antes que os usuários entrar). VPN Always On, no entanto, tiver atenuado a maioria dessas limitações ou expandida a funcionalidade VPN, além dos recursos do DirectAccess.         |



---