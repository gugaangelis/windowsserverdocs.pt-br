---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Atualizações de componentes dos Serviços de Diretório
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 54a2ef82d5eccabaf8be0971ca0324498e75bb78
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966408"
---
# <a name="directory-services-component-updates"></a>Atualizações de componentes dos Serviços de Diretório

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
Esta lição explica as atualizações de componentes de serviços de diretório no Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>O que você aprenderá  
Explique as seguintes novas atualizações de componentes de serviços de diretório:  
  
-   Explique as seguintes novas atualizações de componentes de serviços de diretório:  
  
    -   [Níveis funcionais de domínio e de floresta](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [NTFRS preterido](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Alterações do otimizador de consultas LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Melhorias no evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Melhoria da taxa de transferência de replicação do Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="domain-and-forest-functional-levels"></a><a name="BKMK_FL"></a>Níveis funcionais de domínio e floresta  
  
### <a name="overview"></a>Visão geral  
A seção fornece uma breve introdução às alterações no nível funcional de domínio e floresta.  
  
### <a name="new-dfl-and-ffl"></a>Novo DFL e FFL  
Com o lançamento, há novos níveis funcionais de domínio e floresta:  
  
-   Nível funcional da floresta: Windows Server 2012 R2  
  
-   Nível funcional do domínio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>O nível funcional de domínio do Windows Server 2012 R2 habilita o suporte para o seguinte:  
  
1.  Proteções no lado do DC para *usuários protegidos*  
  
    *Usuários protegidos* que se autenticam em um domínio do Windows Server 2012 R2 **não podem mais**:  
  
    -   Autenticar-se com a autenticação NTLM  
  
    -   Usar pacotes de criptografia DES ou RC4 na pré-autenticação Kerberos  
  
    -   Ser delegados com delegação restrita ou irrestrita  
  
    -   Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial de quatro horas  
  
2.  Políticas de autenticação  
  
    Novas políticas de Active Directory baseadas em floresta que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar em quais hosts uma conta pode se conectar e aplicar condições de controle de acesso para autenticação em serviços em execução como uma conta  
  
3.  Silos de política de autenticação  
  
    Novo objeto de Active Directory baseado em floresta que pode criar uma relação entre o usuário, o serviço gerenciado e as contas de computador a serem usadas para classificar contas para políticas de autenticação ou para isolamento de autenticação.  
  
Consulte como configurar contas protegidas para obter mais informações.  
  
Além dos recursos acima, o nível funcional de domínio do Windows Server 2012 R2 garante que qualquer controlador de domínio no domínio execute o Windows Server 2012 R2.  
O nível funcional de floresta do Windows Server 2012 R2 não fornece novos recursos, mas garante que qualquer novo domínio criado na floresta operará automaticamente no nível funcional de domínio do Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Mínimo de DFL imposto na nova criação de domínio  
O Windows Server 2008 DFL é o nível funcional mínimo com suporte na nova criação de domínio.  
  
> [!NOTE]  
> A reprovação do FRS é realizada removendo a capacidade de instalar um novo domínio com um nível funcional de domínio inferior ao Windows Server 2008 com Gerenciador do Servidor ou por meio do Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reduzindo os níveis funcionais de floresta e domínio  
Os níveis funcionais de floresta e domínio são definidos como Windows Server 2012 R2 por padrão no novo domínio e nova criação de floresta, mas podem ser reduzidos usando o Windows PowerShell.  
  
Para aumentar ou diminuir o nível funcional da floresta usando o Windows PowerShell, use o cmdlet **set-ADForestMode** .  
  
**Para definir o contoso.com FFL para o modo do Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Para aumentar ou diminuir o nível funcional do domínio usando o Windows PowerShell, use o cmdlet Set-ADDomainMode.  
  
**Para definir o contoso.com DFL para o modo do Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
A promoção de um DC que executa o Windows Server 2012 R2 como uma réplica adicional em um domínio existente que executa o 2003 DFL funciona.  
  
Nova criação de domínio em uma floresta existente  
  
![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>A  
Não há nenhuma nova operação de floresta ou domínio nesta versão.  
  
Esses arquivos. ldf contêm alterações de esquema para o **serviço de registro de dispositivo**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Pastas de trabalho:**  
  
1.  Sch66  
  
**MSODS**  
  
1.  Sch60  
  
**Políticas e silos de autenticação**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="deprecation-of-ntfrs"></a><a name="BKMK_NTFRS"></a>Reprovação de NTFRS  
  
### <a name="overview"></a>Visão geral  
O FRS foi preterido no Windows Server 2012 R2.  A substituição do FRS é realizada pela imposição de um DFL (nível funcional mínimo de domínio) do Windows Server 2008.  Essa imposição estará presente somente se o novo domínio for criado usando Gerenciador do Servidor ou o Windows PowerShell.  
  
Use o parâmetro-DomainMode com os cmdlets install-ADDSForest ou install-ADDSDomain para especificar o nível funcional do domínio.  Os valores com suporte para esse parâmetro podem ser um número inteiro válido ou um valor de cadeia de caracteres enumerado correspondente. Por exemplo, para definir o nível de modo de domínio como Windows Server 2008 R2, você pode especificar o valor 4 ou "Win2008R2".  Ao executar esses cmdlets do Server 2012 R2, os valores válidos incluem aqueles para Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) Windows Server 2012 (5, Win2012) e Windows Server 2012 R2 (6, Win2012R2). O nível de domínio funcional não pode ser inferior ao nível funcional da floresta, mas pode ser superior.  Como o FRS foi preterido nesta versão, o Windows Server 2003 (2, Win2003) não é um parâmetro reconhecido com esses cmdlets quando executado do Windows Server 2012 R2.  
  
![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="ldap-query-optimizer-changes"></a><a name="BKMK_LDAPQuery"></a>Alterações do otimizador de consulta LDAP  
  
### <a name="overview"></a>Visão geral  
O algoritmo do otimizador de consulta LDAP foi reavaliado e otimizado ainda mais.  O resultado é a melhoria de desempenho na eficiência da pesquisa LDAP e o tempo de pesquisa LDAP de consultas complexas.  
  
> [!NOTE]
> <strong>Do desenvolvedor:</strong>aprimoramentos no desempenho de pesquisas por meio de aprimoramentos na consulta mapeamento de LDAP para ESE.  Os filtros LDAP além de um determinado nível de complexidade impedem a seleção de índice otimizado, resultando em um desempenho drasticamente reduzido (mil vezes maior ou mais). Essa alteração altera a maneira como selecionamos índices para consultas LDAP para evitar esse problema.  
> 
> [!NOTE]
> Uma revisão completa do algoritmo do otimizador de consulta LDAP, resultando em:  
> 
> -   Tempos de pesquisa mais rápidos  
> -   Os ganhos de eficiência permitem que os DCs façam mais  
> -   Menos chamadas de suporte em relação a problemas de desempenho do AD  
> -   Back-Ported para o Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Segundo plano  
A capacidade de Pesquisar Active Directory é um serviço principal fornecido pelos controladores de domínio.  Outros serviços e aplicativos de linha de negócios dependem de Active Directory pesquisas.  As operações de negócios podem deixar de ser interrompidas se esse recurso não estiver disponível.  Como um serviço principal e muito usado, é imperativo que os controladores de domínio manipulem o tráfego de pesquisa LDAP com eficiência.  O algoritmo otimizador de consulta LDAP tenta tornar as pesquisas LDAP mais eficientes possíveis ao mapear filtros de pesquisa LDAP para um conjunto de resultados que pode ser satisfeito por meio de registros já indexados no banco de dados.  Esse algoritmo foi reavaliado e otimizado ainda mais.  O resultado é a melhoria de desempenho na eficiência da pesquisa LDAP e o tempo de pesquisa LDAP de consultas complexas.  
  
### <a name="details-of-change"></a>Detalhes da alteração  
Uma pesquisa LDAP contém:  
  
-   Um local (cabeçalho NC, UO, objeto) na hierarquia para iniciar a pesquisa  
  
-   Um filtro de pesquisa  
  
-   Uma lista de atributos a serem retornados  
  
O processo de pesquisa pode ser resumido da seguinte maneira:  
  
1.  Simplifique o filtro de pesquisa, se possível.  
  
2.  Selecione um conjunto de chaves de índice que retornará o menor conjunto coberto.  
  
3.  Execute uma ou mais interseções de chaves de índice para reduzir o conjunto coberto.  
  
4.  Para cada registro no conjunto coberto, avalie a expressão de filtro, bem como a segurança. Se o filtro for avaliado como TRUE e o acesso for concedido, retorne esse registro para o cliente.  
  
O trabalho de otimização de consulta LDAP modifica as etapas 2 e 3 para reduzir o tamanho do conjunto coberto. Mais especificamente, a implementação atual seleciona chaves de índice duplicadas e executa interseções redundantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparação entre o algoritmo antigo e o novo  
O destino da pesquisa de LDAP ineficiente neste exemplo é um controlador de domínio do Windows Server 2012.  A pesquisa é concluída em aproximadamente 44 segundos como resultado da falha ao encontrar um índice mais eficiente.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Resultados de exemplo usando o novo algoritmo  
Este exemplo repete exatamente a mesma pesquisa como acima, mas tem como alvo um controlador de domínio do Windows Server 2012 R2.  A mesma pesquisa é concluída em menos de um segundo devido às melhorias no algoritmo do otimizador de consulta LDAP.  
  
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
  
    -   Por exemplo: uma expressão na árvore estava sobre uma coluna não indexada  
  
    -   Registrar uma lista de índices que impedem a otimização  
  
    -   Exposto por meio de rastreamento ETW e ID de evento 1644  
  
        ![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="to-enable-the-stats-control-in-ldp"></a><a name="BKMK_EnableStats"></a>Para habilitar o controle Stats no LDP  
  
1.  Abra LDP.exe e conecte-se e associe-se a um controlador de domínio.  
  
2.  No menu **Opções** , clique em **controles**.  
  
3.  Na caixa de diálogo controles, expanda o menu suspenso **carregar predefinido** , clique em **Pesquisar estatísticas** e clique em **OK**.  
  
    ![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  No menu **procurar** , clique em **Pesquisar**  
  
5.  Na caixa de diálogo Pesquisar, selecione o botão **Opções** .  
  
6.  Verifique se a caixa de seleção **estendida** está marcada na caixa de diálogo opções de pesquisa e selecione **OK**.  
  
    ![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Experimente: usar o LDP para retornar estatísticas de consulta  
Execute o seguinte em um controlador de domínio ou em um cliente ou servidor ingressado no domínio que tenha as ferramentas de AD DS instaladas.  Repita o seguinte direcionamento para o seu controlador de domínio do Windows Server 2012 e seu controlador de domínio do Windows Server 2012 R2.  
  
1.  Examine o artigo ["Criando aplicativos habilitados para o Microsoft ad mais eficientes"](/previous-versions/ms808539(v=msdn.10)) e refira-o conforme necessário.  
  
2.  Usando o LDP, habilite estatísticas de pesquisa (consulte [para habilitar o controle Stats no LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Realize várias pesquisas LDAP e observe as informações estatísticas na parte superior dos resultados.  Você repetirá a mesma pesquisa em outras atividades, portanto, documente-as em um arquivo de texto do bloco de notas.  
  
4.  Executar uma pesquisa LDAP que o otimizador de consulta deve ser capaz de otimizar devido a índices de atributos  
  
5.  Tentativa de construir uma pesquisa que leva muito tempo para ser concluída (talvez você queira aumentar a opção de **limite de tempo** para que a pesquisa não tenha tempo limite).  
  
### <a name="additional-resources"></a>Recursos adicionais  
[O que são Active Directory pesquisas?](/previous-versions/windows/it-pro/windows-server-2003/cc783845(v=ws.10))  
  
[Como Active Directory pesquisas funcionam](/previous-versions/windows/it-pro/windows-server-2003/cc755809(v=ws.10))  
  
[Criando aplicativos mais eficientes habilitados para o Microsoft Active Directory](/previous-versions/ms808539(v=msdn.10))  
  
[951581](https://support.microsoft.com/kb/951581) consultas LDAP são executadas mais lentamente do que o esperado no serviço de diretório ad ou LDS/Adam e a ID de evento 1644 pode ser registrada  
  
## <a name="1644-event-improvements"></a><a name="BKMK_1644"></a>Melhorias no evento 1644  
  
### <a name="overview"></a>Visão geral  
Esta atualização adiciona estatísticas de resultados de pesquisa LDAP adicionais à ID de evento 1644 para auxiliar na solução de problemas.  Além disso, há um novo valor de registro que pode ser usado para habilitar o registro em log em um limite baseado em tempo.  Esses aprimoramentos foram disponibilizados no Windows Server 2012 e no Windows Server 2008 R2 SP1 por meio do KB [2800945](https://support.microsoft.com/kb/2800945) e serão disponibilizados para o Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Estatísticas de pesquisa LDAP adicionais são adicionadas à ID de evento 1644 para ajudar a solucionar problemas de pesquisas de LDAP ineficientes ou dispendiosas  
> -   Agora você pode especificar um limite de tempo de pesquisa (por exemplo, Log Event 1644 para pesquisas demorando mais de 100 ms) em vez de especificar os valores de limite de resultados de pesquisa dispendiosos e ineficientes  
  
### <a name="background"></a>Segundo plano  
Ao solucionar problemas de desempenho Active Directory, fica claro que a atividade de pesquisa LDAP pode estar contribuindo para o problema.  Você decide habilitar o log para que você possa ver consultas LDAP caras ou ineficientes processadas pelo controlador de domínio.  Para habilitar o registro em log, você deve definir o valor de diagnóstico de engenharia de campo e pode, opcionalmente, especificar os valores de limite de resultados de pesquisa caros/ineficientes.  Ao habilitar o nível de log de engenharia de campo para um valor de 5, qualquer pesquisa que atenda a esses critérios será registrada no log de eventos de serviços de diretório com uma ID de evento 1644.  
  
O evento contém:  
  
-   IP e porta do cliente  
  
-   Nó inicial  
  
-   Filtrar  
  
-   Escopo da pesquisa  
  
-   Seleção de atributo  
  
-   Controles de servidor  
  
-   Entradas visitadas  
  
-   Entradas retornadas  
  
No entanto, os dados de chave estão faltando no evento, como a quantidade de tempo gasto na operação de pesquisa e o que (se houver) foi usado.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Estatísticas de pesquisa adicionais adicionadas ao evento 1644  
  
-   Índices usados  
  
-   Páginas referenciadas  
  
-   Páginas lidas do disco  
  
-   Páginas lidas do disco  
  
-   Limpar páginas modificadas  
  
-   Páginas sujas modificadas  
  
-   Tempo de pesquisa  
  
-   Atributos impedindo a otimização  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Novo valor de registro de limite baseado em tempo para log do evento 1644  
Em vez de especificar os valores de limite de resultados de pesquisa dispendiosos e ineficientes, você pode especificar o limite de tempo de pesquisa.  Se você quisesse registrar em log todos os resultados da pesquisa que levaram 50 MS ou mais, você deve especificar 50 decimal/32 Hex (além de definir o valor de engenharia de campo).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparação entre a ID de evento antiga e a nova 1644  
OLD  
  
![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NEW  
  
![atualizações de serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Experimente: Use o log de eventos para retornar estatísticas de consulta  
  
1.  Repita o seguinte direcionamento para o seu controlador de domínio do Windows Server 2012 e seu controlador de domínio do Windows Server 2012 R2. Observe a ID do evento 1644s em ambos os DCs após cada pesquisa.  
  
2.  Usando o regedit, habilite o log da ID do evento 1644 usando um limite baseado em tempo no controlador de domínio do Windows Server 2012 R2 e o antigo método no Windows Server 2012 DC.  
  
3.  Realize várias pesquisas LDAP que excedem o limite e observe as informações estatísticas na parte superior dos resultados.  Use as consultas LDAP que você documentou anteriormente e repita as mesmas pesquisas.  
  
4.  Execute uma pesquisa LDAP que o otimizador de consulta não é capaz de otimizar porque um ou mais atributos não estão indexados.  
  
## <a name="active-directory-replication-throughput-improvement"></a><a name="BKMK_ADRepl"></a>Melhoria da taxa de transferência de replicação Active Directory  
  
### <a name="overview"></a>Visão geral  
A replicação do AD usa RPC para seu transporte de replicação. Por padrão, o RPC usa um buffer de transmissão de 8K e um tamanho de pacote 5K. Isso tem o efeito líquido em que a instância de envio transmitirá três pacotes (aproximadamente 15 mil dados) e precisará aguardar uma viagem de ida e volta da rede antes de enviar mais. Supondo um tempo de ida e volta de 3MS, a taxa de transferência mais alta seria cerca de 40Mbps, mesmo em redes 1 Gbps ou 10 Gbps.  
  
> [!NOTE]  
> -   Essa atualização ajusta a taxa de transferência máxima de replicação do AD de 40Mbps para cerca de 600 Mbps.  
>   
>     -   Ele aumenta o tamanho do buffer de envio RPC que reduz o número de viagens de ida e volta da rede  
> -   O efeito será mais perceptível em alta velocidade, rede de alta latência.  
  
Essas atualizações aumentam a taxa de transferência máxima para cerca de 600 Mbps, alterando o tamanho do buffer de envio RPC de 8K para 256KB.  Essa alteração permite que o tamanho da janela TCP cresça além de 8K, reduzindo o número de viagens de ida e volta da rede.  
  
> [!NOTE]  
> Não há configurações configuráveis para modificar esse comportamento.  
  
### <a name="additional-resources"></a>Recursos adicionais  
[Como funciona o modelo de replicação de Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc772726(v=ws.10))  
  
