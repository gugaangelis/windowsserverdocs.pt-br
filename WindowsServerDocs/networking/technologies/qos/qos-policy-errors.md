---
title: Mensagens de erro e evento da política de QoS
description: Este tópico fornece uma lista de mensagens de erro e evento para a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d0eb137716795c324afcf1a708fff00c2f7266d6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315559"
---
# <a name="qos-policy-error-and-event-messages"></a>Mensagens de erro e evento da política de QoS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

A seguir estão as mensagens de erro e de evento associadas à política de QoS.  
  
## <a name="informational-messages"></a>Mensagens informativas  

A seguir está uma lista de mensagens informativas de política de QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS de computador atualizadas com êxito. Nenhuma alteração detectada.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS de computador atualizadas com êxito. Alterações de política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS de usuário atualizadas com êxito. Nenhuma alteração detectada.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Idioma**|Inglês|  
|**Mensagem**|Políticas de QoS de usuário atualizadas com êxito. Alterações de política detectadas.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP de entrada foi atualizada com êxito. O valor da configuração não é especificado por nenhuma política de QoS. O padrão do computador local será aplicado.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP de entrada foi atualizada com êxito. O valor da configuração é nível 0 (taxa de transferência mínima).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP de entrada foi atualizada com êxito. O valor da configuração é nível 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP de entrada foi atualizada com êxito. O valor da configuração é nível 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para o nível de taxa de transferência TCP de entrada foi atualizada com êxito. O valor da configuração é nível 3 (taxa de transferência máxima).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para as substituições de marcação DSCP foi atualizada com êxito. O valor da configuração não foi especificado. Os aplicativos podem definir valores de DSCP independentemente das políticas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para as substituições de marcação DSCP foi atualizada com êxito. As solicitações de marcação DSCP do aplicativo serão ignoradas. Somente as políticas de QoS podem definir valores DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Idioma**|Inglês|  
|**Mensagem**|A configuração de QoS avançada para as substituições de marcação DSCP foi atualizada com êxito. Os aplicativos podem definir valores de DSCP independentemente das políticas de QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Idioma**|Inglês|  
|**Mensagem**|A aplicação seletiva de políticas de QoS com base na categoria de rede de domínio foi desabilitada. As políticas de QoS serão aplicadas a todas as interfaces de rede.|  
  
## <a name="warning-messages"></a>Mensagens de Aviso

A seguir está uma lista de mensagens de aviso da política de QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * testando\*\*\*[, com uma cadeia de caracteres] "%2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Idioma**|Inglês|  
|**Mensagem**|EQOS: * * * testando\*\*\*[, com duas cadeias de caracteres, seqüência1 é] "%2" [, string2 é] "%3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS do computador "%2" tem um número de versão inválido. Esta política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS de usuário "%2" tem um número de versão inválido. Esta política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS do computador "%2" não especifica um valor DSCP ou uma taxa de limitação. Esta política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS de usuário "%2" não especifica um valor DSCP ou uma taxa de limitação. Esta política não será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|O número máximo de políticas de QoS do computador foi excedido. A política de QoS "%2" e as políticas de QoS de computador subsequentes não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Idioma**|Inglês|  
|**Mensagem**|O número máximo de políticas de QoS de usuário foi excedido. A política de QoS "%2" e as políticas de QoS de usuário subsequentes não serão aplicadas.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS de computador "%2" pode estar em conflito com outras políticas de QoS. Consulte a documentação para obter regras sobre qual política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS de usuário "%2" pode estar em conflito com outras políticas de QoS. Consulte a documentação para obter regras sobre qual política será aplicada.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS do computador "%2" foi ignorada porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválido, conter uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|Aviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Idioma**|Inglês|  
|**Mensagem**|A política de QoS de usuário "%2" foi ignorada porque o caminho do aplicativo não pode ser processado. O caminho do aplicativo pode ser inválido, conter uma letra de unidade inválida ou conter uma unidade de rede mapeada.|  
  
## <a name="error-messages"></a>Mensagens de erro  

A seguir está uma lista de mensagens de erro de política de QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao atualizar as políticas de QoS do computador. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha ao atualizar as políticas de QoS de usuário. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao abrir a chave raiz no nível da máquina para políticas de QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha do QoS ao abrir a chave raiz no nível do usuário para políticas de QoS. Código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma política de QoS de computador excede o comprimento máximo de nome permitido. A política incorreta está listada na chave raiz da política de QoS no nível da máquina, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma política de QoS de usuário excede o comprimento máximo de nome permitido. A política incorreta está listada na chave raiz de política de QoS de nível de usuário, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma política de QoS de computador tem um nome de comprimento zero. A política incorreta está listada na chave raiz da política de QoS no nível da máquina, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Idioma**|Inglês|  
|**Mensagem**|Uma política de QoS de usuário tem um nome de comprimento zero. A política incorreta está listada na chave raiz de política de QoS de nível de usuário, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha do QoS ao abrir a subchave do registro para uma política de QoS do computador. A política está listada na chave raiz da política de QoS no nível da máquina, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao abrir a subchave do registro para uma política de QoS de usuário. A política está listada na chave raiz de política de QoS de nível de usuário, com o índice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao ler ou validar o campo "%2" para a política de QoS do computador "%3".|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao ler ou validar o campo "%2" para a política de QoS de usuário "%3".|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao ler ou definir o nível de taxa de transferência TCP de entrada, código de erro: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|Error|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Idioma**|Inglês|  
|**Mensagem**|Falha de QoS ao ler ou definir a configuração de substituição de marcação DSCP, código de erro: "%2".|  

Para o próximo tópico deste guia, consulte perguntas frequentes [sobre a política de QoS](qos-policy-faq.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).
