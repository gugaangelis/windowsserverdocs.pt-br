---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permitindo que os clientes localizem o controlador de domínio mais próximo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ed7663242ae254ecea945a749ee3ce5fac8f96f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408837"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitindo que os clientes localizem o controlador de domínio mais próximo

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você tiver um controlador de domínio que executa o Windows Server 2008 ou mais recente, poderá torná-lo possível para computadores cliente que executam o Windows Vista ou mais recente ou o Windows Server 2008 ou mais recente para localizar controladores de domínio com mais eficiência, habilitando o **novo site mais próximo** Configuração de política de grupo. Essa configuração melhora o localizador de controlador de domínio (localizador de DC) ajudando a simplificar o tráfego de rede, especialmente em grandes empresas que têm muitas filiais e sites.

Essa nova configuração pode afetar a maneira como você configura os custos do link do site, pois ele afeta a ordem em que os controladores de domínio estão localizados. Para empresas que têm muitos sites de Hub e filiais, você pode reduzir significativamente o tráfego de Active Directory na rede, garantindo que os clientes realizem failover para o site de Hub mais próximo quando não conseguirem encontrar um controlador de domínio no site de Hub mais próximo.

Como uma prática recomendada geral, você deve simplificar a topologia do site e os custos do link do site o máximo possível se habilitar a configuração do **site tentar mais próximo** . Em empresas com muitos sites de Hub, isso pode simplificar os planos que você faz para lidar com situações em que os clientes de um site precisam fazer failover para um controlador de domínio em outro site.

Por padrão, a configuração **tentar site mais próximo** não está habilitada. Quando a configuração não estiver habilitada, o localizador de DC usará o seguinte algoritmo para localizar um controlador de domínio:

- Tente localizar um controlador de domínio no mesmo site.
- Se nenhum controlador de domínio estiver disponível no mesmo site, tente localizar qualquer controlador de domínio no domínio.

> [!NOTE]
> Esse é o mesmo algoritmo que o localizador de DC usado em versões anteriores do Active Directory. Para obter mais informações, consulte o artigo [como o suporte a DNS para Active Directory funciona](https://go.microsoft.com/fwlink/?LinkId=108587).

Se você habilitar a configuração de **experimentar o site mais próximo** , o localizador de DC usará o seguinte algoritmo para localizar um controlador de domínio:

- Tente localizar um controlador de domínio no mesmo site.
- Se nenhum controlador de domínio estiver disponível no mesmo site, tente encontrar um controlador de domínio no próximo site mais próximo. Um site ficará mais próximo se tiver um custo de link de site inferior a outro site com um custo de link de site mais alto.
- Se nenhum controlador de domínio estiver disponível no próximo site mais próximo, tente localizar qualquer controlador de domínio no domínio.

A configuração **Experimente o site mais próximo** funciona em coordenação com a cobertura automática de site. Por exemplo, se o site mais próximo não tiver nenhum controlador de domínio, o localizador de DC tentará localizar o controlador de domínio que executa a cobertura automática de site para esse site.

Por padrão, o localizador de DC não considera nenhum site que contenha um RODC (controlador de domínio somente leitura) ao determinar o próximo site mais próximo. Além disso, quando o cliente recebe uma resposta de um controlador de domínio que executa uma versão anterior ao Windows Server 2008, o comportamento do localizador de DC é o mesmo que a configuração não está habilitada.

Por exemplo, suponha que uma topologia de site tenha quatro sites com os valores de link de site na ilustração a seguir. Neste exemplo, todos os controladores de domínio são controladores de domínio graváveis que executam o Windows Server 2008 ou mais recente.

![permitindo que os clientes localizem o DC](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Quando a configuração tentar Política de Grupo do **site mais próximo** estiver habilitada neste exemplo, se um computador cliente no Site_B tentar localizar um controlador de domínio, ele primeiro tentará localizar um controlador de domínio em seu próprio Site_B. Se nenhum estiver disponível em Site_B, ele tentará encontrar um controlador de domínio em Site_A.

Se a configuração não estiver habilitada, o cliente tentará localizar um controlador de domínio em Site_A, Site_C ou Site_D se nenhum controlador de domínio estiver disponível no Site_B.

> [!NOTE]
> A configuração **Experimente o site mais próximo** funciona em coordenação com a cobertura automática de site. Por exemplo, se o site mais próximo não tiver nenhum controlador de domínio, o localizador de DC tentará localizar o controlador de domínio que executa a cobertura automática de site para esse site.

Para aplicar a configuração **experimentar o site mais próximo** , você pode criar um objeto de política de grupo (GPO) e vinculá-lo ao objeto apropriado para sua organização ou pode modificar a política de domínio padrão para que ela afete todos os clientes que executam o Windows Vista ou mais recente e Windows Server 2008 ou mais recente no domínio. Para obter mais informações sobre como definir a configuração do **site try Next mais** próximo, consulte [habilitar clientes para localizar um controlador de domínio no próximo site mais](https://technet.microsoft.com/library/cc772592.aspx)próximo.
