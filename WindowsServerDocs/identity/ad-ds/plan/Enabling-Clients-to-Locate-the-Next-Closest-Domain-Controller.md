---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permitindo que os clientes localizem o controlador de domínio mais próximo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880357"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitindo que os clientes localizem o controlador de domínio mais próximo

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você tiver um controlador de domínio que executa o Windows Server 2008 ou mais recente, você pode fazer ele possível para computadores cliente que executam o Windows Vista ou mais recente ou o Windows Server 2008 ou mais recente para localizar os controladores de domínio de forma mais eficiente, habilitando o **tentar Avançar Site mais próximo** configuração de diretiva de grupo. Esta configuração aprimora o localizador de controlador de domínio (DC Locator), ajudando a simplificar o tráfego de rede, especialmente em grandes empresas que têm várias filiais e em sites.

Essa nova configuração pode afetar como você configura os custos de link de site porque ele afeta a ordem na qual os controladores de domínio estão localizados. Para empresas que têm muitos sites de hub e filiais, você pode reduzir significativamente o tráfego do Active Directory na rede, garantindo que os clientes fazer failover para o próximo site mais próximo de hub quando eles não é possível encontrar um controlador de domínio no site mais próximo do hub.

Como uma prática recomendada geral, você deve simplificar a sua topologia de site e os custos de link de site tanto quanto possível, se você habilitar a **tentar Site mais próximo** configuração. Em empresas com muitos sites de hub, isso pode simplificar a todos os planos que fazer para lidar com situações em que os clientes em um site precisam fazer failover para um controlador de domínio em outro site.

Por padrão, o **tentar Site mais próximo** não está habilitada. Quando a configuração não está habilitada, o localizador de controlador de domínio usa o seguinte algoritmo para localizar um controlador de domínio:

- Tente localizar um controlador de domínio no mesmo site.
- Se nenhum controlador de domínio está disponível no mesmo site, tente localizar qualquer controlador de domínio no domínio.

> [!NOTE]
> Isso é o mesmo algoritmo que o localizador de controlador de domínio usado nas versões anteriores do Active Directory. Para obter mais informações, consulte o artigo [como o suporte de DNS para funciona do Active Directory](https://go.microsoft.com/fwlink/?LinkId=108587).

Se você habilitar a **tentar Site mais próximo** configuração, o localizador de controlador de domínio usa o seguinte algoritmo para localizar um controlador de domínio:

- Tente localizar um controlador de domínio no mesmo site.
- Se nenhum controlador de domínio está disponível no mesmo site, tente localizar um controlador de domínio no próximo site mais próximo. Um site é mais próximo, se ele tiver um site-link menor custo que outro site com um site-link mais alto custo.
- Se nenhum controlador de domínio está disponível no próximo site mais próximo, tente localizar qualquer controlador de domínio no domínio.

O **tentar Site mais próximo** configuração funciona em coordenação com cobertura de site automática. Por exemplo, se o site mais próximo não tem nenhum controlador de domínio, o localizador de controlador de domínio tenta encontrar o controlador de domínio que executa a cobertura de site automática para o site.

Por padrão, o localizador de DC não considera qualquer site que contém um controlador de domínio somente leitura (RODC) quando ele determina o próximo site mais próximo. Além disso, quando o cliente recebe uma resposta do controlador de domínio que executa uma versão anterior ao Windows Server 2008, o comportamento de localizador do controlador de domínio é o mesmo que quando o, em seguida, a configuração não está habilitado.

Por exemplo, suponha que uma topologia de site tem quatro sites com os valores de link de site na ilustração a seguir. Neste exemplo, todos os controladores de domínio são controladores de domínio graváveis que executam o Windows Server 2008 ou mais recente.

![habilitar clientes para localizar o controlador de domínio](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Quando o **tentar Site mais próximo** configuração de diretiva de grupo está habilitada neste exemplo, se um computador cliente em Site_B tenta localizar um controlador de domínio, ele primeiro tenta encontrar um controlador de domínio em seu próprio Site_B. Se nenhum estiver disponível no Site_B, ele tenta encontrar um controlador de domínio em Site_A.

Se a configuração não estiver habilitada, o cliente tenta localizar um controlador de domínio em Site_A, Site_C ou Site_D se nenhum controlador de domínio está disponível no Site_B.

> [!NOTE]
> O **tentar Site mais próximo** configuração funciona em coordenação com cobertura de site automática. Por exemplo, se o site mais próximo não tem nenhum controlador de domínio, o localizador de controlador de domínio tenta encontrar o controlador de domínio que executa a cobertura de site automática para o site.

Para aplicar a **tentar Site mais próximo** configuração, você pode criar um objeto de diretiva de grupo (GPO) e vinculá-lo ao objeto apropriado para sua organização, ou você pode modificar a diretiva de domínio padrão para que ele afeta todos os clientes que executam o Windows Vista ou mais recente e no Windows Server 2008 ou mais recente no domínio. Para obter mais informações sobre como definir as **tentar Site mais próximo** , consulte [habilitar clientes para localizar um controlador de domínio no próximo Site mais próximo](https://technet.microsoft.com/library/cc772592.aspx).
