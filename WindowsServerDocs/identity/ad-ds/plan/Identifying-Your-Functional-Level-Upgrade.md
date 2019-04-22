---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificar a sua atualização de nível funcional
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812037"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificar a sua atualização de nível funcional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de aumentar os níveis funcionais de domínio e floresta, você precisa avaliar seu ambiente atual e identificar o requisito de nível funcional que melhor atenda às necessidades da sua organização. Avaliar seu ambiente atual, identificando os domínios na sua floresta, controladores de domínio que estão localizados em cada domínio, o sistema operacional e service packs com cada controlador de domínio está em execução e a data em que você pretende atualizar o domínio controladores. Se você planeja desativar um controlador de domínio, certifique-se de que você entender o impacto total que isso terá no seu ambiente.  
  
As seguintes circunstâncias podem impedir que você atualizar uma versão anterior do sistema operacional Windows Server para o nível funcional do Windows Server 2008 ou Windows Server 2008 R2:  
  
-   Hardware insuficiente  
  
-   Um controlador de domínio executando um programa antivírus que é incompatível com o Windows Server 2008 ou Windows Server 2008 R2   
  
-   Uso de um programa específico da versão que não é executado no Windows Server 2008 ou Windows Server 2008 R2   
  
-   A necessidade de atualizar um programa com o service pack mais recente  
  
Documentar essas informações pode ajudá-lo a identificar as etapas necessárias para garantir que você tenha um ambiente totalmente funcional do Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois de avaliar seu ambiente atual, você precisa identificar a atualização do nível funcional que se aplica à sua organização. Essas opções estão disponíveis:  
  
-   Ambiente de modo nativo do Windows 2000 para o Windows Server 2008 ou Windows Server 2008 R2   
  
-   Floresta do Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2   
  
-   Nova floresta do Windows Server 2008  
  
-   Nova floresta do Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Atualizando os níveis funcionais em uma floresta do Active Directory do Windows 2000 nativo  
Em um ambiente de Windows 2000 nativo que consiste somente em controladores de domínio baseados no Windows 2000, os níveis funcionais são configurados por padrão para os seguintes níveis e permanecem nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Nível funcional de floresta do Windows 2000  
  
Para usar todos os recursos de nível de floresta e domínio do Windows Server 2008 ou Windows Server 2008 R2, você precisa atualizar este ambiente do Windows 2000 para o Windows Server 2008 ou Windows Server 2008 R2. Você pode executar essa atualização em qualquer uma das seguintes maneiras:  
  
-   Introduzir recém-instalada do Windows Server 2008-com base ou o Windows Server 2008 R2-com base em controladores de domínio na floresta e, em seguida, desativa todos os controladores de domínio que executam o Windows 2000.  
  
