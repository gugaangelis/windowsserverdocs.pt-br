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
ms.openlocfilehash: 7ffda6916bd91f1195ac0c289959becafff1d2c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407209"
---
# <a name="ad-fs-troubleshooting---dns"></a>Solução de problemas AD FS-DNS 
Uma das primeiras coisas a verificar, se AD FS não estiver funcionando ou respondendo, é a resolução de nomes DNS.  Esses são os testes básicos para determinar se os servidores de AD FS ou os servidores WAP estão sendo encontrados na sua rede.  Para usuários internos, esses testes devem ser resolvidos para os servidores de AD FS (STS).    Para usuários externos, esses testes devem ser resolvidos para os servidores WAP.

O restante deste documento mostrará como fazer algumas verificações de resolução de nome rápidas usando ferramentas de linha de comando.

## <a name="ping-test"></a>Teste de ping
Verifica a conectividade em nível de IP para outro computador TCP/IP enviando mensagens de solicitação de eco ICMP (Internet Control Message Protocol). O recebimento de mensagens de resposta de eco correspondentes é exibido, juntamente com tempos de ida e volta.  Para obter mais informações, consulte [ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Lembre-se de que algumas organizações bloqueiam essa porta em seus servidores e você pode receber uma **solicitação com tempo limite** de resposta.

### <a name="to-use-a-ping-test"></a>Para usar um teste de PING
1.  Abrir um prompt de comando
2. Insira PING <name of adfs server> a. Exemplo: PING sts.contoso.com
3. Você deverá ver uma resposta do servidor

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Exibe informações que você pode usar para diagnosticar a infraestrutura do DNS (sistema de nomes de domínio).  Para obter mais informações, consulte [nslookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Para usar um NSLookup
1.  Abrir um prompt de comando
2. Insira PING <name of adfs server> a. Exemplo: nslookup sts.contoso.com
3. Você deve ver as informações de DNS para o servidor ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina o caminho levado para um destino enviando uma solicitação de eco do protocolo ICMP ou mensagens ICMPv6 para o destino com valores de campo TTL (vida útil) incrementalmente crescentes.   Para obter mais informações, consulte [tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Para usar tracert
1.  Abrir um prompt de comando
2. Insira tracert <name of adfs server> a. Exemplo: tracert sts.contoso.com
3. Você deve ver o caminho de destino usado para acessar o servidor ![tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)