---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planning for Federation Server Proxy Capacity (Planejando a capacidade do proxy do servidor de federação)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5ea4ae7c9dcffc8d0aa464feb1043833e80877d7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947472"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planning for Federation Server Proxy Capacity (Planejando a capacidade do proxy do servidor de federação)

O planejamento de capacidade para proxies de servidor de Federação ajuda a estimar:

-   Os requisitos de hardware apropriados para cada proxy de servidor de Federação.

-   O número de servidores de Federação e proxies de servidor de Federação a serem colocados em cada organização.

Os proxies de servidor de Federação redirecionam tokens de segurança de um servidor de Federação protegido na rede corporativa para usuários federados. A finalidade de implantar um proxy de servidor de Federação é permitir que usuários externos se conectem a um servidor de Federação. Na verdade, ele não assina tokens ou grava em dados no banco de AD FS configuração. Portanto, os requisitos de hardware para o proxy do servidor de Federação geralmente são menores do que os requisitos de hardware para um servidor de Federação.

Como cada solicitação para um proxy de servidor de Federação resulta em uma solicitação para um servidor de Federação ou um farm de servidores de Federação, o planejamento de capacidade para servidores de Federação e proxies de servidor de Federação deve ser executado em paralelo.

Estimar as entradas de pico \- por segundo para o proxy do servidor de Federação requer uma compreensão dos padrões de uso dos usuários federados que entrarão por meio do proxy do servidor de Federação. Em muitas implantações, os usuários federados que se conectam usando o proxy do servidor de Federação estão localizados na Internet. Você pode estimar as entradas de pico \- por segundo examinando os padrões de uso desses usuários federados nos aplicativos Web existentes que serão protegidos pelo AD FS.

> [!NOTE]
> Para implantações de produção, recomendamos um mínimo de dois proxies de servidor de Federação para cada instância de farm de servidores de Federação que você implantar.

## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimar o número de proxies de servidor de Federação necessários para sua organização
Antes de poder estimar o número de máquinas proxy do servidor de Federação AD FS necessário, primeiro você precisará determinar o número total de servidores de Federação que serão implantados em sua organização. Para obter mais informações sobre como fazer isso, consulte [planejando a capacidade do servidor de Federação](Planning-for-Federation-Server-Capacity.md).

Depois de decidir sobre o número de servidores de Federação, multiplique esse número de servidores pela porcentagem de solicitações de autenticação federada de entrada que você espera que sejam feitas de usuários externos \( localizados fora da rede corporativa \) . O valor desse cálculo fornecerá o número estimado de proxies de servidor de Federação que manipulará as solicitações de autenticação de entrada para seus usuários externos.

Por exemplo, se o número de servidores de Federação recomendados for 3, e você esperar que o número total de solicitações de autenticação que serão feitas de usuários externos será de aproximadamente 60% do número total de solicitações de autenticação federada, seu cálculo será igual a 1,8 \( 3 X. 60, \) que pode ser arredondado para 2.  Portanto, nesse caso, você precisaria implantar dois computadores proxy de servidor de Federação para acomodar a carga de solicitações de autenticação de usuário externo para os três servidores de Federação.

Em testes executados pela equipe de produto AD FS, a utilização geral da CPU em cada proxy de servidor de Federação foi considerada significativamente menor do que a utilização da CPU observada nos servidores de Federação para o mesmo farm.  Em um teste, enquanto uma CPU de servidor de Federação estava indicando que estava completamente saturada, a CPU para um proxy de servidor de Federação que fornece serviços de proxy para esse mesmo farm foi observada em apenas 20% de utilização. Portanto, nossos testes revelaram que a carga na CPU de um proxy de servidor de Federação, que usa especificações de hardware semelhantes, conforme discutido anteriormente nesta seção, poderia lidar razoavelmente com a carga de processamento para aproximadamente três servidores de Federação.

No entanto, para fins de tolerância a falhas, recomendamos um mínimo de dois proxies de servidor de Federação para cada farm de servidores de Federação implantado.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
