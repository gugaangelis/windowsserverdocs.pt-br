---
title: Solucionar problemas de políticas de restrição de software
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 003a71ed8c6b7e8d9b788c4eb8aa5efcc4bd6286
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640211"
---
# <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve problemas comuns e suas soluções ao solucionar problemas de SRP (diretivas de restrição de software) que começam com o Windows Server 2008 e o Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Eles são integrados ao Microsoft Active Directory Domain Services e Política de Grupo, mas também podem ser configurados em computadores autônomos. Para obter mais informações sobre o SRP, consulte as [diretivas de restrição de software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e do Windows 7, o Windows AppLocker pode ser usado em vez de ou em conjunto com o SRP para uma parte da sua estratégia de controle de aplicativo.

### <a name="windows-cannot-open-a-program"></a>O Windows não pode abrir um programa
Os usuários recebem uma mensagem dizendo que "o Windows não pode abrir este programa porque ele foi impedido por uma diretiva de restrição de software. Para obter mais informações, abra Visualizador de Eventos ou contate o administrador do sistema. " Ou, na linha de comando, uma mensagem diz "o sistema não pode executar o programa especificado".

**Causa:** O nível de segurança padrão (ou uma regra) foi criado para que o programa de software seja definido como não **permitido**e, consequentemente, ele não será iniciado.

**Solução:** Examine o log de eventos para obter uma descrição detalhada da mensagem. A mensagem de log de eventos indica qual programa de software está definido como não **permitido** e qual regra é aplicada ao programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>As políticas de restrição de software modificadas não estão tendo efeito
**Causa:** As políticas de restrição de software especificadas em um domínio por meio de Política de Grupo substituem as configurações de diretiva que são configuradas localmente. Isso pode implicar que há uma configuração de política do domínio que está substituindo sua configuração de política.

**Causa:** Política de Grupo pode não ter atualizado suas configurações de política. Política de Grupo aplica alterações às configurações de política periodicamente; Portanto, é provável que as alterações de política feitas no diretório ainda não tenham sido atualizadas.

**Soluções**

1.  O computador no qual você modifica as políticas de restrição de software para a rede deve ser capaz de contatar um controlador de domínio. Verifique se o computador pode contatar um controlador de domínio.

2.  Atualize a política fazendo logoff da rede e, em seguida, fazendo logon na rede novamente. Se qualquer política for aplicada por meio de Política de Grupo, o logon novamente atualizará essas políticas.

3.  Você pode atualizar as configurações de política com o utilitário de linha de comando gpupdate ou fazendo logoff de e, em seguida, fazendo logon novamente no computador. Para obter melhores resultados, execute gpupdate e, em seguida, faça logoff em e faça logon novamente no computador. Em geral, as configurações de segurança são atualizadas a cada 90 minutos em uma estação de trabalho ou servidor e a cada 5 minutos em um controlador de domínio. As configurações também são atualizadas a cada 16 horas, com ou sem qualquer alteração. Essas são configurações configuráveis, de modo que os intervalos de atualização podem ser diferentes em cada domínio.

4.  Verifique quais políticas se aplicam. Verifique as políticas no nível do domínio para nenhuma configuração de **substituição** .

5.  As políticas de restrição de software especificadas em um domínio por meio de Política de Grupo substituem as políticas que são configuradas localmente. Use a ferramenta de linha de comando gpresult para determinar qual é o efeito líquido da política. Isso pode implicar que há uma política do domínio que está substituindo sua configuração local.

6.  Se as configurações de política de SRP e AppLocker estiverem no mesmo GPO, as configurações do AppLocker terão precedência no Windows 7, Windows Server 2008 R2 e posterior. É recomendável colocar as configurações de política do SRP e do AppLocker em GPOs diferentes.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Depois de adicionar uma regra por meio do SRP, você não pode fazer logon no computador
**Causa:** O computador acessa muitos programas e arquivos quando ele é iniciado. Você pode ter definido inadvertidamente um desses programas ou arquivos como não **permitido**. Como o computador não pode acessar o programa ou arquivo, ele não pode ser iniciado corretamente.

**Solução:** Inicie o computador no modo de segurança, faça logon como administrador local e, em seguida, altere as diretivas de restrição de software para permitir que o programa ou arquivo seja executado.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Uma nova configuração de política não está sendo aplicada a uma extensão de nome de arquivo específica
**Causa:** A extensão de nome de arquivo não está na lista de tipos de arquivos com suporte.

**Solução:** Adicione a extensão de nome de arquivo à lista de tipos de arquivos com suporte pelo SRP.

As diretivas de restrição de software resolvem o problema de regulagem de código desconhecido ou não confiável. As diretivas de restrição de software são configurações de segurança para identificar o software e controlar sua capacidade de execução em um computador local, em um site, domínio ou UO e pode ser implementado por meio de um GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Uma regra padrão não está restringindo conforme o esperado
**Causa:** Regras que são aplicadas em uma ordem específica que pode fazer com que as regras padrão sejam substituídas por regras específicas. O SRP aplica regras na seguinte ordem (mais específica para geral):

1.  Regras de hash

2.  Regras de certificado

3.  Regras de caminho

4.  Regras de zona da Internet

5.  Regras padrão

**Solução:** Avalie as regras que restringem o aplicativo e, se apropriado, remova todas as regras, exceto a padrão.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Não é possível descobrir quais restrições são aplicadas
**Causa:** Não há nenhuma causa aparente para o comportamento inesperado, e a atualização de GPO não resolveu o problema, de modo que uma investigação adicional é necessária.

**Soluções**

1.  Investigue o log de eventos do sistema, filtrando a origem da "diretiva de restrição de software". As entradas explicitamente deformam qual regra é implementada para cada aplicativo.

2.  Habilite o registro em log avançado. Consulte [determinar a lista de permissões de negação e o inventário de aplicativos para](software-restriction-policies.md) obter as políticas de restrição de software para obter mais informações.


