---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoria de processo de linha de comando
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66ae6992775319cf614b0cb4c21f864150746687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859547"
---
# <a name="command-line-process-auditing"></a>Auditoria de processo de linha de comando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

**Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
## <a name="overview"></a>Visão geral  
  
-   O evento de auditoria da criação do processo pré-existente 4688 ID agora incluirá informações de auditoria para processos de linha de comando.  
  
-   Ele também registrará hash SHA1/2 do executável no log de eventos do Applocker  
  
    -   Aplicativos e serviços Logs\Microsoft\Windows\AppLocker  
  
-   Habilitar por meio do GPO, mas ela é desabilitada por padrão  
  
    -   "Incluir a linha de comando em eventos de criação de processo"  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura Figura SEQ \\ \* evento 16 árabe 4688**  
  
Revise o evento ID 4688 atualizado em REF _Ref366427278 \h Figura 16.  Antes disso, atualize nenhuma informação para **linha de comando de processo** é registrado em log.  Devido a esse registro em log adicional pode agora vemos que não só foi o wscript.exe início do processo, mas que também foi usado para executar um script do VB.  
  
## <a name="configuration"></a>Configuração  
Para ver os efeitos dessa atualização, você precisará habilitar duas configurações de política.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Você deve ter a criação do processo de auditoria Auditoria habilitada para ver a ID de evento 4688.  
Para habilitar a política de criação do processo de auditoria, edite a política de grupo a seguir:  
  
**Local da política:** Configuração do computador > Políticas > configurações do Windows > configurações de segurança > Configuração de auditoria avançada > detalhados de acompanhamento  
  
**Nome da política:** Auditoria de Criação de Processo  
  
**Suporte para:** Windows 7 e acima  
  
**Descrição/ajuda:**  
  
Essa configuração de política de segurança determina se o sistema operacional gera eventos de auditoria quando um processo é criado (início) e o nome do programa ou do usuário que o criou.  
  
Esses eventos de auditoria pode ajudá-lo a entender como um computador está sendo usado para controlar atividades do usuário.  
  
Volume de eventos: Baixo a médio, dependendo do uso do sistema  
  
**Padrão:** Não configurado  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver as adições à ID de evento 4688, você deve habilitar a nova configuração de política: Incluir a linha de comando em eventos de criação de processo  
**Tabela tabela SEQ \\ \* configuração de política do processo de linha de comando de 19 árabe**  
  
|Configuração de política|Detalhes|  
|------------------------|-----------|  
|**Path**|Criação de processo administrativo Templates\System\Audit|  
|**Configuração**|**Incluir a linha de comando em eventos de criação de processo**|  
|**Configuração padrão**|Não configurado (não ativado)|  
|**Suporte para:**|?|  
|**Descrição**|Essa configuração de política determina quais informações são registradas em eventos de auditoria de segurança quando um novo processo tiver sido criado.<br /><br />Essa configuração se aplica somente quando a política de criação do processo de auditoria está habilitada. Se você habilitar essa configuração as informações de linha de comando para cada processo será registrado em texto sem formatação no log de eventos de segurança como parte do evento 4688 criação do processo de auditoria de política, "um novo processo foi criado," em estações de trabalho e servidores nos quais essa política configuração é aplicada.<br /><br />Se você desabilitar ou não definir essa configuração de política, informações de linha de comando do processo não serão incluídas nos eventos de criação do processo de auditoria.<br /><br />Default: Não configurado<br /><br />Observação: Quando essa configuração de política está habilitada, qualquer usuário com acesso para ler que os eventos de segurança será capazes de ler os argumentos de linha de comando para qualquer um com êxito criou o processo. Argumentos de linha de comando podem conter informações confidenciais ou privadas, como senhas ou dados de usuário.|  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Ao usar configurações da Configuração de Política de Auditoria Avançada, você precisa confirmar que essas configurações não foram substituídas pelas configurações básicas de política de auditoria.  Evento 4719 é registrado quando as configurações serão substituídas.  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
O procedimento a seguir mostra como evitar conflitos, bloqueando a aplicação de quaisquer configurações de política de auditoria básicas.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para garantir que as configurações da Configuração de Política de Auditoria Avançada não foram substituídas  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Abra o console de gerenciamento de diretiva de grupo  
  
2.  Política de domínio padrão com o botão direito e, em seguida, clique em Editar.  
  
3.  Clique duas vezes em configuração do computador, clique duas vezes em políticas e, em seguida, clique duas vezes em configurações do Windows.  
  
4.  Clique duas vezes em configurações de segurança, clique duas vezes em diretivas locais e, em seguida, clique em Opções de segurança.  
  
5.  Clique duas vezes em auditoria: Forçar configurações de subcategorias de política de auditoria (Windows Vista ou posterior) para substituir configurações de categorias de política de auditoria e, em seguida, clique em definir essa configuração de política.  
  
6.  Clique em ativado e, em seguida, clique em Okey.  
  
## <a name="additional-resources"></a>Recursos adicionais  
[Criação do processo de auditoria](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guia passo a passo de política de auditoria de segurança avançada](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Perguntas frequentes](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Tente o seguinte: Explore a auditoria do processo de linha de comando  
  
1.  Habilitar **criação de processos de auditoria** eventos e verifique se a configuração de diretiva de auditoria avançada não será substituída.  
  
2.  Crie um script que gerará alguns eventos de interesse e execute o script.  Observe os eventos.  O script usado para gerar o evento na lição tinha esta aparência:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Habilitar a auditoria de processo de linha de comando  
  
4.  Executar o mesmo script como antes e observar os eventos  
  


