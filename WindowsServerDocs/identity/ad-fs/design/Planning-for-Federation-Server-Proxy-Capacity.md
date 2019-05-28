---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planning for Federation Server Proxy Capacity (Planejando a capacidade do proxy do servidor de federação)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c3efbb4081336ebfdfe9d3ab8a2b91412aa82dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191082"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planning for Federation Server Proxy Capacity (Planejando a capacidade do proxy do servidor de federação)

Planejamento de capacidade para proxies de servidor de Federação ajuda a estimar:  
  
-   Os requisitos de hardware apropriado para cada proxy do servidor de Federação.  
  
-   O número de servidores de Federação e proxies do servidor de federação para colocar em cada organização.  
  
Proxies de servidor de Federação redirecionam os tokens de segurança de um servidor de Federação protegidos na rede corporativa para usuários federados. O propósito de implantar um proxy do servidor de Federação é permitir que usuários externos para se conectar a um servidor de Federação. Ele faz na verdade não assinar tokens ou gravar dados no banco de dados de configuração do AD FS. Portanto, os requisitos de hardware para o proxy do servidor de Federação são geralmente menores do que os requisitos de hardware para um servidor de Federação.  
  
Porque cada solicitação para um proxy do servidor de Federação resulta em uma solicitação para um servidor de Federação ou farm de servidores de federação, o planejamento de capacidade para servidores de Federação e proxies de servidor de federação deve ser executada em paralelo.  
  
Estimando o sinal de pico\-ins por segundo para o proxy do servidor de Federação requer uma compreensão de padrões de uso dos usuários federados que irá entrar por meio do proxy de servidor de Federação. Em muitas implantações, os usuários federados que entram usando o proxy do servidor de Federação estão localizados na Internet. Você pode estimar o sinal de pico\-ins por segundo, observando os padrões de uso desses federado os usuários nos aplicativos Web existentes que serão protegidos pelo AD FS.  
  
> [!NOTE]  
> Para implantações de produção, é recomendável um mínimo de dois proxies de servidor de federação para cada instância de farm de servidor de federação que você implanta.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimar o número de proxies de servidor de Federação necessários para sua organização  
Antes de você pode estimar o número de máquinas de proxy do servidor de Federação do AD FS necessário, primeiro você precisará determinar o número total de servidores de federação que você implantará na sua organização. Para obter mais informações sobre como fazer isso, consulte [planejamento de capacidade do servidor de Federação](Planning-for-Federation-Server-Capacity.md).  
  
Depois de decidir o número de servidores de federação, multiplicar este número de servidores com o percentual de entrada autenticação federada solicita que você espera que serão feitas de usuários externos \(localizados fora da rede corporativa\). O valor desse cálculo fornecerá a você com o número estimado de proxies de servidor de federação que manipulará as solicitações de autenticação de entrada para os usuários externos.  
  
Por exemplo, se o número de servidores de Federação recomendado é 3 e esperar que o número total de solicitações de autenticação que serão feitas de usuários externos será aproximadamente 60% do número total de solicitações de autenticação federada, seu cálculo seria igual 1.8 \(3 X.60\) que você pode arredondar até 2.  Portanto, nesse caso, você precisaria implantar duas máquinas de proxy de servidor de federação para acomodar a carga das solicitações de autenticação de usuário externo para os servidores de três federação.  
  
Em testes executados pela equipe de produto do AD FS, a utilização geral da CPU em cada proxy do servidor de Federação foi encontrado para ser significativamente menor do que a utilização da CPU que foi observada em servidores de federação para o mesmo farm.  Em um teste, enquanto uma CPU de servidor de Federação foi indicando que foi completamente saturado, a CPU para um proxy do servidor de Federação fornecendo serviços de proxy para esse mesmo farm foi observada em apenas 20% de utilização. Portanto, nossos testes revelaram que a carga da CPU de um proxy de servidor de federação, que usa as especificações de hardware semelhante conforme discutido anteriormente nesta seção, razoavelmente pôde lidar com a carga de processamento para aproximadamente três servidores de Federação.  
  
No entanto, para fins de tolerância a falhas, é recomendável um mínimo de dois proxies de servidor de federação para cada farm de servidores de federação que você implanta.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
