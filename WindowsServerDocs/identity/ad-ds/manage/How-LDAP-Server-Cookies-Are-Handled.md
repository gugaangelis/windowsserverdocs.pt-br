---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: "Como os Cookies de servidor LDAP são manipulados"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>Como os Cookies de servidor LDAP são manipulados

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em LDAP, conjunto de alguns resultados de consultas em um resultado grande. Essas consultas apresentam alguns desafios para o Windows Server.  
  
Coletando e criação desses conjuntos grandes de resultado são um trabalho significativo. Muitos dos atributos precisam ser convertido de uma representação interna para a representação de conexão com fio LDAP. Para muitos atributos, uma conversão de um formato interno, muitas vezes binário, deve acontecer em um formato UTF-8 com base em texto no quadro da resposta LDAP.  
  
Outro desafio é conjuntos de resultados com dezenas de milhares de objetos muito grandes, torne-se facilmente várias centenas Mega-Bytes. Muitos desses, em seguida, exigir virtual espaço de endereço e também a transferência em rede tem problemas conforme o esforço inteiro será perdido quando a sessão TCP divide em trânsito.  
  
Esses problemas logísticas e capacidade levaram os desenvolvedores LDAP da Microsoft à criação de uma extensão LDAP conhecida como "Paginada consulta". Que está implementando um controle LDAP para separar uma consulta enorme em blocos de conjuntos menores de resultado. Ele se tornou um padrão RFC como [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Cookie manipulação no cliente  
O método de consulta paginada usa o tamanho da página seja definida pelo cliente ou por meio um [diretiva LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). O cliente sempre terá que permitir paginação enviando um controle LDAP.  

  
Ao trabalhar em uma consulta com muitos resultados, em algum momento, o número máximo de objetos permitidos é atingido. O servidor LDAP empacota a mensagem de resposta e adiciona um cookie que contém as informações necessárias para continuar mais tarde a pesquisa.  
  
O aplicativo cliente deve tratar o cookie como um blob opaco. Ele pode recuperar a contagem de objetos na resposta e pode continuar a pesquisa com base na presença do cookie. O cliente continua a pesquisa enviando a consulta para o servidor LDAP novamente com os mesmos parâmetros como objeto base e filtro e inclui o valor do cookie que foi retornado na resposta anterior.  
  
Se o número de objetos não preencher uma página, a consulta LDAP é completa e a resposta não contém nenhum cookie na página. Se nenhum cookie é retornado pelo servidor, o cliente deve considerar a pesquisa paginada seja concluído com êxito.  
  
Se um erro é retornado pelo servidor, o cliente deve considerar a pesquisa paginada seja bem-sucedida. Repetir a pesquisa resultará em reiniciar a pesquisa da primeira página.  
  
## <a name="server-side-cookie-handling"></a>Manipulação de Cookie do lado do servidor  
O Windows Server retorna o cookie para o cliente e, às vezes, armazena informações relacionadas ao cookie no servidor. Essas informações são armazenadas no servidor em um cache e estão sujeito a determinadas limites.  
  
Nesse caso, o cookie enviado ao cliente pelo servidor também é usado pelo servidor consultar as informações do cache no servidor. Quando o cliente continuará a pesquisa paginada, o Windows Server usará o cookie do cliente, bem como quaisquer informações relacionadas do cache de cookie do servidor para continuar a pesquisa. Se o servidor não conseguir encontrar informações de cookie relacionados do cache do servidor por qualquer motivo, a pesquisa será interrompida e erro é retornado para o cliente.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Como o pool de cookie é gerenciado  
Obviamente, o servidor LDAP serve mais de um cliente de cada vez, e também mais de um cliente de cada vez pode iniciar consultas que exigem o uso do cache de cookie do servidor. Portanto, a implementação do Windows Server lá é um rastreamento de uso do pool de cookie e limites são colocar em prática para que o pool de cookie não está demorando muito recursos. Os limites podem ser definidos pelo administrador usando as seguintes configurações em política LDAP. Os padrões e explicações são:  
  
**MinResultSets: 4**  
  
O servidor LDAP não irá procurar no tamanho máximo do pool discutido abaixo, se houver menos de MinResultSets entradas no cache de cookie do servidor.  
  
**MaxResultSetSize: bytes 262.144**  
  
O tamanho do cache de cookie total no servidor não deve exceder o máximo de MaxResultSetSize em bytes. Em caso afirmativo, desde a mais antiga de cookies são excluídos até que o pool for menor do que MaxResultSetSize bytes ou menor do que MinResultSets cookies estão no pool. Isso significa que o servidor LDAP usando as configurações padrão, considera um pool de 450KB para ser Okey se houver apenas 3 cookies armazenados.  
  
**MaxResultSetsPerConn: 10**  
  
O servidor LDAP permite não mais do que cookies MaxResultSetsPerConn por conexão LDAP no pool.  
  
## <a name="handling-deleted-cookies"></a>Manipulando excluído Cookies  
A remoção das informações de cookie do cache de servidor LDAP não resulta em um erro de imediato para aplicativos em todos os casos. Os aplicativos podem reiniciar a pesquisa paginada desde o início e concluí-lo em outra tentativa. Alguns aplicativos têm esse tipo de um mecanismo de repetição para adicionar robustez.  
  
Alguns aplicativos podem acessar por meio de uma pesquisa de página e nunca concluí-la. Isso pode deixar entradas no servidor LDAP cache de cookie, que é manipulado por meio do mecanismo da seção 4. Isso é essencial para liberar memória no servidor para pesquisas LDAP ativas.  
  
O que acontece quando um cookie é excluído do servidor e o cliente continuará a pesquisa com esse identificador de cookie? O servidor LDAP não encontrará o cookie no servidor de cache de cookie e retornar um erro para a consulta, a resposta de erro será semelhante a:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> O valor hexadecimal atrás "DSID" varia de acordo com a versão de compilação dos binários de servidor LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Relatórios sobre o pool de cookie  
O servidor LDAP tem a capacidade de registrar eventos por meio de categoria "16 Ldap Interface" [chave de diagnóstico NTDS](https://support.microsoft.com/kb/314980/en-us). Se você definir esta categoria para "2", você pode obter os eventos a seguir:  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2898  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.  
Maximum number of Result Sets allowed per LDAP connection:  
10  
Current number of Result Sets for this LDAP connection:  
11  
  
User Action  
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.  
  
```  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2899  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.   
  
Number of result sets currently stored:   
4   
Current Result Set Size:   
263504   
Maximum Result Set Size:   
262144   
Size of single Result Set being discarded:   
40876   
User Action   
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.  
  
```  
  
Os eventos de sinalizam que um cookie armazenado foi removido. Isso não significa que um cliente viu o erro LDAP, mas apenas que o servidor LDAP alcançou os limites de administração de cache.  Em alguns casos, um cliente LDAP pode ter abandonada a pesquisa paginada e nunca pode aparecer o erro.  
  
## <a name="monitoring-the-cookie-pool"></a>O pool de cookie de monitoramento  
Se você nunca tiver erros de pesquisa LDAP em seu domínio, você nunca precisa monitorar o pool de cookie de pesquisa do LDAP server página. Caso você veja página LDAP pesquisa erros relacionados em seu ambiente, você pode ter um problema com os limites de administrador do pool de cookie.  
  
Eventos 2898 e 2899 são a única maneira de saber que o servidor LDAP alcançou os limites de administrador. Quando você enfrentar esse erro consultas LDAP por causa do erro acima de processamento de controle, você deve examinar aumentando limites em uma ou mais das configurações de política de LDAP mencionadas na seção 4, dependendo de quais eventos você recebeu.  
  
Se você estiver vendo o evento 2898 em seu servidor DC/LDAP, recomendamos que você defina MaxResultSetsPerConn para 25. Mais de 25 pesquisas paginadas paralelas em uma única conexão LDAP não é normal. Se você continuar vendo evento 2898, considere a possibilidade de investigação de seu aplicativo de cliente LDAP que encontra o erro. A suspeita seria que ele alguma forma travar recuperando resultados paginados adicionais, deixa o cookie pendentes e reinicia uma nova consulta. Por isso, veja se o aplicativo em algum momento teria cookies suficientes para as finalidades, você também pode aumentar o valor de MaxResultSetsPerConn além 25. quando você vir eventos 2899 registrados nos controladores de domínio, o plano seria diferente. Se o servidor LDAP/DC for executado em uma máquina com memória suficiente (vários GB de memória livre), recomendamos que você defina o MaxResultsetSize no servidor LDAP para > = 250MB. Esse limite é grande o suficiente para acomodar grandes volumes de pesquisas LDAP até mesmo em diretórios muito grandes.  
  
Se você ainda está vendo eventos 2899 com um pool de 250MB ou mais, têm mais probabilidade de ter muitos clientes com um número muito alto de objetos retornados, consultada de maneira muito frequente. Os dados que você pode coletar com o [conjunto de Coletores de dados de diretório do Active](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) pode ajudam a encontrar repetitivas consultas paginadas que mantêm seus servidores LDAP ocupado. Essas consultas serão mostrada com um número de "Entradas retornadas" que corresponda ao tamanho da página usado.  
  
Se possível, você deve examinar o design do aplicativo e implementar uma abordagem diferente com uma frequência mais baixa, o volume de dados e/ou menos instâncias de cliente consultando esses dados. Em caso de aplicativos para os quais você tem acesso ao código fonte, este guia para [criando aplicativos eficientes de AD-Enabled](https://msdn.microsoft.com/en-us/library/ms808539.aspx) podem ajudá-lo a entender a maneira ideal para aplicativos acessem AD.  
  
Se o comportamento da consulta não pode ser alterado, uma abordagem é também adicionando mais replicadas instâncias dos contextos de nomeação necessários e para redistribuir os clientes e, eventualmente, reduzir a carga nos servidores LDAP individuais.  
  


