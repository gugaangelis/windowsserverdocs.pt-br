---
title: Eventos de log do sistema do clustering de failover
description: Uma lista de eventos de clustering de failover no log do sistema do Windows Server. Esses eventos podem ser usados para solucionar problemas de um cluster.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 01/14/2020
ms.openlocfilehash: 24be2b39c8130b97d22ee182c0064986b3378549
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472973"
---
# <a name="failover-clustering-system-log-events"></a>Eventos de log do sistema do clustering de failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico lista os eventos de clustering de failover do log do sistema do Windows Server (visível no Visualizador de Eventos). Todos esses eventos compartilham a origem do evento de **FailoverClustering** e podem ser úteis ao solucionar problemas de um cluster.

## <a name="critical-events"></a>Eventos críticos

### <a name="event-1000-unexpected_fatal_error"></a>Evento 1000: UNEXPECTED_FATAL_ERROR

Serviço de cluster sofreu um erro fatal inesperado na linha %1 do módulo de origem %2. O código de erro foi %3.

### <a name="event-1001-assertion_failure"></a>Evento 1001: ASSERTION_FAILURE

Falha na Serviço de cluster uma verificação de validade na linha %1 do módulo de origem %2. ' %3 '

### <a name="event-1006-nm_event_membership_halt"></a>Evento 1006: NM_EVENT_MEMBERSHIP_HALT

Serviço de cluster foi interrompida devido à conectividade incompleta com outros nós de cluster.

### <a name="event-1057-dm_database_corrupt_or_missing"></a>Evento 1057: DM_DATABASE_CORRUPT_OR_MISSING

Não foi possível carregar o banco de dados do cluster. O arquivo pode estar ausente ou corrompido.
O reparo automático pode ser tentado.

### <a name="event-1070-nm_event_begin_join_failed"></a>Evento 1070: NM_EVENT_BEGIN_JOIN_FAILED

O nó não pôde ingressar no cluster de failover ' %1 ' devido ao código de erro ' %2 '.

### <a name="event-1073-cs_event_inconsistency_halt"></a>Evento 1073: CS_EVENT_INCONSISTENCY_HALT

O Serviço de cluster foi interrompido para evitar uma inconsistência no cluster de failover. O código de erro era ' %1 '.

### <a name="event-1080-cs_diskwrite_failure"></a>Evento 1080: CS_DISKWRITE_FAILURE

O Serviço de cluster falhou ao atualizar o banco de dados do cluster (código de erro ' %1 ').
As possíveis causas são espaço em disco insuficiente ou corrupção do sistema de arquivos.

### <a name="event-1090-cs_event_reg_operation_failed"></a>Evento 1090: CS_EVENT_REG_OPERATION_FAILED

Não é possível iniciar o Serviço de cluster. Falha ao tentar ler os dados de configuração do registro do Windows com o erro ' %1 '. Use o snap-in de gerenciamento de cluster de failover para garantir que este computador seja membro de um cluster.
Se você pretende adicionar esse computador a um cluster existente, use o assistente para adicionar nó. Como alternativa, se esse computador tiver sido configurado como membro de um cluster, será necessário restaurar os dados de configuração ausentes necessários para que o serviço de cluster identifique que ele é membro de um cluster.
Execute uma restauração do estado do sistema deste computador para restaurar os dados de configuração.

### <a name="event-1092-nm_event_mm_form_failed"></a>Evento 1092: NM_EVENT_MM_FORM_FAILED

Falha ao formar o cluster ' %1 ' com o código de erro ' %2 '. O cluster de failover não estará disponível.

### <a name="event-1093-nm_event_node_not_member"></a>Evento 1093: NM_EVENT_NODE_NOT_MEMBER

O Serviço de cluster não pode identificar o nó ' %1 ' como membro do cluster de failover ' %2 '. Se o nome do computador para esse nó foi alterado recentemente, considere reverter para o nome anterior. Como alternativa, adicione o nó ao cluster de failover e, se necessário, reinstale os aplicativos clusterizados.

### <a name="event-1105-cs_event_rpc_init_failed"></a>Evento 1105: CS_EVENT_RPC_INIT_FAILED

Falha ao iniciar a Serviço de cluster porque não foi possível registrar a (s) interface (ões) com o serviço RPC. O código de erro era ' %1 '.

### <a name="event-1135-event_node_down"></a>Evento 1135: EVENT_NODE_DOWN

O nó de cluster ' %1 ' foi removido da Associação de cluster de failover ativa. O Serviço de cluster neste nó pode ter sido interrompido. Isso também pode ser devido ao nó ter perdido a comunicação com outros nós ativos no cluster de failover.
Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados aos adaptadores de rede neste nó. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1146-rcm_event_resmon_died"></a>Evento 1146: RCM_EVENT_RESMON_DIED

O processo de Subsistema de Hospedagem de Recursos do cluster (RHS) foi encerrado e será reiniciado. Normalmente, isso é associado à detecção de integridade do cluster e à recuperação de um recurso. Consulte o log de eventos do sistema para determinar qual recurso e DLL de recurso estão causando o problema.

### <a name="event-1177-mm_event_arbitration_failed"></a>Evento 1177: MM_EVENT_ARBITRATION_FAILED

O Serviço de cluster está sendo desligado porque o quorum foi perdido. Isso pode ocorrer devido à perda de conectividade de rede entre alguns ou todos os nós no cluster ou um failover do disco testemunha.

Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1181-netres_resource_start_error"></a>Evento 1181: NETRES_RESOURCE_START_ERROR

O recurso de cluster ' %1 ' não pode ser colocado online porque o serviço associado falhou ao iniciar. O código de retorno do serviço é ' %2 '. Verifique se há eventos adicionais associados ao serviço e certifique-se de que o serviço seja iniciado corretamente.

### <a name="event-1247-cluster_event_invalid_service_sid_type"></a>Evento 1247: CLUSTER_EVENT_INVALID_SERVICE_SID_TYPE

O tipo de SID (identificador de segurança) do serviço de cluster está configurado como ' %1 ', mas o tipo de SID esperado é ' irrestrito '. O serviço de cluster está modificando automaticamente sua configuração de tipo de SID com o SCM (Gerenciador de controle de serviço) e será reiniciado para que essa alteração entre em vigor.

### <a name="event-1248-cluster_event_service_sid_missing"></a>Evento 1248: CLUSTER_EVENT_SERVICE_SID_MISSING

O identificador de segurança (SID) ' %1 ' associado ao serviço de cluster não está presente no token de processo. O serviço de cluster corrigirá automaticamente esse problema e será reiniciado.

### <a name="event-1282-sm_event_handshake_timeout"></a>Evento 1282: SM_EVENT_HANDSHAKE_TIMEOUT

O handshake de segurança entre pontos de extremidade locais e remotos ' %2 ' não foi concluído em ' %1 ' segundos, nó encerrando a conexão

### <a name="event-1542-service_prerestore_failed"></a>Evento 1542: SERVICE_PRERESTORE_FAILED

A solicitação de restauração dos dados de configuração do cluster falhou. Essa restauração falhou durante a fase de ' pré-restauração ', geralmente indicando que alguns nós que compõem o cluster não estão operacionais no momento. Verifique se o serviço de cluster está sendo executado com êxito em todos os nós que compõem este cluster.

### <a name="event-1543-service_postrestore_failed"></a>Evento 1543: SERVICE_POSTRESTORE_FAILED

Falha na operação de restauração dos dados de configuração do cluster. Essa restauração falhou durante o estágio ' pós-restauração ', geralmente indicando que alguns nós que compõem o cluster não estão operacionais no momento. É recomendável que você substitua o arquivo de dados de configuração do cluster atual (ClusDB) por ' %1 '.

### <a name="event-1546-service_form_version_incompatible"></a>Evento 1546: SERVICE_FORM_VERSION_INCOMPATIBLE

O nó ' %1 ' não pôde formar um cluster de failover. Isso ocorreu devido a um ou mais nós executando versões incompatíveis do software do serviço de cluster. Se o nó ' %1 ' ou um nó diferente no cluster tiver sido atualizado recentemente, verifique novamente se todos os nós estão executando versões compatíveis do software do serviço de cluster.

### <a name="event-1547-service_connect_version_incompatible"></a>Evento 1547: SERVICE_CONNECT_VERSION_INCOMPATIBLE

O nó ' %1 ' tentou ingressar em um cluster de failover, mas falhou devido a incompatibilidade entre as versões do software do serviço de cluster. Se o nó ' %1 ' ou um nó diferente no cluster tiver sido atualizado recentemente, verifique se há suporte para a implantação de cluster alterada com versões diferentes do software do serviço de cluster.

### <a name="event-1553-service_no_network_connectivity"></a>Evento 1553: SERVICE_NO_NETWORK_CONNECTIVITY

Este nó de cluster não tem conectividade de rede. Ele não pode participar do cluster até que a conectividade seja restaurada. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1554-service_network_connectivity_lost"></a>Evento 1554: SERVICE_NETWORK_CONNECTIVITY_LOST

Este nó de cluster perdeu toda a conectividade de rede. Ele não pode participar do cluster até que a conectividade seja restaurada. O serviço de cluster neste nó será encerrado.

### <a name="event-1556-service_unhandled_exception_in_worker_thread"></a>Evento 1556: SERVICE_UNHANDLED_EXCEPTION_IN_WORKER_THREAD

O serviço de cluster encontrou um problema inesperado e será desligado. O código de erro era ' %2 '.

### <a name="event-1561-service_nonstorage_witness_better_tag"></a>Evento 1561: SERVICE_NONSTORAGE_WITNESS_BETTER_TAG

Serviço de cluster falhou ao iniciar porque este nó detectou que ele não tem a cópia mais recente dos dados de configuração do cluster. As alterações no cluster ocorreram enquanto este nó não estava em associação e, como resultado, não foi possível receber atualizações de dados de configuração.

#### <a name="guidance"></a>Diretrizes

Tente iniciar o serviço de cluster em todos os nós no cluster para que os nós com a cópia mais recente dos dados de configuração do cluster possam primeiro formar o cluster. Esse nó então será capaz de ingressar no cluster e obterá automaticamente os dados de configuração do cluster atualizados. Se não houver nós disponíveis com a cópia mais recente dos dados de configuração do cluster, execute o cmdlet ' Start-ClusterNode-FQ ' do Windows PowerShell. O uso do parâmetro ForceQuorum (FQ) iniciará o serviço de cluster e marcará a cópia dos dados de configuração do cluster para ser autoritativa. Forçar o quorum em um nó com uma cópia desatualizada do banco de dados do cluster pode resultar em alterações de configuração do cluster que ocorreram enquanto o nó não estava participando do cluster para ser perdido.

### <a name="event-1564-res_fsw_arbitratefailure"></a>Evento 1564: RES_FSW_ARBITRATEFAILURE

O recurso de testemunha de compartilhamento de arquivos ' %1 ' não pôde arbitrar para o compartilhamento de arquivos ' %2 '.
Verifique se o compartilhamento de arquivos ' %2 ' existe e se está acessível pelo cluster.

### <a name="event-1570-service_connect_authentication_failure"></a>Evento 1570: SERVICE_CONNECT_AUTHENTICATION_FAILURE

O nó ' %1 ' não estabeleceu uma sessão de comunicação ao ingressar no cluster.
Isso ocorreu devido a uma falha de autenticação. Verifique se os nós estão executando versões compatíveis do software do serviço de cluster.

### <a name="event-1571-service_connect_authorizaion_failure"></a>Evento 1571: SERVICE_CONNECT_AUTHORIZAION_FAILURE

O nó ' %1 ' não estabeleceu uma sessão de comunicação ao ingressar no cluster.
Isso ocorreu devido a uma falha de autorização. Verifique se os nós estão executando versões compatíveis do software do serviço de cluster.

### <a name="event-1572-service_netft_route_initial_failure"></a>Evento 1572: SERVICE_NETFT_ROUTE_INITIAL_FAILURE

O nó ' %1 ' não pôde ingressar no cluster porque ele não pôde enviar e receber mensagens de rede de detecção de falha com outros nós de cluster. Execute o assistente para validar uma configuração para garantir as configurações de rede. Verifique também as regras de "clusters de failover" do firewall do Windows.

### <a name="event-1574-dm_database_unload_failed"></a>Evento 1574: DM_DATABASE_UNLOAD_FAILED

Não foi possível descarregar o banco de dados do cluster de failover. Se a reinicialização do serviço de cluster não corrigir o problema, reinicie o computador.

### <a name="event-1575-dm_database_corrupt_or_missing_fixquorum"></a>Evento 1575: DM_DATABASE_CORRUPT_OR_MISSING_FIXQUORUM

Falha na tentativa de iniciar o serviço de cluster de forma forçada porque os dados de configuração do cluster neste nó estão ausentes ou corrompidos. Primeiro, inicie o serviço de cluster em outro nó que tenha uma cópia intacta e válida dos dados de configuração do cluster. Em seguida, tente novamente a operação de inicialização neste nó (que tentará obter informações de configuração válidas atualizadas automaticamente). Se nenhum outro nó estiver disponível, use WBAdmin. msc para executar uma restauração do estado do sistema desse nó a fim de restaurar os dados de configuração.

### <a name="event-1593-dm_could_not_discard_changes"></a>Evento 1593: DM_COULD_NOT_DISCARD_CHANGES

Não foi possível descarregar o banco de dados do cluster de failover e não foi possível descartar nenhuma alteração potencialmente incorreta na memória. O serviço de cluster tentará reparar o banco de dados recuperando-o de outro nó de cluster. Se o serviço de cluster não ficar online, reinicie o serviço de cluster nesse nó. Se a reinicialização do serviço de cluster não corrigir o problema, reinicie o computador. Se o serviço de cluster não ficar online após uma reinicialização, restaure o banco de dados do cluster do último backup. O banco de dados atual foi copiado para ' %1 '. Se não houver backups disponíveis, copie ' %1 ' para ' %2 ' e tente iniciar o serviço de cluster. Se o serviço de cluster ficar online neste nó, algumas alterações de configuração de cluster poderão ser perdidas e o cluster poderá não funcionar corretamente. Execute o assistente para validar uma configuração para verificar a configuração do cluster e verificar se os serviços hospedados e os aplicativos estão online e funcionando corretamente.

### <a name="event-1672-event_node_quarantined"></a>Evento 1672: EVENT_NODE_QUARANTINED

O nó de cluster ' %1 ' foi colocado em quarentena. O nó enfrentou ' %2 ' falhas consecutivas em um curto período de tempo e foi removido do cluster para evitar mais interrupções. O nó será colocado em quarentena até ' %3 ' e, em seguida, o nó tentará novamente ingressar no cluster automaticamente.

Consulte os logs de eventos do sistema e do aplicativo para determinar os problemas neste nó. Quando o problema for resolvido, a quarentena poderá ser limpa manualmente para permitir que o nó ingresse novamente com o cmdlet ' Start-ClusterNode – ClearQuarantine ' do Windows PowerShell.

Nome do nó: %1

Número de associações de cluster consecutivas perdida: %2

A quarentena de tempo será automaticamente desmarcada: %3

## <a name="error-events"></a>Eventos de erro

### <a name="event-1024-cp_reg_ckpt_restore_failed"></a>Evento 1024: CP_REG_CKPT_RESTORE_FAILED

