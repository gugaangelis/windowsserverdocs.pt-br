---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Examinando os modelos de domínio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b77f634d8548994d2f9e130faad9ca34aa226327
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821949"
---
# <a name="reviewing-the-domain-models"></a>Examinando os modelos de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os fatores a seguir afetam o modelo de design de domínio que você selecionar:  
  
- Quantidade de capacidade disponível em sua rede que você está disposto a alocar para Active Directory Domain Services (AD DS). O objetivo é selecionar um modelo que forneça replicação eficiente de informações com impacto mínimo sobre a largura de banda de rede disponível.  

- Número de usuários em sua organização. Se sua organização incluir um grande número de usuários, a implantação de mais de um domínio permitirá que você particione seus dados e oferecerá mais controle sobre a quantidade de tráfego de replicação que passará por uma determinada conexão de rede. Isso possibilita que você controle onde os dados são replicados e reduza a carga criada pelo tráfego de replicação em links lentos em sua rede.  

O design de domínio mais simples é um domínio único. Em um único design de domínio, todas as informações são replicadas para todos os controladores de domínio. Se necessário, no entanto, você pode implantar domínios regionais adicionais. Isso pode ocorrer se partes da infraestrutura de rede estiverem conectadas por links lentos e o proprietário da floresta quiser ter certeza de que o tráfego de replicação não excede a capacidade que foi alocada para AD DS.  

É melhor minimizar o número de domínios que você implanta em sua floresta. Isso reduz a complexidade geral da implantação e, como resultado, reduz o custo total de propriedade. A tabela a seguir lista os custos administrativos associados à adição de domínios regionais.  

|Custo|Envolvidas|  
|--------|----------------|  
|Gerenciamento de vários grupos de administradores de serviços|Cada domínio tem seus próprios grupos de administradores de serviços que precisam ser gerenciados de forma independente. A associação desses grupos de administradores de serviços deve ser cuidadosamente controlada.|  
|Manter a consistência entre Política de Grupo configurações comuns a vários domínios|Política de Grupo configurações que precisam ser aplicadas em toda a floresta devem ser aplicadas separadamente a cada domínio individual na floresta.|  
|Manter a consistência entre controle de acesso e configurações de auditoria comuns a vários domínios|As configurações de controle de acesso e auditoria que precisam ser aplicadas na floresta devem ser aplicadas separadamente a cada domínio individual na floresta.|  
|Maior probabilidade de movimentação de objetos entre domínios|Quanto maior o número de domínios, maior será a probabilidade de que os usuários precisem passar de um domínio para outro. Essa mudança pode afetar potencialmente os usuários finais.|  

> [!NOTE]  
> As políticas de bloqueio de conta e senha refinadas do Windows Server também podem afetar o modelo de design de domínio selecionado. Antes desta versão do Windows Server 2008, você pode aplicar apenas uma política de bloqueio de conta e senha, que é especificada na política de domínio padrão de domínio, para todos os usuários no domínio. Como resultado, se você quisesse configurações de bloqueio de senha e conta diferentes para diferentes conjuntos de usuários, era necessário criar um filtro de senha ou implantar vários domínios. Agora você pode usar políticas de senha refinadas para especificar várias políticas de senha e aplicar restrições de senha e políticas de bloqueio de conta diferentes a diferentes conjuntos de usuários em um único domínio. Para obter mais informações sobre políticas refinadas de bloqueio de senha e de conta, consulte o artigo [guia passo a passo para configuração de política de bloqueio de conta e senha](https://go.microsoft.com/fwlink/?LinkID=91477)refinada.  

## <a name="single-domain-model"></a>Modelo de domínio único

Um único modelo de domínio é o mais fácil de administrar e o menos caro a ser mantido. Ele consiste em uma floresta que contém um único domínio. Esse é o domínio raiz da floresta e contém todas as contas de usuário e grupo na floresta.  

Um modelo de floresta de domínio único reduz a complexidade administrativa fornecendo as seguintes vantagens:  

- Qualquer controlador de domínio pode autenticar qualquer usuário na floresta.  

- Todos os controladores de domínio podem ser catálogos globais, portanto, você não precisa planejar o posicionamento do servidor de catálogo global.  
  
Em uma floresta de domínio único, todos os dados de diretório são replicados para todos os locais geográficos que hospedam controladores de domínio. Embora esse modelo seja o mais fácil de gerenciar, ele também cria a maior parte do tráfego de replicação dos dois modelos de domínio. O particionamento do diretório em vários domínios limita a replicação de objetos a regiões geográficas específicas, mas resulta em mais sobrecarga administrativa.  
  
## <a name="regional-domain-model"></a>Modelo de domínio regional

Todos os dados de objeto em um domínio são replicados para todos os controladores de domínio nesse domínio. Por esse motivo, se sua floresta incluir um grande número de usuários que são distribuídos em diferentes locais geográficos conectados por uma WAN (rede de longa distância), talvez seja necessário implantar domínios regionais para reduzir o tráfego de replicação nos links WAN. Os domínios regionais baseados geograficamente podem ser organizados de acordo com a conectividade WAN da rede.  
  
O modelo de domínio regional permite que você mantenha um ambiente estável ao longo do tempo. Baseie as regiões usadas para definir domínios em seu modelo em elementos estáveis, como limites continental. Domínios com base em outros fatores, como grupos dentro da organização, podem ser alterados com frequência e podem exigir que você reestruture seu ambiente.  
  
O modelo de domínio regional consiste em um domínio raiz da floresta e em um ou mais domínios regionais. Criar um design de modelo de domínio regional envolve identificar qual domínio é o domínio raiz da floresta e determinar o número de domínios adicionais necessários para atender aos seus requisitos de replicação. Se sua organização incluir grupos que exigem isolamento de dados ou isolamento de serviço de outros grupos na organização, crie uma floresta separada para esses grupos. Os domínios não fornecem isolamento de dados ou isolamento de serviço.  
