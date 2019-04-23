---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas DNS integradas ao Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873247"
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas DNS integradas ao Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de nome DNS (sistema) de domínio que executam em controladores de domínio podem armazenar suas zonas nos serviços de domínio Active Directory (AD DS). Dessa forma, não é necessário configurar uma topologia de replicação DNS separada que usa as transferências de zona DNS comuns porque todos os dados da zona são replicados automaticamente por meio de replicação do Active Directory. Isso simplifica o processo de implantação do DNS e fornece as seguintes vantagens:  
  
-   Vários mestres são criados para replicação do DNS. Portanto, qualquer controlador de domínio do domínio que executam o serviço servidor DNS pode gravar atualizações para as zonas DNS integradas ao Active Directory para o nome de domínio para o qual eles são autoritativos. Uma topologia de transferência de zona DNS separada não é necessária.  
  
-   Há suporte para atualizações dinâmicas seguras. Atualizações dinâmicas seguras permitem que um administrador controlar quais computadores atualizam quais nomes e impedir que computadores não autorizados substituindo nomes existentes no DNS.  
  
DNS do Active Directory integrado no Windows Server 2008 armazena dados da zona em partições de diretório de aplicativo. (Não há nenhuma alteração de comportamento da integração de DNS baseados no Windows Server 2003 com Active Directory.) As seguintes partições de diretório de aplicativo específicas do DNS são criadas durante a instalação do AD DS:  
  
-   Uma partição de diretório de aplicativos em toda a floresta chamada ForestDnsZones  
  
-   Partições de diretório de aplicativo de todo o domínio para cada domínio na floresta, chamado DomainDnsZones  
  
Para obter mais informações sobre como o AD DS armazena informações de DNS em partições de aplicativo, consulte o [referência técnica do DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> É recomendável instalar o DNS, quando você executar o Active Directory Assistente de instalação do serviços de domínio Active (Dcpromo.exe). Se você fizer isso, o assistente cria automaticamente a delegação de zona DNS. Para obter mais informações, consulte [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


