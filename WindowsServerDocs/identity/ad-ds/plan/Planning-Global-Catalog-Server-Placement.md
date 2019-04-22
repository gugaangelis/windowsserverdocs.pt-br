---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planejando posicionamento do servidor de catálogo global
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820677"
---
# <a name="planning-global-catalog-server-placement"></a>Planejando posicionamento do servidor de catálogo global

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Posicionamento de catálogo global requer um planejamento, exceto se você tiver uma floresta de domínio único. Em uma floresta de domínio único, configure todos os controladores de domínio como servidores de catálogo global. Como cada controlador de domínio armazena a partição de diretório único domínio na floresta, configuração de cada controlador de domínio como um servidor de catálogo global não exige qualquer uso do espaço em disco adicional, o uso da CPU ou o tráfego de replicação. Em uma floresta de domínio único, todos os controladores de domínio atuam como servidores de catálogo global virtual; ou seja, eles podem todos responder a qualquer autenticação ou a solicitação de serviço. Essa condição especial para florestas de domínio único ocorre por design. Solicitações de autenticação não exigem entrando em contato com o servidor de catálogo global quando há vários domínios e um usuário pode ser um membro de um grupo universal que existe em um domínio diferente. No entanto, somente os controladores de domínio que são designados como servidores de catálogo global podem responder a consultas de catálogo global a porta 3268 do catálogo global. Para simplificar a administração neste cenário e para garantir respostas consistentes, que designa a todos os controladores de domínio como servidores de catálogo global elimina a preocupação sobre qual domínio os controladores podem responder a consultas de catálogo global. Especificamente, sempre que um usuário usa Start\Search\For pessoas ou localizar impressoras ou expande grupos universais, essas solicitações vão apenas para o catálogo global.  
  
Em florestas de vários domínios, servidores de catálogo global facilitam as solicitações de logon de usuário e pesquisas em toda a floresta. A ilustração a seguir mostra como determinar quais locais exigem servidores de catálogo global.  
  
![Planejando o posicionamento de gc](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Na maioria dos casos, é recomendável que você inclua o catálogo global quando você instala novos controladores de domínio. As seguintes exceções se aplicam:  
  
- Largura de banda limitada: Em sites remotos, se o link de rede de (longa distância WAN) de longa distância entre o site remoto e o site de hub é limitado, você pode usar o cache de associação de grupo universal no site remoto para acomodar as necessidades de logon de usuários no site.  
- Incompatibilidade de função de mestre de operações de infraestrutura: Não coloque o catálogo global em um controlador de domínio que hospeda as operações de infra-estrutura função de mestre no domínio, a menos que todos os controladores de domínio no domínio são servidores de catálogo global ou a floresta possui apenas um domínio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Adição de servidores de catálogo global com base nos requisitos de aplicativo

Determinados aplicativos, como Microsoft Exchange, enfileiramento de mensagens (também conhecido como MSMQ) e aplicativos que usam DCOM não fornecer resposta adequado por links WAN latentes e, portanto, precisa de uma infraestrutura de catálogo global altamente disponível para fornecer a consulta baixa latência. Determine se todos os aplicativos que um desempenho insatisfatório em um link WAN lento estão em execução em locais ou se os locais exigem o Microsoft Exchange Server. Se seus locais incluem aplicativos que não fornecem a resposta adequada em um link WAN, você deve colocar um servidor de catálogo global no local para reduzir a latência da consulta.  
  
> [!NOTE]  
> Controladores de domínio somente leitura (RODCs) podem ser promovidos com êxito para o status do servidor de catálogo global. No entanto, certos aplicativos habilitados por diretório não podem dar suporte a um RODC como um servidor de catálogo global. Por exemplo, nenhuma versão do Microsoft Exchange Server usa os RODCs. No entanto, Microsoft Exchange Server funciona em ambientes que incluem os RODCs, desde que há controladores de domínio graváveis disponíveis. O Exchange Server 2007 efetivamente ignora os RODCs. Exchange Server 2003 também ignora os RODCs em condições padrão em que componentes do Exchange detectam automaticamente controladores de domínio disponíveis. Nenhuma alteração foi feita para o Exchange Server 2003 para torná-lo informado sobre os servidores de diretório somente leitura. Portanto, tentando forçar os serviços do Exchange Server 2003 e ferramentas de gerenciamento para usar os RODCs pode resultar em comportamento imprevisível.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Adição de servidores de catálogo global para um grande número de usuários

Coloque os servidores de catálogo global em todos os locais que contêm mais de 100 usuários para reduzir o congestionamento de rede de LONGA distância e para evitar a perda de produtividade no caso de falha de link WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Usando a largura de banda altamente disponível

Você não precisa colocar um catálogo global em um local que não inclui aplicativos que exigem um servidor de catálogo global, inclui menos de 100 usuários e também é conectado para outro local que inclui um servidor de catálogo global por um link WAN que seja 100 por cento um disponíveis para os serviços de domínio do Active Directory (AD DS). Nesse caso, os usuários podem acessar o servidor de catálogo global pelo link WAN.  
  
Os usuários móveis precisam entrar em contato com os servidores de catálogo global sempre que efetuarem logon pela primeira vez em qualquer local. Se o horário de logon pelo link WAN for inaceitável, coloque um catálogo global em um local que é visitado por um grande número de usuários móveis.  
  
## <a name="enabling-universal-group-membership-caching"></a>Habilitação do cache de associação de grupo universal

Para locais que incluem menos de 100 usuários e que não incluem um grande número de usuários de roaming ou aplicativos que exigem um servidor de catálogo global, você pode implantar controladores de domínio que executam o Windows Server 2008 e habilitar a associação de grupo universal armazenamento em cache. Certifique-se de que os servidores de catálogo global não estão mais de um salto de replicação do controlador de domínio no qual o cache de associação de grupo universal está habilitado para que as informações de grupo universal no cache podem ser atualizadas. Para obter informações sobre como funciona o cache universal como grupo, consulte o artigo [como o funciona Catálogo Global](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Para uma planilha ajudar a documentar a onde você planeja colocar servidores de catálogo global e controladores de domínio com o cache de grupo universal habilitado, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_ Designing_and_Deploying_Directory_and_Security_Services.zip e abra o posicionamento de controlador de domínio (DSSTOPO_4.doc). Consulte as informações sobre os locais em que você precisa colocar servidores de catálogo global quando você implanta o domínio raiz da floresta e domínios regionais.  
