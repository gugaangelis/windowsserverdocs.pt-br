---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas DNS integradas ao Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 858dbc539d48dc3d024ceb871fa9f2d1f80cce0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402666"
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas DNS integradas ao Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os servidores DNS (sistema de nomes de domínio) executados em controladores de domínio podem armazenar suas zonas no Active Directory Domain Services (AD DS). Dessa forma, não é necessário configurar uma topologia de replicação de DNS separada que usa transferências de zona DNS comuns, pois todos os dados de zona são replicados automaticamente por meio de Active Directory replicação. Isso simplifica o processo de implantação do DNS e oferece as seguintes vantagens:  
  
-   Vários mestres são criados para replicação de DNS. Portanto, qualquer controlador de domínio no domínio que executa o serviço do servidor DNS pode gravar atualizações nas zonas DNS integradas ao Active Directory para o nome de domínio para o qual elas são autoritativas. Uma topologia de transferência de zona DNS separada não é necessária.  
  
-   Há suporte para atualizações dinâmicas seguras. As atualizações dinâmicas seguras permitem que um administrador controle quais computadores atualizam quais nomes e impedir que computadores não autorizados substituam nomes existentes no DNS.  
  
O DNS integrado ao Active Directory no Windows Server 2008 armazena dados de zona em partições de diretório de aplicativo. (Não há alterações comportamentais na integração de DNS com base no Windows Server 2003 com Active Directory.) As partições de diretório de aplicativo específicas do DNS a seguir são criadas durante a instalação do AD DS:  
  
-   Uma partição de diretório de aplicativos de toda a floresta, chamada ForestDnsZones  
  
-   Partições de diretório de aplicativo de todo o domínio para cada domínio na floresta, chamado DomainDnsZones  
  
Para obter mais informações sobre como AD DS armazena informações de DNS em partições de aplicativo, consulte a [referência técnica do DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Recomendamos que você instale o DNS ao executar o Assistente para Instalação do Active Directory Domain Services (Dcpromo. exe). Se você fizer isso, o assistente criará automaticamente a delegação de zona DNS. Para obter mais informações, consulte [implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


