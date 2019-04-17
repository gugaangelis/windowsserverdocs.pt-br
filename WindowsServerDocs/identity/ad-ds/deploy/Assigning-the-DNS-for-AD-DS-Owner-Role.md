---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: "Atribuindo o DNS para a função do AD DS proprietário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e393cbf32aa5a13ff22044eabb8c575508baaf79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Atribuindo o DNS para a função do AD DS proprietário

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O proprietário da floresta atribui um sistema DNS (Domain Name) para o proprietário de serviços de domínio do Active Directory (AD DS) para a floresta. O DNS para proprietário da floresta do AD DS é uma pessoa (ou grupo de pessoas) que é responsável por supervisionar a implantação do DNS para a infraestrutura do AD DS e certificando-se de que (se necessário) nomes de domínio são registrados com as autoridades competentes da Internet.  
  
O DNS para proprietário do AD DS é responsável pelo DNS para o design do AD DS para floresta. Se sua organização está funcionando no momento um serviço de servidor DNS, o designer DNS para o serviço de servidor DNS existente funciona com o DNS para proprietário do AD DS delegar o nome DNS raiz da floresta para servidores DNS funcionando em controladores de domínio.  
  
O DNS para proprietário da floresta do AD DS também mantém o contato com o grupo Dynamic Host Configuration Protocol (DHCP) e o grupo DNS da organização e coordenadas os planos dos proprietários DNS individuais de cada domínio na floresta (se houver) com esses grupos. O proprietário DNS para a floresta garante que os grupos DHCP e DNS estão envolvidos no DNS para o processo de design do AD DS para que cada grupo está ciente do plano de design de DNS e pode fornecer entrada no início.  
  


