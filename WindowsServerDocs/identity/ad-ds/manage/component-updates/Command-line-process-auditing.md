---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoria de processo de linha de comando
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1499927a0ecc01255e5da5f9c0fff2ea064d4b5e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943451"
---
# <a name="command-line-process-auditing"></a>Auditoria de processo de linha de comando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

**Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows

> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.

## <a name="overview"></a>Visão geral

-   A ID do evento 4688 de auditoria de criação de processo já existente incluirá informações de auditoria para processos de linha de comando.

-   Ele também registrará o hash SHA1/2 do executável no log de eventos do AppLocker

    -   Logs\Microsoft\Windows\AppLocker de aplicativos e serviços

-   Você habilita por meio de GPO, mas está desabilitada por padrão

    -   "Incluir linha de comando em eventos de criação de processo"

![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)

**Figura SEQ figura \\ \* árabe 16 evento 4688**

Examine a ID de evento 4688 atualizada em REF _Ref366427278 \h Figura 16.  Antes dessa atualização, nenhuma das informações de **linha de comando do processo** é registrada.  Por causa desse log adicional, agora podemos ver que não só o processo de wscript.exe foi iniciado, mas também foi usado para executar um script VB.

## <a name="configuration"></a>Configuração
Para ver os efeitos dessa atualização, você precisará habilitar duas configurações de política.

### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Você deve ter a auditoria de criação do processo de auditoria habilitada para ver a ID do evento 4688.
Para habilitar a política de criação de processo de auditoria, edite a seguinte política de Grupo:

**Local da política:** Configuração do computador > políticas > configurações do Windows > configurações de segurança > configuração de auditoria avançada > controle detalhado

**Nome da política:** Criação de processo de auditoria

**Com suporte em:** Windows 7 e posterior

**Descrição/ajuda:**

Essa configuração de política de segurança determina se o sistema operacional gera eventos de auditoria quando um processo é criado (inicia) e o nome do programa ou usuário que o criou.

Esses eventos de auditoria podem ajudá-lo a entender como um computador está sendo usado e controlar a atividade do usuário.

Volume do evento: baixo para médio, dependendo do uso do sistema

**Padrão:** Não configurado

### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver as adições à ID de evento 4688, você deve habilitar a nova configuração de política: incluir linha de comando em eventos de criação de processo
**Configuração da política de processo de linha de comando da tabela SEQ \\ \* árabe 19**

|Configuração de política|Detalhes|
|------------------------|-----------|
|**Caminho**|Criação de processo de Templates\System\Audit administrativo|
|**Configuração**|**Incluir linha de comando em eventos de criação de processo**|
|**Configuração padrão**|Não configurado (não habilitado)|
|**Com suporte em:**|?|
|**Descrição**|Essa configuração de política determina quais informações são registradas nos eventos de auditoria de segurança quando um novo processo é criado.<p>Essa configuração se aplica somente quando a política de criação de processo de auditoria está habilitada. Se você habilitar essa configuração de política, as informações de linha de comando para cada processo serão registradas em texto sem formatação no log de eventos de segurança como parte do evento de criação de processo de auditoria 4688, "um novo processo foi criado" nas estações de trabalho e servidores nos quais essa configuração de política é aplicada.<p>Se você desabilitar ou não definir essa configuração de política, as informações de linha de comando do processo não serão incluídas nos eventos de criação do processo de auditoria.<p>Padrão: não configurado<p>Observação: quando essa configuração de política estiver habilitada, qualquer usuário com acesso para ler os eventos de segurança poderá ler os argumentos de linha de comando para qualquer processo criado com êxito. Argumentos de linha de comando podem conter informações confidenciais ou privadas, como senhas ou dados de usuário.|

![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)

Ao usar configurações da Configuração de Política de Auditoria Avançada, você precisa confirmar que essas configurações não foram substituídas pelas configurações básicas de política de auditoria.  O evento 4719 é registrado quando as configurações são substituídas.

![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)

O procedimento a seguir mostra como evitar conflitos, bloqueando a aplicação de quaisquer configurações de política de auditoria básicas.

### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para garantir que as configurações da Configuração de Política de Auditoria Avançada não foram substituídas
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)

1.  Abra o console de gerenciamento do Política de Grupo

2.  Clique com o botão direito do mouse em Diretiva de Domínio Padrão e clique em Editar.

3.  Clique duas vezes em Configuração do Computador, clique duas vezes em Políticas e clique duas vezes em Configurações do Windows.

4.  Clique duas vezes em configurações de segurança, clique duas vezes em políticas locais e em opções de segurança.

5.  Clique duas vezes em Auditoria: forçar configurações de subcategorias de diretivas de auditoria (Windows Vista ou superior) para substituir configurações de categorias de diretivas de auditoria e clique em Definir esta configuração de política.

6.  Clique em Habilitadoe em OK.

## <a name="additional-resources"></a>Recursos adicionais
[Auditoria do processo de criação](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941613(v=ws.10))

[Guia Passo a Passo de Política de Auditoria de Segurança Avançada](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd408940(v=ws.10))

[AppLocker: perguntas frequentes](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619725(v=ws.10))

## <a name="try-this-explore-command-line-process-auditing"></a>Experimente: explorar a auditoria do processo de linha de comando

1.  Habilitar eventos de **criação de processo de auditoria** e garantir que a configuração de política de auditoria antecipada não seja substituída

2.  Crie um script que irá gerar alguns eventos de interesse e executar o script.  Observe os eventos.  O script usado para gerar o evento na lição ficou assim:

    ```
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs
    del c:\systemfiles\temp\*.* /Q
    ```

3.  Habilitar a auditoria do processo de linha de comando

4.  Executar o mesmo script anterior e observar os eventos

