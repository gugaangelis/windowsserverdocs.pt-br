---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Criando um projeto de ponte de Link de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>Criando um projeto de ponte de Link de Site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma ponte de link de site conecta dois ou mais site links e permite transitividade entre os links do site. Cada link de site em uma ponte deve ter um site em comum com outro link site na ponte. O verificador de consistência de Conhecimento (KCC) usa as informações em cada link de site para calcular o custo da replicação entre sites em um link de site e nos outros links de site da ponte. Sem a presença de um site comum entre os links do site, o KCC também não é possível estabelecer conexões diretas entre controladores de domínio nos sites que estão conectados, a ponte do link site mesmo.  
  
Por padrão, todos os links de site são transitivos. Recomendamos que você mantenha transitividade ativada mudando o valor padrão de não **ponte todos os links de sites** (ativado por padrão). No entanto, você precisa desabilitar **ponte todos os links de sites** e concluir um design de ponte de link de site se:  
  
-   Sua rede IP não é totalmente roteado. Quando você desativa **ponte todos os links de sites**, todos os links são considerados intransitivas e você pode criar e configurar os objetos de ponte de link de site para modelar o comportamento de roteamento real da rede.  
  
-   Você precisa controlar o fluxo de replicação das alterações feitas nos serviços de domínio do Active Directory (AD DS). Desabilitando **ponte todos os links de sites** para o transporte IP do link de site e configurar uma ponte de link de site, a ponte do link site se torna o equivalente a uma rede separado. Todos os links de site dentro da ponte de link de site pode encaminhar temporariamente, mas eles não roteiam fora da ponte de conexão.  
  
Para obter mais informações sobre como usar o snap-in Serviços e Sites do Active Directory para desabilitar o **ponte todos os links de sites** configuração, consulte Habilitar ou desabilitar pontes ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073)).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controlando o fluxo de replicação do AD DS  
Dois cenários em que você precisa de um design de ponte de link de site para controlar o fluxo de replicação incluem controlando a replicação failover e controlando a replicação por meio de um firewall.  
  
### <a name="controlling-replication-failover"></a>Controlando o failover de replicação  
Se sua organização tiver uma topologia de rede de hub e spoke, geralmente não quiser os sites de satélite para criar conexões de replicação para outros sites de satélite se todos os controladores de domínio no site do hub falharem. Nesses cenários, você deverá desabilitar **ponte todos os links de sites** e criar pontes para que as conexões de replicação são criadas entre o site de satélite e outro site de hub é apenas um ou dois saltos para fora do site de satélite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controlando a replicação por meio de um firewall  
Se dois controladores de domínio que representam o mesmo domínio em dois locais diferentes são permitidos especificamente para se comunicar uns com os outros apenas por meio de um firewall, você pode desabilitar **ponte todos os links de sites** e criar pontes para sites no mesmo lado do firewall. Portanto, se sua rede separada por firewalls, recomendamos que você desative transitividade dos links de site e cria pontes para a rede em um lado do firewall. Para obter informações sobre o gerenciamento de replicação através de firewalls, consulte Active Directory em redes segmentadas por Firewalls ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074)).  
  


