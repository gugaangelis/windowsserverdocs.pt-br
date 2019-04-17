---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Criando um projeto de infraestrutura DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>Criando um projeto de infraestrutura DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de criar seus designs de floresta e domínio do Active Directory, você deve projetar uma infraestrutura de sistema de nome de domínio (DNS) para dar suporte a estrutura lógica do Active Directory. DNS permite que os usuários usem nomes amigáveis que são fáceis de lembrar para se conectar a computadores e outros recursos em redes IP. Serviços de domínio Active Directory (AD DS) no Windows Server 2008 requer que o DNS.  
  
O processo para criar o DNS para oferecer suporte ao AD DS varia de acordo com se sua organização já tem um serviço de servidor DNS existente ou se você estiver implantando um novo serviço de servidor DNS:  
  
-   Se você já tiver uma infraestrutura DNS existente, você deve integrar o namespace do Active Directory para esse ambiente. Para obter mais informações, consulte [integrando AD DS em uma infraestrutura existente do DNS ](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
  
-   Se você não tiver uma infraestrutura DNS no lugar, você deve projetar e implantar uma nova infraestrutura DNS para dar suporte ao AD DS. Para obter mais informações, consulte sistema de nome de domínio (DNS) implantando ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656)).  
  
Caso a organização tenha uma infraestrutura DNS existente, você deve garantir que você entende como sua infraestrutura DNS vai interagir com o namespace do Active Directory. Para uma planilha para ajudá-lo a documentar seu design existente de infraestrutura DNS, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra "DNS inventário" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Além de IP versão 4 (IPv4) endereços, Windows Server 2008 também suporta IP versão 6 (IPv6) resolve. Para uma planilha para ajudá-lo em detalhes os endereços IPv6 enquanto documentar o método de resolução recursiva nome da sua estrutura DNS atual, consulte [apêndice a: DNS inventário](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).  
  
Antes de criar sua infraestrutura DNS para suporte ao AD DS, ele pode ser útil saber mais sobre a hierarquia DNS, o processo de resolução de nome DNS e como o DNS oferece suporte ao AD DS. Para obter mais informações sobre o processo de resolução de hierarquia e o nome DNS, consulte a referência técnica do DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Para saber mais sobre como o DNS oferece suporte ao AD DS, consulte o suporte de DNS para referência técnica do Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Revisando os conceitos do DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS e o AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [Atribuindo o DNS para a função do AD DS proprietário](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [Integração do AD DS em uma infraestrutura DNS existente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  