Não foi possível restaurar o ponto de verificação do registro para o recurso de cluster ' %1 ' na chave do registro HKEY_LOCAL_MACHINE \\ %2. O recurso pode não funcionar corretamente.
Certifique-se de que nenhum outro processo tenha identificadores abertos para chaves do registro nesta subárvore do registro.

### <a name="event-1034-res_disk_missing"></a>Evento 1034: RES_DISK_MISSING

O recurso de disco físico do cluster ' %1 ' não pode ser colocado online porque o disco associado não foi encontrado. A assinatura esperada do disco era ' %2 '.
Se o disco foi substituído ou restaurado, no snap-in Gerenciador de Cluster de Failover, você pode usar a função reparar (na folha de propriedades do disco) para reparar o disco novo ou restaurado. Se o disco não for substituído, exclua o recurso de disco associado.

### <a name="event-1035-res_disk_mount_failed"></a>Evento 1035: RES_DISK_MOUNT_FAILED

Enquanto o recurso de disco ' %1 ' estava sendo colocado online, o acesso a um ou mais volumes falhou com o erro ' %2 '. Execute o assistente para validar uma configuração para verificar sua configuração de armazenamento. Opcionalmente, talvez você queira executar chkdsk para verificar a integridade de todos os volumes neste disco.

### <a name="event-1037-res_disk_filesystem_failed"></a>Evento 1037: RES_DISK_FILESYSTEM_FAILED

O sistema de arquivos para uma ou mais partições no disco para o recurso ' %1 ' pode estar corrompido. Execute o assistente para validar uma configuração para verificar sua configuração de armazenamento. Opcionalmente, talvez você queira executar chkdsk para verificar a integridade de todos os volumes neste disco.

### <a name="event-1038-res_disk_reservation_lost"></a>Evento 1038: RES_DISK_RESERVATION_LOST

A propriedade do disco de cluster ' %1 ' foi perdida inesperadamente por este nó. Execute o assistente para validar uma configuração para verificar sua configuração de armazenamento.

### <a name="event-1039-res_genapp_create_failed"></a>Evento 1039: RES_GENAPP_CREATE_FAILED

O aplicativo genérico ' %1 ' não pôde ser colocado online (com o erro ' %2 ') durante uma tentativa de criar o processo. As possíveis causas incluem: o aplicativo pode não estar presente neste nó, o nome do caminho pode ter sido especificado incorretamente, o nome binário pode ter sido especificado incorretamente.

### <a name="event-1040-res_gensvc_open_failed"></a>Evento 1040: RES_GENSVC_OPEN_FAILED

O serviço genérico ' %1 ' não pôde ser colocado online (com o erro ' %2 ') durante uma tentativa de abrir o serviço. As possíveis causas incluem: o serviço não está instalado ou o nome do serviço especificado é inválido.

### <a name="event-1041-res_gensvc_start_failed"></a>Evento 1041: RES_GENSVC_START_FAILED

O serviço genérico ' %1 ' não pôde ser colocado online (com o erro ' %2 ') durante uma tentativa de iniciar o serviço. Causa possível: os parâmetros de serviço especificados podem ser inválidos.

### <a name="event-1042-res_gensvc_failed_after_start"></a>Evento 1042: RES_GENSVC_FAILED_AFTER_START

O serviço genérico ' %1 ' falhou com o erro ' %2 '. Examine o log de eventos do aplicativo.

### <a name="event-1044-res_ipaddr_nbt_interface_create_failed"></a>Evento 1044: RES_IPADDR_NBT_INTERFACE_CREATE_FAILED

Ocorreu uma falha ao tentar criar uma nova interface NetBIOS ao colocar o recurso ' %1 ' online (código de erro ' %2 '). O número máximo de nomes NetBIOS pode ter sido excedido.

### <a name="event-1046-res_ipaddr_invalid_subnet"></a>Evento 1046: RES_IPADDR_INVALID_SUBNET

O recurso de endereço IP do cluster ' %1 ' não pode ser colocado online porque o valor da máscara de sub-rede é inválido. Verifique suas propriedades de recurso de endereço IP.

### <a name="event-1047-res_ipaddr_invalid_address"></a>Evento 1047: RES_IPADDR_INVALID_ADDRESS

O recurso de endereço IP do cluster ' %1 ' não pode ser colocado online porque o valor do endereço é inválido. Verifique suas propriedades de recurso de endereço IP.

### <a name="event-1048-res_ipaddr_invalid_adapter"></a>Evento 1048: RES_IPADDR_INVALID_ADAPTER

O recurso de endereço IP do cluster ' %1 ' não pôde ser colocado online. Não foi possível determinar os dados de configuração do adaptador de rede correspondente à interface de rede de cluster ' %2 ' (código de erro: ' %3 '). Verifique se o recurso de endereço IP está configurado com as propriedades de endereço e rede corretas.

### <a name="event-1049-res_ipaddr_in_use"></a>Evento 1049: RES_IPADDR_IN_USE

O recurso de endereço IP do cluster ' %1 ' não pode ser colocado online porque um endereço IP duplicado ' %2 ' foi detectado na rede. Verifique se todos os endereços IP são exclusivos.

### <a name="event-1050-res_netname_duplicate"></a>Evento 1050: RES_NETNAME_DUPLICATE

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online porque o nome ' %2 ' corresponde a este nome de nó de cluster. Verifique se os nomes de rede são exclusivos.

### <a name="event-1051-res_netname_no_ip_address"></a>Evento 1051: RES_NETNAME_NO_IP_ADDRESS

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online. Verifique se os adaptadores de rede para os recursos de endereço IP dependentes têm acesso a pelo menos um servidor DNS. Como alternativa, habilite o NetBIOS para endereços IP dependentes.

### <a name="event-1052-res_netname_cant_add_name_status"></a>Evento 1052: RES_NETNAME_CANT_ADD_NAME_STATUS

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online porque o nome não pôde ser adicionado ao sistema. O código de erro associado é armazenado na seção de dados.

### <a name="event-1053-res_smb_cant_create_share"></a>Evento 1053: RES_SMB_CANT_CREATE_SHARE

O compartilhamento de arquivos de cluster ' %1 ' não pode ser colocado online porque o compartilhamento não pôde ser criado.

### <a name="event-1054-res_smb_share_not_found"></a>Evento 1054: RES_SMB_SHARE_NOT_FOUND

Falha na verificação de integridade do recurso de compartilhamento de arquivos ' %1 '. A recuperação de informações para o compartilhamento ' %2 ' (com escopo para o nome de rede %3) retornou o código de erro ' %4 '. Verifique se o compartilhamento existe e está acessível.

### <a name="event-1055-res_smb_share_failed"></a>Evento 1055: RES_SMB_SHARE_FAILED

Falha na verificação de integridade do recurso de compartilhamento de arquivos ' %1 '. A recuperação de informações para o compartilhamento ' %2 ' (com escopo para o nome de rede %3) indicou que o compartilhamento não existe (código de erro ' %4 '). Verifique se o compartilhamento existe e está acessível.

### <a name="event-1069-rcm_resource_failure"></a>Evento 1069: RCM_RESOURCE_FAILURE

O recurso de cluster ' %1 ' na função clusterizada ' %2 ' falhou.

### <a name="event-1069-rcm_resource_failure_with_typename"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_TYPENAME

Falha no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 '.<br><br>Com base nas políticas de falha do recurso e da função, o serviço de cluster pode tentar colocar o recurso online nesse nó ou mover o grupo para outro nó do cluster e reiniciá-lo. Verifique o recurso e o estado do grupo usando Gerenciador de Cluster de Failover ou o cmdlet Get-ClusterResource do Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_with_cause"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_CAUSE

Falha no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 '. O código de erro era ' %5 ' (' %4 ').<br><br>Com base nas políticas de falha do recurso e da função, o serviço de cluster pode tentar colocar o recurso online nesse nó ou mover o grupo para outro nó do cluster e reiniciá-lo. Verifique o recurso e o estado do grupo usando Gerenciador de Cluster de Failover ou o cmdlet Get-ClusterResource do Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_with_error_code"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_ERROR_CODE

Falha no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 '. O código de erro era ' %4 '.<br><br>Com base nas políticas de falha do recurso e da função, o serviço de cluster pode tentar colocar o recurso online nesse nó ou mover o grupo para outro nó do cluster e reiniciá-lo. Verifique o recurso e o estado do grupo usando Gerenciador de Cluster de Failover ou o cmdlet Get-ClusterResource do Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_due_to_veto"></a>Evento 1069: RCM_RESOURCE_FAILURE_DUE_TO_VETO

O recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 ' falhou devido a uma tentativa de bloquear uma alteração de estado necessária nesse recurso de cluster.

### <a name="event-1077-res_ipaddr_ipv4_address_interface_failed"></a>Evento 1077: RES_IPADDR_IPV4_ADDRESS_INTERFACE_FAILED

A verificação de integridade para a interface IP ' %1 ' (endereço ' %2 ') falhou (o status é ' %3 '). Execute o assistente para validar uma configuração para garantir que o adaptador de rede esteja funcionando corretamente.

### <a name="event-1078-res_ipaddr_wins_address_failed"></a>Evento 1078: RES_IPADDR_WINS_ADDRESS_FAILED

O recurso de endereço IP do cluster ' %1 ' não pode ser colocado online porque o registro do WINS falhou na interface ' %2 ' com o erro ' %3 '. Verifique se um servidor WINS válido e acessível foi especificado.

### <a name="event-1121-cp_crypto_ckpt_restore_failed"></a>Evento 1121: CP_CRYPTO_CKPT_RESTORE_FAILED

As configurações criptografadas para o recurso de cluster ' %1 ' não puderam ser aplicadas com êxito ao nome de contêiner ' %2 ' neste nó.

### <a name="event-1127-tm_event_cluster_netinterface_failed"></a>Evento 1127: TM_EVENT_CLUSTER_NETINTERFACE_FAILED

Falha na interface de rede de cluster ' %1 ' para o nó de cluster ' %2 ' na rede ' %3 '. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1129-tm_event_cluster_network_partitioned"></a>Evento 1129: TM_EVENT_CLUSTER_NETWORK_PARTITIONED

A rede de clusters ' %1 ' está particionada. Alguns nós de cluster de failover anexados não podem se comunicar entre si pela rede. O cluster de failover não pôde determinar o local da falha. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1130-tm_event_cluster_network_down"></a>Evento 1130: TM_EVENT_CLUSTER_NETWORK_DOWN

A rede de clusters ' %1 ' está inoperante. Nenhum dos nós disponíveis pode se comunicar usando essa rede. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1137-rcm_drain_move_failed"></a>Evento 1137: RCM_DRAIN_MOVE_FAILED

Não foi possível concluir a movimentação da função de cluster ' %1 ' para drenagem. A operação falhou com o código de erro %2.

### <a name="event-1138-res_smb_cant_create_dfs_root"></a>Evento 1138: RES_SMB_CANT_CREATE_DFS_ROOT

O recurso de compartilhamento de arquivos de cluster ' %1 ' não pode ser colocado online. Falha na criação da raiz de namespace do DFS com o erro ' %3 '. Isso pode ser devido à falha na inicialização do serviço ' namespace do DFS ' ou à falha ao criar a raiz DFS-N para o compartilhamento ' %2 '.

### <a name="event-1141-res_smb_cant_init_dfs_svc"></a>Evento 1141: RES_SMB_CANT_INIT_DFS_SVC

O recurso de compartilhamento de arquivos de cluster ' %1 ' não pode ser colocado online. Falha na ressincronização do destino raiz do DFS ' %2 ' com o erro ' %3 '.

### <a name="event-1142-res_smb_cant_online_dfs_root"></a>Evento 1142: RES_SMB_CANT_ONLINE_DFS_ROOT

O recurso de compartilhamento de arquivos do cluster ' %1 ' para o namespace do DFS não pode ser colocado online devido ao erro ' %2 '.

### <a name="event-1182-netres_resource_stop_error"></a>Evento 1182: NETRES_RESOURCE_STOP_ERROR

O recurso de cluster ' %1 ' não pode ser colocado online porque o serviço associado falhou ao tentar reiniciar. O código de erro é ' %2 '. O serviço exigiu uma reinicialização para atualizar os parâmetros. No entanto, o serviço falhou antes de ser interrompido e reiniciado. Verifique se há eventos adicionais associados ao serviço e certifique-se de que o serviço esteja funcionando corretamente.

### <a name="event-1183-res_disk_invalid_mp_source_not_clustered"></a>Evento 1183: RES_DISK_INVALID_MP_SOURCE_NOT_CLUSTERED

O recurso de disco de cluster ' %1 ' contém um ponto de montagem inválido. Os discos de origem e de destino associados ao ponto de montagem devem ser discos clusterizados e devem ser membros do mesmo grupo. <br>O ponto de montagem ' %2 ' do volume ' %3 ' faz referência a um disco de origem inválido. Verifique se o disco de origem também é um disco clusterizado e no mesmo grupo que o disco de destino (hospedando o ponto de montagem).

### <a name="event-1191-res_netname_delete_computer_account_failed_status"></a>Evento 1191: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED_STATUS

O recurso de nome de rede de cluster ' %1 ' não excluiu seu objeto de computador associado no domínio ' %2 '. O código de erro era ' %3 '. <br>Faça com que um administrador de domínio exclua manualmente o objeto de computador do domínio Active Directory.

### <a name="event-1192-res_netname_delete_computer_account_failed"></a>Evento 1192: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED

O recurso de nome de rede de cluster ' %1 ' não excluiu seu objeto de computador associado no domínio ' %2 ' pelo seguinte motivo:<br>%3. <br><br>Faça com que um administrador de domínio exclua manualmente o objeto de computador do domínio Active Directory.

### <a name="event-1193-res_netname_add_computer_account_failed_status"></a>Evento 1193: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED_STATUS

O recurso de nome de rede de cluster ' %1 ' não pôde criar seu objeto de computador associado no domínio ' %2 ' pelo seguinte motivo: %3.<br><br>O código de erro associado é: %5<br><br>
Trabalhe com o administrador de domínio para garantir que:

- A identidade do cluster ' %4 ' pode criar objetos de computador. Por padrão, todos os objetos de computador são criados no contêiner ' computadores '; consulte o administrador de domínio se esse local tiver sido alterado.
- A cota de objetos de computador não foi atingida.
- Se houver um objeto Computer existente, verifique se a identidade do cluster ' %4 ' tem a permissão ' controle total ' para esse objeto de computador usando a ferramenta Active Directory usuários e computadores.

### <a name="event-1194-res_netname_add_computer_account_failed"></a>Evento 1194: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED

O recurso de nome de rede de cluster ' %1 ' não pôde criar seu objeto de computador associado no domínio ' %2 ' durante: %3.<br><br>O texto do código de erro associado é: %4

Trabalhe com o administrador de domínio para garantir que:

- A identidade do cluster ' %5 ' tem permissões para criar objetos de computador. Por padrão, todos os objetos de computador são criados no mesmo contêiner que a identidade de cluster ' %5 '.
- A cota de objetos de computador não foi atingida.
- Se houver um objeto Computer existente, verifique se a identidade do cluster ' %5 ' tem a permissão ' controle total ' para esse objeto de computador usando a ferramenta Active Directory usuários e computadores.

