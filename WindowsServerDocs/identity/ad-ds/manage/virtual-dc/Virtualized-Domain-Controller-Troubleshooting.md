---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Solução de problemas do controlador de domínio virtualizado
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ca58d36599be76e4d196d91f298b9841884e7a36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863657"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Solução de problemas do controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece metodologia detalhada sobre como solucionar problemas do recurso do controlador de domínio virtualizado.  
  
-   [Solução de problemas virtualizados a clonagem do controlador de domínio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Solução de problemas de restauração segura do controlador de domínio virtualizado](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introdução  
A forma mais importante de melhorar suas habilidades de solução de problemas é criar um laboratório de testes e examinar rigorosamente os cenários normais de trabalho. Se encontrar erros, eles serão mais óbvios e fáceis de entender, uma vez que você tem agora uma base sólida de como a promoção do controlador de domínio funciona. Isso também permite que você construa sua análise e as habilidades de análise de rede. Isso vale para todas as tecnologias de sistemas distribuídos, e não apenas na implantação do controlador de domínio virtualizado.  
  
Os elementos críticos para a solução avançada de problemas de configuração do controlador de domínio são:  
  
1.  Análise linear combinada com foco e atenção aos detalhes.  
  
2.  Entendimento da análise de captura de rede  
  
3.  Entendimento dos logs internos  
  
O primeiro e o segundo estão além do escopo deste tópico, mas o terceiro pode ser explicado com algum detalhe. A solução de problemas do controlador de domínio virtualizado requer um método lógico e linear. O importante é abordar o problema usando os dados fornecidos e só recorrer a ferramentas e análises complexas quando você esgotar a saída e o registro em log fornecidos.  
  
## <a name="BKMK_TshootVDCCloning"></a>Solução de problemas virtualizados a clonagem do controlador de domínio  
Esta seção abrange:  
  
-   [Ferramentas para solução de problemas](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Opções de log](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Metodologia geral para solução de problemas de domínio a clonagem do controlador](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core e o Log de eventos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Solucionando problemas específicos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
A estratégia de solução de problemas de clonagem do controlador de domínio virtualizado segue este formato geral:  
  
![solução de problemas do controlador de domínio virtual](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Ferramentas para solução de problemas  
  
#### <a name="BKMK_LoggingOptions"></a>Opções de log  
Os logs internos são a ferramenta mais importante para a solução de problemas de clonagem do controlador de domínio. Todos esses logs são habilitados e configurados para o máximo de detalhamento, por padrão.  
  
|||  
|-|-|  
|**operação**|**Log**|  
|**Clonagem**|-Eventos Visualizador logs\system.<br />-Event viewer\Applications and services \ serviço de diretório<br />-   %systemroot%\debug\dcpromo.log|  
|**Promoção**|-   %systemroot%\debug\dcpromo.log<br />-Event viewer\Applications and services \ serviço de diretório<br />-Eventos Visualizador logs\system.<br />-Event viewer\Applications and services logs\File serviço de replicação<br />-Event viewer\Applications and services \ replicação do DFS|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e comandos para solução de problemas de configuração do controlador de domínio  
Para solucionar problemas não explicados pelos logs, use as seguintes ferramentas como um ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Monitor de Rede 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>Metodologia geral para solução de problemas de domínio a clonagem do controlador  
  
1.  A VM está sendo inicializada em DSRM (Modo de Restauração de DS)? Isso indica que a solução de problemas é necessária. Para fazer logon no DSRM, use a conta **.\Administrator** e especifique a senha do DSRM.  
  
    1.  Examine Dcpromo.log.  
  
        1.  As etapas de clonagem iniciais foram bem-sucedidas, mas houve erro na promoção do controlador de domínio?  
  
        2.  Os erros indicam problemas com o controlador de domínio local ou com o ambiente AD DS, como erros retornados do emulador PDC?  
  
    2.  Examine os logs de evento do Sistema e Serviços de Diretório e o dccloneconfig.xml e CustomDCCloneAllowList.xml.  
  
        1.  Um aplicativo incompatível precisa estar na lista de permissões CustomDCCloneAllowList.xml?  
  
        2.  O endereço IP ou nome do computador, duplicado ou inválido, está no dccloneconfig.xml?  
  
        3.  O site do Active Directory é inválido no dccloneconfig.xml?  
  
        4.  O endereço IP não está configurado no dccloningconfig.xml e não há servidor DHCP disponível?  
  
        5.  O emulador PDC está online e disponível pelo protocolo RPC?  
  
        6.  O controlador de domínio é um membro do grupo de Controladores de Domínio Clonáveis? A permissão **'Permitir que um controlador de domínio crie um clone dele mesmo** está configurada na raiz de domínio desse grupo?  
  
        7.  O arquivo Dccloneconfig.xml contém erros de sintaxe que impedem a análise correta?  
  
        8.  O hipervisor é compatível?  
  
        9. A promoção do controlador de domínio falhou após o início bem-sucedido da clonagem?  
  
        10. O número máximo de nomes de controladores de domínio gerados automaticamente (9.999) foi excedido?  
  
        11. O endereço MAC está duplicado?  
  
2.  O nome de host do clone é o mesmo que o DC de origem?  
  
    1.  Existe um arquivo Dccloneconfig.xml em um dos locais permitidos?  
  
3.  A inicialização da VM está no modo normal e a clonagem concluída, mas o controlador de domínio não está funcionando corretamente?  
  
    1.  Primeiro verifique se o nome do host está alterado no clone. Se o nome do host for diferente, a clonagem terá sido concluída parcialmente.  
  
    2.  O controlador de domínio tem um endereço IP duplicado do controlador de domínio de origem do dccloneconfig.xml, mas o controlador de domínio de origem estava offline durante a clonagem?  
  
    3.  Se o controlador de domínio for de publicidade, trate o problema como qualquer problema de pós-promoção normal que você teria sem a clonagem.  
  
    4.  Se o controlador de domínio não for de publicidade, examine os logs de evento Serviço de Diretório, Sistema, Aplicativo, Replicação de Arquivos e Replicação do DFS quanto a erros de pós-promoção.  
  
#### <a name="disabling-dsrm-boot"></a>Desabilitando a inicialização do DSRM  
Uma vez iniciado em DSRM devido a qualquer erro, diagnostique a causa da falha e se o dcpromo.log não indicar que a clonagem não pode ser repetida, corrija a causa da falha e reinicie o sinalizador DSRM. Um clone com falha não retorna ao modo normal por conta própria na próxima reinicialização. Você deve remover o sinalizador de inicialização do Modo de Restauração DS para tentar a clonagem novamente. Todas essas etapas requerem execução com permissões de administrador.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Removendo o DSRM com Msconfig.exe  
Para desativar a inicialização do DSRM usando uma GUI, use a ferramenta Configuração do Sistema:  
  
1.  Execute msconfig.exe.  
  
2.  Na guia **Inicializar**, em**Opções de Inicialização**, desmarque **Inicialização segura** (já está selecionada com a opção **Reparo do Active Directory** habilitada).  
  
3.  Clique em OK e reinicie quando solicitado  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Removendo o DSRM com Bcdedit.exe  
Para desativar a inicialização do DSRM da linha de comando, use o Editor de Repositório de Dados de Configuração da Inicialização:  
  
1.  Abra um prompt de CMD e execute:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Reinicie o computador com:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe também funciona em um console do Windows PowerShell. Os comandos são:  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core e o Log de eventos  
Os logs de eventos contêm grande parte das informações úteis sobre as operações de clonagem do controlador de domínio virtualizado. Por padrão, uma instalação do Windows Server 2012 no computador é uma instalação Server Core, o que significa que não há interface gráfica e, portanto, não há maneira de executar o snap-in do Visualizador de Eventos local.  
  
Para analisar os logs de eventos em um servidor com uma instalação Server Core:  
  
-   Execute a ferramenta Wevtutil.exe localmente  
  
-   Execute o cmdlet do PowerShell Get-WinEvent localmente  
  
-   Se você tiver habilitado as regras de Firewall avançado do Windows para os grupos de "Gerenciamento remoto do Log de eventos" (ou portas equivalentes) permitir a comunicação de entrada, você pode gerenciar o log de eventos remotamente usando Eventvwr.exe, wevtutil.exe ou Get-Winevent. Isso pode ser feito na instalação do Server Core usando NETSH.exe, a Política de Grupo ou o novo cmdlet Set-NetFirewallRule no Windows PowerShell 3.0.  
  
> [!WARNING]  
> Não tente adicionar o shell gráfico de volta ao computador enquanto ele estiver no DSRM. A pilha de atendimento do Windows (CBS) não pode funcionar corretamente enquanto está em Modo de Segurança ou DSRM. As tentativas de adicionar recursos ou funções enquanto está no DSRM não serão concluídas e deixam o computador em um estado instável até que ele seja iniciado normalmente. Uma vez que um clone do controlador de domínio virtualizado no DSRM não pode ser iniciado normalmente, e não deve ser inicializado normalmente na maioria das circunstâncias, é impossível adicionar o shell gráfico com segurança. Essa ação é incompatível e pode deixá-lo com um servidor inutilizável.  
  
### <a name="BKMK_SpecificProblems"></a>Solucionando problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos os eventos de clonagem do controlador de domínio virtualizado são gravados no log de eventos dos Serviços de Diretório da VM do controlador de domínio do clone. Os logs de eventos Aplicativo, Serviço de Replicação de Arquivo e Replicação do DFS também podem conter informações úteis de solução de problemas para clonagem com falha. Falhas durante a chamada de RPC para o emulador PDC podem estar disponíveis no log de eventos do emulador PDC.  
  
Abaixo estão os eventos específicos de clonagem do Windows Server 2012 do log de eventos dos Serviços de Diretório, com notas e soluções sugeridas para os erros.  
  
##### <a name="directory-services-event-log"></a>Log de eventos dos Serviços de Diretório  
  
|||  
|-|-|  
|**ID do evento**|**2160**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O local *<COMPUTERNAME>* encontrou um arquivo de configuração de clonagem do controlador de domínio virtual.<br /><br />O arquivo de configuração de clonagem do controlador de domínio virtual é encontrado em: %1<br /><br />A existência do arquivo de configuração de clonagem do controlador de domínio virtual indica que o controlador de domínio virtual local é um clone de outro controlador de domínio virtual. O *<COMPUTERNAME>* dará início à própria clonagem.|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado. Examine o Diretório de Trabalho DSA, %systemroot%\ntds, e retire qualquer disco local ou removível do arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID do evento**|**2161**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O local *<COMPUTERNAME>* não localizou o arquivo de configuração de clonagem do controlador de domínio virtual. O computador local não é um DC clonado.|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado. Examine o Diretório de Trabalho DSA, %systemroot%\ntds, e retire qualquer disco local ou removível do arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID do evento**|**2162**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha na clonagem do controlador de domínio virtual.<br /><br />Verifique os eventos registrados nos logs de eventos do Sistema e o %systemroot%\debug\dcpromo.log para obter mais informações sobre os erros que correspondem à tentativa de clonagem do controlador de domínio virtual.<br /><br />Código de erro: %1|  
|**Notas e resolução**|Siga as instruções da mensagem. Este erro é uma resposta genérica.|  
  
|||  
|-|-|  
|**ID do evento**|**2163**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O serviço DsRoleSvc foi iniciado para clonar o controlador de domínio virtual local.|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado. Examine o Diretório de Trabalho DSA, %systemroot%\ntds, e retire qualquer disco local ou removível do arquivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID do evento**|**2164**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao iniciar o serviço DsRoleSvc para clonar o controlador de domínio virtual local.|  
|**Notas e resolução**|Examine as configurações de serviço para o serviço Servidor de Função DS (DsRoleSvc) e verifique se o tipo de inicialização está definido como Manual. Valide que nenhum programa de terceiros está impedindo o início desse serviço.|  
  
|||  
|-|-|  
|**ID do evento**|**2165**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao iniciar um thread durante a clonagem do controlador de domínio virtual local.<br /><br />Código de erro:%1<br /><br />Mensagem de erro:%2<br /><br />Nome do thread:%3|  
|**Notas e resolução**|Entre em contato com o Microsoft Product Support|  
  
|||  
|-|-|  
|**ID do evento**|**2166**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* precisa do serviço RPCSS para reinicializar no DSRM. Falha ao aguardar a inicialização do RPCSS em um estado de execução.<br /><br />Código de erro:%1|  
|**Notas e resolução**|Examine o log de eventos do Sistema e as configurações de serviço para o serviço do Servidor RPC (Rpcss)|  
  
|||  
|-|-|  
|**ID do evento**|**2167**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* não foi possível inicializar o conhecimento do controlador de domínio virtual. Veja a entrada do log de eventos anterior para obter detalhes.<br /><br />Dados adicionais<br /><br />Código de falha:%1|  
|**Notas e resolução**|Siga as instruções da mensagem. Este erro é uma resposta genérica.|  
  
|||  
|-|-|  
|**ID do evento**|**2168**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />O DC está sendo executado em um hipervisor compatível. ID de Geração de VM detectada.<br /><br />Valor atual da ID de Geração de VM: %1|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2169**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Não foi detectada uma ID de Geração de VM. O DC está hospedado em uma máquina física, uma versão inferior do Hyper-V ou um hipervisor não compatível com a ID de Geração da VM.<br /><br />Dados adicionais<br /><br />Código de falha retornado ao verificar a ID de Geração de VM:%1|  
|**Notas e resolução**|Este é um evento de sucesso quando não destinado à clonagem. Caso contrário, examine o log de eventos do Sistema e verifique a documentação de suporte ao produto do hipervisor.|  
  
|||  
|-|-|  
|**ID do evento**|**2170**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Aviso|  
|**Mensagem**|Uma alteração na ID de Geração foi detectada.<br /><br />ID de Geração armazenada em cache no DS (valor antigo):%1<br /><br />ID de Geração atualmente na VM (novo valor):%2<br /><br />A alteração da ID de Geração ocorre após a aplicação de um instantâneo de máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. *<COMPUTERNAME>* criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método permitido para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory.|  
|**Notas e resolução**|Este é um evento de sucesso quando destinado à clonagem. Caso contrário, examine o log de eventos do Sistema.|  
  
|||  
|-|-|  
|**ID do evento**|**2171**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Nenhuma alteração na ID de Geração foi detectada.<br /><br />ID de Geração armazenada em cache no DS (valor antigo):%1<br /><br />ID de Geração atualmente na VM (novo valor):%2|  
|**Notas e resolução**|Este é um evento de sucesso quando não destinado à clonagem, e deve ser visto em cada reinicialização de um DC virtualizado. Caso contrário, examine o log de eventos do Sistema.|  
  
|||  
|-|-|  
|**ID do evento**|**2172**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Leia o atributo msDS-GenerationId do objeto de computador do Controlador de Domínio.<br /><br />Valor do atributo msDS-GenerationId:%1|  
|**Notas e resolução**|Este é um evento de sucesso quando destinado à clonagem. Caso contrário, examine o log de eventos do Sistema.|  
  
|||  
|-|-|  
|**ID do evento**|**2173**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Falha ao ler o atributo msDS-GenerationId do objeto de computador do Controlador de Domínio. Isso pode ser causado por uma falha de transação do banco de dados, ou a ID de geração não existe no banco de dados local. O msDS-GenerationId não existe durante a primeira reinicialização após dcpromo ou o DC não é um controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Código de falha:%1|  
|**Notas e resolução**|Este é um evento de sucesso, se destinado à clonagem, e é a primeira reinicialização da VM após a conclusão da clonagem. Ele também pode ser ignorado em controladores de domínio não virtuais. Caso contrário, examine o log de eventos do Sistema.|  
  
|||  
|-|-|  
|**ID do evento**|**2174**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O DC não é nem um clone do controlador de domínio virtual nem um instantâneo do controlador de domínio virtual restaurado.|  
|**Notas e resolução**|Este é um evento de sucesso quando não destinado à clonagem. Caso contrário, examine o log de eventos do Sistema.|  
  
|||  
|-|-|  
|**ID do evento**|**2175**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|O arquivo de configuração do clone do controlador de domínio virtual existe em uma plataforma incompatível.|  
|**Notas e resolução**|Isso ocorre quando um dccloneconfig.xml é encontrado, mas foi possível encontrar uma ID de Geração de VM, por exemplo, quando um arquivo dccloneconfig.xml é encontrado em um computador físico ou em um hipervisor não compatível com ID de Geração de VM.|  
  
|||  
|-|-|  
|**ID do evento**|**2176**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Arquivo de configuração de clone do controlador de domínio virtual renomeado.<br /><br />Dados adicionais<br /><br />Antigo nome do arquivo:%1<br /><br />Novo nome do arquivo:%2|  
|**Notas e resolução**|Renomeação esperada durante a inicialização de um backup de VM de origem, porque a ID de Geração de VM não foi alterada. Isso impede que o controlador de domínio de origem tente clonar.|  
  
|||  
|-|-|  
|**ID do evento**|**2177**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha ao renomear o arquivo de configuração de clone do controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Nome do arquivo:%1<br /><br />Código de falha:%2 %3|  
|**Notas e resolução**|Tentativa de renomeação esperada durante a inicialização de um backup de VM de origem, porque a ID de Geração de VM não foi alterada. Isso impede que o controlador de domínio de origem tente clonar. Renomeie manualmente o arquivo e investigue produtos de terceiros instalados que possam estar impedindo a renomeação de arquivos.|  
  
|||  
|-|-|  
|**ID do evento**|**2178**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Arquivo de configuração de clone do controlador de domínio virtual detectado, mas a ID de Geração de VM não foi alterada. O DC local é o DC de origem do clone. Renomeie o arquivo de configuração do clone.|  
|**Notas e resolução**|Esperado durante a inicialização de um backup de VM de origem, porque a ID de Geração de VM não foi alterada. Isso impede que o controlador de domínio de origem tente clonar.|  
  
|||  
|-|-|  
|**ID do evento**|**2179**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O atributo msDS-GenerationId do objeto de computador do Controlador de Domínio foi definido para o seguinte parâmetro:<br /><br />Atributo GenerationID:%1|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2180**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Aviso|  
|**Mensagem**|Falha ao definir o atributo msDS-GenerationId do objeto de computador do Controlador de Domínio.<br /><br />Dados adicionais<br /><br />Código de falha:%1|  
|**Notas e resolução**|Examine o log de eventos do Sistema e o Dcpromo.log. Procure pelo erro específico no MS TechNet, na Base de Dados de Conhecimento MS e nos blogs da MS para determinar seu significado usual, e depois solucione problemas com base nesses resultados.|  
  
|||  
|-|-|  
|**ID do evento**|**2182**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Evento interno: O Serviço de Diretório foi solicitado a clonar um DSA remoto:|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2183**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|Evento interno: *<COMPUTERNAME>* concluiu a solicitação para clonar o Directory System Agent.<br /><br />Nome original do DC:%3<br /><br />Solicitar nome do DC do clone:%4<br /><br />Solicitar local do DC do clone:%5<br /><br />Dados adicionais<br /><br />Valor do erro:%1 %2|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2184**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao criar uma conta de controlador de domínio para o DC clonado.<br /><br />Nome original do DC: %1<br /><br />Número permitido de DC clonado:%2<br /><br />O limite no número de contas do controlador de domínio que pode ser gerado pela clonagem *<COMPUTERNAME>* foi excedido.|  
|**Notas e resolução**|Um único nome do controlador de domínio de origem só pode ser gerado automaticamente 9.999 vezes se os controladores de domínio não forem rebaixados, com base na convenção de nomenclatura. Use o elemento <computername> no XML para gerar um novo nome exclusivo ou clone de um DC com nome diferente.|  
  
|||  
|-|-|  
|**ID do evento**|**2191**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* Defina o seguinte valor de registro para desabilitar atualizações de DNS.<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem. O processo de clonagem permitirá atualizações de DNS novamente após a conclusão da clonagem.|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2192**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao definir o seguinte valor de registro para desabilitar atualizações de DNS.<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />Código de erro: %4<br /><br />Mensagem de erro: %5<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando as atualizações de Registro.|  
  
|||  
|-|-|  
|**ID do evento**|**2193**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* Defina o seguinte valor de registro para habilitar atualizações de DNS.<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem.|  
|**Notas e resolução**|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|**ID do evento**|**2194**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao definir o seguinte valor de registro para habilitar atualizações de DNS.<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />Código de erro: %4<br /><br />Mensagem de erro: %5<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando as atualizações de Registro.|  
  
|||  
|-|-|  
|**ID do evento**|**2195**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha ao definir a inicialização do DSRM.<br /><br />Código de erro:%1<br /><br />Mensagem de erro:%2<br /><br />Quando a clonagem do controlador de domínio virtual falhar ou o arquivo de configuração de clone do controlador de domínio virtual aparecer em um hipervisor sem suporte, o computador local reinicializará no DSRM para solucionar os problemas. Falha na configuração de inicialização do DSRM.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando as atualizações de Registro.|  
  
|||  
|-|-|  
|**ID do evento**|**2196**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha ao habilitar privilégio de desligamento.<br /><br />Código de erro:%1<br /><br />Mensagem de erro:%2<br /><br />Quando a clonagem do controlador de domínio virtual falhar ou o arquivo de configuração de clone do controlador de domínio virtual aparecer em um hipervisor sem suporte, o computador local reinicializará no DSRM para solucionar os problemas. Falha ao habilitar privilégio de desligamento.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando o uso dos privilégios.|  
  
|||  
|-|-|  
|**ID do evento**|**2197**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha ao iniciar o desligamento do sistema.<br /><br />Código de erro:%1<br /><br />Mensagem de erro:%2<br /><br />Quando a clonagem do controlador de domínio virtual falhar ou o arquivo de configuração de clone do controlador de domínio virtual aparecer em um hipervisor sem suporte, o computador local reinicializará no DSRM para solucionar os problemas. Falha ao iniciar o desligamento do sistema.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando o uso dos privilégios.|  
  
|||  
|-|-|  
|**ID do evento**|**2198**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao criar ou modificar o seguinte objeto DC clonado.<br /><br />Dados adicionais:<br /><br />Objeto:<br /><br />%1<br /><br />Valor do erro: %2<br /><br />%3|  
|**Notas e resolução**|Procure pelo erro específico no MS TechNet, na Base de Dados de Conhecimento MS e nos blogs da MS para determinar seu significado usual, e depois solucione problemas com base nesses resultados.|  
  
|||  
|-|-|  
|**ID do evento**|**2199**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao criar o seguinte objeto DC clonado porque o objeto já existe.<br /><br />Dados adicionais:<br /><br />DC de origem:<br /><br />%1<br /><br />Objeto:<br /><br />%2|  
|**Notas e resolução**|A validação do dccloneconfig.xml não especificou um controlador de domínio existente ou que cópias do dccloneconfig.xml foram usadas em vários clones sem edição do nome. Se a colisão ainda for inesperada, determine qual administrador a promoveu. Contate-o para discutir se o controlador de domínio existente deve ser rebaixado, se os metadados existentes do controlador de domínio devem ser limpos ou se o clone deve usar um nome diferente.|  
  
|||  
|-|-|  
|**ID do evento**|**2203**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|Falha na última clonagem do controlador de domínio virtual. Esta é a primeira reinicialização desde então, portanto, esta deve ser uma nova tentativa da clonagem. No entanto, o arquivo de configuração de clone do controlador de domínio virtual não existe nem foi detectada alteração na ID de geração da máquina virtual. Inicialize no DSRM.<br /><br />Falha na última clonagem do controlador de domínio virtual:%1<br /><br />O arquivo de configuração de clone do controlador de domínio virtual existe:%2<br /><br />Detectada alteração na ID de geração da máquina virtual:%3|  
|**Notas e resolução**|Esperado se houve falha anterior na clonagem, em razão de dccloneconfig.xml ausente ou inválido|  
  
|||  
|-|-|  
|ID de evento|2210|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Falha do <COMPUTERNAME> ao criar objetos para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %6<br /><br />Nome do controlador de domínio do clone: %1<br /><br />Loop de nova tentativa: %2<br /><br />Valor da exceção: %3<br /><br />Valor do erro: %4<br /><br />DSID: %5|  
|Notas e resolução|Verifique os logs de eventos do Sistema e dos Serviços de Diretório e o dcpromo.log para mais detalhes sobre por que a clonagem falhou.|  
  
|||  
|-|-|  
|ID de evento|2211|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criou objetos para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %3<br /><br />Nome do controlador de domínio do clone: %1<br /><br />Loop de nova tentativa: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2212|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> começou a criar objetos para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Nome do clone: %2<br /><br />Site do clone: %3<br /><br />RODC do clone: %4|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2213|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criou um novo objeto KrbTgt para clonagem do controlador de domínio somente leitura.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Nova GUID do Objeto KrbTgt: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2214|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará um objeto de computador para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Controlador de domínio original: %2<br /><br />Controlador de domínio do clone: %3|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2215|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> irá adicionar o controlador de domínio do clone no seguinte site.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Site: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2216|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará um contêiner de servidores para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Contêiner de Servidores: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2217|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará um objeto de servidor para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Objeto de Servidor: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2218|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará um objeto Configurações NTDS para o controlador de domínio do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Objeto: %2|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2219|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará objetos de conexão para o controlador de domínio somente leitura do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2220|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> criará objetos SYSVOL para o controlador de domínio somente leitura do clone.<br /><br />Dados adicionais:<br /><br />ID do clone: %1|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2221|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Falha do <COMPUTERNAME> ao gerar uma senha aleatória para o controlador de domínio clonado.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Nome do controlador de domínio do clone: %2<br /><br />Erro: %3 %4|  
|Notas e resolução|Examine o log de eventos do sistema para obter mais detalhes sobre por que a senha da conta do computador não pôde ser criada.|  
  
|||  
|-|-|  
|ID de evento|2222|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Falha do <COMPUTERNAME> ao definir senha para o controlador de domínio clonado.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Nome do controlador de domínio do clone: %2<br /><br />Erro: %3 %4|  
|Notas e resolução|Examine o log de eventos do sistema para obter mais detalhes sobre por que a senha da conta do computador não pôde ser definida.|  
  
|||  
|-|-|  
|ID de evento|2223|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|O <COMPUTERNAME> definiu com sucesso a senha da conta do computador para o controlador do domínio clonado.<br /><br />Dados adicionais:<br /><br />ID do clone: %1<br /><br />Nome do controlador de domínio do clone: %2<br /><br />Tempo total de novas tentativas: %3|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2224|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Falha na clonagem do controlador de domínio virtual. As seguintes %1 Contas de Serviço Gerenciado existem no computador clonado:<br /><br />%2<br /><br />Para a clonagem ter sucesso, todas as Contas de Serviço Gerenciado devem ser removidas. Isso pode ser feito usando o cmdlet do PowerShell Remove-ADComputerServiceAccount.|  
|Notas e resolução|Previsto ao usar MSAs autônomos (não MSA em grupo). *Não* siga o aviso de evento para remover a conta - está escrito incorretamente. Use desinstalar-AdServiceAccount - [ https://technet.microsoft.com/library/hh852310 ](https://technet.microsoft.com/library/hh852310).<br /><br />MSAs autônomos - lançados pela primeira vez no Windows Server 2008 R2 - foram substituídos no Windows Server 2012 por MSAs em grupo (gMSA). Clonagem de suporte a GMSAs.|  
  
|||  
|-|-|  
|ID de evento|2225|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Informativo|  
|Mensagem|Os segredos armazenados em cache da seguinte entidade de segurança foram removidos com sucesso do controlador de domínio local:<br /><br />%1<br /><br />Após a clonagem de um controlador de domínio somente leitura, os segredos previamente armazenados em cache no controlador de domínio somente leitura da origem da clonagem serão removidos do controlador do domínio clonado.|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|2226|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Falha ao remover segredos em cache da seguinte entidade de segurança do controlador de domínio local:<br /><br />%1<br /><br />Erro: %2 (%3)<br /><br />Após a clonagem de um controlador de domínio somente leitura, os segredos previamente armazenados em cache no controlador de domínio somente leitura da origem da clonagem precisam ser removidos do clone para diminuir o risco de um invasor obter essas credenciais de um clone roubado ou comprometido. Se a entidade de segurança for uma conta altamente privilegiada e deve ser protegida contra isso, use a operação rootDSE rODCPurgeAccount para limpar manualmente seus segredos no controlador de domínio local.|  
|Notas e resolução|Examine os logs de eventos do Sistema e dos Serviços de Diretório para obter mais informações.|  
  
|||  
|-|-|  
|ID de evento|2227|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|Uma exceção é gerada ao tentar remover segredos armazenados em cache do controlador de domínio local.<br /><br />Dados adicionais:<br /><br />Valor da exceção: %1<br /><br />Valor do erro: %2<br /><br />DSID: %3<br /><br />Após a clonagem de um controlador de domínio somente leitura, os segredos previamente armazenados em cache no controlador de domínio somente leitura da origem da clonagem precisam ser removidos do clone para diminuir o risco de um invasor obter essas credenciais de um clone roubado ou comprometido. Se qualquer uma das entidades de segurança for uma conta altamente privilegiada e dever ser protegida contra isso, use a operação rootDSE rODCPurgeAccount para limpar manualmente seus segredos no controlador de domínio local.|  
|Notas e resolução|Examine os logs de eventos do Sistema e dos Serviços de Diretório para obter mais informações.|  
  
|||  
|-|-|  
|ID de evento|2228|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravidade|Erro|  
|Mensagem|A ID de geração da máquina virtual no banco de dados do Active Directory deste controlador de domínio é diferente do valor atual da máquina virtual. No entanto, um arquivo de configuração de clone do controlador de domínio virtual (DCCloneConfig.xml) não pôde ser localizado, de modo que não foi feita a tentativa de clonagem do controlador de domínio. Se uma operação de clonagem do controlador de domínio foi pretendida, garanta que um DCCloneConfig.xml seja fornecido em qualquer um dos locais de suporte. Além disso, o endereço IP deste controlador de domínio entra em conflito com o endereço IP de outro controlador de domínio. Para garantir que não haja interrupções no serviço, o controlador de domínio foi configurado para inicializar no DSRM.<br /><br />Dados adicionais:<br /><br />O endereço IP duplicado: %1|  
|Notas e resolução|Este mecanismo de proteção para os controladores de domínio duplicados quando possível (não o fará durante o uso de DHCP, por exemplo). Adicione um arquivo DcCloneConfig.xml válido, remova o sinalizador de DSRM e tente nova clonagem.|  
  
|||  
|-|-|  
|ID de evento|29218|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na clonagem do controlador de domínio virtual. A operação de clonagem não pôde ser concluída e o controlador de domínio clonado foi reinicializado em DSRM (Modo de Restauração dos Serviços de Diretório).<br /><br />Verifique os eventos anteriormente registrados e o %systemroot%\debug\dcpromo.log para mais informações sobre os erros que correspondem à tentativa de clonagem do controlador de domínio virtual e se esta imagem do clone pode ser reutilizada ou não.<br /><br />Se uma ou mais entradas de log indicarem que o processo de clonagem não pode ser repetido, a imagem deverá ser destruída com segurança. Caso contrário, você pode corrigir os erros, desmarcar o sinalizador de inicialização do DSRM e reinicializar normalmente. Ao reinicializar, a operação de clonagem será repetida.|  
|Notas e resolução|Verifique os logs de eventos do Sistema e dos Serviços de Diretório e o dcpromo.log para mais detalhes sobre por que a clonagem falhou.|  
  
|||  
|-|-|  
|ID de evento|29219|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Informativo|  
|Mensagem|Clonagem do controlador de domínio virtual bem-sucedida.|  
|Notas e resolução|Este é um evento de sucesso e é um problema apenas se for inesperado.|  
  
|||  
|-|-|  
|ID de evento|29248|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha da clonagem do controlador de domínio virtual ao obter Notificação de Winlogon. O código de erro retornado é %1 (%2).<br /><br />Para mais informações sobre este erro, consulte o %systemroot%\debug\dcpromo.log quanto a erros que correspondem à tentativa de clonagem do controlador de domínio virtual.|  
|Notas e resolução|Entre em contato com o Microsoft Product Support|  
  
|||  
|-|-|  
|ID de evento|29249|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha da clonagem do controlador de domínio virtual ao analisar o arquivo de configuração do controlador de domínio virtual.<br /><br />O código HRESULT retornado é 1%.<br /><br />O arquivo de configuração é:%2<br /><br />Corrija os erros no arquivo de configuração e repita a operação de clonagem.<br /><br />Para mais informações sobre este erro, consulte %systemroot%\debug\dcpromo.log.|  
|Notas e resolução|Examine o arquivo dclconeconfig.xml quanto a erros de sintaxe usando um editor XML e o arquivo de esquema DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|ID de evento|29250|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na clonagem do controlador de domínio virtual. Existem software ou serviços habilitados atualmente no controlador de domínio virtual clonado que não estão presentes na lista de aplicativos autorizados para clonagem do controlador de domínio virtual.<br /><br />A seguir, as entradas ausentes:<br /><br />%2<br /><br />% 1 (se houver) foi usado como a lista de inclusão definida.<br /><br />A operação de clonagem não poderá ser concluída se houver aplicativos não clonáveis instalados.<br /><br />Execute o cmdlet Active Directory PowerShell Get-ADDCCloningExcludedApplicationList para verificar quais aplicativos estão instalados no computador clonado, mas não incluídos na lista de permissões, e adicione-os à lista de permissões, se forem compatíveis com a clonagem do controlador de domínio virtual. Se algum desses aplicativos não for compatível com a clonagem do controlador de domínio virtual, desinstale-o antes de tentar novamente a operação de clonagem.<br /><br />O processo de clonagem do controlador de domínio virtual pesquisa o arquivo de lista de aplicativos permitidos, CustomDCCloneAllowList.xml, com base na ordem de pesquisa abaixo. O primeiro arquivo encontrado é usado e todos os outros são ignorados:<br /><br />1. O nome de valor do Registro: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. O mesmo diretório onde a pasta Diretório de Trabalho DSA reside<br /><br />3. %windir%\NTDS<br /><br />4. Mídia de leitura/gravação removível em ordem de letra da unidade, na raiz da unidade|  
|Notas e resolução|Siga as instruções da mensagem|  
  
|||  
|-|-|  
|ID de evento|29251|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha da clonagem do controlador de domínio virtual em redefinir os endereços IP do computador clone.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Esse erro pode ser causado por configuração incorreta nas seções de configuração de rede no arquivo de configuração do controlador de domínio virtual.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre erros que correspondem à redefinição de endereços IP durante as tentativas de clonagem do controlador de domínio virtual.<br /><br />Detalhes sobre a redefinição de endereços IP no computador clonado podem ser encontrados em https://go.microsoft.com/fwlink/?LinkId=208030|  
|Notas e resolução|Verifique se as informações de IP definidas no dccloneconfig.xml são válidas e não duplicam o computador original.|  
  
|||  
|-|-|  
|ID de evento|29253|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na clonagem do controlador de domínio virtual. O controlador de domínio do clone não conseguiu localizar as operações mestras do PDC (Controlador de Domínio Primário) no domínio inicial do computador clonado.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Verifique se o controlador de domínio primário no domínio inicial do computador clonado está atribuído a um controlador de domínio ativo, está online e operacional. Verifique se o computador clonado tem conectividade LDAP/RPC com o controlador de domínio primário nas portas e protocolos necessários.|  
|Notas e resolução|Confirme se as informações de IP e DNS do controlador de domínio clonado estão definidas. Use Dcdiag.exe /test:locatorcheck para confirmar se o PDCE está online, use Nltest.exe. exe /server:*<PDCE>* /dclist:*<domain>* válido RPC, obtenha uma captura de rede do PDCE durante a a clonagem falhará e analisar o tráfego.|  
  
|||  
|-|-|  
|ID de evento|29254|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha da clonagem do controlador de domínio virtual em associar ao controlador de domínio primário %1.<br /><br />O código de erro retornado é %2 (%3).<br /><br />Verifique se o controlador de domínio primário % 1 está online e está operacional. Verifique se o computador clonado tem conectividade LDAP/RPC com o controlador de domínio primário nas portas e protocolos necessários.|  
|Notas e resolução|Confirme se as informações de IP e DNS do controlador de domínio clonado estão definidas. Use Dcdiag.exe /test:locatorcheck para confirmar se o PDCE está online, use Nltest.exe. exe /server:*<PDCE>* /dclist:*<domain>* válido RPC, obtenha uma captura de rede do PDCE durante a a clonagem falhará e analisar o tráfego.|  
  
|||  
|-|-|  
|ID de evento|29255|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na clonagem do controlador de domínio virtual.<br /><br />Uma tentativa de criar objetos no controlador de domínio primário %1, necessários à imagem que está sendo clonada, retornou o erro %2 (%3).<br /><br />Verifique se o controlador de domínio clonado tem privilégio para clonar a si mesmo. Verifique se há eventos relacionados no log de eventos do Serviço de Diretório no controlador de domínio primário %1.|  
|Notas e resolução|Procure pelo erro específico no MS TechNet, na Base de Dados de Conhecimento MS e nos blogs da MS para determinar seu significado típico, e depois solucione problemas com base nesses resultados.|  
  
|||  
|-|-|  
|ID de evento|29256|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na tentativa de definir o sinalizador Inicialização no Modo de Restauração dos Serviços de Diretório com o código de erro %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre os erros.|  
|Notas e resolução|Examine o log dos Serviços de Diretório e o dcpromo.log para mais detalhes. Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando o uso dos privilégios.|  
  
|||  
|-|-|  
|ID de evento|29257|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|A clonagem do controlador de domínio virtual foi feita. Falha na tentativa de reinicializar o computador com o código de erro %1.<br /><br />Reinicialize o computador para concluir a operação de clonagem.|  
|Notas e resolução|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando o uso dos privilégios.|  
  
|||  
|-|-|  
|ID de evento|29264|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha na tentativa de limpar o sinalizador Inicialização no Modo de Restauração dos Serviços de Diretório com o código de erro %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obter mais informações sobre os erros.|  
|Notas e resolução|Examine o log dos Serviços de Diretório e o dcpromo.log para mais detalhes. Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando o uso dos privilégios.|  
  
|||  
|-|-|  
|ID de evento|29265|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Informativo|  
|Mensagem|Clonagem do controlador de domínio virtual bem-sucedida. O arquivo de configuração de clonagem do controlador de domínio virtual %1 foi renomeado para %2.|  
|Notas e resolução|N/A, este é um evento bem-sucedido.|  
  
|||  
|-|-|  
|ID de evento|29266|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Clonagem do controlador de domínio virtual bem-sucedida. Falha na tentativa de renomear o arquivo de configuração de clonagem do controlador de domínio virtual %1 com o código de erro %2 (%3).|  
|Notas e resolução|Renomeie manualmente o arquivo dccloneconfig.xml.|  
  
|||  
|-|-|  
|ID de evento|29267|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravidade|Erro|  
|Mensagem|Falha da clonagem do controlador de domínio virtual em verificar a lista de aplicativos permitidos para clonagem do controlador de domínio virtual.<br /><br />O código de erro retornado é %1 (%2).<br /><br />Esse erro pode ter sido causado por um erro de sintaxe no arquivo de lista de permissões do clone (O arquivo que está sendo verificado é: %3). Para mais informações sobre este erro, consulte %systemroot%\debug\dcpromo.log.|  
|Notas e resolução|Siga as instruções do evento|  
  
##### <a name="error-messages"></a>Mensagens de erro  
Não há erros interativos diretos para clonagem com falha do controlador de domínio virtualizado; todas as informações de clonagem são registradas nos logs de Sistema e Serviços de Diretório e a promoção do controlador de domínio é registrada no dcpromo.log. No entanto, se o servidor for inicializado em Modo de Restauração de DS, investigue imediatamente durante a falha da promoção ou clonagem.  
  
O dcpromo.log é o primeiro lugar para verificar a falha na clonagem. Dependendo da falha listada, pode ser necessário verificar posteriormente os logs do Sistema e dos Serviços de Diretório para diagnóstico posterior.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conhecidos e cenários de suporte  
A seguir, problemas comuns observados durante o processo de desenvolvimento do Windows Server 2012. Todos esses problemas são estruturais e têm uma solução alternativa válida ou uma técnica mais adequada para evitá-los em primeiro lugar. Alguns podem ser resolvidos em versões mais recentes do Windows Server 2012.  
  
|||  
|-|-|  
|**Problema**|**Falha na clonagem, DSRM**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório|  
|**Resolução e notas**|Valide todas as etapas seguidas das seções Implantando o controlador de domínio virtualizado e [Metodologia geral para solução de problemas de clonagem do controlador de domínio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Descrito na KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Concessões extras de IP ao usar o DHCP para clonar**|  
|**Sintomas**|Após a clonagem bem-sucedida de um DC e o uso de DHCP, a primeira inicialização do clone exige uma concessão de DHCP. Então, quando o servidor é renomeado e reiniciado como um DC, exige uma segunda concessão de DHCP. O primeiro endereço IP não é liberado e você acaba com uma concessão "fantasma"|  
|**Resolução e notas**|Exclua manualmente a concessão do endereço não usada no DHCP ou permita que ela expire normalmente. Descrito na KB 2742836.|  
  
|||  
|-|-|  
|**Problema**|**Falha da clonagem no DSRM após muito tempo de atraso**|  
|**Sintomas**|A clonagem parece fazer uma pausa em "A clonagem do controlador de domínio está em X% da conclusão" entre 8 e 15 minutos. Depois disso, a clonagem falha e inicia em DSRM.|  
|**Resolução e notas**|O computador clonado não pode obter um endereço IP dinâmico de DHCP ou SLAAC, está usando um endereço IP duplicado ou não pode encontrar o PDC. Várias tentativas de repetição realizadas por clonagem levam ao atraso. Resolva o problema da rede para permitir a clonagem.<br /><br />Descrito na KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**A clonagem não recria todos os nomes de entidade de serviço**|  
|**Sintomas**|Se um conjunto de SPNs (nomes da entidade de serviço) de *três partes* incluir um nome NetBIOS com uma porta e um nome NetBIOS idêntico sem uma porta, a entrada sem porta não será recriada com o novo nome do computador. Por exemplo:<br /><br />customspn/DC1:200/app1 INVALID USE OF SYMBOLS *isso é recriado com o novo nome do computador*<br /><br />customspn/DC1/app1 INVALID USE OF SYMBOLS *isso não é recriado com o novo nome do computador*<br /><br />Nomes totalmente qualificados são recriados e SPNs sem três partes são recriados, independentemente das portas. Por exemplo, estes são recriados com sucesso no clone:<br /><br />customspn/DC1:202 INVALID USE OF SYMBOLS *isso é recriado*<br /><br />customspn/DC1 INVALID USE OF SYMBOLS *isso é recriado*<br /><br />customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS *este é um nome recriado*<br /><br />customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS *isso é recriado*|  
|**Resolução e notas**|Esta é uma limitação do processo de renomeação do controlador de domínio no Windows, não apenas na clonagem. SPNS de três partes não são processados pela lógica de renomeação em nenhum cenário. A maioria dos serviços incluídos do Windows não é afetada por isso, já que recriam qualquer SPN ausente, conforme necessário. Outros aplicativos podem exigir a entrada manual do SPN para resolver o problema.<br /><br />Descrito na KB 2742874.|  
  
|||  
|-|-|  
|**Problema**|**A clonagem falha, inicia em DSRM, erros gerais de rede**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório. Há erros gerais de rede.|  
|Solução e notas|Verifique se o novo clone não tem um endereço MAC estático duplicado atribuído a partir do controlador de domínio de origem; você pode ver se uma VM usa endereços MAC estáticos executando este comando no host hipervisor para as máquinas virtuais de origem e clone:<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Altere o endereço MAC para um endereço estático exclusivo ou passe a usar endereços MAC dinâmicos.<br /><br />Descrito na KB 2742844|  
  
|||  
|-|-|  
|**Problema**|**A clonagem falha, inicia em DSRM como uma duplicata do DC de origem**|  
|**Sintomas**|Um novo clone inicia sem a clonagem. O dccloneconfig.xml não é renomeado e o servidor inicia no Modo de Restauração do DS. O log de eventos dos Serviços de Diretório mostra o erro 2164<br /><br />*<COMPUTERNAME>* Falha ao iniciar o serviço DsRoleSvc para clonar o controlador de domínio virtual local.|  
|**Resolução e notas**|Examine as configurações de serviço para o serviço Servidor de Função DS (DsRoleSvc) e verifique se o tipo de inicialização está definido como Manual. Valide que nenhum programa de terceiros está impedindo o início desse serviço.<br /><br />Para mais informações sobre como recuperar esse DC secundário enquanto assegura que as atualizações sejam replicadas externamente, consulte o artigo 2742970 da base de dados de conhecimento da Microsoft.|  
  
|||  
|-|-|  
|**Problema**|**A clonagem falha, inicia em DSRM, erro 8610**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório. Dcpromo.log mostra o erro 8610 (que é ERROR_DS_ROLE_NOT_VERIFIED 8610 ou 0x21A2)|  
|**Resolução e notas**|Acontecerá se o PDC puder ser descoberto, mas não tiver realizado replicação suficiente para permitir a si mesmo assumir a função. Por exemplo, se a clonagem for iniciada e outro administrador mover a função PDCE FSMO para um novo DC.<br /><br />Descrito na KB 2742916.|  
  
|||  
|-|-|  
|**Problema**|**A clonagem falha, inicia em DSRM, erros gerais de rede**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório. Há erros gerais de rede.|  
|**Resolução e notas**|Verifique se o novo clone não tem um endereço MAC estático duplicado atribuído a partir do controlador de domínio de origem; você pode ver se uma VM usa endereços MAC estáticos executando este comando no host Hyper-V para as máquinas virtuais de origem e clone:<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Altere o endereço MAC para um endereço estático exclusivo ou passe a usar endereços MAC dinâmicos.<br /><br />Descrito na KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**A clonagem falha, inicia em DSRM**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório|  
|**Resolução e notas**|Verifique se dccloneconfig.xml contém a definição do esquema (consulte sampledccloneconfig.xml, linha 2):<br /><br />**<d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig">**<br /><br />Descrito na KB 2742844|  
  
|||  
|-|-|  
|Problema|**Nenhum servidor de logon é o log de erros disponíveis no DSRM**|  
|**Sintomas**|O clone inicia no Modo de Restauração dos Serviços de Diretório. Você tenta fazer logon e recebe o erro:<br /><br />**Não há nenhum servidor de logon está disponíveis para atender à solicitação de logon**|  
|**Resolução e notas**|Faça logon com a conta de administrador do DSRM, não com a conta do domínio. Use a seta para a esquerda e digite um nome de usuário de:<br /><br />**.\administrator**<br /><br />Descrito na KB 2742908|  
  
|||  
|-|-|  
|**Problema**|**Falha de origem do clone no DSRM, erro**|  
|**Sintomas**|Durante a clonagem, falha 8437 "Falha ao criar objetos DC do clone no PDC" (0x20f5)|  
|**Resolução e notas**|Nome duplicado do computador foi definido em DCCloneConfig.xml como um DC de origem ou um DC existente. O nome do computador também precisa estar no formato de nome do computador NetBIOS (15 caracteres ou menos, não um FQDN).<br /><br />Repare o arquivo dccloneconfig.xml configurando um nome válido, único.<br /><br />Descrito na KB 2742959|  
  
|||  
|-|-|  
|**Problema**|**Erro New-addccloneconfigfile "o índice está fora do intervalo"**|  
|**Sintomas**|Ao executar o cmdlet new-addccloneconfigfile, você recebe um erro:<br /><br />O índice está fora do intervalo. Deve ser não negativo e menor do que o tamanho da coleção.|  
|**Resolução e notas**|É necessário executar o cmdlet em um console do Windows PowerShell com permissão de administrador. Este erro é causado pela falta de associação ao grupo de administradores locais no computador.<br /><br />Descrito na KB 2742927|  
  
|||  
|-|-|  
|**Problema**|**Falha na clonagem, DC duplicado**|  
|**Sintomas**|O clone inicia sem a clonagem, duplica o DC de origem existente|  
|**Resolução e notas**|O computador foi copiado e iniciou, mas não contém um arquivo DcCloneConfig.xml em nenhum dos locais com suporte, e não tinha um endereço IP duplicado com o controlador de domínio de origem. O DC deve ser removido corretamente para evitar a perda de dados.<br /><br />Descrito na KB 2742970|  
  
|||  
|-|-|  
|**Problema**|**New-ADDCCloneConfigFile falha com o servidor não é erro operacional quando verifica se o controlador de domínio de origem é um membro do grupo de controladores de domínio Clonáveis se um GC não está disponível.**|  
|**Sintomas**|Ao executar o New-ADDCCloneConfigFile para criar um arquivo dccloneconfig.xml, você recebe um erro:<br /><br />Código - o servidor não está funcionando|  
|**Resolução e notas**|Verifique a conectividade com um GC a partir do servidor em que você executa New-ADDCCloneConfigFile e verifique se a associação do controlador de domínio de origem no grupo de Controladores de Domínio Clonáveis foi replicada para esse GC.<br /><br />Execute o seguinte comando como um meio de limpar o cache do localizador de DC para casos em que um GC ou DC tenha sido tirado do ar recentemente:<br /><br />Código - nltest /dsgetdc: /FORCE de /GC|  
  
### <a name="advanced-troubleshooting"></a>Solução de problemas avançada  
Este módulo procura ensinar solução de problemas avançada, usando logs de *trabalho* como exemplos, com alguma explicação sobre o que ocorreu. Se você entender o que é uma operação do controlador de domínio virtualizado bem-sucedida, as falhas se tornarão evidentes em seu ambiente. Esses logs são apresentados por sua origem, com a ordem crescente de eventos *esperados* (mesmo quando são avisos e erros) relacionados a um controlador de domínio clonado dentro de cada log.  
  
#### <a name="cloning-a-domain-controller"></a>Clonando um controlador de domínio  
Neste exemplo, o controlador de domínio do clone usa DHCP para obter um endereço IP, replica SYSVOL usando FRS ou DFSR (veja o log adequado, se necessário), é um catálogo global e usa um arquivo dccloneconfig.xml em branco.  
  
##### <a name="directory-services-event-log"></a>Log de eventos dos Serviços de Diretório  
O log de Serviços de Diretório contém a maioria das informações operacionais de clonagem baseadas em eventos. O hipervisor muda a ID de Geração de VM e o serviço NTDS a anota. Em seguida, invalida o pool RID e altera a ID de invocação. A nova ID de Geração da VM está definida e o servidor replica a entrada de dados no Active Directory. O serviço DFSR é parado e seu banco de dados que hospeda SYSVOL é excluído, forçando uma entrada de sincronização não autoritativa. A marca d'água alta de USN é ajustada.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**2160**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory locais encontraram um arquivo de configuração de clonagem do controlador de domínio virtual.<br /><br />O arquivo de configuração de clonagem do controlador de domínio virtual é encontrado em:<br /><br />*<path>* \DCCloneConfig.xml<br /><br />A existência do arquivo de configuração de clonagem do controlador de domínio virtual indica que o controlador de domínio virtual local é um clone de outro controlador de domínio virtual. Os Serviços de Domínio Active Directory começarão a clonagem de si mesmos.|  
|**2191**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory definiram o seguinte valor de Registro para desabilitar atualizações de DNS.<br /><br />Chave do Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valor do Registro:<br /><br />UseDynamicDns<br /><br />Dados de Valor do Registro:<br /><br />0<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem. O processo de clonagem permitirá atualizações de DNS novamente após a conclusão da clonagem.|  
|**2191**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory definiram o seguinte valor de Registro para desabilitar atualizações de DNS.<br /><br />Chave do Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valor do Registro:<br /><br />RegistrationEnabled<br /><br />Dados de Valor do Registro:<br /><br />0<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem. O processo de clonagem permitirá atualizações de DNS novamente após a conclusão da clonagem.<br /><br />"Informações 2/7/2012 3:12:49 configuração interna do PM Microsoft-Windows-Domainservice 2191" Active Directory Domain Services defina o seguinte valor de registro para desabilitar atualizações de DNS.<br /><br />Chave do Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valor do Registro:<br /><br />DisableDynamicUpdate<br /><br />Dados de Valor do Registro:<br /><br />1<br /><br />Durante o processo de clonagem, o computador local pode ter o mesmo nome do computador que o computador de origem de clonagem por um curto período. Os registros de gravação de DNS A e AAAA são desabilitados durante esse período, portanto, os clientes não podem enviar solicitações ao computador local que está passando pela clonagem. O processo de clonagem permitirá atualizações de DNS novamente após a conclusão da clonagem.|  
|**2172**|ActiveDirectory_DomainService|Leia o atributo msDS-GenerationId do objeto de computador do Controlador de Domínio.<br /><br />Valor de atributo msDS-GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|Uma alteração na ID de Geração foi detectada.<br /><br />ID de Geração armazenada em cache no DS (valor antigo):<br /><br />*<Number>*<br /><br />ID de Geração atualmente na VM (novo valor):<br /><br />*<Number>*<br /><br />A alteração da ID de Geração ocorre após a aplicação de um instantâneo de máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. Os Serviços de Domínio Active Directory criarão uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método permitido para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory.|  
|**1109**|ActiveDirectory_DomainService|The invocationID attribute for this directory server has been changed. O número de sequência de atualização mais alto no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (antigo valor):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<Number>*<br /><br />O invocationID é alterado quando um servidor de diretório é restaurado da mídia de backup, está configurado para hospedar uma partição de diretório de aplicativos gravável, foi retomado após um instantâneo da máquina virtual ter sido aplicado, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método com suporte para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory.|  
|**1000**|ActiveDirectory_DomainService|Inicialização dos Serviços de Domínio Microsoft Active Directory concluída.|  
|**1394**|ActiveDirectory_DomainService|Todos os problemas que impedem atualizações no banco de dados Serviços de Domínio Active Directory foram excluídos. Novas atualizações para o banco de dados Serviços de Domínio Active Directory estão tendo sucesso. O serviço Logon de Rede foi reiniciado|  
|**2163**|ActiveDirectory_DomainService|O serviço DsRoleSvc foi iniciado para clonar o controlador de domínio virtual local.|  
|**326**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo de banco de dados anexou um banco de dados (1, C:\Windows\NTDS\ntds.dit). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache Salvo: 1|  
|**103**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados interrompeu a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados (6.02.8225.0000) está iniciando uma nova instância (0).|  
|**105**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Serviços de Domínio Active Directory desligados com sucesso.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados (6.02.8225.0000) está iniciando uma nova instância (0).|  
|**326**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo de banco de dados anexou um banco de dados (1, C:\Windows\NTDS\ntds.dit). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache Salvo: 1|  
|**105**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=1 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|**1109**|ActiveDirectory_DomainService|The invocationID attribute for this directory server has been changed. O número de sequência de atualização mais alto no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (antigo valor):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<Number>*<br /><br />O invocationID é alterado quando um servidor de diretório é restaurado da mídia de backup, está configurado para hospedar uma partição de diretório de aplicativos gravável, foi retomado após um instantâneo da máquina virtual ter sido aplicado, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método com suporte para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory.|  
|**1168**|ActiveDirectory_DomainService|Erro interno: Ocorreu um erro de Serviços de Domínio Active Directory.<br /><br />Dados adicionais<br /><br />Valor do erro (decimal):<br /><br />2<br /><br />Valor do erro (hexadecimal):<br /><br />2<br /><br />ID interno:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|A promoção deste controlador de domínio para um catálogo global será adiada para o intervalo seguinte.<br /><br />Intervalo (minutos):<br /><br />5<br /><br />Esse atraso é necessário para que as partições de diretório exigidas sejam preparadas antes de o catálogo global ser anunciado. No Registro, você pode especificar o número de segundos que o agente de sistema de diretório irá esperar antes de promover o controlador de domínio local para um catálogo global. Para mais informações sobre o valor de Registro do Anúncio de Atraso do Catálogo Global, consulte o Guia de Sistemas Distribuídos do Kit de Recursos|  
|**103**|NTDS ISAM|NTDS (536) NTDSA: O mecanismo do banco de dados interrompeu a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Serviços de Domínio Active Directory desligados com sucesso.|  
|**1539**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory não puderam desabilitar o cache de gravação do disco baseado em software no disco rígido seguinte.<br /><br />Disco rígido:<br /><br />c:<br /><br />Dados podem ser perdidos durante falhas do sistema|  
|**2179**|ActiveDirectory_DomainService|O atributo msDS-GenerationId do objeto de computador do Controlador de Domínio foi definido para o seguinte parâmetro:<br /><br />Atributo GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|Falha ao ler o atributo msDS-GenerationId do objeto de computador do Controlador de Domínio. Isso pode ser causado por uma falha de transação do banco de dados, ou a ID de geração não existe no banco de dados local. O msDS-GenerationId não existe durante a primeira reinicialização após dcpromo ou o DC não é um controlador de domínio virtual.<br /><br />Dados adicionais<br /><br />Código de falha:<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Inicialização dos Serviços de Domínio Microsoft Active Directory concluída, versão 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Todos os problemas que impedem atualizações no banco de dados Serviços de Domínio Active Directory foram excluídos. Novas atualizações para o banco de dados Serviços de Domínio Active Directory estão tendo sucesso. O serviço Logon de Rede foi reiniciado.|  
|**1128**|ActiveDirectory_DomainService|1128 Knowledge Consistency Checker "Uma conexão de replicação foi criada a partir do seguinte serviço de diretório de origem para o serviço de diretório local.<br /><br />Serviço de diretório de origem:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Serviço de diretório local:<br /><br />CN = NTDS Settings, *<Domain Controller DN>*<br /><br />Dados adicionais<br /><br />Código de Motivo:<br /><br />0x2<br /><br />ID Interna de Ponto de Criação:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|O serviço do diretório de origem otimizou o USN (número de sequência de atualização) apresentado pelo serviço de diretório de destino. Os serviços de diretório de origem e destino têm um parceiro de replicação comum. O serviço de diretório de destino está atualizado com o parceiro de replicação comum, e o serviço de diretório de origem foi instalado usando um backup desse parceiro.<br /><br />ID do serviço de diretório de destino:<br /><br />*<GUID> (<FQDN>)*<br /><br />ID do serviço de diretório comum:<br /><br />*<GUID>*<br /><br />USN de propriedade comum:<br /><br />*<Number>*<br /><br />Como resultado, o vetor de atualização do serviço de diretório de destino foi definido com as configurações a seguir.<br /><br />USN anterior do objeto:<br /><br />0<br /><br />USN anterior da propriedade:<br /><br />0<br /><br />GUID do banco de dados:<br /><br />*<GUID>*<br /><br />USN de objeto:<br /><br />*<Number>*<br /><br />USN de propriedade:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Log de Eventos do Sistema  
As próximas indicações de operações de clonagem estão no log de Eventos do Sistema. Assim que o hipervisor diz ao computador convidado que foi clonado ou restaurado com base em um instantâneo, o controlador de domínio imediatamente invalida seu pool RID para evitar a duplicação posterior de objetos de segurança. Conforme a clonagem prossegue, várias operações e mensagens esperadas aparecem, principalmente em torno de inicialização e parada de serviços e alguns erros esperados causados por isso. Quando concluído, o log de eventos do Sistema registra sucesso geral na clonagem.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**16654**|Directory-Services-SAM|Um pool de RIDs (identificadores de conta) foi invalidado. Isso pode ocorrer nos seguintes casos previstos:<br /><br />1. Um controlador de domínio é restaurado a partir do backup.<br /><br />2. Um controlador de domínio em execução em uma máquina virtual é restaurado a partir do instantâneo.<br /><br />3. Um administrador invalidou manualmente o pool|  
|**7036**|Gerenciador de Controle de Serviços|Os Serviços de Domínio Active Directory entraram no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço do Centro de Distribuição de Chaves Kerberos entrou no estado de execução.|  
|**3096**|Netlogon|O Controlador de Domínio primário para este domínio não pôde ser localizado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Gerenciador de Contas de Segurança entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Servidor entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Netlogon entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Serviços Web do Active Directory entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Replicação do DFS entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O Serviço de Replicação de Arquivos entrou no estado de execução.|  
|**14533**|Microsoft-Windows-DfsSvc|O DFS acaba de criar todos os namespaces.|  
|**14531**|Microsoft-Windows-DfsSvc|O servidor DFS terminou a inicialização.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Namespace do DFS entrou no estado de execução.|  
|**7023**|Gerenciador de Controle de Serviços|O serviço Mensagens entre Sites finalizou com o seguinte erro:<br /><br />O servidor especificado não pode executar a operação solicitada.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Mensagens entre Sites entrou no estado parado.|  
|**5806**|Netlogon|Atualizações dinâmicas foram desabilitadas manualmente neste controlador de domínio.<br /><br />AÇÃO DO USUÁRIO<br /><br />Reconfigure este controlador de domínio para usar as atualizações dinâmicas ou adicione manualmente os registros DNS do arquivo '%SystemRoot%\System32\Config\Netlogon.dns' para o banco de dados do DNS.|  
|**16651**|Directory-Services-SAM|Falha na solicitação de um novo pool de identificadores de conta. A operação será repetida até que a solicitação seja bem-sucedida. O erro é<br /><br />Falha na operação FSMO solicitada. O atual titular do FSMO não pôde ser contatado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Servidor DNS entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Servidor de Função de SD entrou no estado de execução.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Netlogon entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O Serviço de Replicação de Arquivos entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço do Centro de Distribuição de Chaves Kerberos entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Servidor DNS entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Serviços de Domínio Active Directory entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Netlogon entrou no estado de execução.|  
|**7040**|Gerenciador de Controle de Serviços|O tipo de inicialização dos Serviços de Domínio Active Directory foi alterado de inicialização automática para desabilitado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Netlogon entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O Serviço de Replicação de Arquivos entrou no estado de execução.|  
|**29219**|DirectoryServices-DSROLE-Server|Clonagem do controlador de domínio virtual bem-sucedida.|  
|**29223**|DirectoryServices-DSROLE-Server|Este servidor é agora um Controlador de Domínio.|  
|**29265**|DirectoryServices-DSROLE-Server|Clonagem do controlador de domínio virtual bem-sucedida. O arquivo de configuração de clonagem do controlador de domínio virtual C:\Windows\NTDS\DCCloneConfig.xml foi renomeado para C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|O processo C:\Windows\system32\lsass.exe (DC2) iniciou a reinicialização do computador DC2 em nome do usuário NT AUTHORITY\SYSTEM pelo seguinte motivo: Sistema Operacional: Reconfiguração (Planejada)<br /><br />Código de Motivo: 0x80020004<br /><br />Tipo de Desligamento: reiniciar<br /><br />Comentário: "|  
  
##### <a name="dcpromolog"></a>DCPROMO.LOG  
O Dcpromo.log contém a parte de promoção real de clonagem que o log de eventos dos Serviços de Diretório não descreve. Uma vez que o log não fornece o nível de explicação que as entradas do log de eventos comunicam, esta seção do módulo contém anotação adicional.  
  
O processo de promoção significa que a clonagem é iniciada, o DC é limpo de sua configuração atual e promovido novamente usando o banco de dados existente do AD (muito parecido com uma promoção IFM) e, em seguida, o DC replica os deltas de alteração de entrada do AD e SYSVOL e a clonagem está concluída.  
  
> [!NOTE]  
> O log foi modificado neste módulo para facilitar a leitura. A coluna de data foi removida.  
  
> [!NOTE]  
> Para mais explicações sobre o dcpromo.log, veja Noções básicas e solução de problemas da administração simplificada de AD DS no Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Iniciar promoção baseada em clone  
  
-   Defina o sinalizador do Modo de Restauração dos Serviços de Diretório de modo que o servidor não inicie o backup normalmente como o clone original e cause colisões de nomenclatura ou Serviço de Diretório  
  
-   Atualize o log de eventos dos Serviços de Diretório  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Pare o serviço NetLogon para que o controlador de domínio não anuncie  
  
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
  
-   Examine o arquivo dccloneconfig.xml quanto a personalizações especificadas pelo administrador.  
  
-   Neste caso de exemplo, é um arquivo em branco, portanto todas as configurações são geradas automaticamente e o endereçamento IP automático é exigido da rede  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Confirme se não há serviços ou programas instalados que não fazem parte do DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Habilite o DHCP nos adaptadores de rede, uma vez que as informações de IP não foram especificadas pelo administrador  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Localize o emulador PDC  
  
-   Defina o site do clone (gerado automaticamente, neste caso)  
  
-   Defina o nome do clone (gerado automaticamente neste caso)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Crie o novo objeto de computador do clone  
  
-   Renomeie o clone para corresponde com o novo nome  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Forneça as configurações da promoção, com base no dccloneconfig.xml anterior ou nas regras de geração automática  
  
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
  
-   Inicie a promoção  
  
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
  
-   Pare e configure todos os serviços relacionados ao AD DS (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> A demora do desligamento do serviço DNS é esperada neste cenário, uma vez que está usando zonas integradas ao AD que não estavam mais disponíveis, mesmo antes de o serviço NTDS ter parado - veja os eventos de DNS descritos mais adiante nesta seção do módulo.  
  
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
  
-   Force a sincronização de tempo do NT5DS (NTP) com outro controlador de domínio (geralmente, o PDCE)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contate um controlador de domínio que detém a conta do controlador de domínio de origem do clone  
  
-   Limpe todos os tíquetes existentes do Kerberos  
  
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
  
-   Pare o serviço NetLogon e defina seu tipo de início  
  
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
  
-   Configure os serviços DFSR/NTFRS para serem executados automaticamente  
  
-   Exclua os arquivos de banco de dados existentes para forçar a sincronização não autoritativa do SYSVOL quando o serviço for iniciado na próxima vez  
  
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
  
-   Inicie o processo de promoção usando o arquivo existente do banco de dados NTDS  
  
-   Contate o Mestre RID  
  
> [!NOTE]  
> O serviço AD DS não está realmente instalado aqui, esta é a instrumentação herdada no log  
  
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
  
-   Altere a ID de invocação que existia no banco de dados dos computadores de origem  
  
-   Crie um novo objeto Configurações NTDS para este clone  
  
-   Replique um delta de objeto AD do controlador de domínio parceiro  
  
> [!NOTE]  
> Mesmo que todos os objetos estejam listados como replicados, estes são os metadados necessários para incorporar as atualizações. Todos os objetos inalterados no banco de dados NTDS clonado já existem e não necessitam de nova replicação, o mesmo que usar a promoção baseada em IFM.  
  
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
  
-   Preencha as partições GC com atualizações ausentes, conforme necessário  
  
-   Complete a parte crítica de AD DS da promoção  
  
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
  
-   Complete a replicação de entrada de SYSVOL  
  
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
  
-   Habilite o registro DNS do cliente  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Execute os módulos SYSPREP especificados pelo elemento DefaultDCCloneAllowList.xml <SysprepInformation>.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   A promoção da clonagem está concluída  
  
-   Remova o sinalizador de inicialização do DSRM para que a inicialização do servidor ocorra normalmente na próxima vez  
  
-   Renomeie o dccloneconfig.xml para que ele não seja lido novamente na próxima inicialização  
  
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
  
##### <a name="active-directory-web-services-event-log"></a>Log de eventos de Serviços Web do Active Directory  
Enquanto a clonagem está ocorrendo, o banco de dados NTDS.DIT fica, muitas vezes, offline por períodos prolongados. O serviço ADWS registra pelo menos um evento para isso. Após a conclusão da clonagem, o serviço ADWS começa, observa que não há ainda um certificado de computador válido (pode ou não haver, dependendo do ambiente de implantação de um Microsoft PKI com a inscrição automática ou não) e depois inicia a instância para o novo controlador de domínio.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**1202**|Eventos de Instância do ADWS|Este computador está agora hospedando a instância do diretório especificado, mas os Serviços Web do Active Directory não podem atendê-lo. Os Serviços Web do Active Directory repetirão esta operação periodicamente.<br /><br />Instância do diretório: NTDS<br /><br />Porta LDAP de instância do diretório: 389<br /><br />Porta SSL de instância do diretório: 636|  
|**1000**|Eventos de Instância do ADWS|Os Serviços Web do Active Directory estão iniciando|  
|**1008**|Eventos de Instância do ADWS|Os Serviços Web do Active Directory reduziram com sucesso seus privilégios de segurança|  
|**1100**|Eventos de Instância do ADWS|Os valores especificados na seção <appsettings> do arquivo de configuração de Serviços Web do Active Directory foram carregados sem erros.|  
|**1400**|Eventos de Instância do ADWS|Os Serviços Web do Active Directory de Eventos de Certificado do ADWS não conseguiram encontrar um certificado de servidor com o nome do certificado especificado. É necessário um certificado para usar conexões SSL/TLS. Para usar conexões SSL/TLS, verifique se um certificado de autenticação de servidor válido de uma AC (Autoridade de Certificação) confiável está instalado no computador.<br /><br />Nome do certificado: *<Server FQDN>*|  
|**1100**|Eventos de Instância do ADWS|Os valores especificados na seção <appsettings> do arquivo de configuração de Serviços Web do Active Directory foram carregados sem erros.|  
|**1200**|Eventos de Instância do ADWS|Os Serviços Web do Active Directory estão agora atendendo à instância de diretório especificada.<br /><br />Instância do diretório: NTDS<br /><br />Porta LDAP de instância do diretório: 389<br /><br />Porta SSL de instância do diretório: 636|  
  
##### <a name="dns-server-event-log"></a>Log de eventos do Servidor DNS  
O serviço DNS irá experimentar breves interrupções esperadas durante a clonagem, enquanto o serviço DNS ainda está em execução e o banco de dados do AD DS está offline. Isso ocorre se for usado o DNS Integrado ao Active Directory, mas não ocorre se não for usado o DNS Secundário ou Primário Padrão. Esses erros são registrados várias vezes. Após a conclusão da clonagem, o DNS volta a ficar online normalmente.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**4013**|DNS-Server-Service|O servidor DNS está à espera do AD DS (Serviços de Domínio Active Directory) para sinalizar que a sincronização inicial do diretório foi concluída. O serviço do servidor DNS não pode começar até que a sincronização inicial seja concluída, pois os dados críticos do DNS ainda não podem ser replicados para este controlador de domínio. Se os eventos no log de eventos do AD DS indicarem que há um problema com a resolução de nomes DNS, considere adicionar o endereço IP de outro servidor DNS desse domínio à lista de servidores DNS nas propriedades de protocolo da Internet deste computador. Este evento será registrado a cada dois minutos até que o AD DS sinalize que a sincronização inicial foi concluída com êxito.|  
|**4015**|DNS-Server-Service|O servidor DNS encontrou um erro crítico do Active Directory. Verifique se o Active Directory está funcionando corretamente. As informações de depuração prolongada de erro (que podem estar vazias) são """". Os dados do evento contêm erro.|  
|**4000**|DNS-Server-Service|O servidor DNS não pôde abrir o Active Directory.  Esse servidor DNS está configurado para obter e usar informações do diretório para esta zona e não pode carregar a zona sem elas.  Verifique se o Active Directory está funcionando corretamente e recarregue a zona. Os dados do evento são o código do erro.|  
|**4013**|DNS-Server-Service|O servidor DNS está à espera do AD DS (Serviços de Domínio Active Directory) para sinalizar que a sincronização inicial do diretório foi concluída. O serviço do servidor DNS não pode começar até que a sincronização inicial seja concluída, pois os dados críticos do DNS ainda não podem ser replicados para este controlador de domínio. Se os eventos no log de eventos do AD DS indicarem que há um problema com a resolução de nomes DNS, considere adicionar o endereço IP de outro servidor DNS desse domínio à lista de servidores DNS nas propriedades de protocolo da Internet deste computador. Este evento será registrado a cada dois minutos até que o AD DS sinalize que a sincronização inicial foi concluída com êxito.|  
|**2**|DNS-Server-Service|O servidor DNS foi iniciado.|  
|**4**|DNS-Server-Service|O servidor DNS terminou o carregamento em segundo plano das zonas. Todas as zonas estão agora disponíveis para atualizações de DNS e transferências de zona, conforme permitido por sua configuração de zona individual.|  
  
##### <a name="file-replication-service-event-log"></a>Log de eventos do Serviço de Replicação de Arquivos  
O Serviço de Replicação de Arquivos realiza a sincronização, de forma não autoritativa, com base em um parceiro durante a clonagem. A clonagem faz isso excluindo os arquivos de banco de dados NTFRS e deixando o conteúdo de SYSVOL intocado, para uso como dados pré-propagados. As duas tentativas de sincronização são esperadas.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**13562**|NtFrs|Segue um resumo dos avisos e erros encontrados pelo Serviço de Replicação de Arquivos durante a sondagem do Controlador de Domínio DC2.root.fabrikam.com para informações de configuração do conjunto de réplicas do FRS.<br /><br />Não foi possível associar a um Controlador de Domínio. Tentaremos novamente no próximo ciclo de sondagem|  
|**13502**|NtFrs|O Serviço de Replicação de Arquivos está parando.|  
|**13565**|NtFrs|O Serviço de Replicação de Arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. O Computador DC2 não pode se tornar um controlador de domínio até que este processo seja concluído. O volume do sistema será, então, compartilhado como SYSVOL.<br /><br />Para verificar o compartilhamento de SYSVOL, no prompt de comando, digite:<br /><br />net share<br /><br />Quando o Serviço de Replicação de Arquivos conclui o processo de inicialização, o compartilhamento SYSVOL aparece.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, da disponibilidade de outros controladores de domínio e do intervalo de replicação entre os controladores de domínio.|  
|**13501**|NtFrs|O Serviço de Replicação de Arquivos está iniciando|  
|**13502**|NtFrs|O Serviço de Replicação de Arquivos está parando.|  
|**13503**|NtFrs|O Serviço de Replicação de Arquivos parou.|  
|**13565**|NtFrs|O Serviço de Replicação de Arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. O Computador DC2 não pode se tornar um controlador de domínio até que este processo seja concluído. O volume do sistema será, então, compartilhado como SYSVOL.<br /><br />Para verificar o compartilhamento de SYSVOL, no prompt de comando, digite:<br /><br />net share<br /><br />Quando o Serviço de Replicação de Arquivos conclui o processo de inicialização, o compartilhamento SYSVOL aparece.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, da disponibilidade de outros controladores de domínio e do intervalo de replicação entre os controladores de domínio.|  
|**13501**|NtFrs|O Serviço de Replicação de Arquivos está iniciando.|  
|**13553**|NtFrs|O Serviço de Replicação de Arquivos adicionou com sucesso este computador ao seguinte conjunto de réplicas:<br /><br />"VOLUME DE SISTEMA DO DOMÍNIO (COMPARTILHAMENTO SYSVOL)"<br /><br />As informações relativas a esse evento são mostradas abaixo:<br /><br />Nome DNS do computador é  *<Domain Controller FQDN>*<br /><br />Nome de membro do conjunto de réplicas é *<Domain Controller>*<br /><br />Caminho de raiz do conjunto de réplicas é *<path>*<br /><br />Caminho de diretório de preparo de réplica é *<path>*<br /><br />Caminho do diretório de trabalho de réplica é *<path>*|  
|**13520**|NtFrs|O serviço de replicação de arquivos moveu os arquivos preexistentes <path>à *<path>* \ntfrs_preexisting___see_eventlog.<br /><br />O serviço de replicação de arquivos pode excluir os arquivos no *<path>* \NtFrs_PreExisting___See_EventLog em qualquer momento. Arquivos podem ser salvos da exclusão copiando-os de *<path>* \ntfrs_preexisting___see_eventlog. A cópia dos arquivos para c:\windows\sysvol\domain pode levar a conflitos de nome se os arquivos já existirem em algum outro parceiro de replicação.<br /><br />Em alguns casos, o serviço de replicação de arquivos pode copiar um arquivo de *<path>* \NtFrs_PreExisting___See_EventLog para *<path>* em vez de replicar o arquivo de algum outro parceiro de replicação.<br /><br />Espaço pode ser recuperado a qualquer momento excluindo os arquivos no *<path>* \ntfrs_preexisting___see_eventlog. "|  
|**13508**|NtFrs|Serviço de replicação de arquivos está tendo problemas para habilitar a replicação de *\\ \\ <Domain Controller FQDN>* para *<Domain Controller>* para *<path>* usando o<br /><br />Nome DNS *\\ \\ <Domain Controller FQDN>*. O FRS continuará tentando.<br /><br />A seguir, alguns dos motivos de você estar vendo este aviso.<br /><br />[1] o FRS não pode resolver corretamente o nome DNS *\\ \\ <Domain Controller FQDN>* deste computador.<br /><br />[2] o FRS não está em execução *\\ \\ <Domain Controller FQDN>*.<br /><br />[3] As informações de topologia nos Serviços de Domínio Active Directory para esta réplica ainda não foram replicadas para todos os Controladores de Domínio.<br /><br />Esta mensagem do log de eventos será exibida uma vez por conexão. Depois que o problema for corrigido, você verá outra mensagem do log de eventos indicando que a conexão foi estabelecida.|  
|**13509**|NtFrs|O serviço de replicação de arquivos habilitou a replicação do *\\ \\ <Domain Controller FQDN>* para *<Domain Controller>* para *<Path>* após várias tentativas.|  
|**13516**|NtFrs|O serviço de replicação de arquivo não está impedindo que o computador *<Domain Controller>* se torne um controlador de domínio. O volume do sistema foi inicializado com êxito e o serviço Netlogon foi notificado de que o volume do sistema está agora pronto para ser compartilhado como SYSVOL.<br /><br />Digite "net share" para verificar o compartilhamento do SYSVOL."|  
  
##### <a name="dfs-replication-event-log"></a>Log de eventos de Replicação do DFS  
O serviços de DFSR sincronizam, de forma não autoritativa, com base em um parceiro durante a clonagem. A clonagem faz isso excluindo os arquivos do banco de dados DFSR e deixando o conteúdo do SYSVOL intocado, para uso como dados pré-propagados. As duas tentativas de sincronização são esperadas.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**1004**|DFSR|O serviço Replicação do DFS foi iniciado.|  
|**1314**|DFSR|O serviço Replicação do DFS configurou com sucesso os arquivos de log de depuração.<br /><br />Informações adicionais:<br /><br />Caminho do Arquivo de Log de Depuração: C:\Windows\debug|  
|**6102**|DFSR|O serviço Replicação do DFS foi registrado com sucesso no provedor WMI|  
|**1206**|DFSR|O serviço Replicação do DFS contatou com sucesso o controlador de domínio DC2.corp.contoso.com para acessar informações de configuração.|  
|**1210**|DFSR|O serviço Replicação do DFS configurou com sucesso um ouvinte RPC para solicitações de replicação recebidas.<br /><br />Informações adicionais:<br /><br />Porta: 0"|  
|**4614**|DFSR|O serviço Replicação do DFS iniciou o SYSVOL no caminho local C:\Windows\SYSVOL\domain e está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que tenha replicado com o seu parceiro. Se o servidor estava sendo promovido a controlador de domínio, o controlador de domínio não anunciará nem funcionará como um controlador de domínio até que esse problema seja resolvido. Isso pode ocorrer se o parceiro especificado também estiver no estado de sincronização inicial, ou se forem encontradas violações de compartilhamento neste servidor ou no parceiro de sincronização. Se esse evento ocorreu durante a migração de SYSVOL do FRS (Serviço de Replicação de Arquivos) para a Replicação do DFS, as alterações não serão replicadas até que o problema seja resolvido. Isso pode fazer com que a pasta SYSVOL no servidor fique fora de sincronia com outros controladores de domínio.<br /><br />Informações adicionais:<br /><br />Nome da Pasta Replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada: *<GUID>*<br /><br />Nome do Grupo de Replicação: Volume do Sistema de Domínio<br /><br />ID do grupo de replicação: *<GUID>*<br /><br />ID do membro: *<GUID>*<br /><br />Somente leitura: 0|  
|**4604**|DFSR|O serviço Replicação do DFS inicializou com sucesso a pasta replicada SYSVOL no caminho local C:\Windows\SYSVOL\domain. Este membro concluiu a sincronização inicial do SYSVOL com o parceiro dc1.corp.contoso.com.  Para verificar a presença do compartilhamento SYSVOL, abra uma janela de prompt de comando e digite ""net share"".<br /><br />Informações adicionais:<br /><br />Nome da Pasta Replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada: *<GUID>*<br /><br />Nome do Grupo de Replicação: Volume do Sistema de Domínio<br /><br />ID do grupo de replicação: *<GUID>*<br /><br />ID do membro: *<GUID>*<br /><br />Parceiro de sincronização: *<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Solução de problemas de restauração segura do controlador de domínio virtualizado  
  
### <a name="tools-for-troubleshooting"></a>Ferramentas para solução de problemas  
  
#### <a name="logging-options"></a>Opções de registro em log  
Os logs internos são a ferramenta mais importante para solucionar problemas de restauração de instantâneo de segurança do controlador de domínio. Todos esses logs são habilitados e configurados para o máximo de detalhamento, por padrão.  
  
|||  
|-|-|  
|**operação**|**Log**|  
|**Criação do instantâneo**|-Event viewer\Applications and services logs\Microsoft\Windows\Hyper-V-Worker|  
|**Restauração de instantâneo**|-Event viewer\Applications and services \ serviço de diretório<br />-Eventos Visualizador logs\system.<br />-Log\aplicativos do Visualizador de eventos<br />-Event viewer\Applications and services logs\File serviço de replicação<br />-Event viewer\Applications and services \ replicação do DFS<br />-Event viewer\Applications and services \ de DNS<br />-Event viewer\Applications and services logs\Microsoft\Windows\Hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e comandos para solução de problemas de configuração do controlador de domínio  
Para solucionar problemas não explicados pelos logs, use as seguintes ferramentas como um ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Monitor de Rede 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Metodologia geral para solução de problemas de restauração segura do controlador de domínio  
  
1.  A restauração segura do instantâneo é esperada, mas está tendo problemas?  
  
    1.  Examine o log de eventos dos Serviços de Diretório  
  
        1.  Existem erros de restauração do instantâneo?  
  
        2.  Existem erros de replicação de AD?  
  
    2.  Examine o log de eventos do Sistema.  
  
        1.  Existem erros de comunicação?  
  
        2.  Existem erros de AD?  
  
2.  A restauração do instantâneo de segurança é inesperada?  
  
    1.  Examine os logs de auditoria do hipervisor para determinar quem ou o que causou uma reversão  
  
    2.  Contate todos os administradores do hipervisor e interrogue-os a respeito de quem reverteu a VM sem notificação  
  
3.  O servidor está implementando a proteção de reversão do USN e não está restaurando de forma segura?  
  
    1.  Examine o log de eventos dos Serviços de Diretório quanto a um hipervisor ou serviços de integração sem suporte  
  
    2.  Examinar o sistema operacional e validar o Windows Server 2012 em execução?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Solucionando problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos os eventos de restauração de instantâneo de segurança do controlador de domínio virtualizado são gravados no log de eventos dos Serviços de Diretório da VM do controlador de domínio restaurado. Os logs de eventos do Aplicativo, Sistema, Serviço de Replicação de Arquivos e Replicação do DFS também podem conter informações úteis de solução de problemas para restaurações com falha.  
  
Abaixo estão os eventos específicos de restauração segura do Windows Server 2012 no log de eventos dos Serviços de Diretório.  
  
|||  
|-|-|  
|**ID do evento**|**2170**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Aviso|  
|**Mensagem**|Uma alteração na ID de Geração foi detectada.<br /><br />ID de Geração armazenada em cache no DS (valor antigo):%1<br /><br />ID de Geração atualmente na VM (novo valor):%2<br /><br />A alteração da ID de Geração ocorre após a aplicação de um instantâneo de máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. *<COMPUTERNAME>* criará uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método permitido para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory.|  
|**Notas e resolução**|Este é um evento de sucesso se o instantâneo era esperado. Se não, examine o log de eventos do Hyper-V-Worker ou entre em contato com o administrador do hipervisor.|  
  
|||  
|-|-|  
|**ID do evento**|**2174**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O DC não é nem um clone do controlador de domínio virtual nem um instantâneo do controlador de domínio virtual restaurado.|  
|**Notas e resolução**|Evento esperado ao iniciar os controladores de domínio físicos ou controladores de domínio virtualizados não restaurados a partir do instantâneo.|  
  
|||  
|-|-|  
|**ID do evento**|**2181**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|A transação foi anulada porque a máquina virtual está sendo revertida para um estado anterior.  Isso ocorre após a aplicação de um instantâneo de máquina virtual, depois de uma operação de importação de máquina virtual ou depois de uma operação de migração ao vivo.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. As transações acompanham a alteração da ID de Geração de VM.|  
  
|||  
|-|-|  
|**ID do evento**|**2185**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* interrompido o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:%1<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* deve inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. O evento 2187 será registrado quando o serviço FRS ou DFSR for reiniciado.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados do SYSVOL neste controlador de domínio são substituídos por cópia de um DC parceiro.|  
|**ID do evento**|2186|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao parar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:%1<br /><br />Código de erro: %2<br /><br />Mensagem de erro: %3<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* deve inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço de replicação FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. *<COMPUTERNAME>* Falha ao parar o serviço em execução atual e não pode concluir a restauração não autoritativa. Realize uma restauração não autoritativa manualmente.|  
|**Notas e resolução**|Examine os logs de eventos do Sistema, FRS e DFSR para mais informações.|  
  
|||  
|-|-|  
|**ID do evento**|**2187**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* iniciado o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:%1<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* necessário para inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso foi feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados do SYSVOL neste controlador de domínio são substituídos por cópia de um DC parceiro.|  
  
|||  
|-|-|  
|**ID do evento**|**2188**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao iniciar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:%1<br /><br />Código de erro: %2<br /><br />Mensagem de erro: %3<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* Você precisa inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço FRS ou DFSR usado para replicar o SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. *<COMPUTERNAME>* Falha ao iniciar o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e não é possível concluir a restauração não autoritativa. Realize uma restauração não autoritativa manualmente e reinicie o serviço.|  
|**Notas e resolução**|Examine os logs de eventos do Sistema, FRS e DFSR para mais informações.|  
  
|||  
|-|-|  
|**ID do evento**|**2189**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* Defina os seguintes valores de registro para inicializar a réplica SYSVOL durante uma restauração não autoritativa:<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* Você precisa inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Todos os dados do SYSVOL neste controlador de domínio são substituídos por cópia de um DC parceiro.|  
  
|||  
|-|-|  
|**ID do evento**|**2190**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao definir os seguintes valores de registro para inicializar a réplica SYSVOL durante uma restauração não autoritativa:<br /><br />Chave do Registro:%1<br /><br />Valor do Registro: %2<br /><br />Dados de Valor do Registro: %3<br /><br />Código de erro: %4<br /><br />Mensagem de erro: %5<br /><br />O Active Directory detectou que a máquina virtual que hospeda a função do controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* Você precisa inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. *<COMPUTERNAME>* Falha ao definir os valores de registro acima e não é possível concluir a restauração não autoritativa. Realize uma restauração não autoritativa manualmente.|  
|**Notas e resolução**|Examine os logs de eventos de Aplicativo e Sistema. Investigue aplicativos de terceiros que possam estar bloqueando as atualizações de Registro.|  
  
|||  
|-|-|  
|**ID do evento**|**2200**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* inicializa a replicação para atualizar o controlador de domínio. O evento 2201 será registrado quando a replicação for concluída.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Marca o início da replicação do AD de entrada.|  
  
|||  
|-|-|  
|**ID do evento**|**2201**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* concluiu a replicação para atualizar o controlador de domínio atual.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Marca o fim da replicação do AD de entrada.|  
  
|||  
|-|-|  
|**ID do evento**|**2202**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* replicação com falha para atualizar o controlador de domínio. O controlador de domínio será atualizado após a próxima replicação periódica.|  
|**Notas e resolução**|Examine os logs de eventos do Sistema e Serviços de Diretório. Use repadmin.exe para tentar forçar a replicação e anotar todas as falhas.|  
  
|||  
|-|-|  
|**ID do evento**|**2204**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* foi detectada uma alteração da ID de geração de máquina virtual. A alteração significa que o controlador de domínio virtual foi revertido para um estado anterior. *<COMPUTERNAME>* executará as seguintes operações para proteger o controlador de domínio revertido contra uma possível divergência de dados e para proteger a criação de entidades de segurança com SIDs duplicados:<br /><br />Crie uma nova ID de invocação<br /><br />Invalide o pool RID atual<br /><br />A propriedade das funções FSMO será validada na próxima replicação de entrada. Durante esta janela, se o controlador de domínio realizou uma função FSMO, essa função não estará disponível.<br /><br />Inicie a operação de restauração do serviço de replicação do SYSVOL.<br /><br />Comece a replicação para trazer o controlador de domínio revertido para o estado mais atual.<br /><br />Solicite um novo pool RID.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Isso explica as várias operações de redefinição que irão ocorrer como parte do processo de restauração segura.|  
  
|||  
|-|-|  
|**ID do evento**|**2205**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* invalidou o pool RID atual depois que o controlador de domínio virtual foi revertido ao estado anterior.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. O pool RID local deve ser destruído uma vez que o controlador de domínio foi revertido e ele pode já ter sido emitido.|  
  
|||  
|-|-|  
|**ID do evento**|**2206**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|ERRO|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao invalidar o pool RID atual depois que o controlador de domínio virtual foi revertido ao estado anterior.<br /><br />Dados adicionais:<br /><br />Código de erro: %1<br /><br />Valor do erro: %2|  
|**Notas e resolução**|Examine os logs de eventos do Sistema e Serviços de Diretório. Confirme que o Mestre RID está online e pode ser alcançado neste servidor usando Dcdiag.exe /test:ridmanager|  
  
|||  
|-|-|  
|**ID do evento**|**2207**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|ERRO|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao restaurar depois que o controlador de domínio virtual foi revertido ao estado anterior. Uma reinicialização em DSRM foi solicitada. Verifique os eventos anteriores para mais informações.|  
|**Notas e resolução**|Examine os logs de eventos do Sistema e Serviços de Diretório.|  
  
|||  
|-|-|  
|**ID do evento**|**2208**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Informativo|  
|**Mensagem**|*<COMPUTERNAME>* excluir bancos de dados DFSR para inicializar a réplica SYSVOL durante uma restauração não autoritativa.|  
|**Notas e resolução**|Esperado ao restaurar um instantâneo. Isso garante que o DFSR sincronize de forma não autoritativa o SYSVOL de um DC parceiro. Note que quaisquer outras Pastas Replicadas de DFSR no mesmo volume que o SYSVOL também sincronizarão de modo não autoritativo (controladores de domínio não são recomendados para hospedar conjuntos de DFSR personalizados no mesmo volume do SYSVOL).|  
  
|||  
|-|-|  
|**ID do evento**|**2209**|  
|**Origem**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severidade**|Erro|  
|**Mensagem**|*<COMPUTERNAME>* Falha ao excluir bancos de dados DFSR.<br /><br />Dados adicionais:<br /><br />Código de erro: %1<br /><br />Valor do erro: %2<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. *<COMPUTERNAME>* Você precisa inicializar uma restauração não autoritativa na réplica SYSVOL local. Para DFSR, isso é feito parando o serviço DFSR, excluindo bancos de dados DFSR e reiniciando o serviço. Na reinicialização, o DFSR reconstruirá os bancos de dados e começará a sincronização inicial.|  
|**Notas e resolução**|Examine o log de eventos da DFSR.|  
  
#### <a name="error-messages"></a>Mensagens de erro  
Não há erros interativos diretos para restauração de instantâneo segura do controlador de domínio virtualizado com falha; todas as informações de clonagem serão registradas nos logs de eventos dos Serviços de Diretório. Naturalmente, qualquer erro crítico de replicação ou anúncio do servidor manifesta-se como sintomas em outros lugares.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conhecidos e cenários de suporte  
O [metodologia geral para solução de problemas de domínio restauração segura do controlador](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) geralmente são adequados para solucionar a maioria dos problemas.  
  
|||  
|-|-|  
|**Problema**|**Não é possível criar novas entidades de segurança no controlador de domínio restaurado segurança recentemente.**|  
|**Sintomas**|Depois de restaurar um instantâneo, as tentativas de criar uma nova entidade de segurança (usuário, computador, grupo) no controlador de domínio falham com:<br /><br />Erro 0x2010<br /><br />O serviço de diretório não pôde alocar um identificador relativo.|  
|**Resolução e notas**|Esse problema é causado pelo conhecimento obsoleto do computador restaurado da função FSMO do Mestre RID. Se a função moveu-se para outro controlador de domínio depois de um instantâneo ter sido tirado e depois restaurado, o controlador de domínio restaurado não terá conhecimento do mestre RID até que a replicação inicial seja concluída.<br /><br />Para resolver o problema, permita que a replicação do AD conclua a entrada para o controlador de domínio restaurado. Se ainda assim não funcionar, verifique se todos os controladores de domínio têm o mesmo conhecimento correto de qual DC hospeda o Mestre RID.|  
  
|||  
|-|-|  
|**Problema**|**Controladores de domínio restaurado não compartilham SYSVOL, anúncio**|  
|**Sintomas**|Depois de restaurar um instantâneo, um ou mais DCs não anunciam, não compartilham sysvol e não têm conteúdo SYSVOL atualizado|  
|**Resolução e notas**|Parceiros upstream do DC não têm uma réplica SYSVOL funcional que esteja replicando corretamente com DFSR ou FRS. Esse problema não está relacionado à restauração segura, mas é provável que se manifeste como um problema de restauração segura, porque o cliente não tinha conhecimento do outro problema de replicação que está afetando os DCs não restaurados.|  
  
### <a name="advanced-troubleshooting"></a>Solução de problemas avançada  
Este módulo procura ensinar solução de problemas avançada, usando logs de *trabalho* como exemplos, com alguma explicação sobre o que ocorreu. Se você entender o que é uma operação do controlador de domínio virtualizado bem-sucedida, as falhas se tornarão evidentes em seu ambiente. Estes logs são apresentados por sua origem, com a ordem crescente de eventos *esperados* relacionada a um controlador de domínio clonado dentro de cada log.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restaurando um controlador de domínio que replica SYSVOL usando DFSR  
  
##### <a name="directory-services-event-log"></a>Log de eventos dos Serviços de Diretório  
O log de Serviços de Diretório contém a maioria das informações operacionais de restauração segura. O hipervisor muda a ID de Geração de VM e o serviço NTDS a anota. Em seguida, invalida o pool RID e altera a ID de invocação. A nova ID de Geração da VM está definida e os servidores replicam a entrada de dados no AD. O serviço DFSR é parado e seu banco de dados que hospeda SYSVOL é excluído, forçando uma entrada de sincronização não autoritativa. A marca d'água alta de USN é ajustada.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**2170**|ActiveDirectory_DomainService|Uma alteração na ID de Geração foi detectada.<br /><br />ID de Geração armazenada em cache no DS (valor antigo):<br /><br />*<number>*<br /><br />ID de Geração atualmente na VM (novo valor):<br /><br />*<number>*<br /><br />A alteração da ID de Geração ocorre após a aplicação de um instantâneo de máquina virtual, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. Os Serviços de Domínio Active Directory criarão uma nova ID de invocação para recuperar o controlador de domínio. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método com suporte para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory."|  
|**2181**|ActiveDirectory_DomainService|A transação foi anulada porque a máquina virtual está sendo revertida para um estado anterior.  Isso ocorre após a aplicação de um instantâneo de máquina virtual, depois de uma operação de importação de máquina virtual ou depois de uma operação de migração ao vivo.|  
|**2204**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory detectaram uma alteração na ID de geração da máquina virtual. A alteração significa que o controlador de domínio virtual foi revertido para um estado anterior. Os Serviços de Domínio Active Directory irão realizar as seguintes operações para proteger o controlador de domínio revertido contra uma possível divergência de dados e para proteger a criação de entidades de segurança com SIDs duplicados:<br /><br />Crie uma nova ID de invocação<br /><br />Invalide o pool RID atual<br /><br />A propriedade das funções FSMO será validada na próxima replicação de entrada. Durante esta janela, se o controlador de domínio realizou uma função FSMO, essa função não estará disponível.<br /><br />Inicie a operação de restauração do serviço de replicação do SYSVOL.<br /><br />Comece a replicação para trazer o controlador de domínio revertido para o estado mais atual.<br /><br />Solicite um novo pool RID."|  
|**2181**|ActiveDirectory_DomainService|A transação foi anulada porque a máquina virtual está sendo revertida para um estado anterior.  Isso ocorre após a aplicação de um instantâneo de máquina virtual, depois de uma operação de importação de máquina virtual ou depois de uma operação de migração ao vivo.|  
|**1109**|ActiveDirectory_DomainService|The invocationID attribute for this directory server has been changed. O número de sequência de atualização mais alto no momento em que o backup foi criado é o seguinte:<br /><br />Atributo InvocationID (antigo valor):<br /><br />*<GUID>*<br /><br />Atributo InvocationID (novo valor):<br /><br />*<GUID>*<br /><br />Número de sequência de atualização:<br /><br />*<number>*<br /><br />O invocationID é alterado quando um servidor de diretório é restaurado da mídia de backup, está configurado para hospedar uma partição de diretório de aplicativos gravável, foi retomado após um instantâneo da máquina virtual ter sido aplicado, após uma operação de importação de máquina virtual ou após uma operação de migração ao vivo. Controladores de domínio virtualizados não devem ser restaurados usando instantâneos de máquinas virtuais. O método com suporte para restaurar ou reverter o conteúdo de um banco de dados de Serviços de Domínio Active Directory é restaurar um backup do estado do sistema feito com um aplicativo de backup voltado aos Serviços de Domínio Active Directory."|  
|**2179**|ActiveDirectory_DomainService|O atributo msDS-GenerationId do objeto de computador do Controlador de Domínio foi definido para o seguinte parâmetro:<br /><br />Atributo GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Os Serviços de Domínio Active Directory inicializam a replicação para atualizar o controlador de domínio. O evento 2201 será registrado quando a replicação for concluída.|  
|**2201**|ActiveDirectory_DomainService|O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Os Serviços de Domínio Active Directory finalizaram a replicação para atualizar o controlador de domínio.|  
|**2185**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory pararam o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:<br /><br />DFSR<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Os Serviços de Domínio Active Directory devem inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso é feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. O evento 2187 será registrado quando o serviço FRS ou DFSR for reiniciado."|  
|**2208**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory excluíram bancos de dados DFSR para inicializar a réplica SYSVOL durante uma restauração não autoritativa.<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Serviços de domínio do Active Directory precisa inicializar uma restauração não autoritativa na réplica SYSVOL local. Para DFSR, isso é feito parando o serviço DFSR, excluindo bancos de dados DFSR e reiniciando o serviço. Após a reinicialização, o DFSR reconstrói os bancos de dados e iniciar a sincronização inicial. "|  
|**2187**|ActiveDirectory_DomainService|Os Serviços de Domínio Active Directory iniciaram o serviço FRS ou DFSR usado para replicar a pasta SYSVOL.<br /><br />Nome do serviço:<br /><br />DFSR<br /><br />O Active Directory detectou que a máquina virtual que hospeda o controlador de domínio foi revertida para um estado anterior. Os Serviços de Domínio Active Directory precisavam inicializar uma restauração não autoritativa na réplica SYSVOL local. Isso foi feito parando o serviço FRS ou DFSR usado para replicar a pasta SYSVOL e iniciando-o com as chaves e os valores de Registro apropriados para acionar a restauração. "|  
|**1587**|ActiveDirectory_DomainService|Este serviço de diretório foi restaurado ou configurado para hospedar uma partição de diretório de aplicativos. Como resultado, sua identidade de replicação foi alterada. Um parceiro solicitou alterações de replicação usando nossa antiga identidade. O número da sequência inicial foi ajustado.<br /><br />O serviço de diretório de destino correspondente à seguinte GUID do objeto solicitou alterações a partir de um USN que precede o USN em que o serviço de diretório local foi restaurado da mídia de backup.<br /><br />GUID de objeto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN no momento da restauração:<br /><br />*<number>*<br /><br />Como resultado, o vetor de atualização do serviço de diretório de destino foi definido com as configurações a seguir.<br /><br />GUID anterior do banco de dados:<br /><br />*<GUID>*<br /><br />USN anterior do objeto:<br /><br />*<number>*<br /><br />USN anterior da propriedade:<br /><br />*<number>*<br /><br />Novo GUID de banco de dados:<br /><br />*<GUID>*<br /><br />Novo USN de objeto:<br /><br />*<number>*<br /><br />Novo USN da propriedade:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Log de Eventos do Sistema  
O log de eventos do Sistema registra a hora do computador que ocorre quando um computador virtual offline é colocado online novamente e quando é feita a sincronização com a hora do host. O pool RID é invalidado e os serviços DFSR ou FRS são reiniciados.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**1**|Kernel-General|A hora do sistema foi alterado para *?<now>* partir *< data/hora do instantâneo >*.<br /><br />Motivo da alteração: um aplicativo ou componente do sistema alterou a hora.|  
|**16654**|Directory-Services-SAM|Um pool de RIDs (identificadores de conta) foi invalidado. Isso pode ocorrer nos seguintes casos previstos:<br /><br />1. Um controlador de domínio é restaurado a partir do backup.<br /><br />2. Um controlador de domínio em execução em uma máquina virtual é restaurado a partir do instantâneo.<br /><br />3. Um administrador invalidou manualmente o pool.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=226247 para mais informações.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Replicação do DFS entrou no estado parado.|  
|**7036**|Gerenciador de Controle de Serviços|O serviço Replicação do DFS entrou no estado de execução.|  
  
##### <a name="application-event-log"></a>Log de eventos do Aplicativo  
O log de eventos do Aplicativo registra a parada e a inicialização do banco de dados DFSR.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**103**|ESENT|DFSRs (1360) \\\\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: O mecanismo do banco de dados interrompeu a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|DFSRs (532) \\\\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: O mecanismo do banco de dados (6.02.8189.0000) está iniciando uma nova instância (0).|  
|**105**|ESENT|DFSRs (532) \\\\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|||DFSRs (532) \\\\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: O mecanismo de banco de dados criou um novo banco de dados (1, \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000.|  
  
##### <a name="dfs-replication-event-log"></a>Log de eventos de Replicação do DFS  
O serviço DFSR é parado e o banco de dados que contém o SYSVOL é excluído, forçando uma entrada de sincronização não autoritativa.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**1006**|DFSR|O serviço de Replicação do DFS está parando.|  
|**1008**|DFSR|O serviço de Replicação do DFS foi interrompido.|  
|**1002**|DFSR|O serviço de Replicação do DFS está iniciando.|  
|**1004**|DFSR|O serviço Replicação do DFS foi iniciado.|  
|**1314**|DFSR|O serviço Replicação do DFS configurou com sucesso os arquivos de log de depuração.<br /><br />Informações adicionais:<br /><br />Caminho do Arquivo de Log de Depuração: C:\Windows\debug|  
|**6102**|DFSR|O serviço Replicação do DFS foi registrado com sucesso no provedor WMI.|  
|**1206**|DFSR|O controlador de domínio contatado com êxito do serviço de replicação do DFS *<domain controller FQDN>* para acessar informações de configuração.|  
|**1210**|DFSR|O serviço Replicação do DFS configurou com sucesso um ouvinte RPC para solicitações de replicação recebidas.<br /><br />Informações adicionais:<br /><br />Porta: 0|  
|**4614**|DFSR|O serviço Replicação do DFS iniciou o SYSVOL no caminho local C:\Windows\SYSVOL\domain e está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que tenha replicado com o seu parceiro. Se o servidor estava sendo promovido a controlador de domínio, o controlador de domínio não anunciará nem funcionará como um controlador de domínio até que esse problema seja resolvido. Isso pode ocorrer se o parceiro especificado também estiver no estado de sincronização inicial, ou se forem encontradas violações de compartilhamento neste servidor ou no parceiro de sincronização. Se esse evento ocorreu durante a migração de SYSVOL do FRS (Serviço de Replicação de Arquivos) para a Replicação do DFS, as alterações não serão replicadas até que o problema seja resolvido. Isso pode fazer com que a pasta SYSVOL no servidor fique fora de sincronia com outros controladores de domínio.<br /><br />Informações adicionais:<br /><br />Nome da Pasta Replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada: *<GUID>*<br /><br />Nome do Grupo de Replicação: Volume do Sistema de Domínio<br /><br />ID do grupo de replicação: *<GUID>*<br /><br />ID do membro: *<GUID>*<br /><br />Somente leitura: 0|  
|**4604**|DFSR|O serviço Replicação do DFS inicializou com sucesso a pasta replicada SYSVOL no caminho local C:\Windows\SYSVOL\domain. Este membro concluiu a sincronização inicial do SYSVOL com o parceiro dc1.corp.contoso.com.  Para verificar a presença do compartilhamento SYSVOL, abra uma janela de prompt de comando e digite "net share".<br /><br />Informações adicionais:<br /><br />Nome da Pasta Replicada: Compartilhamento SYSVOL<br /><br />ID da pasta replicada: *<GUID>*<br /><br />Nome do Grupo de Replicação: Volume do Sistema de Domínio<br /><br />ID do grupo de replicação: *<GUID>*<br /><br />ID do membro: *<GUID>*<br /><br />Parceiro de sincronização: *<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restaurando um controlador de domínio que replica SYSVOL usando FRS  
O log Eventos de Replicação de Arquivos é usado em vez do log de eventos do DFSR neste caso. O log de eventos do Aplicativo também grava diferentes eventos relacionados ao FRS. Caso contrário, os serviços de diretório e o log de eventos do sistema mensagens são geralmente as mesmas e na mesma ordem conforme anteriormente descrito.  
  
##### <a name="file-replication-service-event-log"></a>Log de eventos do Serviço de Replicação de Arquivos  
O serviço FRS é parado e reiniciado com um valor D2 BURFLAGS para uma sincronização não autoritativa com o SYSVOL.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**13502**|NTFRS|O Serviço de Replicação de Arquivos está parando.|  
|**13503**|NTFRS|O Serviço de Replicação de Arquivos parou.|  
|**13501**|NTFRS|O Serviço de Replicação de Arquivos está iniciando|  
|**13512**|NTFRS|O Serviço de Replicação de Arquivos detectou um cache de gravação habilitado na unidade que contém o diretório c:\windows\ntfrs\jet no computador DC4. O Serviço de Replicação de Arquivos pode não se recuperar quando a alimentação para a unidade é interrompida e atualizações críticas são perdidas.|  
|**13565**|NTFRS|O Serviço de Replicação de Arquivos está inicializando o volume do sistema com dados de outro controlador de domínio. O Computador DC4 não pode se tornar um controlador de domínio até que este processo seja concluído. O volume do sistema será, então, compartilhado como SYSVOL.<br /><br />Para verificar o compartilhamento de SYSVOL, no prompt de comando, digite:<br /><br />net share<br /><br />Quando o Serviço de Replicação de Arquivos conclui o processo de inicialização, o compartilhamento SYSVOL aparece.<br /><br />A inicialização do volume do sistema pode levar algum tempo. O tempo depende da quantidade de dados no volume do sistema, da disponibilidade de outros controladores de domínio e do intervalo de replicação entre os controladores de domínio."|  
|**13520**|NTFRS|O serviço de replicação de arquivos moveu os arquivos preexistentes *<path>* à *<path>* \ntfrs_preexisting___see_eventlog.<br /><br />O serviço de replicação de arquivos pode excluir os arquivos no *<path>* \NtFrs_PreExisting___See_EventLog em qualquer momento. Arquivos podem ser salvos da exclusão copiando-os de *<path>* \ntfrs_preexisting___see_eventlog. Cópia dos arquivos em *<path>* pode levar a conflitos de nome se os arquivos já existirem em algum outro parceiro de replicação.<br /><br />Em alguns casos, o serviço de replicação de arquivos pode copiar um arquivo de *<path>* \NtFrs_PreExisting___See_EventLog para *<path>* em vez de replicar o arquivo de algum outro parceiro de replicação.<br /><br />Espaço pode ser recuperado a qualquer momento excluindo os arquivos no *<path>* \ntfrs_preexisting___see_eventlog.|  
|**13553**|NTFRS|O Serviço de Replicação de Arquivos adicionou com sucesso este computador ao seguinte conjunto de réplicas:<br /><br />"VOLUME DE SISTEMA DO DOMÍNIO (COMPARTILHAMENTO SYSVOL)"<br /><br />As informações relativas a esse evento são mostradas abaixo:<br /><br />Nome DNS do computador é "*<domain controller FQDN>*"<br /><br />Nome de membro do conjunto de réplicas é "*<domain controller name>*"<br /><br />Caminho de raiz do conjunto de réplicas é "*<path>*"<br /><br />Caminho do diretório de preparo da réplica é "*<path>* "<br /><br />Caminho do diretório de trabalho de réplica é "*<path>*"|  
|**13554**|NTFRS|O Serviço de Replicação de Arquivos adicionou com sucesso as conexões mostradas abaixo ao conjunto de réplicas:<br /><br />"VOLUME DE SISTEMA DO DOMÍNIO (COMPARTILHAMENTO SYSVOL)"<br /><br />Entrada de "*<partner domain controller FQDN>*"<br /><br />Saída para "*<partner domain controller FQDN>*"<br /><br />Mais informações podem aparecer em mensagens de log de eventos subsequentes.|  
|**13516**|NTFRS|O Serviço de Replicação de Arquivos não está mais impedindo que o computador DC4 transforme-se em um controlador de domínio. O volume do sistema foi inicializado com êxito e o serviço Netlogon foi notificado de que o volume do sistema está agora pronto para ser compartilhado como SYSVOL.<br /><br />Digite "net share" para verificar o compartilhamento do SYSVOL.|  
  
##### <a name="application-event-log"></a>Log de eventos do Aplicativo  
O banco de dados FRS para e começa, e é removido devido à operação D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID do evento**|**Origem**|**Mensagem**|  
|**327**|ESENT|ntfrs (1424) O mecanismo de banco de dados desanexou um banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<br /><br />Cache Restaurado: 0|  
|**103**|ESENT|ntfrs (1424) O mecanismo do banco de dados parou a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) O mecanismo do banco de dados (6.02.8189.0000) está iniciando uma nova instância (0).|  
|**105**|ESENT|ntfrs (3000) O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141.|  
|**103**|ESENT|ntfrs (3000) O mecanismo do banco de dados parou a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) O mecanismo do banco de dados (6.02.8189.0000) está iniciando uma nova instância (0).|  
|**105**|ESENT|ntfrs (3000) O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109.|  
|**325**|ESENT|ntfrs (3000) O mecanismo de banco de dados criou um novo banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000.|  
|**103**|ESENT|ntfrs (3000) O mecanismo do banco de dados parou a instância (0).<br /><br />Desligamento Anormal: 0<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) O mecanismo do banco de dados (6.02.8189.0000) está iniciando uma nova instância (0).|  
|**105**|ESENT|ntfrs (3000) O mecanismo do banco de dados iniciou uma nova instância (0). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000.|  
|**326**|ESENT|ntfrs (3000) O mecanismo de banco de dados anexou um banco de dados (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 segundo)<br /><br />Sequência de Sincronismo Interno: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache Salvo: 1|  
  


