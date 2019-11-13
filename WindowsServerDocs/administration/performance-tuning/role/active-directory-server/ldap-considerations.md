---
title: Considerações sobre LDAP no adiciona ajuste de desempenho
description: Considerações sobre LDAP em cargas de trabalho Active Directory
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f6670c8cfd718360518869f0551461c45e5aed27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370276"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considerações sobre LDAP no adiciona ajuste de desempenho

> [!IMPORTANT]
> Veja a seguir um resumo das principais recomendações e considerações para otimizar o hardware do servidor para Active Directory cargas de trabalho abordadas em mais detalhes no artigo [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Os leitores são altamente incentivados a examinar o [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para obter uma compreensão técnica mais alta e as implicações dessas recomendações.

## <a name="verify-ldap-queries"></a>Verificar consultas LDAP

Verifique se as consultas LDAP estão em conformidade com as recomendações de criação de consultas eficientes.

Há uma abrangente documentação sobre o MSDN sobre como escrever, estruturar e analisar consultas de forma adequada para uso em Active Directory. Para obter mais informações, consulte [criando aplicativos mais eficientes habilitados para o Microsoft Active Directory](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Otimizar tamanhos de página LDAP

Ao retornar resultados com vários objetos em resposta às solicitações do cliente, o controlador de domínio deve armazenar temporariamente o conjunto de resultados na memória. O aumento dos tamanhos de página causará mais utilização de memória e os itens de idade poderão ser desnecessariamente armazenados no cache. Nesse caso, as configurações padrão são ideais. Há vários cenários em que foram feitas recomendações para aumentar as configurações de tamanho de página. É recomendável usar os valores padrão, a menos que especificamente identificado como inadequado.

Quando as consultas têm muitos resultados, um limite de consultas semelhantes executadas simultaneamente pode ser encontrado.  Isso ocorre porque o servidor LDAP pode depauperam uma área de memória global conhecida como o pool de cookies.  Pode ser necessário aumentar o tamanho do pool conforme discutido em [como os cookies do servidor LDAP são manipulados](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Para ajustar essas configurações, consulte [o Windows Server 2008 e o controlador de domínio mais recente retornam apenas 5000 valores em uma resposta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinar se os índices devem ser adicionados

Atributos de indexação são úteis ao pesquisar objetos que têm o nome do atributo em um filtro. A indexação pode reduzir o número de objetos que devem ser visitados ao avaliar o filtro. No entanto, isso reduz o desempenho das operações de gravação porque o índice deve ser atualizado quando o atributo correspondente é modificado ou adicionado. Ele também aumenta o tamanho do banco de dados do diretório, embora os benefícios geralmente superem o custo do armazenamento. O registro em log pode ser usado para encontrar as consultas caras e ineficientes. Depois de identificado, considere indexar alguns atributos que são usados nas consultas correspondentes para melhorar o desempenho da pesquisa. Para obter mais informações sobre como Active Directory pesquisas funcionam, consulte [como Active Directory pesquisas funcionam](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Cenários que se beneficiam na adição de índices

-   A carga do cliente na solicitação dos dados está gerando um uso de CPU significativo e o comportamento de consulta do cliente não pode ser alterado ou otimizado. Por carga significativa, considere que ele está aparecendo em uma lista dos 10 principais infratores no Supervisor de desempenho de servidor ou no conjunto de coletores de dados Active Directory interno e está usando mais de 1% da CPU.

-   A carga do cliente está gerando e/s de disco significativa em um servidor devido a um atributo não indexado e o comportamento de consulta do cliente não pode ser alterado ou otimizado.

-   Uma consulta está demorando muito e não está sendo concluída em um período de tempo aceitável para o cliente devido à falta de índices abrangentes.

- Grandes volumes de consultas com altas durações estão causando consumo e esgotamento de threads de LDAP do ATQ. Monitore os seguintes contadores de desempenho:

    - **\\a latência de solicitação de NTDS** – isso está sujeito a quanto tempo a solicitação leva para ser processada. Active Directory expirar solicitações após 120 segundos (padrão), no entanto, a maioria deve ser executada com mais rapidez e as consultas extremamente longas devem ficar ocultas nos números gerais. Procure alterações nesta linha de base, em vez de limites absolutos.

        > [!NOTE]
        > Os valores altos aqui também podem ser indicadores de atrasos em solicitações de "proxy" para outros domínios e verificações de CRL.

    - **NTDS\\atraso estimado da fila** – é ideal que isso seja próximo de 0 para o desempenho ideal, pois isso significa que as solicitações não gastam tempo esperando para serem atendidas.

Esses cenários podem ser detectados usando uma ou mais das seguintes abordagens:

-   [Determinando o tempo de consulta com o controle de estatísticas](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Acompanhamento de pesquisas caras e ineficientes](https://msdn.microsoft.com/library/ms808539.aspx)

-   Active Directory conjunto de coletores de dados de diagnóstico no monitor de desempenho ([filho de Spa: conjuntos de coletores de dados do AD em Win2008 e além](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Supervisor de desempenho de servidor da Microsoft](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Pacote Advisor Active Directory

-   Pesquisa usando qualquer filtro além de "(objectClass =\*)" que usa o índice ancestrais.

### <a name="other-index-considerations"></a>Outras considerações de índice

-   Certifique-se de que a criação do índice seja a solução certa para o problema depois que o ajuste da consulta tiver sido esgotado como uma opção. O dimensionamento correto do hardware é muito importante. Os índices devem ser adicionados somente quando a correção correta é indexar o atributo, e não uma tentativa de ofuscar os problemas de hardware.

-   Os índices aumentam o tamanho do banco de dados por um mínimo do tamanho total do atributo que está sendo indexado. Portanto, uma estimativa do crescimento do banco de dados pode ser avaliada por meio da obtenção do tamanho médio do atributo e da multiplicação pelo número de objetos que terão o atributo preenchido. Geralmente, trata-se de um aumento de 1% no tamanho do banco de dados. Para obter mais informações, consulte [como funciona o armazenamento de dados](https://technet.microsoft.com/library/cc772829.aspx).

-   Se o comportamento da pesquisa for feito predominantemente no nível da unidade organizacional, considere a indexação para pesquisas em contêineres.

-   Os índices de tupla são maiores que os índices normais, mas é muito mais difícil estimar o tamanho. Use estimativas de tamanho de índices normais como o andar para crescimento, com um máximo de 20%. Para obter mais informações, consulte [como funciona o armazenamento de dados](https://technet.microsoft.com/library/cc772829.aspx).

-   Se o comportamento da pesquisa for feito predominantemente no nível da unidade organizacional, considere a indexação para pesquisas em contêineres.

-   Os índices de tupla são necessários para dar suporte a cadeias de caracteres de pesquisa e finais de pesquisa. Os índices de tupla não são necessários para as cadeias de caracteres de pesquisa iniciais.

    -   Cadeia de caracteres de pesquisa inicial – (samAccountName = MYPC\*)

    -   Cadeia de pesquisa medial-(samAccountName =\*MYPC\*)

    -   Cadeia de caracteres de pesquisa final – (samAccountName =\*MYPC $)

-   A criação de um índice irá gerar e/s de disco enquanto o índice está sendo compilado. Isso é feito em um thread em segundo plano com prioridade mais baixa e solicitações de entrada serão priorizadas sobre a compilação do índice. Se o planejamento de capacidade para o ambiente tiver sido feito corretamente, isso deverá ser transparente. No entanto, cenários de gravação intensa ou um ambiente em que a carga no armazenamento do controlador de domínio é desconhecido pode prejudicar a experiência do cliente e deve ser feito fora do horário de expediente.

-   Afeta o tráfego de replicação é mínimo, pois a criação de índices ocorre localmente.

Para obter mais informações, consulte o seguinte:

-   [Criando aplicativos mais eficientes habilitados para o Microsoft Active Directory](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Pesquisando no Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Atributos indexados](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>Consulte também

- [Ajuste de desempenho Active Directory servidores](index.md)
- [Considerações sobre hardware](hardware-considerations.md)
- [Posicionamento adequado dos controladores de domínio e considerações sobre o local](site-definition-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)