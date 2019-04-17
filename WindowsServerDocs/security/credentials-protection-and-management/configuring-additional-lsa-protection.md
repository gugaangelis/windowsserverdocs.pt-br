---
title: "Configurando a LSA adicional de proteção"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>Configurando a LSA adicional de proteção

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI explica como configurar a proteção adicional para o processo de autoridade de segurança Local (LSA) impedir a inclusão de código que poderia comprometer as credenciais.

A LSA, que inclui o processo de serviço de servidor de autoridade de segurança Local (LSASS), valida os usuários para locais e remotos acessos e impõe políticas de segurança local. O sistema operacional Windows 8.1 fornece proteção adicional para a LSA impedir que a memória de leitura e injeção de código por processos não protegidos. Isso proporciona segurança adicional para as credenciais que a LSA armazena e gerencia. A configuração de processo protegido para LSA pode ser configurada no Windows 8.1, mas ele não pode ser configurado no Windows RT 8.1. Quando essa configuração é usada em conjunto com a inicialização segura, proteção adicional é obtida porque desabilitando a chave do registro HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. não tem efeito.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisitos de processo protegido para plug-ins ou drivers
Para um plug-in a LSA ou driver carregar como um processo protegido com êxito, ele deverá atender aos seguintes critérios:

1.  Verificação de assinatura

    Modo protegido exige que qualquer plug-in que está carregado na LSA é assinado digitalmente com uma assinatura da Microsoft. Portanto, qualquer plug-ins que são assinados ou não são assinados com uma assinatura da Microsoft não poderá ser carregado no LSA. Exemplos desses plug-ins são drivers de cartão inteligente, criptográficos plug-ins e filtros de senha.

    A LSA plug-ins que são drivers, como drivers de cartão inteligente, precisam ser assinados usando a certificação WHQL. Para obter mais informações, consulte [assinatura de lançamento WHQL (Windows Drivers)](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    A LSA plug-ins que não têm um processo de certificação WHQL, devem ser assinados usando o [arquivo de assinatura de serviço para LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Conformidade com as diretrizes de processo do ciclo de vida de desenvolvimento de segurança da Microsoft (SDL)

    Todos os plug-ins devem estar em conformidade com as diretrizes de processo SDL aplicável. Para obter mais informações, consulte o [apêndice de ciclo de vida de desenvolvimento de segurança da Microsoft (SDL)](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Mesmo se os plug-ins corretamente são assinados com uma assinatura da Microsoft, não conformidade com o processo SDL pode resultar em falha para carregar um plug-in.

#### <a name="recommended-practices"></a>Práticas recomendadas
Use a lista a seguir para testar essa proteção LSA está habilitado antes de implantar amplamente o recurso:

-   Identifica todos os plug-ins LSA e drivers que estão sendo usados em sua organização. 
    Isso inclui drivers não são da Microsoft ou plug-ins, como drivers de cartão inteligente e criptográficos plug-ins e qualquer desenvolvidos internamente software que é usado para aplicar filtros de senha ou receber notificações de alteração de senha.

-   Certifique-se de que todos os plug-ins da LSA são assinados digitalmente com um certificado da Microsoft para que o plug-in não não será carregado.

-   Certifique-se de que todos os plug-ins assinados corretamente com êxito podem carregar em LSA e sua execução conforme o esperado.

-   Use os logs de auditoria para identificar a LSA plug-ins e drivers que não funcionará como um processo protegido.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Como identificar a LSA plug-ins e drivers que não funcionará como um processo protegido
Os eventos descritos nesta seção estão localizados na Operational log em Applications and Services Logs\Microsoft\Windows\CodeIntegrity. Eles podem ajudá-lo a identificar LSA plug-ins e drivers que não for possível carregar, devido a questões de assinatura. Para gerenciar esses eventos, você pode usar o **wevtutil** ferramenta de linha de comando. Para obter informações sobre essa ferramenta, consulte [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Antes de optar em: como identificar o plug-ins e drivers carregados pelo lsass.exe
Você pode usar o modo de auditoria para identificar a LSA plug-ins e drivers que não serão carregado no modo de proteção da LSA. Enquanto estiver no modo de auditoria, o sistema irá gerar logs de eventos, identificando todos os plug-ins e drivers que não serão carregado em LSA se proteção LSA está habilitada. As mensagens são registradas sem bloquear o plug-ins ou drivers.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Para habilitar o modo de auditoria para Lsass.exe em um único computador editando o registro

1.  Abra o Editor do registro (RegEdit.exe) e navegue até a chave do registro que está localizada em: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image arquivo execução Options\LSASS.exe.

2.  Defina o valor da chave do registro para **AuditLevel = dword:00000008**.

3.  Reinicie o computador.

Analise os resultados do evento 3065 e evento 3066.

-   **Evento 3065**: esse evento registra que a integridade de código verificar determinou que um processo (geralmente lsass.exe) tentou carregar um driver específico que não atendeu aos requisitos de segurança para seções compartilhadas. No entanto, devido a política de sistema que é definida, a imagem foi permissão para ser carregada.

-   **Evento 3066**: esse evento registra que a integridade de código verificar determinou que um processo (geralmente lsass.exe) tentou carregar um driver específico que não atendeu a requisitos de nível de assinatura da Microsoft. No entanto, devido a política de sistema que é definida, a imagem foi permissão para ser carregada.

> [!IMPORTANT]
> Esses eventos operacionais não são gerados quando um depurador de kernel está conectado e habilitado em um sistema.
> 
> Se um plug-in ou driver contém seções compartilhadas, 3066 de evento é registrado com o evento 3065. Remover as seções compartilhadas deve impedir que ambos os eventos ocorra, a menos que o plug-in não atende a requisitos de nível de assinatura da Microsoft.

Para habilitar o modo de auditoria para vários computadores em um domínio, você pode usar a extensão do lado do cliente do registro para política de grupo para implantar o valor de registro de nível de auditoria Lsass.exe. Você precisa modificar a chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image arquivo execução Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Para criar a configuração de valor AuditLevel em um GPO

1.  Abra o Console de gerenciamento de política de grupo (GPMC).

2.  Crie uma nova política de grupo objeto (GPO) que é vinculado no nível do domínio ou que está vinculado à unidade organizacional que contém as suas contas de computador. Ou você pode selecionar um GPO que já tenha sido implantado.

3.  Clique com botão direito no GPO e clique em **editar** para abrir o Editor de gerenciamento de política de grupo.

4.  Expanda **configuração do computador**, expanda **preferências**e, em seguida, expanda **configurações do Windows**.

5.  Clique com botão direito **registro**, aponte para **nova**e clique em **Item do registro**. O **novas propriedades de registro** caixa de diálogo aparece.

6.  No **seção** listar, clique em **HKEY_LOCAL_MACHINE.**

7.  No **Key Path** listar, navegue até **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image arquivo execução Options\LSASS.exe**.

8.  No **nome do valor**, digite **AuditLevel**.

9. No **tipo do valor** box, clique para selecionar o **REG_DWORD**.

10. No **dados do valor**, digite **00000008**.

11. Clique em **Okey**.

> [!NOTE]
> Para o GPO entrem em vigor, a alteração de GPO deve ser replicada para todos os controladores de domínio no domínio.

Para aceitar-in de proteção adicional da LSA em vários computadores, você pode usar a extensão do lado do cliente do registro para política de grupo modificando HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.. Para obter etapas sobre como fazer isso, consulte [como configurar a proteção de LSA adicional de credenciais](#BKMK_HowToConfigure) neste tópico.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Após recusando-no: como identificar o plug-ins e drivers carregados pelo lsass.exe
Você pode usar o log de eventos para identificar a LSA plug-ins e drivers que não conseguiu carregar no modo de proteção da LSA. Quando o processo da LSA protegida está habilitado, o sistema gera os logs de eventos que identificar todos os plug-ins e drivers que não conseguiu carregar em LSA.

Analise os resultados do evento 3033 e 3063 de evento.

-   **Evento 3033**: esse evento registra que a integridade de código verificar determinou que um processo (geralmente lsass.exe) tentou carregar um driver que não atendeu a requisitos de nível de assinatura da Microsoft.

-   **Evento 3063**: esse evento registra que a integridade de código verificar determinou que um processo (geralmente lsass.exe) tentou carregar um driver que não atendeu aos requisitos de segurança para seções compartilhadas.

Seções compartilhadas geralmente são o resultado de técnicas de programação que permitem que os dados de instância interagir com outros processos que usam o mesmo contexto de segurança. Isso pode criar vulnerabilidades de segurança.

## <a name="BKMK_HowToConfigure"></a>Como configurar a proteção de LSA adicional de credenciais
Em dispositivos que executam o Windows 8.1 (com ou sem a inicialização segura ou da UEFI), configuração é possível seguindo os procedimentos descritos nesta seção. Para dispositivos que executam o Windows RT 8.1, proteção lsass.exe está sempre habilitada e não pode ser desativado.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>Em dispositivos baseados em x86 ou com base em x64 ou não, usando a inicialização segura e UEFI
Em dispositivos baseados em x86 ou com base em x64 que usam a inicialização segura e UEFI, uma variável UEFI é definida no firmware UEFI quando proteção LSA é ativada usando a chave do registro. Quando a configuração é armazenada no firmware, a variável UEFI não pode ser excluída ou alterada na chave do registro. A variável UEFI deve ser redefinida.

Baseado em x64 ou x86 dispositivos que não dão suporte a UEFI ou a inicialização segura estiverem desabilitados, não é possível armazenar a configuração para proteção da LSA no firmware e dependa exclusivamente a presença da chave do registro. Nesse cenário, é possível desativar a proteção de LSA usando o acesso remoto ao dispositivo.

Você pode usar os procedimentos a seguir para habilitar ou desabilitar proteção LSA:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Para habilitar a proteção de LSA em um único computador

1.  Abra o Editor do registro (RegEdit.exe) e navegue até a chave do registro que está localizada em: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa..

2.  Defina o valor da chave do registro para: "RunAsPPL" = DWORD: 00000001.

3.  Reinicie o computador.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Para habilitar a proteção de LSA usando política de grupo

1.  Abra o Console de gerenciamento de política de grupo (GPMC).

2.  Crie um novo GPO vinculado no nível do domínio ou que está vinculado à unidade organizacional que contém as suas contas de computador. Ou você pode selecionar um GPO que já tenha sido implantado.

3.  Clique com botão direito no GPO e clique em **editar** para abrir o Editor de gerenciamento de política de grupo.

4.  Expanda **configuração do computador**, expanda **preferências**e, em seguida, expanda **configurações do Windows**.

5.  Clique com botão direito **registro**, aponte para **nova**e clique em **Item do registro**. O **novas propriedades de registro** caixa de diálogo aparece.

6.  No **seção** listar, clique em **HKEY_LOCAL_MACHINE**.

7.  No **Key Path** listar, navegue até **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  No **nome do valor**, digite **RunAsPPL**.

9. No **tipo do valor**, clique no **REG_DWORD**.

10. No **dados do valor**, digite **00000001**.

11. Clique em **Okey**.

##### <a name="to-disable-lsa-protection"></a>Para desativar a proteção de LSA

1.  Abra o Editor do registro (RegEdit.exe) e navegue até a chave do registro que está localizada em: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa..

2.  Exclua o seguinte valor da chave do registro: "RunAsPPL" = DWORD: 00000001.

3.  Use a ferramenta de autoridade de segurança Local (LSA) processo protegido recusa para excluir a variável UEFI se o dispositivo está usando a inicialização segura.

    Para obter mais informações sobre a ferramenta de recusa, consulte [autoridade baixar de segurança Local (LSA) processo protegido recusa do Centro de Download da Microsoft oficial](https://www.microsoft.com/download/details.aspx?id=40897).

    Para obter mais informações sobre como gerenciar a inicialização segura, consulte [Firmware UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Quando a inicialização segura está desativada, são redefinidas todas as configurações relacionadas ao UEFI e inicialização segura. Você deve desativar a inicialização segura somente quando todos os outros meios para desativar a proteção de LSA falharam.

### <a name="verifying-lsa-protection"></a>Verificando proteção LSA
Para descobrir se LSA foi iniciada no modo protegido quando o Windows é iniciado, procure o seguinte evento WinInit no **sistema** fazer logon em **Logs do Windows**:

-   12: LSASS.exe foi iniciado como um processo protegido com nível de: 4

## <a name="additional-resources"></a>Recursos adicionais
[Gerenciamento e proteção de credenciais](credentials-protection-and-management.md)

[Arquivo de assinatura de serviço para LSA](https://go.microsoft.com/fwlink/?LinkId=392590)


