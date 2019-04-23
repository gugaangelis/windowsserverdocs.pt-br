---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Criar um design de infraestrutura de DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856127"
---
# <a name="creating-a-dns-infrastructure-design"></a>Criar um design de infraestrutura de DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de criar seus designs de floresta e domínio do Active Directory, você deve projetar uma infraestrutura de sistema de nome de domínio (DNS) para dar suporte à estrutura lógica do Active Directory. DNS permite que os usuários usem nomes amigáveis que são fáceis de lembrar-se para se conectar a computadores e outros recursos em redes IP. Serviços de domínio Active Directory (AD DS) no Windows Server 2008 requer que o DNS.  
  
O processo para a criação de DNS para dar suporte ao AD DS varia de acordo com se sua organização já tem um serviço de servidor DNS existente ou se você estiver implantando um novo serviço de servidor DNS:  
  
- Se você já tiver uma infra-estrutura DNS existente, você deve integrar o namespace do Active Directory no ambiente. Para obter mais informações, consulte [integrando o AD DS em uma infra-estrutura DNS existente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Se você não tiver uma infraestrutura de DNS em vigor, você deve projetar e implantar uma nova infra-estrutura DNS para oferecer suporte ao AD DS. Para obter mais informações, consulte [sistema de nome de domínio (DNS) implantando](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Se sua organização tiver uma infra-estrutura DNS existente, certifique-se de que você compreenda como a sua infraestrutura DNS irá interagir com o namespace do Active Directory. Para uma planilha ajudar a documentar o design de infraestrutura DNS existente, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip da [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e Abra "Inventário de DNS" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Além de IP versão 4 (IPv4) endereços, endereços do Windows Server também oferece suporte a IP versão 6 (IPv6). Para uma planilha para ajudá-lo listando os endereços IPv6 ao documentarem o método de resolução de nome recursiva de sua estrutura atual do DNS, consulte [apêndice a: Inventário DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Antes de projetar sua infraestrutura DNS para dar suporte ao AD DS, ele pode ser útil saber mais sobre a hierarquia de DNS, o processo de resolução de nome DNS e como o DNS dá suporte ao AD DS. Para obter mais informações sobre o processo de resolução de hierarquia e o nome DNS, consulte a referência técnica do DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Para obter mais informações sobre como o DNS oferece suporte ao AD DS, consulte o suporte de DNS para referência técnica do Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Nesta seção  

- [Revisando os conceitos DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS e AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Atribuir o DNS para a função do AD DS proprietário](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [A integração do AD DS em uma infra-estrutura DNS existente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
