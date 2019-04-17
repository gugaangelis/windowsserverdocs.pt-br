---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Melhorias de auditoria para o AD FS no Windows Server 2016
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Melhorias de auditoria para o AD FS no Windows Server 2016

>Aplica-se a: Windows Server 2016

Atualmente, no AD FS para o Windows Server 2012 R2 lá são vários eventos de auditoria gerados para uma única solicitação e as informações relevantes sobre um log-in ou atividade de emissão de token está ausente (em algumas versões do AD FS) ou se espalham entre vários eventos de auditoria. Por padrão o AD FS eventos de auditoria estão desativados devido à sua natureza detalhada.  
    Com o lançamento do AD FS no Windows Server 2016, auditoria tornou-se mais simplificada e menos detalhada.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Níveis de auditoria no AD FS para o Windows Server 2016  
Por padrão, o AD FS no Windows Server 2016 tem básica de auditoria habilitado.  Com auditoria básica, os administradores verá eventos 5 ou menos para uma única solicitação.  Isso marca uma redução significativa no número de eventos que os administradores têm para examinar, para ver uma única solicitação.   O nível de auditoria pode ser aumentado ou diminuído usando o PowerShell cmdlt: conjunto AdfsProperties - AuditLevel.  A tabela a seguir explica os níveis de auditoria disponíveis.  
  
||||  
|-|-|-|  
|Nível de auditoria|Sintaxe do PowerShell|Descrição|  
|None|Conjunto-AdfsProperties - AuditLevel None|Auditoria é desabilitada e nenhum evento será registrado em log.|  
|Basic (padrão)|Conjunto-AdfsProperties - AuditLevel Basic|Não mais do que 5 eventos serão registrados para uma única solicitação|  
|Detalhada|Conjunto-AdfsProperties - AuditLevel detalhada|Todos os eventos serão registrados.  Isso registrará uma quantidade significativa de informações por solicitação.|  
  
Para exibir o nível atual de auditoria, você pode usar o cmdlt PowerShell: Get-AdfsProperties.  
  
![melhorias de auditoria](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
O nível de auditoria pode ser aumentado ou diminuído usando o PowerShell cmdlt: conjunto AdfsProperties - AuditLevel.  
  
![melhorias de auditoria](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipos de eventos de auditoria  
Eventos de auditoria do AD FS pode ser de tipos diferentes, com base em tipos diferentes de solicitações processadas pelo AD FS. Cada tipo de evento de auditoria tem dados específicos associados a ele.  O tipo de eventos de auditoria pode ser diferenciado entre solicitações de logon (ou seja, solicitações de token) versus solicitações do sistema (chamadas de servidores incluindo buscar informações de configuração).    
  A tabela a seguir descreve os tipos básicos de eventos de auditoria.  
  
||||  
|-|-|-|  
|Tipo de evento de auditoria|ID de evento|Descrição|  
|Êxito de validação de credenciais atualizado|1202|Uma solicitação onde novas credenciais são validadas com êxito pelo serviço de Federação. Isso inclui WS-Trust, WS-Federation SAML-P (primeiro passo à frente para gerar SSO) e pontos de extremidade do OAuth autorizar.|  
|Erro de validação de credenciais atualizado|1203|Uma solicitação onde Falha na validação de credenciais atualizado sobre o serviço de Federação. Isso inclui WS-Trust, WS-Fed, P SAML (primeiro passo à frente para gerar SSO) e pontos de extremidade do OAuth autorizar.|  
|Êxito de Token do aplicativo|1200|Uma solicitação onde um token de segurança é emitido com êxito pelo serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação é processada com o artefato SSO SAML-P. (por exemplo, o cookie de logon único).|  
|Falha de Token do aplicativo|1201|Uma solicitação de onde a emissão de token de segurança falha no serviço de Federação. Para WS-Federation, isso é registrado quando a solicitação foi processada com o artefato SSO SAML-P. (por exemplo, o cookie de logon único).|  
|Êxito de solicitação de alteração de senha|1204|Uma transação onde a senha de alterar a solicitação foi processada com êxito pelo serviço de Federação.|  
|Erro de solicitação de alteração de senha|1205|Uma transação onde a senha alterar solicitação falhou a ser processado pelo serviço de Federação.| 
|Saia de sucesso|1206|Descreve uma solicitação de saída bem-sucedida.|  
|Saia de falha|1207|Descreve uma solicitação de saída com falha.|  

  


