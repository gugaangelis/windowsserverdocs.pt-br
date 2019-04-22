---
title: Solucionando problemas do AD FS - resolução de DNS
description: Este documento descreve como solucionar problemas de aspectos DNS do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815897"
---
# <a name="ad-fs-troubleshooting---dns"></a>Solucionando problemas do AD FS - DNS 
Uma das primeiras coisas a verificar, se o AD FS não está funcionando ou respondendo, é a resolução de nomes DNS.  Esses são os testes básicos para determinar se os servidores do AD FS ou servidores WAP estão sendo encontrados em sua rede.  Para usuários internos, esses testes devem resolver para os servidores do AD FS (STS).    Para usuários externos, esses testes devem resolver para os servidores WAP.

O restante deste documento mostrará como fazer algumas verificações de resolução de nome rápida usando ferramentas de linha de comando.

## <a name="ping-test"></a>Teste de ping
Verifica a conectividade de nível de IP para outro computador TCP/IP enviando mensagens de solicitação de eco do ICMP Internet Control Message Protocol (). O recebimento de mensagens de resposta de eco correspondentes são exibidos, juntamente com os tempos de ida e volta.  Para obter mais informações, consulte [Ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Lembre-se de que algumas organizações bloqueiam essa porta nos seus servidores e você poderá receber um **solicitação atingiu o tempo limite** resposta.

### <a name="to-use-a-ping-test"></a>Para usar um teste de PING
1.  Abra um prompt de comando
2. Digite PING <name of adfs server> um. Exemplo:  PING sts.contoso.com
3. Você deve ver uma resposta do servidor

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Exibe informações que você pode usar para diagnosticar a infra-estrutura do sistema de nome de domínio (DNS).  Para obter mais informações, consulte [NSLookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Para usar um NSLookup
1.  Abra um prompt de comando
2. Digite PING <name of adfs server> um. Exemplo: sts.contoso.com de nslookup
3. Você deve ver as informações de dns para o servidor ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina o caminho tomado para um destino de solicitação de eco de ICMP Internet Control Message Protocol () enviando mensagens ou ICMPv6 para o destino com valores cada vez maiores valores de campo de vida (TTL).   Para obter mais informações, consulte [Tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Para usar o Tracert
1.  Abra um prompt de comando
2. Insira o tracert <name of adfs server> um. Exemplo: sts.contoso.com de tracert
3. Você deve ver o caminho de destino usado para acessar o servidor ![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)