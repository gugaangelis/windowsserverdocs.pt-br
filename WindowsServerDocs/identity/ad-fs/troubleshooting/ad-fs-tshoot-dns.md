---
title: Solução de problemas AD FS-resolução de DNS
description: Este documento descreve como solucionar problemas de aspectos de DNS de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a9be4a72cd60cfdd5807c67132dba837093be4db
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959018"
---
# <a name="ad-fs-troubleshooting---dns"></a>Solução de problemas AD FS-DNS 
Uma das primeiras coisas a verificar, se AD FS não estiver funcionando ou respondendo, é a resolução de nomes DNS.  Esses são os testes básicos para determinar se os servidores de AD FS ou os servidores WAP estão sendo encontrados na sua rede.  Para usuários internos, esses testes devem ser resolvidos para os servidores de AD FS (STS).    Para usuários externos, esses testes devem ser resolvidos para os servidores WAP.

O restante deste documento mostrará como fazer algumas verificações de resolução de nome rápidas usando ferramentas de linha de comando.

## <a name="ping-test"></a>Teste de ping
Verifica a conectividade em nível de IP para outro computador TCP/IP enviando mensagens de solicitação de eco ICMP (Internet Control Message Protocol). O recebimento de mensagens de resposta de eco correspondentes é exibido, juntamente com tempos de ida e volta.  Para obter mais informações, consulte [ping](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961503(v=ws.11)).


>[!NOTE]
>Lembre-se de que algumas organizações bloqueiam essa porta em seus servidores e você pode receber uma **solicitação com tempo limite** de resposta.

### <a name="to-use-a-ping-test"></a>Para usar um teste de PING
1.  Abra um prompt de comando
2. Digite PING <name of adfs server> a. Exemplo: PING sts.contoso.com
3. Você deverá ver uma resposta do servidor

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Exibe informações que você pode usar para diagnosticar a infraestrutura do DNS (sistema de nomes de domínio).  Para obter mais informações, consulte [nslookup](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc725991(v=ws.11)).

### <a name="to-use-a-nslookup"></a>Para usar um NSLookup
1.  Abra um prompt de comando
2. Digite PING <name of adfs server> a. Exemplo: nslookup sts.contoso.com
3. Você deve ver as informações de DNS para o servidor ![ nslookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina o caminho levado para um destino enviando uma solicitação de eco do protocolo ICMP ou mensagens ICMPv6 para o destino com valores de campo TTL (vida útil) incrementalmente crescentes.   Para obter mais informações, consulte [tracert](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961507(v=ws.11)).


### <a name="to-use-tracert"></a>Para usar tracert
1.  Abra um prompt de comando
2. Digite tracert <name of adfs server> a. Exemplo: tracert sts.contoso.com
3. Você deve ver o caminho de destino usado para acessar o servidor ![ tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
