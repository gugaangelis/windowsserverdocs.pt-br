---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Logon automático de reinício do Winlogon (ARSO)
description: Como o logon automático de reinicialização do Windows pode ajudar a tornar seus usuários mais produtivos.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.openlocfilehash: 711a3fc22977d7aa9751c8e200524f4cd295110b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943303"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Logon automático de reinício do Winlogon (ARSO)

Durante uma Windows Update, há processos específicos do usuário que devem acontecer para que a atualização seja concluída. Esses processos exigem que o usuário esteja conectado ao seu dispositivo. No primeiro logon após a inicialização de uma atualização, os usuários devem aguardar até que esses processos específicos do usuário sejam concluídos antes que possam começar a usar seu dispositivo.

## <a name="how-does-it-work"></a>Como ela funciona?

Quando Windows Update inicia uma reinicialização automática, o ARSO extrai as credenciais derivadas do usuário conectado no momento, persiste-as em disco e configura o logon automático para o usuário. Windows Update em execução como sistema com privilégio TCB iniciará a chamada RPC para fazer isso.

Após a reinicialização final do Windows Update, o usuário será automaticamente conectado por meio do mecanismo de logon automático e a sessão do usuário será alimentada com os segredos persistentes. Além disso, o dispositivo está bloqueado para proteger a sessão do usuário. O bloqueio será iniciado por meio do Winlogon, enquanto o gerenciamento de credenciais é feito pela autoridade de segurança local (LSA). Após uma configuração de ARSO e um logon bem-sucedidos, as credenciais salvas são excluídas imediatamente do disco.

Ao fazer logon automaticamente e bloquear o usuário no console do, Windows Update pode concluir os processos específicos do usuário antes que o usuário retorne ao dispositivo. Dessa forma, o usuário pode começar imediatamente a usar seu dispositivo.

O ARSO trata de forma diferente os dispositivos gerenciados e não. Para dispositivos não gerenciados, a criptografia do dispositivo é usada, mas não é necessária para que o usuário obtenha ARSO. Para dispositivos gerenciados, o TPM 2,0, o SecureBoot e o BitLocker são necessários para a configuração do ARSO. Os administradores de ti podem substituir esse requisito por meio de Política de Grupo. O ARSO para dispositivos gerenciados está disponível no momento somente para dispositivos que ingressaram em Azure Active Directory.

| Windows Update | Shutdown-g-t 0 | Reinicializações iniciadas pelo usuário | APIs com sinalizadores de SHUTDOWN_ARSO/EWX_ARSO |
|--|--|--|--|
| Dispositivos gerenciados-Sim<p>Dispositivos não gerenciados – Sim | Dispositivos gerenciados-Sim<p>Dispositivos não gerenciados – Sim | Dispositivos gerenciados-não<p>Dispositivos não gerenciados – Sim | Dispositivos gerenciados-Sim<p>Dispositivos não gerenciados – Sim |

> [!NOTE]
> Após uma reinicialização Windows Update induzida, o último usuário interativo é conectado automaticamente e a sessão é bloqueada. Isso permite que os aplicativos da tela de bloqueio de um usuário ainda sejam executados apesar da Windows Update reinicialização.

