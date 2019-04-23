---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federação autônomo usando WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876517"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federação autônomo usando WID

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um modo de espera\-servidor de Federação autônomo nos serviços de Federação do Active Directory \(do AD FS\) consiste em um único servidor que hospeda um serviço de Federação configurados para usar o banco de dados interno do Windows \(WID\). Essa topologia do AD FS é para laboratórios de teste. Não recomendamos-lo para ambientes de produção porque ela tem um limite de apenas um servidor de federação, e ele não pode ser usado para escalar verticalmente para mais servidores.  
  
Se você quiser adicionar servidores de Federação adicionais ao seu laboratório de teste, você deve recompilar o serviço de Federação do zero com a implantação de qualquer uma das outras topologias mencionadas posteriormente nesta seção. Portanto, é recomendável que você use essa topologia para um laboratório de teste ou uma prova\-de\-ambiente conceito em sua rede privada de teste no qual um servidor de Federação único é adequado, conforme mostrado na ilustração a seguir.  
  
![servidor usando o WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerações sobre o laboratório de teste  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas com esta topologia para ambientes de laboratório de teste.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Tecnologia da informação \(IT\) profissionais ou arquitetos de TI que desejam avaliar ou desenvolver uma prova de conceito para esta tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Fácil de configurar em um ambiente de laboratório de teste  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Apenas um servidor de federação por serviço de Federação \(nenhuma capacidade de escalar verticalmente para um farm\)  
  
-   Não redundantes \(existe apenas uma única instância do banco de dados de configuração do AD FS\)  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
