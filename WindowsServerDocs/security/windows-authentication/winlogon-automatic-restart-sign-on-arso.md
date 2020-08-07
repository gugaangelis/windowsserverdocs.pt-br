---
title: ARSO (Logon de Reinicialização Automática) de Winlogon
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 3e709c76bb1ae8c3557748d3a1e14f80fce89525
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936456"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>ARSO (Logon de Reinicialização Automática) de Winlogon

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

**Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows

> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.

## <a name="overview"></a>Visão geral
O Windows 8 introduziu aplicativos de tela de bloqueio.  Esses são os aplicativos que executam e exibem notificações enquanto a sessão do usuário está bloqueada (compromissos de calendário, email e mensagens, etc.).  Os dispositivos que são reiniciados devido ao processo de Windows Update falham ao exibir essas notificações da tela de bloqueio após a reinicialização.  Alguns usuários dependem desses aplicativos da tela de bloqueio.

## <a name="whats-changed"></a>O que mudou?
Quando um usuário entra em um dispositivo Windows 8.1, o LSA salvará as credenciais do usuário em memória criptografada acessível somente pelo lsass.exe. Quando Windows Update iniciar uma reinicialização automática sem a presença do usuário, essas credenciais serão usadas para configurar o logon automático para o usuário. Windows Update em execução como sistema com privilégio TCB iniciará a chamada RPC para fazer isso.

Na reinicialização, o usuário será automaticamente conectado por meio do mecanismo de logon automático e, em seguida, bloqueado para proteger a sessão do usuário. O bloqueio será iniciado por meio do Winlogon, enquanto o gerenciamento de credenciais é feito pela LSA.  Ao fazer logon automaticamente e bloquear o usuário no console do, os aplicativos da tela de bloqueio do usuário serão reiniciados e disponibilizados.

> [!NOTE]
> Após uma reinicialização Windows Update induzida, o último usuário interativo é automaticamente conectado e a sessão é bloqueada para que os aplicativos da tela de bloqueio do usuário possam ser executados.

![Captura de tela mostrando o bloqueio](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)

![Captura de tela mostrando os aplicativos de ecrã de bloqueio](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)

**Visão geral rápida**

-   Windows Update requer reinicialização

-   O computador pode ser reiniciado (*nenhum aplicativo em execução que perca dados*)?

    -   Reinicie o para você

    -   Fazer logon novamente

    -   Bloquear computador

-   Habilitado ou desabilitado pelo Política de Grupo

    -   Desabilitado por padrão em SKUs de servidor

-   Por quê?

    -   Algumas atualizações não podem ser concluídas até que o usuário faça logon novamente.

    -   Melhor experiência do usuário: não é necessário aguardar 15 minutos para que as atualizações concluam a instalação

-   Como posso fazer isso? Logon automático

    -   armazena a senha, usa essa credencial para fazer logon

    -   salva a credencial como um segredo de LSA na memória paginável

    -   Só poderá ser habilitado se o BitLocker estiver habilitado

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Política de Grupo: conectar o último usuário interativo automaticamente após uma reinicialização iniciada pelo sistema
No Windows 8.1/Windows Server 2012 R2, o logon automático do usuário da tela de bloqueio após uma reinicialização Windows Update é opcional para SKUs de servidor e recusa-se a SKUs do cliente.

**Local da política:** Configurações do computador > políticas > Modelos Administrativos > componentes do Windows > opção de logon do Windows

**Nome da política:** Conectar o último usuário interativo automaticamente após uma reinicialização iniciada pelo sistema

**Com suporte em:** Pelo menos Windows Server 2012 R2, Windows 8.1 ou Windows RT 8,1

**Descrição/ajuda:**

Essa configuração de política controla se um dispositivo irá entrar automaticamente no último usuário interativo depois que Windows Update reinicia o sistema.

Se você habilitar ou não definir essa configuração de política, o dispositivo salvará com segurança as credenciais do usuário (incluindo o nome de usuário, o domínio e a senha criptografada) para configurar a entrada automática após a reinicialização de um Windows Update. Após a reinicialização do Windows Update, o usuário é automaticamente conectado e a sessão é bloqueada automaticamente com todos os aplicativos de tela de bloqueio configurados para esse usuário.

Se você desabilitar essa configuração de política, o dispositivo não armazenará as credenciais do usuário para a entrada automática após uma reinicialização Windows Update. Os aplicativos da tela de bloqueio dos usuários não são reiniciados após a reinicialização do sistema.

**Editor do Registro**

|Nome do valor|Type|Dados|
|-------|----|----|
|DisableAutomaticRestartSignOn|DWORD|0<p>**Exemplo:**<p>0 (habilitado)<p>1 (desabilitado)|

**Local do registro de política:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nome do registro:** DisableAutomaticRestartSignOn

Valor: 0 ou 1

0 = habilitado

1 = desabilitado

![Captura de tela mostrando a configuração de política controla a interface do usuário em que você pode especificar se um dispositivo entrará automaticamente no último usuário interativo depois que Windows Update reinicia o sistema](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Solução de problemas
Quando o WinLogon é bloqueado automaticamente, o rastreamento de estado do WinLogon será armazenado no log de eventos do WinLogon.

O status de uma tentativa de configuração de logon automático é registrado

-   Se for bem-sucedido

    -   registra-o como tal

-   Se for uma falha:

    -   registra o que a falha foi

-   Quando o estado do BitLocker é alterado:

    -   a remoção das credenciais será registrada

        -   Eles serão armazenados no log operacional LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivos pelos quais o logon automático pode falhar
Há vários casos em que um logon automático do usuário não pode ser obtido.  Esta seção destina-se a capturar os cenários conhecidos nos quais isso pode ocorrer.

### <a name="user-must-change-password-at-next-login"></a>O usuário deve alterar a senha no próximo logon
O logon do usuário pode entrar em um estado bloqueado quando a alteração de senha no próximo logon for necessária.  Isso pode ser detectado antes da reinicialização na maioria dos casos, mas nem todos (por exemplo, expiração de senha podem ser alcançados entre o desligamento e o próximo logon.

### <a name="user-account-disabled"></a>Conta de usuário desabilitada
Uma sessão de usuário existente pode ser mantida mesmo se estiver desabilitada.  A reinicialização de uma conta desabilitada pode ser detectada localmente na maioria dos casos com antecedência, dependendo da GP, ela pode não ser para contas de domínio (alguns cenários de logon em cache de domínio funcionam mesmo que a conta esteja desabilitada no DC).

### <a name="logon-hours-and-parental-controls"></a>Horas de logon e controles dos pais
O horário de logon e os controles dos pais podem proibir a criação de uma nova sessão de usuário.  Se uma reinicialização fosse executada durante essa janela, o usuário não teria permissão para fazer logon.  Há uma política adicional que causa o bloqueio ou logout como uma ação de conformidade.  Isso pode ser problemático para muitos casos filho em que o bloqueio de conta pode ocorrer entre o tempo e a ativação, especialmente se a janela de manutenção for normalmente durante esse tempo.

## <a name="additional-resources"></a>Recursos adicionais
**Tabela SEQ tabela \\ \* árabe 3: Glossário de ARSO**

|Termo|Definição|
|----|-------|
|Autologon|O logon automático é um recurso que está presente no Windows para várias versões.  É um recurso documentado do Windows que até mesmo tem ferramentas como o logon automático para Windows v 3.01 * [http:/technet. Microsoft. com/Sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<p>Ele permite que um único usuário do dispositivo se conecte automaticamente sem inserir as credenciais. As credenciais são configuradas e armazenadas no registro como um segredo de LSA criptografado.|


