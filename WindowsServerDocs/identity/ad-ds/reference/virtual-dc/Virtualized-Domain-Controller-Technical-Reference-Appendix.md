---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: "Apêndice de referência técnica do controlador de domínio virtualizado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Apêndice de referência técnica do controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda:  
  
-   [Terminologia](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologia  
  
-   **Instantâneo** -o estado de uma máquina virtual em um determinado momento. É dependente da cadeia de instantâneos anteriores tirados, no hardware e na plataforma de virtualização.  
  
-   **Clone** - um concluir e separar a cópia de uma máquina virtual. Ele depende do hardware virtual (hipervisor).  
  
-   **Full Clone** -um clone completo é uma cópia de uma máquina virtual que não compartilha nenhum recurso com a máquina virtual de pai após a operação de clonagem independente. Operação contínua de um clone completo é totalmente separada da máquina virtual pai.  
  
-   **Diferencial de disco** -uma cópia de uma máquina virtual que compartilha discos virtuais com a máquina virtual de pai de forma contínua. Geralmente, isso economiza espaço em disco e permite várias máquinas virtuais para usar a mesma instalação de software.  
  
-   **Cópia de VM**- uma cópia do sistema de todos os arquivos relacionados e pastas de uma máquina virtual do arquivo.  
  
-   **Cópia do arquivo VHD** -uma cópia de VHD de uma máquina virtual  
  
-   **ID de geração de VM** - um inteiro de 128 bits dada para a máquina virtual pelo hipervisor. Essa ID é armazenado na memória e redefinir sempre que um instantâneo é aplicado. O design usa um mecanismo de hipervisor independentes para o ID de geração de VM na máquina virtual à tona por meio. A implementação do Hyper-V expõe a ID na tabela ACPI da máquina virtual.  
  
-   **Importação/exportação** -recurso A Hyper-V que permite que o usuário salve a toda a máquina virtual (VM arquivos VHD e configuração do computador). Ele permite que os usuários usando esse conjunto de arquivos para trazer o computador novamente na mesma máquina como a mesma VM (restauração), em um computador diferente, como a mesma VM (mover), ou uma nova VM (copiar)  
  
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
  