### <a name="event-1195-res_netname_dns_registration_failed_status"></a>Evento 1195: RES_NETNAME_DNS_REGISTRATION_FAILED_STATUS

O recurso de nome de rede de cluster ' %1 ' falhou no registro de um ou mais nomes DNS associados. O código de erro era ' %2 '. Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com acesso a pelo menos um servidor DNS.

### <a name="event-1196-res_netname_dns_registration_failed"></a>Evento 1196: RES_NETNAME_DNS_REGISTRATION_FAILED

O recurso de nome de rede de cluster ' %1 ' falhou no registro de um ou mais nomes DNS associados pelo seguinte motivo:<br>%2.<br><br>Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com pelo menos um servidor DNS acessível.

### <a name="event-1205-rcm_event_group_failed_online_offline"></a>Evento 1205: RCM_EVENT_GROUP_FAILED_ONLINE_OFFLINE

O Serviço de cluster falhou ao colocar a função clusterizada ' %1 ' completamente online ou offline. Um ou mais recursos podem estar em um estado de falha. Isso pode afetar a disponibilidade da função clusterizada.

### <a name="event-1206-res_netname_update_computer_account_failed_status"></a>Evento 1206: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED_STATUS

O objeto de computador associado ao recurso de nome de rede de cluster ' %1 ' não pôde ser atualizado no domínio ' %2 '. O código de erro era ' %3 '. A identidade do cluster ' %4 ' pode não ter permissões necessárias para atualizar o objeto. Trabalhe com o administrador de domínio para garantir que a identidade do cluster possa atualizar objetos de computador no domínio.

### <a name="event-1207-res_netname_update_computer_account_failed"></a>Evento 1207: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED

O objeto de computador associado ao recurso de nome de rede de cluster ' %1 ' não pôde ser atualizado no domínio ' %2 ' durante o <br>operação de %3.<br><br>O texto do código de erro associado é: %4<br> <br>A identidade do cluster ' %5 ' pode não ter permissões necessárias para atualizar o objeto. Trabalhe com o administrador de domínio para garantir que a identidade do cluster possa atualizar objetos de computador no domínio.

### <a name="event-1208-res_disk_invalid_mp_target_not_clustered"></a>Evento 1208: RES_DISK_INVALID_MP_TARGET_NOT_CLUSTERED

O recurso de disco de cluster ' %1 ' contém um ponto de montagem inválido. Os discos de origem e de destino associados ao ponto de montagem devem ser discos clusterizados e devem ser membros do mesmo grupo. <br>O ponto de montagem ' %2 ' do volume ' %3 ' faz referência a um disco de destino inválido. Verifique se o disco de destino também é um disco clusterizado e no mesmo grupo que o disco de origem (hospedando o ponto de montagem).

### <a name="event-1211-res_netname_no_writeable_dc_status"></a>Evento 1211: RES_NETNAME_NO_WRITEABLE_DC_STATUS

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online. Falha na tentativa de localizar um controlador de domínio gravável (no domínio %2) para criar ou atualizar um objeto de computador associado ao recurso. O código de erro era ' %3 '.
Verifique se um controlador de domínio gravável está acessível para este nó dentro do domínio configurado. Verifique também se o servidor DNS está em execução para resolver o nome do controlador de domínio.

### <a name="event-1212-res_netname_no_writeable_dc"></a>Evento 1212: RES_NETNAME_NO_WRITEABLE_DC

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online. A tentativa de localizar um controlador de domínio gravável (no domínio %2) para criar ou atualizar um objeto de computador associado ao recurso falhou pelo seguinte motivo:<br>%3.<br><br> O código de erro era ' %4 '. Verifique se um controlador de domínio gravável está acessível para este nó dentro do domínio configurado. Verifique também se o servidor DNS está em execução para resolver o nome do controlador de domínio.

### <a name="event-1213-res_netname_rename_restore_failed"></a>Evento 1213: RES_NETNAME_RENAME_RESTORE_FAILED

O recurso de nome de rede de cluster ' %1 ' não pôde renomear completamente o objeto de computador associado no controlador de domínio ' %2 '. A tentativa de renomear o objeto de computador com base no novo nome ' %3 ' de volta para seu nome original ' %4 ' também falhou. O código de erro era ' %5 '. Isso pode afetar a conectividade do cliente até que o nome da rede e seu nome de objeto de computador associado sejam consistentes. Contate o administrador de domínio para renomear manualmente o objeto de computador.

### <a name="event-1214-res_netname_cant_add_name2"></a>Evento 1214: RES_NETNAME_CANT_ADD_NAME2

O recurso de nome de rede do cluster não pode ser registrado com NetBIOS. <br><br>Nome da rede: ' %3 ' <br>Motivo da falha: %2 <br>Nome do recurso: ' %1 ' <br><br> Execute o nbtstat para o nome de rede para garantir que o nome ainda não esteja registrado com NetBIOS.

### <a name="event-1215-res_netname_not_registered_with_rdr"></a>Evento 1215: RES_NETNAME_NOT_REGISTERED_WITH_RDR

O recurso de nome de rede de cluster ' %1 ' falhou em uma verificação de integridade. O nome de rede ' %2 ' não está mais registrado neste nó. O código de erro era ' %3 '. Verifique se há erros de hardware ou software relacionados ao adaptador de rede. Além disso, você pode executar o assistente para validar uma configuração para verificar sua configuração de rede.

### <a name="event-1218-res_netname_online_rename_recovery_missing_account"></a>Evento 1218: RES_NETNAME_ONLINE_RENAME_RECOVERY_MISSING_ACCOUNT

O recurso de nome de rede de cluster ' %1 ' não pôde executar uma operação de alteração de nome (tentando alterar o nome original ' %3 ' para o nome ' %4 '). Não foi possível encontrar o objeto de computador no controlador de domínio ' %2 ' (onde ele foi criado). Será feita uma tentativa de recriar o objeto de computador na próxima vez em que o recurso for colocado online. Além disso, trabalhe com o administrador de domínio para garantir que o objeto de computador exista no domínio.

### <a name="event-1219-res_netname_online_rename_dc_not_found"></a>Evento 1219: RES_NETNAME_ONLINE_RENAME_DC_NOT_FOUND

O recurso de nome de rede de cluster ' %1 ' não pôde executar uma operação de alteração de nome.
O controlador de domínio ' %2 ', em que o objeto de computador ' %3 ' estava sendo renomeado, não pôde ser contatado. O código de erro era ' %4 '. Verifique se um controlador de domínio gravável está acessível e verifique se há algum problema de conectividade.

### <a name="event-1220-res_netname_online_rename_recovery_failed"></a>Evento 1220: RES_NETNAME_ONLINE_RENAME_RECOVERY_FAILED

A conta de computador do recurso ' %1 ' não pôde ser renomeada de %2 para %3 usando o controlador de domínio %4. O código de erro associado é armazenado na seção de dados.<br>
<br>A conta de computador deste recurso estava sendo renomeada e não foi concluída. Isso foi detectado durante o processo online deste recurso.
Para recuperar, a conta de computador deve ser renomeada para o valor atual da propriedade Name, ou seja, o nome apresentado na rede.<br> <br>O controlador de domínio em que a tentativa renomeada pode não estar disponível; Se esse for o caso, aguarde o controlador de domínio estar disponível novamente. O controlador de domínio pode negar acesso à conta; Depois de resolver o acesso, tente colocar o nome novamente online.<br> <br>Se isso não for possível, desabilite e habilite novamente a autenticação Kerberos e uma tentativa será feita para localizar a conta de computador em um controlador de domínio diferente. Você precisa alterar a propriedade Name para %2 a fim de usar a conta de computador existente.

### <a name="event-1223-res_ipaddr_invalid_network_role"></a>Evento 1223: RES_IPADDR_INVALID_NETWORK_ROLE

O recurso de endereço IP do cluster ' %1 ' não pode ser colocado online porque a rede de cluster ' %2 ' não está configurada para permitir acesso de cliente. Use o snap-in Gerenciador de Cluster de Failover para verificar as propriedades configuradas da rede de cluster.

### <a name="event-1226-res_netname_tcb_not_held"></a>Evento 1226: RES_NETNAME_TCB_NOT_HELD

O recurso de nome de rede ' %1 ' (com o nome de rede associado ' %2 ') tem o suporte de autenticação Kerberos habilitado. Falha ao adicionar as credenciais necessárias ao LSA-o código de erro associado ' %3 ' indica privilégios insuficientes normalmente necessários para esta operação. O privilégio necessário é ' base de computação confiável ' e deve ser habilitado localmente em cada nó que compreende o cluster.

### <a name="event-1227-res_netname_lsa_error"></a>Evento 1227: RES_NETNAME_LSA_ERROR

O recurso de nome de rede ' %1 ' (com o nome de rede associado ' %2 ') tem o suporte de autenticação Kerberos habilitado. Falha ao adicionar as credenciais necessárias à LSA-o código de erro associado é ' %3 '.

### <a name="event-1228-res_netname_clone_failure"></a>Evento 1228: RES_NETNAME_CLONE_FAILURE

O recurso de nome de rede de cluster ' %1 ' encontrou um erro ao habilitar o nome de rede neste nó. O motivo da falha foi: <br> ' %2 '.<br><br> O código de erro era ' %3 '. <br><br> Você pode colocar o recurso de nome de rede offline e online novamente para tentar novamente.

### <a name="event-1229-res_netname_no_ips_for_dns"></a>Evento 1229: RES_NETNAME_NO_IPS_FOR_DNS

O recurso de nome de rede de cluster ' %1 ' não pôde identificar nenhum endereço IP para se registrar em um servidor DNS. Verifique se há uma ou mais redes habilitadas para uso do cluster com a configuração ' permitir que os clientes se conectem por meio desta rede ' e se cada nó tem um endereço IP válido configurado para as redes.

### <a name="event-1230-rcm_deadlock_or_crash_detected"></a>Evento 1230: RCM_DEADLOCK_OR_CRASH_DETECTED

Um componente no servidor não respondeu em tempo hábil. Isso fez com que o recurso de cluster ' %1 ' (tipo de recurso ' %2 ', DLL ' %3 ') exceda seu limite de tempo limite. Como parte da detecção de integridade do cluster, as ações de recuperação serão executadas.
O cluster tentará se recuperar automaticamente encerrando e reiniciando o processo de Subsistema de Hospedagem de Recursos (RHS) que está executando esse recurso. Verifique se a infraestrutura subjacente (como armazenamento, rede ou serviços) associada ao recurso está funcionando corretamente.

### <a name="event-1230-rcm_resource_control_deadlock_detected"></a>Evento 1230: RCM_RESOURCE_CONTROL_DEADLOCK_DETECTED

Um componente no servidor não respondeu em tempo hábil. Isso fez com que o recurso de cluster ' %1 ' (tipo de recurso ' %2 ', DLL ' %3 ') exceda seu limite de tempo limite durante o processamento do código de controle ' %4; '. Como parte da detecção de integridade do cluster, as ações de recuperação serão executadas. O cluster tentará se recuperar automaticamente encerrando e reiniciando o processo de Subsistema de Hospedagem de Recursos (RHS) que está executando esse recurso. Verifique se a infraestrutura subjacente (como armazenamento, rede ou serviços) associada ao recurso está funcionando corretamente.

### <a name="event-1231-res_netname_logon_failure"></a>Evento 1231: RES_NETNAME_LOGON_FAILURE

O recurso de nome de rede de cluster ' %1 ' encontrou um erro ao fazer logon no domínio. O motivo da falha foi: <br> ' %2 '.<br><br> O código de erro era ' %3 '.
<br><br> Verifique se um controlador de domínio está acessível para esse nó dentro do domínio configurado.

### <a name="event-1232-res_genscript_timeout"></a>Evento 1232: RES_GENSCRIPT_TIMEOUT

O ponto de entrada ' %2 ' no recurso de script genérico do cluster ' %1 ' não concluiu a execução em tempo hábil. Isso pode ser devido a um loop infinito ou a outros problemas, possivelmente resultando em uma espera infinita. Como alternativa, o valor de tempo limite pendente especificado pode ser muito curto para esse recurso. Examine o ponto de entrada de script ' %2 ' para garantir que todas as esperas infinitas possíveis no código de script foram corrigidas. Em seguida, considere aumentar o valor de tempo limite pendente se considerado necessário.

### <a name="event-1233-res_genscript_hangmode"></a>Evento 1233: RES_GENSCRIPT_HANGMODE

Recurso de script genérico do cluster ' %1 ': a solicitação para executar a operação ' %2 ' não será processada. Isso ocorre devido a uma tentativa com falha anteriormente de executar o ponto de entrada ' %3 ' em tempo hábil. Examine o código do script para esse ponto de entrada para garantir que não exista nenhum loop infinito ou outros problemas, possivelmente resultando em uma espera infinita. Em seguida, considere aumentar o valor de tempo limite pendente do recurso se considerado necessário.

### <a name="event-1234-cluster_event_account_missing_privs"></a>Evento 1234: CLUSTER_EVENT_ACCOUNT_MISSING_PRIVS

O Serviço de cluster detectou que sua conta de serviço não tem um ou mais dos privilégios necessários. A lista de privilégios ausentes é: ' %1 ' e não é concedida atualmente à conta de serviço. Use o ' sc.exe qprivs ClusSvc ' para verificar os privilégios do Serviço de cluster (ClusSvc). Além disso, verifique se há políticas de segurança ou políticas de grupo em Active Directory Domain Services que podem ter alterado os privilégios padrão. Digite o seguinte comando para conceder ao Serviço de cluster os privilégios necessários para funcionar corretamente:

```
sc.exe privs
clussvc
SeBackupPrivilege/SeRestorePrivilege/SeIncreaseQuotaPrivilege/SeIncreaseBasePriorityPrivilege/SeTcbPrivilege/SeDebugPrivilege/SeSecurityPrivilege/SeAuditPrivilege/SeImpersonatePrivilege/SeChangeNotifyPrivilege/SeIncreaseWorkingSetPrivilege/SeManageVolumePrivilege/SeCreateSymbolicLinkPrivilege/SeLoadDriverPrivilege
```

### <a name="event-1242-res_ipaddr_lease_expired"></a>Evento 1242: RES_IPADDR_LEASE_EXPIRED

A concessão do endereço IP ' %2 ' associada ao recurso de endereço IP do cluster ' %1 ' expirou ou está prestes a expirar e, no momento, não pode ser renovado. Verifique se o servidor DHCP associado está acessível e configurado corretamente para renovar a concessão nesse endereço IP.

### <a name="event-1245-res_ipaddr_lease_renewal_failed"></a>Evento 1245: RES_IPADDR_LEASE_RENEWAL_FAILED

O recurso de endereço IP do cluster ' %1 ' falhou ao renovar a concessão para o endereço IP ' %2 '.
Verifique se o servidor DHCP está acessível e configurado corretamente para renovar a concessão nesse endereço IP.

### <a name="event-1250-rcm_resource_embedded_failure"></a>Evento 1250: RCM_RESOURCE_EMBEDDED_FAILURE

O recurso de cluster ' %1 ' na função clusterizada ' %2 ' recebeu uma notificação de estado crítico. Para uma máquina virtual, isso indica que um aplicativo ou serviço dentro da máquina virtual está em um estado não íntegro. Verifique a funcionalidade do serviço ou do aplicativo que está sendo monitorado na máquina virtual.

