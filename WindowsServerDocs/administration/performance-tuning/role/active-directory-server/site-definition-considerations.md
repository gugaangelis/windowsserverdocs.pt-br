---
title: Definição de site e posicionamento do controlador de domínio no adiciona ajuste de desempenho
description: Definição de site e considerações de posicionamento do controlador de domínio em Active Directory ajuste de desempenho.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c6f94f1f3fee924ba0d9a3bfa0c712d62bb095
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947117"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Posicionamento adequado de controladores de domínio e considerações de site

A definição de site adequada é essencial para o desempenho. Os clientes que estão saindo do site podem experimentar mau desempenho para autenticações e consultas. Além disso, com a introdução do IPv6 em clientes, a solicitação pode vir do endereço IPv4 ou IPv6 e o Active Directory precisa ter sites adequadamente definidos para IPv6. O sistema operacional prefere o IPv6 para IPv4 quando ambos estiverem configurados.

A partir do Windows Server 2008, o controlador de domínio tenta usar a resolução de nomes para fazer uma pesquisa inversa a fim de determinar o site no qual o cliente deve estar. Isso pode causar esgotamento do pool de threads do ATQ e fazer com que o controlador de domínio fique sem resposta. A resolução apropriada para isso é definir corretamente a topologia do site para IPv6. Como alternativa, é possível otimizar a infraestrutura de resolução de nomes para responder rapidamente às solicitações do controlador de domínio. Para obter mais informações, consulte [resposta atrasada do controlador de domínio do Windows server 2008 ou do Windows server 2008 R2 para solicitações LDAP ou Kerberos](https://support.microsoft.com/kb/2668820).

Uma área adicional de consideração é localizar controladores de rede de leitura/gravação para cenários em que os RODCs estão em uso.  Determinadas operações exigem acesso a um controlador de domínio gravável ou direcionam um controlador de domínio gravável quando um controlador de domínio somente leitura seria suficiente.  A otimização desses cenários levaria dois caminhos:
-   Contatar controladores de domínio graváveis quando um controlador de domínio somente leitura seria suficiente.  Isso requer uma alteração no código do aplicativo.
-   Onde um controlador de domínio gravável pode ser necessário.  Coloque os controladores de domínio de leitura/gravação em locais centrais para minimizar a latência.

Para referência de informações adicionais:
-   [Compatibilidade de aplicativos com RODCs](https://technet.microsoft.com/library/cc772597.aspx)
-   [ADSI (interface do serviço de Active Directory) e o RODC (controlador de domínio somente leitura) – evitando problemas de desempenho](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Otimizar para referências

As referências são como as consultas LDAP são redirecionadas quando o controlador de domínio não hospeda uma cópia da partição consultada. Quando uma referência é retornada, ela contém o nome distinto da partição, um nome DNS e um número da porta. O cliente usa essas informações para continuar a consulta em um servidor que hospeda a partição. Esse é um cenário DCLocator e todas as recomendações de definições de site e posicionamento de controlador de domínio são mantidas, mas os aplicativos que dependem de referências geralmente são ignorados. É recomendável garantir que a topologia do AD, incluindo definições de site e posicionamento do controlador de domínio, reflita corretamente as necessidades do cliente. Além disso, isso pode incluir a existência de controladores de domínio de vários domínios em um único site, o ajuste das configurações de DNS ou a realocação do site de um aplicativo.

## <a name="optimization-considerations-for-trusts"></a>Considerações sobre otimização para relações de confiança

Em um cenário de floresta, as relações de confiança são processadas de acordo com a seguinte hierarquia de domínio: global-filho de domínio-&gt; domínio filho-&gt; domínio raiz da floresta-&gt; domínio filho-&gt; domínio filho principal. Isso significa que os canais seguros na raiz da floresta e cada um deles podem ficar sobrecarregados devido à agregação de solicitações de autenticação que entram em trânsito os DCs na hierarquia de confiança. Isso também pode gerar atrasos em diretórios ativos de grande dispersão geográfica quando a autenticação também tiver que transmitir links altamente latentes para afetar o fluxo acima. Sobrecargas podem ocorrer em cenários de confiança entre florestas e de nível inferior. As seguintes recomendações se aplicam a todos os cenários:

-   Ajuste corretamente o MaxConcurrentAPI para dar suporte à carga em todo o canal seguro. Para obter mais informações, consulte [como fazer o ajuste de desempenho para autenticação NTLM usando a configuração MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Crie confianças de atalho conforme apropriado com base na carga.

-   Verifique se cada controlador de domínio no domínio é capaz de executar a resolução de nomes e se comunicar com os controladores de domínio no domínio confiável.

-   Garanta que as considerações de localidade sejam levadas em conta para relações de confiança.

-   Habilite o Kerberos sempre que possível e minimize o uso do canal seguro para reduzir o risco de execução em afunilamentos de MaxConcurrentAPI.

Os cenários de confiança entre domínios são uma área que tem sido consistentemente um ponto problemático para muitos clientes. Resolução de nomes e problemas de conectividade, geralmente devido a firewalls, causam esgotamento de recursos no controlador de domínio confiante e afetam todos os clientes. Além disso, um cenário geralmente ignorado está otimizando o acesso a controladores de domínio confiáveis. As principais áreas para garantir que funcionem corretamente são as seguintes:

-   Verifique se a resolução de nomes DNS e WINS que os controladores de domínio confiáveis estão usando pode resolver uma lista precisa de controladores de domínio para o domínio confiável.

    -   Os registros adicionados estaticamente têm uma tendência de se tornar obsoletos e reintroduzir problemas de conectividade ao longo do tempo. Os encaminhamentos DNS, DNS dinâmico e mesclagem de infraestruturas WINS/DNS são mais mantidos em longa execução.

    -   Garanta a configuração adequada de encaminhadores, encaminhamentos condicionais e cópias secundárias para zonas de pesquisa direta e inversa para cada recurso no ambiente que um cliente pode precisar acessar. Novamente, isso exige a manutenção manual e tem uma tendência de se tornar obsoleto. A consolidação de infraestruturas é ideal.

-   Os controladores de domínio no domínio confiante tentarão localizar controladores de domínio no domínio confiável que estão no mesmo site primeiro e, em seguida, efetuarão failback para os localizadores genéricos.

    -   Para obter mais informações sobre como o DCLocator funciona, consulte [localizando um controlador de domínio no site mais próximo](https://technet.microsoft.com/library/cc978016.aspx).

    -   Convergir nomes de site entre os domínios confiáveis e confiantes para refletir o controlador de domínio no mesmo local. Verifique se os mapeamentos de sub-rede e endereço IP estão vinculados corretamente a sites em ambas as florestas. Para obter mais informações, consulte [localizador de domínio em uma relação de confiança de floresta](https://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Verifique se as portas estão abertas, de acordo com as necessidades do DCLocator, para o local do controlador de domínio. Se existirem firewalls entre os domínios, verifique se os firewalls estão configurados corretamente para todas as relações de confiança. Se os firewalls não estiverem abertos, o controlador de domínio confiante ainda tentará acessar o domínio confiável. Se a comunicação falhar por algum motivo, o controlador de domínio confiante eventualmente atingirá o tempo limite da solicitação para o controlador de domínio confiável. No entanto, esses tempos limite podem levar vários segundos por solicitação e podem esgotar portas de rede no controlador de domínio confiável se o volume de solicitações de entrada for alto. O cliente pode experimentar as esperas para o tempo limite no controlador de domínio como threads suspensos, que podem ser convertidos em aplicativos suspensos (se o aplicativo executar a solicitação no thread em primeiro plano). Para obter mais informações, consulte [como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442).

    -   Use DnsAvoidRegisterRecords para eliminar o mau desempenho ou controladores de domínio de alta latência, como aqueles em sites de satélite, de anúncios para os localizadores genéricos. Para obter mais informações, consulte [como otimizar o local de um controlador de domínio ou catálogo global que reside fora do site de um cliente](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Há um limite prático de cerca de 50 para o número de controladores de domínio que o cliente pode consumir. Eles devem ser os controladores de domínio de capacidade mais altos e ideais do site.

    
    -  Considere colocar controladores de domínio de domínios confiáveis e confiantes no mesmo local físico.

Para todos os cenários de confiança, as credenciais são roteadas de acordo com o domínio especificado nas solicitações de autenticação. Isso também é verdadeiro para consultas ao LookupAccountName e LsaLookupNames (bem como a outras, são apenas as APIs mais usadas). Quando os parâmetros de domínio dessas APIs forem passados como um valor nulo, o controlador de domínio tentará localizar o nome de conta especificado em todos os domínios confiáveis disponíveis.

-   Desabilite a verificação de todas as relações de confiança disponíveis quando um domínio nulo for especificado. [Como restringir a pesquisa de nomes isolados em domínios confiáveis externos usando a entrada de registro LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Desabilitar a passagem de solicitações de autenticação com um domínio nulo especificado em todas as relações de confiança disponíveis. [O processo Lsass. exe pode parar de responder se você tiver muitas relações de confiança externas em um controlador de domínio Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Veja também
- [Ajuste de desempenho Active Directory servidores](index.md)
- [Considerações sobre hardware](hardware-considerations.md)
- [Considerações sobre LDAP](ldap-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)