---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Práticas recomendadas para proteger o Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8935a08b9ab8f99b5f221a14b93974872f11a091
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821289"
---
# <a name="best-practices-for-securing-active-directory"></a>Práticas recomendadas para proteger o Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento fornece uma perspectiva do profissional e contém um conjunto de técnicas práticas para ajudar os executivos de ti a proteger um ambiente corporativo de Active Directory. O Active Directory desempenha um papel fundamental na infraestrutura de TI e garante a harmonia e segurança de diferentes recursos de rede em um ambiente interconectado global. Os métodos abordados são baseados amplamente na experiência da organização do ISRM (segurança de informações e gerenciamento de riscos) da Microsoft, que é responsável por proteger os ativos da TI da Microsoft e outras divisões de negócios da Microsoft, além de aconselhar um número selecionado de clientes do Microsoft Global 500.  
  
-   [Síntese](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introdução](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Alternativas ao comprometimento](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Contas atraentes para roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Reduzindo a superfície de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementando modelos administrativos de menos privilégio](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Como implementar hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protegendo controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Active Directory de monitoramento para sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendações de política de auditoria](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planejando o comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Mantendo um ambiente mais seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apêndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apêndice B: contas e grupos com privilégios no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice C: contas e grupos protegidos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apêndice E: protegendo grupos de administradores corporativos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice F: protegendo grupos de administradores de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apêndice G: protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apêndice H: Protegendo contas e grupos de administrador local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apêndice I: Criando contas de gerenciamento para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apêndice L: eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apêndice M: links de documentos e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