### <a name="event-1254-rcm_group_terminal_failure"></a>Evento 1254: RCM_GROUP_TERMINAL_FAILURE

A função clusterizada ' %1 ' excedeu seu limite de failover. Ele esgotou o número configurado de tentativas de failover dentro do período de failover de tempo alocado para ele e será deixado em um estado de falha. Nenhuma tentativa adicional será feita para colocar a função online ou fazer o failover para outro nó no cluster.
Verifique os eventos associados à falha. Depois que os problemas que causam a falha forem resolvidos, a função poderá ser colocada online manualmente ou o cluster poderá tentar colocá-lo online novamente após o período de atraso de reinicialização.

### <a name="event-1255-rcm_resource_network_failure"></a>Evento 1255: RCM_RESOURCE_NETWORK_FAILURE

O recurso de cluster ' %1 ' na função clusterizada ' %2 ' recebeu uma notificação de estado crítico. Para uma máquina virtual, isso indica que uma rede crítica da máquina virtual está em um estado não íntegro. Verifique a conectividade de rede da máquina virtual e as redes virtuais que a máquina virtual está configurada para usar.

### <a name="event-1256-res_netname_dns_registration_failed_dynamic_dns_zone"></a>Evento 1256: RES_NETNAME_DNS_REGISTRATION_FAILED_DYNAMIC_DNS_ZONE

O recurso de nome de rede de cluster falhou no registro de um ou mais nomes DNS associados porque a zona DNS correspondente não aceita atualizações dinâmicas.<br><br>Nome da rede do cluster: ' %1 '<br>Zona DNS: ' %2 '

#### <a name="guidance"></a>Diretrizes

Verifique se o DNS está configurado como uma zona DNS dinâmica. Se o servidor DNS não aceitar atualizações dinâmicas, desmarque a lista ' registrar os endereços desta conexão no DNS ' nas propriedades do adaptador de rede.

### <a name="event-1257-res_netname_dns_registration_failed_secure_dns_zone"></a>Evento 1257: RES_NETNAME_DNS_REGISTRATION_FAILED_SECURE_DNS_ZONE

O recurso de nome de rede de cluster falhou no registro de um ou mais nomes DNS associados porque o acesso para atualizar a zona DNS segura foi negado.<br><br>Nome da rede do cluster: ' %1 '<br>Zona DNS: ' %2 '<br><br>Verifique se o CNO (objeto de nome de cluster) recebeu permissões para a zona DNS segura.

### <a name="event-1258-res_netname_dns_registration_failed_timeout"></a>Evento 1258: RES_NETNAME_DNS_REGISTRATION_FAILED_TIMEOUT

Falha no registro do recurso de nome de rede do cluster de um ou mais nomes DNS associados, pois o servidor DNS não pôde ser acessado.<br><br>Nome da rede do cluster: ' %1 '<br>Zona DNS: ' %2 '<br>Servidor DNS: ' %3 '<br><br>Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com pelo menos um servidor DNS acessível.

### <a name="event-1259-res_netname_dns_registration_failed_cleanup"></a>Evento 1259: RES_NETNAME_DNS_REGISTRATION_FAILED_CLEANUP

O recurso de nome de rede do cluster falhou no registro de um ou mais nomes DNS associados porque o serviço de cluster falhou ao limpar os registros existentes correspondentes ao nome da rede.<br><br>Nome da rede do cluster: ' %1 '<br>Zona DNS: ' %2 '<br><br>Verifique se o CNO (objeto de nome de cluster) recebeu permissões para a zona DNS segura.

### <a name="event-1260-res_netname_dns_registration_modify_failed"></a>Evento 1260: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED

Falha do recurso de nome de rede de cluster ao modificar o registro de DNS.<br><br>Nome da rede do cluster: ' %1 '<br>Código de erro: ' %2 '

#### <a name="guidance"></a>Diretrizes

Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com acesso a pelo menos um servidor DNS.

### <a name="event-1261-res_netname_dns_registration_modify_failed_status"></a>Evento 1261: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED_STATUS

Falha do recurso de nome de rede de cluster ao modificar o registro de DNS.<br><br>Nome da rede do cluster: ' %1 '<br>Motivo: ' %2 '

#### <a name="guidance"></a>Diretrizes

Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com acesso a pelo menos um servidor DNS.

### <a name="event-1262-res_netname_dns_registration_publish_ptr_failed"></a>Evento 1262: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED

Falha do recurso de nome de rede do cluster ao publicar o registro PTR na zona de pesquisa inversa do DNS.<br><br>Nome da rede do cluster: ' %1 '<br>Código de erro: ' %2 '

#### <a name="guidance"></a>Diretrizes

Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com acesso a pelo menos um servidor DNS e se a zona de pesquisa inversa de DNS existe.

### <a name="event-1264-res_netname_dns_registration_publish_ptr_failed_status"></a>Evento 1264: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED_STATUS

Falha do recurso de nome de rede do cluster ao publicar o registro PTR na zona de pesquisa inversa do DNS.<br><br>Nome da rede do cluster: ' %1 '<br>Motivo: ' %2 '

#### <a name="guidance"></a>Diretrizes

Verifique se os adaptadores de rede associados aos recursos de endereço IP dependentes estão configurados com acesso a pelo menos um servidor DNS e se a zona de pesquisa inversa de DNS existe.

### <a name="event-1265-res_type_control_timed_out"></a>Evento 1265: RES_TYPE_CONTROL_TIMED_OUT

O tipo de recurso de cluster ' %1 ' atingiu o tempo limite ao processar o código de controle %2. O cluster tentará se recuperar automaticamente encerrando e reiniciando o processo de Subsistema de Hospedagem de Recursos (RHS) que estava processando a chamada.

### <a name="event-1289-netft_adapter_not_found"></a>Evento 1289: NETFT_ADAPTER_NOT_FOUND

O serviço de cluster não pôde acessar o adaptador de rede ' %1 '. Verifique se outros adaptadores de rede estão funcionando corretamente e verifique o Gerenciador de dispositivos em busca de erros associados ao adaptador ' %1 '. Se a configuração do adaptador ' %1 ' tiver sido alterada, poderá ser necessário reinstalar o recurso de clustering de failover neste computador.

### <a name="event-1360-res_ipaddr_invalid_network"></a>Evento 1360: RES_IPADDR_INVALID_NETWORK

O recurso de endereço IP do cluster ' %1 ' não pôde ser colocado online. Verifique se a propriedade de rede ' %2 ' corresponde a um nome de rede de cluster ou se a propriedade de endereço ' %3 ' corresponde a uma das sub-redes em uma rede de cluster. Se esse for um tipo de endereço IPv6, verifique se a rede de cluster correspondente a esse recurso tem pelo menos um prefixo IPv6 que não seja link-local ou de túnel.

### <a name="event-1361-res_ipaddr_missing_dependant"></a>Evento 1361: RES_IPADDR_MISSING_DEPENDANT

O recurso de endereço de túnel IPv6 ' %1 ' não foi colocado online porque não depende de um recurso de endereço IP (IPv4). É necessária uma dependência em pelo menos um recurso de endereço IP (IPv4).

### <a name="event-1362-res_ipaddr_missing_data"></a>Evento 1362: RES_IPADDR_MISSING_DATA

O recurso de endereço IP do cluster ' %1 ' não foi colocado online porque a propriedade ' %2 ' não pôde ser lida. Verifique se o recurso está configurado corretamente.

### <a name="event-1363-res_ipaddr_no_isatap_support"></a>Evento 1363: RES_IPADDR_NO_ISATAP_SUPPORT

O recurso de endereço de túnel IPv6 ' %1 ' não pôde ser colocado online. A rede de cluster ' %2 ' associada ao recurso de endereço IP dependente (IPv4) ' %3 ' não oferece suporte a túnel ISATAP. Verifique se a rede de cluster dá suporte ao túnel ISATAP.

### <a name="event-1540-service_backup_noquorum"></a>Evento 1540: SERVICE_BACKUP_NOQUORUM

A operação de backup dos dados de configuração do cluster foi anulada porque o quorum do cluster ainda não foi atingido. Repita a operação de backup depois que o cluster atingir o quorum.

### <a name="event-1554-service_restore_invaliduser"></a>Evento 1554: SERVICE_RESTORE_INVALIDUSER

Falha na operação de restauração dos dados de configuração do cluster. Isso ocorreu devido a privilégios insuficientes associados à conta de usuário que executa a restauração. Verifique se a conta de usuário tem privilégios de administrador local.

### <a name="event-1557-service_witness_attach_failed"></a>Evento 1557: SERVICE_WITNESS_ATTACH_FAILED

Serviço de cluster falha ao atualizar os dados de configuração do cluster no recurso de testemunha. Verifique se o recurso de testemunha está online e acessível.

### <a name="event-1559-res_witness_new_node_conflict"></a>Evento 1559: RES_WITNESS_NEW_NODE_CONFLICT

O compartilhamento de arquivos ' %1 ' associado ao recurso de testemunha de compartilhamento de arquivos está atualmente hospedado pelo servidor ' %2 '. Este servidor ' %2 ' acabou de ser adicionado como um novo nó dentro do cluster de failover. A hospedagem da testemunha de compartilhamento de arquivos por qualquer nó que compreende o mesmo cluster não é recomendada. Escolha uma testemunha de compartilhamento de arquivos que não seja hospedada por nenhum nó dentro do mesmo cluster e modifique as configurações do recurso de testemunha de compartilhamento de arquivos adequadamente.

### <a name="event-1560-res_smb_share_conflict"></a>Evento 1560: RES_SMB_SHARE_CONFLICT

O recurso de compartilhamento de arquivos de cluster ' %1 ' detectou conflitos de pastas compartilhadas. Como resultado, algumas dessas pastas compartilhadas podem não estar acessíveis. Para corrigir essa situação, certifique-se de que várias pastas compartilhadas não tenham o mesmo nome de compartilhamento.

### <a name="event-1563-res_fsw_onlinefailure"></a>Evento 1563: RES_FSW_ONLINEFAILURE

O recurso de testemunha de compartilhamento de arquivos ' %1 ' não pôde ser colocado online. Verifique se o compartilhamento de arquivos ' %2 ' existe e se está acessível pelo cluster.

### <a name="event-1566-res_netname_timedout"></a>Evento 1566: RES_NETNAME_TIMEDOUT

O recurso de nome de rede de cluster ' %1 ' não pode ser colocado online. O recurso de nome de rede foi encerrado pelo subsistema do host de recursos porque não concluiu uma operação em um tempo aceitável. A operação atingiu o tempo limite ao executar:<br> ' %2 '

### <a name="event-1567-service_failed_to_change_log_size"></a>Evento 1567: SERVICE_FAILED_TO_CHANGE_LOG_SIZE

Serviço de cluster falha ao alterar o tamanho do log de rastreamento. Verifique a configuração de ClusterLogSize com o cmdlet do PowerShell ' Get-cluster \| Format-List \* '. Além disso, use o snap-in Monitor de desempenho para verificar as configurações de sessão de rastreamento de eventos para FailoverClustering.

### <a name="event-1567-res_vipaddr_address_interface_failed"></a>Evento 1567: RES_VIPADDR_ADDRESS_INTERFACE_FAILED

A verificação de integridade para a interface IP ' %1 ' (endereço ' %2 ') falhou (o status é ' %3 '). Verifique se há erros de hardware ou software relacionados aos adaptadores de rede física ou virtual.

### <a name="event-1568-res_cloud_witness_cant_communicate_to_azure"></a>Evento 1568: RES_CLOUD_WITNESS_CANT_COMMUNICATE_TO_AZURE

O recurso de testemunha em nuvem não pôde alcançar serviços de armazenamento Microsoft Azure.<br><br>Recurso de cluster: %1 <br>Nó do cluster: %2

#### <a name="guidance"></a>Diretrizes

Isso pode ser devido à comunicação de rede entre o nó de cluster e o serviço de Microsoft Azure que está sendo bloqueado. Verifique a conectividade com a Internet do nó para Microsoft Azure. Conecte-se ao portal do Microsoft Azure e verifique se a conta de armazenamento existe.

### <a name="event-1569-service_using_restricted_network"></a>Evento 1569: SERVICE_USING_RESTRICTED_NETWORK

A rede ' %1 ' que foi desabilitada para uso de cluster de failover foi considerada a única rede possível atualmente que o nó ' %2 ' pode usar para se comunicar com outros nós no cluster. Isso pode afetar a capacidade do nó de participar do cluster. Verifique a conectividade de rede do nó ' %2 ' e habilite pelo menos uma rede para a comunicação do cluster. Execute o assistente para validar uma configuração para verificar sua configuração de rede.

### <a name="event-1569-res_cloud_witness_token_expired"></a>Evento 1569: RES_CLOUD_WITNESS_TOKEN_EXPIRED

Falha do recurso de testemunha na nuvem ao autenticar com serviços de armazenamento Microsoft Azure. Um erro de acesso negado foi retornado ao tentar entrar em contato com a conta de armazenamento de Microsoft Azure. <br><br>Recurso de cluster: %1

#### <a name="guidance"></a>Diretrizes

A chave de acesso da conta de armazenamento pode não ser mais válida. Use o assistente para configurar quorum de cluster no Gerenciador de Cluster de Failover ou no cmdlet Set-ClusterQuorum do Windows PowerShell, para configurar o recurso de testemunha na nuvem com a chave de acesso da conta de armazenamento atualizada.

### <a name="event-1573-service_form_witness_failed"></a>Evento 1573: SERVICE_FORM_WITNESS_FAILED

O nó ' %1 ' não pôde formar um cluster. Isso ocorreu porque a testemunha não estava acessível. Verifique se o recurso de testemunha está online e disponível.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 ' em uma zona DNS segura. Isso ocorreu devido a um registro existente com esse nome, e a identidade do cluster não tem privilégios suficientes para atualizar esse registro. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster tem permissões no registro DNS ' %2 '.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 ' em uma zona DNS segura. Isso ocorreu devido a um registro existente com esse nome, e a identidade do cluster não tem privilégios suficientes para atualizar esse registro. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster tem permissões no registro DNS ' %2 '.

### <a name="event-1585-res_fileserver_fscheck_srvsvc_stopped"></a>Evento 1585: RES_FILESERVER_FSCHECK_SRVSVC_STOPPED

O recurso de servidor de arquivos de cluster ' %1 ' falhou em uma verificação de integridade. Isso se deve ao fato de o serviço do servidor não estar sendo iniciado. Use Gerenciador do Servidor para confirmar o estado do serviço servidor neste nó de cluster.

### <a name="event-1586-res_fileserver_fscheck_scoped_name_not_registered"></a>Evento 1586: RES_FILESERVER_FSCHECK_SCOPED_NAME_NOT_REGISTERED

O recurso de servidor de arquivos de cluster ' %1 ' falhou em uma verificação de integridade. Isso porque algumas de suas pastas compartilhadas estavam inacessíveis. Verifique se as pastas estão acessíveis de clientes. Além disso, confirme o estado do serviço servidor nesse nó de cluster usando Gerenciador do Servidor e procure outros eventos relacionados ao serviço servidor nesse nó de cluster. Pode ser necessário reiniciar o recurso de nome de rede ' %2 ' nesta função clusterizada.

