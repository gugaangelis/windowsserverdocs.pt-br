---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: "Examinando os modelos de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c57c23c0bd8694b66ab29b45bd9e47826ed1e01d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="reviewing-the-domain-models"></a>Examinando os modelos de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os seguintes fatores afetam o modelo de design de domínio que você selecionar:  
  
-   Quantidade de capacidade disponível em sua rede que esteja disposto a alocar os serviços de domínio do Active Directory (AD DS). O objetivo é selecionar um modelo que fornece replicação eficiente de informações com um impacto mínimo sobre largura de banda de rede disponível.  
  
-   Número de usuários em sua organização. Se sua organização incluir um grande número de usuários, implantar mais de um domínio permite que você particionar seus dados e oferece a você mais controle sobre a quantidade de tráfego de replicação que passará por meio de uma conexão de rede específica. Isso torna possível para controlar onde os dados são replicados e reduzir a carga criada pelo tráfego de replicação em links lenta em sua rede.  
  
O design de domínio mais simples é um único domínio. Design de um único domínio, todas as informações são replicadas para todos os controladores de domínio. No entanto, se necessário, você pode implantar domínios regionais adicionais. Isso pode ocorrer se partes da infraestrutura de rede estão conectados por links lentos, e quiser que o proprietário da floresta certificar-se de que o tráfego de replicação não exceder a capacidade que foi alocada no AD DS.  
  
É melhor minimizar o número de domínios que você implantar na floresta. Isso reduz a complexidade geral da implantação e, consequentemente, reduz o custo total de propriedade. A tabela a seguir lista os custos administrativos associados com a adição de domínios regionais.  
  
|Custo|Implicações|  
|--------|----------------|  
|Gerenciamento de vários grupos de administrador de serviço|Cada domínio tem seus próprios grupos de administradores de serviço que precisam ser gerenciados de forma independente. A associação desses grupos de administrador de serviço deve ser controlada com cuidado.|  
|Manter a consistência entre as configurações de política de grupo que são comuns a vários domínios|Configurações de política de grupo que precisam ser aplicados toda a floresta devem ser aplicadas separadamente para cada domínio na floresta individual.|  
|Manter a consistência entre o controle de acesso e as configurações que são comuns a vários domínios de auditoria|Controle de acesso e as configurações que precisam ser aplicadas em toda a floresta de auditoria devem ser aplicados separadamente para cada domínio na floresta individual.|  
|Maior probabilidade de objetos movendo entre domínios|Quanto maior o número de domínios, maior a probabilidade de que os usuários precisam mover de um domínio para outro. Essa mudança pode afetar os usuários finais.|  
  
> [!NOTE]  
>  Políticas de bloqueio de conta e de senha refinadas do Windows Server 2008 também podem afetar o modelo de design de domínio que você selecionar. Antes de nesta versão do Windows Server 2008, você pode aplicar apenas uma conta e senha política de bloqueio, que é especificada no domínio política de domínio padrão, a todos os usuários no domínio. Como resultado, se você quisesse senha diferente e configurações de bloqueio de conta para diferentes conjuntos de usuários, você precisava criar um filtro de senha ou implantar vários domínios. Agora você pode usar políticas de senha refinadas para especificar várias políticas de senha e aplicar políticas de bloqueio de conta e restrições de senha diferentes para diferentes conjuntos de usuários em um único domínio. Para obter mais informações sobre políticas de bloqueio de conta e de senha refinadas, veja o Step-by-Step guia de senha refinadas e configuração de política de bloqueio de conta ([https://go.microsoft.com/fwlink/?LinkID=91477](https://go.microsoft.com/fwlink/?LinkID=91477)).  
  
## <a name="single-domain-model"></a>Modelo de domínio único  
Um modelo de domínio único é o mais fácil de administrar e menos caro manter. Ele consiste em uma floresta que contém um único domínio. Este é o domínio de raiz floresta e contém todas as contas de usuário e grupo na floresta.  
  
Um modelo de floresta única domínio reduz a complexidade administrativa, fornecendo as seguintes vantagens:  
  
-   Qualquer controlador de domínio pode autenticar qualquer usuário na floresta.  
  
-   Todos os controladores de domínio podem ser catálogos globais, portanto você não precisa planejar o posicionamento de servidor do catálogo global.  
  
Em uma floresta de domínio único, todos os dados de diretório é replicado em todas as localizações geográficas que hospeda os controladores de domínio. Enquanto esse modelo é fácil de gerenciar, ele também cria o tráfego de replicação a maioria dos modelos de dois domínio. O diretório de particionamento em vários domínios limita a replicação de objetos para regiões geográficas específicas, mas gera mais sobrecarga administrativa.  
  
## <a name="regional-domain-model"></a>Modelo de domínio regional  
Todos os dados de objeto em um domínio é replicado para todos os controladores de domínio nesse domínio. Por esse motivo, se seu floresta contém um grande número de usuários que são distribuídos em diferentes locais geográficos conectados por uma rede de longa distância (WAN), convém implantar domínios regionais para reduzir o tráfego de replicação nos links WAN. Geograficamente baseado em domínios regionais podem ser organizados de acordo com a conectividade de rede WAN.  
  
O modelo de domínio regional permite que você mantenha um ambiente estável ao longo do tempo. Baseie as regiões usadas para definir domínios no seu modelo de elementos estáveis, como limites continentais. Domínios com base em outros fatores, como grupos dentro da organização, podem alterar com frequência e podem exigir a reestruturação seu ambiente.  
  
O modelo de domínio regional consiste em um domínio raiz da floresta e um ou mais domínios regionais. Criando um projeto de modelo de domínio regional envolve a identificação de qual é o domínio raiz de floresta e determinar o número de domínios adicionais que são necessárias para atender às suas necessidades de replicação. Se sua organização incluir grupos que exigem o isolamento de dados ou isolamento de serviço de outros grupos da organização, crie uma floresta separada para esses grupos. Domínios não fornecer isolamento de dados ou isolamento de serviço.  
  


