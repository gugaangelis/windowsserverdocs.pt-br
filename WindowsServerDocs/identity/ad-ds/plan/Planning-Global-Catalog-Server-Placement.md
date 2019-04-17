---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: "Planejar o posicionamento do servidor de Catálogo Global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>Planejar o posicionamento do servidor de Catálogo Global

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Posicionamento do catálogo global requer planejamento exceto se você tiver uma floresta de domínio único. Em uma floresta de domínio único, configure todos os controladores de domínio como servidores de catálogo global. Como cada controlador de domínio armazena a partição de diretório único domínio na floresta, configurar cada controlador de domínio como um servidor de catálogo global não exige qualquer uso de espaço em disco adicional, o uso da CPU ou o tráfego de replicação. Em uma floresta de domínio único, todos os controladores de domínio atuarem como servidores de catálogo global virtual; ou seja, ele pode todos responder a qualquer autenticação ou a solicitação de serviço. Essa condição especial para o domínio único florestas ocorre por design. Solicitações de autenticação não exigem contatar um servidor de catálogo global como fazem quando há vários domínios, e um usuário pode ser um membro de um grupo universal que existe em um domínio diferente. No entanto, somente os controladores de domínio que são designados como servidores de catálogo global podem responder às consultas do catálogo global na porta 3268 do catálogo global. Para simplificar a administração neste cenário e assegurar respostas consistentes, a designação de todos os controladores de domínio como servidores de catálogo global elimina a preocupação sobre qual domínio controladores podem responder às consultas do catálogo global. Especificamente, sempre que um usuário usa Start\Search\For pessoas ou localizar impressoras ou expande grupos universais, essas solicitações acessem apenas no catálogo global.  
  
Nas florestas de vários domínios, servidores de catálogo global facilitam solicitações de logon do usuário e pesquisas de toda a floresta. A ilustração a seguir mostra como determinar quais locais exigem servidores de catálogo global.  
  
![Planejar o posicionamento de gc](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Na maioria dos casos, é recomendável que você inclua o catálogo global quando você instala novos controladores de domínio. Aplicam as seguintes exceções:  
  
-   Largura de banda limitada: em locais remotos, se o link WAN (rede) de longa distância entre no site remoto e o site de hub é limitado, você pode usar o cache de membros de grupos universais no site remoto para acomodar as necessidades de logon dos usuários no site.  
  
-   Incompatibilidade de função de mestre de operações de infraestrutura: não coloque o catálogo global em um controlador de domínio que hospeda a função mestre de operações de infraestrutura no domínio, a menos que todos os controladores de domínio no domínio são servidores de catálogo global ou floresta tem apenas um domínio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Adicionando servidores de catálogo global com base nos requisitos de aplicativo  
Certos aplicativos, como Microsoft Exchange, enfileiramento de mensagens (também conhecido como MSMQ) e aplicativos que usam o DCOM não fornecer resposta adequado por links WAN latentes e, portanto, precisa de uma infraestrutura de catálogo global altamente disponíveis para fornecer latência baixa consulta. Determine se todos os aplicativos que desempenho insatisfatório em um link WAN lento estão executando em locais ou se os locais exigem o Microsoft Exchange Server. Se seus locais incluem aplicativos que não oferecem a resposta adequado em um link WAN, você deve colocar um servidor de catálogo global no local para reduzir a latência de consulta.  
  
> [!NOTE]  
> Read-only controladores de domínio (RODCs) podem ser promovidos com êxito para o status do servidor catálogo global. No entanto, certos aplicativos habilitados no diretório não oferece suporte um RODC como um servidor de catálogo global. Por exemplo, nenhuma versão do Microsoft Exchange Server usa RODCs. No entanto, Microsoft Exchange Server funciona em ambientes que incluem RODCs, como controladores de domínio gravável estão disponíveis. Exchange Server 2007 efetivamente ignora RODCs. Exchange Server 2003 também ignora RODCs em condições padrão onde a componentes do Exchange detectam automaticamente controladores de domínio disponíveis. Nenhuma alteração foi feita com o Exchange Server 2003 conscientizá dos servidores de diretório somente leitura. Portanto, tentando forçar os serviços do Exchange Server 2003 e ferramentas de gerenciamento para usar RODCs pode resultar em comportamento inesperado.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Adicionando servidores de catálogo global para um grande número de usuários  
Coloque os servidores de catálogo global em todos os locais que contêm mais de 100 usuários para reduzir o congestionamento WAN de conexões de rede e a evitar a perda de produtividade em caso de falha do link WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Usando a largura de banda altamente disponível  
Você não precisa colocar um catálogo global em um local que não inclui aplicativos que exigem um servidor de catálogo global inclui menos de 100 usuários e também é conectado a outro local que inclui um servidor de catálogo global por um link WAN que é 100% disponíveis para os serviços de domínio do Active Directory (AD DS). Nesse caso, os usuários podem acessar o servidor de catálogo global através do link WAN.  
  
Os usuários móveis precisam entrar em contato com os servidores de catálogo global sempre que eles fizerem logon pela primeira vez em qualquer local. Se o tempo de logon através do link WAN é inaceitável, coloque um catálogo global em um local que é visitado por um grande número de usuários móveis.  
  
## <a name="enabling-universal-group-membership-caching"></a>Habilitar o cache de membros de grupos universais  
Para locais que incluem menos de 100 usuários e que não incluem um grande número de usuários ou aplicativos que exigem um servidor de catálogo global roaming, você pode implantar controladores de domínio que estão executando o Windows Server 2008 e habilitar o cache de membros de grupos universais. Certifique-se de que os servidores de catálogo global não são mais de um salto de replicação do controlador de domínio no qual o cache de membros de grupos universais está habilitado para que as informações de grupos universais em cache podem ser atualizadas. Para obter informações sobre como funciona o cache grupo como universal, veja como o funciona Catálogo Global ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063)).  
  
Para uma planilha para ajudá-lo em documentar onde você planeja colocar controladores de domínio e servidores de catálogo global com cache grupos universais habilitado, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe < DICT__Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open Domain Controlador posicionamento (DSSTOPO_4.doc). Consulte as informações sobre localizações no qual você precisa colocar servidores de catálogo global quando você implanta o domínio raiz da floresta e domínios regionais.  
  


