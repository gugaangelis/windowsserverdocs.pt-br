---
title: Solucionar problemas de políticas de restrição de software
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872377"
---
# <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve problemas comuns e suas soluções ao solucionar problemas de políticas de restrição de Software (SRP) a partir do Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Elas são integradas ao Microsoft Active Directory Domain Services e a diretiva de grupo, mas também podem ser configuradas em computadores autônomos. Para obter mais informações sobre o SRP, consulte o [diretivas de restrição de Software](software-restriction-policies.md).

Começando com o Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com as SRP por uma parte de sua estratégia de controle de aplicativo.

### <a name="windows-cannot-open-a-program"></a>Windows não é possível abrir um programa
Os usuários recebem uma mensagem que diz "o Windows não podem abrir este programa porque ele foi impedido por uma diretiva de restrição de software. Para obter mais informações, abra o Visualizador de eventos ou entre em contato com o administrador do sistema." Ou, na linha de comando, uma mensagem informando que "o sistema não é possível executar o programa especificado".

**Causa:** O nível de segurança padrão (ou uma regra) foi criada para que o programa de software é definido como **não permitido**, e assim não iniciará.

**Solução:** Procure no log de eventos para obter uma descrição detalhada da mensagem. A mensagem de log de eventos indica qual programa de software está definido como **não permitido** e qual regra é aplicada ao programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Políticas de restrição de software modificado são não entrarem em vigor
**Causa:** Políticas de restrição de software que são especificadas em um domínio por meio da diretiva de grupo substituem as configurações de política que são configuradas localmente. Isso pode significa que há uma configuração de política do domínio que está substituindo sua configuração de política.

**Causa:** Política de grupo não pode ter atualizada suas configurações de política. Diretiva de grupo aplica alterações às configurações de política periodicamente; Portanto, é provável que as alterações de política que foram feitas no diretório ainda não foram atualizadas.

**Soluções:**

1.  O computador no qual você modificar políticas de restrição de software para a rede deve ser capaz de contatar um controlador de domínio. Verifique se que o computador pode contatar um controlador de domínio.

2.  Atualize a política de registro em log fora da rede e, em seguida, fazer logon na rede novamente. Se nenhuma política for aplicada por meio da diretiva de grupo, o logon novamente irá atualizar essas políticas.

3.  Você pode atualizar configurações de política com o utilitário de linha de comando gpupdate ou por logoff e depois logon novamente no computador. Para obter melhores resultados, execute gpupdate e, faça logoff e logon novamente no seu computador. Em geral, as configurações de segurança são atualizadas a cada 90 minutos em um servidor ou estação de trabalho e a cada 5 minutos em um controlador de domínio. As configurações também são atualizadas a cada 16 horas, com ou sem qualquer alteração. Essas são definições configuráveis para que intervalos de atualização podem ser diferentes em cada domínio.

4.  Verificação de quais políticas se aplicam. Verifique as políticas de nível de domínio para **não substituir** configurações.

5.  Políticas de restrição de software que são especificadas em um domínio por meio da diretiva de grupo substituem qualquer política que é configurada localmente. Use a ferramenta de linha de comando Gpresult para determinar o que é o efeito da política. Isso pode implica que haja uma política do domínio que está substituindo sua configuração local.

6.  Se as configurações de política SRP e AppLocker estiverem no mesmo GPO, as configurações do AppLocker terá precedência sobre o Windows 7, Windows Server 2008 R2 e versões posteriores. É recomendável colocar configurações de política SRP e AppLocker no GPOs diferentes.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Depois de adicionar uma regra por meio do SRP, você não pode fazer logon no seu computador
**Causa:** O computador acessa muitos programas e arquivos quando ele é iniciado. Você pode ter inadvertidamente definido um desses programas ou arquivos a serem **não permitido**. Porque o computador não pode acessar o arquivo ou programa, ele não pode ser iniciado corretamente.

**Solução:** Inicie o computador no modo de segurança, faça logon como um administrador local e, em seguida, alterar as políticas de restrição de software para permitir que o programa ou o arquivo a ser executado.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Uma nova configuração de política não está sendo aplicada a uma extensão de nome de arquivo específico
**Causa:** A extensão de nome de arquivo não está na lista de tipos de arquivo com suporte.

**Solução:** Adicione a extensão de nome de arquivo à lista de tipos de arquivo com suporte pela política SRP.

Políticas de restrição de software resolvem o problema de regulação código desconhecido ou não confiável. Políticas de restrição de software são as configurações de segurança para identificar o software e controlar sua capacidade de executar em um computador local, em um site, domínio ou unidade Organizacional e podem ser implementadas através de um GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Uma regra padrão não está restringindo conforme o esperado
**Causa:** Regras que são aplicadas em uma ordem específica que pode fazer com que as regras padrão a ser substituído por regras específicas. SRP aplica regras na seguinte ordem (mais específico para geral):

1.  Regras de hash

2.  Regras de certificado

3.  Regras de caminho

4.  Regras de zona da Internet

5.  Regras padrão

**Solução:** Avalie as regras que restringem o aplicativo e, se apropriado, remova todos, exceto a regra padrão.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Não é possível descobrir quais restrições são aplicadas
**Causa:** Não há nenhuma causa aparente para o comportamento inesperado e atualização do GPO não resolveu o problema para que uma investigação adicional é necessária.

**Soluções:**

1.  Investigue o Log de eventos do sistema, filtragem na origem da "Diretiva de restrição de Software." As entradas de declarar explicitamente qual regra é implementada para cada aplicativo.

2.  Habilite registro em log avançado. Ver [determinar lista de negação de permissão e o inventário de aplicativos para políticas de restrição de Software](software-restriction-policies.md) para obter mais informações.