### <a name="event-1587-res_fileserver_fscheck_failed"></a>Evento 1587: RES_FILESERVER_FSCHECK_FAILED

O recurso de servidor de arquivos de cluster ' %1 ' falhou em uma verificação de integridade. Isso porque algumas de suas pastas compartilhadas estavam inacessíveis. Verifique se as pastas estão acessíveis de clientes. Além disso, confirme o estado do serviço servidor nesse nó de cluster usando Gerenciador do Servidor e procure outros eventos relacionados ao serviço servidor nesse nó de cluster.

### <a name="event-1588-res_fileserver_share_cant_add"></a>Evento 1588: RES_FILESERVER_SHARE_CANT_ADD

O recurso de servidor de arquivos de cluster ' %1 ' não pode ser colocado online. O recurso não pôde criar o compartilhamento de arquivos ' %2 ' associado ao nome de rede ' %3 '. O código de erro era ' %4 '. Verifique se as pastas existem e estão acessíveis. Além disso, confirme o estado do serviço servidor nesse nó de cluster usando Gerenciador do Servidor e procure outros eventos relacionados neste nó de cluster. Pode ser necessário reiniciar o recurso de nome de rede ' %3 ' nesta função clusterizada.

### <a name="event-1600-clusapi_create_cannot_set_ad_dacl"></a>Evento 1600: CLUSAPI_CREATE_CANNOT_SET_AD_DACL

Serviço de cluster falhou ao definir as permissões no objeto de computador do cluster ' %1 '. Entre em contato com seu administrador de rede para verificar o descritor de segurança de cluster do objeto de computador em Active Directory, verifique se a DACL não é muito grande e remova qualquer ACE extra desnecessário no objeto, se necessário.

### <a name="event-1603-res_fileserver_clone_failed"></a>Evento 1603: RES_FILESERVER_CLONE_FAILED

Não foi possível iniciar o servidor de arquivos porque a dependência esperada no recurso ' nome de rede ' não foi encontrada ou não foi configurada corretamente. Erro = 0x %2.

### <a name="event-1606-res_disk_cno_check_failed"></a>Evento 1606: RES_DISK_CNO_CHECK_FAILED

O recurso de disco de cluster ' %1 ' contém um volume protegido pelo BitLocker, ' %2 ', mas, para esse volume, a conta de nome de cluster Active Directory (também chamada de objeto de nome de cluster ou CNO) não é um protetor BitLocker para o volume. Isso é necessário para volumes protegidos pelo BitLocker. Para corrigir isso, primeiro remova o disco do cluster. Em seguida, use a ferramenta de linha de comando Manage-bde.exe para adicionar o nome do cluster como um protetor ADAccountOrGroup, usando o formato \\ ClusterName \$ para o nome do cluster. Em seguida, adicione o disco de volta ao cluster. Para obter mais informações, consulte a documentação para Manage-bde.exe

### <a name="event-1607-res_disk_cno_unlock_failed"></a>Evento 1607: RES_DISK_CNO_UNLOCK_FAILED

O recurso de disco de cluster ' %1 ' não pôde desbloquear o volume protegido pelo BitLocker ' %2 '. O objeto de nome de cluster (CNO) não está definido para ser um protetor BitLocker válido para este volume. Para corrigir isso, remova o disco do cluster. Em seguida, use a ferramenta de linha de comando Manage-bde.exe para adicionar o nome do cluster como um protetor ADAccountOrGroup, usando o formato \\ ClusterName \$ e adicione o disco de volta ao cluster. Para obter mais informações, consulte a documentação para Manage-bde.exe.

### <a name="event-1608-res_fileserver_leader_failed"></a>Evento 1608: RES_FILESERVER_LEADER_FAILED

Não foi possível iniciar o servidor de arquivos porque a dependência esperada no recurso ' nome de rede ' não foi encontrada ou não foi configurada corretamente. Erro = 0x %2.

### <a name="event-1609-res_soda_fileserver_leader_failed"></a>Evento 1609: RES_SODA_FILESERVER_LEADER_FAILED

Não foi possível iniciar o servidor de arquivos Scale Out porque a dependência esperada do recurso ' nome de rede distribuída ' não foi encontrada ou não foi configurada corretamente. Erro = 0x %2.

### <a name="event-1632-clusapi_create_mismatched_ou"></a>Evento 1632: CLUSAPI_CREATE_MISMATCHED_OU

Falha na criação do cluster. Não é possível criar o objeto de nome de cluster ' %1 ' na unidade organizacional do Active Directory ' %2 '. O objeto já existe na unidade organizacional ' %3 '. Verifique se o caminho de nome distinto especificado e o objeto de nome de cluster estão corretos. Se o caminho do nome distinto não for especificado, o objeto de computador existente ' %1 ' será usado.

### <a name="event-1652-service_tcp_connection_failure"></a>Evento 1652: SERVICE_TCP_CONNECTION_FAILURE

O nó de cluster ' %1 ' não pôde ingressar no cluster. Uma conexão TCP não pôde ser estabelecida com nó (s) ' %2 '. Verifique a conectividade de rede e a configuração de qualquer firewall de rede.

### <a name="event-1652-service_udp_connection_failure"></a>Evento 1652: SERVICE_UDP_CONNECTION_FAILURE

O nó de cluster ' %1 ' não pôde ingressar no cluster. Uma conexão UDP não pôde ser estabelecida com nó (s) ' %2 '. Verifique a conectividade de rede e a configuração de qualquer firewall de rede.

### <a name="event-1652-service_virtual_tcp_connection_failure"></a>Evento 1652: SERVICE_VIRTUAL_TCP_CONNECTION_FAILURE

O nó de cluster ' %1 ' não pôde ingressar no cluster. Não foi possível estabelecer uma conexão TCP usando o adaptador virtual do cluster de failover da Microsoft aos nós ' %2 '. Verifique a conectividade de rede e a configuração de qualquer firewall de rede.

### <a name="event-1653-service_no_connectivity"></a>Evento 1653: SERVICE_NO_CONNECTIVITY

O nó de cluster ' %1 ' não pôde ingressar no cluster porque ele não pôde se comunicar pela rede com nenhum outro nó no cluster. Verifique a conectividade de rede e a configuração de qualquer firewall de rede.

### <a name="event-1654-res_vipaddr_invalid_adaptername"></a>Evento 1654: RES_VIPADDR_INVALID_ADAPTERNAME

O recurso de endereço IP não contíguo do cluster ' %1 ' falhou ao ser colocado online. Não foi possível determinar os dados de configuração do adaptador de rede correspondente ao adaptador de rede ' %2 ' (código de erro: ' %3 '). Verifique se o recurso de endereço IP está configurado com as propriedades de endereço e rede corretas.

### <a name="event-1655-res_vipaddr_invalid_vsid"></a>Evento 1655: RES_VIPADDR_INVALID_VSID

O recurso de endereço IP não contíguo do cluster ' %1 ' falhou ao ser colocado online. Não foi possível determinar os dados de configuração do adaptador de rede correspondente à ID de sub-rede virtual ' %2 ' e à ID de domínio de roteamento ' %3 ' (código de erro: ' %4 '). Verifique se o recurso de endereço IP está configurado com as propriedades de endereço e rede corretas.

### <a name="event-1656-res_vipaddr_address_create_failed"></a>Evento 1656: RES_VIPADDR_ADDRESS_CREATE_FAILED

Falha ao adicionar o endereço IP ' %2 ' para o recurso de endereço IP não contíguo ' %1 ' (código de erro: ' %3 '). Verifique se há erros de hardware ou software relacionados aos adaptadores de rede física ou virtual.

### <a name="event-1664-cluster_upgrade_incomplete"></a>Evento 1664: CLUSTER_UPGRADE_INCOMPLETE

Falha ao atualizar o nível funcional do cluster. Verifique se todos os nós do cluster estão em execução no momento e se são da mesma versão do Windows Server e execute o cmdlet Update-ClusterFunctionalLevel do Windows PowerShell novamente.

### <a name="event-1676-event_local_node_quarantined"></a>Evento 1676: EVENT_LOCAL_NODE_QUARANTINED

O nó do cluster local foi colocado em quarentena por ' %1 '. O nó será colocado em quarentena até ' %2 ' e, em seguida, o nó tentará novamente ingressar no cluster automaticamente.
<br><br>Consulte os logs de eventos do sistema e do aplicativo para determinar os problemas neste nó. Quando o problema for resolvido, a quarentena poderá ser limpa manualmente para permitir que o nó ingresse novamente com o cmdlet ' Start-ClusterNode – ClearQuarantine ' do Windows PowerShell.<br><br>QuarantineType: em quarentena por %1<br>A quarentena de tempo será automaticamente desmarcada: %2

### <a name="event-1677-event_node_drain_failed"></a>Evento 1677: EVENT_NODE_DRAIN_FAILED

Falha no descarregamento do nó no nó de cluster %1. <br><br>Referencie os logs de eventos do sistema e do aplicativo do nó e os logs de cluster para investigar a causa da falha de descarga. Quando o problema for resolvido, você poderá repetir a operação de descarga.

### <a name="event-1683-res_netname_computer_account_no_dc"></a>Evento 1683: RES_NETNAME_COMPUTER_ACCOUNT_NO_DC

O serviço de cluster não pôde acessar nenhum controlador de domínio disponível no domínio. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Servidor DC: %1

#### <a name="guidance"></a>Diretrizes

Verifique se os controladores de domínio estão acessíveis na rede para os nós de cluster.

### <a name="event-1684-res_netname_computer_object_vco_not_found"></a>Evento 1684: RES_NETNAME_COMPUTER_OBJECT_VCO_NOT_FOUND

O recurso de nome de rede do cluster não pôde localizar o objeto de computador associado no Active Directory. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Nome da rede: %1<br>Unidade organizacional: %2

#### <a name="guidance"></a>Diretrizes

Restaure o objeto de computador para o nome da rede na lixeira Active Directory. Como alternativa, offline o recurso de nome de rede de cluster e execute a ação de reparo para recriar o objeto de computador no Active Directory.

### <a name="event-1685-res_netname_computer_object_cno_not_found"></a>Evento 1685: RES_NETNAME_COMPUTER_OBJECT_CNO_NOT_FOUND

O recurso de nome de rede do cluster não pôde localizar o objeto de computador associado no Active Directory. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Nome da rede: %1<br>Unidade organizacional: %2
#### <a name="guidance"></a>Diretrizes

Restaure o objeto de computador para o nome da rede na lixeira Active Directory.

### <a name="event-1686-res_netname_computer_object_vco_disabled"></a>Evento 1686: RES_NETNAME_COMPUTER_OBJECT_VCO_DISABLED

O recurso de nome de rede de cluster encontrou o objeto de computador associado no Active Directory a ser desabilitado. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Nome da rede: %1<br>Unidade organizacional: %2

#### <a name="guidance"></a>Diretrizes

Habilite o objeto de computador para o nome de rede em Active Directory.

### <a name="event-1687-res_netname_computer_object_cno_disabled"></a>Evento 1687: RES_NETNAME_COMPUTER_OBJECT_CNO_DISABLED

O recurso de nome de rede de cluster encontrou o objeto de computador associado no Active Directory a ser desabilitado. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Nome da rede: %1<br>Unidade organizacional: %2

#### <a name="guidance"></a>Diretrizes

Habilite o objeto de computador para o nome de rede em Active Directory. Como alternativa, offline o recurso de nome de rede de cluster e execute a ação de reparo para habilitar o objeto de computador em Active Directory.

### <a name="event-1688-res_netname_computer_object_failed"></a>Evento 1688: RES_NETNAME_COMPUTER_OBJECT_FAILED

O recurso de nome de rede de cluster detectou que o objeto de computador associado no Active Directory foi desabilitado e falhou em sua tentativa de habilitá-lo. Isso pode afetar a funcionalidade que depende da autenticação de nome de rede do cluster.<br><br>Nome da rede: %1<br>Unidade organizacional: %2

#### <a name="guidance"></a>Diretrizes

Habilite o objeto de computador para o nome de rede em Active Directory.

### <a name="event-4608-nodecleanup_get_clustered_disks_failed"></a>Evento 4608: NODECLEANUP_GET_CLUSTERED_DISKS_FAILED

Serviço de cluster falhou ao recuperar a lista de discos clusterizados ao destruir o cluster. O código de erro era ' %1 '. Se esses discos não estiverem acessíveis, execute o cmdlet ' Clear-ClusterDiskReservation ' do PowerShell.

### <a name="event-4611-nodecleanup_release_clustered_disks_from_partmgr_failed"></a>Evento 4611: NODECLEANUP_RELEASE_CLUSTERED_DISKS_FROM_PARTMGR_FAILED

O disco clusterizado com a ID ' %2 ' não foi liberado pelo Gerenciador de partições ao destruir o cluster. O código de erro era ' %1 '. Reiniciar o computador garantirá que o disco seja liberado pelo Gerenciador de partições.

### <a name="event-4613-nodecleanup_clear_clusdisk_database_failed"></a>Evento 4613: NODECLEANUP_CLEAR_CLUSDISK_DATABASE_FAILED

O serviço de cluster não pôde limpar corretamente um disco clusterizado com a ID ' %2 ' ao destruir o cluster. O código de erro era ' %1 '. Você pode não conseguir acessar este disco até que a limpeza tenha sido concluída com êxito. Para a limpeza manual, exclua o valor ' AttachedDisks ' da chave ' HKEY_LOCAL_MACHINE \\ System \\ CurrentControlSet \\ Services \\ Clusdisk \\ Parameters ' no registro do Windows.

### <a name="event-4615-nodecleanup_disable_cluster_service_failed"></a>Evento 4615: NODECLEANUP_DISABLE_CLUSTER_SERVICE_FAILED

O serviço de cluster foi interrompido e definido como desabilitado como parte da limpeza do nó de cluster.

### <a name="event-4616-nodecleanup_disable_cluster_service_timeout"></a>Evento 4616: NODECLEANUP_DISABLE_CLUSTER_SERVICE_TIMEOUT

O encerramento do serviço de cluster durante a limpeza do nó de cluster não foi concluído no período de tempo esperado. Reinicie este computador para garantir que o serviço de cluster não esteja mais em execução.

### <a name="event-4618-nodecleanup_reset_cluster_registry_entries_failed"></a>Evento 4618: NODECLEANUP_RESET_CLUSTER_REGISTRY_ENTRIES_FAILED

Falha na redefinição das entradas do registro do serviço de cluster durante a limpeza do nó do cluster.
O código de erro era ' %1 '. Talvez você não consiga criar ou ingressar em um cluster com este computador até que a limpeza tenha sido concluída com êxito. Para a limpeza manual, execute o cmdlet ' Clear-ClusterNode ' do PowerShell neste computador.

### <a name="event-4620-nodecleanup_unload_cluster_hive_failed"></a>Evento 4620: NODECLEANUP_UNLOAD_CLUSTER_HIVE_FAILED

Falha ao descarregar o hive do registro do serviço de cluster durante a limpeza do nó do cluster.
O código de erro era ' %1 '. Talvez você não consiga criar ou ingressar em um cluster com este computador até que a limpeza tenha sido concluída com êxito. Para a limpeza manual, execute o cmdlet ' Clear-ClusterNode ' do PowerShell neste computador.

