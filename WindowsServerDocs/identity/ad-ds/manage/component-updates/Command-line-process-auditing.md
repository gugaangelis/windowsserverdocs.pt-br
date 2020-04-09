---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoria de processo de linha de comando
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dc6cba306a36589d8b585b23ecb43e7d16b7d201
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823069"
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
  
**Figura SEQ figura \\\* árabe 16 evento 4688**  
  
Examine a ID de evento 4688 atualizada em REF _Ref366427278 \h Figura 16.  Antes dessa atualização, nenhuma das informações de **linha de comando do processo** é registrada.  Por causa desse log adicional, agora podemos ver que não só o processo WScript. exe foi iniciado, mas também foi usado para executar um script VB.  
  
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
  
Volume de eventos: de baixo para médio, dependendo do uso do sistema  
  
**Padrão:** Não configurado  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver as adições à ID de evento 4688, você deve habilitar a nova configuração de política: incluir linha de comando em eventos de criação de processo  
**Tabela SEQ tabela \\\* configuração de política de processo de linha de comando árabe 19**  
  
|Configuração de política|Detalhes|  
|------------------------|-----------|  
|**Multi-Path**|Criação de processo de Templates\System\Audit administrativo|  
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
  
6.  Clique em habilitado e em OK.  
  
## <a name="additional-resources"></a>Recursos adicionais  
[Criação de processo de auditoria](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guia passo a passo da política de auditoria de segurança avançada](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: perguntas frequentes](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
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
  


