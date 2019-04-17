---
title: "Atualizações de componentes de serviços de domínio do Active Directory"
description: "Este documento discute as atualizações de componente do AD DS para o Windows Server 2012 R2"
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 52d3dab542b4250670067e927f42ddf1597fc852
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-domain-services-component-updates"></a>Atualizações de componentes de serviços de domínio do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Esse módulo apresenta os componentes que recebeu pequenas atualizações em serviços de diretório e espaços de identidade.  
  
|Sobre a autora|  
|--------------------|  
|**Autor:**|Tema Justin Turner|  
|**Biografia:**|Tema Justin é um engenheiro de escalonamento de suporte sênior com a equipe de serviços de diretório baseada em Irving, Texas, EUA.  Ele criou ou contribuiu para muitos cursos de treinamento e artigos para a Microsoft Knowledgebase nos últimos 12 anos. Ele ensina os funcionários da Microsoft e a arquitetura de produto novos clientes, é um estatuto Microsoft Certified mestre (MCM), Microsoft Certified treinador (MCT) e segura é um mestre Grau no computador educação e sistemas cognitiva.|  
|**Colaboradores**|Esse módulo de treinamento inclui contribuições de *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* e *Herbert Mauerer*|  
|**Revisores**|Nossos agradecimentos os seguintes indivíduos que gasto seus próprios tempo revisando e fornecer feedback: *Joey Seifert*, *Justin Hall*|  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
### <a name="what-you-will-learn"></a>O que você aprenderá  
Depois de concluir esse módulo, você será capaz de:  
  
-   Explique as atualizações de componente feitas nas áreas de tecnologia de identidade e serviços de diretório no Windows Server 2012 R2  
  
    -   [Exclusividade SPN e UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Winlogon automático reiniciar logon & #40; ARSO & #41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Atestado de chave do TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Cmdlets CA Backup e restauração do Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Auditoria do processo de linha de comando](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Gerenciamento e proteção de credenciais](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Atualizações de componentes de serviços de diretório](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Níveis funcionais de domínio e da floresta](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Substituição de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Alterações do otimizador de consulta LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Melhorias de evento da falha 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Melhoria de taxa de transferência Active Directory replicação](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


