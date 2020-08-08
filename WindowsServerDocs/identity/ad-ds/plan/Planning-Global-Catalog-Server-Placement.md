---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planejando posicionamento do servidor de catálogo global
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a8ef90ec13b67fdb3bc0e37e02d571721a0ea77a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970973"
---
# <a name="planning-global-catalog-server-placement"></a>Planejando posicionamento do servidor de catálogo global

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O posicionamento do catálogo global requer planejamento, exceto se você tiver uma floresta de domínio único. Em uma floresta de domínio único, configure todos os controladores de domínio como servidores de catálogo global. Como cada controlador de domínio armazena a única partição de diretório de domínio na floresta, configurar cada controlador de domínio como um servidor de catálogo global não requer uso de espaço em disco adicional, uso de CPU ou tráfego de replicação. Em uma floresta de domínio único, todos os controladores de domínio atuam como servidores de catálogo global virtual; ou seja, todos eles podem responder a qualquer autenticação ou solicitação de serviço. Essa condição especial para florestas de domínio único é por design. As solicitações de autenticação não exigem o contato de um servidor de catálogo global, pois elas são feitas quando há vários domínios, e um usuário pode ser membro de um grupo universal que existe em um domínio diferente. No entanto, somente os controladores de domínio designados como servidores de catálogo global podem responder a consultas de catálogo global na porta de catálogo global 3268. Para simplificar a administração nesse cenário e garantir respostas consistentes, a designação de todos os controladores de domínio como servidores de catálogo global elimina a preocupação sobre quais controladores de domínio podem responder às consultas de catálogo global. Especificamente, sempre que um usuário usar Start\Search\For pessoas ou encontrar impressoras ou expandir grupos universais, essas solicitações vão apenas para o catálogo global.

Em florestas de vários domínios, os servidores de catálogo global facilitam as solicitações de logon do usuário e as pesquisas em toda a floresta. A ilustração a seguir mostra como determinar quais locais exigem servidores de catálogo global.

![planejando o posicionamento do GC](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)

Na maioria dos casos, é recomendável que você inclua o catálogo global ao instalar novos controladores de domínio. As seguintes exceções se aplicam:

- Largura de banda limitada: em sites remotos, se o link WAN (rede de longa distância) entre o site remoto e o site do Hub for limitado, você poderá usar o cache de associação de grupo universal no site remoto para acomodar as necessidades de logon dos usuários no site.
- Incompatibilidade de função de mestre de operações de infraestrutura: não coloque o catálogo global em um controlador de domínio que hospede a função de mestre de operações de infraestrutura no domínio, a menos que todos os controladores de domínio no domínio sejam servidores de catálogo global ou a floresta tenha apenas um domínio.

## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Adicionando servidores de catálogo global com base nos requisitos do aplicativo

Determinados aplicativos, como o Microsoft Exchange, o enfileiramento de mensagens (também conhecido como MSMQ), e os aplicativos que usam DCOM não fornecem resposta adequada sobre links WAN latentes e, portanto, precisam de uma infraestrutura de catálogo global altamente disponível para fornecer baixa latência de consulta. Determine se os aplicativos que executam insatisfatoriamente em um link WAN lento estão em execução em locais ou se os locais exigem o Microsoft Exchange Server. Se seus locais incluírem aplicativos que não fornecem resposta adequada em um link de WAN, você deverá posicionar um servidor de catálogo global no local para reduzir a latência da consulta.

> [!NOTE]
> Os RODCs (controladores de domínio somente leitura) podem ser promovidos com êxito para o status do servidor de catálogo global. No entanto, determinados aplicativos habilitados para diretório não podem dar suporte a um RODC como um servidor de catálogo global. Por exemplo, nenhuma versão do Microsoft Exchange Server usa RODCs. No entanto, o Microsoft Exchange Server funciona em ambientes que incluem RODCs, desde que haja controladores de domínio graváveis disponíveis. O Exchange Server 2007 ignora efetivamente os RODCs. O Exchange Server 2003 também ignora os RODCs nas condições padrão em que os componentes do Exchange detectam automaticamente os controladores de domínio disponíveis. Nenhuma alteração foi feita no Exchange Server 2003 para deixá-lo ciente dos servidores de diretório somente leitura. Portanto, tentar forçar os serviços do Exchange Server 2003 e as ferramentas de gerenciamento para usar os RODCs pode resultar em um comportamento imprevisível.

## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Adicionando servidores de catálogo global para um grande número de usuários

Coloque servidores de catálogo global em todos os locais que contenham mais de 100 usuários para reduzir o congestionamento de links de WAN de rede e para evitar a perda de produtividade em caso de falha de link de WAN.

## <a name="using-highly-available-bandwidth"></a>Usando largura de banda altamente disponível

Você não precisa inserir um catálogo global em um local que não inclui aplicativos que exigem um servidor de catálogo global, inclui menos de 100 usuários e também está conectado a outro local que inclui um servidor de catálogo global por um link de WAN que é 100% disponível para Active Directory Domain Services (AD DS). Nesse caso, os usuários podem acessar o servidor de catálogo global pelo link WAN.

Os usuários móveis precisam entrar em contato com os servidores de catálogo global sempre que fizerem logon pela primeira vez em qualquer local. Se o tempo de logon pelo link de WAN for inaceitável, coloque um catálogo global em um local que é visitado por um grande número de usuários móveis.

## <a name="enabling-universal-group-membership-caching"></a>Habilitando o cache de associação de grupo universal

Para locais que incluem menos de 100 usuários e que não incluem um grande número de usuários ou aplicativos móveis que exigem um servidor de catálogo global, você pode implantar controladores de domínio que executam o Windows Server 2008 e habilitar o cache de associação de grupo universal. Verifique se os servidores de catálogo global não são mais de um salto de replicação do controlador de domínio no qual o cache de associação de grupo universal está habilitado para que as informações de grupo universal no cache possam ser atualizadas. Para obter informações sobre como funciona o cache de grupo universal, consulte o artigo [como o catálogo global funciona](/previous-versions/windows/it-pro/windows-server-2003/cc737410(v=ws.10)).

Para uma planilha para ajudá-lo a documentar onde você planeja colocar os servidores de catálogo global e controladores de domínio com o cache de grupo universal habilitado, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir o posicionamento do controlador de domínio (DSSTOPO_4.doc). Consulte as informações sobre os locais nos quais você precisa posicionar os servidores de catálogo global ao implantar o domínio raiz da floresta e os domínios regionais.
