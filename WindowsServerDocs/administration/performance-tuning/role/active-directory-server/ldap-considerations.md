---
title: Considerações de LDAP no ajuste de desempenho do ADDS
description: Considerações de LDAP em cargas de trabalho do Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 79f95c88c49d384f8a13b8808c63a0dc00de53cb
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266626"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considerações de LDAP no ajuste de desempenho do ADDS

>[!Important]
> A seguir está um resumo da chave recomendações e considerações para otimizar o hardware de servidor para cargas de trabalho do Active Directory abordado em mais detalhes na [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) artigo. Os leitores são altamente incentivados a revisar [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para uma maior compreensão técnica e as implicações dessas recomendações.

## <a name="verify-ldap-queries"></a>Verifique se as consultas LDAP

Verifique se que as consultas LDAP está em conformidade com as recomendações de consultas eficientes de criação.

Há uma ampla documentação no MSDN sobre como escrever, estrutura e analisar consultas para uso no Active Directory corretamente. Para obter mais informações, consulte [Creating More Efficient Microsoft Active Directory-Enabled aplicativos](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Otimizar os tamanhos de página LDAP

Ao retornar resultados com vários objetos em resposta às solicitações do cliente, o controlador de domínio precisa armazenar temporariamente o conjunto de resultados na memória. Cada vez maior de tamanhos de página fará com que mais uso de memória e têm duração itens fora do cache desnecessariamente. Nesse caso, as configurações padrão são ideais. Há várias situações em que as recomendações foram feitas para aumentar as configurações de tamanho de página. É recomendável usar os valores padrão, a menos que especificamente identificados como inadequados.

Quando consultas tem muitos resultados, um limite de consultas semelhantes executado simultaneamente pode ser encontrado.  Isso ocorre pois o servidor LDAP depauperam o uma área de memória global, conhecida como o pool de cookies.  Talvez seja necessário aumentar o tamanho do pool, conforme discutido em [como LDAP Server os Cookies são tratados](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Para ajustar essas configurações, consulte [controlador de domínio de 2008 e mais recente do Windows Server retorna valores apenas 5000 em uma resposta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinar se deseja adicionar índices

Os atributos de indexação é útil ao procurar por objetos que têm o nome do atributo em um filtro. A indexação pode reduzir o número de objetos que devem ser visitados ao avaliar o filtro. No entanto, isso reduz o desempenho de operações de gravação porque o índice deve ser atualizado quando o atributo correspondente é modificado ou adicionado. Isso também aumenta o tamanho do banco de dados de diretório, embora os benefícios com frequência superam o custo de armazenamento. Registro em log pode ser usado para localizar as consultas ineficientes e caras. Uma vez identificado, considere a indexar alguns atributos que são usados nas consultas correspondentes para melhorar o desempenho da pesquisa. Para obter mais informações sobre o funcionamento de pesquisas do Active Directory, consulte [Active Directory pesquisas funcionamento](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Cenários que se beneficiam na adição de índices

-   Carga do cliente na solicitação de dados está gerando o uso de CPU significativo e o comportamento de consulta do cliente não pode ser alterado ou otimizado. Por carga significativa, considere a possibilidade de que ele está mostrando em si em uma lista de criminoso 10 principais no Server Performance Advisor ou o interno Active Directory dados conjunto de Coletores e está usando mais de 1% da CPU.

-   A carga de cliente está gerando a e/s de disco significativo em um servidor devido a um atributo não indexado e o comportamento de consulta do cliente não pode ser alterado ou otimizado.

-   Uma consulta está demorando muito tempo e não está concluindo em um período de tempo aceitável para o cliente devido à falta de cobertura de índices.

-   Grandes volumes de consultas com alta durações estão causando o esgotamento dos Threads de LDAP ATQ e o consumo. Monitore os contadores de desempenho a seguir:

    -   **NTDS\\latência de solicitação** – isso está sujeito a quanto a solicitação é necessário para processar. Active Directory solicitações expira depois de 120 segundos (padrão), no entanto, a maioria deve ser executado muito mais rapidamente e consultas extremamente longas devem obter ocultos nos números geral. Procure alterações nessa linha de base, em vez de limites absolutos.

        > [!Note]   Valores altos aqui também podem ser indicadores de atrasos nas solicitações de "proxy" para outros domínios e verificações CRL.


    -   **NTDS\\estimado atraso de fila** – ideal seria próximos a 0 para um desempenho ideal, isso significa que as solicitações não gastam nenhum tempo aguardando para serem atendidas.

Esses cenários podem ser detectados usando uma ou mais das seguintes abordagens:

-   [Determinar o tempo de consulta com o controle de estatísticas](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Pesquisas de ineficientes e caros de acompanhamento](https://msdn.microsoft.com/library/ms808539.aspx)

-   Conjunto de Coletores de dados de diagnóstico do Active Directory no Monitor de desempenho ([Son do SPA: No Win2008 e além dos conjuntos de Coletores de dados do AD](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) pacote de Supervisor do Active Directory

-   Pesquisas usando qualquer filtro além de "(objectClass =\*)" que usam o índice de ancestrais.

### <a name="other-index-considerations"></a>Outras considerações de índice

-   Certifique-se de que criar o índice é a solução certa para o problema, depois de ajustar a consulta tiver se esgotado como uma opção. Dimensionar o hardware corretamente é muito importante. Índices devem ser adicionados somente quando a correção certa é para indexar o atributo e não uma tentativa de ofuscar problemas de hardware.

-   Índices de aumentam o tamanho do banco de dados em um mínimo de tamanho total do atributo que está sendo indexado. Uma estimativa do crescimento do banco de dados, portanto, pode ser avaliada levando o tamanho médio dos dados no atributo e multiplicando-se pelo número de objetos que terão o atributo preenchido. Geralmente, trata-se um aumento de 1% no tamanho do banco de dados. Para obter mais informações, consulte [como o Data Store funciona](https://technet.microsoft.com/library/cc772829.aspx).

-   Se o comportamento de pesquisa é feito principalmente no nível da unidade de organização, considere indexar para pesquisas contidas.

-   Índices de tupla são maiores do que os índices normais, mas é muito mais difícil de estimar o tamanho. Use as estimativas de tamanho de índices normais como a base para crescimento, com um máximo de 20%. Para obter mais informações, consulte [como o Data Store funciona](https://technet.microsoft.com/library/cc772829.aspx).

-   Se o comportamento de pesquisa é feito principalmente no nível da unidade de organização, considere indexar para pesquisas contidas.

-   Índices de tupla são necessários para dar suporte a média de pesquisar cadeias de caracteres e cadeias de caracteres de pesquisa final. Índices de tupla não são necessários para cadeias de caracteres de pesquisa inicial.

    -   Inicial de pesquisa de cadeia de caracteres – (samAccountName = MYPC\*)

    -   Cadeia de caracteres de pesquisa média - (samAccountName =\*MYPC\*)

    -   Cadeia de caracteres de pesquisa final – (samAccountName =\*MYPC$)

-   Criar um índice irá gerar e/s de disco, enquanto o índice está sendo construído. Isso é feito em um thread em segundo plano com prioridade mais baixa e solicitações de entrada terá prioridade sobre a compilação de índice. Se o planejamento de capacidade para o ambiente tiver sido feita corretamente, isso deverá ser transparente. No entanto, os cenários de gravação intensa ou um ambiente em que a carga no armazenamento do controlador de domínio é desconhecida pode degradar a experiência do cliente e deve ser feito fora do horário comercial.

-   Afeta o tráfego de replicação é mínima, pois a criação de índices ocorre localmente.

Para obter mais informações, consulte o seguinte:

-   [Criação de aplicativos mais eficientes do Microsoft Active Directory habilitado](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Pesquisar no Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Atributos indexados](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)


## <a name="see-also"></a>Consulte também
- [Servidores do Active Directory de ajuste de desempenho](index.md)
- [Considerações sobre hardware](hardware-considerations.md)
- [Posicionamento adequado dos controladores de domínio e considerações sobre o local](site-definition-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)