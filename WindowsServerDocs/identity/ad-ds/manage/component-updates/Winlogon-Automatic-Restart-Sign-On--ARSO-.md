---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: "Winlogon automático reiniciar logon (ARSO)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 27d4285d34105908555458a95bd70fc04fd2901a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon automático reiniciar logon (ARSO)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows

> [!NOTE]
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.

## <a name="overview"></a>Visão geral
O Windows 8 introduziu os aplicativos de tela de bloqueio.  Estes são os aplicativos que são executados e exibam notificações enquanto a sessão do usuário é bloqueada (calendário compromissos, email e mensagens, etc.).  Dispositivos que são reiniciados devido o processo de atualização do Windows falharem exibir essas notificações de tela de bloqueio após a reinicialização.  Alguns usuários dependem desses aplicativos de tela de bloqueio.

## <a name="whats-changed"></a>O que mudou?
Quando um usuário entra em um dispositivo Windows 8.1, a LSA salvará as credenciais do usuário na memória criptografada acessível apenas por lsass.exe. Quando o Windows Update inicia uma reinicialização automática sem a presença do usuário, essas credenciais serão usadas para configurar o logon automático para o usuário. Atualização do Windows em execução como sistema com o privilégio TCB iniciará a chamada RPC para fazer isso.

Na reinicialização, o usuário será automaticamente ser conectado por meio do mecanismo de Autologon e, em seguida, além disso bloqueado para proteger a sessão do usuário. O bloqueio será iniciado por meio do Winlogon enquanto o gerenciamento de credenciais é feito ao LSA.  Assinando automaticamente e bloqueando o usuário no console, aplicativos de tela de bloqueio do usuário será reiniciado e estará disponível.

> [!NOTE]
> Depois que uma atualização do Windows induzido reinicialização, o último usuário interativo é conectado automaticamente e a sessão estiver bloqueada então podem executar aplicativos de tela de bloqueio do usuário.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Visão geral rápida**

-   Windows Update requer reinicialização

-   É possível reiniciar o computador (*nenhum aplicativos em execução que perderia dados*)?

    -   Reiniciar para você

    -   Faça logon novamente

    -   Máquina de bloqueio

-   Habilitado ou desabilitado pela política de grupo

    -   Desabilitada por padrão em SKUs de servidor

-   Por quê?

    -   Algumas atualizações não podem concluir até que o usuário fizer logon novamente.

    -   Melhor experiência do usuário: não tenha de esperar 15 minutos para atualizações concluir a instalação

-   Como? Logon automático

    -   Armazena a senha, que usa essa credencial para conectá-lo

    -   Credenciais salva como um segredo LSA na memória paginada

    -   Pode ser habilitado apenas quando o BitLocker está habilitado

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Política de grupo: Entrar último usuário interativo automaticamente após um reinício iniciadas pelo sistema
No Windows 8.1 / Windows Server 2012 R2, autologon do usuário de tela de bloqueio após a reinicialização do Windows Update é aceitar SKUs de servidor e recusar para SKUs do cliente.

**Local de política:** configuração do computador > Políticas > modelos administrativos > componentes do Windows > opção de Logon do Windows

**Nome da política:** entrar último usuário interativo automaticamente após um reinício iniciadas pelo sistema

**Suportado em:** pelo menos Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1

**Descrição/ajuda:**

Esta configuração de política controla se um dispositivo será automaticamente entrada do último usuário interativo após a atualização do Windows reinicia o sistema.

Se você habilita ou não definir esta configuração de política, o dispositivo salva com segurança as credenciais do usuário (incluindo o nome de usuário, domínio e senha criptografada) para configurar a entrada automática após reiniciar uma atualização do Windows. Após a reinicialização do Windows Update, o usuário está conectado no automaticamente e a sessão estiver bloqueada automaticamente com todos os aplicativos de tela de bloqueio configurados para esse usuário.

Se você desabilitar esta configuração de política, o dispositivo não armazena as credenciais do usuário para entrar automático após reiniciar uma atualização do Windows. Aplicativos de tela de bloqueio dos usuários não são reiniciados depois que o sistema for reiniciado.

**Editor do registro**

|Nome do valor|Tipo|Dados|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Exemplo:**<br /><br />0 (ativado)<br /><br />1 (desabilitado)|

**Política do Registro local:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nome do registro:** DisableAutomaticRestartSignOn

Valor: 0 ou 1

0 = ativada

1 = disabled

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Solução de problemas
Quando o WinLogon bloqueia automaticamente, rastreamento de estado do WinLogon será armazenado no log de eventos do WinLogon.

O status de uma tentativa de configuração de Autologon está conectado

-   Se ele for bem-sucedido

    -   Registra como tal

-   Se for uma falha:

    -   Registra a falha foi

-   Quando muda o estado do BitLocker:

    -   A remoção de credenciais será registrado em log

        -   Eles serão armazenados no log operacional LSA.

### <a name="reasons-why-autologon-might-fail"></a>Razões autologon falhar
Há diversos casos em que o logon automático de usuário não pode ser obtido.  Esta seção destina-se para capturar os conhecidos cenários em que isso pode ocorrer.

### <a name="user-must-change-password-at-next-login"></a>Usuário deve alterar a senha no próximo logon.
Logon do usuário pode inserir um estado bloqueado quando a alteração de senha no próximo logon é necessária.  Isso pode ser detectado antes da reinicialização na maioria dos casos, mas não todas (por exemplo, a expiração de senha pode ser acessada entre desligamento e no próximo logon.

### <a name="user-account-disabled"></a>Conta de usuário desativada
Uma sessão de usuário existentes pode ser mantida mesmo que desabilitada.  Reiniciar para uma conta que está desabilitada pode ser detectada localmente na maioria dos casos com antecedência, dependendo de gp pode não ser para contas de domínio (alguns domínio armazenadas em cache trabalho de cenários de logon mesmo que a conta está desabilitada em DC).

### <a name="logon-hours-and-parental-controls"></a>Horas de logon e os controles dos pais
As horas de Logon e os controles dos pais podem proibir uma nova sessão de usuário seja criado.  Se fosse uma reinicialização ocorrer durante esta janela, o usuário não seria permitido para fazer logon.  Há política adicional que faz com que o bloqueio ou logoff como uma ação de conformidade.  Isso pode ser problemático para muitos casos filho onde o bloqueio de conta pode ocorrer entre cama tempo e despertar, especialmente se a janela de manutenção é comumente durante esse tempo.

## <a name="additional-resources"></a>Recursos adicionais
**Tabela SEQ tabela \ \ \ * árabe 3: ARSO Glossário**

|Termo|Definição|
|--------|--------------|
|Logon automático|AutoLogon é um recurso que esteve presente no Windows em várias versões.  É um recurso do Windows até mesmo com ferramentas como v 3.01 Autologon para Windows documentado *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Ele permite que um usuário único do dispositivo fazer logon automaticamente sem inserir credenciais. As credenciais são configuradas e armazenadas no registro, como um segredo LSA criptografado.|