### <a name="event-4622-nodecleanup_errors"></a>Evento 4622: NODECLEANUP_ERRORS

O Serviço de cluster encontrou um erro durante a limpeza do nó. Talvez você não consiga criar ou ingressar em um cluster com este computador até que a limpeza tenha sido concluída com êxito. Use o cmdlet ' Clear-ClusterNode ' do PowerShell neste nó.

### <a name="event-4624-nodecleanup_reset_nlbsflags_failed"></a>Evento 4624: NODECLEANUP_RESET_NLBSFLAGS_FAILED

Falha ao redefinir o valor do registro de tempo limite da Associação de segurança IPSec durante a limpeza do nó do cluster. O código de erro era ' %1 '. Para a limpeza manual, execute o cmdlet ' Clear-ClusterNode ' do PowerShell neste computador. Como alternativa, você pode redefinir o tempo limite da Associação de segurança IPSec excluindo o valor ' %2 ' e o valor ' %3 ' de HKEY_LOCAL_MACHINE no registro do Windows.

### <a name="event-4627-nodecleanup_delete_cluster_tasks_failed"></a>Evento 4627: NODECLEANUP_DELETE_CLUSTER_TASKS_FAILED

Falha na exclusão de tarefas clusterizadas durante a limpeza do nó. O código de erro era ' %1 '.
Use o Windows Agendador de Tarefas para excluir todas as tarefas clusterizadas restantes.

### <a name="event-4629-nodecleanup_delete_local_account_failed"></a>Evento 4629: NODECLEANUP_DELETE_LOCAL_ACCOUNT_FAILED

Durante a limpeza do nó, a conta de usuário local gerenciada pelo cluster não foi excluída. O código de erro era ' %1 '. Abra usuários e grupos locais (lusrmgr. msc) para excluir a conta.

### <a name="event-4864-res_vsstask_open_failed"></a>Evento 4864: RES_VSSTASK_OPEN_FAILED

Falha na criação do recurso ' %1 ' da tarefa do serviço de cópias de sombra de volume. O código de erro era ' %2 '.

### <a name="event-4865-res_vsstask_terminate_task_failed"></a>Evento 4865: RES_VSSTASK_TERMINATE_TASK_FAILED

Falha no recurso de tarefa do serviço de cópias de sombra de volume ' %1 '. O código de erro era ' %2 '.
Isso ocorre porque a tarefa associada não pôde ser interrompida como parte de uma operação offline. Talvez seja necessário interrompê-lo manualmente usando o snap-in de Agendador de Tarefas.

### <a name="event-4866-res_vsstask_delete_task_failed"></a>Evento 4866: RES_VSSTASK_DELETE_TASK_FAILED

Falha no recurso de tarefa do serviço de cópias de sombra de volume ' %1 '. O código de erro era ' %2 '.
Isso ocorre porque a tarefa associada não pôde ser excluída como parte de uma operação offline. Talvez seja necessário excluí-lo manualmente usando o snap-in de Agendador de Tarefas.

### <a name="event-4867-res_vsstask_online_failed"></a>Evento 4867: RES_VSSTASK_ONLINE_FAILED

Falha no recurso de tarefa do serviço de cópias de sombra de volume ' %1 '. O código de erro era ' %2 '.
Isso ocorre porque a tarefa associada não pôde ser adicionada como parte de uma operação online. Use o snap-in de Agendador de Tarefas para garantir que suas tarefas estejam configuradas corretamente.

### <a name="event-4868-unable_to_start_autologger"></a>Evento 4868: UNABLE_TO_START_AUTOLOGGER

Serviço de cluster falhou ao iniciar a sessão de rastreamento de log do cluster. O código de erro era ' %2 '. O cluster funcionará corretamente, mas as informações de log complementares estarão ausentes. Use o snap-in Monitor de desempenho para verificar as configurações de canal de evento para ' %1 '.

### <a name="event-4869-netft_watchdog_process_hung"></a>Evento 4869: NETFT_WATCHDOG_PROCESS_HUNG

O monitoramento de integridade do modo de usuário detectou que o sistema não está respondendo. O adaptador virtual do cluster de failover perdeu contato com o processo ' %1 ' com uma ID de processo ' %2 ', para ' %3 ' segundos. Use o monitor de desempenho para avaliar a integridade do sistema e determinar qual processo pode estar afetando negativamente o sistema.

### <a name="event-4870-netft_watchdog_process_terminated"></a>Evento 4870: NETFT_WATCHDOG_PROCESS_TERMINATED

O monitoramento de integridade do modo de usuário detectou que o sistema não está respondendo. O adaptador virtual do cluster de failover perdeu contato com o processo ' %1 ' com uma ID de processo ' %2 ', para ' %3 ' segundos. A ação de recuperação será executada.

### <a name="event-4871-netft_miniport_initialization_failure"></a>Evento 4871: NETFT_MINIPORT_INITIALIZATION_FAILURE

Falha ao iniciar o serviço de cluster. Isso ocorreu porque o adaptador virtual do cluster de failover falhou ao inicializar o adaptador de miniporta. O código de erro era ' %1 '. Verifique se outros adaptadores de rede estão funcionando corretamente e verifique se há erros no Gerenciador de dispositivos. Se a configuração tiver sido alterada, talvez seja necessário reinstalar o recurso de clustering de failover neste computador.

### <a name="event-4872-netft_missing_datalink_address"></a>Evento 4872: NETFT_MISSING_DATALINK_ADDRESS

O adaptador virtual do cluster de failover não pôde gerar um endereço MAC exclusivo.
Não foi possível encontrar um adaptador Ethernet físico do qual gerar um endereço exclusivo ou o endereço gerado está em conflito com outro adaptador neste computador. Execute o assistente para validar uma configuração para verificar sua configuração de rede.

### <a name="event-5122-dcm_event_root_creation_failed"></a>Evento 5122: DCM_EVENT_ROOT_CREATION_FAILED

Serviço de cluster falhou ao criar o diretório raiz de volumes compartilhados do cluster ' %2 '.
A mensagem de erro foi ' %1 '.

### <a name="event-5142-dcm_volume_no_access"></a>Evento 5142: DCM_VOLUME_NO_ACCESS

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') não está mais acessível deste nó de cluster devido ao erro ' %3 '. Solucione a conectividade deste nó com o dispositivo de armazenamento e a conectividade de rede.

### <a name="event-5143-dcm_veto_resource_move_due_to_cc"></a>Evento 5143: DCM_VETO_RESOURCE_MOVE_DUE_TO_CC

A movimentação do disco (' %2 ') é vetada com base no estado atual do Gerenciador de cache no nó ' %1 ' para evitar um deadlock em potencial. ' Páginas sujas do Gerenciador de cache possui ' é %3 e ' páginas sujas do Gerenciador de cache ' é %4. A movimentação é permitida se ' páginas sujas do Gerenciador de cache ' for inferior a 70% de ' páginas sujas do Gerenciador de cache possui ' ou se ' páginas sujas do Gerenciador de cache possui ' menos ' páginas sujas do Gerenciador de cache ' for maior que 128000 páginas (sobre 500 MB se um tamanho de página for de 4096 bytes).
A movimentação de recursos vetados de cluster para evitar um deadlock potencial devido à limitação do Gerenciador de cache de gravações em buffer enquanto volumes compartilhados do cluster nesse disco estão sendo pausados.

### <a name="event-5144-dcm_snapshot_diff_area_failure"></a>Evento 5144: DCM_SNAPSHOT_DIFF_AREA_FAILURE

Ao adicionar o disco (' %1 ') aos volumes compartilhados clusterizados, a configuração da Associação de área de comparação de instantâneo explícita para o volume (' %2 ') falhou com o erro ' %3 '. A única Associação de área de comparação de instantâneo de software com suporte para volumes compartilhados do cluster é a própria.

### <a name="event-5145-dcm_snapshot_diff_area_delete_failure"></a>Evento 5145: DCM_SNAPSHOT_DIFF_AREA_DELETE_FAILURE

O recurso de disco de cluster ' %1 ' não pôde excluir um instantâneo de software. Não foi possível dissociar a área de comparação no volume ' %3 ' do volume ' %2 '. Isso pode ser causado por instantâneos ativos. Os volumes compartilhados do cluster exigem que o instantâneo de software esteja localizado no mesmo disco.

### <a name="event-5146-dcm_veto_resource_move_due_to_dismount"></a>Evento 5146: DCM_VETO_RESOURCE_MOVE_DUE_TO_DISMOUNT

A movimentação do recurso de Volume Compartilhado Clusterizado ' %1 ' é vetada porque um dos volumes que pertencem ao recurso está no estado desmontado. Repita a ação depois que a operação de desmontagem for concluída.

### <a name="event-5147-dcm_veto_resource_move_due_to_snapshot"></a>Evento 5147: DCM_VETO_RESOURCE_MOVE_DUE_TO_SNAPSHOT

A movimentação do recurso de Volume Compartilhado Clusterizado ' %1 ' é vetada porque um dos volumes que pertencem ao recurso está no estado desmontado. Repita a ação depois que a operação de desmontagem for concluída.

### <a name="event-5148-dcm_veto_resource_move_due_to_io_mode_change"></a>Evento 5148: DCM_VETO_RESOURCE_MOVE_DUE_TO_IO_MODE_CHANGE

A movimentação do recurso de Volume Compartilhado Clusterizado ' %1 ' é vetada porque uma operação de alteração de modo de e/s (e/s direta para e/s redirecionada ou vice-versa) está em andamento em um dos volumes que pertencem ao recurso. Repita a ação depois que a operação for concluída.

### <a name="event-5150-dcm_set_resource_in_failed_state"></a>Evento 5150: DCM_SET_RESOURCE_IN_FAILED_STATE

O recurso de disco físico do cluster ' %1 ' falhou. O Volume Compartilhado Clusterizado foi colocado em estado de falha com o seguinte erro: ' %2 '

### <a name="event-5200-cam_cannot_create_cno_token"></a>Evento 5200: CAM_CANNOT_CREATE_CNO_TOKEN

Serviço de cluster falhou ao criar um token de identidade de cluster para volumes compartilhados clusterizados. O código de erro era ' %1 '. Verifique se o controlador de domínio está acessível e verifique se há problemas de conectividade. Até que a conexão com o controlador de domínio seja recuperada, algumas operações nesse nó em relação aos volumes compartilhados do cluster poderão falhar.

### <a name="event-5216-csv_sw_snapshot_failed"></a>Evento 5216: CSV_SW_SNAPSHOT_FAILED

A criação de instantâneo de software no Volume Compartilhado Clusterizado ' %1 ' (' %2 ') falhou com o erro %3. O recurso deve estar online para dar suporte à criação de instantâneo. Verifique o estado do recurso.

### <a name="event-5217-csv_sw_snapshot_set_failed"></a>Evento 5217: CSV_SW_SNAPSHOT_SET_FAILED

A criação de instantâneo de software em Volume Compartilhado Clusterizado (' %1 ') com a ID de conjunto de instantâneos ' %2 ' falhou com o erro ' %3 '. Verifique o estado dos recursos CSV e os eventos do sistema dos nós do proprietário do recurso.

### <a name="event-5219-csv_register_snapshot_prov_with_vss_failed"></a>Evento 5219: CSV_REGISTER_SNAPSHOT_PROV_WITH_VSS_FAILED

Serviço de cluster falhou ao registrar o provedor de instantâneo dos volumes compartilhados do cluster com o serviço de sombra de volume (VSS). Isso pode ser devido ao desligamento do serviço VSS ou pode ser um problema com o serviço VSS tendo um problema que faz com que ele não aceite solicitações de entrada. <br>Erro: %1

### <a name="event-5377-operation_exceeded_timeout"></a>Evento 5377: OPERATION_EXCEEDED_TIMEOUT

Uma operação interna de Serviço de cluster excedeu o limite definido de ' %2 ' segundos. A Serviço de cluster foi encerrada para recuperação. O Gerenciador de controle de serviço reiniciará o Serviço de cluster e o nó reingressará no cluster.

### <a name="event-5396-two_partitions_have_quorum"></a>Evento 5396: TWO_PARTITIONS_HAVE_QUORUM

O Serviço de cluster neste nó está sendo desligado porque detectou que há outros nós de cluster que têm quorum. Isso ocorre quando o Serviço de cluster detecta outro nó que foi iniciado com a opção de quorum forçado (/FQ). O nó que foi iniciado com a opção Forçar quorum permanecerá em execução. Use Gerenciador de Cluster de Failover para verificar se esse nó ingressou automaticamente no cluster quando o serviço de cluster for reiniciado.

### <a name="event-5397-rlua_account_failed"></a>Evento 5397: RLUA_ACCOUNT_FAILED

O recurso de cluster ' %1 ' não pôde criar ou modificar a conta de usuário local replicada ' %2 ' neste nó. Verifique os logs do cluster para obter mais informações.

### <a name="event-5398-nm_event_cluster_failed_to_form"></a>Evento 5398: NM_EVENT_CLUSTER_FAILED_TO_FORM

Falha ao iniciar o cluster. A cópia mais recente dos dados de configuração do cluster não estava disponível no conjunto de nós tentando iniciar o cluster. As alterações no cluster ocorreram enquanto o conjunto de nós não estava em associação e, como resultado, não era possível receber atualizações de dados de configuração. .<br><br>Votos necessários para iniciar o cluster: %1<br>Votos disponíveis: %2<br>Nós com votos: %3

#### <a name="guidance"></a>Diretrizes

Tente iniciar o serviço de cluster em todos os nós no cluster para que os nós com a cópia mais recente dos dados de configuração do cluster possam primeiro formar o cluster. O cluster será capaz de iniciar e os nós obterão automaticamente os dados de configuração de cluster atualizados. Se não houver nós disponíveis com a cópia mais recente dos dados de configuração do cluster, execute o cmdlet ' Start-ClusterNode-FQ ' do Windows PowerShell. O uso do parâmetro ForceQuorum (FQ) iniciará o serviço de cluster e marcará a cópia dos dados de configuração do cluster para ser autoritativa. Forçar o quorum em um nó com uma cópia desatualizada do banco de dados do cluster pode resultar em alterações de configuração do cluster que ocorreram enquanto o nó não estava participando do cluster para ser perdido.

## <a name="warning-events"></a>Eventos de aviso

### <a name="event-1011-nm_node_evicted"></a>Evento 1011: NM_NODE_EVICTED

O nó de cluster %1 foi removido do cluster de failover.

### <a name="event-1045-res_ipaddr_ipv4_address_create_failed"></a>Evento 1045: RES_IPADDR_IPV4_ADDRESS_CREATE_FAILED

Nenhuma interface de rede correspondente foi encontrada para o recurso ' %1 ' endereço IP ' %2 ' (o código de retorno era ' %3 '). Se os nós de cluster abrangerem sub-redes diferentes, isso pode ser normal.

### <a name="event-1066-res_disk_corrupt_disk"></a>Evento 1066: RES_DISK_CORRUPT_DISK

O recurso de disco de cluster ' %1 ' indica corrupção para o volume ' %2 '. Chkdsk está sendo executado para reparar problemas. O disco ficará indisponível até que chkdsk seja concluído.
A saída do chkdsk será registrada no arquivo ' %3 '.<br> O chkdsk também pode gravar informações no log de eventos do aplicativo.

