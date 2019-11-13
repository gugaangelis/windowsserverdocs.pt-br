---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Criar um design de infraestrutura de DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b4b7cea18a6bb6b435b3c3fb6b4e94cfdddb2c04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408971"
---
# <a name="creating-a-dns-infrastructure-design"></a>Criar um design de infraestrutura de DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de criar seus Active Directory designs de floresta e domínio, você deve criar uma infraestrutura de DNS (sistema de nomes de domínio) para dar suporte à sua estrutura lógica Active Directory. O DNS permite que os usuários usem nomes amigáveis que são fáceis de lembrar para se conectar a computadores e outros recursos em redes IP. O Active Directory Domain Services (AD DS) no Windows Server 2008 requer DNS.  
  
O processo para criar o DNS para dar suporte a AD DS varia de acordo com se sua organização já tem um serviço de servidor DNS existente ou se você está implantando um novo serviço do servidor DNS:  
  
- Se você já tiver uma infraestrutura de DNS existente, deverá integrar o namespace Active Directory nesse ambiente. Para obter mais informações, consulte [integrando AD DS em uma infraestrutura de DNS existente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Se você não tiver uma infraestrutura de DNS em vigor, deverá projetar e implantar uma nova infraestrutura de DNS para dar suporte a AD DS. Para obter mais informações, consulte [implantando o DNS (sistema de nomes de domínio)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Se sua organização tiver uma infraestrutura de DNS existente, você deve certificar-se de que entendeu como sua infraestrutura de DNS irá interagir com o namespace Active Directory. Para uma planilha para ajudá-lo a documentar seu design de infraestrutura de DNS existente, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de [auxílios de trabalho para o kit de implantação do Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558) e abra "inventário de DNS" (DSSLOGI_8. doc).  
  
> [!NOTE]  
> Além dos endereços IP versão 4 (IPv4), o Windows Server também dá suporte a endereços IP versão 6 (IPv6). Para uma planilha para ajudá-lo a listar os endereços IPv6 ao documentar o método de resolução de nomes recursivos da sua estrutura DNS atual, consulte o [Apêndice a: inventário de DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Antes de projetar sua infraestrutura DNS para dar suporte ao AD DS, pode ser útil ler sobre a hierarquia DNS, o processo de resolução de nomes DNS e como o DNS dá suporte a AD DS. Para obter mais informações sobre a hierarquia de DNS e o processo de resolução de nomes, consulte a referência técnica do DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Para obter mais informações sobre como o DNS dá suporte a AD DS, consulte o suporte do DNS para Active Directory referência técnica ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Nesta seção  

- [Como examinar conceitos do DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS e AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Como atribuir o DNS para a função de proprietário do AD DS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [Como integrar o AD DS a uma infraestrutura de DNS existente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
