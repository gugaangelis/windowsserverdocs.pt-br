---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: Planeje para o arquivo de auditoria de acesso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="plan-for-file-access-auditing"></a>Planeje para o arquivo de auditoria de acesso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As informações neste tópico explicam a melhorias de auditoria que são introduzidas no Windows Server 2012 e novas configurações que você deve considerar ao implantar o controle de acesso dinâmico em sua empresa de auditoria de segurança. As configurações de política de auditoria real que você implantar dependerá suas metas, que podem incluir conformidade regulatória, monitoramento, perícia e solução de problemas.  
  
> [!NOTE]  
> Informações detalhadas sobre como planejar e implantar uma estratégia de auditoria para sua empresa é explicada em geral de segurança [planejamento e implantação de políticas de auditoria de segurança avançada](https://go.microsoft.com/fwlink/?LinkID=191139). Para obter mais informações sobre como configurar e implantar uma política de auditoria de segurança, consulte o [política de auditoria de segurança avançada Step-by-Step guia](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Os seguintes recursos de auditoria de segurança no Windows Server 2012 podem ser usados com o controle de acesso dinâmico para estender sua estratégia de auditoria de segurança geral.  
  
-   **Políticas de auditoria baseadas em expressão**. Controle de acesso dinâmico permite que você crie políticas de auditoria de destino usando expressões com base em declarações do usuário, o computador e o recurso. Por exemplo, você pode criar uma política de auditoria para acompanhar a leitura e gravação todas as operações em arquivos classificados como impacto de negócios alto por funcionários que não têm um percurso de alta segurança. Políticas de auditoria baseadas em expressão podem ser criadas diretamente para um arquivo ou pasta ou centralmente por meio da política de grupo. Para obter mais informações, consulte [política de grupo usando Global Object Access Auditing](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Informações adicionais de auditoria de acesso a objeto**. Auditoria de acesso a arquivo não é novo no Windows Server 2012. Com a política de auditoria certa no lugar, os sistemas operacionais Windows e Windows Server geram um evento de auditoria de cada vez que um usuário acessa um arquivo. Eventos de acesso a arquivos existentes (4656, 4663) contêm informações sobre os atributos do arquivo que foi acessado. Essas informações podem ser usadas por ferramentas de filtragem de log de eventos para ajudá-lo a identificar os eventos de auditoria mais relevantes. Para obter mais informações, consulte [auditoria de manipulação de identificador](https://technet.microsoft.com//library/dd772626(WS.10).aspx) e [Gerenciador de contas de segurança auditoria](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Para obter mais informações de eventos de logon do usuário**. Com a política de auditoria certa no lugar, sistemas operacionais Windows geram um evento de auditoria sempre que um usuário entra em um computador local ou remotamente. No Windows Server 2012 ou Windows 8, você também pode monitorar declarações de dispositivo e usuário associadas ao token de segurança do usuário. Exemplos podem incluir permissões departamento, empresa, projeto e segurança. Evento 4626 contém informações sobre essas declarações do usuário e declarações de dispositivo, que podem ser aproveitadas por ferramentas de gerenciamento de log de auditoria para correlacionar os eventos de logon do usuário com eventos de acesso a objetos para ativar a filtragem de eventos com base em atributos de arquivo e atributos de usuário. Para obter mais informações sobre a auditoria de logon do usuário, consulte [Logon de auditoria](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Alteração de acompanhamento novos tipos de objetos protegíveis**. Controle de alterações em objetos protegíveis pode ser importante nos seguintes cenários:  
  
    -   **Rastreamento de alterações para políticas de acesso central e regras de acesso central**. Políticas de acesso central e regras de acesso central definem a política central que pode ser usada para controlar o acesso a recursos críticos. Qualquer alteração para estes pode influenciar diretamente as permissões de acesso de arquivo que são concedidas para usuários em vários computadores. Portanto, o controle de alterações em regras de acesso central e políticas de acesso central pode ser importante para sua organização. Como políticas de acesso central e regras de acesso central são armazenadas nos serviços de domínio do Active Directory (AD DS), você pode auditar tentativas de modificá-las, como a auditoria de alterações em um objeto protegível no AD DS. Para obter mais informações, consulte [auditoria de acesso de serviço de diretório](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Rastreamento de alterações para definições no dicionário reivindicação**. Definições de declarações incluem o nome da declaração, descrição e valores possíveis. Qualquer alteração na definição de declaração pode afetar as permissões de acesso em recursos críticos. Portanto, controlar alterações de declaração definições pode ser importante para sua organização. Como políticas de acesso central e regras de acesso central, definições de declarações são armazenadas no AD DS; Portanto, eles podem ser auditados como qualquer outro objeto protegível no AD DS. Para obter mais informações, consulte [auditoria de acesso de serviço de diretório](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Rastreamento de alterações para atributos de arquivo**. Atributos de arquivo determinam quais central acesso regra se aplica ao arquivo. Uma alteração para os atributos de arquivo pode afetar as restrições de acesso no arquivo. Portanto, pode ser importante para controlar as alterações nos atributos de arquivos. Você pode controlar as alterações nos atributos de arquivos em qualquer computador, configurando a política de auditoria de alteração de política autorização. Para obter mais informações, consulte [auditoria de alteração de política de autorização](https://go.microsoft.com/fwlink/?LinkId=241504) e [auditoria de acesso a objetos para sistemas de arquivos](https://go.microsoft.com/fwlink/?LinkId=241505). No Windows Server 2012, o evento 4911 diferencia as alterações de política de atributo do arquivo de outros eventos de alteração de política de autorização.  
  
    -   **Chang para a política de acesso central associada a um arquivo de rastreamento.** Evento 4913 exibe os identificadores de segurança (SIDs) das políticas de acesso central antiga e nova. Cada política de acesso central também tem um nome amigável que pode ser procurado usando esse identificador de segurança. Para obter mais informações, consulte [auditoria de alteração de política de autorização](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Rastreamento de alterações para atributos de usuário e computador**. Como arquivos, objetos de usuário e computador podem ter atributos e alterações para esses atributos podem afetar a capacidade do usuário para acessar arquivos. Portanto, pode ser valioso para controlar as alterações nos atributos de usuário ou computador. Objetos de usuário e computador são armazenados no AD DS; Portanto, as alterações em seus atributos podem ser auditadas. Para obter mais informações, consulte [acesso DS](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Preparo de alteração de política**. Alterações nas políticas de acesso central podem afetar as decisões de controle de acesso em todos os computadores em que as políticas são impostas. Uma política flexível pode conceder acesso mais do que desejado e uma política excessivamente restritiva poderia gerar um número excessivo de chamadas de suporte técnico. Como resultado, pode ser extremamente valioso para verificar as alterações a uma política de acesso central antes de forçá a alteração. Para essa finalidade, Windows Server 2012 apresenta o conceito de "teste". Preparo permite que os usuários verifiquem as alterações de política proposta antes de forçá-los. Para usar o preparo de política, políticas propostas são implantadas com as políticas impostas, mas políticas preparadas na verdade, não conceder ou negar permissões. Em vez disso, o Windows Server 2012 registra um evento de auditoria (4818) sempre que o resultado da verificação de acesso que usa a política preparada for diferente do resultado de uma verificação de acesso que usa a política imposta.  
  