-   Realize uma atualização in-loco de todos os controladores de domínio existentes que executam o Windows 2000 na floresta para controladores de domínio executando o Windows Server 2003. Em seguida, execute uma atualização in-loco desses controladores de domínio para o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte [atualizando domínios do Active Directory para domínios do Windows Server 2008 AD DS \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 é um sistema de operacional baseado em x64. Se seu servidor estiver executando uma versão baseada em x64 do Windows Server 2003, você pode executar com êxito uma atualização in-loco do sistema de operacional deste computador para o Windows Server 2008 R2. Se seu servidor estiver executando uma versão baseada em x86 do Windows Server 2003, você não pode atualizar este computador para o Windows Server 2008 R2.  
  
Para usar os recursos de nível de domínio do Windows Server 2008 ou Windows Server 2008 R2 sem atualizar sua floresta inteira do Windows 2000 para o Windows Server 2008 ou Windows Server 2008 R2, aumentar apenas o nível funcional do domínio para o Windows Server 2008 ou Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de elevar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados no Windows 2000 nesse domínio para o Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois de substituir todos os controladores de domínio baseados no Windows 2000 na floresta com controladores de domínio que executam o Windows Server 2008 ou Windows Server 2008 R2, você pode aumentar o nível funcional de floresta para Windows Server 2008 ou Windows Server 2008 R2. Fazendo isso automaticamente aumenta o nível funcional de todos os domínios na floresta que são definidas para o Windows 2000 nativo ou posterior para Windows Server 2008 ou Windows Server 2008 R2.  
  
Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para os procedimentos para executar essas tarefas, consulte [implantação de um domínio raiz de floresta do Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Atualizando os níveis funcionais em uma floresta do Active Directory do Windows Server 2003  
Em um ambiente do Windows Server 2003 que consiste somente controladores de domínio baseados no Windows Server 2003, os níveis funcionais são configurados por padrão para os seguintes níveis e permanecem nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Nível funcional de floresta do Windows 2000  
  
Para usar todos os recursos de nível de floresta e domínio do Windows Server 2008 ou Windows Server 2008 R2, você precisa atualizar este ambiente do Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2. Você pode executar essa atualização em qualquer uma das seguintes maneiras:  
  
-   Introduzir um Windows Server 2008 recém-instalado-com base ou o Windows Server 2008 R2-com base em controlador de domínio na floresta e, em seguida, desativa todos os controladores de domínio executando o Windows Server 2003 ou atualizá-los para o Windows Server 2008 ou Windows Server 2008 R2.  
  
-   Realize uma atualização in-loco de todos os controladores de domínio existente executando o Windows Server 2003 para controladores de domínio que executam o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte [atualizando domínios do Active Directory para domínios do Windows Server 2008 AD DS \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 é um sistema de operacional baseado em x64. Se seu servidor estiver executando uma versão baseada em x64 do Windows Server 2003, você pode executar com êxito uma atualização in-loco do sistema de operacional deste computador para o Windows Server 2008 R2. Se seu servidor estiver executando uma versão baseada em x86 do Windows Server 2003, é possível atualizar este computador para executar o Windows Server 2008 R2.  
  
Para usar recursos de nível de domínio de todos os Windows Server 2008 ou Windows Server 2008 R2 sem atualizar sua floresta inteira do Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2, gerar apenas o nível funcional do domínio para Windows Server 2008 ou Windows S ervidor 2008 R2.  
  
> [!NOTE]  
> Antes de elevar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados no Windows Server 2003 no domínio para o Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois de atualizar todos os controladores de domínio baseados no Windows Server 2003 na floresta para Windows Server 2008 ou Windows Server 2008 R2, você pode aumentar o nível funcional de floresta para Windows Server 2008 ou Windows Server 2008 R2. Fazendo isso automaticamente aumenta o nível funcional de todos os domínios na floresta que são definidas para o Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2.  
  
Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para os procedimentos para executar essas tarefas, consulte [implantação de um domínio raiz de floresta do Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Atualizando os níveis funcionais em uma nova floresta do Windows Server 2008  
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008, os níveis funcionais são definidos por padrão para os seguintes níveis e permanecem nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Nível funcional de floresta do Windows 2000  
  
Níveis funcionais são definidos nesses níveis padrão lhe oferece a opção de adicionar controladores de domínio baseados no Windows Server 2003 ou Windows 2000 para sua nova floresta do Windows Server 2008. Depois de criar um domínio raiz da floresta, o nível funcional de domínio para cada domínio que você adicionar à floresta do Windows Server 2008 é definido como Windows 2000 nativo. No entanto, se desejar que todos os controladores de domínio em seu novo ambiente do Windows Server 2008 para executar o Windows Server 2008, defina o nível funcional da floresta e, em seguida, o nível funcional de domínio, ao Windows Server 2008 quando você instala o primeiro controlador de domínio em seu fores t. Isso economiza tempo e permite que todos os recursos de nível de floresta e domínio do Windows Server 2008.  
  
> [!IMPORTANT]  
> Se a floresta opera no Windows Server 2008 funcional nível e você tentar instalar o Active Directory em um servidor membro com base no Windows Server 2003 ou um servidor de membro baseado no Windows 2000, a instalação falhará.  
  
Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para os procedimentos para executar essas tarefas, consulte [implantação de um domínio raiz de floresta do Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Atualizando os níveis funcionais em uma nova floresta do Windows Server 2008 R2  
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008 R2, os níveis funcionais são definidos por padrão para os seguintes níveis e permanecem nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio do Windows Server 2003  
  
-   Nível funcional de floresta do Windows Server 2003  
  
Níveis funcionais são definidos nesses níveis padrão lhe oferece a opção de adicionar controladores de domínio baseados no Windows Server 2003 para sua nova floresta do Windows Server 2008 R2. Depois de criar um domínio raiz da floresta, o nível funcional de domínio para cada domínio que você adicionar à floresta do Windows Server 2008 R2 é definido para o Windows Server 2003. No entanto, se desejar que todos os controladores de domínio em seu novo ambiente do Windows Server 2008 R2 para executar o Windows Server 2008 R2, defina o nível funcional da floresta e, em seguida, o nível funcional de domínio, ao Windows Server 2008 R2, quando você instala o primeiro controlador de domínio em yo sua floresta. Isso economiza tempo e permite que todos os recursos de nível de floresta e domínio do Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Se a floresta opera no nível funcional do Windows Server 2008 R2 e você tentar instalar o Active Directory no Windows Server 2008-servidor membro com base ou com base no Windows Server 2003, ou em um servidor membro com base no Windows 2000, a instalação falhará.  
  
Para obter mais informações sobre como aumentar os níveis funcionais de floresta e domínio e para os procedimentos para executar essas tarefas, consulte [implantação de um domínio raiz de floresta do Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Embora o ADMT v3.1 deve ser instalado no Windows Server 2008, você pode usar o ADMT v3.1 para migrar objetos para um domínio que é hospedado por um ou mais controladores de domínio do Windows Server 2008 R2. Para obter mais informações, consulte [976659 do artigo](https://go.microsoft.com/fwlink/?LinkId=180398) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


