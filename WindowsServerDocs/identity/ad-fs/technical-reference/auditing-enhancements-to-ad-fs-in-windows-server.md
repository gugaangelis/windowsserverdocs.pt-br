---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Aprimoramentos de auditoria para o AD FS no Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880227"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Aprimoramentos de auditoria para o AD FS no Windows Server 2016

>Aplica-se a: Windows Server 2016

Atualmente, no AD FS para o Windows Server 2012 R2 lá são vários eventos de auditoria gerados por uma única solicitação e as informações relevantes sobre um log-in ou atividade de emissão de token está ausente (em algumas versões do AD FS) ou espalhada em vários eventos de auditoria. Por padrão, o AD FS eventos de auditoria são desativados devido à sua natureza detalhada.  
    Com o lançamento do AD FS no Windows Server 2016, o auditoria tornou mais simples e menos detalhada.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Níveis de auditoria do AD FS do Windows Server 2016  
Por padrão, o AD FS no Windows Server 2016 tem auditoria básica habilitada.  Com a auditoria básica, os administradores verão eventos 5 ou para uma única solicitação.  Isso marca uma redução significativa no número de eventos, os administradores têm a analisar, para ver uma única solicitação.   O nível de auditoria pode ser aumentado ou diminuído a usando o cmdlet do PowerShell:  Set-AdfsProperties - AuditLevel.  A tabela a seguir explica os níveis de auditoria disponíveis.  
  
||||  
|-|-|-|  
|Nível de Auditoria|Sintaxe do PowerShell|Descrição|  
|Nenhuma|Set-AdfsProperties - AuditLevel None|A auditoria está desabilitada e nenhum evento será registrado.|  
|Básico (padrão)|Set-AdfsProperties - AuditLevel Basic|Não mais de 5 eventos serão registrados para uma única solicitação|  
|Verbose|Set-AdfsProperties - AuditLevel detalhado|Todos os eventos serão registrados.  Isso registrará uma quantidade significativa de informações por solicitação.|  
  
Para exibir o nível de auditoria atual, você pode usar o cmdlet do PowerShell:  Get-AdfsProperties.  
  
![aprimoramentos de auditoria](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
O nível de auditoria pode ser aumentado ou diminuído a usando o cmdlet do PowerShell:  Set-AdfsProperties - AuditLevel.  
  
![aprimoramentos de auditoria](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipos de eventos de auditoria  
Eventos de auditoria do AD FS pode ser de tipos diferentes, com base em diferentes tipos de solicitações processadas pelo AD FS. Cada tipo de evento de auditoria tem dados específicos associados a ele.  O tipo de eventos de auditoria pode ser diferenciado entre as solicitações de logon (ou seja, solicitações de token) em vez de solicitações do sistema (chamadas de servidores como buscar informações de configuração).    
  A tabela a seguir descreve os tipos básicos de eventos de auditoria.  
  
||||  
|-|-|-|  
|Tipo de evento de auditoria|ID de evento|Descrição|  
|Sucesso da validação de credencial atualizada|1202|Uma solicitação em que as novas credenciais são validadas com êxito pelo serviço de Federação. Isso inclui o WS-Trust, WS-Federation, SAML-P (primeira ramificação para gerar SSO) e pontos de extremidade OAuth autorizar.|  
|Erro de validação de credencial atualizada|1203|Uma solicitação em que a validação de credenciais atualizado falhou no serviço de Federação. Isso inclui o WS-Trust, WS-Fed, SAML-P (primeira ramificação para gerar SSO) e pontos de extremidade OAuth autorizar.|  
|Êxito de Token de aplicativo|1200|Uma solicitação em que um token de segurança é emitido com êxito pelo serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação é processada com o artefato de SSO do SAML-P. (por exemplo, o cookie SSO).|  
|Falha de Token de aplicativo|1201|Uma solicitação em que a emissão de token de segurança falha no serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação foi processada com o artefato de SSO do SAML-P. (por exemplo, o cookie SSO).|  
|Êxito na solicitação de alteração de senha|1204|Uma transação em que a alteração de senha solicitação foi processada com êxito pelo serviço de Federação.|  
|Erro de solicitação de alteração de senha|1205|Falha de uma transação em que a senha de solicitação de alteração a ser processado pelo serviço de Federação.| 
|Sair com êxito|1206|Descreve uma solicitação de saída com êxito.|  
|Sair com falha|1207|Descreve uma solicitação de saída com falha.|  

  


