---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificar a sua atualização de nível funcional
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7036a3be6a504f91cc6545484d7cd4f0dbd485ef
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943020"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificar a sua atualização de nível funcional

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de poder aumentar os níveis funcionais de domínio e floresta, você precisa avaliar seu ambiente atual e identificar o requisito de nível funcional que melhor atende às necessidades de sua organização. Avalie seu ambiente atual identificando os domínios em sua floresta, os controladores de domínio localizados em cada domínio, o sistema operacional e os Service Packs que cada controlador de domínio está executando e a data em que você planeja atualizar os controladores de domínio. Se você planeja desativar um controlador de domínio, certifique-se de entender o impacto total que isso terá em seu ambiente.

As seguintes circunstâncias podem impedi-lo de atualizar uma versão anterior do sistema operacional Windows Server para o nível funcional do Windows Server 2008 ou do Windows Server 2008 R2:

- Hardware insuficiente

- Um controlador de domínio que executa um programa antivírus que é incompatível com o Windows Server 2008 ou o Windows Server 2008 R2

- Uso de um programa específico da versão que não é executado no Windows Server 2008 ou no Windows Server 2008 R2

- A necessidade de atualizar um programa com as service pack mais recentes

Documentar essas informações pode ajudá-lo a identificar as etapas a serem tomadas para garantir que você tenha um ambiente totalmente funcional do Windows Server 2008 ou do Windows Server 2008 R2.

Depois de avaliar seu ambiente atual, você precisa identificar a atualização de nível funcional que se aplica à sua organização. Essas opções estão disponíveis:

- Ambiente de modo nativo do Windows 2000 para Windows Server 2008 ou Windows Server 2008 R2

- Floresta do Windows Server 2003 para Windows Server 2008 ou Windows Server 2008 R2

- Nova floresta do Windows Server 2008

- Nova floresta do Windows Server 2008 R2

## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Atualizando os níveis funcionais em uma floresta nativa Active Directory do Windows 2000
Em um ambiente nativo do Windows 2000 que consiste apenas em controladores de domínio baseados no Windows 2000, os níveis funcionais são definidos por padrão para os seguintes níveis e permanecem nesses níveis até que você os gere manualmente:

- Nível funcional de domínio nativo do Windows 2000

- Nível funcional de floresta do Windows 2000

Para usar todos os recursos de nível de floresta e domínio no Windows Server 2008 ou no Windows Server 2008 R2, você precisa atualizar esse ambiente do Windows 2000 para o Windows Server 2008 ou o Windows Server 2008 R2. Você pode executar essa atualização de uma das seguintes maneiras:

- Apresente controladores de domínio baseados no Windows Server 2008 ou Windows Server 2008 R2 instalados recentemente na floresta e, em seguida, desative todos os controladores de domínio que executam o Windows 2000.

- Execute uma atualização in-loco de todos os controladores de domínio existentes que executam o Windows 2000 na floresta para controladores de domínio que executam o Windows Server 2003. Em seguida, execute uma atualização in-loco desses controladores de domínio para o Windows Server 2008 ou o Windows Server 2008 R2. Para obter mais informações, consulte [atualizando domínios de Active Directory para os domínios do Windows server 2008 e do Windows server 2008 R2 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)).

    > [!IMPORTANT]
    >  O Windows Server 2008 R2 é um sistema operacional baseado em x64. Se o servidor estiver executando uma versão baseada em x64 do Windows Server 2003, você poderá executar com êxito uma atualização in-loco do sistema operacional deste computador para o Windows Server 2008 R2. Se o servidor estiver executando uma versão baseada em x86 do Windows Server 2003, você não poderá atualizar este computador para o Windows Server 2008 R2.

Para usar os recursos de nível de domínio do Windows Server 2008 ou do Windows Server 2008 R2 sem Atualizar toda a floresta do Windows 2000 para o Windows Server 2008 ou Windows Server 2008 R2, aumente apenas o nível funcional do domínio para o Windows Server 2008 ou o Windows Server 2008 R2.

> [!NOTE]
> Antes de aumentar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados no Windows 2000 nesse domínio para o Windows Server 2008 ou o Windows Server 2008 R2.

Depois de substituir todos os controladores de domínio baseados no Windows 2000 na floresta por controladores de domínio que executam o Windows Server 2008 ou o Windows Server 2008 R2, você pode aumentar o nível funcional da floresta para o Windows Server 2008 ou o Windows Server 2008 R2. Fazer isso gera automaticamente o nível funcional de todos os domínios na floresta que são definidos como Windows 2000 nativo ou superior para o Windows Server 2008 ou Windows Server 2008 R2.

Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para procedimentos para executar essas tarefas, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Atualizando os níveis funcionais em uma floresta Active Directory do Windows Server 2003
Em um ambiente do Windows Server 2003 que consiste apenas em controladores de domínio baseados no Windows Server 2003, os níveis funcionais são definidos por padrão para os seguintes níveis e permanecem nesses níveis até que você os gere manualmente:

- Nível funcional de domínio nativo do Windows 2000

- Nível funcional de floresta do Windows 2000

Para usar todos os recursos de nível de floresta e domínio no Windows Server 2008 ou no Windows Server 2008 R2, você precisa atualizar esse ambiente do Windows Server 2003 para o Windows Server 2008 ou o Windows Server 2008 R2. Você pode executar essa atualização de uma das seguintes maneiras:

