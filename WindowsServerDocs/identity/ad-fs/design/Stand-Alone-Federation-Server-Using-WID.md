---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: "Servidor de Federação autônomo usando o trabalho"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de Federação autônomo usando o trabalho

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um servidor de Federação stand\ sozinho em serviços de Federação do Active Directory \(AD FS\) consiste em um único servidor que hospeda um serviço de Federação configurada para usar \(WID\) o banco de dados interno do Windows. Essa topologia AD FS é para laboratórios de teste. Não recomendamos-lo para ambientes de produção porque ele tem um limite de servidor de Federação apenas uma, e ele não pode ser usado de dimensionar para servidores mais.  
  
Se você quiser adicionar servidores de Federação adicionais de laboratório de teste, você deve recriar o serviço de Federação do zero Implantando qualquer uma das outras topologias mencionadas posteriormente nesta seção. Portanto, recomendamos que você use essa topologia para um laboratório de teste ou um ambiente de conceito de of\ proof\ em sua rede de teste particular em que um servidor de Federação único é adequado, conforme mostrado na ilustração a seguir.  
  
![servidor usando o trabalho](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerações de laboratório de teste  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que são associadas essa topologia para ambientes de laboratório de teste.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   \(IT\) profissionais ou arquitetos de TI que desejam avaliar ou desenvolver uma prova de conceito para essa tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são as vantagens de usar essa topologia?  
  
-   Fácil de configurar em um ambiente de laboratório de teste  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações de usar essa topologia?  
  
-   Servidor de Federação apenas um por um serviço de Federação \ (nenhuma funcionalidade de dimensionar para um farm\)  
  
-   Não redundantes \ (somente uma única instância de exists\ de banco de dados de configuração do AD FS)  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
