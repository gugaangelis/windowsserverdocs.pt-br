---
title: Configurando a proteção LSA adicional
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: eaebac19119525b659c09b5506c497afdbd9a263
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386995"
---
# <a name="configuring-additional-lsa-protection"></a>Configurando a proteção LSA adicional

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de TI explica como configurar proteção adicional para o processo de LSA (Autoridade de Segurança Local), a fim de impedir a injeção de código que poderá comprometer credenciais.

A LSA, que inclui o processo LSASS (Serviço do Servidor de Autoridade de Segurança Local), valida usuários para entradas remotas e locais e força as políticas de segurança local. O sistema operacional Windows 8.1 fornece proteção adicional para a LSA para evitar a leitura de memória e injeção de código por processos não protegidos. Isso proporciona mais segurança para as credenciais que a LSA armazena e gerencia. A configuração de processo protegido para LSA pode ser configurada no Windows 8.1, mas não pode ser configurada no Windows RT 8,1. Quando essa configuração é usada em conjunto com a Inicialização Segura, proteção adicional é obtida, pois a desabilitação da chave de Registro HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa não tem efeito.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisitos do processo protegido para plug-ins ou drivers
Para um plug-in ou driver de LSA carregar com êxito como um processo protegido, ele deve atender aos seguintes critérios:

