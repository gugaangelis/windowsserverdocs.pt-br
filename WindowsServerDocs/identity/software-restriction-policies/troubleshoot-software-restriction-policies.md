---
title: "Solucionar problemas de políticas de restrição de Software"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve problemas comuns e suas soluções ao início de políticas de restrição de Software (SRP) com o Windows Server 2008 e Windows Vista de solução de problemas.

## <a name="introduction"></a>Introdução
Políticas de restrição de software (SRP) é um recurso baseado em política de grupo que identifica os programas de software executados em computadores em um domínio e controla a capacidade desses programas para execução. Você pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Esses são integrados ao Microsoft Active Directory Domain Services e política de grupo, mas também podem ser configuradas em computadores autônomos. Para obter mais informações sobre o SRP, consulte o [políticas de restrição de Software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com SRP para uma parte da sua estratégia de controle do aplicativo.

### <a name="windows-cannot-open-a-program"></a>O Windows não pode abrir um programa
Os usuários recebem uma mensagem que diz "o Windows não pode abrir esse programa porque ele foi impedido por uma política de restrição de software. Para saber mais, abra o Visualizador de eventos ou entre em contato com o administrador do sistema." Ou, na linha de comando, uma mensagem dizendo que "o sistema não pode executar o programa especificado."

**Causa:** o nível de segurança padrão (ou uma regra) foi criada para que o programa de software está definido como **não permitido**, e assim ele não será iniciado.

**Solução:** examine o log de eventos para uma descrição detalhada da mensagem. A mensagem de log de eventos indica qual programa de software é definido como **não permitido** e qual regra é aplicada para o programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Políticas de restrição de software modificadas não estão tendo efeito
**Causa:** políticas de restrição de Software que são especificadas em um domínio por meio da política de grupo substituem as configurações de política definidas localmente. Isso pode implica em que há uma configuração de política do domínio que está substituindo sua configuração de política.

**Causa:** diretiva de grupo não pode ter atualizada suas configurações de política. Política de grupo se aplica as alterações às configurações de política periodicamente; Portanto, é provável que as alterações de política que foram feitas no diretório ainda não foram atualizadas.

**Soluções:**

1.  O computador no qual você modificar as políticas de restrição de software para a rede deve ser capaz de entrar em contato com um controlador de domínio. Certifique-se de que o computador pode contatar um controlador de domínio.

2.  Atualize a política por fazer logoff da rede e, em seguida, logon na rede novamente. Se qualquer política é aplicada por meio da política de grupo, fazer logon novamente no atualizará essas políticas.

3.  Você pode atualizar as configurações de política com o utilitário de linha de comando gpupdate ou logoff de e, em seguida, fazer logon novamente no computador. Para obter melhores resultados, execute gpupdate e, faça logoff e logon novamente no seu computador. Em geral, as configurações de segurança são atualizadas a cada 90 minutos em uma estação de trabalho ou servidor e a cada 5 minutos em um controlador de domínio. As configurações também são atualizadas a cada 16 horas, se houver quaisquer alterações ou não. Esses são configuráveis para que intervalos de atualização podem ser diferentes em cada domínio.

4.  Verifique quais políticas se aplicam. Verificar políticas de nível de domínio para **não substituir** configurações.

5.  Políticas de restrição de software que são especificadas em um domínio por meio da política de grupo substituem quaisquer políticas definidas localmente. Use a ferramenta de linha de comando Gpresult para determinar qual é o efeito da política. Isso pode implica em que há uma política do domínio que está substituindo sua configuração local.

6.  Se as configurações de política SRP e AppLocker estão no mesmo GPO, configurações do AppLocker terá precedência no Windows 7, Windows Server 2008 R2 e versões posteriores. É recomendável colocar as configurações de política SRP e AppLocker em GPOs diferentes.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Depois de adicionar uma regra por meio de SRP, você não pode fazer logon seu computador
**Causa:** seu computador acessa muitos programas e arquivos quando ele é iniciado. Você pode ter inadvertidamente configurado um desses programas ou arquivos a serem **não permitido**. Porque o computador não conseguir acessar o programa ou arquivo, ele não é iniciado corretamente.

**Solução:** iniciar o computador no modo de segurança, faça logon como um administrador local e, em seguida, alterar as políticas de restrição de software para permitir que o programa ou arquivo seja executado.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Uma nova configuração de política não está sendo aplicada a uma extensão de nome de arquivo específico
**Causa:** a extensão de nome de arquivo não está na lista de tipos de arquivo com suporte.

**Solução:** adicione a extensão de nome de arquivo à lista de tipos de arquivo compatíveis com SRP.

Políticas de restrição de software resolverem o problema de regular código desconhecido ou não confiável. Políticas de restrição de software são as configurações de segurança para identificar o software e controlar a capacidade de executar em um computador local, em um site, domínio ou unidade Organizacional e podem ser implementadas por meio de um GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Uma regra padrão não está restringindo conforme o esperado
**Causa:** regras que são aplicadas em uma ordem específica que pode fazer com que as regras padrão sejam substituídas pelas regras específicas. A SRP aplica as regras na seguinte ordem (mais específica para geral):

1.  Regras de hash

2.  Regras de certificado

3.  Regras de caminho

4.  Regras de zona da Internet

5.  Regras padrão

**Solução:** avaliar as regras restringindo o aplicativo e, se apropriado, remova todas menos a regra padrão.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Não é possível descobrir quais restrições são aplicadas
**Causa:** não há nenhuma causa aparente para o comportamento inesperado e atualização de GPO não solucionou o problema para investigação adicional é necessária.

**Soluções:**

1.  Investigue o Log de eventos do sistema, filtragem na fonte de "Política de restrição de Software." As entradas claramente qual regra está implementada para cada aplicativo.

2.  Habilite o registro em log avançado. Consulte [lista de permissões de negação determinar e inventário de aplicativos para políticas de restrição de Software](software-restriction-policies.md) para obter mais informações.


