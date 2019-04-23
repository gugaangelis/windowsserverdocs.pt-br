---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura de computador
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842157"
---
# <a name="computer-naming"></a>Nomenclatura de computador

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando um computador executando o sistema operacional Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista ingressa um domínio, por padrão, o computador atribui a próprio um nome. O nome, ele atribui a próprio inclui o nome do host do computador (ou seja, o nome de computador nas propriedades do sistema) e o nome do sistema de nome de domínio (DNS) do que o computador ingressado no domínio do Active Directory (ou seja, sufixo DNS primário nas propriedades do sistema). A concatenação do nome do host e o nome DNS do domínio é conhecida como o nome de domínio totalmente qualificado (FQDN). Por exemplo, se um computador com o host nome Server1 junções ao domínio corp.contoso.com, o FQDN do computador é server1.corp.contoso.com.  
  
Se um computador já tiver um nome de domínio DNS diferente que foi inserido em uma zona DNS ou registrado por um serviço de servidor DNS dinâmico/Host Configuration Protocol (DHCP) integrado estaticamente, o FQDN do computador é diferente do nome que foi registrado anteriormente. O computador pode ser referenciado por qualquer nome.  
  
Para obter mais informações sobre convenções de nomenclatura no Active Directory Domain Services (AD DS), consulte convenções de nomenclatura no Active Directory para computadores, domínios, sites e unidades organizacionais ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