### <a name="event-1068-res_smb_share_cant_add"></a>Evento 1068: RES_SMB_SHARE_CANT_ADD

O recurso de compartilhamento de arquivos de cluster ' %1 ' não pode ser colocado online. Falha na criação do compartilhamento de arquivos ' %2 ' (escopo para o nome de rede %3) devido ao erro ' %4 '. Esta operação será repetida automaticamente.

### <a name="event-1071-rcm_resource_online_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_ONLINE_BLOCKED_BY_LOCKED_MODE

A operação tentada no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 ' não pôde ser concluída porque o recurso ou um de seus provedores tem o status bloqueado.

### <a name="event-1071-rcm_resource_offline_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_OFFLINE_BLOCKED_BY_LOCKED_MODE

A operação tentada no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 ' não pôde ser concluída porque o recurso ou um de seus dependentes tem status bloqueado.

### <a name="event-1094-sm_invalid_security_level"></a>Evento 1094: SM_INVALID_SECURITY_LEVEL

A propriedade comum do cluster SecurityLevel não pode ser alterada neste cluster. O nível de segurança do cluster não pode ser alterado porque o cluster está atualmente configurado para nenhum modo de autenticação.

### <a name="event-1119-res_netname_dns_registration_missing"></a>Evento 1119: RES_NETNAME_DNS_REGISTRATION_MISSING

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome DNS ' %2 ' sobre o adaptador ' %4 ' pelo seguinte motivo: <br><br>' %3 '

### <a name="event-1125-tm_event_cluster_netinterface_unreachable"></a>Evento 1125: TM_EVENT_CLUSTER_NETINTERFACE_UNREACHABLE

A interface de rede de cluster ' %1 ' para o nó de cluster ' %2 ' na rede ' %3 ' está inacessível por pelo menos um outro nó de cluster anexado à rede. O cluster de failover não pôde determinar o local da falha. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados ao adaptador de rede. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1149-res_netname_cant_delete_dns_records"></a>Evento 1149: RES_NETNAME_CANT_DELETE_DNS_RECORDS

O host DNS (A) e os registros de ponteiro (PTR) associados ao recurso de cluster ' %1 ' não foram removidos do servidor DNS associado do recurso. Se necessário, eles podem ser excluídos manualmente. Contate o administrador do DNS para ajudar com esse esforço.

### <a name="event-1150-res_netname_dns_ptr_record_delete_failed"></a>Evento 1150: RES_NETNAME_DNS_PTR_RECORD_DELETE_FAILED

A remoção do registro de ponteiro de DNS (PTR) ' %2 ' para o host ' %3 ' que está associado ao recurso de nome de rede de cluster ' %1 ' falhou com o erro ' %4 '.
Se necessário, o registro pode ser excluído manualmente. Contate o administrador do DNS para obter assistência.

### <a name="event-1151-res_netname_dns_a_record_delete_failed"></a>Evento 1151: RES_NETNAME_DNS_A_RECORD_DELETE_FAILED

A remoção do registro de host DNS (A) ' %2 ' associado ao recurso de nome de rede de cluster ' %1 ' falhou com o erro ' %3 '. Se necessário, o registro pode ser excluído manualmente. Contate o administrador do DNS para obter assistência.

### <a name="event-1155-rcm_event_exited_queuing"></a>Evento 1155: RCM_EVENT_EXITED_QUEUING

A movimentação pendente para a função ' %1 ' não foi concluída.

### <a name="event-1197-res_netname_delete_disable_failed"></a>Evento 1197: RES_NETNAME_DELETE_DISABLE_FAILED

O recurso de nome de rede do cluster ' %1 ' não pôde excluir ou desabilitar seu objeto de computador associado ' %2 ' durante a exclusão do recurso. O código de erro era ' %3 '. <br>Verifique se o site é somente leitura. Verifique se o objeto de nome do cluster tem as permissões apropriadas no objeto ' %2 ' no Active Directory.

### <a name="event-1198-res_netname_delete_vco_guid_failed"></a>Evento 1198: RES_NETNAME_DELETE_VCO_GUID_FAILED

O recurso de nome de rede de cluster ' %1 ' não pôde excluir o objeto de computador com o GUID ' %2 '. O código de erro era ' %3 '. <br>Verifique se o site é somente leitura. Verifique se o objeto de nome do cluster tem as permissões apropriadas no objeto ' %2 ' no Active Directory.

### <a name="event-1216-service_netname_change_warning"></a>Evento 1216: SERVICE_NETNAME_CHANGE_WARNING

Falha em uma operação de alteração de nome no recurso de NetName do núcleo do cluster.
A tentativa de reverter a operação de alteração de nome de volta para o nome original também falhou. O código de erro era ' %1 '. Talvez você não consiga gerenciar remotamente o cluster usando o nome do cluster até que essa situação tenha sido corrigida manualmente.

### <a name="event-1221-res_netname_rename_out_of_synch_with_compobj"></a>Evento 1221: RES_NETNAME_RENAME_OUT_OF_SYNCH_WITH_COMPOBJ

O recurso de nome de rede de cluster ' %1 ' tem um nome ' %2 ' que não corresponde ao nome de objeto de computador correspondente ' %3 '. É provável que uma alteração de nome anterior do objeto de computador não tenha sido replicada para todos os controladores de domínio no domínio. Você não poderá renomear o recurso de nome de rede até que os nomes se tornem consistentes. Se o objeto de computador não tiver sido alterado recentemente, entre em contato com o administrador de domínio para renomear o objeto de computador e, portanto, torná-lo consistente. Além disso, verifique se a replicação entre os controladores de domínio foi concluída com êxito.

### <a name="event-1222-res_netname_set_permissions_failed"></a>Evento 1222: RES_NETNAME_SET_PERMISSIONS_FAILED

O objeto de computador associado ao recurso de nome de rede de cluster ' %1 ' não pôde ser atualizado.<br><br>O texto do código de erro associado é: %2<br> <br>A identidade do cluster ' %3 ' pode não ter permissões necessárias para atualizar o objeto. Trabalhe com o administrador de domínio para garantir que a identidade do cluster possa atualizar objetos de computador no domínio.

### <a name="event-1240-res_ipaddr_obtain_lease_failed"></a>Evento 1240: RES_IPADDR_OBTAIN_LEASE_FAILED

O recurso de endereço IP do cluster ' %1 ' não pôde obter um endereço IP concedido.

### <a name="event-1243-res_ipaddr_release_lease_failed"></a>Evento 1243: RES_IPADDR_RELEASE_LEASE_FAILED

O recurso de endereço IP do cluster ' %1 ' não pôde liberar a concessão para o endereço IP ' %2 '.

### <a name="event-1251-rcm_group_preempted"></a>Evento 1251: RCM_GROUP_PREEMPTED

A função clusterizada ' %2 ' foi obtida offline. Esta função foi antecipada pela função clusterizada de prioridade mais alta ' %1 '. O serviço de cluster tentará colocar a função clusterizada ' %2 ' online posteriormente quando os recursos do sistema estiverem disponíveis.

### <a name="event-1544-service_vss_onabort"></a>Evento 1544: SERVICE_VSS_ONABORT

A operação de backup dos dados de configuração do cluster foi cancelada. O gravador VSS (cluster Serviço de Cópias de Sombra de Volume) recebeu uma solicitação de anulação.

### <a name="event-1548-service_connect_version_compatible"></a>Evento 1548: SERVICE_CONNECT_VERSION_COMPATIBLE

O nó ' %1 ' estabeleceu uma comunicação com o nó ' %2 ' e detectou que está executando uma versão diferente, mas compatível, do sistema operacional. Recomendamos que todos os nós executem a mesma versão do sistema operacional. Depois que todos os nós tiverem sido atualizados, execute o cmdlet Update-ClusterFunctionalLevel do Windows PowerShell para concluir a atualização do cluster.

### <a name="event-1550-service_connect_novercheck"></a>Evento 1550: SERVICE_CONNECT_NOVERCHECK

O nó ' %1 ' estabeleceu uma sessão de comunicação com o nó ' %2 ' sem executar uma verificação de compatibilidade de versão porque a verificação de compatibilidade de versão está desabilitada. Não há suporte para a desabilitação da verificação de compatibilidade de versão.

### <a name="event-1551-service_form_novercheck"></a>Evento 1551: SERVICE_FORM_NOVERCHECK

O nó ' %1 ' formou um cluster de failover sem executar uma verificação de compatibilidade de versão porque a verificação de compatibilidade de versão está desabilitada. Não há suporte para a desabilitação da verificação de compatibilidade de versão.

### <a name="event-1555-service_netft_disable_autoconfig_failed"></a>Evento 1555: SERVICE_NETFT_DISABLE_AUTOCONFIG_FAILED

Falha ao tentar usar o IPv4 para o adaptador de rede ' %1 '. Isso ocorreu devido a uma falha ao desabilitar a configuração automática de IPv4 e o DHCP. Isso pode ser esperado se o serviço cliente DHCP já estiver desabilitado. O IPv6 será usado se estiver habilitado, caso contrário, esse nó poderá não ser capaz de participar de um cluster de failover.

### <a name="event-1558-service_witness_failover_attempt"></a>Evento 1558: SERVICE_WITNESS_FAILOVER_ATTEMPT

O serviço de cluster detectou um problema com o recurso de testemunha. O recurso de testemunha passará por failover para outro nó dentro do cluster, em uma tentativa de restabelecer o acesso aos dados de configuração do cluster.

### <a name="event-1562-res_fsw_alivefailure"></a>Evento 1562: RES_FSW_ALIVEFAILURE

O recurso de testemunha de compartilhamento de arquivos ' %1 ' falhou em uma verificação de integridade periódica no compartilhamento de arquivos ' %2 '. Verifique se o compartilhamento de arquivos ' %2 ' existe e se está acessível pelo cluster.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 ' em uma zona DNS segura. Isso ocorreu devido a um registro existente com esse nome, e a identidade do cluster não tem privilégios suficientes para atualizar esse registro. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster tem permissões no registro DNS ' %2 '.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 ' em uma zona DNS segura. Isso ocorreu devido a um registro existente com esse nome, e a identidade do cluster não tem privilégios suficientes para atualizar esse registro. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster tem permissões no registro DNS ' %2 '.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 '. Não foi possível contatar o servidor DNS. O código de erro era ' %3. ' Verifique se um servidor DNS pode ser acessado deste nó de cluster. O registro de DNS será repetido mais tarde.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar o nome ' %2 ' sobre o adaptador ' %4 '. Não foi possível contatar o servidor DNS. O código de erro era ' %3. ' Verifique se um servidor DNS pode ser acessado deste nó de cluster. O registro de DNS será repetido mais tarde.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar atualizações dinâmicas para o nome ' %2 ' sobre o adaptador ' %4 '. O servidor DNS pode não estar configurado para aceitar atualizações dinâmicas. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se o servidor DNS está disponível e configurado para atualizações dinâmicas.<br><br>Como alternativa, você pode desabilitar as atualizações dinâmicas de DNS desmarcando a configuração ' registrar os endereços desta conexão no DNS ' nas configurações avançadas de TCP/IP para o adaptador ' %4 ' na guia DNS.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

O recurso de nome de rede de cluster ' %1 ' falhou ao registrar atualizações dinâmicas para o nome ' %2 ' sobre o adaptador ' %4 '. O servidor DNS pode não estar configurado para aceitar atualizações dinâmicas. O código de erro era ' %3 '. Entre em contato com o administrador do servidor DNS para verificar se o servidor DNS está disponível e configurado para atualizações dinâmicas.<br><br>Como alternativa, você pode desabilitar as atualizações dinâmicas de DNS desmarcando a configuração ' registrar os endereços desta conexão no DNS ' nas configurações avançadas de TCP/IP para o adaptador ' %4 ' na guia DNS.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

O recurso de nome de rede de cluster ' %1 ' não pôde atualizar o registro DNS para o nome ' %2 ' sobre o adaptador ' %4 '. O código de erro era ' %3 '. Verifique se um servidor DNS está acessível deste nó de cluster e entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster pode atualizar o registro DNS ' %2 '.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

O recurso de nome de rede de cluster ' %1 ' não pôde atualizar o registro DNS para o nome ' %2 ' sobre o adaptador ' %4 '. O código de erro era ' %3 '. Verifique se um servidor DNS está acessível deste nó de cluster e entre em contato com o administrador do servidor DNS para verificar se a identidade do cluster pode atualizar o registro DNS ' %2 '.

### <a name="event-1581-clussvc_unable_to_move_hive_to_safe_file"></a>Evento 1581: CLUSSVC_UNABLE_TO_MOVE_HIVE_TO_SAFE_FILE

A solicitação de restauração dos dados de configuração do cluster não pôde fazer uma cópia do arquivo de dados de configuração de cluster existente (ClusDB). Ao tentar preservar a configuração existente, a operação de restauração não pôde criar uma cópia no local ' %1 '. Isso poderá ser esperado se o arquivo de dados de configuração existente estiver corrompido. A operação de restauração continuou, mas as tentativas de reverter para a configuração de cluster existente podem não ser possíveis.

### <a name="event-1582-clussvc_unable_to_move_restored_hive_to_current"></a>Evento 1582: CLUSSVC_UNABLE_TO_MOVE_RESTORED_HIVE_TO_CURRENT

Falha do serviço de cluster ao mover o hive do cluster restaurado em ' %1 ' para ' %2 '. Isso pode impedir que a operação de restauração seja bem-sucedida com êxito. Se a configuração de cluster não tiver sido restaurada corretamente, repita a ação de restauração.

### <a name="event-1583-clussvc_netft_disable_connectionsecurity_failed"></a>Evento 1583: CLUSSVC_NETFT_DISABLE_CONNECTIONSECURITY_FAILED

Serviço de cluster falhou ao desabilitar o protocolo IPsec no adaptador virtual do cluster de failover ' %1 '. Isso pode ter um impacto negativo no desempenho de comunicação do cluster. Se esse problema persistir, verifique suas políticas de segurança de conexão local e de domínio aplicando-se ao IPSec e ao firewall do Windows. Além disso, verifique se há eventos relacionados ao serviço do mecanismo de filtragem base.

### <a name="event-1584-shared_volume_not_ready_for_snapshot"></a>Evento 1584: SHARED_VOLUME_NOT_READY_FOR_SNAPSHOT

Um aplicativo de backup iniciou um instantâneo VSS no Volume Compartilhado Clusterizado ' %1 ' (' %3 ') sem preparar corretamente o volume para o instantâneo. Esse instantâneo pode ser inválido e o backup pode não ser utilizável para operações de restauração. Entre em contato com o fornecedor do aplicativo de backup para verificar a compatibilidade com volumes compartilhados do cluster.

### <a name="event-1589-res_netname_dns_returning_ip_that_is_not_provider"></a>Evento 1589: RES_NETNAME_DNS_RETURNING_IP_THAT_IS_NOT_PROVIDER

O recurso de nome de rede de cluster ' %1 ' encontrou um ou mais endereços IP associados ao nome DNS ' %2 ' que não são recursos de endereço IP dependentes. Os endereços adicionais encontrados foram ' %3 '. Isso pode afetar a conectividade do cliente até que o nome da rede e seus registros DNS associados sejam consistentes. Entre em contato com o administrador do servidor DNS para verificar os registros associados ao nome ' %2 '.

