---
title: Planejar contas de usuário para o ambiente do MultiPoint Services
description: Informações de planejamento para contas de usuário nos serviços do MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 28ee7a1475ec55352fe344842b8df7633abb9137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853389"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planejar contas de usuário para o ambiente do MultiPoint Services
A melhor maneira de implementar contas de usuário nos serviços do MultiPoint depende do tamanho e da complexidade de sua implantação:  
  
-   **Contas de usuário local** -para uma pequena implantação com apenas alguns computadores que executam os serviços MultiPoind e poucos usuários, talvez seja mais conveniente usar *contas de usuário locais* que são criadas nos serviços do MultiPoint. Você pode criar uma conta individual para cada pessoa que usará o sistema ou criará uma conta genérica para cada estação, que pode ser usada por qualquer pessoa para fazer logon. Os administradores de serviços do MultiPoint criam e gerenciam contas de usuário local usando o MultiPoint Manager. As contas locais podem ser administradores, ter direitos administrativos limitados ou ser usuários comuns sem acesso ao MultiPoint Services Desktop ou ao MultiPoint Manager.  
  
-   **Contas de domínio** -se o seu ambiente tiver muitos computadores executando os serviços do MultiPoint e muitos usuários, você provavelmente achará mais útil configurar um Active Directory Domain Services \(AD DS domínio\) e usar *contas de usuário de domínio*, que permitem que um usuário acesse seu próprio perfil de usuário e configurações de qualquer estação no domínio. As contas de usuário de domínio devem ser criadas no controlador de domínio por um administrador de domínio.  
  
> [!NOTE]  
> As seções a seguir discutem cenários que você pode implementar para contas de usuário local nos serviços do MultiPoint. Se você estiver usando contas de usuário de domínio, consulte o cenário "um ou mais servidores MultiPoint em um ambiente de rede de domínio" em [cenários de exemplo: contas de usuário dos serviços do MultiPoint](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planejando contas de usuário local  
As seções a seguir consideram as vantagens, as desvantagens e os requisitos de várias maneiras de implementar contas de usuário locais individuais ou compartilhadas em seu ambiente de serviços do Windows MultiPoint.  
  
### <a name="use-individual-local-user-accounts"></a>Usar contas de usuário locais individuais  
Ao criar contas de usuário local, você tem a opção duas abordagens.  Atribua cada usuário a um servidor específico executando os serviços do MultiPoint e crie uma única conta para cada usuário. Ou crie contas de usuário local para todos os usuários em todos os computadores que executam os serviços do MultiPoint. Uma vantagem importante da implementação de contas de usuário individuais é que cada usuário tem sua própria experiência de área de trabalho do Windows que inclui pastas privadas para armazenar dados. 
  
Do ponto de vista do gerenciamento do sistema, a atribuição de usuários a um computador de serviços do MultiPoint específico pode ser mais conveniente. Por exemplo, se você tiver dois servidores MultiPoint com cinco estações cada, você poderá criar contas de usuário local, conforme ilustrado na tabela a seguir.  
  
**Tabela 1: atribuindo contas de usuário local a computadores específicos que executam os serviços do MultiPoint**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
Nesse cenário, cada usuário tem uma única conta em um computador específico. Portanto, todas as pessoas que têm uma conta local no computador A podem fazer logon em sua conta de qualquer estação associada ao computador A. No entanto, esses usuários não poderão acessar suas contas se usarem uma estação associada ao computador B e vice-versa. Uma vantagem dessa abordagem é que, ao sempre se conectar ao mesmo computador, os usuários sempre podem encontrar e acessar seus arquivos.  
  
Por outro lado, também é possível replicar contas de usuário individuais em todos os computadores que executam os serviços do MultiPoint, conforme ilustrado na tabela a seguir.  
  
**Tabela 2: replicando contas de usuário em todos os computadores que executam os serviços do MultiPoint**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Uma vantagem dessa abordagem é que os usuários têm uma conta de usuário local em todos os serviços do MultiPoint disponíveis. No entanto, as desvantagens podem superar essa vantagem. Por exemplo, mesmo que o nome de usuário e a senha de uma pessoa específica sejam os mesmos em ambos os computadores, as contas não são vinculadas entre si. Portanto, se um usuário fizer logon em sua conta no computador A na segunda-feira, salvar um arquivo e, em seguida, fizer logon em sua conta no computador B na terça-feira, ele não poderá acessar o arquivo salvo anteriormente no computador A. Além disso, a replicação de contas de usuário em vários computadores aumenta a sobrecarga administrativa e os requisitos de armazenamento.  
  
### <a name="use-generic-local-user-accounts"></a>Usar contas de usuário local genéricas  
Se o sistema de serviços do MultiPoint não estiver conectado a um domínio e você não quiser criar uma conta individual para cada usuário, poderá criar contas genéricas para cada estação. Por exemplo, se você tiver dois computadores executando os serviços do MultiPoint e cinco estações estiverem associadas a cada computador, você poderá decidir criar contas de usuário semelhantes às mostradas na tabela a seguir.  
  
**Tabela 3: Criando contas de usuário genéricas, uma conta por estação**  
  
|Computador A|Computador B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
Nesse cenário, cada conta de estação tem a mesma senha e as senhas e os nomes de conta de usuário genérico estão disponíveis para todos os usuários. Uma vantagem dessa abordagem é que a sobrecarga do gerenciamento de contas de usuário provavelmente será menor que se estiver usando contas individuais, porque normalmente há menos estações do que os usuários. Além disso, a sobrecarga causada pela replicação de contas de usuário em cada servidor é eliminada.  
  
Outra opção é criar contas genéricas em cada servidor. Cada usuário faz logon em um servidor como a mesma conta. Para permitir isso, você deve habilitar várias sessões por conta. Você pode simplificar ainda mais usando o mesmo nome de conta e senha em todos os servidores. Isso simplifica o logon para os usuários, que precisam saber apenas um nome de conta e senha para usar qualquer estação em qualquer servidor. Deve-se observar que, nesse cenário, todos os usuários podem ver qualquer alteração que qualquer usuário faça. Por exemplo, se um arquivo for salvo na área de trabalho, todos os usuários poderão ver o arquivo.  
  
> [!IMPORTANT]  
> É importante entender que, quando os usuários compartilham uma conta de usuário, um por servidor ou um por estação, arquivos salvos no servidor – até mesmo arquivos salvos em meus documentos – não são privados. Qualquer usuário que fizer logon com a conta terá acesso a esses arquivos. Quando você usa uma conta por estação, se um usuário salvar arquivos em meus documentos em uma estação, o usuário não terá acesso a esses arquivos em uma estação diferente. O mesmo ocorre ao fazer logon em diferentes computadores de serviços do MultiPoint.  
  
Para permitir que os usuários acessem seus arquivos de qualquer estação, você pode usar um servidor de arquivos, criar um compartilhamento de arquivos para cada conta de usuário ou permitir que os usuários armazenem seus documentos pessoais em uma unidade flash USB ou outro dispositivo de armazenamento privado. Unidades flash USB individuais permitem que usuários individuais armazenem documentos particulares mesmo se estiverem compartilhando uma conta de usuário em um MultiPoint Services.