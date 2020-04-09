---
title: Considerações de rede e contas de usuário
description: Fornece informações de planejamento para diferentes cenários de rede e de usuário com os serviços do MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ed9770ff6e91e548dfc38a1de927646590a25165
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853409"
---
# <a name="network-considerations-and-user-accounts"></a>Considerações de rede e contas de usuário
Os serviços do MultiPoint podem ser implantados em uma variedade de ambientes de rede e podem dar suporte a contas de usuário local e contas de usuário de domínio. Em geral, as contas de usuário dos serviços do MultiPoint serão gerenciadas em um dos seguintes ambientes de rede:  
  
-   Um único computador executando os serviços do MultiPoint com contas de usuário locais  
  
-   Vários computadores que executam os serviços do MultiPoint, cada um com uma conta de usuário local  
  
-   Vários computadores que executam os serviços do MultiPoint e que estão usando contas de usuário de domínio

Por definição, *as contas de usuário local* só podem ser acessadas no computador em que foram criadas. As contas de usuário local são contas de usuário criadas em um computador específico que está executando os serviços do MultiPoint. Por outro lado, *as contas de usuário de domínio* são contas de usuário que residem em um controlador de domínio e podem ser acessadas de qualquer computador que esteja conectado ao domínio. Ao decidir qual tipo de ambiente de rede usar, considere o seguinte:  
  
-   Os recursos serão compartilhados entre servidores?  
  
-   Os usuários mudarão entre servidores?  
  
-   Os usuários acessarão servidores de banco de dados que exigem autenticação?  
  
-   Os usuários acessarão servidores Web internos que exigem autenticação?  
  
-   Existe uma infraestrutura de domínio Active Directory existente em vigor?  
  
-   Quem usará o console do MultiPoint Manager para gerenciar áreas de trabalho de usuários, exibir miniaturas, adicionar usuários, limitar sites e assim por diante? Essa pessoa estará gerenciando mais de um servidor? Essa pessoa deve ter privilégios administrativos nos servidores.  
  
As seções a seguir abordam o gerenciamento de contas de usuário nesses ambientes de rede.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Servidor MultiPoint único com contas de usuário local  
Em ambientes com um único computador que está executando os serviços do MultiPoint, não há nenhum requisito para ter uma rede. No entanto, para aproveitar os recursos da Internet, os requisitos de rede podem ser tão básicos quanto um roteador e uma conexão com um provedor de serviços de Internet (ISP). As conexões de rede associadas a um adaptador de rede nos serviços do MultiPoint são configuradas, por padrão, para obter um endereço IP e o endereço do servidor DNS automaticamente por meio do DHCP. Os roteadores da Internet normalmente são configurados como servidores DHCP e fornecem endereços IP privados a computadores que se conectam a eles na rede interna. Portanto, um único computador executando os serviços do MultiPoint pode ser capaz de se conectar à interface interna do roteador, obter informações de IP automáticas e conectar-se à Internet sem esforço ou configuração significativa por um administrador.  
  
Uma maneira comum de gerenciar usuários nesse tipo de ambiente é criar uma conta de usuário local para cada pessoa que acessará o sistema. Qualquer pessoa que tenha uma conta de usuário local nesse computador pode fazer logon nos serviços do MultiPoint de qualquer estação associada ao sistema. As contas de usuário local podem ser criadas e gerenciadas no MultiPoint Manager.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Vários sistemas MultiPoint Server com contas de usuário local  
Considerando que as contas de usuário local só podem ser acessadas do computador no qual foram criadas, ao implantar vários sistemas de serviços do MultiPoint em um ambiente, você pode gerenciar contas de usuário local de uma destas duas maneiras:  
  
-   Você pode criar contas de usuário para indivíduos específicos em computadores específicos que executam os serviços do MultiPoint.  
  
-   Você pode usar o MultiPoint Manager para criar contas para cada usuário em todos os computadores que executam os serviços do MultiPoint.  
  
Por exemplo, se você planeja atribuir usuários a um computador específico executando os serviços do MultiPoint, você pode criar quatro contas de usuário local no computador A (User01, User02, user03 e user04) e quatro contas de usuário local no computador B (user05, user06, user07 e user08). Nesse cenário, os usuários 01\-04 podem fazer logon no computador A de qualquer estação que esteja conectada a ele; no entanto, eles não podem fazer logon no computador B. O mesmo é verdadeiro para os usuários 05\-08, que poderão fazer logon somente no computador B, mas não no computador A. dependendo do ambiente de implantação específico, isso pode ser aceitável ou até mesmo desejável.  
  
No entanto, se todos os usuários tiverem que ser capazes de fazer logon em qualquer um dos computadores que executam os serviços do MultiPoint, uma conta de usuário local deverá ser criada para cada usuário em cada computador que estiver executando os serviços do MultiPoint. Optar por gerenciar usuários dessa maneira apresenta determinadas complexidades. Por exemplo, se User01 fizer logon no computador A na segunda-feira e salvar um arquivo na pasta documentos e o usuário fizer logon no computador B na terça-feira, o arquivo que foi salvo na pasta documentos no computador A não estará acessível no computador B.  
  
Além disso, se um usuário tiver contas no computador A e no computador B, não será possível sincronizar automaticamente as senhas para as contas. Isso pode resultar em usuários com dificuldade de fazer logon caso a senha da conta seja alterada em um computador, mas não na outra. Você pode simplificar o gerenciamento de contas de usuário nesse tipo de ambiente de rede, atribuindo a cada usuário um único computador que esteja executando os serviços do MultiPoint. Dessa forma, o usuário pode fazer logon em qualquer uma das estações associadas a esse computador e acessar os arquivos apropriados.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Vários sistemas de serviços do MultiPoint com contas de domínio  
Ambientes de domínio são comuns em grandes ambientes de rede que incluem vários servidores. Por exemplo, você pode ingressar em um ou mais computadores que executam a função de serviços do MultiPoint em um domínio e, em seguida, usar o Microsoft Active Directory para gerenciar contas de usuário que podem ser acessadas de qualquer computador no domínio. Isso permite que contas de usuário de domínio individual sejam criadas e acessadas de qualquer estação em qualquer sistema de serviços do MultiPoint que tenha ingressado no domínio.  
 
Ao implantar os serviços do MultiPoint em um ambiente de domínio, há vários fatores a serem considerados:  
  
-   Se as contas de domínio forem usadas, elas não poderão ser gerenciadas no MultiPoint Manager.  
  
-   Por padrão, os serviços do MultiPoint são configurados para conceder a cada usuário permissão para fazer logon em apenas uma estação por vez. Se você decidir permitir que os usuários façam logon em várias estações ao mesmo tempo usando uma única conta, poderá usar a opção **Editar configurações do servidor** no MultiPoint Manager.  
  
-   O local dos controladores de domínio pode afetar a velocidade e a confiabilidade com as quais os usuários poderão se autenticar com o domínio e localizar recursos.  
  
## <a name="single-user-account-for-multiple-stations"></a>Conta de usuário único para várias estações  
Os serviços do MultiPoint têm a capacidade de fazer logon em várias estações no mesmo computador simultaneamente usando uma única conta de usuário. Esse recurso é útil em ambientes em que os usuários não recebem nomes de usuário exclusivos e o local em que o uso de uma única conta de usuário pode simplificar o gerenciamento do sistema MultiPoint Services.  
  