### <a name="event-1604-res_disk_chkdsk_spotfix_needed"></a>Evento 1604: RES_DISK_CHKDSK_SPOTFIX_NEEDED

O recurso de disco de cluster ' %1 ' detectou corrupção para o volume ' %2 '. Spotfix chkdsk é necessário para reparar problemas.

### <a name="event-1605-res_disk_spotfix_performed"></a>Evento 1605: RES_DISK_SPOTFIX_PERFORMED

O recurso de disco de cluster ' %1 ' concluiu a execução de ChkDsk.exe/spotfix no volume ' %2 '.
O código de retorno era ' %4 '. A saída do ChkDsk foi registrada no arquivo ' %3 '.<br>
Verifique o log de eventos do aplicativo para obter informações adicionais do ChkDsk.

### <a name="event-1671-res_disk_online_set_attributes_completed_failure"></a>Evento 1671: RES_DISK_ONLINE_SET_ATTRIBUTES_COMPLETED_FAILURE

O recurso de disco físico do cluster não pode ser colocado online.<br><br>Nome do recurso do disco físico: %1<br>Código de erro: %2<br>Tempo decorrido (segundos): %3

#### <a name="guidance"></a>Diretrizes

Execute o assistente para validar uma configuração para verificar sua configuração de armazenamento. Se o código de erro foi ERROR_CLUSTER_SHUTDOWN, o estado online pendente foi cancelado por um administrador. Se esse for um volume replicado, isso pode ser o resultado de uma falha ao definir os atributos do disco. Examine os eventos de replicação de armazenamento para obter informações adicionais.

### <a name="event-1673-cluster_node_entered_isolated_state"></a>Evento 1673: CLUSTER_NODE_ENTERED_ISOLATED_STATE

O nó de cluster ' %1 ' entrou no estado isolado.

### <a name="event-1675-event_joining_node_quarantined"></a>Evento 1675: EVENT_JOINING_NODE_QUARANTINED

O nó de cluster ' %1 ' foi colocado em quarentena por ' %2 ' e não pode ingressar no cluster. O nó será colocado em quarentena até ' %3 ' e, em seguida, o nó tentará novamente ingressar no cluster automaticamente. <br><br>Consulte os logs de eventos do sistema e do aplicativo para determinar os problemas neste nó. Quando o problema for resolvido, a quarentena poderá ser limpa manualmente para permitir que o nó ingresse novamente com o cmdlet ' Start-ClusterNode – ClearQuarantine ' do Windows PowerShell.<br><br>Nome do nó: %1<br>QuarantineType: quarentena por %2<br>A quarentena de tempo será automaticamente desmarcada: %3

### <a name="event-1681-cluster_groups_unmonitored_on_node"></a>Evento 1681: CLUSTER_GROUPS_UNMONITORED_ON_NODE

As máquinas virtuais no nó ' %1 ' entraram em um estado não monitorado. A integridade das máquinas virtuais será monitorada novamente quando o nó retornar de um estado isolado ou puder fazer failover se o nó não retornar. A máquina virtual que não está mais sendo monitorada é: %2.

### <a name="event-1689-event_disable_and_stop_other_service"></a>Evento 1689: EVENT_DISABLE_AND_STOP_OTHER_SERVICE

O serviço de cluster detectou um serviço que não é compatível com o clustering de failover. O serviço foi desabilitado para evitar possíveis problemas com o cluster de failover.<br><br>Nome do serviço: ' %1 '

### <a name="event-4625-nodecleanup_reset_nlbsflags_preserved"></a>Evento 4625: NODECLEANUP_RESET_NLBSFLAGS_PRESERVED

Falha ao redefinir o valor do registro de tempo limite da Associação de segurança IPSec durante a limpeza do nó do cluster. Isso ocorre porque o tempo limite da Associação de segurança IPSec foi modificado depois que esse computador foi configurado para ser membro de um cluster. Para a limpeza manual, execute o cmdlet ' Clear-ClusterNode ' do PowerShell neste computador. Como alternativa, você pode redefinir o tempo limite da Associação de segurança IPSec excluindo o valor ' %1 ' e o valor ' %2 ' de HKEY_LOCAL_MACHINE no registro do Windows.

### <a name="event-4873-netft_clussvc_terminate_because_of_watchdog"></a>Evento 4873: NETFT_CLUSSVC_TERMINATE_BECAUSE_OF_WATCHDOG

O serviço de cluster detectou que o adaptador virtual de cluster de failover foi interrompido. Isso é esperado quando a CPU de substituição ativa é executada neste nó.
Serviço de cluster será interrompido e deverá ser reiniciado automaticamente depois que a operação for concluída. Verifique se há eventos adicionais associados ao serviço e certifique-se de que o serviço de cluster foi reiniciado neste nó.

### <a name="event-5120-dcm_volume_auto_pause_after_failure"></a>Evento 5120: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') entrou em um estado de pausa por causa de ' %3 '.
Toda e/s será colocada na fila temporariamente até que um caminho para o volume seja restabelecido.

### <a name="event-5123-dcm_event_root_rename_success"></a>Evento 5123: DCM_EVENT_ROOT_RENAME_SUCCESS

O diretório raiz de volumes compartilhados do cluster ' %1 ' já existe. O diretório ' %1 ' foi renomeado para ' %2 '. Verifique se os aplicativos que estão acessando os dados neste local foram atualizados conforme necessário.

### <a name="event-5124-dcm_unsafe_filters_found"></a>Evento 5124: DCM_UNSAFE_FILTERS_FOUND

Volume Compartilhado Clusterizado ' %1 ' (' %3 ') identificou um ou mais drivers de filtro ativos nesta pilha de dispositivos que poderiam interferir nas operações de CSV. O acesso de e/s será redirecionado para o dispositivo de armazenamento pela rede por meio de outro nó de cluster. Isso pode resultar em degradação do desempenho. Entre em contato com o fornecedor do driver de filtro para verificar a interoperabilidade com volumes compartilhados do cluster. <br><br>Drivers de filtro ativos encontrados:<br>%2

### <a name="event-5125-dcm_unsafe_volfilter_found"></a>Evento 5125: DCM_UNSAFE_VOLFILTER_FOUND

Volume Compartilhado Clusterizado ' %1 ' (' %3 ') identificou um ou mais drivers de volume ativos nesta pilha de dispositivos que poderiam interferir nas operações CSV. O acesso de e/s será redirecionado para o dispositivo de armazenamento pela rede por meio de outro nó de cluster. Isso pode resultar em degradação do desempenho. Entre em contato com o fornecedor do driver de volume para verificar a interoperabilidade com volumes compartilhados do cluster. <br><br>Drivers de volume ativos encontrados:<br>%2

### <a name="event-5126-dcm_event_cannot_disable_short_names"></a>Evento 5126: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

O recurso de disco físico ' %1 ' não permite desabilitar a geração de nome curto. Isso pode causar problemas de compatibilidade de aplicativos. Use ' fsutil 8dot3name set 2 ' para permitir a desabilitação da geração de nome curto e, em seguida, coloque offline/online o recurso.

### <a name="event-5128-dcm_event_cannot_disable_short_names"></a>Evento 5128: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

O recurso de disco físico ' %1 ' não permite desabilitar a geração de nome curto. Isso pode causar problemas de compatibilidade de aplicativos. Use ' fsutil 8dot3name set 2 ' para permitir a desabilitação da geração de nome curto e, em seguida, coloque offline/online o recurso.

### <a name="event-5133-dcm_cannot_restore_drive_letters"></a>Evento 5133: DCM_CANNOT_RESTORE_DRIVE_LETTERS

O disco de cluster ' %1 ' foi removido e colocado de volta no grupo de clusters ' armazenamento disponível '. Durante esse processo, uma tentativa de restaurar as letras da unidade original levou mais tempo do que o esperado, possivelmente porque as letras da unidade já estão em uso.

### <a name="event-5134-dcm_cannot_set_acl_on_root"></a>Evento 5134: DCM_CANNOT_SET_ACL_ON_ROOT

Serviço de cluster falhou ao definir as permissões (ACL) no diretório raiz de volumes compartilhados do cluster ' %1 '. O erro foi ' %2 '.

### <a name="event-5135-dcm_cannot_set_acl_on_volume_folder"></a>Evento 5135: DCM_CANNOT_SET_ACL_ON_VOLUME_FOLDER

Serviço de cluster falhou ao definir as permissões no diretório de Volume Compartilhado Clusterizado ' %1 ' (' %2 '). O erro foi ' %3 '.

### <a name="event-5136-dcm_csv_into_redirected_mode"></a>Evento 5136: DCM_CSV_INTO_REDIRECTED_MODE

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') acesso Redirecionado foi ativado. O acesso ao dispositivo de armazenamento será redirecionado pela rede de todos os nós de cluster que estão acessando esse volume. Isso pode resultar em degradação do desempenho. Desligue o acesso Redirecionado para este volume para retomar as operações normais.

### <a name="event-5149-dcm_csv_block_cache_resized"></a>Evento 5149: DCM_CSV_BLOCK_CACHE_RESIZED

O tamanho do cache foi redimensionado para ' %1 ' com base na memória preenchida; o SetSize do usuário é muito alto.

### <a name="event-5156-dcm_volume_auto_pause_after_snapshot_failure"></a>Evento 5156: DCM_VOLUME_AUTO_PAUSE_AFTER_SNAPSHOT_FAILURE

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') entrou em um estado de pausa por causa de ' %3 '.
Esse erro é encontrado quando os instantâneos de VolSnap subjacentes ao volume CSV são excluídos fora de uma solicitação de usuário. As possíveis causas dos instantâneos serem excluídos são a falta de espaço no volume para que os instantâneos cresçam ou a falha de e/s ao tentar atualizar os dados do instantâneo. Toda e/s será colocada na fila temporariamente até que o estado do instantâneo seja sincronizado com o Volsnap.

### <a name="event-5157-dcm_volume_auto_pause_after_failure_expected"></a>Evento 5157: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE_EXPECTED

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') entrou em um estado de pausa por causa de ' %3 '.
Toda e/s será colocada na fila temporariamente até que um caminho para o volume seja restabelecido.
Esse erro é geralmente causado por uma falha de infraestrutura. Por exemplo, perder a conectividade com o armazenamento ou o nó que possui a Volume Compartilhado Clusterizado que está sendo removida da Associação de cluster ativa.

### <a name="event-5394-pool_online_warnings"></a>Evento 5394: POOL_ONLINE_WARNINGS

O Serviço de cluster encontrou alguns erros de armazenamento ao tentar colocar o pool de armazenamento online. Execute a validação de armazenamento de cluster para solucionar o problema.

### <a name="event-5395-rcm_group_auto_move_storage_pool"></a>Evento 5395: RCM_GROUP_AUTO_MOVE_STORAGE_POOL

O cluster está movendo o grupo para o pool de armazenamento ' %1 ' porque o nó atual ' %3 ' não tem conectividade ideal com os discos físicos do pool de armazenamento.

## <a name="informational-events"></a>Eventos informativos

### <a name="event-1592-clussvc_tcp_reconnect_connection_reestablished"></a>Evento 1592: CLUSSVC_TCP_RECONNECT_CONNECTION_REESTABLISHED

O nó de cluster ' %1 ' perdeu a comunicação com o nó de cluster ' %2 '. A comunicação de rede foi restabelecida. Isso pode ser devido à comunicação temporariamente bloqueada por uma atualização de firewall ou de política de segurança de conexão. Se o problema persistir e a comunicação de rede não for restabelecida, o serviço de cluster em um ou mais nós será interrompido. Se isso acontecer, execute o assistente para validar uma configuração para verificar sua configuração de rede. Além disso, verifique se há erros de hardware ou software relacionados aos adaptadores de rede neste nó e verifique se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

### <a name="event-1594-cluster_functional_level_upgrade_complete"></a>Evento 1594: CLUSTER_FUNCTIONAL_LEVEL_UPGRADE_COMPLETE

Serviço de cluster atualizou com êxito o nível funcional do cluster. <br><br>
Nível funcional: %1 <br> Atualizar versão: %2

### <a name="event-1635-rcm_resource_failure_info_with_typename"></a>Evento 1635: RCM_RESOURCE_FAILURE_INFO_WITH_TYPENAME

Falha no recurso de cluster ' %1 ' do tipo ' %3 ' na função clusterizada ' %2 '.

### <a name="event-1636-clussvc_password_changed"></a>Evento 1636: CLUSSVC_PASSWORD_CHANGED

O Serviço de cluster alterou a senha da conta ' %1 ' no nó ' %2 '.

### <a name="event-1680-cluster_node_exited_isolated_state"></a>Evento 1680: CLUSTER_NODE_EXITED_ISOLATED_STATE

O nó de cluster ' %1 ' saiu do estado isolado.

### <a name="event-1682-cluster_group_moved_no_longer_unmonitored"></a>Evento 1682: CLUSTER_GROUP_MOVED_NO_LONGER_UNMONITORED

A máquina virtual ' %2 ', que não foi monitorada no nó isolado ' %1 ', passou por failover para outro nó. A integridade da máquina virtual está sendo monitorada novamente.

### <a name="event-1682-cluster_multiple_groups_no_longer_unmonitored"></a>Evento 1682: CLUSTER_MULTIPLE_GROUPS_NO_LONGER_UNMONITORED

O nó ' %1 ' reingressou no cluster e as seguintes máquinas virtuais em execução nesse nó estão novamente com seu estado de integridade monitorado: %2.

### <a name="event-4621-nodecleanup_success"></a>Evento 4621: NODECLEANUP_SUCCESS

Este nó foi removido com êxito do cluster.

### <a name="event-5121-dcm_volume_no_direct_io_due_to_failure"></a>Evento 5121: DCM_VOLUME_NO_DIRECT_IO_DUE_TO_FAILURE

Volume Compartilhado Clusterizado ' %1 ' (' %2 ') não está mais acessível diretamente deste nó de cluster. O acesso de e/s será redirecionado para o dispositivo de armazenamento pela rede para o nó que possui o volume. Se isso resultar em degradação de desempenho, solucione a conectividade desse nó com o dispositivo de armazenamento e a e/s continuará para um estado íntegro quando a conectividade com o dispositivo de armazenamento for restabelecida.

### <a name="event-5218-csv_old_sw_snapshot_deleted"></a>Evento 5218: CSV_OLD_SW_SNAPSHOT_DELETED

O recurso de disco físico do cluster ' %1 ' excluiu um instantâneo de software. O instantâneo de software na Volume Compartilhado Clusterizado ' %2 ' foi excluído porque era anterior a ' %3 ' dia (s). A ID do instantâneo foi ' %4 ' e foi criada a partir do nó ' %5 ' em ' %6 '.
Espera-se que os instantâneos sejam excluídos por um aplicativo de backup após a conclusão de um trabalho de backup. Esse instantâneo excedeu o tempo esperado para que um instantâneo exista. Verifique com o aplicativo de backup se os trabalhos de backup estão sendo concluídos com êxito.

## <a name="additional-references"></a>Referências adicionais

-   [Informações detalhadas sobre o evento para componentes de clustering de failover no Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753362(v%3dws.10))
