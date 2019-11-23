---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federação autônomo usando WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fa89a4a57c618fd711234b8770a35750f3099bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358957"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federação autônomo usando WID

Um servidor de Federação autônomo\-no Serviços de Federação do Active Directory (AD FS) \(AD FS\) consiste em um único servidor que hospeda um Serviço de Federação configurado para usar o banco de dados interno do Windows \(WID\). Essa AD FS topologia é para os laboratórios de teste. Não é recomendável para ambientes de produção porque ele tem um limite de apenas um servidor de Federação e não pode ser usado para escalar verticalmente para mais servidores.  
  
Se desejar adicionar servidores de Federação adicionais ao laboratório de teste, você deverá recriar o Serviço de Federação do zero implantando qualquer uma das outras topologias mencionadas mais adiante nesta seção. Portanto, recomendamos que você use essa topologia para um laboratório de teste ou uma\-de prova de\-ambiente de conceito em sua rede de teste particular, na qual um único servidor de Federação é adequado, conforme mostrado na ilustração a seguir.  
  
![servidor usando WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerações de laboratório de teste  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia para ambientes de laboratório de teste.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   A tecnologia da informação \(\) profissionais de ti ou arquitetos de ti que desejam avaliar ou desenvolver uma prova de conceito para essa tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Fácil de configurar em um ambiente de laboratório de teste  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Somente um servidor de Federação por Serviço de Federação não \(capacidade de escalar verticalmente para um farm\)  
  
-   Não redundante \(apenas uma única instância do banco de dados de configuração AD FS existe\)  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