![Página de configurações](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>#1 de política

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>Entrar e bloquear o último usuário interativo automaticamente após uma reinicialização

No Windows 10, o ARSO está desabilitado para SKUs de servidor e recusa-se para SKUs de cliente.

**Local da política de Grupo:** Configuração do computador > Modelos Administrativos > componentes do Windows > opções de logon do Windows

**Política do Intune:**

- Plataforma: Windows 10 e posterior
- Tipo de perfil: Modelos Administrativos
- Caminho: \Windows do Windows\windows opções de logon

**Com suporte em:** Pelo menos Windows 10 versão 1903

**Descrição:**

Essa configuração de política controla se um dispositivo fará logon automaticamente e bloqueará o último usuário interativo após a reinicialização do sistema ou após um desligamento e uma inicialização a frio.

Isso só ocorrerá se o último usuário interativo não tiver se desconectado antes da reinicialização ou desligamento.

Se o dispositivo tiver ingressado em Active Directory ou Azure Active Directory, essa política só se aplicará a Windows Update reinicializações. Caso contrário, isso se aplicará a reinicializações Windows Updates e reinicializações iniciadas pelo usuário e desligamentos.

Se você não definir essa configuração de política, ela será habilitada por padrão. Quando a política está habilitada, o usuário é automaticamente conectado e a sessão é bloqueada automaticamente com todos os aplicativos de tela de bloqueio configurados para esse usuário após a inicialização do dispositivo.

Depois de habilitar essa política, você pode definir suas configurações por meio da política ConfigAutomaticRestartSignOn, que configura o modo de entrar automaticamente e bloquear o último usuário interativo após uma reinicialização ou inicialização a frio.

Se você desabilitar essa configuração de política, o dispositivo não configurará a conexão automática. Os aplicativos da tela de bloqueio do usuário não são reiniciados após a reinicialização do sistema.

**Editor do registro:**

| Nome do valor | Type | Dados |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (habilitar ARSO) |
|   |   | 1 (desabilitar ARSO) |

**Local do registro de política:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>#2 de política

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>Configurar o modo de entrar automaticamente e bloquear o último usuário interativo após uma reinicialização ou inicialização a frio

**Local da política de Grupo:** Configuração do computador > Modelos Administrativos > componentes do Windows > opções de logon do Windows

**Política do Intune:**

- Plataforma: Windows 10 e posterior
- Tipo de perfil: Modelos Administrativos
- Caminho: \Windows do Windows\windows opções de logon

**Com suporte em:** Pelo menos Windows 10 versão 1903

**Descrição:**

Essa configuração de política controla a configuração sob a qual uma reinicialização automática e logon e bloqueio ocorre após uma reinicialização ou inicialização a frio. Se você escolher "desabilitado" na política "entrar e bloquear o último usuário interativo automaticamente após uma reinicialização", o logon automático não ocorrerá e essa política não precisará ser configurada.

Se você habilitar essa configuração de política, poderá escolher uma das duas opções a seguir:

1. "Habilitado se o BitLocker estiver ativado e não suspenso" especifica que o logon automático e o bloqueio só ocorrerão se o BitLocker estiver ativo e não for suspenso durante a reinicialização ou o desligamento. Os dados pessoais podem ser acessados no disco rígido do dispositivo no momento se o BitLocker não estiver ativado ou suspenso durante uma atualização. A suspensão do BitLocker remove temporariamente a proteção de componentes e dados do sistema, mas pode ser necessária em determinadas circunstâncias para atualizar com êxito os componentes críticos de inicialização.
   - O BitLocker será suspenso durante as atualizações se:
      - O dispositivo não tem o TPM 2,0 e o PCR7, ou
      - O dispositivo não usa um protetor somente TPM
2. "Always Enabled" especifica que o logon automático ocorrerá mesmo se o BitLocker estiver desativado ou suspenso durante a reinicialização ou o desligamento. Quando o BitLocker não está habilitado, os dados pessoais ficam acessíveis no disco rígido. A reinicialização automática e o logon só devem ser executados sob essa condição se você tiver certeza de que o dispositivo configurado está em um local físico seguro.

Se você desabilitar ou não definir essa configuração, o logon automático usará como padrão o comportamento "habilitado se o BitLocker estiver ligado e não suspenso".

**Editor do registro**

| Nome do valor | Type | Dados |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (habilitar ARSO se seguro) |
|   |   | 1 (habilitar ARSO Always) |

**Local do registro de política:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>Solução de problemas

Quando o WinLogon é bloqueado automaticamente, o rastreamento de estado do WinLogon será armazenado no log de eventos do WinLogon.

O status de uma tentativa de configuração de logon automático é registrado

- Se for bem-sucedido
   - registra-o como tal
- Se for uma falha:
   - registra o que a falha foi
- Quando o estado do BitLocker é alterado:
   - a remoção das credenciais será registrada
   - Eles serão armazenados no log operacional LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivos pelos quais o logon automático pode falhar

Há vários casos em que um logon automático do usuário não pode ser obtido.  Esta seção destina-se a capturar os cenários conhecidos nos quais isso pode ocorrer.

### <a name="user-must-change-password-at-next-login"></a>O usuário deve alterar a senha no próximo logon

O logon do usuário pode entrar em um estado bloqueado quando a alteração de senha no próximo logon for necessária.  Isso pode ser detectado antes da reinicialização na maioria dos casos, mas nem todos (por exemplo, expiração de senha podem ser alcançados entre o desligamento e o próximo logon.

### <a name="user-account-disabled"></a>Conta de usuário desabilitada

Uma sessão de usuário existente pode ser mantida mesmo se estiver desabilitada.  A reinicialização de uma conta desabilitada pode ser detectada localmente na maioria dos casos com antecedência, dependendo da GP, ela pode não ser para contas de domínio (alguns cenários de logon em cache de domínio funcionam mesmo que a conta esteja desabilitada no DC).

### <a name="logon-hours-and-parental-controls"></a>Horas de logon e controles dos pais

O horário de logon e os controles dos pais podem proibir a criação de uma nova sessão de usuário.  Se uma reinicialização fosse executada durante essa janela, o usuário não teria permissão para fazer logon.  Há uma política adicional que causa o bloqueio ou logout como uma ação de conformidade. O status de uma tentativa de configuração de logon automático é registrado.

## <a name="security-details"></a>Detalhes de segurança

### <a name="credentials-stored"></a>Credenciais armazenadas

| Hash de senha | Chave de credencial | Tíquete de concessão de tíquete | Token de atualização primário |
|--|--|--|--|
| Conta local-Sim | Conta local-Sim | Conta local-não | Conta local-não |
| Conta MSA-Sim | Conta MSA-Sim | Conta MSA-não | Conta MSA-não |
| Conta unida do Azure AD – Sim | Conta unida do Azure AD – Sim | Conta unida do Azure AD – Sim (se híbrido) | Conta unida do Azure AD – Sim |
| Conta ingressada no domínio-Sim | Conta ingressada no domínio-Sim | Conta ingressada no domínio-Sim | Conta ingressada no domínio – Sim (se híbrido) |

### <a name="credential-guard-interaction"></a>Interação do Credential Guard

Se um dispositivo tiver o Credential Guard habilitado, os segredos derivados de um usuário serão criptografados com uma chave específica para a sessão de inicialização atual. Portanto, o ARSO não tem suporte atualmente em dispositivos com o Credential Guard habilitado.

## <a name="additional-resources"></a>Recursos adicionais

O logon automático é um recurso que está presente no Windows para várias versões. É um recurso documentado do Windows que até mesmo tem ferramentas como o logon automático para Windows [http:/technet. Microsoft. com/Sysinternals/bb963905. aspx](/sysinternals/downloads/autologon). Ele permite que um único usuário do dispositivo se conecte automaticamente sem inserir as credenciais. As credenciais são configuradas e armazenadas no registro como um segredo de LSA criptografado. Isso pode ser problemático para muitos casos filho em que o bloqueio de conta pode ocorrer entre o tempo e a ativação, especialmente se a janela de manutenção for normalmente durante esse tempo.
