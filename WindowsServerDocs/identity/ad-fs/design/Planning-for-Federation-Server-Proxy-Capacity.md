---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "Planejamento de capacidade de Proxy do servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d537f499af29716f0f82f842d10a93fd3f889f5d
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planejamento de capacidade de Proxy do servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Capacidade de planejamento de proxies de servidor de Federação ajuda você a calcular:  
  
-   Os requisitos de hardware apropriado para cada proxy de servidor de Federação.  
  
-   O número de servidores de Federação e proxies de servidor de Federação colocar em cada organização.  
  
Proxies de servidor de Federação redirecionar tokens de segurança de um servidor de Federação protegido na rede corporativa para os usuários federados. A finalidade de implantação de um proxy de servidor de Federação é permitir que usuários externos para se conectar a um servidor de Federação. Ele faz na verdade, não entrar tokens ou gravar dados no banco de dados de configuração do AD FS. Portanto, os requisitos de hardware para o proxy do servidor de Federação são geralmente menor do que os requisitos de hardware para um servidor de Federação.  
  
Porque cada solicitação de um proxy de servidor de Federação resulta em uma solicitação para um servidor de Federação ou farm de servidores de federação, capacidade de planejamento para servidores de Federação e proxies de servidor de federação deve ser executada em paralelo.  
  
Estimar os pico sign\-ins por segundo para o proxy do servidor de Federação requer uma compreensão dos padrões de uso dos usuários federados que fizerem logon através do proxy do servidor de Federação. Em muitos implantações, os usuários federados que logon usando o proxy do servidor de Federação estão localizados na Internet. Você pode estimar os pico sign\-ins por segundo, observando os padrões de uso desses usuários federados nos aplicativos Web existentes que serão protegidos pelo AD FS.  
  
> [!NOTE]  
> Para implantações de produção, é recomendável que pelo menos dois proxies de servidor de federação para cada instância de farm do servidor de federação que implantar.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Estimar o número de proxies de servidor de Federação necessários para sua organização  
Antes de você pode estimar o número de máquinas de proxy de servidor de Federação do AD FS necessária, você primeiro precisa determinar o número total de servidores de federação que você implantará em sua organização. Para saber mais sobre como fazer isso, consulte [planejamento de capacidade do servidor de Federação](Planning-for-Federation-Server-Capacity.md).  
  
Depois que você decidiu no número de servidores de federação, multiplique esse número de servidores pelo percentual de entrada autenticação federada solicita que você espera que serão feitas de usuários externos \ (localizada fora da rede a \ corporativa). O valor do cálculo fornecerá o número estimado de proxies de servidor de federação que manipulará as solicitações de autenticação de entrada para os usuários externos.  
  
Por exemplo, se o número de servidores de Federação recomendado é 3, e você espera que o número total de solicitações de autenticação que serão feitas de usuários externos será aproximadamente 60% do número total de solicitações de autenticação federada, o cálculo seria igual a 1,8 \ (3 X. 60\) que você pode arredondar até 2.  Portanto, neste caso, você precisa implantar dois computadores de proxy de servidor de federação para acomodar a carga de solicitações de autenticação de usuário externo para os servidores de três federação.  
  
Em testes realizados pela equipe de produto do AD FS, a utilização geral da CPU em cada proxy de servidor de Federação foi encontrada para ser significativamente menor do que a utilização da CPU que foi observada nos servidores de federação para o mesmo farm.  Em um teste, enquanto uma CPU de servidor de Federação foi indicando que ele foi totalmente saturado, a CPU para um proxy de servidor de Federação fornecimento de serviços de proxy para esse mesmo farm foi observada a única de utilização de 20%. Portanto, nossos testes revelaram que a carga da CPU de um proxy de servidor de federação, que usa as especificações de hardware semelhante conforme discutido anteriormente nesta seção, razoavelmente poderia lidar com a carga de processamento para servidores de Federação aproximadamente três.  
  
No entanto, para fins de tolerância de falhas, nós recomendamos um mínimo de dois proxies de servidor de federação para cada farm de servidores de federação que implantar.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
