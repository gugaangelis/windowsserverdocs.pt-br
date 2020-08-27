---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funções do site
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 20ca1c9e3a4b0ef750d787289bf8563ead5a5ae1
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938356"
---
# <a name="site-functions"></a>Funções do site

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 O Windows Server 2008 usa informações do site para muitas finalidades, incluindo replicação de roteamento, afinidade de cliente, replicação de volume do sistema (SYSVOL), namespaces de Sistema de Arquivos Distribuído (DFSN) e local do serviço.

## <a name="routing-replication"></a>Replicação de roteamento
O Active Directory Domain Services (AD DS) usa um método de replicação de vários mestres, de armazenamento e encaminhamento. Um controlador de domínio comunica as alterações de diretório a um segundo controlador de domínio, que, em seguida, se comunica com um terceiro, e assim por diante, até que todos os controladores de domínio tenham recebido a alteração. Para obter o melhor equilíbrio entre reduzir a latência de replicação e reduzir o tráfego, os controles de topologia de site Active Directory a replicação, diferenciando entre a replicação que ocorre dentro de um site e a replicação que ocorre entre os sites.

Em sites, a replicação é otimizada para velocidade, as atualizações de dados disparam a replicação e os dados são enviados sem a sobrecarga exigida pela compactação de dados. Por outro lado, a replicação entre sites é compactada para minimizar o custo de transmissão em links de WAN (rede de longa distância). Quando ocorre a replicação entre sites, um único controlador de domínio por domínio em cada site coleta e armazena as alterações de diretório e as comunica em um horário agendado para um controlador de domínio em outro site.

## <a name="client-affinity"></a>Afinidade de cliente
Os controladores de domínio usam informações do site para informar Active Directory clientes sobre controladores de domínio presentes no site mais próximo que o cliente. Por exemplo, considere um cliente no site de Seattle que não conhece sua afiliação de site e entre em contato com um controlador de domínio do site de Atlanta. Com base no endereço IP do cliente, o controlador de domínio em Atlanta determina em qual site o cliente está realmente e envia as informações do site de volta para o cliente. O controlador de domínio também informa ao cliente se o controlador de domínio escolhido é o mais próximo deles. O cliente armazena em cache as informações do site fornecidas pelo controlador de domínio em Atlanta, consulta o registro de recurso SRV (serviço específico do site) (um registro de recurso do sistema de nomes de domínio (DNS) usado para localizar controladores de domínio para AD DS) e, portanto, localiza um controlador de domínio no mesmo site.

Ao localizar um controlador de domínio no mesmo site, o cliente evita as comunicações sobre links WAN. Se nenhum controlador de domínio estiver localizado no site do cliente, um controlador de domínio que tenha as conexões de custo mais baixo em relação a outros sites conectados se anunciará (registra um registro de recurso SRV (serviço específico do site) no DNS) no site que não tem um controlador de domínio. Os controladores de domínio que são publicados no DNS são aqueles do site mais próximo, conforme definido pela topologia do site. Esse processo garante que cada site tenha um controlador de domínio preferencial para autenticação.

Para obter mais informações sobre o processo de localização de um controlador de domínio, consulte [Active Directory coleção](/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)).

## <a name="sysvol-replication"></a>Replicação do SYSVOL
O SYSVOL é uma coleção de pastas no sistema de arquivos que existe em cada controlador de domínio em um domínio. As pastas SYSVOL fornecem um local de Active Directory padrão para arquivos que devem ser replicados em todo um domínio, incluindo objetos de Política de Grupo (GPOs), scripts de inicialização e desligamento e scripts de logon e logoff.  O Windows Server 2008 pode usar o FRS (serviço de replicação de arquivos) ou o DFSR (replicação de Sistema de Arquivos Distribuído) para replicar as alterações feitas nas pastas SYSVOL de um controlador de domínio para outros controladores de domínio. O FRS e o DFSR replicam essas alterações de acordo com o agendamento que você cria durante o design da topologia do site.

## <a name="dfsn"></a>DFSN
O DFSN usa informações do site para direcionar um cliente ao servidor que está hospedando os dados solicitados no site. Se DFSN não encontrar uma cópia dos dados no mesmo site que o cliente, o DFSN usará as informações do site em AD DS para determinar qual servidor de arquivos que tem dados compartilhados de DFSN está mais próximo do cliente.

## <a name="service-location"></a>Local do serviço
Ao publicar serviços como serviços de arquivo e impressão no AD DS, você permite que Active Directory clientes localizem o serviço solicitado dentro do mesmo site ou mais próximo. Os serviços de impressão usam o atributo local armazenado em AD DS para permitir que os usuários procurem impressoras por local sem saber seu local preciso. Para obter mais informações sobre como criar e implantar servidores de impressão, consulte [projetando e implantando servidores de impressão](/previous-versions/windows/it-pro/windows-server-2003/cc785842(v=ws.10)).
