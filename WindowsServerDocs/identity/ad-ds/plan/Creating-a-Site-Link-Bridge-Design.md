---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Criar um design de link de site
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813987"
---
# <a name="creating-a-site-link-bridge-design"></a>Criar um design de link de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma ponte de link de site conecta-se dois ou mais sites vincula e permite que a transitividade da relação entre links de site. Cada link de site em uma ponte deve ter um site em comum com outro link de site na ponte. O Knowledge Consistency Checker (KCC) usa as informações em cada link de site para calcular o custo da replicação entre sites em um link de site e de outros links de site da ponte. Sem a presença de um site comum entre links de site, o KCC também não pode estabelecer conexões diretas entre os controladores de domínio nos sites que estão conectados pela mesma ponte de link de site.  
  
Por padrão, todos os links de site são transitivos. É recomendável que você mantenha a transitividade habilitada ao não alterar o valor padrão de **ponte para todos os links de site** (habilitada por padrão). No entanto, você precisará desabilitar **ponte para todos os links de site** e concluir um design de ponte de link de site se:  

- A rede IP não é totalmente roteada. Quando você desabilita **ponte para todos os links de site**, todos os links de site são considerados intransitivas, e você pode criar e configurar objetos de ponte de link de site para modelar o comportamento de roteamento real da sua rede.  
- Você precisa controlar o fluxo de replicação das alterações feitas nos serviços de domínio Active Directory (AD DS). Desabilitando **ponte para todos os links de site** para o transporte IP de link de site e configurar uma ponte de link de site, a ponte de link de site se torna o equivalente a uma rede não contíguo. Todos os links de site em que a ponte de link de site pode rotear transitivamente, mas eles não encaminham fora a ponte de link de site.  

Para obter mais informações sobre como usar o snap-in Serviços e Sites do Active Directory para desabilitar o **ponte para todos os links de site** , consulte o artigo [habilitar ou desabilitar pontes de link de site](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controlando o fluxo de replicação do AD DS

Dois cenários em que você precisa de um design de ponte de link de site para controlar o fluxo de replicação incluem controlar o failover de replicação e controlar replicação por meio de um firewall.  
  
### <a name="controlling-replication-failover"></a>Controlando o failover de replicação

Se sua organização tiver uma topologia de rede de hub e spoke, você geralmente não querem os sites de satélite para criar conexões de replicação para outros sites de satélite, se todos os controladores de domínio no site do hub falham. Nesses cenários, você deve desabilitar **ponte para todos os links de site** e criar pontes de link de site para que as conexões de replicação são criadas entre o site de satélite e outro site de hub que é apenas um ou dois saltos fora do site de satélite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controlando a replicação por meio de um firewall

Se dois controladores de domínio que representam o mesmo domínio em dois locais distintos especificamente têm permissão para se comunicar entre si por meio de um firewall, você pode desabilitar **ponte para todos os links de site** e criar pontes de links de site sites no mesmo lado do firewall. Portanto, se sua rede for separado por firewalls, é recomendável que você desabilite a transitividade da relação de links de site e cria pontes de link de site para a rede em um dos lados do firewall. Para obter informações sobre como gerenciar a replicação por meio de firewalls, consulte o artigo [do Active Directory em redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=107074).
