---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura de computador
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8355b5b54e2ff17a4230c658b63745158f573fce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822809"
---
# <a name="computer-naming"></a>Nomenclatura de computador

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando um computador que executa o sistema operacional Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista ingressa em um domínio, por padrão, o computador atribui um nome a si mesmo. O nome que ele atribui é composto pelo nome do host do computador (ou seja, nome do computador nas propriedades do sistema) e pelo nome DNS (sistema de nomes de domínio) do domínio Active Directory que o computador ingressou (ou seja, o sufixo DNS primário nas propriedades do sistema). A concatenação do nome do host e o nome DNS do domínio são conhecidas como o FQDN (nome de domínio totalmente qualificado). Por exemplo, se um computador com o nome de host Server1 ingressar no domínio corp.contoso.com, o FQDN do computador será server1.corp.contoso.com.  
  
Se um computador já tiver um nome de domínio DNS diferente que foi inserido estaticamente em uma zona DNS ou registrado por um serviço de servidor DNS/DHCP (protocolo de configuração de host dinâmico) integrado, o FQDN do computador será distinto do nome que foi registrado anteriormente. O computador pode ser referenciado por um dos nomes.  
  
Para obter mais informações sobre convenções de nomenclatura no Active Directory Domain Services (AD DS), consulte Convenções de nomenclatura no Active Directory para computadores, domínios, sites e unidades organizacionais ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