1.  Verificação de assinatura

    O modo protegido exige que qualquer plug-in carregado na LSA tenha uma assinatura digital da Microsoft. Portanto, os plug-ins não assinados ou não assinados com uma assinatura da Microsoft não carregarão na LSA. Exemplos desses plug-ins são os drivers de cartão inteligente, os plug-ins criptográficos e os filtros de senha.

    Os plug-ins de LSA que são drivers, como os drivers de cartão inteligente, precisam ser assinados usando a Certificação WHQL. Para obter mais informações, consulte [assinatura de versão do WHQL](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    Os plug-ins de LSA que não têm um processo de Certificação de WHQL devem ser assinados usando o [serviço de assinatura de arquivo para LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Aderência à orientação do processo SDL (Security Development Lifecycle) da Microsoft

    Todos os plug-ins devem estar em conformidade com a orientação do processo SDL aplicável. Para obter mais informações, consulte [Microsoft Security Development Lifecycle (SDL) Appendix](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Mesmo se os plug-ins estiverem adequadamente assinados com uma assinatura da Microsoft, a não conformidade com o processo SDL pode resultar em falha no carregamento de um plug-in.

#### <a name="recommended-practices"></a>Práticas recomendadas
Use a lista a seguir para testar completamente se essa proteção LSA está habilitada antes de você implantar amplamente o recurso:

-   Identifique todos os plug-ins e drivers de LSA que estão em uso em sua organização. 
    Isso inclui drivers ou plug-ins não Microsoft, como drivers de cartão inteligente e plug-ins criptográficos, bem como qualquer software desenvolvido internamente que seja usado para forçar filtros de senha ou notificações de mudança de senha.

-   Verifique se todos os plug-ins de LSA estão digitalmente assinados com um certificado da Microsoft, de forma que o carregamento do plug-in não falhe.

-   Verifique se todos os plug-ins assinados corretamente podem ser carregados com êxito na LSA e se eles são executados conforme esperado.

-   Use os logs de auditoria para identificar plug-ins e drivers de LSA que não são executados como um processo protegido.

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>Limitações introduzidas com a proteção LSA habilitada

Se a proteção de LSA estiver habilitada, você não poderá depurar um plug-in LSA personalizado.
Não é possível anexar um depurador ao LSASs quando ele é um processo protegido.
Em geral, não há nenhuma maneira com suporte para depurar um processo protegido em execução.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Como identificar plug-ins e drivers de LSA que não são executados como um processo protegido
Os eventos descritos nesta seção estão localizados no log Operacional em Logs de Aplicativos e Serviços\Microsoft\Windows\CodeIntegrity. Eles podem ajudá-lo a identificar os plug-ins e drivers de LSA que não estão carregando devido a motivos de assinatura. Para gerenciar esses eventos, você pode usar a ferramenta de linha de comando **wevtutil**. Para obter informações sobre essa ferramenta, consulte [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Antes de aceitar: Como identificar plug-ins e drivers carregados pelo lsass.exe
Você pode usar o modo de auditoria para identificar plug-ins e drivers de LSA que não carregarão no modo Proteção de LSA. Enquanto estiver no modo de auditoria, o sistema gerará logs de eventos, identificando todos os plug-ins e drivers que não carregarão em LSA se a Proteção de LSA estiver habilitada. As mensagens são registradas sem bloquear os plug-ins ou drivers.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Para habilitar o modo de auditoria para Lsass.exe em um único computador editando o Registro

1.  Abra o Editor de Registro (RegEdit.exe) e navegue para a chave do Registro localizada em: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

2.  Defina o valor da chave de Registro como **AuditLevel=dword:00000008**.

3.  Reinicie o computador.

Analise os resultados dos eventos 3065 e 3066.

Depois disso, você poderá ver esses eventos em Visualizador de Eventos: Microsoft-Windows-CodeIntegrity/operacional:

-   **Evento 3065**: Este evento registra que uma verificação de integridade de código determinou que um processo (geralmente, lsass.exe) tentou carregar um driver específico que não atendeu aos requisitos de segurança para Seções Compartilhadas. No entanto, devido à política de sistema definida, foi permitido o carregamento da imagem.

-   **Evento 3066**: Este evento registra que uma verificação de integridade de código determinou que um processo (geralmente, lsass.exe) tentou carregar um driver específico que não atendeu aos requisitos de nível de assinatura da Microsoft. No entanto, devido à política de sistema definida, foi permitido o carregamento da imagem.

> [!IMPORTANT]
> Esses eventos operacionais não são gerados quando um depurador de kernel está conectado e habilitado em um sistema.
> 
> Se um plug-in ou driver contiver Seções Compartilhadas, o Evento 3066 será registrado com o Evento 3065. A remoção das Seções Compartilhadas deve impedir que os dois eventos ocorram, a menos que o plug-in não atenda aos requisitos de nível de assinatura da Microsoft.

Para habilitar o modo de auditoria para vários computadores em um domínio, é possível usar a Extensão do Lado do Cliente do Registro para Política de Grupo para implantar o valor do Registro de nível de auditoria Lsass.exe. Você precisa modificar a chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Para criar a configuração do valor AuditLevel em um GPO

1.  Abra o GPMC (Console de Gerenciamento de Diretiva de Grupo).

2.  Crie um novo GPO (Objeto de Política de Grupo) que esteja vinculado no nível de domínio ou à unidade organizacional que contém as contas do computador. Ou então, você pode selecionar um GPO que já esteja implantado.

3.  Clique com o botão direito do mouse no GPO e clique em **Editar** para abrir o Editor de Gerenciamento de Política de Grupo.

4.  Expanda **Configuração do Computador**, **Preferências** e **Configurações do Windows**.

5.  Clique com o botão direito do mouse em **Registro**, aponte para **Novo**e clique em **Item do Registro**. A caixa de diálogo **Novas Propriedades do Registro** é exibida.

6.  Na lista **Hive** , clique em **HKEY_LOCAL_MACHINE.**

7.  Na lista **Caminho da Chave** , procure **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**.

8.  Na caixa **Nome do valor**, digite **AuditLevel**.

9. Na caixa **Tipo de valor**, clique para selecionar o **REG_DWORD**.

10. Na caixa **Dados do valor** , digite **00000008**.

11. Clique em **OK**.

> [!NOTE]
> Para o GPO ser efetivado, a sua alteração deve ser replicada para todos os controladores de domínio no domínio.

Para aceitar a proteção de LSA adicional em vários computadores, você pode usar a Extensão do Lado do Cliente do Registro para Política de Grupo modificando HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Para obter etapas sobre como fazer isso, consulte [Como configurar a proteção de LSA adicional de credenciais](#BKMK_HowToConfigure) neste tópico.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Depois de aceitar: Como identificar plug-ins e drivers carregados pelo lsass.exe
Você pode usar o registro de eventos para identificar plug-ins e drivers de LSA que não carregaram no modo Proteção de LSA. Quando o processo protegido por LSA está habilitado, o sistema gera logs de eventos que identificam todos os plug-ins e drivers que não carregaram na LSA.

Analise os resultados dos Eventos 3033 e 3063.

Depois disso, você poderá ver esses eventos em Visualizador de Eventos: Microsoft-Windows-CodeIntegrity/operacional:

-   **Evento 3033**: Este evento registra que uma verificação de integridade de código determinou que um processo (geralmente, lsass.exe) tentou carregar um driver que não atendeu aos requisitos de nível de assinatura da Microsoft.

-   **Evento 3063**: Este evento registra que uma verificação de integridade de código determinou que um processo (geralmente, lsass.exe) tentou carregar um driver que não atendeu aos requisitos de segurança para Seções Compartilhadas.

As Seções Compartilhadas geralmente são o resultado de técnicas de programação que permitem que os dados da instância interajam com outros processos que usam o mesmo contexto de segurança. Isso pode criar vulnerabilidades de segurança.

## <a name="BKMK_HowToConfigure"></a>Como configurar a proteção de credenciais de LSA adicional
Em dispositivos que executam o Windows 8.1 (com ou sem inicialização segura ou UEFI), a configuração é possível executando os procedimentos descritos nesta seção. Para dispositivos que executam o Windows RT 8,1, a proteção LSASS. exe está sempre habilitada e não pode ser desativada.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>Nos dispositivos baseados em x86 ou x64 que usam Inicialização Segura e UEFI ou não
Nos dispositivos baseados em x86 ou x64 que usam a Inicialização Segura e UEFI, uma variável de UEFI é definida no firmware da UEFI quando a proteção de LSA é habilitada usando a chave de Registro. Quando a configuração está armazenada no firmware, a variável de UEFI não pode ser excluída ou alterada na chave de Registro. A variável de UEFI deve ser redefinida.

Os dispositivos baseados em x86 ou x64 que não têm suporte para UEFI ou Inicialização Segura estão desabilitados, não podem armazenar a configuração para a proteção de LSA no firmware e contam exclusivamente com a presença da chave de Registro. Neste cenário, é possível desabilitar a proteção de LSA usando o acesso remoto ao dispositivo.

Você pode usar os procedimentos a seguir para habilitar ou desabilitar a proteção de LSA:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Para habilitar a proteção de LSA em um único computador

1.  Abra o Editor de Registro (RegEdit.exe) e navegue para a chave do Registro localizada em: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Defina o valor da chave do Registro como: "RunAsPPL"=dword:00000001.

3.  Reinicie o computador.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Para habilitar a proteção de LSA usando a Política de Grupo

1.  Abra o GPMC (Console de Gerenciamento de Diretiva de Grupo).

2.  Crie um novo GPO que esteja vinculado no nível de domínio ou à unidade organizacional que contém as contas do computador. Ou então, você pode selecionar um GPO que já esteja implantado.

3.  Clique com o botão direito do mouse no GPO e clique em **Editar** para abrir o Editor de Gerenciamento de Política de Grupo.

4.  Expanda **Configuração do Computador**, **Preferências** e **Configurações do Windows**.

5.  Clique com o botão direito do mouse em **Registro**, aponte para **Novo**e clique em **Item do Registro**. A caixa de diálogo **Novas Propriedades do Registro** é exibida.

6.  Na lista **Hive** , clique em **HKEY_LOCAL_MACHINE**.

7.  Na lista **Caminho da Chave** , procure **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  Na caixa **Nome do valor**, digite **RunAsPPL**.

9. Na caixa **Tipo de valor**, clique em **REG_DWORD**.

10. Na caixa **Dados do valor** , digite **00000001**.

11. Clique em **OK**.

##### <a name="to-disable-lsa-protection"></a>Para desabilitar a proteção de LSA

1.  Abra o Editor de Registro (RegEdit.exe) e navegue para a chave do Registro localizada em: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Exclua o seguinte valor da chave do Registro: "RunAsPPL"=dword:00000001.

3.  Use a ferramenta Recusa de Processo Protegido da LSA (Autoridade de Segurança Local) para excluir a variável de UFEI se o dispositivo estiver usando Inicialização Segura.

    Para obter mais informações sobre a ferramenta de recusa, consulte [Baixar Protected Process Opt-out de LSA (Autoridade de Segurança Local) do Centro Oficial de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=40897).

    Para obter mais informações sobre o gerenciamento de Inicialização Segura, consulte [Firmware de UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Quando a Inicialização Segura está desativada, todas as configurações relacionadas à Inicialização Segura e à UEFI são redefinidas. Você deve desativar a Inicialização Segura somente quando todos os outros meios para desabilitar a proteção de LSA falharem.

### <a name="verifying-lsa-protection"></a>Verificando a proteção de LSA
Para descobrir se a LSA foi iniciada no modo protegido quando o Windows foi iniciado, procure o seguinte evento WinInit no log **Sistema** em **Logs do Windows**:

-   12: LSASS.exe foi iniciado como um processo protegido com nível: 4

## <a name="additional-resources"></a>Recursos adicionais
[Proteção e gerenciamento de credenciais](credentials-protection-and-management.md)

[Serviço de assinatura de arquivo para LSA](https://go.microsoft.com/fwlink/?LinkId=392590)