- Introduza um controlador de domínio baseado no Windows Server 2008 ou Windows Server 2008 R2 instalado recentemente na floresta e, em seguida, desative todos os controladores de domínio que executam o Windows Server 2003 ou atualize-os para o Windows Server 2008 ou o Windows Server 2008 R2.

- Execute uma atualização in-loco de todos os controladores de domínio existentes que executam o Windows Server 2003 em controladores de domínio que executam o Windows Server 2008 ou o Windows Server 2008 R2. Para obter mais informações, consulte [atualizando domínios de Active Directory para os domínios do Windows server 2008 e do Windows server 2008 R2 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)).

> [!IMPORTANT]
> O Windows Server 2008 R2 é um sistema operacional baseado em x64. Se o servidor estiver executando uma versão baseada em x64 do Windows Server 2003, você poderá executar com êxito uma atualização in-loco do sistema operacional deste computador para o Windows Server 2008 R2. Se o servidor estiver executando uma versão baseada em x86 do Windows Server 2003, você não poderá atualizar este computador para executar o Windows Server 2008 R2.

Para usar todos os recursos de nível de domínio do Windows Server 2008 ou do Windows Server 2008 R2 sem Atualizar toda a floresta do Windows Server 2003 para o Windows Server 2008 ou o Windows Server 2008 R2, aumente apenas o nível funcional do domínio para o Windows Server 2008 ou o Windows Server 2008 R2.

> [!NOTE]
> Antes de aumentar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados no Windows Server 2003 nesse domínio para o Windows Server 2008 ou o Windows Server 2008 R2.

Depois de atualizar todos os controladores de domínio baseados no Windows Server 2003 na floresta para o Windows Server 2008 ou o Windows Server 2008 R2, você pode aumentar o nível funcional da floresta para o Windows Server 2008 ou o Windows Server 2008 R2. Fazer isso gera automaticamente o nível funcional de todos os domínios na floresta que são definidos como Windows Server 2003 para Windows Server 2008 ou Windows Server 2008 R2.

Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para procedimentos para executar essas tarefas, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Atualizando os níveis funcionais em uma nova floresta do Windows Server 2008
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008, os níveis funcionais são definidos por padrão nos seguintes níveis e permanecem nesses níveis até que você os gere manualmente:

- Nível funcional de domínio nativo do Windows 2000

- Nível funcional de floresta do Windows 2000

Os níveis funcionais são definidos nesses níveis padrão para oferecer a opção de adicionar controladores de domínio baseados no Windows 2000 ou no Windows Server 2003 à sua nova floresta do Windows Server 2008. Depois de criar um domínio raiz de floresta, o nível funcional de domínio para cada domínio que você adiciona à floresta do Windows Server 2008 é definido como Windows 2000 nativo. No entanto, se você quiser que todos os controladores de domínio em seu novo ambiente do Windows Server 2008 executem o Windows Server 2008, defina o nível funcional da floresta e, em seguida, o nível funcional do domínio para o Windows Server 2008 ao instalar o primeiro controlador de domínio em sua floresta. Isso poupa tempo e habilita todos os recursos de nível de floresta e domínio no Windows Server 2008.

> [!IMPORTANT]
> Se a floresta operar no nível funcional do Windows Server 2008 e você tentar instalar o Active Directory em um servidor membro baseado no Windows Server 2003 ou em um servidor membro baseado no Windows 2000, a instalação falhará.

Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para procedimentos para executar essas tarefas, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Atualizando os níveis funcionais em uma nova floresta do Windows Server 2008 R2
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008 R2, os níveis funcionais são definidos por padrão para os seguintes níveis e permanecem nesses níveis até que você os gere manualmente:

- Nível funcional de domínio do Windows Server 2003

- Nível funcional de floresta do Windows Server 2003

Os níveis funcionais são definidos nesses níveis padrão para oferecer a opção de adicionar controladores de domínio baseados no Windows Server 2003 à sua nova floresta do Windows Server 2008 R2. Depois de criar um domínio raiz da floresta, o nível funcional do domínio para cada domínio que você adiciona à floresta do Windows Server 2008 R2 é definido como Windows Server 2003. No entanto, se você quiser que todos os controladores de domínio em seu novo ambiente do Windows Server 2008 R2 executem o Windows Server 2008 R2, defina o nível funcional da floresta e, em seguida, o nível funcional do domínio para o Windows Server 2008 R2 ao instalar o primeiro controlador de domínio em sua floresta. Isso poupa tempo e habilita todos os recursos de nível de floresta e domínio no Windows Server 2008 R2.

> [!IMPORTANT]
> Se a floresta operar no nível funcional do Windows Server 2008 R2 e você tentar instalar o Active Directory em um servidor membro baseado no Windows Server 2008 ou no Windows Server 2003, ou em um servidor membro baseado no Windows 2000, a instalação falhará.

Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para procedimentos para executar essas tarefas, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

> [!NOTE]
> Embora o ADMT v 3.1 deva ser instalado no Windows Server 2008, você pode usar o ADMT v 3.1 para migrar objetos para um domínio hospedado por um ou mais controladores de domínio do Windows Server 2008 R2. Para obter mais informações, consulte o artigo 976659 na base de dados de conhecimento Microsoft, [problemas conhecidos que podem ocorrer quando você usa o ADMT 3,1 para migrar para um domínio que contém controladores de domínio do Windows Server 2008 R2](https://support.microsoft.com/help/976659/).
