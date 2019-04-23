---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisando os modelos de domínio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 23c1eb66eeecab8df63cbd7910a9398bc4e3c705
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870917"
---
# <a name="reviewing-the-domain-models"></a>Revisando os modelos de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os seguintes fatores afetam o modelo de design de domínio que você selecione:  
  
- Quantidade da capacidade disponível em sua rede que você está disposto a ser alocada para os serviços de domínio do Active Directory (AD DS). O objetivo é selecionar um modelo que fornece replicação eficiente de informações com impacto mínimo de largura de banda de rede disponível.  

- Número de usuários em sua organização. Se sua organização inclui um grande número de usuários, implantar mais de um domínio permite que você particione seus dados e dá a você mais controle sobre a quantidade de tráfego de replicação que passará por meio de uma conexão de rede fornecido. Isso torna possível para que você possa controlar onde os dados são replicados e reduzir a carga criada pelo tráfego de replicação em links lentos em sua rede.  

O design de domínio mais simples é um único domínio. Em um design de domínio único, todas as informações são replicadas para todos os controladores de domínio. No entanto, se necessário, você pode implantar domínios regionais adicionais. Isso pode ocorrer se as partes da infraestrutura de rede são conectadas por links lentos, e o proprietário da floresta quer ter certeza de que o tráfego de replicação não exceda a capacidade que foi alocada para o AD DS.  

É melhor minimizar o número de domínios que você implantar em sua floresta. Isso reduz a complexidade geral da implantação e, consequentemente, reduz o custo total de propriedade. A tabela a seguir lista os custos administrativos associados com a adição de domínios regionais.  

|Custo|Implicações|  
|--------|----------------|  
|Gerenciamento de vários grupos de administrador de serviço|Cada domínio tem seus próprios grupos de administradores de serviço que precisam ser gerenciados de forma independente. A associação desses grupos de administrador de serviço deve ser cuidadosamente controlada.|  
|Manter a consistência entre as configurações de diretiva de grupo que são comuns a vários domínios|Configurações de diretiva de grupo que precisam ser aplicadas em toda a floresta devem ser aplicadas separadamente para cada domínio individual na floresta.|  
|Manter a consistência entre o controle de acesso e as configurações que são comuns a vários domínios de auditoria|Controle de acesso e as configurações que precisam ser aplicadas em toda a floresta de auditoria devem ser aplicadas separadamente para cada domínio individual na floresta.|  
|Aumento da probabilidade de objetos movendo entre domínios|Quanto maior o número de domínios, quanto maior a probabilidade de que os usuários precisarão se mover de um domínio para outro. Essa mudança pode afetar os usuários finais.|  

> [!NOTE]  
> Diretivas de bloqueio de conta e senha refinada do Windows Server também podem afetar o modelo de design de domínio que você selecionar. Antes desta versão do Windows Server 2008, você pode aplicar apenas uma conta e senha diretiva de bloqueio, que é especificada no domínio de diretiva de domínio padrão, a todos os usuários no domínio. Como resultado, se você quisesse senha diferente e configurações de bloqueio de conta para diferentes conjuntos de usuários, você precisava criar um filtro de senha ou implantar vários domínios. Agora você pode usar políticas de senha refinada para especificar várias diretivas de senha e aplicar diretivas de bloqueio de conta e restrições de senha diferente para diferentes conjuntos de usuários em um único domínio. Para obter mais informações sobre as diretivas de bloqueio de conta e senha refinada, consulte o artigo [guia passo a passo para configuração de política de bloqueio de conta e de senha refinada](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modelo de domínio único

Um único modelo de domínio é o mais fácil de administrar e menos dispendioso para manter. Ele consiste em uma floresta que contém um único domínio. Esse domínio é o domínio raiz da floresta e contém todas as contas de usuário e grupo na floresta.  

Um modelo de floresta de domínio único reduz a complexidade administrativa, fornecendo as seguintes vantagens:  

- Qualquer controlador de domínio pode autenticar qualquer usuário na floresta.  

- Todos os controladores de domínio podem ser catálogos globais, portanto, você não precisará planejar o posicionamento do servidor de catálogo global.  
  
Em uma floresta de domínio único, todos os dados do diretório é replicada para todos os locais geográficos que hospedam controladores de domínio. Embora esse modelo é o mais fácil de gerenciar, ele também cria o tráfego de replicação mais dos modelos de domínio de dois. Particionando o diretório em vários domínios limita a replicação de objetos, mas resulta em mais sobrecarga administrativa de regiões geográficas específicas.  
  
## <a name="regional-domain-model"></a>Modelo de domínio regional

Todos os dados de objeto dentro de um domínio é replicada para todos os controladores de domínio nesse domínio. Por esse motivo, se sua floresta inclui um grande número de usuários que são distribuídos em diferentes locais geográficos conectados por uma rede de longa distância (WAN), você talvez precise implantar domínios regionais para reduzir o tráfego de replicação em links WAN. Domínios regionais geograficamente com base podem ser organizados de acordo com a conectividade de rede WAN.  
  
O modelo de domínio regional permite que você mantenha um ambiente estável ao longo do tempo. As regiões usadas para definir domínios em seu modelo em elementos estáveis, como limites continental de base. Domínios com base em outros fatores, como grupos da organização, podem ser alterados com frequência e podem exigir que você reestruturar seu ambiente.  
  
O modelo de domínio regional consiste em um domínio raiz da floresta e um ou mais domínios regionais. Criando um projeto de modelo de domínio regional envolve identificar qual domínio é o domínio raiz da floresta e determinar o número de domínios adicionais que são necessários para atender às suas necessidades de replicação. Se sua organização inclui os grupos que exigem isolamento de dados ou isolamento de serviço de outros grupos da organização, crie uma floresta separada para esses grupos. Domínios não fornecem isolamento de dados ou isolamento de serviço.  
