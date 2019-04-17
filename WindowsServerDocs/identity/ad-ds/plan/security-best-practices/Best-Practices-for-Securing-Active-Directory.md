---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: "Práticas recomendadas para proteger o Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>Práticas recomendadas para proteger o Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento fornece perspectiva do profissional e contém um conjunto de práticas técnicas para ajudar a proteger um ambiente do Active Directory corporativo executivos de TI. Active Directory desempenha um papel fundamental na infraestrutura de TI e garante a harmonia e a segurança dos recursos de rede diferentes em um ambiente global, interconectado. Os métodos abordados se baseiam em grande parte a experiência da organização Microsoft informações de segurança e gerenciamento de risco (ISRM), que é sua responsabilidade proteger ativos de IT da Microsoft e outros divisões de empresas Microsoft, além de sugerindo que um número selecionado de clientes do Microsoft Global 500.  
  
-   [Foreword](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [Confirmações](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [Resumo executivo](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introdução](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Caminhos para comprometer](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Contas atraentes de roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Reduzir a superfície de ataque do Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementando modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementando Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protegendo controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [O monitoramento do Active Directory de sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendações de política de auditoria](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planejamento de comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Manter um ambiente mais seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apêndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apêndice b: privilegiadas contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice c: protegidas contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice d: proteção de contas de administrador interno no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apêndice e: Protegendo grupos de administradores de empresa no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice f: Protegendo grupos de administradores de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apêndice h: Protegendo grupos e contas de Administrador Local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apêndice l: eventos de Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apêndice m: Links do documento e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


