---
title: Erro de política de QoS e mensagens de evento
description: Este tópico fornece uma lista de mensagens de erro e evento para a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824047"
---
# <a name="qos-policy-error-and-event-messages"></a>Erro de política de QoS e mensagens de evento

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A seguir estão as mensagens de erro e de eventos que estão associadas com a política de QoS.  
  
## <a name="informational-messages"></a>Mensagens informativas  

A seguir está uma lista de mensagens informativas de política de QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de computador atualizadas com êxito. Nenhuma alteração detectada.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de computador atualizadas com êxito. Alterações de política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de usuário atualizadas com êxito. Nenhuma alteração detectada.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Diretivas de QoS de usuário atualizadas com êxito. Alterações de política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para o nível de taxa de transferência de entrada TCP foi atualizado com êxito. Definir o valor não for especificado por uma política de QoS. Padrão do computador local será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para o nível de taxa de transferência de entrada TCP foi atualizado com êxito. Valor da configuração é nível 0 (taxa de transferência mínima).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para o nível de taxa de transferência de entrada TCP foi atualizado com êxito. Valor da configuração é o nível 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para o nível de taxa de transferência de entrada TCP foi atualizado com êxito. Valor da configuração é o nível 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para o nível de taxa de transferência de entrada TCP foi atualizado com êxito. Valor da configuração é o nível 3 (taxa de transferência máxima).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para a marcação de DSCP substitui atualizadas com êxito. Definir o valor não for especificado. Aplicativos podem definir valores DSCP independentemente das políticas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para a marcação de DSCP substitui atualizadas com êxito. As solicitações de marcação de DSCP do aplicativo serão ignoradas. Somente as políticas de QoS podem definir valores DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Idioma**|Inglês|  
|**Mensagem**|Configuração de QoS avançada para a marcação de DSCP substitui atualizadas com êxito. Aplicativos podem definir valores DSCP independentemente das políticas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severidade**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Idioma**|Inglês|  
|**Mensagem**|Aplicativo seletivo das políticas de QoS com base na categoria de rede de domínio foi desabilitado. As políticas de QoS serão aplicadas a todas as interfaces de rede.|  
  
## <a name="warning-messages"></a>Mensagens de Aviso

A seguir está uma lista de mensagens de aviso da política de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * Testando\*\*\*[, com uma cadeia de caracteres] "%2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * Testando\*\*\*[, com duas cadeias de caracteres, string1 é] "%2" [, string2 é] "%3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|O computador a política de QoS "%2" tem um número de versão inválido. Essa política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário a política de QoS "%2" tem um número de versão inválido. Essa política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|O computador a política de QoS "%2" não especifica uma taxa de limitação ou de valor DSCP. Essa política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS "%2" do usuário não especifica uma taxa de limitação ou de valor DSCP. Essa política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|Excedido o número máximo de políticas de QoS do computador. A política de QoS "%2" e as políticas de QoS subsequentes de computadores não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|Excedido o número máximo de políticas de QoS do usuário. A política de QoS "%2" e as políticas de QoS de usuário subsequentes não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS "%2" do computador pode apresentar conflito com outras políticas de QoS. Consulte a documentação para as regras sobre os quais a política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário "%2" política de QoS pode apresentar conflito com outras políticas de QoS. Consulte a documentação para as regras sobre os quais a política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS "%2" do computador foi ignorada porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválida, conter uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severidade**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|O usuário a política de QoS "%2" foi ignorado porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválida, conter uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
## <a name="error-messages"></a>Mensagens de erro  

A seguir está uma lista das mensagens de erro de política de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|As políticas de QoS do computador falha ao atualizar. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|As políticas de QoS do usuário Falha ao atualizar. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao abrir a chave de raiz de nível de máquina para políticas de QoS do QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao abrir a chave de raiz de nível de usuário para políticas de QoS do QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma diretiva de QoS de computador excede o comprimento máximo permitido do nome. A diretiva incorreta é listada na chave raiz da diretiva QoS de máquina com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Um política de QoS de usuário excede o comprimento máximo permitido do nome. A diretiva incorreta é listada na chave raiz da diretiva QoS de usuário com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma diretiva de QoS de computador tem um nome de comprimento zero. A diretiva incorreta é listada na chave raiz da diretiva QoS de máquina com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Um política de QoS de usuário tem um nome de comprimento zero. A diretiva incorreta é listada na chave raiz da diretiva QoS de usuário com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a subchave do registro para um computador política de QoS. A diretiva é listada na chave raiz da diretiva QoS de máquina com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS Falha ao abrir a subchave do registro para um usuário de política de QoS. A diretiva é listada na chave raiz da diretiva QoS de usuário com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao ler ou validar o campo "%2" para o computador "%3" política de QoS do QoS.|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao ler ou validar o campo "%2" para o usuário "%3" política de QoS do QoS.|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Idioma**|Inglês|  
|**Mensagem**|QoS não conseguiu ler ou definir a entrada TCP taxa de transferência nível, código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severidade**|Erro|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao ler ou definir a marcação DSCP de QoS substituir a configuração, o código de erro: "%2".|  

Para o próximo tópico neste guia, consulte [QoS política Frequently Asked Questions](qos-policy-faq.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
