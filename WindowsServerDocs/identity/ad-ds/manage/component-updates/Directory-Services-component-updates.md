---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: "Atualizações de componentes de serviços de diretório"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>Atualizações de componentes de serviços de diretório

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
Nesta lição explica as atualizações do componente de serviços de diretório no Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>O que você aprenderá  
Explique as seguintes novas atualizações de componentes de serviços de diretório:  
  
-   Explique as seguintes novas atualizações de componentes de serviços de diretório:  
  
    -   [Níveis funcionais de domínio e da floresta](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Substituição de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Alterações do otimizador de consulta LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Melhorias de evento da falha 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Melhoria de taxa de transferência Active Directory replicação](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Níveis funcionais de domínio e da floresta  
  
### <a name="overview"></a>Visão geral  
A seção fornece uma breve introdução com as alterações de nível funcionais floresta e domínio.  
  
### <a name="new-dfl-and-ffl"></a>FFL e DFL novo  
Com o lançamento, há novos níveis funcionais de floresta e domínio:  
  
-   Nível funcional da floresta: Windows Server 2012 R2  
  
-   Nível funcional do domínio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>O Windows Server 2012 R2 nível funcional do domínio habilita o suporte para o seguinte:  
  
1.  Proteções de DC voltada para *usuários protegido*  
  
    *Protegido por usuários* autenticação em um domínio do Windows Server 2012 R2 pode **não é mais**:  
  
    -   Autenticar com a autenticação NTLM  
  
    -   Usar pacotes de codificação DES ou RC4 na pré-autenticação Kerberos  
  
    -   Ser recebido com a delegação restrita ou irrestrita  
  
    -   Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial 4 horas  
  
2.  Políticas de autenticação  
  
    Nova floresta do Active Directory políticas que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar quais hosts uma conta pode logon do e aplicar condições de controle de acesso para autenticação para os serviços executados como uma conta  
  
3.  Silos de política de autenticação  
  
    Objeto baseado em nova floresta do Active Directory que pode criar um relacionamento entre o usuário, serviço gerenciado e contas de computador a ser usada para classificar contas para políticas de autenticação ou para isolamento de autenticação.  
  
Veja como configurar contas protegido para obter mais informações.  
  
Além dos recursos acima, o nível funcional de domínio do Windows Server 2012 R2 garante que qualquer controlador de domínio no domínio seja executado em Windows Server 2012 R2.  
O nível funcional da floresta Windows Server 2012 R2 não fornece os novos recursos, mas garante que qualquer novo domínio criado na floresta automaticamente funcionará no nível funcional de domínio do Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Mínimo DFL imposta em criar novo domínio  
Windows Server 2008 DFL é o nível funcional mínimo com suporte em criar novo domínio.  
  
> [!NOTE]  
> A substituição de FRS é feita, removendo a capacidade de instalar um novo domínio com um nível funcional do domínio menor que o Windows Server 2008 com o Gerenciador do servidor ou por meio do Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reduzindo os níveis funcionais de floresta e domínio  
Os níveis funcionais de floresta e domínio são definidos para o Windows Server 2012 R2 por padrão na criação de domínio e nova floresta novo, mas podem ser reduzidos usando o Windows PowerShell.  
  
Para aumentar ou diminuir o nível funcional da floresta usando o Windows PowerShell, use o **conjunto ADForestMode** cmdlet.  
  
**Para definir o contoso.com FFL para o modo do Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Para aumentar ou diminuir o nível funcional do domínio usando o Windows PowerShell, use o cmdlet Set-ADDomainMode.  
  
**Para definir o contoso.com DFL para o modo do Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Promoção de um controlador de domínio que executam o Windows Server 2012 R2 como uma réplica adicional em um domínio existente executando DFL 2003 funciona.  
  
Criar novo domínio em uma floresta existente  
  
![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Não há nenhuma nova floresta ou operações de domínio nesta versão.  
  
Esses arquivos. ldf contêm alterações no esquema para o **serviço de registro de dispositivo**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Pastas de trabalho:**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**Políticas de autenticação e silos**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Substituição de NTFRS  
  
### <a name="overview"></a>Visão geral  
FRS foi preterido no Windows Server 2012 R2.  A substituição de FRS é feita através da aplicação de um nível funcional do domínio mínimo (DFL) do Windows Server 2008.  Essa imposição está presente somente se o novo domínio é criado usando o Gerenciador do servidor ou do Windows PowerShell.  
  
Você pode usar o parâmetro - DomainMode com os cmdlets Install-ADDSForest ou Install-ADDSDomain para especificar o nível funcional do domínio.  Valores com suporte para esse parâmetro podem ser um número inteiro válido ou um valor de cadeia de caracteres enumerada correspondente. Por exemplo, para definir o nível de modo de domínio para o Windows Server 2008 R2, você pode especificar um valor de 4 ou "Win2008R2".  Durante a execução desses cmdlets do Server 2012 R2 válido valores incluem as apresentadas para o Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) (5, Win2012) do Windows Server 2012 e Windows Server 2012 R2 (6, Win2012R2). O nível funcional do domínio não pode ser menor que o nível funcional da floresta, mas pode ser maior.  Desde que FRS é substituído nesta versão, o Windows Server 2003 (2, Win2003) não é um parâmetro reconhecido com esses cmdlets quando executado a partir do Windows Server 2012 R2.  
  
![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Alterações do otimizador de consulta LDAP  
  
### <a name="overview"></a>Visão geral  
O algoritmo do otimizador de consulta LDAP foi reavaliado e ainda mais otimizado.  O resultado é o aprimoramento de desempenho na eficiência da pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.  
  
> [!NOTE]  
> **Do desenvolvedor:**melhorias no desempenho de pesquisas por meio de melhorias no mapeamento do LDAP consultar a consulta ESE.  Filtros LDAP além de um determinado nível de complexidade evitar a seleção de índice otimizado, resultando em drasticamente uma redução de desempenho (1000 x ou mais). Essa alteração altera a maneira na qual podemos selecionar índices de consultas LDAP para evitar esse problema.  
  
> [!NOTE]  
> Uma revisão completa do algoritmo do otimizador de consulta LDAP, resultando em:  
>   
> -   Tempos de pesquisa  
> -   Ganhos de eficiência permitem que os controladores de domínio fazer mais  
> -   Menos chamadas de suporte sobre desempenho do anúncio problemas  
> -   Voltar adaptado para o Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Em segundo plano  
A capacidade de pesquisar no Active Directory é um serviço de core fornecido pelos controladores de domínio.  Outros serviços e aplicativos linha de negócios dependem de pesquisas do Active Directory.  Operações de negócios podem rescindir interromper se esse recurso não está disponível.  Como core e uso intensivo de serviço, é importante que os controladores de domínio manipulam o tráfego de pesquisa LDAP com eficiência.  O algoritmo do otimizador de consulta LDAP tenta fazer pesquisas LDAP eficiente quanto possível por meio do mapeamento filtros de pesquisa LDAP para um conjunto de resultados que pode ser atendido por meio de registros já indexados no banco de dados.  Esse algoritmo foi reavaliado e ainda mais otimizado.  O resultado é o aprimoramento de desempenho na eficiência da pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.  
  
### <a name="details-of-change"></a>Detalhes de alteração  
Uma pesquisa LDAP contém:  
  
-   Um local (head de NC, unidade Organizacional, Object) dentro da hierarquia para iniciar a pesquisa  
  
-   Um filtro de pesquisa  
  
-   Uma lista de atributos para retornar  
  
O processo de pesquisa pode ser resumido da seguinte maneira:  
  
1.  Simplificar o filtro de pesquisa se possível.  
  
2.  Selecione um conjunto de chaves de índice que retornará o menor conjunto coberto.  
  
3.  Execute interseções uma ou mais chaves de índice, para reduzir o conjunto de coberto.  
  
4.  Para cada registro no conjunto de cobertura, avalie a expressão de filtro, bem como a segurança. Se o filtro é avaliada como TRUE e o acesso é concedido, em seguida, retorne esse registro ao cliente.  
  
O trabalho de otimização de consulta LDAP modifica as etapas 2 e 3, para reduzir o tamanho do conjunto de cobertura. Mais especificamente, a implementação atual seleciona chaves de índice duplicadas e executa interseções redundantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparação entre o antigo e novo algoritmo  
O destino da pesquisa LDAP ineficiente neste exemplo é um controlador de domínio do Windows Server 2012.  A pesquisa é concluída em aproximadamente 44 segundos como resultado de falha ao encontrar um índice mais eficiente.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt  
  
Using server: WINSRV-DC1.blue.contoso.com:389  
  
<removed search results>  
  
Statistics  
=====  
Elapsed Time: 44640 (ms)  
Returned 324 entries of 553896 visited - (0.06%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 DNT_index:516615:N  
  
Pages Referenced          : 4619650  
Pages Read From Disk      : 973  
Pages Pre-read From Disk  : 180898  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
### <a name="sample-results-using-the-new-algorithm"></a>Exemplo de resultados usando o algoritmo de novo  
Este exemplo repete a mesma pesquisa exata acima, mas direcionado para um controlador de domínio do Windows Server 2012 R2.  A mesma pesquisa concluída em menos de um segundo devido as melhorias no algoritmo do otimizador de consulta LDAP.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt  
  
Using server: winblueDC1.blue.contoso.com:389  
  
.<removed search results>  
  
Statistics  
=====  
Elapsed Time: 672 (ms)  
Returned 324 entries of 648 visited - (50.00%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 idx_userPrincipalName:648:N  
 idx_postalCode:323:N  
 idx_cn:1:N  
  
Pages Referenced          : 15350  
Pages Read From Disk      : 176  
Pages Pre-read From Disk  : 2  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
-   Se não for possível otimizar a árvore:  
  
    -   Por exemplo: uma expressão na árvore foi sobre uma coluna não indexada  
  
    -   Registrar uma lista de índices que impedem a otimização  
  
    -   Expostos por meio de rastreamento ETW e evento ID da falha 1644  
  
        ![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Para habilitar o controle de estatísticas em LDP  
  
1.  Abra LDP.exe e se conectar e associar a um controlador de domínio.  
  
2.  Sobre o **opções** menu, clique em **controles**.  
  
3.  Na caixa de diálogo de controles, expanda o **Load predefinidas** menu suspenso, clique em **estatísticas de pesquisa** e, em seguida, clique em **Okey**.  
  
    ![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Sobre o **procurar** menu, clique em **pesquisa**  
  
5.  Na caixa de diálogo de pesquisa, selecione o **opções** botão.  
  
6.  Certifique-se a **estendido** caixa de seleção está marcada na caixa de diálogo de opções de pesquisa e selecione **Okey**.  
  
    ![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Tente o seguinte: Use LDP para retornar as estatísticas de consulta  
Faça o seguinte em um controlador de domínio ou de um domínio cliente ou servidor que tenha as ferramentas do AD DS instaladas.  Repita as seguintes direcionando seu controlador de domínio do Windows Server 2012 e o controlador de domínio do Windows Server 2012 R2.  
  
1.  Reveja o ["Criando mais eficiente Microsoft aplicativos habilitados para AD"](https://msdn.microsoft.com/library/ms808539.aspx) do artigo e voltar a ele conforme necessário.  
  
2.  Usando o LDP, habilitar estatísticas de pesquisa (veja [para habilitar o controle de estatísticas em LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Realizar várias pesquisas LDAP e observar as informações estatísticas na parte superior dos resultados.  Repita a mesma pesquisa em outros atividades então documentá-los em um arquivo de texto de bloco de notas.  
  
4.  Realizar uma pesquisa LDAP o otimizador deve ser capaz de otimizar por causa de índices de atributos  
  
5.  Tentar construir uma pesquisa que leva muito tempo para ser concluída (talvez você queira aumentar o **limite de tempo** opção para que a pesquisa faz atingirá o tempo limite).  
  
### <a name="additional-resources"></a>Recursos adicionais  
[O que são pesquisas do Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Como o trabalho de pesquisas do Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Criação de aplicativos mais eficientes do Microsoft Active Directory habilitado](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) consultas LDAP são executadas mais lentamente que o esperado no AD ou serviço de diretório LDS/ADAM e da falha 1644 ID de evento podem ser registrado  
  
## <a name="BKMK_1644"></a>Melhorias de evento da falha 1644  
  
### <a name="overview"></a>Visão geral  
Esta atualização adiciona estatísticas de resultados de pesquisa LDAP adicionais da falha 1644 ID de evento para ajudar na solução de problemas.  Além disso, há um novo valor de registro que pode ser usado para ativar o registro em um limite de tempo.  Essas melhorias foram feitas disponíveis no Windows Server 2012 e Windows Server 2008 R2 SP1 via KB [2800945](https://support.microsoft.com/kb/2800945) e estarão disponíveis para Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Estatísticas de pesquisa LDAP adicionais são adicionadas ao evento da falha 1644 ID para ajudar na solução ineficientes ou caras pesquisas LDAP  
> -   Agora você pode especificar um limite de tempo de pesquisa (ex.: Evento de log da falha 1644 para pesquisas levando mais de 100 ms) em vez de especificar os valores de limite de resultados de pesquisa caro e ineficiente  
  
### <a name="background"></a>Em segundo plano  
Durante a solução de problemas de desempenho do Active Directory, fica evidente que a atividade de pesquisa LDAP pode estar contribuindo para o problema.  Você decidir ativar registro em log para que você possa ver caras ou ineficientes consultas LDAP processadas pelo controlador de domínio.  Para habilitar o registro em log, você deve definir o valor de diagnóstico campo engenharia e, opcionalmente, pode especificar os valores de limite de resultados de pesquisa caro / ineficiente.  Após a habilitação do campo de engenharia nível de log para um valor de 5, qualquer pesquisa que atenda a esses critérios é registrada no log de eventos de serviços de diretório com um evento ID da falha 1644.  
  
O evento contém:  
  
-   Cliente de IP e porta  
  
-   A partir do nó  
  
-   Filtro  
  
-   Escopo da pesquisa  
  
-   Seleção de atributo  
  
-   Controles de servidor  
  
-   Entradas visitadas  
  
-   Entradas retornadas  
  
No entanto, os dados da chave estão ausentes do evento, como a quantidade de tempo gasto na operação de pesquisa e o que (se houver) índice foi usado.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Estatísticas de pesquisa adicionais adicionadas ao evento da falha 1644  
  
-   Índices usados  
  
-   Páginas de referência  
  
-   Páginas lidas do disco  
  
-   Páginas previamente lidas do disco  
  
-   Limpas páginas modificadas  
  
-   Coisas obscenas páginas modificadas  
  
-   Tempo de pesquisa  
  
-   Atributos impedindo otimização  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Novo valor de registro baseadas em tempo limite para o log de eventos da falha 1644  
Em vez de especificar os valores de limite de resultados de pesquisa caro e ineficiente, você pode especificar o limite de tempo de pesquisa.  Se você desejasse para registrar todos os resultados de pesquisa que leva 50 ms ou superior, você especificaria decimal 50 / 32 hex (além de definir o valor de campo engenharia).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparação do evento antigo e o novo ID da falha 1644  
ANTIGO  
  
![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
Novo  
  
![Atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Tente o seguinte: Usar o log de eventos para retornar as estatísticas de consulta  
  
1.  Repita as seguintes direcionando seu controlador de domínio do Windows Server 2012 e o controlador de domínio do Windows Server 2012 R2. Observe o evento 1644s ID em ambos os controladores de domínio após cada pesquisa.  
  
2.  Usar regedit, habilite o log de eventos da falha 1644 ID usando um limite de tempo no controlador de domínio do Windows Server 2012 R2 e o método antigo no controlador de domínio do Windows Server 2012.  
  
3.  Realize várias pesquisas LDAP que excedem o limite e observem as informações estatísticas na parte superior dos resultados.  Utilize as consultas LDAP que você anteriores documentado e repita as pesquisas mesmo.  
  
4.  Realizar uma pesquisa LDAP que o otimizador não é capaz de otimizar porque um ou mais atributos não são indexados.  
  
## <a name="BKMK_ADRepl"></a>Melhoria de taxa de transferência Active Directory replicação  
  
### <a name="overview"></a>Visão geral  
Replicação do AD usa RPC para o transporte de replicação. Por padrão, o RPC usa um buffer de transmissão 8K e um tamanho de pacote de 5 KB. Isso tem o efeito onde a instância de envio será transmitir três pacotes (aproximadamente 15K vale a pena de dados) e, em seguida, tenha de esperar uma rede ida antes de enviar mais. Pressupondo um 3 MS tempo de resposta, a mais alta taxa de transferência seria ao redor 40Mbps, até mesmo em 1Gbps ou 10 Gbps redes.  
  
> [!NOTE]  
> -   Esta atualização se ajusta a replicação do AD taxa de transferência máxima de 40Mbps para cerca de 600 Mbps.  
>   
>     -   Ela aumenta o tamanho do buffer de envio RPC que reduz o número de rede idas e vindas  
> -   O efeito será mais perceptível em alta velocidade, rede de alta latência.  
  
Essas atualizações aumentam a taxa de transferência máxima para cerca de 600 Mbps alterando o tamanho de buffer de envio RPC de 8K para 256KB.  Essa alteração permite que o tamanho da janela TCP ultrapasse a 8K, processamentos de reduzir o número de rede.  
  
> [!NOTE]  
> Não há nenhum definições configuráveis para modificar esse comportamento.  
  
### <a name="additional-resources"></a>Recursos adicionais  
[Como funciona o modelo de replicação do Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


