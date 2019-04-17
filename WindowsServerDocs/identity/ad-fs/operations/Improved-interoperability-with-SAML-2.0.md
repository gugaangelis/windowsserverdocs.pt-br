---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Melhor interoperabilidade com SAML 2.0
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>Melhor interoperabilidade com SAML 2.0

>Aplica-se a: Windows Server 2016

  
AD FS no Windows Server 2016 contém suporte de protocolo SAML adicional, incluindo suporte para a importação de relações de confiança com base nos metadados que contém várias entidades.  Isso permite que você configurar o AD FS para participar confederações como federação InCommon e outras implementações em conformidade com o padrão 2.0 eGov.   
  
A nova funcionalidade baseia-se em grupos de terceiro ou relações de confiança de provedor de declarações. Cada grupo é um elemento de EntitiesDescriptor (< md:EntitiesDescriptor >) conforme especificado no eGov 2.0 perfil, que contém um ou vários elementos EntityDescriptor.  Os grupos têm regras de autorização comuns e todas as outras propriedades podem ser modificadas como objetos de confiança individuais.  
  
Depois que os grupos de confiança são importados para o AD FS, o AD FS atualiza automaticamente as relações de confiança como um grupo com base no documento de metadados.  
  
Habilitar esses cenários é tão simple quanto usar o novo commandlets do PowerShell que objetos adicionar e remover AdfsClaimsProviderTrustsGroup e AdfsRelyingPartyTrustsGroup. Isso pode ser feito usando uma URL de metadados ou um arquivo, conforme mostrado nos exemplos abaixo.  
  
Além disso, o AD FS 2016 tem suporte para o parâmetro de escopo, conforme descrito na especificação SAML Core, seção 3.4.1.2. Esse elemento permite que a dependência das partes para especificar um ou mais provedores de identidade para uma autenticação solicitam.  
  
## <a name="examples"></a>Exemplos  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Referências  
  
O perfil eGov 2.0 pode ser encontrado [aqui.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
A especificação SAML Core pode ser encontrada [aqui.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


