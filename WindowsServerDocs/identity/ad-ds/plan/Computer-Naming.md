---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura do computador
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d0dbe2da76a1cf3d1a4dd74183b5dd7106ef17b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="computer-naming"></a>Nomenclatura do computador

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando um computador executando o sistema operacional Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista ingressa em um domínio, por padrão o computador atribui a próprio um nome. O nome atribui a próprio compreende o nome do host do computador (ou seja, o nome de computador nas propriedades do sistema) e o nome do sistema de nome de domínio (DNS) do domínio do Active Directory que tenha ingressado no computador (ou seja, sufixo DNS primário nas propriedades do sistema). A concatenação do nome do host e o nome DNS do domínio é conhecida como o nome de domínio totalmente qualificado (FQDN). Por exemplo, se um computador com o nome de host Server1 ingressar no domínio corp.contoso.com, o FQDN do computador é server1.corp.contoso.com.  
  
Se um computador já tem um nome de domínio DNS diferente que foi inserido em uma zona DNS estaticamente ou registrado por um serviço de servidor DNS/Dynamic Host Configuration protocolo DHCP () integrado, o FQDN do computador é diferente do nome que foi registrado anteriormente. O computador pode ser referenciado por qualquer nome.  
  
Para saber mais sobre as convenções de nomenclatura nos serviços de domínio do Active Directory (AD DS), consulte convenções de nomenclatura no Active Directory para computadores, domínios, sites e unidades organizacionais ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


