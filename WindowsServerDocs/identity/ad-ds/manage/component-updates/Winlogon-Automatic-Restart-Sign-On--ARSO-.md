---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: ARSO (Logon de Reinicialização Automática) de Winlogon
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4024a00c6c186aa929e88cb2aa86b0ec04a731b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883617"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>ARSO (Logon de Reinicialização Automática) de Winlogon

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows

> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.

## <a name="overview"></a>Visão geral
Aplicativos de tela de bloqueio do Windows 8 introduziu.  Esses são os aplicativos que executam o e exibam as notificações enquanto a sessão do usuário está bloqueada (calendário compromissos, email e mensagens, etc.).  Os dispositivos são reiniciados devido ao processo de atualização do Windows não conseguem exibir essas notificações da tela de bloqueio na reinicialização.  Alguns usuários dependem desses aplicativos de tela de bloqueio.

## <a name="whats-changed"></a>O que mudou?
Quando um usuário faz logon em um dispositivo Windows 8.1, a LSA salvará as credenciais do usuário na memória criptografada acessível somente por lsass.exe. Quando a atualização do Windows inicia uma reinicialização automática sem a presença do usuário, essas credenciais serão usadas para configurar o logon automático para o usuário. Atualização do Windows em execução como system com privilégio TCB iniciará a chamada RPC para fazer isso.

Na reinicialização, o usuário será automaticamente ser conectado por meio do mecanismo de logon automático e, em seguida, adicionalmente bloqueado para proteger a sessão do usuário. O bloqueio será iniciado por meio do Winlogon, enquanto o gerenciamento de credenciais é feito pela LSA.  Assinando automaticamente e bloqueio de usuário no console, aplicativos de tela de bloqueio do usuário será reiniciada e está disponível.

> [!NOTE]
> Depois que uma atualização do Windows induzida reinicialização, o último usuário interativo é conectado automaticamente e a sessão estiver bloqueada assim podem executar aplicativos de tela de bloqueio do usuário.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Visão geral rápida**

-   Atualização do Windows exige reinicialização

-   É possível reiniciar do computador (*não há aplicativos em execução que pode perder dados*)?

    -   Reinicialização para você

    -   Faça logon no

    -   Máquina de bloqueio

-   Habilitada ou desabilitada pela diretiva de grupo

    -   Desabilitado por padrão em SKUs de servidor

-   Por quê?

    -   Algumas atualizações não podem concluir até que o usuário faz logon novamente.

    -   Melhor experiência do usuário: não precisa esperar 15 minutos para que as atualizações concluir a instalação

-   Como? AutoLogon

    -   armazena a senha, usa essa credencial para fazer o logon

    -   credencial salva como um segredo LSA em memória paginável

    -   Só pode ser habilitada se o BitLocker está habilitado

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Política de grupo: Entrar no último usuário interativo automaticamente após uma reinicialização iniciada pelo sistema
No Windows 8.1 / Windows Server 2012 R2, o logon automático do usuário bloqueio de tela após uma reinicialização do Windows Update é aceitar para SKUs de servidor e recusar para SKUs de cliente.

**Local da política:** Configuração do computador > Políticas > modelos administrativos > componentes do Windows > opção de Logon do Windows

**Nome da política:** Entrar no último usuário interativo automaticamente após uma reinicialização iniciada pelo sistema

**Suporte para:** Pelo menos o Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1

**Descrição/ajuda:**

Essa configuração de política controla se um dispositivo será automaticamente entrar o último usuário interativo após a atualização do Windows reinicia o sistema.

Se você habilitar ou não definir essa configuração de política, o dispositivo salva com segurança as credenciais do usuário (incluindo o nome de usuário, domínio e senha criptografada) para configurar a entrada automática após a reinicialização de uma atualização do Windows. Após a reinicialização do Windows Update, o usuário está conectado no automaticamente e a sessão seja bloqueada automaticamente com todos os aplicativos de tela de bloqueio configurados para esse usuário.

Se você desabilitar essa configuração de política, o dispositivo não armazena as credenciais do usuário para entrar automática após a reinicialização de uma atualização do Windows. Aplicativos de tela de bloqueio dos usuários não são reiniciados depois que o sistema for reiniciado.

**Editor do registro**

|Nome do valor|Tipo|Dados|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Exemplo:**<br /><br />0 (ativado)<br /><br />1 (desabilitado)|

**Política local do registro:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nome do registro:** DisableAutomaticRestartSignOn

Valor: 0 ou 1

0 = Enabled

1 = desabilitado

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Solução de problemas
Quando o WinLogon bloqueia automaticamente, rastreamento de estado do WinLogon será armazenado no log de eventos do WinLogon.

O status de uma tentativa de configuração de logon automático é registrado

-   Se for bem-sucedido

    -   records it as such

-   Se uma falha:

    -   registra a falha ocorreu

-   Quando altera o estado do BitLocker:

    -   a remoção de credenciais será registrada em log

        -   Eles serão armazenados no log operacional de LSA.

### <a name="reasons-why-autologon-might-fail"></a>Razões autologon falhar
Há vários casos em que um logon automático de usuário não pode ser obtido.  Esta seção destina-se para capturar os conhecidos cenários em que isso pode ocorrer.

### <a name="user-must-change-password-at-next-login"></a>Usuário deve alterar a senha no próximo logon.
Logon de usuário pode entrar em um estado bloqueado quando a alteração de senha no próximo logon é necessária.  Isso pode ser detectado antes da reinicialização, na maioria dos casos, mas não todos (por exemplo, expiração de senha pode ser acessada entre o desligamento e no próximo logon.

### <a name="user-account-disabled"></a>Conta de usuário desativada
Uma sessão de usuário existente pode ser mantida mesmo se desabilitado.  Reinicialização de uma conta que está desabilitada pode ser detectada localmente na maioria dos casos com antecedência, dependendo da política de grupo pode não ser para contas de domínio (algum domínio em cache trabalho de cenários de logon, mesmo se a conta está desabilitada no controlador de domínio).

### <a name="logon-hours-and-parental-controls"></a>Horas de logon e controles dos pais
As horas de Logon e os controles dos pais podem impedir que uma nova sessão de usuário do que está sendo criado.  Se a reinicialização ocorrer durante essa janela, o usuário não seria permitido a fazer logon.  Não há políticas adicionais que faz com que o bloqueio ou logoff como uma ação de conformidade.  Isso pode ser problemático em muitos casos filho em que o bloqueio de conta pode ocorrer entre a hora de base e wake-up, especialmente se a janela de manutenção é comumente durante esse tempo.

## <a name="additional-resources"></a>Recursos adicionais
**Tabela tabela SEQ \\ \* árabe 3: ARSO Glossário**

|Termo|Definição|
|--------|--------------|
|Autologon|Logon automático é um recurso que está presente no Windows para lançamentos.  É um recurso documentado do Windows que tem até mesmo ferramentas como o v logon automático para Windows 3.01  *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Ele permite que um único usuário do dispositivo para entrar automaticamente sem inserir as credenciais. As credenciais estão configuradas e armazenadas no registro como um segredo LSA criptografado.|


