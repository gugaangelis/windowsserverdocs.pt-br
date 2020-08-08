---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: Planejar a auditoria de acesso ao arquivo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7a67994f9b4f2753d85d0f69d7ed9ede6eb8eac5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952744"
---
# <a name="plan-for-file-access-auditing"></a>Planejar a auditoria de acesso ao arquivo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As informações neste tópico explicam os aprimoramentos de auditoria de segurança introduzidos no Windows Server 2012 e novas configurações de auditoria que você deve considerar ao implantar o controle de acesso dinâmico em sua empresa. As configurações reais da política de auditoria que você implanta vão depender dos seus objetivos, que podem incluir a conformidade normativa, o monitoramento, a análise forense e a solução de problemas.

> [!NOTE]
> Informações detalhadas sobre como planejar e implantar uma estratégia geral de auditoria de segurança para sua empresa são explicadas em [Planejando e implantando políticas de auditoria de segurança avançada](https://go.microsoft.com/fwlink/?LinkID=191139). Para obter mais informações sobre como configurar e implantar uma política de auditoria de segurança, consulte o [Guia Passo a Passo de Política de Auditoria de Segurança Avançada](https://go.microsoft.com/fwlink/?LinkID=191141).

Os seguintes recursos de auditoria de segurança do Windows Server 2012 podem ser usados com o controle de acesso dinâmico para estender sua estratégia geral de auditoria de segurança.

-   **Políticas de auditoria com base em expressões**. O Controle de Acesso Dinâmico permite que você crie políticas de auditoria de destino usando expressões com base em declarações de usuário, computador e recurso. Por exemplo, você poderia criar uma política de auditoria para acompanhar todas as operações de Leitura e Gravação em arquivos classificados como de alto impacto nos negócios por funcionários que não têm uma permissão de alta segurança. As políticas de auditoria baseadas em expressão podem ser criadas diretamente para um arquivo ou pasta ou centralmente por meio da Política de Grupo. Para obter mais informações, consulte [Política de Grupo usando a auditoria de acesso a objetos globais](https://go.microsoft.com/fwlink/?LinkId=241498).

-   **Informações adicionais da auditoria de acesso a objeto**. A auditoria de acesso a arquivos não é novidade no Windows Server 2012. Com a aplicação da política de auditoria adequada, o sistema operacional Windows e Windows Server gera um evento de auditoria sempre que um usuário acessa um arquivo. Os eventos de Acesso a Arquivo Existente (4656, 4663) contêm informações sobre os atributos do arquivo que foi acessado. Essas informações podem ser usadas por ferramentas de filtragem de logs de eventos a fim de ajudá-lo a identificar os eventos de auditoria mais relevantes. Para obter mais informações, consulte [Auditoria de manipulação de identificador](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v=ws.10)) e [Gerenciador de contas de segurança de auditoria](https://go.microsoft.com/fwlink/?LinkId=241501).

-   **Mais informações de eventos de logon de usuário**. Com a aplicação da política de auditoria adequada, os sistemas operacionais Windows geram um evento de auditoria sempre que um usuário entra em um computador localmente ou remotamente. No Windows Server 2012 ou no Windows 8, você também pode monitorar declarações de usuário e dispositivo associadas ao token de segurança de um usuário. Exemplos podem incluir espaços vazios de Departamento, Empresa, Projeto e Segurança. O evento 4626 contém informações sobre esses declarações de usuários e de dispositivos, que podem ser aproveitadas por ferramentas de gerenciamento de logs de auditoria para correlacionar eventos de logon de usuário a eventos de acesso a objetos para permitir a filtragem de eventos com base em atributos de arquivo e de usuário. Para obter mais informações sobre a auditoria de logon do usuário, consulte [Logon de auditoria](https://go.microsoft.com/fwlink/?LinkId=241502).

-   **Acompanhamento de alterações de novos tipos de objetos protegíveis**. O acompanhamento de alterações em objetos protegíveis pode ser importante nas seguintes situações:

    -   **Acompanhamento de alterações nas políticas e regras de acesso central**. As políticas de acesso central e as regras de acesso central definem a política central que pode ser usada para controlar o acesso a recursos críticos. Qualquer alteração nelas pode afetar diretamente as permissões de acesso a arquivo que são concedidas aos usuários em vários computadores. Portanto, o acompanhamento de alterações em políticas e regras de acesso central pode ser importante para sua organização. Como as políticas e regras de acesso central são armazenadas no AD DS (Serviços de Domínio Active Directory), você pode fazer auditoria das tentativas para modificá-las, como a auditoria de alterações de qualquer outro objeto que pode ser protegido no AD DS. Para obter mais informações, consulte [Auditoria de acesso ao serviço de diretório](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941618(v=ws.10)).

    -   **Acompanhamento de alterações nas definições do dicionário de declarações**. As definições de declarações incluem o nome da declaração, a descrição e os valores possíveis. Qualquer alteração na definição de declaração pode afetar as permissões de acesso a recursos críticos. Portanto, o acompanhamento de alterações nas definições de declarações pode ser importante para sua organização. Como as políticas e regras de acesso central, as definições de declarações são armazenadas no AD DS. Assim, elas podem sofrer auditorias como qualquer outro objeto protegível no AD DS. Para obter mais informações, consulte [Auditoria de acesso ao serviço de diretório](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941618(v=ws.10)).

    -   **Acompanhamento de alterações nos atributos de arquivo**. Os atributos de arquivo determinam qual regra de central acesso se aplica ao arquivo. Uma alteração nos atributos de arquivo pode afetar as restrições de acesso no arquivo. Portanto, é importante acompanhar as alterações em atributos de arquivo. Você pode acompanhar as alterações nos atributos de arquivo em qualquer computador, configurando a política de auditoria de alteração da política de autorização. Para obter mais informações, consulte [Auditoria de alteração de políticas de autorização](https://go.microsoft.com/fwlink/?LinkId=241504) e [Auditoria de acesso para sistemas de arquivos](https://go.microsoft.com/fwlink/?LinkId=241505). No Windows Server 2012, o evento 4911 diferencia as alterações de política de atributo de arquivo de outros eventos de alteração de política de autorização.

    -   **Rastreio de alterações da política de acesso central associada a um arquivo.** O evento 4913 exibe os identificadores de segurança (SIDs) das políticas de acesso central antigas e novas. Cada política de acesso central também tem um nome de usuário amigável que pode ser pesquisado usando o identificador de segurança. Para obter mais informações, consulte [Auditoria de alteração de políticas de autorização](https://go.microsoft.com/fwlink/?LinkId=241504).

    -   **Acompanhamento de alterações em atributos de usuário e computador**. Como arquivos, os objetos de usuário e de computador podem ter atributos e as alterações nesses atributos podem afetar a capacidade do usuário de acessar arquivos. Portanto, é importante acompanhar as alterações em atributos de usuário ou computador. Os objetos de usuário e computador são armazenados no AD DS, o que indica que seus atributos pode sofrer auditoria. Para obter mais informações, consulte [Acesso ao DS](https://go.microsoft.com/fwlink/?LinkId=241508).

-   **Preparação da alteração de política**. As alterações nas políticas de acesso central podem afetar as decisões de controle de acesso em todos os computadores onde as políticas são aplicadas. Uma política frouxa poderia conceder mais acesso do que o desejado, e uma política excessivamente restritiva poderia gerar um número excessivo de chamadas de Suporte Técnico. Como resultado, pode ser extremamente valioso verificar as alterações a uma política de acesso central antes de aplicar a alteração. Para essa finalidade, o Windows Server 2012 apresenta o conceito de "preparo". A preparação permite que os usuários verifiquem as alterações de política propostas antes de aplicá-las. Para usar a preparação de política, as políticas propostas são implantadas com as políticas impostas, mas as políticas de preparo não concedem nem negam permissões. Em vez disso, o Windows Server 2012 registra um evento de auditoria (4818) sempre que o resultado da verificação de acesso que usa a política preparada é diferente do resultado de uma verificação de acesso que usa a política imposta.

