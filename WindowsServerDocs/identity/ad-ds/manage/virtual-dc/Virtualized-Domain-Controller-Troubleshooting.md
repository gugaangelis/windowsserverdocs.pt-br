---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: "Virtualizados domínio solução de problemas do controlador"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Virtualizados domínio solução de problemas do controlador

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece uma metodologia detalhada sobre o recurso de controlador de domínio virtualizado de solução de problemas.  
  
-   [Solução de problemas virtualizados clonagem de controlador de domínio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Solução de problemas de restauração segura de controlador de domínio virtualizado](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introdução  
A maneira mais importante de melhorar suas habilidades de solução de problemas é um laboratório de teste de compilação e rigorosamente examinar normal, trabalhando cenários. Se você encontrar erros, eles são mais nítido e fácil de entender, desde que você tem uma base sólida de como funciona a promoção de controlador de domínio. Isso também permite que você crie suas habilidades de análise de análise e rede. Isso vai para todas as tecnologias de sistemas distribuídos, implantação de controlador de domínio não apenas virtualizado.  
  
Os elementos fundamentais para solução de problemas avançada de configuração de controlador de domínio são:  
  
1.  Análise linear combinadas com foco e atenção aos detalhes.  
  
2.  Noções básicas sobre a análise de captura de rede  
  
3.  Noções básicas sobre os logs internos  
  
São primeiro e segundo além do escopo deste tópico, mas o terceiro pode ser explicado em detalhes. Solução de problemas de controlador de domínio virtualizado requer um método lógico e linear. A chave é abordar o problema usando os dados fornecidos e apenas recorrer às ferramentas complexas e análise quando houver nenhuma saída fornecida e registrar em log.  
  
## <a name="BKMK_TshootVDCCloning"></a>Solução de problemas virtualizados clonagem de controlador de domínio  
Isso seções capas:  
  
-   [Ferramentas para solução de problemas](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Opções de registro em log](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Metodologia geral para solução de problemas de domínio clonagem do controlador](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Núcleo do servidor e o Log de eventos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Solução de problemas específicos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
A estratégia de solução de problemas de clonagem de controlador de domínio virtualizado segue este formato geral:  
  
![Solução de problemas de dc virtual](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Ferramentas para solução de problemas  
  
#### <a name="BKMK_LoggingOptions"></a>Opções de registro em log  
Os logs internos serão a ferramenta mais importante para solução de problemas com a clonagem do controlador de domínio. Todos esses logs são habilitados e configurados para máximo detalhamento, por padrão.  
  
|||  
|-|-|  
|**Operação**|**Log**|  
|**Clonagem**|-Evento viewer\Windows logs\System<br />-Evento viewer\Applications e serviços logs\Directory serviço<br />-%systemroot%\debug\dcpromo.log|  
|**Promoção**|-%systemroot%\debug\dcpromo.log<br />-Evento viewer\Applications e serviços logs\Directory serviço<br />-Evento viewer\Windows logs\System<br />-Evento viewer\Applications e serviços logs\File replicação do serviço<br />-Evento viewer\Applications e serviços logs\DFS replicação|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e os comandos de solução de problemas de configuração do controlador de domínio  
Para solucionar problemas não explicados pelos logs, use as ferramentas a seguir como ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 Do Monitor de rede  
  
### <a name="BKMK_GeneralMethodology"></a>Metodologia geral para solução de problemas de domínio clonagem do controlador  
  
1.  A VM está sendo inicializado no modo de reparo DS (DSRM)? Isso indica a solução de problemas é necessária. Para fazer logon no DSRM, use **. \Administrator** da conta e especifique a senha DSRM.  
  
    1.  Examine Dcpromo.log.  
  
        1.  Oferecia etapas iniciais de clonagem ter sucesso, mas promoção de controlador de domínio falha?  
  
        2.  Erros de indicar problemas com o controlador de domínio local ou com o ambiente AD DS, como erros retornados do emulador do PDC?  
  
    2.  Examinar os logs de eventos do sistema e os serviços de diretório e os dccloneconfig.xml e CustomDCCloneAllowList.xml  
  
        1.  Um aplicativo incompatível precisa estar no CustomDCCloneAllowList.xml lista de permissões?  
  
        2.  É o nome do computador ou o endereço IP duplicado ou inválido no dccloneconfig.xml?  
  
        3.  Site do Active Directory é inválido no dccloneconfig.xml?  
  
        4.  O endereço IP não é definido no dccloningconfig.xml e nenhum servidor DHCP está disponível?  
  
        5.  É o emulador do PDC online e disponível por meio do protocolo RPC?  
  
        6.  O controlador de domínio é um membro do grupo controladores de domínio Cloneable? Esta é a permissão **permitem que um controlador de domínio criar um clone de si** definido na raiz do domínio para esse grupo?  
  
        7.  O arquivo Dccloneconfig.xml conter erros de sintaxe que impedem que análise correto?  
  
        8.  O hipervisor é compatível?  
  
        9. Fez falha de promoção de controlador de domínio depois clonagem foi iniciada com êxito?  
  
        10. Foi o número máximo de nomes de controlador de domínio gerado automaticamente (9999) excedido?  
  
        11. O endereço MAC está duplicado?  
  
2.  É o nome de host do clone o mesmo que o controlador de domínio de origem?  
  
    1.  Há um arquivo Dccloneconfig.xml em um dos locais permitidos?  
  
3.  É a VM inicializar no modo normal e clonagem concluída, mas o controlador de domínio não está funcionando corretamente?  
  
    1.  Primeiro, verifique se o nome do host é alterado no clone. Se o nome do host é diferente, clonagem parcialmente foi concluída.  
  
    2.  O controlador de domínio tem um endereço IP duplicado do controlador de domínio de origem do dccloneconfig.xml, mas o controlador de domínio de origem estava offline durante clonagem?  
  
    3.  Se o controlador de domínio é publicidade, trate o problema como qualquer problema de pós-promoção normal, que você teria sem clonagem.  
  
    4.  Se o controlador de domínio não é publicidade, examine os logs de eventos de serviço de diretório, sistema, aplicativos, duplicação de arquivos e replicação DFS para pós-promoção erros.  
  
#### <a name="disabling-dsrm-boot"></a>Desabilitando a inicialização DSRM  
Quando inicializado no DSRM devido a qualquer erro, diagnosticar a causa falha e se dcpromo.log não indica que a clonagem não pode ser repetida, corrija a causa falha e redefinir o sinalizador DSRM. Um clone com falha não retornar ao modo normal por conta própria na próxima reinicialização; Você deve remover o sinalizador de inicialização do modo de Restauração DS para tentar clonagem novamente. Todas essas etapas exigem a execução como um administrador com privilégios elevados.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Removendo DSRM com Msconfig.exe  
Para ativar a inicialização DSRM desativadas usando uma interface gráfica do usuário, use a ferramenta de configuração do sistema:  
  
1.  Execute msconfig.exe  
  
2.  No **inicialização** guia em **opções de inicialização**, desmarque **inicialização segura** (ele já está marcado com a opção **reparo do Active Directory** habilitado)  
  
3.  Clique em Okey e reinicie quando solicitado  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Removendo DSRM com Bcdedit.exe  
Para desativar a inicialização DSRM na linha de comando, use o Editor de loja de dados de configuração de inicialização:  
  
1.  Abra um CMD prompt e execute:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Reinicie o computador com:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe também funciona em um console do Windows PowerShell. Os comandos há:  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  
  
### <a name="BKMK_ServerCoreEvents"></a>Núcleo do servidor e o Log de eventos  
Os logs de eventos conter muitas das informações úteis sobre operações de clonagem de controlador de domínio virtualizado. Por padrão, a instalação em um computador Windows Server 2012 é uma instalação Server Core, o que significa que não há nenhuma interface gráfica e, portanto, não há maneira de executar o snap-in do Visualizador de eventos local.  
  
Para examinar os logs de eventos em um servidor que executa uma instalação Server Core:  
  
-   Execute a ferramenta de Wevtutil.exe localmente  
  
-   Execute o cmdlet do PowerShell Get-WinEvent localmente  
  
-   Se você tiver ativado as regras de Firewall avançada do Windows para os grupos de "Gerenciamento remoto do Log de eventos" (ou equivalente portas) permitir a comunicação de entrada, você pode gerenciar o log de eventos remotamente usando Eventvwr.exe, wevtutil.exe ou Get-Winevent. Isso pode ser feito em instalação Server Core usando NETSH.exe, a política de grupo ou o cmdlet Set-NetFirewallRule novo no Windows PowerShell 3.0.  
  
> [!WARNING]  
> Não tente adicionar o shell gráfico volta para o computador enquanto ele está no DSRM. Manutenção de pilha (CBS) do Windows não pode funcionar corretamente no modo de segurança ou DSRM. Tentativas de adicionar recursos ou funções enquanto estiver no DSRM não concluir e deixe o computador em um estado instável até que ele é inicializado normalmente. Como um clone de controlador de domínio virtualizado no DSRM não é possível inicializar normalmente e não deve ser inicializado normalmente na maioria das circunstâncias, é impossível adicionar com segurança o shell gráfico. Isso é suportada e poderá expô-lo com um servidor inutilizável.  
  
### <a name="BKMK_SpecificProblems"></a>Solução de problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos os eventos de clonagem de controlador de domínio virtualizado gravar no log de eventos de serviços de diretório do controlador de domínio clone VM. Os logs de eventos de aplicativo, serviço de replicação de arquivo e replicação do DFS também podem conter informações de solução de problemas úteis para clonagem com falha. Falhas durante a chamada RPC para o emulador do PDC podem estar disponíveis no log de eventos no emulador do PDC.  
  
A seguir, os eventos de clonagem específicos do Windows Server 2012 no log de eventos de serviços de diretório, com resoluções sugeridas para erros e notas.  
  
##### <a name="directory-services-event-log"></a>Log de eventos de serviços de diretório  
  
|||  
|-|-|  
|**ID de evento**|**2160**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|O local *<COMPUTERNAME>*encontrou um controlador de domínio virtuais clonagem do arquivo de configuração.<br /><br />O controlador de domínio virtuais clonagem do arquivo de configuração é encontrado em: %1<br /><br />A existência do arquivo de configuração de clonagem do controlador de domínio virtual indica que o controlador de domínio local virtual é um clone de outro controlador de domínio virtual. O *<COMPUTERNAME>*começará a clone em si.|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado. Examine o diretório de trabalho DSA, systemroot%\ntds % e raiz de todos os discos removíveis ou locais para o arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID de evento**|**2161**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|O local *<COMPUTERNAME>*não encontrou o controlador de domínio virtuais clonagem do arquivo de configuração. O computador local não é um controlador de domínio clonado.|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado. Examine o diretório de trabalho DSA, systemroot%\ntds % e raiz de todos os discos removíveis ou locais para o arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID de evento**|**2162**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Clonagem de controlador de domínio virtual falhou.<br /><br />Verifique eventos registrados em logs de eventos do sistema e %systemroot%\debug\dcpromo.log para obter mais informações sobre erros que correspondem ao controlador de domínio virtuais clonagem tentativa.<br /><br />Código de erro: %1|  
|**Notas e resolução**|Instruções de mensagem, esse erro é um capturar todos.|  
  
|||  
|-|-|  
|**ID de evento**|**2163**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|DsRoleSvc serviço foi iniciado para clone o controlador de domínio local virtual.|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado. Examine o diretório de trabalho DSA, systemroot%\ntds % e raiz de todos os discos removíveis ou locais para o arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID de evento**|**2164**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao iniciar o serviço de DsRoleSvc para clonar o controlador de domínio local virtual.|  
|**Notas e resolução**|Examine as configurações de serviço para o serviço de servidor de função DS (DsRoleSvc) e certifique-se de que seu tipo de tela inicial é definido como manual. Valide que nenhum programa de terceiros está impedindo o início desse serviço.|  
  
|||  
|-|-|  
|**ID de evento**|**2165**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao iniciar um thread durante a clonagem de controlador de domínio local virtual.<br /><br />Código de erro: % 1<br /><br />Mensagem de erro: % 2<br /><br />Nome do thread: % 3|  
|**Notas e resolução**|Contate o suporte técnico da Microsoft|  
  
|||  
|-|-|  
|**ID de evento**|**2166**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*precisa de serviço RPCSS para iniciar reinicialização no DSRM. Aguardando RPCSS inicializar em um estado de execução falhou.<br /><br />Código de erro: % 1|  
|**Notas e resolução**|Examine o log de eventos do sistema e configurações de serviço para o serviço de servidor RPC (Rpcss)|  
  
|||  
|-|-|  
|**ID de evento**|**2167**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*não pôde inicializar o conhecimento do controlador de domínio virtual. Veja a entrada de log de eventos anteriores para obter detalhes.<br /><br />Dados adicionais<br /><br />Código de falha: % 1|  
|**Notas e resolução**|Instruções de mensagem, esse erro é um capturar todos.|  
  
|||  
|-|-|  
|**ID de evento**|**2168**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />O controlador de domínio está em execução em um hipervisor com suporte. ID de geração de VM é detectado.<br /><br />Valor atual de ID de geração de VM: %1|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2169**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Não há nenhuma ID de geração de VM detectado. O controlador de domínio é hospedado em um computador físico, uma versão de nível inferior do Hyper-V ou um hipervisor que não aceita a geração de VM ID.<br /><br />Dados adicionais<br /><br />Código de falha retornado durante a verificação de VM geração identificação: % 1|  
|**Notas e resolução**|Isso é um evento success se não pretende clone. Caso contrário, examine o log de eventos do sistema e examine a documentação de suporte do produto de hipervisor.|  
  
|||  
|-|-|  
|**ID de evento**|**2170**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Aviso|  
|**Mensagem**|Uma alteração de ID de geração foi detectada.<br /><br />ID de geração armazenado em cache no DS (valor antigo): %1<br /><br />ID de geração atualmente em VM (novo valor): %2<br /><br />A alteração de ID de geração ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real. *<COMPUTERNAME>*criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método com suporte para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de backup ciente de serviços de domínio do Active Directory.|  
|**Notas e resolução**|Isso é um evento success se pretende clone. Caso contrário, examine o log de eventos do sistema.|  
  
|||  
|-|-|  
|**ID de evento**|**2171**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Nenhuma alteração de ID de geração foi detectada.<br /><br />ID de geração armazenado em cache no DS (valor antigo): %1<br /><br />ID de geração atualmente em VM (novo valor): %2|  
|**Notas e resolução**|Isso é um evento success se não pretende clonar e deve ser visto em cada reinicialização de um controlador de domínio virtualizado. Caso contrário, examine o log de eventos do sistema.|  
  
|||  
|-|-|  
|**ID de evento**|**2172**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Leia o atributo msDS-GenerationId do objeto de computador do controlador de domínio.<br /><br />Valor do atributo de msDS-GenerationId: % 1|  
|**Notas e resolução**|Isso é um evento success se pretende clone. Caso contrário, examine o log de eventos do sistema.|  
  
|||  
|-|-|  
|**ID de evento**|**2173**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Falha ao ler o atributo msDS-GenerationId do objeto de computador do controlador de domínio. Isso pode ser causado por falha de transação de banco de dados ou a id de geração não existir no banco de dados local. MsDS-GenerationId não existe durante a primeira reinicialização após dcpromo ou o controlador de domínio não é um controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Código de falha: % 1|  
|**Notas e resolução**|Isso é um evento success se pretende clonar e é a primeira reinicialização VM depois clonagem for concluída. Ele também pode ser ignorado em controladores de domínio não virtual. Caso contrário, examine o log de eventos do sistema.|  
  
|||  
|-|-|  
|**ID de evento**|**2174**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|O controlador de domínio não é um clone de controlador de domínio virtual nem um instantâneo de controlador de domínio virtual restaurado.|  
|**Notas e resolução**|Isso é um evento success se não pretende clone. Caso contrário, examine o log de eventos do sistema.|  
  
|||  
|-|-|  
|**ID de evento**|**2175**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Arquivo de configuração de clone de controlador de domínio virtual existe em uma plataforma não suportada.|  
|**Notas e resolução**|Isso ocorre quando um dccloneconfig.xml for encontrado, mas não pôde ser encontrado um ID de geração de VM, como quando um arquivo dccloneconfig.xml for encontrado em um computador físico ou em um hipervisor que não aceita a geração de VM-ID.|  
  
|||  
|-|-|  
|**ID de evento**|**2176**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Renomear o arquivo de configuração de clone de controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Antigo arquivo nome: % 1<br /><br />Novo arquivo nome: % 2|  
|**Notas e resolução**|Renomear esperado ao inicializar uma fonte de VM fazer backup, porque a identificação de geração de VM não foi alterada. Isso impede que o controlador de domínio de origem para clonar.|  
  
|||  
|-|-|  
|**ID de evento**|**2177**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Falha ao renomear o arquivo de configuração de clone de controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Nome do arquivo: % 1<br /><br />Código de falha: % 2 %3|  
|**Notas e resolução**|Renomeie tentativa esperada ao inicializar uma fonte de VM fazer backup, porque a identificação de geração de VM não foi alterada. Isso impede que o controlador de domínio de origem para clonar. Renomeie o arquivo e investigar produtos de terceiros instalados que podem estar impedindo renomear o arquivo manualmente.|  
  
|||  
|-|-|  
|**ID de evento**|**2178**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Detectado um arquivo de configuração de clone de controlador de domínio virtual, mas a ID de geração de VM não foi alterado. O controlador de domínio local é a origem do clone DC. Renomeie o arquivo de configuração clone.|  
|**Notas e resolução**|Esperado ao inicializar uma fonte de VM fazer backup, porque a identificação de geração de VM não foi alterada. Isso impede que o controlador de domínio de origem para clonar.|  
  
|||  
|-|-|  
|**ID de evento**|**2179**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|O atributo msDS-GenerationId do objeto de computador do controlador de domínio foi definido como o seguinte parâmetro:<br /><br />GenerationID atributo: % 1|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2180**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Aviso|  
|**Mensagem**|Falha ao definir o atributo msDS-GenerationId do objeto de computador do controlador de domínio.<br /><br />Dados adicionais<br /><br />Código de falha: % 1|  
|**Notas e resolução**|Examine o log de eventos do sistema e Dcpromo.log. Pesquisa o erro específico no TechNet MS, MS Knowledgebase e blogs MS para determinar seu significado usual e, em seguida, solucionar problemas com base nos resultados.|  
  
|||  
|-|-|  
|**ID de evento**|**2182**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Evento interno: O serviço de diretório foi solicitado a clonar uma DSA remota:|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2183**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Evento interno: *<COMPUTERNAME>*concluir a solicitação de clonar o agente de sistema remoto do diretório.<br /><br />DC nome original: % 3<br /><br />Solicitar clone DC nome: % 4<br /><br />Solicitar clone DC site: % 5<br /><br />Dados adicionais<br /><br />Valor de erro: % 1 %2|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2184**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao criar uma conta de controlador de domínio para o controlador de domínio clonado.<br /><br />DC nome original: % 1<br /><br />Número permitido de DC:% clonados 2<br /><br />O limite no número de contas de controlador de domínio que podem ser geradas pelo clonagem *<COMPUTERNAME>*foi excedido.|  
|**Notas e resolução**|Um nome de controlador de domínio de origem única pode gerar somente automaticamente 9999 vezes se controladores de domínio não são rebaixados, com base na convenção de nomenclatura. Use o <computername>elemento no XML para gerar um novo nome exclusivo ou clone de um controlador de domínio nomeado de forma diferente.|  
  
|||  
|-|-|  
|**ID de evento**|**2191**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*Defina o seguinte valor do registro para desativar as atualizações DNS.<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem. O processo de clonagem habilitará DNS atualizações novamente depois de clonagem é concluída.|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2192**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao definir o valor do registro para desativar as atualizações DNS.<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Código de erro: % 4<br /><br />Mensagem de erro: % 5<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando atualizações do registro.|  
  
|||  
|-|-|  
|**ID de evento**|**2193**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*Defina o seguinte valor do registro para permitir que atualizações DNS.<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem.|  
|**Notas e resolução**|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|**ID de evento**|**2194**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao definir o valor do registro para permitir que atualizações DNS.<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Código de erro: % 4<br /><br />Mensagem de erro: % 5<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando atualizações do registro.|  
  
|||  
|-|-|  
|**ID de evento**|**2195**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Falha ao definir inicialização DSRM.<br /><br />Código de erro: % 1<br /><br />Mensagem de erro: % 2<br /><br />Quando clonagem de controlador de domínio virtual falhou ou arquivo de configuração de clone de controlador de domínio virtual aparece em um hipervisor sem suporte, o computador local será reinicializado no DSRM para solução de problemas. Falha na inicialização DSRM de configuração.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando atualizações do registro.|  
  
|||  
|-|-|  
|**ID de evento**|**2196**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Falha ao habilitar o privilégio de desligamento.<br /><br />Código de erro: % 1<br /><br />Mensagem de erro: % 2<br /><br />Quando clonagem de controlador de domínio virtual falhou ou arquivo de configuração de clone de controlador de domínio virtual aparece em um hipervisor sem suporte, o computador local será reinicializado no DSRM para solução de problemas. Falha na habilitação de privilégio de desligamento.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando o uso de privilégio.|  
  
|||  
|-|-|  
|**ID de evento**|**2197**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Falha ao iniciar o desligamento do sistema.<br /><br />Código de erro: % 1<br /><br />Mensagem de erro: % 2<br /><br />Quando clonagem de controlador de domínio virtual falhou ou arquivo de configuração de clone de controlador de domínio virtual aparece em um hipervisor sem suporte, o computador local será reinicializado no DSRM para solução de problemas. Falha na inicialização desligamento do sistema.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando o uso de privilégio.|  
  
|||  
|-|-|  
|**ID de evento**|**2198**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao criar ou modificar o objeto DC clonado seguinte.<br /><br />Dados adicionais:<br /><br />Objeto:<br /><br />%1<br /><br />Valor de erro: %2<br /><br />%3|  
|**Notas e resolução**|Pesquisa o erro específico no TechNet MS, MS Knowledgebase e blogs MS para determinar seu significado usual e, em seguida, solucionar problemas com base nos resultados.|  
  
|||  
|-|-|  
|**ID de evento**|**2199**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao criar o objeto DC clonado seguir porque o objeto já existe.<br /><br />Dados adicionais:<br /><br />Controlador de domínio de origem:<br /><br />%1<br /><br />Objeto:<br /><br />%2|  
|**Notas e resolução**|Validar o dccloneconfig.xml não especificou um controlador de domínio existente ou que cópias do dccloneconfig.xml tem sido usadas em vários clones sem editar o nome. Se a colisão ainda inesperada, determinar qual administrador promovido contatá-lo para discutir se o controlador de domínio existentes deve ser rebaixado, os metadados de controlador de domínio existentes limpo, ou se o clone deve usar um nome diferente.|  
  
|||  
|-|-|  
|**ID de evento**|**2203**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Falha na última clonagem de controlador de domínio virtual. Isso é a primeira reinicialização desde e então deve ser um tente novamente de clonagem. No entanto, nenhum arquivo de configuração de clone de controlador de domínio virtual existe nem alteração de ID de geração de máquina virtual é detectada. Inicializar no DSRM.<br /><br />Controlador de domínio virtual última clonagem falhou: %1<br /><br />Arquivo de configuração de clone de controlador de domínio virtual existe: %2<br /><br />Alteração de ID de geração de máquina virtual é detectada: %3|  
|**Notas e resolução**|Esperado se clonagem falhou anteriormente, devido à ausente ou inválido dccloneconfig.xml|  
  
|||  
|-|-|  
|ID de evento|2210|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|<COMPUTERNAME> Falha ao criar objetos para o controlador de domínio clone.<br /><br />Dados adicionais:<br /><br />Clone Id: %6<br /><br />Nome do controlador de domínio clone: %1<br /><br />Repetir loop: %2<br /><br />Valor da exceção: %3<br /><br />Valor de erro: %4<br /><br />DSID: %5|  
|Notas e resolução|Examine os logs de eventos do sistema e os serviços de diretório e dcpromo.log para obter mais detalhes sobre por que clonagem falhou.|  
  
|||  
|-|-|  
|ID de evento|2211|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criou objetos para o controlador de domínio clone.<br /><br />Dados adicionais:<br /><br />Clone Id: %3<br /><br />Nome do controlador de domínio clone: %1<br /><br />Repetir loop: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2212|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Começar a criar objetos clone controlador de domínio.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Nome do clone: %2<br /><br />Site de clone: %3<br /><br />Clone RODC: %4|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2213|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criado um novo objeto KrbTgt para Read-Only clonagem de controlador de domínio.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Novo Guid de objeto KrbTgt: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2214|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará um objeto de computador para o controlador de domínio clone.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Controlador de domínio original: %2<br /><br />Controlador de domínio clone: %3|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2215|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Adicionará o controlador de domínio clone o seguinte site.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Site: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2216|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará um contêiner de servidores para o controlador de domínio clone.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Contêiner de servidores: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2217|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará um objeto de servidor para o controlador de domínio clone.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Objeto de servidor: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2218|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará um objeto de configurações NTDS clone controlador de domínio.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Objeto: %2|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2219|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará objetos de conexão para o clone Read-Only controlador de domínio.<br /><br />Dados adicionais:<br /><br />Clone Id: %1|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2220|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Criará objetos SYSVOL para o clone Read-Only controlador de domínio.<br /><br />Dados adicionais:<br /><br />Clone Id: %1|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2221|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|<COMPUTERNAME> Falha ao gerar uma senha aleatória para o controlador de domínio clonados.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Nome do controlador de domínio clone: %2<br /><br />Erro: %3 %4|  
|Notas e resolução|Examine o log de eventos do sistema para obter mais detalhes sobre por que a senha de conta de computador não pôde ser criada.|  
  
|||  
|-|-|  
|ID de evento|2222|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|<COMPUTERNAME> Falha ao definir senha para o controlador de domínio clonados.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Nome do controlador de domínio clone: %2<br /><br />Erro: %3 %4|  
|Notas e resolução|Examine o log de eventos do sistema para obter mais detalhes sobre por que não foi possível definir a senha de conta de computador.|  
  
|||  
|-|-|  
|ID de evento|2223|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|<COMPUTERNAME> Configurar com êxito a senha de conta de computador para o controlador de domínio clonados.<br /><br />Dados adicionais:<br /><br />Clone Id: %1<br /><br />Nome do controlador de domínio clone: %2<br /><br />Total de vezes repetir: %3|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2224|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual falhou. As contas de serviço de gerenciada de %1 seguir existe no computador clonado:<br /><br />%2<br /><br />Para clonar para ter sucesso, todas as contas de serviço gerenciado deverá ser removidas. Isso pode ser feito usando o cmdlet do PowerShell Remove-ADComputerServiceAccount.|  
|Notas e resolução|Esperado ao usar MSAs autônomo (não agrupar MSA). Fazer *não* siga o conselho de evento para remover a conta - ele é escrito incorretamente. Use Uninstall-AdServiceAccount - [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />MSAs autônomo - lançado pela primeira vez no Windows Server 2008 R2 - foram substituídos no Windows Server 2012 com o grupo MSAs (gMSA). GMSAs suporte a clonagem.|  
  
|||  
|-|-|  
|ID de evento|2225|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativa|  
|Mensagem|Os segredos em cache do objeto de segurança seguinte foram removidos com êxito do controlador de domínio local:<br /><br />%1<br /><br />Depois de clonagem um controlador de domínio somente leitura, segredos que já foram armazenados em cache no controlador de domínio somente leitura de fonte clonagem serão removidos no controlador de domínio clonados.|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|2226|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|Falha ao remover segredos em cache do objeto de segurança seguinte do controlador de domínio local:<br /><br />%1<br /><br />Erro: %2 (%3)<br /><br />Depois de clonagem um controlador de domínio somente leitura, segredos que anteriormente eram armazenadas em cache na necessidade de controlador de domínio somente leitura origem clonagem seja removido no clone para reduzir o risco de que um invasor pode ser obtido essas credenciais roubada ou comprometida clone. Se a entidade de segurança é uma conta altamente privilegiada e deve ser protegido contra isso, use rootDSE operação rODCPurgeAccount limpar manualmente seus segredos no controlador de domínio local.|  
|Notas e resolução|Examine os logs de eventos do sistema e os serviços de diretório para obter mais informações.|  
  
|||  
|-|-|  
|ID de evento|2227|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|Exceção é gerada durante a tentativa de remover segredos em cache do controlador de domínio local.<br /><br />Dados adicionais:<br /><br />Valor da exceção: %1<br /><br />Valor de erro: %2<br /><br />DSID: %3<br /><br />Depois de clonagem um controlador de domínio somente leitura, segredos que anteriormente eram armazenadas em cache na necessidade de controlador de domínio somente leitura origem clonagem seja removido no clone para reduzir o risco de que um invasor pode ser obtido essas credenciais roubada ou comprometida clone. Se qualquer um dessas entidades de segurança é uma conta altamente privilegiada e devem ser protegidos contra isso, use rootDSE operação rODCPurgeAccount limpar manualmente seus segredos no controlador de domínio local.|  
|Notas e resolução|Examine os logs de eventos do sistema e os serviços de diretório para obter mais informações.|  
  
|||  
|-|-|  
|ID de evento|2228|  
|Fonte|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Erro|  
|Mensagem|A ID de geração de máquina Virtual no banco de dados do Active Directory deste controlador de domínio é diferente do valor atual desta máquina virtual. No entanto, um domínio virtual controlador clone configuração arquivo (DCCloneConfig.xml) não pôde ser localizado para que não houve tentativa clonagem de controlador de domínio. Se um controlador de domínio clonagem operação foi destinado, certifique-se de que um DCCloneConfig.xml é fornecido em qualquer um dos locais com suporte. Além disso, o endereço IP neste controlador de domínio está em conflito com o endereço IP do controlador de domínio outro. Para garantir que ocorrem sem interrupções no serviço, o controlador de domínio foi configurado para inicializar no DSRM.<br /><br />Dados adicionais:<br /><br />O endereço IP duplicado: %1|  
|Notas e resolução|Esse mecanismo de proteção impede que os controladores de domínio duplicados quando for possível (será não quando usando o DHCP, por exemplo). Adicionar um arquivo DcCloneConfig.xml válido, remova o sinalizador DSRM e tentar novamente clonagem|  
  
|||  
|-|-|  
|ID de evento|29218|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual falhou. Não foi possível concluir a operação de clonagem e o controlador de domínio clonados foi reinicializado no modo de restauração dos serviços de diretório (DSRM).<br /><br />Por favor, seleção de eventos e %systemroot%\debug\dcpromo.log para obter mais informações sobre erros que correspondem registrada anteriormente para o controlador de domínio virtuais clonagem tentativa e ou não essa imagem clone pode ser reutilizada.<br /><br />Se uma ou mais entradas de log indicam que o processo de clonagem não pode ser repetido, a imagem deve ser destruída com segurança. Caso contrário, você pode corrigir os erros, limpar o sinalizador de inicialização DSRM e reiniciar normalmente. Após a reinicialização, a operação de clonagem será repetida.|  
|Notas e resolução|Examine os logs de eventos do sistema e os serviços de diretório e dcpromo.log para obter mais detalhes sobre por que clonagem falhou.|  
  
|||  
|-|-|  
|ID de evento|29219|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Informativa|  
|Mensagem|Clonagem de controlador de domínio virtual foi bem-sucedida.|  
|Notas e resolução|Este é um evento success e apenas um problema se inesperado.|  
  
|||  
|-|-|  
|ID de evento|29248|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual Falha ao obter notificações do Winlogon. O código de erro retornado é %1 (%2).<br /><br />Para obter mais informações sobre esse erro, examine %systemroot%\debug\dcpromo.log para erros que correspondem ao controlador de domínio virtuais clonagem tentativa.|  
|Notas e resolução|Contate o suporte técnico da Microsoft|  
  
|||  
|-|-|  
|ID de evento|29249|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual Falha ao analisar o arquivo de configuração do controlador de domínio virtual.<br /><br />O código HRESULT retornado é %1.<br /><br />O arquivo de configuração é: %2<br /><br />Corrija os erros no arquivo de configuração e repita a operação de clonagem.<br /><br />Para obter mais informações sobre esse erro, consulte systemroot%\debug\dcpromo.log %.|  
|Notas e resolução|Examine o arquivo dclconeconfig.xml para erros de sintaxe usando um editor de XML e o arquivo de esquema DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|ID de evento|29250|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual falhou. Há softwares ou serviços habilitados no momento no controlador de domínio virtual clonados não presente na lista de aplicativos permitidos para clonagem de controlador de domínio virtual.<br /><br />A seguir estão as entradas ausentes:<br /><br />%2<br /><br />%1 (se houver) foi usada como a lista de inclusão definido.<br /><br />A operação de clonagem não pode ser concluída, se houver aplicativos não cloneable instalados.<br /><br />Por favor, execute o Cmdlet do Active Directory PowerShell Get-ADDCCloningExcludedApplicationList para verificar quais aplicativos estão instalados no computador clonado, mas não incluído na lista de permissões e adicioná-los à lista de permissões se eles são compatíveis com o controlador de domínio virtual clonagem. Se qualquer um desses aplicativos não são compatíveis com clonagem de controlador de domínio virtual, desinstale-los antes de tentar novamente a operação de clonagem.<br /><br />O controlador de domínio virtuais clonagem pesquisas de processo para o arquivo de lista de aplicativos permitidos, CustomDCCloneAllowList.xml, com base na seguinte ordem de pesquisa; o primeiro arquivo encontrado é usado e todos os outros são ignorados:<br /><br />1. O nome do valor do registro: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. O mesmo diretório em que reside a pasta do diretório de trabalho DSA<br /><br />3. %windir%\NTDS<br /><br />4. Mídia de leitura/gravação removível em ordem de letra de unidade na raiz da unidade|  
|Notas e resolução|Siga as instruções de mensagem|  
  
|||  
|-|-|  
|ID de evento|29251|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual Falha ao redefinir os endereços IP do computador clone.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Esse erro pode ser causado pela configuração incorreta nas seções de configuração de rede no arquivo de configuração de controlador de domínio virtual.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre erros que correspondem aos endereços IP redefinir durante o controlador de domínio virtuais clonagem tentativas.<br /><br />Detalhes sobre como redefinir os endereços IP de máquina na máquina clonado podem ser encontrados em https://go.microsoft.com/fwlink/?LinkId=208030|  
|Notas e resolução|Verifique se as informações de IP definidas no dccloneconfig.xml é válidas e não duplica o computador de origem original.|  
  
|||  
|-|-|  
|ID de evento|29253|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual falhou. O controlador de domínio clone não conseguiu localizar o mestre de operações do domínio primário (PDC) do controlador no domínio de origem do computador clonados da máquina clonado.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Verifique se o controlador de domínio primário no domínio inicial da máquina clonado é atribuído a um controlador de domínio ao vivo, está online e está funcionando. Verifique se a máquina clonada tem conectividade LDAP/RPC com o controlador de domínio primário sobre os protocolos e portas necessárias.|  
|Notas e resolução|Validar o IP de controlador de domínio clonados e informações de DNS são definidas. Use Dcdiag.exe//test: locatorcheck para validar se a PDCE está online, use Nltest.exe /server:*<PDCE>* /dclist:*<domain>* RPC válido, obter uma captura de rede do PDCE enquanto clonagem falha e analisar o tráfego.|  
  
|||  
|-|-|  
|ID de evento|29254|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual Falha ao ligar para o controlador de domínio primário %1.<br /><br />O código de erro retornado é %2 (%3).<br /><br />Verifique se o controlador de domínio primário %1 está online e está funcionando. Verifique se a máquina clonada tem conectividade LDAP/RPC com o controlador de domínio primário sobre os protocolos e portas necessárias.|  
|Notas e resolução|Validar o IP de controlador de domínio clonados e informações de DNS são definidas. Use Dcdiag.exe//test: locatorcheck para validar se a PDCE está online, use Nltest.exe /server:*<PDCE>* /dclist:*<domain>* RPC válido, obter uma captura de rede do PDCE enquanto clonagem falha e analisar o tráfego.|  
  
|||  
|-|-|  
|ID de evento|29255|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual falhou.<br /><br />Uma tentativa de criar objetos no controlador de domínio primário %1 exigida para a imagem está sendo clonada retornado um erro %2 (%3).<br /><br />Verifique se o controlador de domínio clonados tem privilégios de clonar em si. Procure eventos relacionados no log de eventos de serviço de diretório no controlador de domínio primário %1.|  
|Notas e resolução|Pesquisa o erro específico no TechNet MS, MS Knowledgebase e blogs MS para determinar seu significado típico e, em seguida, solucionar problemas com base nos resultados.|  
  
|||  
|-|-|  
|ID de evento|29256|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Falha na tentativa de definir a inicialização em sinalizador de modo de restauração de serviços de diretório com código de erro %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre erros.|  
|Notas e resolução|Examine os logs de serviços de diretório e dcpromo.log para obter detalhes. Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando o uso de privilégio.|  
  
|||  
|-|-|  
|ID de evento|29257|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual fez. Falha na tentativa de reinicializar o computador com o código de erro %1.<br /><br />Reinicialize o computador para concluir a operação de clonagem.|  
|Notas e resolução|Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando o uso de privilégio.|  
  
|||  
|-|-|  
|ID de evento|29264|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Falha na tentativa de limpar a inicialização em sinalizador de modo de restauração de serviços de diretório com código de erro %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre erros.|  
|Notas e resolução|Examine os logs de serviços de diretório e dcpromo.log para obter detalhes. Examine os logs de eventos de aplicativo e do sistema. Investigue o aplicativo de terceiros que pode estar bloqueando o uso de privilégio.|  
  
|||  
|-|-|  
|ID de evento|29265|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Informativa|  
|Mensagem|Clonagem de controlador de domínio virtual foi bem-sucedida. O domínio virtual controlador clonagem arquivo de configuração %1 foi renomeado para %2.|  
|Notas e resolução|N/d, este é um evento de sucesso.|  
  
|||  
|-|-|  
|ID de evento|29266|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual foi bem-sucedida. Falha ao tentar renomear o arquivo de configuração clonagem do domínio virtual controlador %1 com código de erro %2 (%3).|  
|Notas e resolução|Renomeie o arquivo dccloneconfig.xml manualmente.|  
  
|||  
|-|-|  
|ID de evento|29267|  
|Fonte|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Erro|  
|Mensagem|Clonagem de controlador de domínio virtual Falha ao verificar que o controlador de domínio virtual clonagem lista de aplicativos permitidos.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Esse erro pode ser causado por uma sintaxe de erro no clone permitir que o arquivo de lista (o arquivo que está sendo verificado é: %3). Para obter mais informações sobre esse erro, consulte systemroot%\debug\dcpromo.log %.|  
|Notas e resolução|Siga as instruções de evento|  
  
##### <a name="error-messages"></a>Mensagens de erro  
Não existem erros interativos diretos para clonagem de controlador de domínio que virtualizado falhou. todas as informações de clonagem efetua nos logs do sistema e os serviços de diretório e a promoção de controlador de domínio login no dcpromo.log. No entanto, se o servidor for inicializado no modo de Restauração DS, investigue imediatamente, como em promoção ou clonagem falha.  
  
Dcpromo.log é o primeiro lugar para verificar se há clonagem falha. Dependendo da falha listada, pode ser necessário posteriormente revisar logs de sistema para ainda mais o diagnóstico e serviços de diretório.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conhecidos e cenários de suporte  
A seguir estão alguns problemas comuns vistos durante o processo de desenvolvimento do Windows Server 2012. Todos esses problemas são "por design" e tem uma solução alternativa válida ou técnica mais apropriada para evitá-los em primeiro lugar. Alguns podem ser resolvidos em versões futuras do Windows Server 2012.  
  
|||  
|-|-|  
|**Problema**|**Clonagem falhar, DSRM**|  
|**Sintomas**|Clone inicializa no modo de restauração de serviços de diretório|  
|**Resolução e anotações**|Valide todas as etapas seguidas da seção de implantação do controlador de domínio virtualizado seções e [metodologia geral para solução de problemas de clonagem de controlador de domínio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Descritas em KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Extras concessões IP ao usar DHCP para clonar**|  
|**Sintomas**|Depois de clonagem com êxito um controlador de domínio e usando o DHCP, a primeira inicialização do clone leva um DHCP. Em seguida, quando o servidor é renomeado e reiniciado como um controlador de domínio, ela leva um segundo DHCP. O primeiro endereço IP não é lançado e acabar com uma concessão "fantasma"|  
|**Resolução e anotações**|Excluir a concessão de endereço não utilizados em DHCP ou permitir até que eles expirem normalmente manualmente. Descritas em KB 2742836.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha no DSRM após um atraso muito grande**|  
|**Sintomas**|Clonagem aparece para pausar a "clonagem de controlador de domínio é em X % de conclusão" para entre 8 e 15 minutos. Depois disso, a clonagem falha e carrega DSRM.|  
|**Resolução e anotações**|O computador clonado não é possível obter um endereço IP dinâmico de DHCP ou SLAAC, ou está usando um endereço IP duplicado ou não consegue encontrar o PDC. Várias tentativas realizadas pelo clonagem levam o atraso. Resolva o problema de rede para permitir a duplicação.<br /><br />Descritas em KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem não recriar todos os nomes de entidade de serviço**|  
|**Sintomas**|Se um conjunto de *três partes* nomes de entidade de serviço (SPN) inclui tanto um nome NetBIOS com uma porta e um nome NetBIOS idêntico sem uma porta, a entrada de porta não não é recriada com o novo nome de computador. Por exemplo:<br /><br />Customspn / DC1:200 / USE app1 inválido de símbolos *isso é recriado com o novo nome de computador*<br /><br />Uso inválido de símbolos customspn/DC1/app1 *isso não é recriado com o novo nome de computador*<br /><br />Nomes totalmente qualificados são recriados e SPNs sem três partes são recriados, independentemente de portas. Por exemplo, eles são recriados com êxito no clone:<br /><br />Customspn / USE DC1:202 inválido de símbolos *isso é recriado*<br /><br />customspn/DC1 inválido uso de símbolos *isso é recriado*<br /><br />Customspn / DC1.corp.contoso.com:202 uso inválido de símbolos *isso é recriado nome*<br /><br />Customspn / DC1.corp.contoso.com uso inválido de símbolos *isso é recriado*|  
|**Resolução e anotações**|Esta é uma limitação do processo de renomear de controlador de domínio no Windows, não apenas em clonagem. Três partes SPNS não são manipuladas pela lógica do renomeação em qualquer cenário. Mais serviços Windows incluídos não são afetados por isso, conforme eles recriar quaisquer SPNs ausentes conforme necessário. Outros aplicativos podem exigir inserir manualmente o SPN para resolver o problema.<br /><br />Descritas em KB 2742874.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha, é inicializado no DSRM, erros de rede gerais**|  
|**Sintomas**|Clone inicializa no modo de reparo de serviços de diretório. Há gerais de erros de rede.|  
|Resolução e anotações|Certifique-se de que o novo clone não tem um endereço MAC estático duplicado atribuído do controlador de domínio de origem; Você pode ver se uma VM usa endereços MAC estáticos pela execução deste comando no host hipervisor para a origem e o clone máquinas virtuais:<br /><br />Get-VM - VMName *teste vm* & #124; Get-VMNetworkAdapter & #124; Flórida *<br /><br />Alterar o endereço MAC para um endereço exclusivo estático ou alternar para usar endereços MAC dinâmicos.<br /><br />Descrito em KB 2742844|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha, é inicializado no DSRM como uma duplicata do controlador de domínio de origem**|  
|**Sintomas**|Um novo clone é inicializado sem clonagem. O dccloneconfig.xml não é renomeada e o servidor é iniciado no modo de Restauração DS. O log de eventos de serviços de diretório mostra o erro 2164<br /><br />*<COMPUTERNAME>*Falha ao iniciar o serviço de DsRoleSvc para clonar o controlador de domínio local virtual.|  
|**Resolução e anotações**|Examine as configurações de serviço para o serviço de servidor de função DS (DsRoleSvc) e certifique-se de que seu tipo de tela inicial é definido como Manual. Valide que nenhum programa de terceiros está impedindo o início desse serviço.<br /><br />Para obter mais informações sobre como recuperar esse DC secundário, garantindo que as atualizações replicadas saída, veja o artigo da Microsoft KB 2742970.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha, é inicializado no DSRM, erro 8610**|  
|**Sintomas**|Clone inicializa no modo de restauração de serviços de diretório. . Dcpromo log mostra o erro 8610 (que é ERROR_DS_ROLE_NOT_VERIFIED 8610 ou 0x21A2)|  
|**Resolução e anotações**|Acontecerá se o PDC pode ser descoberto, mas ele não realizou duplicação suficiente para permitir a mesmo para assumir a função. Por exemplo, se clonagem é iniciada e outro administrador move a função PDCE FSMO para um novo controlador de domínio.<br /><br />Descritas em KB 2742916.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha, é inicializado no DSRM, erros de rede gerais**|  
|**Sintomas**|Clone inicializa no modo de restauração de serviços de diretório. Há gerais de erros de rede.|  
|**Resolução e anotações**|Certifique-se de que o novo clone não tem um endereço MAC estático duplicado atribuído do controlador de domínio de origem; Você pode ver se uma VM usa endereços MAC estáticos pela execução deste comando sobre o host do Hyper-V para a origem e o clone máquinas virtuais:<br /><br />Get-VM - VMName *teste vm* & #124; Get-VMNetworkAdapter & #124; Flórida *<br /><br />Alterar o endereço MAC para um endereço exclusivo estático ou alternar para usar endereços MAC dinâmicos.<br /><br />Descritas em KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falha, é inicializado no DSRM**|  
|**Sintomas**|Clone inicializa no modo de reparo de serviços de diretório|  
|**Resolução e anotações**|Certifique-se de que o dccloneconfig.xml contém a definição de esquema (veja sampledccloneconfig.xml, linha 2):<br /><br />**< d3c:DCCloneConfig xmlns:d3c = "uri: microsoft.com:schemas:DCCloneConfig" >**<br /><br />Descrito em KB 2742844|  
  
|||  
|-|-|  
|Problema|**Não há servidores de logon são log de erros disponíveis no DSRM**|  
|**Sintomas**|Clone inicializa no modo de reparo de serviços de diretório. Você tenta fazer logon e receber o erro:<br /><br />**Não há nenhum servidores de logon estão disponíveis para atender à solicitação de logon**|  
|**Resolução e anotações**|Certifique-se de que você faça logon com a conta de administrador DSRM e não a conta de domínio. Use a seta para a esquerda e digite um nome de usuário:<br /><br />**. \administrator**<br /><br />Descrito em KB 2742908|  
  
|||  
|-|-|  
|**Problema**|**Clone origem falha no DSRM, erro**|  
|**Sintomas**|Durante a clonagem, falhar 8437 "criar clone DC objetos PDC falhou" (0x20f5)|  
|**Resolução e anotações**|Nome do computador duplicada foi definida em DCCloneConfig.xml como a origem DC ou um controlador de domínio existente. O nome do computador também precisa estar no formato de nome de computador NetBIOS (15 caracteres ou menos, não um FQDN).<br /><br />Corrija o arquivo dccloneconfig.xml definindo um nome exclusivo válido.<br /><br />Descrito em KB 2742959|  
  
|||  
|-|-|  
|**Problema**|**Erro New-addccloneconfigfile "índice estava fora do intervalo"**|  
|**Sintomas**|Ao executar o cmdlet new-addccloneconfigfile, você receber o erro:<br /><br />O índice estava fora do intervalo. Deve ser positivo e menores do que o tamanho da coleção.|  
|**Resolução e anotações**|Você deve executar o cmdlet em um console do Windows PowerShell elevado de administrador. Esse erro é causado por falta de associação do grupo administrador local no computador.<br /><br />Descrito em KB 2742927|  
  
|||  
|-|-|  
|**Problema**|**Clonagem falhar, DC duplicado**|  
|**Sintomas**|Clone inicializações sem clonagem duplicatas existente origem DC|  
|**Resolução e anotações**|O computador foi copiado e iniciado, mas não contém um arquivo DcCloneConfig.xml em qualquer um dos locais com suporte e não tinha um endereço IP duplicado com o controlador de domínio de origem. O controlador de domínio deve ser removido corretamente para evitar perda de dados.<br /><br />Descrito em KB 2742970|  
  
|||  
|-|-|  
|**Problema**|**New-ADDCCloneConfigFile falha com o servidor não é operacional erro quando ele verifica se o controlador de domínio de origem é um membro do grupo de controladores de domínio Cloneable se não houver um GC.**|  
|**Sintomas**|Ao executar New-ADDCCloneConfigFile para criar um arquivo dccloneconfig.xml, você receber o erro:<br /><br />Código - o servidor não está funcionando|  
|**Resolução e anotações**|Verifique a conectividade para um GC do servidor onde você pode executar New-ADDCCloneConfigFile e verifique se que a participação do controlador de domínio de origem no grupo controladores de domínio Cloneable duplicou para esse GC.<br /><br />Execute o comando a seguir como um meio de liberar o cache de localizador de DC para os casos onde um GC ou controlador de domínio pode ter sido desconectado recentemente:<br /><br />Código - nltest /dsgetdc: /GC//FORCE|  
  
### <a name="advanced-troubleshooting"></a>Solução de problemas avançada  
Esse módulo tenta ensinar a solução de problemas avançada usando *trabalhando* logs como exemplos, com algumas explicação sobre o que ocorreu. Se você compreender a aparência de uma operação de controlador de domínio virtualizado bem-sucedida, falhas fica evidente em seu ambiente. Esses logs são apresentados pelo seu código-fonte, com a ordem crescente de *esperado* eventos (mesmo quando eles são avisos e erros) relacionados a um controlador de domínio clonados em cada log.  
  
#### <a name="cloning-a-domain-controller"></a>Clonagem um controlador de domínio  
Neste exemplo, o controlador de domínio clone usa DHCP para obter um endereço IP, replica SYSVOL usando FRS ou DFSR (consulte o log apropriado conforme necessário), é um catálogo global e usa um arquivo em branco dccloneconfig.xml.  
  
##### <a name="directory-services-event-log"></a>Log de eventos de serviços de diretório  
O log de serviços de diretório contém a maioria das informações operacionais clonagem com base em eventos. O hipervisor muda a ID de VM-Generation e o serviço NTDS anotações-lo, em seguida, invalida o pool RID e muda a ID de invocação. O novo ID VM-Generation é definido e o servidor replica os dados de entrada do Active Directory. O serviço DFSR for interrompido e seu banco de dados que hospeda SYSVOL é excluído, forçar uma sincronização não autorizada entrada. A marca d'água USN é ajustada.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**2160**|ActiveDirectory_DomainService|Os serviços de domínio Active Directory local tiver encontrado um arquivo de configuração de clonagem do controlador de domínio virtual.<br /><br />O arquivo de configuração de clonagem do controlador de domínio virtual é encontrado em:<br /><br />*<path>*\DCCloneConfig.XML<br /><br />A existência do arquivo de configuração de clonagem do controlador de domínio virtual indica que o controlador de domínio local virtual é um clone de outro controlador de domínio virtual. Os serviços de domínio do Active Directory começará a clone em si.|  
|**2191**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory defina o seguinte valor do registro para desativar as atualizações DNS.<br /><br />Chave do registro:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valor do registro:<br /><br />UseDynamicDns<br /><br />Dados de valor do registro:<br /><br />0<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem. O processo de clonagem habilitará DNS atualizações novamente depois de clonagem é concluída.|  
|**2191**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory defina o seguinte valor do registro para desativar as atualizações DNS.<br /><br />Chave do registro:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valor do registro:<br /><br />RegistrationEnabled<br /><br />Dados de valor do registro:<br /><br />0<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem. O processo de clonagem habilitará DNS atualizações novamente depois de clonagem é concluída.<br /><br />"Informações 2/7/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191 configuração interna" serviços de domínio do Active Directory defina o seguinte valor do registro para desativar as atualizações DNS.<br /><br />Chave do registro:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valor do registro:<br /><br />DisableDynamicUpdate<br /><br />Dados de valor do registro:<br /><br />1<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome de computador que o computador de origem clone por um curto período. DNS A e registro AAAA são desabilitados durante esse período para que clientes não poderão enviar solicitações na máquina local passando por clonagem. O processo de clonagem habilitará DNS atualizações novamente depois de clonagem é concluída.|  
|**2172**|ActiveDirectory_DomainService|Leia o atributo msDS-GenerationId do objeto de computador do controlador de domínio.<br /><br />Valor do atributo msDS-GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|Uma alteração de ID de geração foi detectada.<br /><br />ID de geração armazenado em cache no DS (valor antigo):<br /><br />*<Number>*<br /><br />ID de geração atualmente em VM (novo valor):<br /><br />*<Number>*<br /><br />A alteração de ID de geração ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real. Serviços de domínio do Active Directory criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método com suporte para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de backup ciente de serviços de domínio do Active Directory.|  
|**1109**|ActiveDirectory_DomainService|O atributo invocationID para esse servidor de diretório foi alterado. O maior número de sequência de atualização no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (valor antigo):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<Number>*<br /><br />O invocationID é alterada quando um servidor de diretório é restaurado da mídia de backup, é configurado para hospedar uma partição de diretório do aplicativo gravável, foi retomado após ter sido aplicado a um instantâneo da máquina virtual, após uma operação de importação de máquina virtual, ou após uma operação de migração em tempo real. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método suportado para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de reconhecimento de domínio do Active Directory serviços de backup.|  
|**1000**|ActiveDirectory_DomainService|Conclua a inicialização do Microsoft Active Directory Domain Services.|  
|**1394**|ActiveDirectory_DomainService|Todos os problemas que impediam as atualizações para o banco de dados de serviços de domínio do Active Directory foram limpos. Novas atualizações para o banco de dados de serviços de domínio do Active Directory estão tendo êxito. O serviço de Logon de rede foi reiniciado|  
|**2163**|ActiveDirectory_DomainService|DsRoleSvc serviço foi iniciado para clone o controlador de domínio local virtual.|  
|**326**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados anexado a um banco de dados (1, C:\Windows\NTDS\ntds.dit). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], 0.000 [5], [6] 0.016, 0.000 [7], [8] 0.000, 0.000 [9], 0.000 [10], 0.000 [11], 0.000 [12].<br /><br />Salva o Cache: 1|  
|**103**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.032, 0.000 [6], [7] 0.000, 0.000 [8], 0.031 [9], 0.000 [10], 0.000 [11], 0.000 [12], [13] 0.000, 0.000 [14], [15] 0.000.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): O banco de dados mecanismo (6.02.8225.0000) é iniciar uma nova instância (0).|  
|**105**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.016 [1], 0.000 [2], [3] 0.015, 0.078 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.046, 0.000 [10], 0.000 [11].|  
|**1004**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory foi encerrado com êxito.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): O banco de dados mecanismo (6.02.8225.0000) é iniciar uma nova instância (0).|  
|**326**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados anexado a um banco de dados (1, C:\Windows\NTDS\ntds.dit). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.015 [2], [3] 0.016, 0.000 [4], [5] 0.031, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.000, 0.000 [10], 0.000 [11], 0.000 [12].<br /><br />Salva o Cache: 1|  
|**105**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 1 segundos)<br /><br />Relógio interno sequência: 0.031 [1], [2] 0.000, 0.000 [3], [4] 0.391, 0.000 [5], 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.031, 0.000 [10], 0.000 [11].|  
|**1109**|ActiveDirectory_DomainService|O atributo invocationID para esse servidor de diretório foi alterado. O maior número de sequência de atualização no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (valor antigo):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<Number>*<br /><br />O invocationID é alterada quando um servidor de diretório é restaurado da mídia de backup, é configurado para hospedar uma partição de diretório do aplicativo gravável, foi retomado após ter sido aplicado a um instantâneo da máquina virtual, após uma operação de importação de máquina virtual, ou após uma operação de migração em tempo real. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método suportado para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de reconhecimento de domínio do Active Directory serviços de backup.|  
|**1168**|ActiveDirectory_DomainService|Erro interno: um Active Directory Domain Services erro tiver ocorrido.<br /><br />Dados adicionais<br /><br />Valor de erro (decimal):<br /><br />2<br /><br />Valor de erro (hexadecimal):<br /><br />2<br /><br />ID interna:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|Promoção de um catálogo global neste controlador de domínio será atrasada para o intervalo a seguir.<br /><br />Intervalo (minutos):<br /><br />5<br /><br />Esse atraso é necessário para que as partições de diretórios necessária podem estar preparadas antes do catálogo global está anunciado. No registro, você pode especificar o número de segundos que o agente de sistema do diretório aguardará antes de promover o controlador de domínio local para um catálogo global. Para obter mais informações sobre o valor de registro de anúncio de atraso de Catálogo Global, consulte o guia de sistemas distribuídos do Resource Kit|  
|**103**|NTDS ISAM|NTDSA NTDS (536): O mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.047, 0.000 [6], [7] 0.000, 0.000 [8], 0.016 [9], 0.000 [10], 0.000 [11], 0.000 [12], [13] 0.000, 0.000 [14], [15] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory foi encerrado com êxito.|  
|**1539**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory não foi possível desativar o cache de disco baseadas em software de gravação no disco rígido a seguir.<br /><br />Disco rígido:<br /><br />c:<br /><br />Dados poderão ser perdidos durante falhas de sistema|  
|**2179**|ActiveDirectory_DomainService|O atributo msDS-GenerationId do objeto de computador do controlador de domínio foi definido como o seguinte parâmetro:<br /><br />Atributo GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|Falha ao ler o atributo msDS-GenerationId do objeto de computador do controlador de domínio. Isso pode ser causado por falha de transação de banco de dados ou a id de geração não existir no banco de dados local. MsDS-GenerationId não existe durante a primeira reinicialização após dcpromo ou o controlador de domínio não é um controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Código de falha:<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Versão completa, do início do Microsoft Active Directory Domain Services 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Todos os problemas que impediam as atualizações para o banco de dados de serviços de domínio do Active Directory foram limpos. Novas atualizações para o banco de dados de serviços de domínio do Active Directory estão tendo êxito. O serviço de Logon de rede foi reiniciado.|  
|**1128**|ActiveDirectory_DomainService|Verificador de consistência de dados de Conhecimento 1128 "uma conexão de replicação foi criado do serviço de diretório de origem seguinte para o serviço de diretório local.<br /><br />Serviço de diretório de origem:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Serviço de diretório local:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Dados adicionais<br /><br />Código de motivo:<br /><br />0x2<br /><br />Criação aponte ID interna:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|O serviço de diretório de origem tem otimizado o número de sequência de atualização (USN) apresentado pelo serviço de diretório de destino. Os serviços de diretório de origem e de destino tem um parceiro de replicação comuns. O serviço de diretório de destino é atualizado com o parceiro de replicação comum, e o serviço de diretório de origem foi instalado usando um backup deste parceiro.<br /><br />ID do serviço de diretório de destino:<br /><br />*<GUID> (<FQDN>)*<br /><br />ID de serviço de diretório comuns:<br /><br />*<GUID>*<br /><br />Propriedade USN comuns:<br /><br />*<Number>*<br /><br />Como resultado, o vetor UTDV do serviço de diretório de destino foi configurado com as seguintes configurações.<br /><br />Objeto USN anterior:<br /><br />0<br /><br />Propriedade USN anterior:<br /><br />0<br /><br />Banco de dados GUID:<br /><br />*<GUID>*<br /><br />Objeto USN:<br /><br />*<Number>*<br /><br />Propriedade USN:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Log de eventos do sistema  
As indicações de próxima de operações de clonagem estão no log de eventos do sistema. Conforme o hipervisor instrui o computador convidado que ele foi clonado ou restaurado a partir de um instantâneo, o controlador de domínio invalida imediatamente seu pool RID para evitar duplicação de entidades de segurança mais tarde. Como clonagem receita, várias operações esperadas e mensagens aparecem, principalmente em serviços Iniciando e interrompendo e alguns esperado erros causados por isso. Quando concluir as notas de log de eventos do sistema gerais de clonagem sucesso.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**16654**|Diretório de serviços de SAM|Um conjunto de identificadores de conta (RIDs) tiver sido invalidado. Isso pode ocorrer nos seguintes casos esperados:<br /><br />1. Um controlador de domínio é restaurado de backup.<br /><br />2. Um controlador de domínio em execução em uma máquina virtual é restaurado do instantâneo.<br /><br />3. Um administrador tem invalidados manualmente o pool|  
|**7036**|Gerenciador de controle de serviço|O serviço do Active Directory Domain Services entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço do Centro de distribuição de chaves Kerberos entrou no estado em execução.|  
|**3096**|Logon de rede|O controlador de domínio primário para este domínio não pôde ser localizado.|  
|**7036**|Gerenciador de controle de serviço|O serviço do Gerenciador de contas de segurança entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de servidor entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de logon de rede entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|Serviços do Active Directory Web entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação DFS entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação do arquivo entrou no estado em execução.|  
|**14533**|Microsoft-Windows-DfsSvc|DFS acabou de criar todos os namespaces.|  
|**14531**|Microsoft-Windows-DfsSvc|Servidor DFS concluiu a inicialização.|  
|**7036**|Gerenciador de controle de serviço|O serviço de Namespace de DFS entrou no estado em execução.|  
|**7023**|Gerenciador de controle de serviço|O serviço de mensagens entre sites encerrado com o seguinte erro:<br /><br />O servidor especificado não pode executar a operação solicitada.|  
|**7036**|Gerenciador de controle de serviço|O serviço de mensagens entre sites entrou no estado parado.|  
|**5806**|Logon de rede|Atualizações dinâmicas foram desabilitadas manualmente no controlador de domínio.<br /><br />AÇÃO DO USUÁRIO<br /><br />Reconfigurar neste controlador de domínio para usar atualizações dinâmicas ou adicionar manualmente os registros de DNS do arquivo '% SystemRoot%\System32\Config\Netlogon.dns' no banco de dados DNS."|  
|**16651**|Diretório de serviços de SAM|Falha na solicitação de um novo pool de identificadores de conta. A operação será repetida até que a solicitação é bem-sucedida. O erro é<br /><br />A operação FSMO solicitada falhou. Não é possível contatar o proprietário atual do FSMO.|  
|**7036**|Gerenciador de controle de serviço|O serviço de servidor DNS entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de servidor de função DS entrou no estado em execução.|  
|**7036**|Gerenciador de controle de serviço|O serviço de logon de rede entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação do arquivo entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço do Centro de distribuição de chaves Kerberos entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço de servidor DNS entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço do Active Directory Domain Services entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço de logon de rede entrou no estado em execução.|  
|**7040**|Gerenciador de controle de serviço|O tipo de inicialização do serviço serviços de domínio do Active Directory foi alterado de auto inicial como desabilitada.|  
|**7036**|Gerenciador de controle de serviço|O serviço de logon de rede entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação do arquivo entrou no estado em execução.|  
|**29219**|DirectoryServices-DSROLE-Server|Clonagem de controlador de domínio virtual foi bem-sucedida.|  
|**29223**|DirectoryServices-DSROLE-Server|Este servidor agora é um controlador de domínio.|  
|**29265**|DirectoryServices-DSROLE-Server|Clonagem de controlador de domínio virtual foi bem-sucedida. O domínio virtual controlador clonagem arquivo de configuração C:\Windows\NTDS\DCCloneConfig.xml foi renomeado para C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|O processo C:\Windows\system32\lsass.exe (DC2) iniciou a reinicialização do computador DC2 em nome de usuário sistema/Autoridade NT pelo seguinte motivo: sistema operacional: reconfiguração (planejada)<br /><br />Código de motivo: 0x80020004<br /><br />Tipo de desligamento: reiniciar<br /><br />Comentário: "|  
  
##### <a name="dcpromolog"></a>DCPROMO.LOG  
Dcpromo.log contém a parte de promoção real de clonagem que não descreve o log de eventos de serviços de diretório. Como o log não fornece o nível de explicação influir nas entradas do log de eventos, esta seção do módulo contém anotação adicional.  
  
Os meios de processo de promoção a clonagem é iniciado, o controlador de domínio apagado de sua configuração atual e depois promovido novamente usando o banco de dados existente do AD (semelhante a uma promoção IFM) e o controlador de domínio replica diferenças de alteração de entrada entre AD e SYSVOL e clonagem for concluída.  
  
> [!NOTE]  
> O log foi modificado nesse módulo para facilitar a leitura, removendo a coluna de data.  
  
> [!NOTE]  
> Para obter mais explicações de dcpromo.log consulte a entender e solucionar problemas AD DS simplificado administração no Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Iniciar baseada em clone promoção  
  
-   Defina o sinalizador de modo de restauração de serviços de diretório para que o servidor não for inicializado backup normalmente como clone original e causa de nomenclatura ou colisões de serviço de diretório  
  
-   Atualizar o log de eventos de serviços de diretório  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Interrompa o serviço de logon de rede para que o controlador de domínio não fará anúncios  
  
```  
15:14:01 [INFO] Stopping service NETLOGON  
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:14:02 [INFO] StopService on NETLOGON returned 0  
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0  
15:14:02 [INFO] Updating service status to 4  
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Examine o arquivo dccloneconfig.xml para personalizações especificada pelo administrador.  
  
-   Neste caso do exemplo é um arquivo em branco, para que todas as configurações são geradas automaticamente e endereçamento IP automático é necessário da rede  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Validar que não há nenhum serviço ou programas instalados que não fazem parte do DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Ativar o DHCP em adaptadores de rede, como informações de IP não foi especificadas pelo administrador  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Localize o emulador do PDC  
  
-   Defina o site do clone (gerado automaticamente nesse caso)  
  
-   Definir nome do clone (gerado automaticamente nesse caso)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Criar um novo objeto de computador clone  
  
-   Renomeie o clone para coincidir com o novo nome  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Forneça as configurações de promoção, com base em anterior dccloneconfig.xml ou as regras de geração automática  
  
```  
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:  
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com  
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com  
15:14:05 [INFO] Site Name: Default-First-Site-Name  
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS  
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS  
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL  
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$  
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)  
```  
  
-   Promoção de tela inicial  
  
```  
15:14:05 [INFO] Promote DC as a clone  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Validate supplied paths  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Path is on an NTFS volume  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Start the worker task  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...  
15:14:05 [INFO] Request for promotion returning 0  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Parar e configurar todos os AD DS serviços relacionados (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> O serviço DNS demorando muito tempo para desligamento é esperado nesse cenário, como ele está usando zonas integradas ao anúncio que não estavam disponíveis até mesmo antes que o serviço NTDS parou - veja os eventos DNS descritos posteriormente nesta seção do módulo.  
  
```  
15:14:15 [INFO] Stopping service NTDS  
15:14:15 [INFO] Stopping service NtFrs  
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)  
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states  
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state  
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0  
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)  
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state  
15:14:16 [INFO] StopService on NtFrs returned 0  
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0  
15:14:16 [INFO] Stopping service Kdc  
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)  
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state  
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0  
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)  
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state  
15:14:17 [INFO] StopService on Kdc returned 0  
15:14:17 [INFO] Configuring service Kdc to 1 returned 0  
15:14:17 [INFO] Stopping service DNS  
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)  
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state  
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0  
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)  
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state  
15:15:00 [INFO] StopService on DNS returned 0  
15:15:00 [INFO] Configuring service DNS to 1 returned 0  
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)  
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state  
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0  
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)  
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state  
15:15:01 [INFO] StopService on NTDS returned 0  
15:15:01 [INFO] Configuring service NTDS to 1 returned 0  
15:15:01 [INFO] Configuring service NTDS  
15:15:01 [INFO] Configuring service NTDS to 64 returned 0  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Forçar a sincronização de tempo NT5DS (NTP) com outro controlador de domínio (geralmente o PDCE)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contatar um controlador de domínio que mantém a conta de controlador de domínio de origem do clone  
  
-   Liberar qualquer tíquetes Kerberos existentes  
  
```  
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$  
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0  
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Interrompa o serviço de logon de rede e defina seu tipo de tela inicial  
  
```  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...  
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:15:03 [INFO] StopService on NETLOGON returned 0  
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0  
15:15:03 [INFO] Stopped NETLOGON  
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...  
```  
  
-   Configurar os serviços DFSR/NTFRS para ser executado automaticamente  
  
-   Excluir seus arquivos de banco de dados existente para forçar a sincronização não autorizada de SYSVOL quando o serviço é iniciado em seguida  
  
```  
15:15:03 [INFO] Configuring service DFSR  
15:15:03 [INFO] Configuring service DFSR to 256 returned 0  
15:15:03 [INFO] Configuring service NTFRS  
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0  
15:15:03 [INFO] Removing DFSR Database files for SysVol  
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb  
15:15:04 [INFO] Created system volume path  
15:15:04 [INFO] Configuring service DFSR  
15:15:04 [INFO] Configuring service DFSR to 128 returned 0  
15:15:04 [INFO] Configuring service NTFRS  
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0  
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...  
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Iniciar o processo de promoção usando o arquivo de banco de dados NTDS existente  
  
-   Entrar em contato com o mestre de RID  
  
> [!NOTE]  
> O serviço do AD DS não está realmente instalado aqui, é herdada instrumentação no log de  
  
```  
15:15:04 [INFO] Installing the Directory Service  
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com  
15:15:04 [INFO] Starting Active Directory Domain Services installation  
15:15:04 [INFO] Validating user supplied options  
15:15:04 [INFO] Determining a site in which to install  
15:15:04 [INFO] Examining an existing forest...  
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...  
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services  
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539  
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.  
Hard disk:  
c:  
Data might be lost during system failures.  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041  
Duplicate event log entries were suppressed.  
See the previous event log entry for details. An entry is considered a duplicate if  
the event code and all of its insertion parameters are identical. The time period for  
this run of duplicates is from the time of the previous event to the time of this event.  
Event Code:  
80000603  
Number of duplicate entries:   
2  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121  
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.  
```  
  
-   Troque a ID de invocação existentes que existia no banco de dados de computadores de origem  
  
-   Crie um novo objeto de configurações NTDS deste clone  
  
-   Replicar em delta de objeto do AD do controlador de domínio de parceiro  
  
> [!NOTE]  
> Mesmo que todos os objetos estão listados como replicados, isso é apenas metadados necessários para subsume as atualizações. Todos os objetos inalterados no banco de dados NTDS clonado já existirem e não exigem replicação novamente, assim como em promoção com base em IFM.  
  
```  
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109  
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:  
InvocationID attribute (old value):  
24e7b22f-4706-402d-9b4f-f2690f730b40  
InvocationID attribute (new value):  
f74cefb2-89c2-442c-b1ba-3234b0ed62f8  
Update sequence number:  
20520  
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.  
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168  
Internal error: An Active Directory Domain Services error has occurred.  
Additional Data  
Error value (decimal):  
2  
Error value (hexadecimal):  
2  
Internal ID:  
7011658  
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...  
15:15:11 [INFO] Replicating the schema directory partition  
15:15:11 [INFO] Replicated the schema container.  
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.  
15:15:12 [INFO] Replicating the configuration directory partition  
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...  
15:15:12 [INFO] Replicated the configuration container.  
15:15:13 [INFO] Replicating critical domain information...  
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...  
15:15:13 [INFO] Replicated the critical objects in the domain container.  
```  
  
-   Preencher as partições GC conforme necessário, com as atualizações ausentes  
  
-   Concluir a parte crítica do AD DS da promoção  
  
```  
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110  
Promotion of this domain controller to a global catalog will be delayed for the following interval.  
Interval (minutes):  
5  
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.  
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000  
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0   
15:15:15 [INFO] Creating new domain users, groups, and computer objects  
15:15:16 [INFO] Completing Active Directory Domain Services installation  
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0  
15:15:16 [INFO] DsRolepInstallDs returned 0  
15:15:16 [INFO] Installed Directory Service  
```  
  
-   Conclua a duplicação de entrada de SYSVOL  
  
```  
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...  
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Completed system volume replication  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0  
15:15:18 [INFO] Set the product type  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Set the system volume path for NETLOGON  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Replicating non critical information  
15:15:18 [INFO] User specified to not replicate non-critical data  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...  
15:15:18 [INFO] Stopped the DS  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...  
15:15:18 [INFO] Configuring service NTDS  
15:15:18 [INFO] Configuring service NTDS to 16 returned 0  
```  
  
-   Ativar o registro do cliente DNS  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Execute os módulos SYSPREP especificados pelo DefaultDCCloneAllowList.xml <SysprepInformation> elemento.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   Promoção de clonagem for concluído  
  
-   Remova o sinalizador de inicialização DSRM para que o servidor seja inicializado normalmente próxima vez  
  
-   Renomeie o dccloneconfig.xml para que ele não é lido novamente na próxima inicialização  
  
-   Reinicie o computador  
  
```  
15:15:32 [INFO] The attempted domain controller operation has completed  
15:15:32 [INFO] Updating service status to 4  
15:15:32 [INFO] DsRolepSetOperationDone returned 0  
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.  
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.  
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...  
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.  
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml  
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml  
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] Rebooting machine  
```  
  
##### <a name="active-directory-web-services-event-log"></a>Log de eventos de serviços do Active Directory da Web  
Enquanto clonagem está ocorrendo, a NTDS.DIT costuma ser offline por períodos prolongados. O serviço ADWS registra pelo menos um evento para isso. Depois clonagem for concluída, o serviço ADWS é iniciado, notas que existe ainda não é um certificado de computador válido, mas (pode haver ou pode não estar, dependendo do seu ambiente de implantação de uma PKI Microsoft com inscrição automática ou não) e, em seguida, inicia a instância do novo controlador de domínio.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**1202**|Eventos de ocorrência ADWS|Este computador agora está hospedando a instância do diretório especificado, mas dos serviços Web do Active Directory não poderia serviço. Serviços do Active Directory Web repetirá essa operação periodicamente.<br /><br />Instância do diretório: NTDS<br /><br />A instância de diretório porta LDAP: 389<br /><br />A instância de diretório porta SSL: 636|  
|**1000**|Eventos de ocorrência ADWS|Serviços do Active Directory da Web está sendo iniciado|  
|**1008**|Eventos de ocorrência ADWS|Serviços do Active Directory da Web com êxito reduziu seus privilégios de segurança|  
|**1100**|Eventos de ocorrência ADWS|Os valores especificados no <appsettings>seção do arquivo de configuração para serviços do Active Directory Web tiverem sido carregados sem erros.|  
|**1400**|Eventos de ocorrência ADWS|Eventos de certificado ADWS "serviços de Web do Active Directory não foi possível localizar um certificado de servidor com o nome de certificado especificado. Um certificado é necessária para usar conexões de SSL/TLS. Para usar conexões de SSL/TLS, verifique se um certificado de autenticação de servidor válido de uma autoridade de certificação confiável (CA) é instalado no computador.<br /><br />Nome de certificado:*<Server FQDN>*|  
|**1100**|Eventos de ocorrência ADWS|Os valores especificados no <appsettings>seção do arquivo de configuração para serviços do Active Directory Web tiverem sido carregados sem erros.|  
|**1200**|Eventos de ocorrência ADWS|Serviços do Active Directory da Web agora está atendendo a instância de diretório especificado.<br /><br />Instância do diretório: NTDS<br /><br />A instância de diretório porta LDAP: 389<br /><br />A instância de diretório porta SSL: 636|  
  
##### <a name="dns-server-event-log"></a>Log de eventos do servidor DNS  
O serviço DNS será sofrem breve esperado enquanto clonagem ocorre, como o serviço DNS ainda está em execução enquanto o banco de dados do AD DS estiver offline. Isso ocorre se usando DNS integrada do Active Directory, mas não se usando o padrão principal ou secundário DNS. Esses erros fazer logon várias vezes. Normalmente após a conclusão de clonagem, DNS ficar online novamente.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**4013**|Serviço de servidor DNS|O servidor DNS está esperando por serviços de domínio Active Directory (AD DS) sinalizar que a sincronização inicial de diretório foi concluída. O serviço de servidor DNS não pode iniciar até que a sincronização inicial seja concluída porque dados críticos de DNS ainda não podem ser replicados para esse controlador de domínio. Se eventos no log de eventos do AD DS indicarem que há um problema com resolução de nomes DNS, considere adicionar o endereço IP do outro servidor DNS para esse domínio para a lista de servidor DNS nas propriedades do protocolo IP deste computador. Esse evento será registrado cada dois minutos até que o AD DS foi sinalizado que a sincronização inicial foi concluída com êxito.|  
|**4015**|Serviço de servidor DNS|O servidor DNS encontrou um erro crítico do Active Directory. Verifique se o Active Directory está funcionando corretamente. As informações de depuração de erro estendido (que podem ser vazias) são "" ". Os dados do evento contém o erro.|  
|**4000**|Serviço de servidor DNS|O servidor DNS não pôde abrir o Active Directory.  Esse servidor DNS esteja configurada para obter e usar informações do diretório para essa zona e não pode carregar a zona sem ele.  Verifique se o Active Directory está funcionando corretamente e recarregue a zona. Os dados do evento são o código de erro.|  
|**4013**|Serviço de servidor DNS|O servidor DNS está esperando por serviços de domínio Active Directory (AD DS) sinalizar que a sincronização inicial de diretório foi concluída. O serviço de servidor DNS não pode iniciar até que a sincronização inicial seja concluída porque dados críticos de DNS ainda não podem ser replicados para esse controlador de domínio. Se eventos no log de eventos do AD DS indicarem que há um problema com resolução de nomes DNS, considere adicionar o endereço IP do outro servidor DNS para esse domínio para a lista de servidor DNS nas propriedades do protocolo IP deste computador. Esse evento será registrado cada dois minutos até que o AD DS foi sinalizado que a sincronização inicial foi concluída com êxito.|  
|**2**|Serviço de servidor DNS|O servidor DNS foi iniciado.|  
|**4**|Serviço de servidor DNS|O servidor DNS terminou o carregamento de plano de fundo de zonas. Todas as regiões agora estão disponíveis para atualizações DNS e transferências de zona, conforme permitido pelas suas configurações de zona individuais.|  
  
##### <a name="file-replication-service-event-log"></a>Log de eventos do serviço de replicação de arquivo  
O serviço de replicação de arquivo sincroniza sem autoridade de um parceiro durante clonagem. Clonagem faz isso excluindo os arquivos de banco de dados NTFRS e deixar o conteúdo de SYSVOL tocados, para uso como dados previamente propagação. Espera-se as duas tentativas de sincronizar.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**13562**|NtFrs|A seguir está o resumo de avisos e erros encontrados pelo serviço de replicação de arquivos enquanto sondagem o controlador de domínio DC2.root.fabrikam.com réplica FRS define informações de configuração.<br /><br />Não pode associar a um controlador de domínio. Tentará novamente no próximo ciclo de sondagem|  
|**13502**|NtFrs|O serviço de replicação de arquivo está parando.|  
|**13565**|NtFrs|Serviço de replicação de arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. DC2 do computador não pode se tornar um controlador de domínio até que esse processo seja concluído. O volume do sistema, em seguida, será compartilhado como SYSVOL.<br /><br />Para verificar se há compartilhamento SYSVOL, no prompt de comando, digite:<br /><br />Compartilhamento de rede<br /><br />Quando o serviço de replicação de arquivo conclui o processo de inicialização, o compartilhamento SYSVOL aparecerá.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, a disponibilidade de outros controladores de domínio e o intervalo de replicação entre controladores de domínio.|  
|**13501**|NtFrs|O serviço de replicação de arquivo está iniciando|  
|**13502**|NtFrs|O serviço de replicação de arquivo está parando.|  
|**13503**|NtFrs|O serviço de duplicação de arquivos foi interrompida.|  
|**13565**|NtFrs|Serviço de replicação de arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. DC2 do computador não pode se tornar um controlador de domínio até que esse processo seja concluído. O volume do sistema, em seguida, será compartilhado como SYSVOL.<br /><br />Para verificar se há compartilhamento SYSVOL, no prompt de comando, digite:<br /><br />Compartilhamento de rede<br /><br />Quando o serviço de replicação de arquivo conclui o processo de inicialização, o compartilhamento SYSVOL aparecerá.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, a disponibilidade de outros controladores de domínio e o intervalo de replicação entre controladores de domínio.|  
|**13501**|NtFrs|O serviço de replicação de arquivo está iniciando.|  
|**13553**|NtFrs|O serviço de duplicação de arquivos adicionado com êxito neste computador para o seguinte conjunto de réplica:<br /><br />"VOLUME DO SISTEMA DE DOMÍNIO (SYSVOL SHARE)"<br /><br />Informações relacionadas a este evento são mostradas abaixo:<br /><br />É o nome do computador DNS *<Domain Controller FQDN>*<br /><br />Nome de membro do conjunto de réplica *<Domain Controller>*<br /><br />Caminho da réplica conjunto raiz é *<path>*<br /><br />Caminho de diretório temporário réplica é *<path>*<br /><br />Caminho do diretório de trabalho da réplica é *<path>*|  
|**13520**|NtFrs|O serviço de duplicação de arquivos movidos os arquivos preexistentes <path>para *<path>*\NtFrs_PreExisting___See_EventLog.<br /><br />O serviço de duplicação de arquivos poderá excluir os arquivos em *<path>*\NtFrs_PreExisting___See_EventLog a qualquer momento. Arquivos podem ser salvos contra exclusão copiando-os de *<path>*\NtFrs_PreExisting___See_EventLog. Copiar os arquivos para c:\windows\sysvol\domain pode levar a conflitos de nome se os arquivos já existirem em alguns outros parceiros de replicação.<br /><br />Em alguns casos, o serviço de duplicação de arquivos poderá copiar um arquivo de *<path>*\NtFrs_PreExisting___See_EventLog em *<path>*em vez de duplicar o arquivo de alguns outros parceiros de replicação.<br /><br />Espaço pode ser recuperado a qualquer momento, excluindo os arquivos em *<path>*\NtFrs_PreExisting___See_EventLog. "|  
|**13508**|NtFrs|Serviço de replicação de arquivo está tendo problemas para habilitar a replicação de *\\\\<Domain Controller FQDN>*para *<Domain Controller>*para *<path>*usando o<br /><br />DNS name *\\\\<Domain Controller FQDN>*. FRS continuará tentando.<br /><br />A seguir estão alguns dos motivos que você veria esse aviso.<br /><br />[1] FRS não possa resolver corretamente o nome DNS *\\\\<Domain Controller FQDN>*deste computador.<br /><br />[2] FRS não está em execução *\\\\<Domain Controller FQDN>*.<br /><br />[3] As informações de topologia no Active Directory Domain Services desta réplica ainda não foi duplicado para todos os controladores de domínio.<br /><br />Esta mensagem de log de eventos será exibida uma vez por conexão, depois que o problema foi corrigido verá outra mensagem de log de eventos que indica que a conexão foi estabelecida.|  
|**13509**|NtFrs|O serviço de replicação de arquivo tiver habilitado a replicação de *\\\\<Domain Controller FQDN>*para *<Domain Controller>*para *<Path>*após várias tentativas repetidas.|  
|**13516**|NtFrs|O serviço de duplicação de arquivos não são mais está impedindo que o computador *<Domain Controller>*de se tornar um controlador de domínio. O volume do sistema foi inicializado com êxito e o serviço de logon de rede tenha sido notificado que o volume do sistema agora está pronto para ser compartilhado como SYSVOL.<br /><br />Tipo de "net share" para verificar se há compartilhamento SYSVOL. "|  
  
##### <a name="dfs-replication-event-log"></a>Log de eventos de replicação do DFS  
Os serviços DFSR sincroniza sem autoridade de um parceiro durante clonagem. Clonagem faz isso excluindo os arquivos de banco de dados DFSR e deixar o conteúdo de SYSVOL tocados, para uso como dados previamente propagação. Espera-se as duas tentativas de sincronizar.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**1004**|DFSR|O serviço de replicação DFS foi iniciado.|  
|**1314**|DFSR|O serviço de replicação DFS configurado com êxito os arquivos de log de depuração.<br /><br />Informações adicionais:<br /><br />Caminho do arquivo de Log de depuração: C:\Windows\debug|  
|**6102**|DFSR|O serviço de replicação DFS foi registrado com êxito o provedor WMI|  
|**1206**|DFSR|O serviço de replicação DFS contatado com êxito o controlador de domínio DC2.corp.contoso.com para acessar informações de configuração.|  
|**1210**|DFSR|O serviço de replicação DFS configurado com êxito um ouvinte de RPC para solicitações de replicação.<br /><br />Informações adicionais:<br /><br />Porta: 0"|  
|**4614**|DFSR|O serviço de replicação DFS inicializada SYSVOL no caminho local C:\Windows\SYSVOL\domain e está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que ele tenha replicada com seu parceiro. Se o servidor estiver no processo sendo promovido para um controlador de domínio, o controlador de domínio não serão anunciar e funcionar como um controlador de domínio até que esse problema seja resolvido. Isso pode ocorrer se o parceiro especificado também está no estado de sincronização inicial, ou se violações de compartilhamento são encontradas nesse servidor ou o parceiro de sincronização. Se esse evento ocorreu durante a migração de SYSVOL do serviço de replicação de arquivos (FRS) para replicação do DFS, as alterações não serão replicadas até que esse problema seja resolvido. Isso pode causar a pasta SYSVOL nesse servidor se torne fora de sincronia com outros controladores de domínio.<br /><br />Informações adicionais:<br /><br />Nome de pasta replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada:*<GUID>*<br /><br />Nome do grupo de replicação: Volume de sistema de domínio<br /><br />ID do grupo de replicação:*<GUID>*<br /><br />ID do membro:*<GUID>*<br /><br />Read-Only: 0|  
|**4604**|DFSR|O serviço de replicação DFS inicializado com êxito a pasta SYSVOL replicado no caminho local C:\Windows\SYSVOL\domain. Esse membro tiver concluído a sincronização inicial de SYSVOL com dc1.corp.contoso.com do parceiro. Para verificar a presença do compartilhamento SYSVOL, abra uma janela de prompt de comando e digite "" net share"".<br /><br />Informações adicionais:<br /><br />Nome de pasta replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada:*<GUID>*<br /><br />Nome do grupo de replicação: Volume de sistema de domínio<br /><br />ID do grupo de replicação:*<GUID>*<br /><br />ID do membro:*<GUID>*<br /><br />Parceiro de sincronização:*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Solução de problemas de restauração segura de controlador de domínio virtualizado  
  
### <a name="tools-for-troubleshooting"></a>Ferramentas para solução de problemas  
  
#### <a name="logging-options"></a>Opções de registro em log  
Os logs internos serão a ferramenta mais importante para solução de problemas com a restauração de instantâneo de segurança de controlador de domínio. Todos esses logs são habilitados e configurados para máximo detalhamento, por padrão.  
  
|||  
|-|-|  
|**Operação**|**Log**|  
|**Criação de instantâneo**|-Evento viewer\Applications e serviços logs\Microsoft\Windows\Hyper-V-Worker|  
|**Restaurar instantâneo**|-Evento viewer\Applications e serviços logs\Directory serviço<br />-Evento viewer\Windows logs\System<br />-Evento viewer\Windows logs\Application<br />-Evento viewer\Applications e serviços logs\File replicação do serviço<br />-Evento viewer\Applications e serviços logs\DFS replicação<br />-Evento viewer\Applications e serviços logs\DNS<br />-Evento viewer\Applications e serviços logs\Microsoft\Windows\Hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e os comandos de solução de problemas de configuração do controlador de domínio  
Para solucionar problemas não explicados pelos logs, use as ferramentas a seguir como ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 Do Monitor de rede  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Metodologia geral para solução de problemas de restauração de segurança de controlador de domínio  
  
1.  É a restauração do instantâneo seguro esperada, mas com problemas?  
  
    1.  Examine o log de eventos de serviços de diretório  
  
        1.  Há erros de restaurar instantâneo?  
  
        2.  Há erros de replicação do AD?  
  
    2.  Examine o log de eventos do sistema  
  
        1.  Há erros de comunicação?  
  
        2.  Há erros de anúncio?  
  
2.  A restauração do instantâneo seguro é inesperados?  
  
    1.  Examine os logs de auditoria de hipervisor para determinar quem ou o que causou uma reversão  
  
    2.  Entre em contato com todos os administradores do hipervisor e investigá-los como para quem revertidas a VM sem notificação  
  
3.  É a proteção de reversão de USN implementação do servidor e não com segurança restaurando?  
  
    1.  Examine o log de eventos de serviços de diretório para um hipervisor ou integração com serviços sem suporte  
  
    2.  Examinar o sistema operacional e validar executando o Windows Server 2012?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Solução de problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos os virtualizados restauração de instantâneo de segurança de controlador de domínio eventos gravar no log de eventos de serviços de diretório do controlador de domínio restaurado VM. Os logs de eventos do aplicativo, do sistema, o serviço de replicação de arquivos e replicação do DFS também podem conter informações úteis de solução de problemas para falhas em restaurações.  
  
Abaixo são os eventos de restauração específico de segurança do Windows Server 2012 no log de eventos dos serviços de diretório.  
  
|||  
|-|-|  
|**ID de evento**|**2170**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Aviso|  
|**Mensagem**|Uma alteração de ID de geração foi detectada.<br /><br />ID de geração armazenado em cache no DS (valor antigo): %1<br /><br />ID de geração atualmente em VM (novo valor): %2<br /><br />A alteração de ID de geração ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real. *<COMPUTERNAME>*criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método com suporte para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de backup ciente de serviços de domínio do Active Directory.|  
|**Notas e resolução**|Isso é um evento success se o instantâneo foi esperado. Caso contrário, examine o log de eventos do Hyper-V-Worker ou contate o administrador de hipervisor.|  
  
|||  
|-|-|  
|**ID de evento**|**2174**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|O controlador de domínio não é um clone de controlador de domínio virtual nem um instantâneo de controlador de domínio virtual restaurado.|  
|**Notas e resolução**|Evento esperado ao iniciar controladores de domínio físico ou controladores de domínio virtualizado não restaurados a partir do instantâneo|  
  
|||  
|-|-|  
|**ID de evento**|**2181**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|A transação foi interrompida devido a máquina virtual sendo revertida para um estado anterior.  Isso ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Transações acompanhar a alteração de ID de geração de VM|  
  
|||  
|-|-|  
|**ID de evento**|**2185**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*parou o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço: % 1<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*deve inicializar um não-autorizada na réplica SYSVOL local. Isso é realizado pelo parar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração. Evento 2187 será registrado quando o serviço FRS ou DFSR é reiniciado.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados SYSVOL no controlador de domínio é substituída por cópia de um parceiro do controlador de domínio.|  
|**ID de evento**|2186|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao interromper o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço: % 1<br /><br />Código de erro: % 2<br /><br />Mensagem de erro: % 3<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*deve inicializar um não-autorizada na réplica SYSVOL local. Isso é feito parar o serviço de replicação FRS ou DFSR usado para replicar a pasta SYSVOL e, em seguida, iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração. *<COMPUTERNAME>*Falha ao interromper o serviço em execução atual e não pode concluir a restauração não autorizada. Execute uma restauração não-autorizada manualmente.|  
|**Notas e resolução**|Examine os logs de eventos do sistema, FRS e DFSR para obter mais informações.|  
  
|||  
|-|-|  
|**ID de evento**|**2187**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*iniciado o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço: % 1<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*necessário para inicializar um não-autorizada na réplica SYSVOL local. Isso foi feito interrompendo o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados SYSVOL no controlador de domínio é substituída por cópia de um parceiro do controlador de domínio.|  
  
|||  
|-|-|  
|**ID de evento**|**2188**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao iniciar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço: % 1<br /><br />Código de erro: % 2<br /><br />Mensagem de erro: % 3<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*Você precisa inicializar um não-autorizada na réplica SYSVOL local. Isso é feito por parar o serviço FRS ou DFSR usado para duplicar o SYSVOL e iniciá-lo com chaves do registro apropriadas e valores para acionar a restauração. *<COMPUTERNAME>*Falha ao iniciar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e não puder concluir a restauração não autorizada. Por favor, executar uma restauração não autorizada manualmente e reiniciar o serviço.|  
|**Notas e resolução**|Examine os logs de eventos do sistema, FRS e DFSR para obter mais informações.|  
  
|||  
|-|-|  
|**ID de evento**|**2189**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*Defina os seguintes valores do registro para inicializar da réplica SYSVOL durante uma restauração não-autorizada:<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*Você precisa inicializar um não-autorizada na réplica SYSVOL local. Isso é feito por parar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados SYSVOL no controlador de domínio é substituída por cópia de um parceiro do controlador de domínio.|  
  
|||  
|-|-|  
|**ID de evento**|**2190**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao definir os seguintes valores do registro para inicializar a réplica SYSVOL durante uma restauração não-autorizada:<br /><br />Chave do registro: % 1<br /><br />Valor do registro: %2<br /><br />Dados de valor do registro: %3<br /><br />Código de erro: % 4<br /><br />Mensagem de erro: % 5<br /><br />Active Directory detectou que a máquina virtual que hospeda a função de controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*Você precisa inicializar um não-autorizada na réplica SYSVOL local. Isso é feito por parar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração. *<COMPUTERNAME>*Falha ao definir os valores do registro acima e não puder concluir a restauração não autorizada. Execute uma restauração não-autorizada manualmente.|  
|**Notas e resolução**|Examine os logs de eventos de aplicativo e do sistema. Investigue os aplicativos de terceiros que podem estar bloqueando atualizações do registro.|  
  
|||  
|-|-|  
|**ID de evento**|**2200**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*inicializa replicação para trazer o controlador de domínio atual. Evento 2201 será registrado quando a replicação é concluída.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Marca o início da entrada replicação do AD.|  
  
|||  
|-|-|  
|**ID de evento**|**2201**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*concluiu a replicação para trazer o controlador de domínio atual.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Marca final da entrada replicação do AD.|  
  
|||  
|-|-|  
|**ID de evento**|**2202**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*Falha na replicação para trazer o controlador de domínio atualizado. O controlador de domínio será atualizado após a próxima replicação periódica.|  
|**Notas e resolução**|Examine os logs de eventos do sistema e os serviços de diretório. Use repadmin.exe tentar forçar a replicação e observe quaisquer falhas.|  
  
|||  
|-|-|  
|**ID de evento**|**2204**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*detectou uma alteração de ID de geração de máquina virtual. A alteração significa que o controlador de domínio virtual foi mudou para um estado anterior. *<COMPUTERNAME>*executarão as operações a seguir para proteger o controlador de domínio revertida contra divergência dados possíveis e para proteger a criação de entidades de segurança com SIDs duplicados:<br /><br />Criar uma nova ID de invocação<br /><br />Invalidar pool RID atual<br /><br />Propriedade das funções FSMO será validada na próxima duplicação de entrada. Durante esta janela se o controlador de domínio considerada uma função FSMO, essa função estará disponível.<br /><br />Inicie a operação de restauração de serviço de replicação de SYSVOL.<br /><br />Inicie a duplicação para trazer o controlador de domínio revertida para o estado mais recente.<br /><br />Solicite um novo pool RID.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Isso explica que todos os diversos Redefinir operações que ocorrerão como parte do processo de restauração segura.|  
  
|||  
|-|-|  
|**ID de evento**|**2205**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*invalidados pool RID atual depois de controlador de domínio virtual foi revertido para o estado anterior.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. O pool RID local deve ser destruído conforme o controlador de domínio tem tempo percorreu e eles podem ter já foi emitidos.|  
  
|||  
|-|-|  
|**ID de evento**|**2206**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|ERRO|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao invalidar pool RID atual depois de controlador de domínio virtual foi revertido para o estado anterior.<br /><br />Dados adicionais:<br /><br />Código de erro: %1<br /><br />Valor de erro: %2|  
|**Notas e resolução**|Examine os logs de eventos do sistema e os serviços de diretório. Validar o mestre RID está online pode ser acessado desse servidor usando Dcdiag.exe /test:ridmanager|  
  
|||  
|-|-|  
|**ID de evento**|**2207**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|ERRO|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao restaurar após o controlador de domínio virtual foi revertido para o estado anterior. Uma reinicialização no DSRM foi solicitada. Verifique eventos anteriores para obter mais informações.|  
|**Notas e resolução**|Examine os logs de eventos do sistema e os serviços de diretório.|  
  
|||  
|-|-|  
|**ID de evento**|**2208**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativa|  
|**Mensagem**|*<COMPUTERNAME>*excluídos DFSR bancos de dados para inicializar da réplica SYSVOL durante uma restauração não autorizada.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Isso garante a DFSR sem autoridade sincroniza SYSVOL de um parceiro DC. Observe que qualquer outro DFSR pastas replicadas no mesmo volume como SYSVOL sincronizará também sem autoridade (domínio controladores não são recomendados para o host personalizado que DFSR define no mesmo volume como SYSVOL).|  
  
|||  
|-|-|  
|**ID de evento**|**2209**|  
|**Fonte**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Erro|  
|**Mensagem**|*<COMPUTERNAME>*Falha ao excluir DFSR bancos de dados.<br /><br />Dados adicionais:<br /><br />Código de erro: %1<br /><br />Valor de erro: %2<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>*Você precisa inicializar um não-autorizada na réplica SYSVOL local. Para DFSR, isso é feito parar o serviço DFSR, excluindo bancos de dados DFSR e reiniciar o serviço. Ao reiniciar DFSR será recriar os bancos de dados e iniciar a sincronização inicial.|  
|**Notas e resolução**|Examine o log de eventos DFSR.|  
  
#### <a name="error-messages"></a>Mensagens de erro  
Existem sem erros interativos diretos para falhou domínio virtualizado controlador instantâneo seguro restauração; todas as informações de clonagem registra nos logs de eventos de serviços de diretório. Naturalmente, quaisquer erros críticos de publicidade de replicação ou servidor se manifestam como sintomas em outro lugar.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conhecidos e cenários de suporte  
O [metodologia geral para solução de problemas de domínio controlador seguro restaurar](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) são geralmente adequadas para a maioria dos problemas.  
  
|||  
|-|-|  
|**Problema**|**Não é possível criar novos objetos de segurança no controlador de domínio restaurado recentemente seguro**|  
|**Sintomas**|Depois de restaurar um instantâneo, tenta criar uma nova entidade de segurança (usuário, computador, grupo) em que falham de controlador de domínio com:<br /><br />Erro 0x2010<br /><br />O serviço de diretório não pôde alocar um identificador relativo.|  
|**Resolução e anotações**|Esse problema é causado por dados de Conhecimento obsoleto da função RID Master FSMO do computador restaurado. Se a função movido para o controlador de domínio nesse ou em outro depois que um instantâneo foi retirado e restaurado posteriormente, o controlador de domínio restaurado não tenha conhecimento do mestre RID até que tenha concluído a replicação inicial.<br /><br />Para resolver o problema, permitir a conclusão da replicação do AD entrada para o controlador de domínio restaurado. Se ainda não funcionar, valide que todos os controladores de domínio têm o mesmo conhecimento correto dos quais DC hospeda o mestre de RID.|  
  
|||  
|-|-|  
|**Problema**|**Controladores de domínio restaurados não compartilhar SYSVOL, anuncie**|  
|**Sintomas**|Depois de restaurar um instantâneo, um ou mais controladores de domínio não anuncie, não compartilhe sysvol e não têm conteúdo SYSVOL atualizado|  
|**Resolução e anotações**|Parceiros de upstream os controladores de domínio não têm uma réplica SYSVOL de funcionar corretamente é replicação com DFSR ou FRS. Esse problema não seja relacionado à restauração segura, mas é provável que se manifestar como um problema de segurança restaurar, porque o cliente estava sabendo de outros problemas de replicação afetar cancelou restaurados controladores de domínio|  
  
### <a name="advanced-troubleshooting"></a>Solução de problemas avançada  
Esse módulo tenta ensinar a solução de problemas avançada usando *trabalhando* logs como exemplos, com algumas explicação sobre o que ocorreu. Se você compreender a aparência de uma operação de controlador de domínio virtualizado bem-sucedida, falhas fica evidente em seu ambiente. Esses logs são apresentados pelo seu código-fonte, com a ordem crescente de *esperado* eventos relacionados a um controlador de domínio clonados em cada log.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restauração de um controlador de domínio que replica SYSVOL usando DFSR  
  
##### <a name="directory-services-event-log"></a>Log de eventos de serviços de diretório  
O log de serviços de diretório contém a maioria das informações operacionais restauração segura. O hipervisor muda a ID de VM-Generation e o serviço NTDS anotações-lo, em seguida, invalida o pool RID e muda a ID de invocação. O novo ID de geração de VM é conjunto e replica os servidores entrada de dados do AD. O serviço DFSR for interrompido e seu banco de dados que hospeda SYSVOL é excluído, forçar uma sincronização não autorizada entrada. A marca d'água USN é ajustada.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**2170**|ActiveDirectory_DomainService|Uma alteração de ID de geração foi detectada.<br /><br />ID de geração armazenado em cache no DS (valor antigo):<br /><br />*<number>*<br /><br />ID de geração atualmente em VM (novo valor):<br /><br />*<number>*<br /><br />A alteração de ID de geração ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real. Serviços de domínio do Active Directory criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método com suporte para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de backup ciente de serviços de domínio do Active Directory."|  
|**2181**|ActiveDirectory_DomainService|A transação foi interrompida devido a máquina virtual sendo revertida para um estado anterior.  Isso ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real.|  
|**2204**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory detectou uma alteração de ID de geração de máquina virtual. A alteração significa que o controlador de domínio virtual foi mudou para um estado anterior. Serviços de domínio do Active Directory executarão as operações a seguir para proteger o controlador de domínio revertida contra divergência dados possíveis e para proteger a criação de entidades de segurança com SIDs duplicados:<br /><br />Criar uma nova ID de invocação<br /><br />Invalidar pool RID atual<br /><br />Propriedade das funções FSMO será validada na próxima duplicação de entrada. Durante esta janela se o controlador de domínio considerada uma função FSMO, essa função estará disponível.<br /><br />Inicie a operação de restauração de serviço de replicação de SYSVOL.<br /><br />Inicie a duplicação para trazer o controlador de domínio revertida para o estado mais recente.<br /><br />Solicitar um novo pool RID."|  
|**2181**|ActiveDirectory_DomainService|A transação foi interrompida devido a máquina virtual sendo revertida para um estado anterior.  Isso ocorre depois da aplicação de um instantâneo da máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração em tempo real.|  
|**1109**|ActiveDirectory_DomainService|O atributo invocationID para esse servidor de diretório foi alterado. O maior número de sequência de atualização no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (valor antigo):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<number>*<br /><br />O invocationID é alterada quando um servidor de diretório é restaurado da mídia de backup, é configurado para hospedar uma partição de diretório do aplicativo gravável, foi retomado após ter sido aplicado a um instantâneo da máquina virtual, após uma operação de importação de máquina virtual, ou após uma operação de migração em tempo real. Controladores de domínio virtualizado não devem ser restauradas usando instantâneos de máquina virtual. O método suportado para restaurar ou reversão o conteúdo de um banco de dados de serviços de domínio do Active Directory é restaurar um backup de estado do sistema feito com um aplicativo de backup de reconhecimento de domínio do Active Directory Services."|  
|**2179**|ActiveDirectory_DomainService|O atributo msDS-GenerationId do objeto de computador do controlador de domínio foi definido como o seguinte parâmetro:<br /><br />Atributo GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory inicializa replicação para trazer o controlador de domínio atual. Evento 2201 será registrado quando a replicação é concluída.|  
|**2201**|ActiveDirectory_DomainService|Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory concluiu a replicação para trazer o controlador de domínio atual.|  
|**2185**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory parou o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:<br /><br />DFSR<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory deve inicializar um não-autorizada na réplica SYSVOL local. Isso é realizado pelo parar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração. Evento 2187 será registrado quando o serviço FRS ou DFSR for reiniciado."|  
|**2208**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory excluído bancos de dados DFSR para inicializar da réplica SYSVOL durante uma restauração não autorizada.<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory precisa inicializar um não-autorizada na réplica SYSVOL local. Para DFSR, isso é feito parar o serviço DFSR, excluindo bancos de dados DFSR e reiniciar o serviço. Ao reiniciar DFSR será recriar os bancos de dados e iniciar a sincronização inicial. "|  
|**2187**|ActiveDirectory_DomainService|Serviços de domínio do Active Directory iniciou o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:<br /><br />DFSR<br /><br />Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory é necessário para inicializar um não-autorizada na réplica SYSVOL local. Isso foi feito interrompendo o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciá-lo com as chaves do registro apropriadas e valores para acionar a restauração. "|  
|**1587**|ActiveDirectory_DomainService|Esse serviço de diretório foi restaurado ou tiver sido configurado para hospedar uma partição de diretório do aplicativo. Como resultado, sua identidade de replicação mudou. Um parceiro solicitou alterações de replicação usando nossa identidade antiga. O número de sequência inicial foi ajustado.<br /><br />O serviço de diretório de destino correspondente ao objeto seguir que GUID solicitou alterações começando em um USN que precede o USN na qual o serviço de diretório local foi restaurado de mídia de backup.<br /><br />GUID de objeto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN no momento da restauração:<br /><br />*<number>*<br /><br />Como resultado, o vetor UTDV do serviço de diretório de destino foi configurado com as seguintes configurações.<br /><br />Banco de dados anterior GUID:<br /><br />*<GUID>*<br /><br />Objeto USN anterior:<br /><br />*<number>*<br /><br />Propriedade USN anterior:<br /><br />*<number>*<br /><br />Novo banco de dados GUID:<br /><br />*<GUID>*<br /><br />Novo objeto USN:<br /><br />*<number>*<br /><br />Nova propriedade USN:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Log de eventos do sistema  
O log de eventos do sistema que notas a hora do computador que ocorre quando trazendo uma máquina virtual offline on-line e sincronizar com o tempo de host. O pool RID invalida e os serviços DFSR ou FRS são reiniciados.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**1**|Kernel geral|A hora do sistema foi alterado para *?<now>* de *< instantâneo hora/data >*.<br /><br />Razão da alteração: Um aplicativo ou um componente do sistema alterada a hora.|  
|**16654**|Diretório de serviços de SAM|Um conjunto de identificadores de conta (RIDs) tiver sido invalidado. Isso pode ocorrer nos seguintes casos esperados:<br /><br />1. Um controlador de domínio é restaurado de backup.<br /><br />2. Um controlador de domínio em execução em uma máquina virtual é restaurado do instantâneo.<br /><br />3. Um administrador tem invalidados manualmente o pool.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=226247 para obter mais informações.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação DFS entrou no estado parado.|  
|**7036**|Gerenciador de controle de serviço|O serviço de replicação DFS entrou no estado em execução.|  
  
##### <a name="application-event-log"></a>Log de eventos de aplicativo  
O log de eventos de aplicativo notas o banco de dados DFSR parar e iniciar.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**103**|ESENT|DFSRs (1360) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: O mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.141, 0.000 [6], [7] 0.000, 0.000 [8], 0.000 [9], 0.000 [10], 0.016 [11], 0.000 [12], [13] 0.000, 0.000 [14], [15] 0.000.|  
|**102**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: O mecanismo de banco de dados (6.02.8189.0000) é iniciar uma nova instância (0).|  
|**105**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: O mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.031, 0.000 [10], 0.000 [11].|  
|||DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: O mecanismo de banco de dados criado um novo banco de dados (1, \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.016, 0.062 [4], [5] 0.000, 0.016 [6], [7] 0.000, 0.000 [8], [9] 0.015, 0.000 [10], 0.000 [11].|  
  
##### <a name="dfs-replication-event-log"></a>Log de eventos de replicação do DFS  
O serviço DFSR for interrompido e o banco de dados que contém SYSVOL é excluído, forçar uma sincronização não autorizada entrada.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**1006**|DFSR|O serviço de replicação do DFS está parando.|  
|**1008**|DFSR|O serviço de replicação DFS foi interrompido.|  
|**1002**|DFSR|O serviço de replicação do DFS está iniciando.|  
|**1004**|DFSR|O serviço de replicação DFS foi iniciado.|  
|**1314**|DFSR|O serviço de replicação DFS configurado com êxito os arquivos de log de depuração.<br /><br />Informações adicionais:<br /><br />Caminho do arquivo de Log de depuração: C:\Windows\debug|  
|**6102**|DFSR|O serviço de replicação DFS foi registrado com êxito o provedor WMI.|  
|**1206**|DFSR|O controlador de domínio contatado com êxito de serviço de replicação DFS * <domain controller FQDN> * para acessar informações de configuração.|  
|**1210**|DFSR|O serviço de replicação DFS configurado com êxito um ouvinte de RPC para solicitações de replicação.<br /><br />Informações adicionais:<br /><br />Porta: 0|  
|**4614**|DFSR|O serviço de replicação DFS inicializada SYSVOL no caminho local C:\Windows\SYSVOL\domain e está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que ele tenha replicada com seu parceiro. Se o servidor estiver no processo sendo promovido para um controlador de domínio, o controlador de domínio não serão anunciar e funcionar como um controlador de domínio até que esse problema seja resolvido. Isso pode ocorrer se o parceiro especificado também está no estado de sincronização inicial, ou se violações de compartilhamento são encontradas nesse servidor ou o parceiro de sincronização. Se esse evento ocorreu durante a migração de SYSVOL do serviço de replicação de arquivos (FRS) para replicação do DFS, as alterações não serão replicadas até que esse problema seja resolvido. Isso pode causar a pasta SYSVOL nesse servidor se torne fora de sincronia com outros controladores de domínio.<br /><br />Informações adicionais:<br /><br />Nome de pasta replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada:*<GUID>*<br /><br />Nome do grupo de replicação: Volume de sistema de domínio<br /><br />ID do grupo de replicação:*<GUID>*<br /><br />ID do membro:*<GUID>*<br /><br />Read-Only: 0|  
|**4604**|DFSR|O serviço de replicação DFS inicializado com êxito a pasta SYSVOL replicado no caminho local C:\Windows\SYSVOL\domain. Esse membro tiver concluído a sincronização inicial de SYSVOL com dc1.corp.contoso.com do parceiro. Para verificar a presença do compartilhamento SYSVOL, abra uma janela de prompt de comando e digite "compartilhamento de rede".<br /><br />Informações adicionais:<br /><br />Nome de pasta replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada:*<GUID>*<br /><br />Nome do grupo de replicação: Volume de sistema de domínio<br /><br />ID do grupo de replicação:*<GUID>*<br /><br />ID do membro:*<GUID>*<br /><br />Parceiro de sincronização:*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restauração de um controlador de domínio que replica SYSVOL usando FRS  
O log de eventos de replicação de arquivo é usado em vez do log de eventos DFSR nesse caso. O log de eventos do aplicativo também grava eventos relacionados à FRS diferentes. Caso contrário, os serviços de diretório e o log de eventos do sistema mensagens geralmente são os mesmos e na mesma ordem como anteriormente descrito.  
  
##### <a name="file-replication-service-event-log"></a>Log de eventos do serviço de replicação de arquivo  
O serviço FRS for interrompido e reiniciado com um valor de BURFLAGS D2 para sincronizar sem autoridade SYSVOL.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**13502**|NTFRS|O serviço de replicação de arquivo está parando.|  
|**13503**|NTFRS|O serviço de duplicação de arquivos foi interrompida.|  
|**13501**|NTFRS|O serviço de replicação de arquivo está iniciando|  
|**13512**|NTFRS|O serviço de duplicação de arquivos detectou um cache de gravação habilitado na unidade que contém o c:\windows\ntfrs\jet diretório no computador DC4. O serviço de duplicação de arquivos não poderá recuperar quando a energia para a unidade for interrompida e atualizações importantes forem perdidas.|  
|**13565**|NTFRS|Serviço de replicação de arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. DC4 do computador não pode se tornar um controlador de domínio até que esse processo seja concluído. O volume do sistema, em seguida, será compartilhado como SYSVOL.<br /><br />Para verificar se há compartilhamento SYSVOL, no prompt de comando, digite:<br /><br />Compartilhamento de rede<br /><br />Quando o serviço de replicação de arquivo conclui o processo de inicialização, o compartilhamento SYSVOL aparecerá.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, a disponibilidade de outros controladores de domínio e o intervalo de replicação entre controladores de domínio."|  
|**13520**|NTFRS|O serviço de duplicação de arquivos movidos os arquivos preexistentes * <path> * para * <path> *\NtFrs_PreExisting___See_EventLog.<br /><br />O serviço de duplicação de arquivos poderá excluir os arquivos em *<path>*\NtFrs_PreExisting___See_EventLog a qualquer momento. Arquivos podem ser salvos contra exclusão copiando-os de *<path>*\NtFrs_PreExisting___See_EventLog. Copiar os arquivos em * <path> * pode causar conflitos de nome se os arquivos já existirem em alguns outros parceiros de replicação.<br /><br />Em alguns casos, o serviço de duplicação de arquivos poderá copiar um arquivo de *<path>*\NtFrs_PreExisting___See_EventLog em *<path>*em vez de duplicar o arquivo de alguns outros parceiros de replicação.<br /><br />Espaço pode ser recuperado a qualquer momento, excluindo os arquivos em * <path> *\NtFrs_PreExisting___See_EventLog.|  
|**13553**|NTFRS|O serviço de duplicação de arquivos adicionado com êxito neste computador para o seguinte conjunto de réplica:<br /><br />"VOLUME DO SISTEMA DE DOMÍNIO (SYSVOL SHARE)"<br /><br />Informações relacionadas a este evento são mostradas abaixo:<br /><br />Nome do computador DNS é "*<domain controller FQDN>*"<br /><br />Nome de membro do conjunto de réplica "*<domain controller name>*"<br /><br />Caminho da réplica conjunto raiz é "*<path>*"<br /><br />Réplica caminho de diretório temporário é "* <path> * "<br /><br />Caminho do diretório de trabalho da réplica é "*<path>*"|  
|**13554**|NTFRS|O serviço de duplicação de arquivos adicionado com êxito as conexões mostradas a seguir para o conjunto de réplica:<br /><br />"VOLUME DO SISTEMA DE DOMÍNIO (SYSVOL SHARE)"<br /><br />Entrada de "*<partner domain controller FQDN>*"<br /><br />Saída para "*<partner domain controller FQDN>*"<br /><br />Mais informações podem aparecer em mensagens de log de eventos subsequentes.|  
|**13516**|NTFRS|O serviço de duplicação de arquivos não são mais está impedindo o computador DC4 de se tornar um controlador de domínio. O volume do sistema foi inicializado com êxito e o serviço de logon de rede tenha sido notificado que o volume do sistema agora está pronto para ser compartilhado como SYSVOL.<br /><br />Tipo de "net share" para verificar se há compartilhamento SYSVOL.|  
  
##### <a name="application-event-log"></a>Log de eventos de aplicativo  
O banco de dados FRS interrompe é iniciado e é removida devido a operação D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID de evento**|**Fonte**|**Mensagem**|  
|**327**|ESENT|NTFRS (1424) o mecanismo de banco de dados desanexado um banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], [2] 0.015, 0.000 [3], [4] 0.000, 0.000 [5], [6] 0.516, 0.000 [7], 0.000 [8], 0.000 [9], 0.000 [10], 0.063 [11], [12] 0.000.<br /><br />Reativada Cache: 0|  
|**103**|ESENT|NTFRS (1424) o mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], 0.000 [3], 0.000 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.031, 0.000 [10], 0.016 [11], 0.000 [12], [13] 0.000, [14] 0.047, 0.000 [15].|  
|**102**|ESENT|NTFRS (3000) o mecanismo de banco de dados (6.02.8189.0000) é iniciar uma nova instância (0).|  
|**105**|ESENT|NTFRS (3000) o mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.062, 0.000 [10], 0.141 [11].|  
|**103**|ESENT|NTFRS (3000) o mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], 0.000 [9], 0.000 [10], 0.000 [11], 0.000 [12], [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|NTFRS (3000) o mecanismo de banco de dados (6.02.8189.0000) é iniciar uma nova instância (0).|  
|**105**|ESENT|NTFRS (3000) o mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.000, 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.078, 0.000 [10], 0.109 [11].|  
|**325**|ESENT|NTFRS (3000) o mecanismo de banco de dados criado um novo banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.016, 0.016 [4], [5] 0.000, 0.015 [6], [7] 0.000, 0.000 [8], [9] 0.078, 0.016 [10], 0.000 [11].|  
|**103**|ESENT|NTFRS (3000) o mecanismo de banco de dados interrompeu a instância (0).<br /><br />Obsceno desligamento: 0<br /><br />Relógio interno sequência: 0.000 [1], 0.000 [2], [3] 0.000, 0.000 [4], [5] 0.078, 0.000 [6], [7] 0.000, 0.000 [8], 0,125 [9], 0.016 [10], 0.000 [11], 0.000 [12], [13] 0.000, 0.000 [14], [15] 0.000.|  
|**102**|ESENT|NTFRS (3000) o mecanismo de banco de dados (6.02.8189.0000) é iniciar uma nova instância (0).|  
|**105**|ESENT|NTFRS (3000) o mecanismo de banco de dados iniciou uma nova instância (0). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.016 [1], [2] 0.000, 0.000 [3], [4] 0.094, 0.000 [5], 0.000 [6], [7] 0.000, 0.000 [8], [9] 0.032, 0.000 [10], 0.000 [11].|  
|**326**|ESENT|NTFRS (3000) o mecanismo de banco de dados anexado a um banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 segundos)<br /><br />Relógio interno sequência: 0.000 [1], [2] 0.015, 0.000 [3], 0.000 [4], [5] 0.016, 0.015 [6], [7] 0.000, 0.000 [8], 0.000 [9], 0.000 [10], 0.000 [11], [12] 0.000.<br /><br />Salva o Cache: 1|  
  


