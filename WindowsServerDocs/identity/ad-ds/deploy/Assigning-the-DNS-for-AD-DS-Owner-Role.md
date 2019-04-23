---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Atribuindo o DNS para a função de proprietário do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884017"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Atribuindo o DNS para a função de proprietário do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O proprietário da floresta atribui um domínio nome DNS (sistema) para o proprietário de serviços de domínio Active Directory (AD DS) para a floresta. O DNS para o proprietário da floresta do AD DS é uma pessoa (ou grupo de pessoas) quem é responsável por supervisionar a implantação do DNS para a infraestrutura do AD DS e certificando-se de que (se necessário) os nomes de domínio são registrados com as autoridades competentes da Internet.  
  
O DNS para o proprietário do AD DS é responsável pelo DNS para o design do AD DS para a floresta. Se sua organização está funcionando no momento um serviço do servidor DNS, o designer DNS para o serviço servidor DNS existente funciona com o DNS para o proprietário do AD DS delegar o nome DNS de raiz da floresta para servidores DNS executando em controladores de domínio.  
  
Além disso, o DNS para o proprietário do AD DS para a floresta mantém contato com o grupo de protocolo de configuração de Host dinâmico (DHCP) e o grupo DNS da organização e coordena os planos dos proprietários individuais do DNS de cada domínio na floresta (se houver) a esses grupos. O proprietário do DNS para a floresta garante que os grupos de DHCP e DNS estão envolvidos no DNS para o processo de design do AD DS para que cada grupo está ciente do plano de design de DNS e pode fornecer a entrada no início.  
  


