---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: "Identificando sua atualização nível funcional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9527a186cb20c470e0b5644fff58f90786520f97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificando sua atualização nível funcional

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de você pode emitir níveis funcionais de domínio e da floresta, você precisa avaliar seu ambiente atual e identificar o requisito de nível funcional que melhor atenda às necessidades da sua organização. Avalie seu ambiente atual, identificando os domínios na floresta, os controladores de domínio que estão localizados em cada domínio, o sistema operacional e os service packs que cada controlador de domínio está em execução e a data em que você pretende atualizar os controladores de domínio. Se você planeja desativar um controlador de domínio, certifique-se de que você entender o impacto completo que isso terá sobre seu ambiente.  
  
As seguintes circunstâncias podem impedir que você atualizar uma versão anterior do sistema operacional Windows Server para o nível funcional do Windows Server 2008 ou Windows Server 2008 R2:  
  
-   Hardware insuficiente  
  
-   Um controlador de domínio que executa um programa antivírus que é incompatível com o Windows Server 2008 ou Windows Server 2008 R2   
  
-   Uso de um programa específico da versão que não é executado no Windows Server 2008 ou Windows Server 2008 R2   
  
-   A necessidade de atualizar um programa com o service pack mais recente  
  
Documentar essas informações pode ajudá-lo a identificar as etapas devem ser executadas para garantir que você tenha um ambiente totalmente funcional do Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois de avaliar seu ambiente atual, você precisa identificar a atualização de nível funcional que se aplica a sua organização. Essas opções estão disponíveis:  
  
-   Ambiente de modo nativo do Windows 2000 para Windows Server 2008 ou Windows Server 2008 R2   
  
-   Floresta de Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2   
  
-   Nova floresta do Windows Server 2008  
  
-   Nova floresta do Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Atualizando níveis funcionais em uma floresta do Active Directory do Windows 2000 nativo  
Em um ambiente de nativo do Windows 2000 que consiste apenas em controladores de domínio baseados em Windows 2000, os níveis funcionais são definidos por padrão para os seguintes níveis e permaneçam nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Windows 2000 nível funcional da floresta  
  
Para usar todos os recursos de nível de floresta e nível de domínio do Windows Server 2008 ou Windows Server 2008 R2, você precisa atualizar esse ambiente do Windows 2000 para o Windows Server 2008 ou Windows Server 2008 R2. Você pode executar essa atualização em qualquer uma das seguintes maneiras:  
  
-   Introduzir o Windows Server 2008 recém-instalado-com base em ou Windows Server 2008 R2-com base em controladores de domínio na floresta e, em seguida, desativar todos os controladores de domínio que executam o Windows 2000.  
  
