---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: Solução de problemas do AD DS
description: Visão geral da seção de solução de problemas para AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b9e1cf812234bc3b0bd81db7045ed83208f3a0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824359"
---
# <a name="ad-ds-troubleshooting"></a>Solução de problemas do AD DS

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Esta seção inclui recomendações de solução de problemas e procedimentos para diagnosticar e corrigir problemas que podem ocorrer durante a replicação de Active Directory. Ele se concentra em como responder às entradas do log de eventos do serviço de diretório e como interpretar mensagens que ferramentas como repadmin. exe e Dcdiag. exe podem relatar.

O Repadmin. exe e o Dcdiag. exe estão disponíveis em todos os controladores de domínio que executam o Windows Server 2012 R2 ou versões posteriores. Para obter mais informações sobre como usar essas ferramentas para solucionar problemas, consulte os artigos a seguir.

- [Configurando um computador para solução de problemas Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [Como solucionar problemas de replicação do Active Directory](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

Outra tecnologia útil é o ETW (rastreamento de eventos para Windows). Você pode usar o ETW para solucionar problemas de comunicações LDAP entre os controladores de domínio. Para obter mais informações, consulte [usando o ETW para solucionar problemas de conexões LDAP](../manage/troubleshoot/troubleshoot-ldap-using-etw.md).

Você também pode instalar o Ferramentas de Administração de Servidor Remoto (RSAT) em um servidor membro que esteja executando o Windows 10. Para obter informações sobre como instalar o RSAT, consulte [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
