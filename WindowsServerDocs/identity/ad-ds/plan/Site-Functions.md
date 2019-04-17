---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: "Funções de site"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>Funções de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 usa informações do site para muitos fins, incluindo replicação de roteamento, afinidade do cliente, replicação de volume (SYSVOL) do sistema, Namespaces de sistema de arquivos distribuído (DFSN) e local do serviço.  
  
## <a name="routing-replication"></a>Replicação de roteamento  
Os serviços de domínio do Active Directory (AD DS) usa um método de replicação mestre múltiplo e armazenar e encaminhar. Um controlador de domínio se comunica alterações de diretório para um segundo controlador de domínio, que se comunica com um terceiro e assim por diante, até que todos os controladores de domínio recebeu a alteração. Para obter o melhor equilíbrio entre a redução da latência da replicação e reduzindo o tráfego, a topologia do site controla a replicação do Active Directory por diferenciar replicação que ocorre dentro de um site e replicação que ocorre entre sites.  
  
Dentro dos sites, replicação é otimizada para disparar a duplicação speeddata atualizações e os dados são enviados sem a sobrecarga exigida pela compactação de dados. Por outro lado, a replicação entre sites é compactada para minimizar o custo de transmissão via conexões de rede (WAN) de longa distância. Quando ocorre a replicação entre sites, um controlador de domínio único por domínio em cada site coleta e armazena as alterações de diretório e se comunica-los em um horário agendado para um controlador de domínio em outro site.  
  
## <a name="client-affinity"></a>Afinidade do cliente  
Controladores de domínio usam informações do site para informar os clientes do Active Directory sobre controladores de domínio presentes no site mais próximo do cliente. Por exemplo, considere um cliente no site Seattle que não sabe afiliação seu site e entra em contato com um controlador de domínio do site Atlanta. Com base no endereço IP do cliente, o controlador de domínio em Atlanta determina quais sites o cliente seja realmente da e envia as informações do site para o cliente. O controlador de domínio também informa o cliente se o controlador de domínio escolhido é o mais próximo a ele. O cliente armazena em cache as informações do site fornecidas pelo controlador de domínio em Atlanta, consultas para o registro de recurso específica do site de serviços (SRV) (um registro de recurso sistema de nome de domínio (DNS) usado para localizar controladores de domínio para o AD DS) e, portanto, encontre um controlador de domínio dentro do mesmo site.  
  
Localizando um controlador de domínio no mesmo site, o cliente evita a comunicação via links WAN. Se nenhum controlador de domínio está localizados no site do cliente, um controlador de domínio que tenha as conexões de custo mais baixos em relação a outros sites conectados notifique a mesmo (registra um registro de recurso específica do site de serviços (SRV) no DNS) no site que não tenha um controlador de domínio. Os controladores de domínio que são publicados no DNS são aqueles do site mais próximo, conforme definido pela topologia de site. Esse processo garante que cada site tem um controlador de domínio preferencial para autenticação.  
  
Para obter mais informações sobre o processo de localização de um controlador de domínio, consulte coleta do Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replicação de SYSVOL  
SYSVOL é uma coleção de pastas no sistema de arquivos que existe em cada controlador de domínio em um domínio. As pastas SYSVOL fornecem um localização de Active Directory padrão para os arquivos que devem ser replicadas em toda um domínio, inclusive objetos de política de grupo (GPOs), scripts de inicialização e desligamento e os scripts de logon e logoff.  Windows Server 2008 pode usar o serviço de replicação de arquivos (FRS) ou distribuído arquivo sistema DFSR (replicação) para replicar as alterações feitas nas pastas SYSVOL de um controlador de domínio para outros controladores de domínio. FRS e DFSR replicam essas alterações de acordo com o agendamento que você cria durante o design de topologia do seu site.  
  
## <a name="dfsn"></a>DFSN  
DFSN usa informações do site para direcionar um cliente para o servidor que hospeda os dados solicitados dentro do site. Se DFSN não encontra uma cópia dos dados dentro do mesmo site como o cliente, DFSN usa as informações do site no AD DS para determinar qual servidor de arquivo que tenha DFSN dados compartilhados são mais próximos ao cliente.  
  
## <a name="service-location"></a>Local do serviço  
Publicando serviços como o arquivo e serviços de impressão no AD DS, você pode permitir que os clientes do Active Directory localizar o serviço solicitado dentro do mesmo ou site mais próximo. Serviços de impressão usam o atributo de localização armazenado no AD DS para permitir que os usuários procurem impressoras pelo local sem saber sua localização precisa. Para obter mais informações sobre como projetar e implantar servidores de impressão, consulte Projetando e implantando servidores de impressão ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


