---
title: Ajuste de desempenho de servidores do Active Directory
description: Ajuste de desempenho de servidores do Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 04c9683c3d14291d5dc2682c6836657313865866
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891937"
---
# <a name="performance-tuning-active-directory-servers"></a>Ajuste de desempenho de servidores do Active Directory

O ajuste de desempenho do Active Directory concentra-se em duas metas:
- Configurar corretamente o Active Directory para atender à carga da maneira mais eficaz possível
- As cargas de trabalho enviadas para o Active Directory devem ser tão eficientes quanto possível

Isso requer atenção adequada para três áreas separadas:
- Planejamento da capacidade apropriada: garantir que há hardware adequado para dar suporte à carga existente
- Ajuste no lado do servidor: configurar os controladores de domínio para tratar a carga da forma mais eficiente possível
- Ajuste do aplicativo/cliente do Active Directory: garantir que os clientes e aplicativos estejam usando o Active Directory da maneira ideal

## <a name="start-with-capacity-planning"></a>Começar com o planejamento da capacidade
Implantar corretamente um número suficiente de controladores de domínio, no domínio correto e nas localidades corretas, para acomodar a redundância é fundamental para garantir o atendimento pontual das solicitações dos clientes. Este é um tópico detalhado e fora do escopo deste guia. Incentivamos os leitores a começar o ajuste do desempenho de seu Active Directory lendo e entendendo as recomendações e diretrizes de [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566).

>[!Important]
> A configuração e o dimensionamento apropriados do Active Directory têm um impacto significativo no desempenho geral do sistema e da carga de trabalho. Aconselhamos que os leitores comecem lendo o [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566).

## <a name="updates-and-evolving-recommendations"></a>Recomendações para atualizações e evoluções

Ocorreram melhorias massivas nas otimizações de desempenho do Active Directory e de clientes nas últimas gerações do sistema operacional e esses esforços continuam nesse sentido. É recomendável implantar as versões mais atuais da plataforma para obter benefícios que incluem:

- Maior confiabilidade
- Melhor desempenho
- Melhor registro em log e ferramentas para solucionar problemas

No entanto, sabemos que isso leva tempo e muitos ambientes estão em execução em um cenário em que é impossível a adoção de 100% da plataforma mais recente. Alguns aprimoramentos foram adicionados a versões mais antigas da plataforma e continuaremos a adicionar mais.

Incentivamos que você se mantenha atualizado sobre as notícias, orientações e práticas recomendadas mais recentes para gerenciar o ADDS pelo blog da nossa equipe, ["Pergunte à equipe do Directory Services"](https://blogs.technet.microsoft.com/askds).

## <a name="see-also"></a>Consulte também
- [Considerações sobre hardware](hardware-considerations.md)
- [Considerações sobre LDAP](ldap-considerations.md)
- [Posicionamento adequado dos controladores de domínio e considerações sobre o local](site-definition-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)
