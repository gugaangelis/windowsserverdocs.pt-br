---
title: Considerações de rede e contas de usuário
description: Fornece informações de planejamento para diferentes cenários de usuário e de rede com o MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880797"
---
# <a name="network-considerations-and-user-accounts"></a>Considerações de rede e contas de usuário
MultiPoint Services podem ser implantado em uma variedade de ambientes de rede, e ele pode dar suporte a contas de usuário locais e contas de usuário de domínio. Em geral, as contas de usuário do MultiPoint Services serão gerenciadas em um dos ambientes de rede a seguir:  
  
-   Um único computador executando o MultiPoint Services com contas de usuário local  
  
-   Vários computadores executando o MultiPoint Services, cada um com uma conta de usuário local  
  
-   Vários computadores que executam o MultiPoint Services e se estiver usando contas de usuário de domínio

Por definição, *contas de usuário local* só pode ser acessado do computador no qual eles foram criados. Contas de usuário local são contas de usuário que são criadas em um computador específico que está executando o MultiPoint Services. Em contraste, *contas de usuário de domínio* são contas de usuário que residem em um controlador de domínio, e eles podem ser acessados de qualquer computador que está conectado ao domínio. Quando você estiver decidindo qual tipo de ambiente de rede para usar, considere o seguinte:  
  
-   Recursos serão compartilhados entre servidores?  
  
-   Os usuários serão ser alternar entre servidores?  
  
-   Os usuários acessam os servidores de banco de dados que requerem autenticação?  
  
-   Os usuários acessam os servidores web internos que exigem a autenticação?  
  
-   Há uma infra-estrutura de domínio do Active Directory existente em vigor?  
  
-   Quem usará o MultiPoint Manager console para gerenciar áreas de trabalho do usuário, exibição de miniaturas, adicionar usuários, limitar sites e assim por diante? Essa pessoa gerenciarão mais de um servidor? Essa pessoa deve ter privilégios administrativos nos servidores.  
  
As seções a seguir abordam o gerenciamento de conta de usuário nesses ambientes de rede.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Único MultiPoint Server com as contas de usuário local  
Em ambientes com um único computador que está executando o MultiPoint Services, não há nenhum requisito para ter uma rede. No entanto, para tirar proveito dos recursos da Internet, os requisitos de rede podem ser tão simples de um roteador e uma conexão para um provedor de serviços de Internet (ISP). Conexões de rede que estão associadas um adaptador de rede no MultiPoint Services são configuradas por padrão, para obter um endereço IP e endereço do servidor DNS automaticamente por meio de DHCP. Roteadores de Internet geralmente são configurados como servidores DHCP e fornecem endereços IP privados para computadores que se conectam a eles na rede interna. Portanto, um único computador executando o MultiPoint Services pode ser capaz de se conectar à interface interna do roteador, obter informações de IP automático e se conectar à Internet sem esforço significativo ou configuração por um administrador.  
  
Uma maneira comum para gerenciar os usuários nesse tipo de ambiente é criar uma conta de usuário local para cada pessoa que acessarão o sistema. Qualquer pessoa que tenha uma conta de usuário local no computador em questão pode fazer logon no MultiPoint Services de qualquer estação que está associada com o sistema. Contas de usuário locais podem ser criadas e gerenciadas do Gerenciador do MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Vários sistemas MultiPoint Server com as contas de usuário local  
Considerando que as contas de usuário local só são acessíveis a partir do computador no qual eles foram criados, quando você implantar vários sistemas MultiPoint Services em um ambiente, você pode gerenciar contas de usuário local em uma das duas maneiras:  
  
-   Você pode criar contas de usuário para indivíduos específicos em computadores específicos, executando o MultiPoint Services.  
  
-   Você pode usar o Gerenciador do MultiPoint para criar contas para cada usuário em todos os computadores executando o MultiPoint Services.  
  
Por exemplo, se você planeja atribuir usuários a um computador específico executando o MultiPoint Services, você pode criar quatro contas de usuário local no computador A do (usuário01, user02, user03 e user04) e quatro contas de usuário local no computador B (user05, user06, user07 e user08). Nesse cenário, os usuários 01\-04 pode fazer logon no computador A partir de qualquer estação que está conectada a ele; no entanto, eles não podem fazer logon no computador B. O mesmo é verdadeiro para os usuários 05\-08, que seria capaz de fazer logon apenas para o computador B, mas não para computador A. dependendo do ambiente de implantação específicas, isso pode ser aceitável ou até mesmo desejáveis.  
  
No entanto, se cada usuário deve ser capaz de fazer logon em qualquer um dos computadores que executam o MultiPoint Services, uma conta de usuário local deve ser criada para cada usuário em cada computador que está executando o MultiPoint Services. Optar por gerenciar os usuários dessa maneira apresenta certas complexidades. Por exemplo, se user01 faz logon no computador A, na segunda-feira e salva um arquivo na pasta documentos e, em seguida, o usuário fizer logon computador B na terça-feira, o arquivo que foi salvo na pasta documentos no computador A não ser acessíveis no computador B.  
  
Além disso, se um usuário tiver contas de computador A e B do computador, não há nenhuma maneira de sincronizar automaticamente as senhas das contas. Isso pode resultar em usuários que têm dificuldade para fazer logon, a senha da conta deve ser alterada em um computador, mas não na outra. Você pode simplificar o gerenciamento de conta de usuário nesse tipo de ambiente de rede, atribuindo a cada usuário a um único computador que está executando o MultiPoint Services. Dessa forma, o usuário pode fazer logon qualquer uma das estações que estão associados esse computador e acessar os arquivos apropriados.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Vários sistemas MultiPoint Services com contas de domínio  
Ambientes de domínio são comuns em grandes ambientes de rede que incluem vários servidores. Por exemplo, você pode ingressar em um ou mais computadores executando a função do MultiPoint Services em um domínio e, em seguida, usar o Microsoft Active Directory para gerenciar contas de usuário que podem ser acessadas de qualquer computador no domínio. Isso permite que contas de usuário de domínio individuais sejam criados e acessados de qualquer estação em qualquer sistema MultiPoint Services que é associado ao domínio.  
 
Quando você implanta o MultiPoint Services em um ambiente de domínio, há vários fatores a considerar:  
  
-   Se as contas de domínio são usadas, eles não podem ser gerenciados do MultiPoint Manager.  
  
-   Por padrão, o MultiPoint Services é configurado para dar a cada usuário permissão para efetuar logon na estação apenas um por vez. Se você optar por permitir que os usuários façam logon em várias estações ao mesmo tempo usando uma única conta, você pode usar o **editar configurações de servidor** opção no Gerenciador do MultiPoint.  
  
-   O local dos controladores de domínio pode afetar a velocidade e a confiabilidade com os quais os usuários poderão autenticar com o domínio e localizar recursos.  
  
## <a name="single-user-account-for-multiple-stations"></a>Conta de usuário único para várias estações  
MultiPoint Services tem a capacidade de fazer logon em várias estações no mesmo computador ao mesmo tempo usando uma conta de usuário único. Esse recurso é útil em ambientes em que os usuários não recebem os nomes de usuário exclusivo e usar uma conta de usuário único pode simplificar o gerenciamento do sistema MultiPoint Services.  
  
