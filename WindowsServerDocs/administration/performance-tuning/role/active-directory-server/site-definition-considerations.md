---
title: Posicionamento de controlador de domínio e de definição de site no ajuste de desempenho do ADDS
description: Site definição e o domínio controlador considerações sobre o posicionamento no ajuste de desempenho do Active Directory.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9861703e5ae88dcaec5e76d9fab426b928d0cb9a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811500"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Posicionamento adequado dos controladores de domínio e considerações de site

Definição de site correto é essencial para o desempenho. Os clientes que estão saindo do site podem enfrentar um desempenho ruim para autenticações e consultas. Além disso, com a introdução do IPv6 nos clientes, a solicitação pode ser originada em IPv4 ou o endereço IPv6 e o Active Directory precisa ter sites definidos corretamente para IPv6. O sistema operacional prefere IPv6 para IPv4, quando ambos estiverem configurados.

A partir do Windows Server 2008, as tentativas de controlador de domínio para usar a resolução de nome para fazer uma pesquisa inversa para determinar o site do cliente devem estar no. Isso pode causar esgotamento do Pool de threads ATQ e fazer com que o controlador de domínio pare de responder. A resolução apropriada para isso é definir corretamente a topologia de site para IPv6. Como alternativa, um pode otimizar a infra-estrutura de resolução de nome para responder rapidamente às solicitações do controlador de domínio. Para obter mais informações, consulte [Windows Server 2008 ou o controlador de domínio do Windows Server 2008 R2 atrasada resposta às solicitações LDAP ou Kerberos](https://support.microsoft.com/kb/2668820).

Uma área adicional de consideração está localizando os controladores de domínio de leitura/gravação para cenários em que os RODCs estão em uso.  Determinadas operações exigem acesso a um controlador de domínio gravável ou um controlador de domínio gravável de destino quando um controlador de domínio somente leitura seria suficiente.  Otimizando desses cenários levaria dois caminhos:
-   Entrando em contato com os controladores de domínio gravável quando um controlador de domínio somente leitura seria suficiente.  Isso exige uma mudança de código do aplicativo.
-   Um controlador de domínio gravável no qual pode ser necessário.  Local leitura-gravação controladores de domínio em locais centrais para minimizar a latência.

Para obter mais informações, consulte:
-   [Compatibilidade de aplicativos com os RODCs](https://technet.microsoft.com/library/cc772597.aspx)
-   [Interface de serviço do Active Directory (ADSI) e a leitura somente domínio RODC (controlador) – evitando problemas de desempenho](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Otimizar para indicações

As referências são como consultas LDAP são redirecionadas quando o controlador de domínio não hospede uma cópia da partição consultada. Quando uma referência é retornada, ele contém um número de porta, um nome DNS e o nome diferenciado da partição. O cliente usa essas informações para continuar a consulta em um servidor que hospeda a partição. Esse é um cenário de DCLocator e todas as recomendações definições de site e posicionamento de controlador de domínio é mantido, mas os aplicativos que dependem de referências que são frequentemente ignorados. É recomendável para garantir que a topologia do AD incluindo definições de site e o posicionamento de controlador de domínio corretamente reflete as necessidades do cliente. Além disso, isso pode incluir ter controladores de domínio de vários domínios em um único site, ajuste as configurações de DNS ou realocar o site de um aplicativo.

## <a name="optimization-considerations-for-trusts"></a>Considerações sobre a otimização para relações de confiança

Em um cenário entre florestas, relações de confiança são processadas de acordo com a seguinte hierarquia de domínio: Domínio netos -&gt; domínio filho -&gt; floresta de domínio - raiz da&gt; domínio filho -&gt; netos de domínio. Isso significa que proteger canais na raiz da floresta e cada pai, fique sobrecarregada devido a agregação de solicitações de autenticação transitar os controladores de domínio na hierarquia de confiança. Isso também possam causar atrasos em Active Directories de dispersão geográfica grande quando autenticação também tem links altamente latente para afetar o fluxo acima de trânsito. Sobrecargas podem ocorrer em cenários de confiança entre florestas e de nível inferior. As seguintes recomendações se aplicam a todos os cenários:

-   Ajuste adequadamente o MaxConcurrentAPI para suportar a carga entre o canal seguro. Para obter mais informações, consulte [como fazer o ajuste de desempenho para a autenticação NTLM, usando a configuração MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Crie relações de confiança de atalho conforme apropriado com base na carga.

-   Certifique-se de que cada controlador de domínio no domínio é capaz de executar a resolução de nome e se comunicar com os controladores de domínio no domínio confiável.

-   Certifique-se de considerações de localidade são levadas em conta para relações de confiança.

-   Habilitar o Kerberos sempre que possível e minimizar o uso do canal seguro para reduzir o risco de execução em MaxConcurrentAPI gargalos.

Entre os cenários são uma área que tem sido consistentemente um ponto problemático para muitos clientes de confiança de domínio. As questões de conectividade e resolução de nome, geralmente devido a firewalls, fazer com que o esgotamento de recursos no controlador de domínio confiante e afetam todos os clientes. Além disso, um cenário de muitas vezes negligenciado é otimizar o acesso aos controladores de domínio confiável. As áreas principais para garantir que isso funcione corretamente são da seguinte maneira:

-   Verifique se a resolução de nome DNS e WINS que os controladores de domínio confiante usam pode resolver uma lista precisa dos controladores de domínio para o domínio confiável.

    -   Registros adicionados estaticamente têm uma tendência a se tornar obsoletos e reintroduzir a problemas de conectividade ao longo do tempo. O DNS encaminha DNS dinâmico, e infraestruturas DNS/WINS mesclagem são mais passível de manutenção a longo prazo.

    -   Verifique se a configuração adequada de encaminhadores, encaminhamentos condicionais e as cópias secundárias para ambas as zonas de Observação progressivas e reversas para todos os recursos no ambiente do que um cliente talvez precise acessar. Novamente, isso exige manutenção manual e tem uma tendência de se tornarem obsoletos. Consolidação de infra-estruturas é ideal.

-   Controladores de domínio no domínio confiante tentará localizar controladores de domínio no domínio confiável que estão no mesmo site primeiro e, em seguida, realizar o failback para os localizadores genéricos.

    -   Para obter mais informações sobre como funciona o DCLocator, consulte [encontrar um controlador de domínio no Site mais próximo](https://technet.microsoft.com/library/cc978016.aspx).

    -   Convergir os nomes de site entre os domínios confiáveis e confiantes para refletir o controlador de domínio no mesmo local. Verifique se a sub-rede e o endereço IP mapeamentos corretamente estão vinculados a sites em ambas as florestas. Para obter mais informações, consulte [domínio localizador em uma confiança de floresta](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Verifique se as portas estão abertas, de acordo com as necessidades de DCLocator, para o local do controlador de domínio. Se houver firewalls entre os domínios, certifique-se de que os firewalls estão configurados corretamente para todas as relações de confiança. Se os firewalls não estiverem abertos, o controlador de domínio confiante ainda tentará acessar o domínio confiável. Se a comunicação falhar por algum motivo, o controlador de domínio confiante, eventualmente, atingirá o tempo limite de solicitação para o controlador de domínio confiável. No entanto, esses tempos pode levar vários segundos por solicitação e pode esgotar a portas de rede no controlador de domínio confiante se o volume de solicitações de entrada for alto. O cliente pode enfrentar o espera para tempo limite no controlador de domínio como threads travados, o que poderia equivaler a aplicativos travados (se o aplicativo é executado a solicitação no thread de primeiro plano). Para obter mais informações, consulte [como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442).

    -   Use DnsAvoidRegisterRecords para eliminar a controladores de domínio de desempenho ruim ou alta latência, como aqueles em locais de satélite, de anúncio para os localizadores genéricos. Para obter mais informações, consulte [como otimizar o local de um controlador de domínio ou catálogo global que reside fora do site do cliente](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Há um limite prático de cerca de 50 para o número de controladores de domínio, que o cliente pode consumir. Eles devem ser mais capacidade ideal de site e maior controladores de domínio.

    
    -  Considere a colocação de controladores de domínio de domínios confiáveis e confiantes no mesmo local físico.

Para todos os cenários de confiança, as credenciais são roteadas de acordo com o domínio especificado nas solicitações de autenticação. Isso também é verdadeiro para consultas ao LookupAccountName e LsaLookupNames (bem como outros, eles são apenas mais usados) APIs. Quando os parâmetros de domínio para essas APIs recebem um valor NULL, o controlador de domínio tentará localizar o nome da conta especificada em cada domínio confiável disponível.

-   Desabilite a verificação de todas as relações de confiança disponíveis quando o domínio NULL é especificado. [Como restringir a pesquisa de nomes isoladas em domínios confiáveis externos usando a entrada do registro LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Desabilitar passar solicitações de autenticação com o domínio NULL especificado entre todas as relações de confiança disponíveis. [O processo de Lsass.exe pode parar de responder se você tiver várias relações de confiança externas em um controlador de domínio do Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Consulte também
- [Servidores do Active Directory de ajuste de desempenho](index.md)
- [Considerações sobre hardware](hardware-considerations.md)
- [Considerações sobre LDAP](ldap-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)