---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Criar um design de link de site
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f75feb34b64e2ab41859dd708147a2e8d05a768a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822759"
---
# <a name="creating-a-site-link-bridge-design"></a>Criar um design de link de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma ponte de link de site conecta dois ou mais links de site e habilita a transitividade entre links de site. Cada link de site em uma ponte deve ter um site em comum com outro link de site na ponte. O Knowledge Consistency Checker (KCC) usa as informações em cada link de site para calcular o custo de replicação entre sites em um link de site e sites nos outros links de site da ponte. Sem a presença de um site comum entre links de site, o KCC também não pode estabelecer conexões diretas entre controladores de domínio nos sites que estão conectados pela mesma ponte de link de site.  
  
Por padrão, todos os links de site são transitivos. Recomendamos que você mantenha a transitividade habilitada não alterando o valor padrão de **ponte de todos os links de site** (habilitado por padrão). No entanto, será necessário desabilitar a **ponte de todos os links de site** e concluir um design de ponte de link de site se:  

- Sua rede IP não está totalmente roteada. Quando você desabilita a **ponte de todos os links de site**, todos os links de site são considerados intransitivos e você pode criar e configurar objetos de ponte de link de site para modelar o comportamento de roteamento real de sua rede.  
- Você precisa controlar o fluxo de replicação das alterações feitas no Active Directory Domain Services (AD DS). Ao desabilitar a **ponte de todos os links de site** para o transporte de IP do link de site e configurar uma ponte de link de site, a ponte de link de site se tornará o equivalente a uma rede não conjunta. Todos os links de site na ponte de link de site podem rotear transitivamente, mas eles não são roteados para fora da ponte de link de site.  

Para obter mais informações sobre como usar o snap-in Active Directory sites e serviços para desabilitar a configuração **ponte de todos os links de site** , consulte o artigo [habilitar ou desabilitar pontes de link de site](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controlando o fluxo de replicação do AD DS

Dois cenários nos quais você precisa de um design de ponte de link de site para controlar o fluxo de replicação incluem controlar o failover de replicação e controlar a replicação por meio de um firewall.  
  
### <a name="controlling-replication-failover"></a>Controlando o failover de replicação

Se sua organização tiver uma topologia de rede hub e spoke, você geralmente não deseja que os sites de satélite criem conexões de replicação com outros sites de satélite se todos os controladores de domínio no site de Hub falharem. Nesses cenários, você deve desabilitar a **ponte de todos os links de site** e criar pontes de link de site para que as conexões de replicação sejam criadas entre o site satélite e outro site de Hub que seja apenas um ou dois saltos de distância do site satélite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controlando a replicação por meio de um firewall

Se dois controladores de domínio que representam o mesmo domínio em dois sites diferentes forem especificamente autorizados a se comunicar entre si somente por meio de um firewall, você poderá desabilitar a **ponte de todos os links de site** e criar pontes de link de site para sites no mesmo lado do firewall. Portanto, se sua rede estiver separada por firewalls, recomendamos que você desabilite a transitividade de links de site e crie pontes de link de site para a rede em um lado do firewall. Para obter informações sobre como gerenciar a replicação por meio de firewalls, consulte o artigo [Active Directory em redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=107074).
