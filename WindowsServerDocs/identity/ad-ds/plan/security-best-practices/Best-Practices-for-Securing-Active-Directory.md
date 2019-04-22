---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Práticas recomendadas para proteger o Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817077"
---
# <a name="best-practices-for-securing-active-directory"></a>Práticas recomendadas para proteger o Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento fornece a perspectiva do profissional e contém um conjunto de técnicas práticas para ajudar executivos de TI a proteger um ambiente do Active Directory corporativo. O Active Directory desempenha um papel fundamental na infraestrutura de TI e garante a harmonia e segurança de diferentes recursos de rede em um ambiente interconectado global. Os métodos abordados baseiam-se a segurança da informação e gerenciamento de risco (ISRM) experiência da organização, que é responsável por proteger os ativos de IT da Microsoft e outras divisões de negócios da Microsoft, além de prestar consultoria em grande parte um número selecionado de clientes do Microsoft Global 500.  
  
-   [Síntese](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introdução](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Alternativas ao comprometimento](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Contas atraentes para roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Redução da superfície de ataque do Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementando modelos administrativos de privilégio mínimo](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementar Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protegendo controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Monitorando o Active Directory em busca de sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendações de política de auditoria](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planejar para comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Manter um ambiente mais seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apêndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apêndice b: Contas privilegiadas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice C: Contas protegidas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apêndice e: Protegendo grupos de administrador corporativo no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice f: Protegendo grupos de administradores de domínio do Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apêndice h: Protegendo grupos e contas de Administrador Local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apêndice i: Criando o gerenciamento de contas para contas protegidas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apêndice l: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apêndice m: Links de documentos e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


