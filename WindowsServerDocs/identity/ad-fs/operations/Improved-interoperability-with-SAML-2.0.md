---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Melhor interoperabilidade com SAML 2.0
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d92859a7f8ae37f847a68dae9ca7fd0245308b6a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954222"
---
# <a name="improved-interoperability-with-saml-20"></a>Melhor interoperabilidade com SAML 2.0




AD FS no Windows Server 2016 contém suporte a protocolo SAML adicional, incluindo suporte para importar relações de confiança com base em metadados que contêm várias entidades.  Isso permite que você configure o AD FS para participar de confederações, como Federação InCommon, bem como de outras implementações em conformidade com o padrão eGov 2.0.

A nova funcionalidade baseia-se em grupos de relações de confiança de provedor de declarações ou de terceira parte confiável. Cada grupo é um elemento EntitiesDescriptor (<MD: EntitiesDescriptor>), conforme especificado no perfil eGov 2,0, contendo um ou vários elementos EntityDescriptor.  Os grupos têm regras de autorização comuns e todas as outras propriedades podem ser modificadas como objetos de confiança individuais.

Depois que os grupos de confiança são importados para o AD FS, AD FS atualiza automaticamente as relações de confiança como um grupo com base no documento de metadados.

Habilitar esses cenários é tão simples quanto usar o novo commandlets do PowerShell que adicionam e removem objetos AdfsClaimsProviderTrustsGroup e AdfsRelyingPartyTrustsGroup. Isso pode ser feito usando uma URL de metadados ou um arquivo, conforme mostrado nos exemplos abaixo.

Além disso, AD FS 2016 tem suporte para o parâmetro de escopo, conforme descrito na especificação do SAML Core, seção 3.4.1.2. Esse elemento permite que partes confiáveis especifiquem um ou mais provedores de identidade para uma solicitação de autenticação.

## <a name="examples"></a>Exemplos

```
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"
```



```
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"
```

## <a name="references"></a>Referências

O perfil eGov 2,0 pode ser encontrado [aqui.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)

A especificação de núcleo SAML pode ser encontrada [aqui.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)


