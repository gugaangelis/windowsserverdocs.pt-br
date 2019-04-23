---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Apêndice de referência técnica do controlador de domínio virtualizado
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832227"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Apêndice de referência técnica do controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda:  
  
-   [Terminologia](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologia  
  
-   **Instantâneo** -o estado de uma máquina virtual em um ponto específico no tempo. Ele é dependente da cadeia do instantâneo anterior, no hardware e a plataforma de virtualização.  
  
-   **Clone** - uma conclusão e separar a cópia de uma máquina virtual. Ele é dependente do hardware virtual (hipervisor).  
  
-   **Total de Clone** -um clone completo é uma cópia independente de uma máquina virtual que não compartilha nenhum recurso com a máquina virtual primária após a operação de clonagem. Operação contínua de um clone completo é completamente separada da máquina virtual pai.  
  
-   **Disco diferencial** -uma cópia de uma máquina virtual que compartilha discos virtuais com a máquina virtual primária de maneira contínua. Normalmente, isso economiza espaço em disco e permite que várias máquinas virtuais para usar a mesma instalação de software.  
  
-   **Cópia da VM**- uma cópia do sistema de todos os arquivos relacionados e as pastas de uma máquina virtual do arquivo.  
  
-   **Cópia de arquivo do VHD** -uma cópia do VHD de uma máquina virtual  
  
-   **ID de geração de VM** – um inteiro de 128 bits dada à máquina virtual pelo hipervisor. Essa ID é armazenada na memória e reiniciado sempre que um instantâneo é aplicado. O projeto usa um mecanismo independente de hipervisor para identificando a ID de geração de VM na máquina virtual. A implementação do Hyper-V expõe a ID na tabela ACPI da máquina virtual.  
  
-   **Importação/exportação** -recurso de um Hyper-V que permite ao usuário salvar a máquina virtual inteira (arquivos VM, VHD e a configuração de máquina). Ele permite que os usuários usando esse conjunto de arquivos para colocar o computador novamente no mesmo computador como a mesma VM (restauração), em um computador diferente, como a mesma VM (mover) ou uma nova VM (cópia)  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
```  
# Unsigned script, requires use of set-executionpolicy remotesigned -force  
# You must run the Windows PowerShell console as an elevated administrator  
  
# Load Active Directory Windows PowerShell Module and switch to AD DS drive  
import-module activedirectory  
cd ad:  
  
## Get Domain NC  
$domainNC = get-addomain  
  
## Get groups and obtain their SIDs   
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
  
$sid1 = (get-adgroup $dcgroup).sid  
  
## Get the DACL of the domain  
$acl = get-acl $domainNC  
  
## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC  
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"  
  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
  
## Add the ACE in the ACL and set the ACL on the object   
  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
write-host "Done writing new VDC permissions."  
cd c:   
```  
  


