---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funções do site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815807"
---
# <a name="site-functions"></a>Funções do site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 usa informações do site para muitos propósitos, incluindo replicação de roteamento, afinidade de cliente, replicação de SYSVOL (volume) do sistema, distribuídos arquivo sistema DFSN (Namespaces) e local do serviço.  
  
## <a name="routing-replication"></a>Replicação de roteamento  
Os serviços de domínio do Active Directory (AD DS) usa um método de replicação multimestre e armazenar e encaminhar. Um controlador de domínio se comunica as alterações de diretório para um segundo controlador de domínio, que se comunica com um terceiro e assim por diante, até que todos os controladores de domínio recebeu a alteração. Para obter o melhor equilíbrio entre a redução da latência de replicação e reduzindo o tráfego, a topologia de site controla a replicação do Active Directory diferenciando a replicação ocorre dentro de um site e a replicação ocorre entre sites.  
  
Dentro dos sites, a replicação é otimizada para velocidade, replicação de gatilho de atualizações de dados e os dados são enviados sem a sobrecarga exigida pela compactação de dados. Por outro lado, a replicação entre sites é compactada para minimizar o custo de transmissão sobre links de rede de (longa distância WAN). Quando ocorrer a replicação entre sites, um único controlador de domínio por domínio em cada site coleta e armazena as alterações de diretório e comunica-se em um horário agendado para um controlador de domínio em outro site.  
  
## <a name="client-affinity"></a>Afinidade de cliente  
Controladores de domínio usam as informações do site para informar os clientes do Active Directory sobre controladores de domínio presentes no site mais próximo do que o cliente. Por exemplo, considere um cliente no site de Seattle que não conhece a afiliação do seu site e entra em contato com um controlador de domínio do site de Atlanta. Com base no endereço IP do cliente, o controlador de domínio em Atlanta determina qual site o cliente é realmente da e envia as informações do site para o cliente. O controlador de domínio também informa o cliente se o controlador de domínio escolhido é o mais próximo a ele. O cliente armazena em cache as informações do site fornecidas pelo controlador de domínio em Atlanta, consultas para o registro de recurso específico do site de serviço (SRV) (sistema de nome de domínio (DNS) registro de recurso usado para localizar os controladores de domínio do AD DS) e, portanto, encontra um domínio controlador de dentro do mesmo site.  
  
Ao localizar um controlador de domínio no mesmo site, o cliente evita a comunicação por links WAN. Se nenhum controlador de domínio estejam localizados no site do cliente, um controlador de domínio que tenha as conexões de custo mais baixas em relação a outros sites conectados anuncia em si (registra um registro de recurso específico do site de serviço (SRV) no DNS) no site que não tenha um controlador de domínio. Os controladores de domínio que são publicados no DNS são aqueles do site mais próximo, conforme definido pela topologia de site. Esse processo garante que cada site tem um controlador de domínio preferencial para autenticação.  
  
Para obter mais informações sobre o processo de localização de um controlador de domínio, consulte a coleção do Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replicação do SYSVOL  
SYSVOL é uma coleção de pastas no sistema de arquivos que existe em cada controlador de domínio em um domínio. As pastas SYSVOL fornecem um local do Active Directory padrão para arquivos que devem ser replicados em todo um domínio, inclusive objetos de diretiva de grupo (GPOs), scripts de inicialização e desligamento e scripts de logon e logoff.  Windows Server 2008 pode usar a replicação FRS (serviço) ou arquivo sistema DFSR (replicação distribuído) para replicar as alterações feitas nas pastas SYSVOL de um controlador de domínio para outros controladores de domínio. FRS e DFSR replicam essas alterações de acordo com o agendamento que você cria durante o design da topologia de site.  
  
## <a name="dfsn"></a>DFSN  
DFSN usa informações do site para direcionar um cliente para o servidor que hospeda os dados solicitados dentro do site. Se DFSN não encontrar uma cópia dos dados dentro do mesmo site do cliente, DFSN usa as informações do site no AD DS para determinar qual servidor de arquivo que tenha DFSN de dados compartilhadas são mais próximas ao cliente.  
  
## <a name="service-location"></a>Local do serviço  
Publicando serviços como o arquivo e os serviços de impressão no AD DS, você pode permitir que os clientes do Active Directory localizar o serviço solicitado na mesma ou site mais próximo. Serviços de impressão usam o atributo de localização armazenado no AD DS para permitir que os usuários procurem impressoras pelo local sem saber seu local exato. Para obter mais informações sobre como projetar e implantar servidores de impressão, consulte Projetando e implantando servidores de impressão ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


