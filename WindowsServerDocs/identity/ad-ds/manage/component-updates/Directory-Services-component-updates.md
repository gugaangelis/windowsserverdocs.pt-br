---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Atualizações de componentes dos Serviços de Diretório
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f3e0553b1919a7f9129d47616d0ffb66b6ff48f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874437"
---
# <a name="directory-services-component-updates"></a>Atualizações de componentes dos Serviços de Diretório

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
Esta lição explica as atualizações de componentes de serviços de diretório no Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>O que você aprenderá  
Explique as atualizações de componentes novos serviços de diretório seguintes:  
  
-   Explique as atualizações de componentes novos serviços de diretório seguintes:  
  
    -   [Níveis funcionais de domínio e de floresta](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [NTFRS preterido](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Alterações do otimizador de consultas LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Melhorias no evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Melhoria da taxa de transferência de replicação do Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Níveis funcionais de domínio e floresta  
  
### <a name="overview"></a>Visão geral  
A seção fornece uma breve introdução para as alterações de nível funcionais floresta e domínio.  
  
### <a name="new-dfl-and-ffl"></a>FFL e DFL novo  
Com o lançamento, há novos níveis funcionais de floresta e domínio:  
  
-   Nível funcional de floresta: Windows Server 2012 R2  
  
-   Nível funcional do domínio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>O Windows Server 2012 R2 nível funcional de domínio habilita o suporte para o seguinte:  
  
1.  Proteções do lado do controlador de domínio para *usuários protegidos*  
  
    *Usuários protegidos pelo* pode fazer a autenticação em um domínio do Windows Server 2012 R2 **não é mais**:  
  
    -   Autenticar com a autenticação NTLM  
  
    -   Usar conjuntos de criptografia DES ou RC4 na pré-autenticação do Kerberos  
  
    -   Ser delegadas com delegação restrita ou irrestrita  
  
    -   Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial de 4 horas  
  
2.  Políticas de autenticação  
  
    Nova floresta do Active Directory políticas que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar quais hosts uma conta pode logon do e aplicar condições de controle de acesso para autenticação para serviços em execução como uma conta  
  
3.  Silos de política de autenticação  
  
    Objeto com base em nova floresta do Active Directory que pode criar uma relação entre um usuário, serviço gerenciado e contas de computador a ser usado para classificar as contas para as políticas de autenticação ou para o isolamento de autenticação.  
  
Veja como configurar contas protegidas para obter mais informações.  
  
Além dos recursos acima, o nível funcional de domínio do Windows Server 2012 R2 garante que qualquer controlador de domínio no domínio executa Windows Server 2012 R2.  
O nível funcional de floresta do Windows Server 2012 R2 fornece novos recursos, mas garante que todo novo domínio criado na floresta opere automaticamente no nível funcional de domínio do Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Mínimo DFL imposta na criação do novo domínio  
DFL do Windows Server 2008 é o nível funcional mínimo com suporte na criação do novo domínio.  
  
> [!NOTE]  
> A substituição do FRS é feita removendo a capacidade de instalar um novo domínio com um nível funcional de domínio menor do que o Windows Server 2008 com o Gerenciador do servidor ou por meio do Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Reduzindo os níveis funcionais de floresta e domínio  
Os níveis funcionais de floresta e domínio são definidos para o Windows Server 2012 R2 por padrão na criação do novo domínio e a nova floresta, mas podem ser reduzidos usando o Windows PowerShell.  
  
Para aumentar ou diminuir o nível funcional de floresta usando o Windows PowerShell, use o **Set-ADForestMode** cmdlet.  
  
**Para definir o contoso.com FFL para modo do Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Para aumentar ou diminuir o nível funcional de domínio usando o Windows PowerShell, use o cmdlet Set-ADDomainMode.  
  
**Para definir o DFL contoso.com para o modo do Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Promoção de um controlador de domínio executando o Windows Server 2012 R2 como uma réplica adicional em um domínio existente executando 2003 DFL funciona.  
  
Criar novo domínio em uma floresta existente  
  
![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Não há nenhuma nova floresta ou operações de domínio nesta versão.  
  
Esses arquivos. ldf contêm alterações de esquema para o **Device Registration Service**.  
  
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
  
**Silos e políticas de autenticação**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>NTFRS preterido  
  
### <a name="overview"></a>Visão geral  
FRS está preterido no Windows Server 2012 R2.  A substituição do FRS é feita através da aplicação de um nível funcional de domínio mínimo (DFL) do Windows Server 2008.  Essa aplicação está presente somente se o novo domínio é criado usando o Gerenciador do servidor ou o Windows PowerShell.  
  
Você pode usar o parâmetro - DomainMode com o Install-ADDSForest ou os cmdlets Install-ADDSDomain para especificar o nível funcional do domínio.  Valores com suporte para esse parâmetro podem ser um inteiro válido ou um valor de cadeia de caracteres enumerada correspondente. Por exemplo, para definir o nível de modo de domínio para o Windows Server 2008 R2, você pode especificar um valor de 4 ou "Win2008R2".  Ao executar esses cmdlets do Server 2012 R2 válido valores incluem aquelas para o Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) (5, Win2012) do Windows Server 2012 e Windows Server 2012 R2 (Win2012R2 6). O nível de domínio funcional não pode ser inferior ao nível funcional da floresta, mas pode ser superior.  Uma vez que o FRS está preterido nesta versão, o Windows Server 2003 (2, Win2003) não é um parâmetro reconhecido com esses cmdlets quando executado no Windows Server 2012 R2.  
  
![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Alterações do otimizador de consulta LDAP  
  
### <a name="overview"></a>Visão geral  
O algoritmo de Otimizador de consulta LDAP foi reavaliado e ainda mais otimizado.  O resultado é a melhoria de desempenho na eficiência da pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.  
  
> [!NOTE]  
> **Do desenvolvedor:** melhorias no desempenho de pesquisas por meio de melhorias no mapeamento de LDAP de consulta à consulta do ESE.  Filtros LDAP, além de um determinado nível de complexidade de impedir que a seleção de índice otimizado, resultando em desempenho reduzido drasticamente (x 1000 ou mais). Essa alteração altera a maneira na qual podemos selecionar índices para consultas LDAP evitar esse problema.  
  
> [!NOTE]  
> Uma revisão completa do algoritmo de Otimizador de consulta LDAP, resultando em:  
>   
> -   Tempos mais rápidos de pesquisa  
> -   Ganhos de eficiência permitem que os controladores de domínio fazer mais  
> -   Menos chamadas ao suporte emite sobre desempenho do AD  
> -   Transferido de volta para o Windows Server 2008 R2 (2862304 KB)  
  
### <a name="background"></a>Histórico  
A capacidade de pesquisar no Active Directory é um serviço de núcleo fornecido pelos controladores de domínio.  Outros serviços e aplicativos de negócios se baseiam em pesquisas do Active Directory.  Operações de negócios podem deixar de uma interrupção se esse recurso não está disponível.  Como um serviço de uso intensivo e core, é imperativo que controladores de domínio processam o tráfego de pesquisa LDAP com eficiência.  O algoritmo de Otimizador de consulta LDAP tenta tornar as pesquisas LDAP eficiente possível, mapeando os filtros de pesquisa LDAP para um conjunto de resultados que pode ser atendido por meio de registros já indexados no banco de dados.  Esse algoritmo foi reavaliado e ainda mais otimizado.  O resultado é a melhoria de desempenho na eficiência da pesquisa LDAP e tempo de pesquisa LDAP de consultas complexas.  
  
### <a name="details-of-change"></a>Detalhes de alteração  
Contém uma pesquisa LDAP:  
  
-   Um local (cabeçalho de NC, UO, objeto) dentro da hierarquia para começar a pesquisa  
  
-   Um filtro de pesquisa  
  
-   Uma lista de atributos a retornar  
  
O processo de pesquisa pode ser resumido da seguinte maneira:  
  
1.  Simplifique o filtro de pesquisa, se possível.  
  
2.  Selecione um conjunto de chaves de índice que retornará o menor conjunto coberto.  
  
3.  Execute interseções de uma ou mais das chaves de índice, para reduzir o conjunto coberto.  
  
4.  Para cada registro no conjunto de coberto, avalie a expressão de filtro, bem como a segurança. Se o filtro é avaliada como TRUE e o acesso é concedido, em seguida, retorne esse registro para o cliente.  
  
O trabalho de otimização de consulta LDAP modifica as etapas 2 e 3, para reduzir o tamanho do conjunto de coberto. Mais especificamente, a implementação atual seleciona as chaves de índice duplicados e executa interseções redundantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparação entre o algoritmo antigo e novo  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Resultados de exemplo usando o novo algoritmo  
Este exemplo repete a mesma pesquisa exatamente como mostrado acima, mas for destinado a um controlador de domínio do Windows Server 2012 R2.  A mesma pesquisa será concluído em menos de um segundo devido às melhorias no algoritmo de Otimizador de consulta LDAP.  
  
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
  
    -   Por exemplo: uma expressão na árvore de era sobre uma coluna não indexada  
  
    -   Registrar uma lista de índices que impedem a otimização  
  
    -   Exposta por meio de rastreamento ETW e ID do evento 1644  
  
        ![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Para habilitar o controle de estatísticas no LDP  
  
1.  Abra LDP.exe e se conectar e associar a um controlador de domínio.  
  
2.  Sobre o **opções** menu, clique em **controles**.  
  
3.  Na caixa de diálogo de controles, expanda o **Load Predefined** menu suspenso, clique em **estatísticas de pesquisa** e, em seguida, clique em **Okey**.  
  
    ![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Sobre o **navegue** menu, clique em **pesquisa**  
  
5.  Na caixa de diálogo de pesquisa, selecione a **opções** botão.  
  
6.  Verifique se o **estendido** caixa de seleção é marcada na caixa de diálogo de opções de pesquisa e selecione **Okey**.  
  
    ![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Tente o seguinte: Usar o LDP para retornar as estatísticas de consulta  
Execute o seguinte em um controlador de domínio ou de um cliente ingressado no domínio ou servidor que tenha as ferramentas de AD DS instaladas.  Repita as seguintes destinados ao seu controlador de domínio do Windows Server 2012 e o controlador de domínio do Windows Server 2012 R2.  
  
1.  Examine os ["Criando mais eficiente Microsoft aplicativos habilitados para AD"](https://msdn.microsoft.com/library/ms808539.aspx) do artigo e voltar a ela conforme necessário.  
  
2.  Usando o LDP, habilitar estatísticas de pesquisa (consulte [para habilitar o controle de estatísticas no LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Realizar várias pesquisas LDAP e observe as informações de estatísticas na parte superior dos resultados.  Repita a mesma pesquisa em outras atividades então Documente-os em um arquivo de texto do bloco de notas.  
  
4.  Executar uma pesquisa LDAP que o otimizador de consulta deve ser capaz de otimizar devido a índices de atributos  
  
5.  Tenta construir uma pesquisa que demora muito para ser concluída (talvez você queira aumentar o **limite de tempo** opção pesquisa faz atinja o tempo limite).  
  
### <a name="additional-resources"></a>Recursos adicionais  
[Quais são as pesquisas do Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Como o trabalho de pesquisas do Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Criação de aplicativos mais eficientes do Microsoft Active Directory habilitado](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) consultas LDAP sejam executadas mais lentamente do que o esperado no AD ou o serviço de diretório do LDS/ADAM e 1644 de ID de evento podem ser registrado  
  
## <a name="BKMK_1644"></a>Melhorias no evento 1644  
  
### <a name="overview"></a>Visão geral  
Essa atualização adiciona estatísticas adicionais de resultado de pesquisa LDAP para a ID do evento 1644 para auxiliar na solução de problemas.  Além disso, há um novo valor de registro que pode ser usado para habilitar o log em um limite de tempo.  Essas melhorias foram feitas disponíveis no Windows Server 2012 e Windows Server 2008 R2 SP1 por meio de KB [2800945](https://support.microsoft.com/kb/2800945) e será disponibilizado para o Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Estatísticas de pesquisa LDAP adicionais são adicionadas à ID do evento 1644 para auxiliar na solução de problemas de pesquisas LDAP ineficientes ou caras  
> -   Agora você pode especificar um limite de tempo de pesquisa (por exemplo. Evento do log 1644 para pesquisas demorando mais do que 100 ms) em vez de especificar os valores de limite de resultado de pesquisa caro e ineficiente  
  
### <a name="background"></a>Histórico  
Ao solucionar problemas de desempenho do Active Directory, ele se torna aparente que a atividade de pesquisa LDAP pode estar contribuindo para o problema.  Você decide habilitar o log para que você possa ver consultas LDAP caras ou ineficientes, processadas pelo controlador de domínio.  Para habilitar o registro em log, você deve definir o valor de diagnóstico de engenharia de campo e, opcionalmente, pode especificar os valores de limite de resultados de pesquisa caro / ineficiente.  Após habilitar a engenharia de campo nível de log para um valor de 5, uma pesquisa que atenda a esses critérios é registrada no log de eventos de serviços de diretório com uma ID do evento 1644.  
  
O evento contém:  
  
-   Cliente IP e porta  
  
-   Nó inicial  
  
-   Filtro  
  
-   Escopo da pesquisa  
  
-   Seleção de atributo  
  
-   Controles de servidor  
  
-   Entradas visitadas  
  
-   Entradas retornadas  
  
No entanto, os dados da chave estão ausentes do evento, como a quantidade de tempo gasto no operação de pesquisa e de qual (se houver) o índice foi usado.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Estatísticas de pesquisa adicional, adicionadas ao evento 1644  
  
-   Índices utilizados  
  
-   Páginas de referência  
  
-   Páginas lidas do disco  
  
-   Páginas previamente lidas do disco  
  
-   Limpas páginas modificadas  
  
-   Páginas sujas foram modificadas  
  
-   Tempo de pesquisa  
  
-   Impedindo a otimização de atributos  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Novo valor de registro com base no tempo limite para o log de evento 1644  
Em vez de especificar os valores de limite de resultado de pesquisa caro e ineficiente, você pode especificar o limite de tempo de pesquisa.  Se você para registrar todos os resultados de pesquisa que levou 50 ms ou maior, você especificaria 50 decimal / 32 hex (além de definir o valor de engenharia de campo).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparação entre o antigo e novo ID do evento 1644  
ANTIGO  
  
![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NOVO  
  
![atualizações dos serviços de diretório](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Tente o seguinte: Use o log de eventos para retornar as estatísticas de consulta  
  
1.  Repita as seguintes destinados ao seu controlador de domínio do Windows Server 2012 e o controlador de domínio do Windows Server 2012 R2. Observe o evento ID 1644s em ambos os controladores de domínio depois de cada pesquisa.  
  
2.  Usando regedit, habilite o log de ID do evento 1644 usando um limite de tempo sobre o controlador de domínio do Windows Server 2012 R2 e o método antigo sobre o controlador de domínio do Windows Server 2012.  
  
3.  Realize várias pesquisas LDAP que excederam o limite e observar as informações de estatísticas na parte superior dos resultados.  Usar as consultas LDAP que é documentado anteriormente e repita as mesmas pesquisas.  
  
4.  Execute uma pesquisa LDAP que o otimizador de consulta não é capaz de otimizar porque um ou mais atributos não são indexados.  
  
## <a name="BKMK_ADRepl"></a>Melhoria da taxa de transferência de replicação de diretório Active Directory  
  
### <a name="overview"></a>Visão geral  
Replicação do AD usa o RPC para o transporte de replicação. Por padrão, o RPC usa um buffer de transmissão de 8K e um tamanho de pacote de 5 mil. Isso tem o efeito líquido no qual a instância de envio transmitirá três pacotes (aproximadamente 15K que vale a pena de dados) e, em seguida, preciso esperar para uma rede de ida e volta antes de enviar mais. Supondo que um 3ms tempo de ida e volta, a taxa de transferência mais alta seria em torno de 40 Mbps, mesmo em 1 Gbps ou 10 Gbps redes.  
  
> [!NOTE]  
> -   Essa atualização ajusta a produtividade máxima de replicação do AD de 40 Mbps para cerca de 600 Mbps.  
>   
>     -   Ele aumenta o tamanho do buffer de envio RPC que reduz o número da rede viagens de ida e volta  
> -   O efeito será mais perceptível em alta velocidade, rede de alta latência.  
  
Essas atualizações aumentam a taxa de transferência máxima para cerca de 600 Mbps, alterando o tamanho de buffer de envio RPC de 8K para 256KB.  Essa alteração permite que o tamanho da janela TCP aumentar além de 8K, reduzindo o número de rede viagens de ida e volta.  
  
> [!NOTE]  
> Não há nenhuma configuração configurável para modificar esse comportamento.  
  
### <a name="additional-resources"></a>Recursos adicionais  
[Como funciona o modelo de replicação do Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


