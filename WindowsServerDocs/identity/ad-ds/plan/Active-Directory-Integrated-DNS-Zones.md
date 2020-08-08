---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas DNS integradas ao Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a10fb5f15579b223540ffcff1ca004c78d6e71ef
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941441"
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas DNS integradas ao Active Directory

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os servidores DNS (sistema de nomes de domínio) executados em controladores de domínio podem armazenar suas zonas no Active Directory Domain Services (AD DS). Dessa forma, não é necessário configurar uma topologia de replicação de DNS separada que usa transferências de zona DNS comuns, pois todos os dados de zona são replicados automaticamente por meio de Active Directory replicação. Isso simplifica o processo de implantação do DNS e oferece as seguintes vantagens:

- Vários mestres são criados para replicação de DNS. Portanto, qualquer controlador de domínio no domínio que executa o serviço do servidor DNS pode gravar atualizações nas zonas DNS integradas ao Active Directory para o nome de domínio para o qual elas são autoritativas. Uma topologia de transferência de zona DNS separada não é necessária.

- Há suporte para atualizações dinâmicas seguras. As atualizações dinâmicas seguras permitem que um administrador controle quais computadores atualizam quais nomes e impedir que computadores não autorizados substituam nomes existentes no DNS.

O DNS integrado ao Active Directory no Windows Server 2008 armazena dados de zona em partições de diretório de aplicativo. (Não há alterações comportamentais na integração de DNS com base no Windows Server 2003 com Active Directory.) As partições de diretório de aplicativo específicas do DNS a seguir são criadas durante a instalação do AD DS:

- Uma partição de diretório de aplicativos de toda a floresta, chamada ForestDnsZones

- Partições de diretório de aplicativo de todo o domínio para cada domínio na floresta, chamado DomainDnsZones

Para obter mais informações sobre como AD DS armazena informações de DNS em partições de aplicativo, consulte a [referência técnica do DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779926(v=ws.10)).

> [!NOTE]
> Recomendamos que você instale o DNS ao executar o Assistente para Instalação do Active Directory Domain Services (Dcpromo.exe). Se você fizer isso, o assistente criará automaticamente a delegação de zona DNS. Para obter mais informações, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).
