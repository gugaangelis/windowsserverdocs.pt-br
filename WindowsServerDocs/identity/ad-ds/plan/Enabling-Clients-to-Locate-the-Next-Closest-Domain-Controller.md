---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "Permitindo que os clientes localizem o controlador de domínio mais próximo próximo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitindo que os clientes localizem o controlador de domínio mais próximo próximo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você tiver um controlador de domínio que executa o Windows Server 2008 ou Windows Server 2008 R2, você pode possibilitar para computadores cliente que executam o Windows Vista, Windows 7, Windows Server 2008 ou Windows Server 2008 R2 para localizar controladores de domínio com mais eficiência, permitindo a **tente próximo Site** configuração de política de grupo. Essa configuração melhora o localizador de controlador de domínio (localizador de DC), ajudando a simplificar o tráfego de rede, especialmente em grandes empresas que possuem muitos filiais e sites.  
  
Essa nova configuração pode afetar como você configurar os custos de link de site porque ele afeta a ordem em que os controladores de domínio estão localizados. Para empresas que possuem muitos filiais e sites de hub, você pode reduzir significativamente o tráfego do Active Directory na rede, garantindo que os clientes de failover para o próximo site hub quando eles não consegue encontrar um controlador de domínio no site do hub mais próximo.  
  
Como prática recomendada geral, você deve simplificar a topologia de sites e link site custa tanto quanto possível, se você habilitar o **tente próximo Site** configuração. Em empresas com muitos sites de hub, isso pode simplificar todos os planos que você faz para lidar com situações em que os clientes em um site precisam alternar para um controlador de domínio em outro site.  
  
Por padrão, o **tente próximo Site** configuração não esteja habilitada. Quando a configuração não está habilitada, o localizador de DC usa o seguinte algoritmo para localizar um controlador de domínio:  
  
-   Tente encontrar um controlador de domínio no mesmo site.  
  
-   Se nenhum controlador de domínio estiver disponível no mesmo site, tente encontrar qualquer controlador de domínio no domínio.  
  
> [!NOTE]  
> Isso é o mesmo algoritmo localizador de DC usada em versões anteriores do Active Directory. Para obter mais informações, consulte como dar suporte a DNS para Active Directory funciona ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587)).  
  
Se você habilitar o **tente próximo Site** configuração, o localizador de DC usa o seguinte algoritmo para localizar um controlador de domínio:  
  
-   Tente encontrar um controlador de domínio no mesmo site.  
  
-   Se nenhum controlador de domínio estiver disponível no mesmo site, tente encontrar um controlador de domínio no próximo site. Um site é melhor se ele tiver um site-link menor custo de outro site com um site-link mais alto custo.  
  
-   Se nenhum controlador de domínio estiver disponível no próximo site, tente encontrar qualquer controlador de domínio no domínio.  
  
Por padrão, o localizador de DC não considera qualquer site que contém um controlador de domínio somente leitura (RODC) ao determinar o site mais próximo. Além disso, pois somente Windows Server 2008 e Windows Server 2008 R2 domínio controladores suporte a seguinte funcionalidade site mais próximo, quando o cliente recebe uma resposta de um controlador de domínio que executa uma versão anterior do Windows Server, o comportamento de localizador de DC é o mesmo quando Configurando não está habilitado.  
  
Por exemplo, suponha que uma topologia de site tem quatro sites com os valores do link do site na ilustração a seguir. Neste exemplo, todos os controladores de domínio são controladores de domínio gravável que executam o Windows Server 2008 ou Windows Server 2008 R2.  
  
![Permitindo que os clientes localizem dc](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
Quando o **tente próximo Site** configuração de política de grupo está habilitada neste exemplo, se um computador cliente que executa o Windows Vista, Windows 7, Windows Server 2008, ou Windows Server 2008 R2 em Site_B tenta localizar um controlador de domínio, ele primeiro tenta encontrar um controlador de domínio em seu próprio Site_B. Se nenhuma estiver disponível em Site_B, ele tenta encontrar um controlador de domínio em Site_A.  
  
Se a configuração não esteja habilitada, o cliente tenta encontrar um controlador de domínio no Site_A, Site_C ou Site_D se nenhum controlador de domínio estiver disponível no Site_B.  
  
> [!NOTE]  
> O **tente próximo Site** configuração funcione em coordenação com cobertura de site automática. Por exemplo, se o site mais próximo não tiver nenhum controlador de domínio, o localizador de DC tenta encontrar o controlador de domínio que executa a cobertura de site automática para esse site.  
  
Para aplicar o **tente próximo Site** configuração, você pode criar um objeto de política de grupo (GPO) e vinculá-la ao objeto apropriado para sua organização, ou você pode modificar a política de domínio padrão para que ele afeta todos os clientes que executam o Windows Vista, Windows 7, Windows Server 2008 ou Windows Server 2008 R2 no domínio. Para obter mais informações sobre como definir o **tente próximo Site** configuração, consulte [permitir que os clientes para localizar um controlador de domínio no próximo Site](https://technet.microsoft.com/library/cc772592.aspx).  
  