-   Realizar uma atualização in-loco de todos os controladores de domínio existentes que executam o Windows 2000 na floresta para controladores de domínio que executam o Windows Server 2003. Em seguida, execute uma atualização in-loco do Windows Server 2008 ou Windows Server 2008 R2 esses controladores de domínio. Para obter mais informações, consulte [atualizando domínios do Active Directory para Windows Server 2008 AD DS domínios \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 é um sistema operacional baseado em x64. Se o servidor está executando uma versão com base em x64 do Windows Server 2003, você pode executar com êxito uma atualização in-loco do sistema operacional do computador Windows Server 2008 R2. Se o servidor está executando uma versão baseada em x86 do Windows Server 2003, você não pode atualizar este computador para o Windows Server 2008 R2.  
  
Para usar os recursos de nível de domínio do Windows Server 2008 ou Windows Server 2008 R2 sem atualizar toda a floresta Windows 2000 para Windows Server 2008 ou Windows Server 2008 R2, gere somente o nível funcional do domínio para o Windows Server 2008 ou Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de você aumentar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados em Windows 2000 no domínio para Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois que você substituir todos os controladores de domínio baseados em Windows 2000 na floresta com controladores de domínio que executam o Windows Server 2008 ou Windows Server 2008 R2, você pode aumentar o nível funcional da floresta para o Windows Server 2008 ou Windows Server 2008 R2. Fazendo assim automaticamente eleva o nível funcional de todos os domínios na floresta que são definidos para o Windows 2000 nativo ou posterior para Windows Server 2008 ou Windows Server 2008 R2.  
  
Para obter mais informações sobre como aumentar níveis funcionais de floresta e domínio e para obter os procedimentos executar essas tarefas, consulte [Implantando um Windows Server 2008 floresta raiz domínio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Atualizando níveis funcionais em uma floresta do Active Directory do Windows Server 2003  
Em um ambiente do Windows Server 2003 que consiste em apenas os controladores de domínio baseados no Windows Server 2003, os níveis funcionais são definidos por padrão para os seguintes níveis e permaneçam nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Windows 2000 nível funcional da floresta  
  
Para usar todos os recursos de nível de floresta e nível de domínio do Windows Server 2008 ou Windows Server 2008 R2, você precisa atualizar esse ambiente Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2. Você pode executar essa atualização em qualquer uma das seguintes maneiras:  
  
-   Introduzir um Windows Server 2008 recém-instalado-com base em ou Windows Server 2008 R2-com base em controlador de domínio na floresta e eliminar todos os controladores de domínio que executam o Windows Server 2003 ou atualizá-los para o Windows Server 2008 ou Windows Server 2008 R2.  
  
-   Realizar uma atualização in-loco de todos os controladores de domínio existentes que executam o Windows Server 2003 para controladores de domínio que executam o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte [atualizando domínios do Active Directory para Windows Server 2008 AD DS domínios \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 é um sistema operacional baseado em x64. Se o servidor está executando uma versão com base em x64 do Windows Server 2003, você pode executar com êxito uma atualização in-loco do sistema operacional do computador Windows Server 2008 R2. Se o servidor está executando uma versão baseada em x86 do Windows Server 2003, você não pode atualizar este computador para executar o Windows Server 2008 R2.  
  
Para usar recursos no nível do domínio todos os Windows Server 2008 ou Windows Server 2008 R2 sem atualizar toda a floresta do Windows Server 2003 para o Windows Server 2008 ou Windows Server 2008 R2, gere somente o nível funcional do domínio para o Windows Server 2008 ou Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de você aumentar o nível funcional do domínio, você deve atualizar todos os controladores de domínio baseados no Windows Server 2003 no domínio para Windows Server 2008 ou Windows Server 2008 R2.  
  
Depois de atualizar todos os controladores de domínio baseados no Windows Server 2003 na floresta para o Windows Server 2008 ou Windows Server 2008 R2, você pode aumentar o nível funcional da floresta para o Windows Server 2008 ou Windows Server 2008 R2. Fazendo assim automaticamente eleva o nível funcional de todos os domínios na floresta que são definidos para o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2.  
  
Para obter mais informações sobre como aumentar níveis funcionais de floresta e domínio e para obter os procedimentos executar essas tarefas, consulte [Implantando um Windows Server 2008 floresta raiz domínio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Atualizando níveis funcionais em uma nova floresta do Windows Server 2008  
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008, níveis funcionais são definidos por padrão para os seguintes níveis e permaneçam nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio nativo do Windows 2000  
  
-   Windows 2000 nível funcional da floresta  
  
Níveis funcionais são definidos nesses níveis padrão para dar a você a opção de adicionar controladores de domínio baseados no Windows Server 2003 ou Windows 2000 para sua nova floresta do Windows Server 2008. Depois de criar um domínio raiz da floresta, o nível funcional de domínio para cada domínio que você adicionar à floresta do Windows Server 2008 é definido para o Windows 2000 nativo. No entanto, se você quiser que todos os controladores de domínio no novo ambiente Windows Server 2008 para executar o Windows Server 2008, defina o nível funcional da floresta e, em seguida, o nível funcional de domínio, como quando você instala o primeiro controlador de domínio na floresta do Windows Server 2008. Isso economiza tempo e permite que todos os recursos de nível de floresta e nível de domínio no Windows Server 2008.  
  
> [!IMPORTANT]  
> Se a floresta opera no Windows Server 2008 funcional nível e você tenta instalar o Active Directory em um servidor membro baseado no Windows Server 2003 ou um servidor de membro baseado no Windows 2000, a instalação falha.  
  
Para obter mais informações sobre como aumentar níveis funcionais de floresta e domínio e para obter os procedimentos executar essas tarefas, consulte [Implantando um Windows Server 2008 floresta raiz domínio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Atualizando níveis funcionais em uma nova floresta do Windows Server 2008 R2  
Quando você instala o primeiro controlador de domínio em uma nova floresta do Windows Server 2008 R2, níveis funcionais são definidos por padrão para os seguintes níveis e permaneçam nesses níveis até gerá-los manualmente:  
  
-   Nível funcional de domínio do Windows Server 2003  
  
-   Windows Server 2003 nível funcional da floresta  
  
Níveis funcionais são definidos nesses níveis padrão para dar a você a opção de adicionar controladores de domínio baseados no Windows Server 2003 para sua nova floresta do Windows Server 2008 R2. Depois de criar um domínio raiz da floresta, o nível funcional de domínio para cada domínio que você adicionar à floresta do Windows Server 2008 R2 é definido para o Windows Server 2003. No entanto, se você quiser que todos os controladores de domínio no novo ambiente Windows Server 2008 R2 para executar o Windows Server 2008 R2, defina o nível funcional da floresta e, em seguida, o nível funcional de domínio, ao Windows Server 2008 R2 quando você instala o primeiro controlador de domínio na floresta. Isso economiza tempo e permite que todos os recursos de nível de floresta e nível de domínio no Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Se a floresta opera no nível funcional do Windows Server 2008 R2 e você tentar instalar o Active Directory no Windows Server 2008-servidor membro baseado no Windows Server 2003 ou com base, ou em um servidor membro baseado no Windows 2000, a instalação falhar.  
  
Para obter mais informações sobre como aumentar níveis funcionais de floresta e domínio e para obter os procedimentos executar essas tarefas, consulte [Implantando um Windows Server 2008 floresta raiz domínio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Embora o ADMT v 3.1 deve ser instalado no Windows Server 2008, você pode usar o ADMT v 3.1 para migrar os objetos em um domínio que está hospedado por um ou mais controladores de domínio do Windows Server 2008 R2. Para obter mais informações, consulte [976659 do artigo](https://go.microsoft.com/fwlink/?LinkId=180398) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


