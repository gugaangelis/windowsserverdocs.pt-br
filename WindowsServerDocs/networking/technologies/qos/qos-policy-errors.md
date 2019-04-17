---
title: Erro de política de QoS e mensagens de evento
description: Este tópico fornece uma lista de mensagens de erro e eventos para política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>Erro de política de QoS e mensagens de evento

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

A seguir está as mensagens de erro e eventos que são associadas a política de QoS.  
  
## <a name="informational-messages"></a>Mensagens informativas  

Esta é uma lista de mensagens informativas QoS política.

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de computador atualizadas com êxito. Nenhuma alteração foi detectada.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de computador atualizadas com êxito. Alterações na política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS atualizadas com êxito. Nenhuma alteração foi detectada.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS atualizadas com êxito. Alterações na política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP entrada atualizadas com êxito. Definir o valor não for especificado por qualquer política de QoS. Padrão do computador local será aplicado.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP entrada atualizadas com êxito. Definir o valor é o nível 0 (taxa de transferência mínima).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP entrada atualizadas com êxito. Valor da configuração é nível 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP entrada atualizadas com êxito. Definir o valor é o nível 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP entrada atualizadas com êxito. Valor da configuração é nível 3 (taxa de transferência máxima).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para marcação DSCP substitui atualizadas com êxito. Definir o valor não for especificado. Aplicativos podem definir valores DSCP independentemente de diretivas QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para marcação DSCP substitui atualizadas com êxito. Solicitações de marcação de DSCP do aplicativo serão ignoradas. Apenas as políticas de QoS podem definir valores DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para marcação DSCP substitui atualizadas com êxito. Aplicativos podem definir valores DSCP independentemente de diretivas QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|Informativa|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Idioma**|Inglês|  
|**Mensagem**|Aplicativo seletivo das políticas de QoS com base na categoria de rede do domínio foi desabilitado. Políticas de QoS serão aplicadas a todas as interfaces de rede.|  
  
## <a name="warning-messages"></a>Mensagens de aviso

Esta é uma lista de mensagens de aviso de política de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * Testing\ * \ * \ * [, com uma cadeia de caracteres] "%2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * Testing\ * \ * \ * [, com duas cadeias de caracteres string1 é] "%2" [, string2 for] "%3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|O computador de política de QoS "%2" tem um número de versão inválido. Esta configuração de política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário diretiva QoS "%2" tem um número de versão inválido. Esta configuração de política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|Computador de política de QoS "%2" não especifica uma taxa de aceleração ou um valor DSCP. Esta configuração de política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário diretiva QoS "%2" não especificar uma taxa de aceleração ou um valor DSCP. Esta configuração de política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|Excedido o número máximo de diretivas QoS de computador. A política de QoS "%2" e as políticas de QoS subsequentes do computador não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|Excedido o número máximo de diretivas QoS de usuário. A diretiva QoS "%2" e as diretivas de QoS de usuário subsequentes não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|Computador de política de QoS "%2" potencialmente entra em conflito com outras políticas de QoS. Consulte a documentação para as regras sobre quais política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário diretiva QoS "%2" potencialmente entra em conflito com outras políticas de QoS. Consulte a documentação para as regras sobre quais política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|Computador de política de QoS "%2" foi ignorado porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválido, contêm uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário diretiva QoS "%2" foi ignorado porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválido, contêm uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
## <a name="error-messages"></a>Mensagens de erro  

Esta é uma lista das mensagens de erro de política de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS computador falha ao atualizar. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS de usuário Falha ao atualizar. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a chave raiz de nível de máquina para diretivas de QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a chave raiz de nível de usuário para diretivas de QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Um computador de política de QoS excede o comprimento máximo do nome permitido. A diretiva incorreta está listada na chave raiz da diretiva QoS de computador com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Um diretiva QoS de usuário excede o comprimento máximo do nome permitido. A diretiva incorreta está listada na chave raiz da diretiva QoS de usuário com índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Um computador de política de QoS tem um nome de comprimento zero. A diretiva incorreta está listada na chave raiz da diretiva QoS de computador com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Um usuário QoS política tem um nome de comprimento zero. A diretiva incorreta está listada na chave raiz da diretiva QoS de usuário com índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a subchave do registro para um computador de política de QoS. A diretiva está listada na chave raiz da diretiva QoS de computador com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a subchave do registro para um usuário QoS política. A diretiva está listada na chave raiz da diretiva QoS de usuário com índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao ler ou validar o campo "%2" para o computador de política de QoS "%3".|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao ler ou validar o campo "%2" para o usuário QoS política "%3".|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao ler ou definir entrada TCP throughput nível, código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao ler ou definir a marcação DSCP substituir a configuração, código de erro: "%2".|  

Para o próximo tópico neste guia, consulte [QoS política perguntas frequentes](qos-policy-faq.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
