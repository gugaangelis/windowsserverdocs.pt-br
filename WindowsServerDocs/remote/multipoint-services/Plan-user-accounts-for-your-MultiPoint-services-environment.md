---
title: Planejar contas de usuário para o ambiente do MultiPoint Services
description: Informações de planejamento para contas de usuário do MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864167"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planejar contas de usuário para o ambiente do MultiPoint Services
A melhor maneira de implementar contas de usuário no MultiPoint Services depende do tamanho e da complexidade da sua implantação:  
  
-   **Contas de usuário locais** – para uma pequena implantação com apenas alguns computadores em execução MultiPoind serviços e alguns usuários, talvez seja mais conveniente usar *contas de usuário local* que são criados no MultiPoint Services. Você pode criar uma conta individual para cada pessoa que usar o sistema, ou criar uma conta genérica para cada estação, que qualquer pessoa pode usar para fazer logon. Os administradores de serviços do multiPoint criar e gerenciar contas de usuário local, usando o MultiPoint Manager. As contas locais podem ser administradores, tem direitos administrativos limitados ou ser usuários regulares sem acesso à área de trabalho do MultiPoint Services ou MultiPoint Manager.  
  
-   **Contas de domínio** -se seu ambiente tiver muitos computadores que executam o MultiPoint Services e muitos usuários, você provavelmente achará mais úteis para configurar um Active Directory Domain Services \(AD DS\) domínio e use *contas de usuário de domínio*, que permitem que um usuário acesse o próprio perfil de usuário e configurações de qualquer estação no domínio. Contas de usuário de domínio devem ser criadas no controlador de domínio por um administrador de domínio.  
  
> [!NOTE]  
> As seções a seguir discutem cenários que você pode implementar para contas de usuário local no MultiPoint Services. Se você estiver usando contas de usuário de domínio, consulte o cenário de "um ou mais MultiPoint servidores em um ambiente de rede de domínio" em [cenários de exemplo: Contas de usuário do multiPoint Services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planejamento de contas de usuário local  
As seções a seguir considere as vantagens, desvantagens e requisitos de várias maneiras de implementar contas de usuário locais individuais ou compartilhado em seu ambiente do Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Usar contas de usuário locais individuais  
Ao criar contas de usuário local, você tem as duas abordagens de opção.  Atribua a cada usuário a um determinado servidor executando o MultiPoint Services e criação de uma única conta para cada usuário. Ou criar contas de usuário local para todos os usuários em todos os computadores executando o Multipoint services. Uma vantagem importante da implementação de contas de usuário individuais é que cada usuário tenha experiência de área de trabalho dele e de Windows que inclui pastas privadas para armazenar dados. 
  
De uma perspectiva de gerenciamento do sistema, atribuindo usuários a um computador específico do MultiPoint Services pode ser mais conveniente. Por exemplo, se você tiver dois servidores com cinco estações do MultiPoint, você pode criar contas de usuário local, conforme ilustrado na tabela a seguir.  
  
**Tabela 1: Atribuir contas de usuário local para computadores específicos, executando o MultiPoint Services**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
Nesse cenário, cada usuário tem uma única conta em um computador específico. Portanto, qualquer pessoa que tenha uma conta local no computador um podem fazer logon na conta de seus de qualquer estação associada ao computador A. No entanto, esses usuários não podem acessar suas contas, se eles usarem uma estação associados ao computador B e vice-versa. Uma vantagem dessa abordagem é que, conectando-se sempre no mesmo computador, os usuários podem sempre localizar e acessar seus arquivos.  
  
Por outro lado, também é possível replicar contas de usuário individuais em todos os computadores executando o MultiPoint Services, conforme ilustrado na tabela a seguir.  
  
**Tabela 2: Replicar contas de usuário em todos os computadores executando o MultiPoint Services**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Uma vantagem dessa abordagem é que os usuários tenham uma conta de usuário local em todos os serviços do MultiPoint disponíveis. No entanto, as desvantagens podem superar essa vantagem. Por exemplo, mesmo se o nome de usuário e senha para uma determinada pessoa são os mesmos em ambos os computadores, as contas não estão vinculadas uns aos outros. Portanto, se um usuário faz logon em seu ou sua conta no computador A, na segunda-feira, salva um arquivo e, em seguida, faz logon para sua conta no computador B na terça-feira, ele ou ela não será capaz de acessar o arquivo gravado anteriormente no computador A. Além disso , contas de usuário em vários computadores de replicação aumenta os requisitos de armazenamento e a sobrecarga administrativos.  
  
### <a name="use-generic-local-user-accounts"></a>Usar contas de usuário local genérico  
Se seu sistema MultiPoint Services não está conectado a um domínio e você não deseja criar uma conta individual para cada usuário, você pode criar contas genéricas para cada estação. Por exemplo, se você tiver dois computadores executando o MultiPoint Services e cinco estações estão associadas a cada computador, você pode decidir criar contas de usuário semelhantes aos mostrados na tabela a seguir.  
  
**Tabela 3: Criar contas de usuário genérica, uma conta por estação**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|Computer_A Station_01|Computer_B Station_01|  
|Computer_A-Station_02|Computer_B Station_02|  
|Computer_A Station_03|Computer_B Station_03|  
|Computer_A Station_04|Computer_B Station_04|  
|Computer_A Station_05|Computer_B-Station_05|  
  
Nesse cenário, todas as contas de estação tem a mesma senha e as senhas e nomes de conta de usuário genérica estão disponíveis para todos os usuários. Uma vantagem dessa abordagem é que a sobrecarga de gerenciamento de contas de usuário é provavelmente será menor que, se estiver usando contas individuais, pois há menos estações que os usuários normalmente. Além disso, a sobrecarga causada pela replicação de contas de usuário em todos os servidores é eliminada.  
  
Outra opção é criar contas genéricas em cada servidor. Cada usuário faz logon em um servidor, como a mesma conta. Para permitir isso, você deve habilitar várias sessões por conta. Você pode simplificar ainda mais usando o mesmo nome de conta e a senha em todos os servidores. Isso simplifica o logon para os usuários, que só precisa saber um nome de conta e senha para usar qualquer estação em qualquer servidor. Deve-se observar que, nesse cenário todos os usuários podem ver qualquer alteração que faz com que qualquer usuário. Por exemplo, se um arquivo é salvo na área de trabalho, todos os usuários podem ver o arquivo.  
  
> [!IMPORTANT]  
> É importante entender que, quando os usuários compartilham uma conta de usuário, um por servidor ou um por estação, arquivos salvos no servidor – até mesmo arquivos salvos em Meus documentos - não são privados. Qualquer usuário que fizer logon com a conta tem acesso a esses arquivos. Quando você usa uma conta por estação, se um usuário salva arquivos para Meus documentos em uma estação, o usuário não tem acesso a esses arquivos em uma estação diferente. O mesmo ocorre ao fazer logon em computadores diferentes do MultiPoint Services.  
  
Para permitir aos usuários acessar seus arquivos em qualquer estação, você pode usar um servidor de arquivos, criar um compartilhamento de arquivo para cada conta de usuário ou permitem que os usuários armazenam seus documentos pessoais em uma unidade flash USB ou outro dispositivo de armazenamento privado. As unidades flash USB individuais permitem que usuários individuais armazenar documentos particulares, mesmo se eles estão compartilhando uma conta de usuário em um MultiPoint Services.