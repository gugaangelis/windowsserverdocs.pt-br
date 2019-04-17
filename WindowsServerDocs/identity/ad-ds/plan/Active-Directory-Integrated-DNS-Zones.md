---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Active Directory integrado zonas de DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory integrado zonas de DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de nome DNS (sistema) do domínio executando em controladores de domínio podem armazenar suas zonas nos serviços de domínio do Active Directory (AD DS). Dessa forma, não é necessário configurar uma topologia de replicação separada do DNS que usa transferências de zona DNS comuns porque todos os dados da zona é replicado automaticamente por meio da replicação do Active Directory. Isso simplifica o processo de implantação de DNS e fornece as seguintes vantagens:  
  
-   Vários mestres são criados para replicação de DNS. Portanto, qualquer controlador de domínio no domínio que executa o serviço de servidor DNS pode gravar as atualizações para as zonas DNS integradas ao Active Directory para o nome de domínio nos quais é autoritativos. Uma topologia separada de transferência de zona DNS não é necessário.  
  
-   Há suporte para atualizações dinâmicas. As atualizações dinâmicas permitem que um administrador controlar quais computadores atualizam quais nomes e impedir que computadores não autorizados substituam os nomes existentes no DNS.  
  
DNS do Active Directory integrado no Windows Server 2008 armazena dados de zona em partições de diretório do aplicativo. (Não há nenhuma alteração comportamentais da integração do DNS com base no Windows Server 2003 com o Active Directory.) As seguintes partições de diretório de aplicativo específicas do DNS são criadas durante a instalação do AD DS:  
  
-   Uma partição de diretório de aplicativos em toda a floresta, chamada ForestDnsZones  
  
-   Partições de diretório do aplicativo de todo o domínio para cada domínio na floresta, denominado DomainDnsZones  
  
Para saber mais sobre como o AD DS armazena informações de DNS em partições de aplicativo, consulte o [referência técnica do DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Recomendamos que você instale DNS quando você executa o Assistente de instalação dos serviços do Active Directory domínio (Dcpromo.exe). Se você fizer isso, o assistente cria a delegação de zona DNS automaticamente. Para obter mais informações, consulte [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


