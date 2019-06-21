---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: Como são tratados os Cookies de servidor LDAP
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 873953155d22bafef5b042887b22e953ff580b5c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280576"
---
# <a name="how-ldap-server-cookies-are-handled"></a>Como são tratados os Cookies de servidor LDAP

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No protocolo LDAP, algumas consultas resultam em um grande conjunto de resultados. Essas consultas apresentam alguns desafios para o Windows Server.  
  
Coletar e criar esses grandes conjuntos de resultados representa um trabalho significativo. Muitos dos atributos precisam ser convertidos de uma representação interna para a representação eletrônica do protocolo LDAP. Para muitos atributos, uma conversão de um formato interno, geralmente binário, deve acontecer em um formato de texto UTF-8 no quadro da resposta do protocolo LDAP.  
  
Outro desafio são os conjuntos de resultados com dezenas de milhares de objetos que se tornam enormes, chegando facilmente a várias centenas de megabytes. Desta forma, eles exigem muito espaço de endereço virtual e, além disso, a transferência pela rede apresenta problemas, pois todo o esforço é perdido quando a sessão TCP se perde durante o trânsito.  
  
Esses problemas de logísticas e capacidade levaram os desenvolvedores de LDAP da Microsoft para criação de uma extensão LDAP conhecida como "Consulta paginável". Ele implementa um controle de LDAP para dividir uma consulta enorme em partes menores de conjuntos de resultados. Ele se tornou um padrão RFC como [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Manipulação de cookies no cliente  
O método de consulta paginável usa o tamanho da página definida pelo cliente ou por um [política de LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). O cliente sempre precisa habilitar a paginação enviando um controle LDAP.  

  
Ao trabalhar em uma consulta com muitos resultados, eventualmente o número máximo de objetos permitidos será atingido. O servidor LDAP empacota a mensagem de resposta e adiciona um cookie que contém as informações necessárias para continuar posteriormente a pesquisa.  
  
O aplicativo cliente deve tratar o cookie como um blob opaco. Ele pode recuperar a contagem de objetos na resposta e pode continuar a pesquisa com base na presença do cookie. O cliente continua a pesquisa, enviando a consulta para o servidor LDAP novamente com os mesmos parâmetros, como objeto base e filtro, e inclui o valor do cookie que foi retornado na resposta anterior.  
  
Se o número de objetos não preencher uma página, a consulta LDAP é concluída e a resposta não contém nenhum cookie na página. Se nenhum cookie for retornado pelo servidor, o cliente deve considerar a pesquisa paginável como concluída com êxito.  
  
Se um erro for retornado pelo servidor, o cliente deve considerar a pesquisa paginável como não concluída com êxito. Repetir a pesquisa resultará no reinício da pesquisa desde a primeira página.  
  
## <a name="server-side-cookie-handling"></a>Manipulação de cookie do lado do servidor  
O Windows Server retorna o cookie para o cliente e, às vezes, armazena informações relacionadas ao cookie no servidor. Essas informações são armazenadas no servidor em um cache e estão sujeitas a determinados limites.  
  
Nesse caso, o cookie enviado ao cliente pelo servidor também é usado pelo servidor para pesquisar as informações do cache no servidor. Quando o cliente continua a pesquisa paginável, o Windows Server usará o cookie do cliente, bem como todas as informações relacionadas do cache de cookie do servidor para continuar a pesquisa. Se o servidor não puder localizar informações de cookie relacionadas no cache do servidor por qualquer motivo, a pesquisa será interrompida e será retornada ao cliente.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Como o pool de cookies é gerenciado  
Obviamente, o servidor LDAP estará atendendo a mais de um cliente por vez, e além disso mais de um cliente por vez pode iniciar consultas que exigem o uso de cache do servidor de cookie. Desta forma, na implementação do Windows Server há um controle de uso do pool de cookie e limites são colocados em prática para que o pool de cookie não use recursos demais. Os limites podem ser definidos pelo administrador usando as seguintes configurações na política de LDAP. As explicações e os padrões são:  
  
**MinResultSets: 4**  
  
O servidor LDAP não examinará o tamanho máximo do pool discutido abaixo, se houver menos do que MinResultSets entradas no cache de cookie do servidor.  
  
**MaxResultSetSize: 262.144 bytes**  
  
O tamanho do cache total de cookie no servidor não deve exceder o máximo de MaxResultSetSize em bytes. Em caso afirmativo, os cookies são excluídos começando pelos mais antigos até que o pool seja menor do que MaxResultSetSize bytes ou menor que MinResultSets cookies estejam no pool. Isso significa que o servidor LDAP usando as configurações padrão considera um pool de 450 KB como O K se houver apenas 3 cookies armazenados.  
  
**MaxResultSetsPerConn: 10**  
  
O servidor LDAP permite no máximo MaxResultSetsPerConn cookies por conexão LDAP no pool.  
  
## <a name="handling-deleted-cookies"></a>Manuseio de cookies excluídos  
A remoção das informações de cookie do cache do servidor LDAP não resulta em um erro de imediato para aplicativos em todos os casos. Os aplicativos podem reiniciar a pesquisa paginável desde o início e concluí-la em outra tentativa. Alguns aplicativos têm esse tipo de um mecanismo de repetição para maior robustez.  
  
Alguns aplicativos podem percorrer uma pesquisa de página e nunca concluí-la. Isso pode deixar as entradas no cache de cookie do servidor LDAP, que é tratado pelo mecanismo de seção 4. Isso é essencial para liberar memória no servidor para pesquisas LDAP ativas.  
  
O que acontece quando um cookie é excluído do servidor e o cliente continua a pesquisa com esse identificador de cookie? O servidor LDAP não encontrará o cookie no cache de cookie de servidor e retornará um erro para a consulta, a resposta de erro será semelhante a:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> O valor hexadecimal em "DSID" varia dependendo da versão de compilação de binários do servidor LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Relatórios sobre o pool de cookies  
O servidor LDAP tem a capacidade de registrar eventos por meio de categoria "Interface de Ldap 16" [chave de diagnóstico NTDS](https://support.microsoft.com/kb/314980/en-us). Se você definir essa categoria para "2", você pode obter os seguintes eventos:  
  
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
  
Os eventos sinalizam que um cookie armazenado foi removido. Isso NÃO significa que um cliente obteve o erro de LDAP, mas apenas que o servidor LDAP atingiu os limites de administração para o cache.  Em alguns casos, um cliente LDAP pode ter abandonado a pesquisa paginável e nunca mostrará o erro.  
  
## <a name="monitoring-the-cookie-pool"></a>Monitorando o pool de cookies  
Se você nunca tiver erros de pesquisa LDAP no seu domínio, pode nunca ser necessário monitorar o pool de cookies de pesquisa de página do servidor do LDAP. Caso receba erros relacionados à pesquisa de página de LDAP no seu ambiente, pode haver um problema com os limites de administrador do pool de cookies.  
  
Os eventos 2898 e 2899 são a única maneira de saber que o servidor LDAP atingiu os limites de administrador. Quando ver que as consultas LDAP expiram com erro devido ao erro de processamento de controle acima, você deve examinar a possibilidade de aumentar os limites de uma ou mais das configurações de política de LDAP mencionadas na seção 4, dependendo de qual evento você está recebendo.  
  
Se você estiver vendo o evento 2898 no servidor DC/LDAP, é recomendável definir MaxResultSetsPerConn para 25. Mais de 25 pesquisas pagináveis paralelas em uma única conexão LDAP não é algo comum. Se você continuar vendo eventos 2898, investigue o aplicativo de cliente LDAP que encontra o erro. A suspeita seria que ele de alguma forma travou ao recuperar resultados paginados adicionais, deixando o cookie pendente e reiniciando uma nova consulta. Para ver se o aplicativo em algum momento teria cookies suficientes para suas finalidades, você também pode aumentar o valor de MaxResultSetsPerConn para acima de 25. Ao ver eventos 2899 registrados nos controladores de domínio, o plano deve ser diferente. Se o servidor LDAP/DC é executado em um computador com memória suficiente (vários GBs de memória livre), recomendamos que você defina o MaxResultsetSize no servidor LDAP para > = 250 MB. Esse limite é grande o suficiente para acomodar grandes volumes de pesquisas de página LDAP mesmo em diretórios muito grandes.  
  
Se você ainda estiver vendo eventos 2899 com um pool de 250 MB ou mais, é provável que muitos clientes tenham um número muito alto de objetos retornados, consultados com muita frequência. Os dados que você pode obter com o [Conjunto de coletores de dados de diretório ativo](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) podem ajudar a localizar consultas paginadas repetitivas que mantêm seus servidores LDAP ocupados. Essas consultas serão mostrados com um número de "Entradas retornadas" que corresponde ao tamanho da página usado.  
  
Se possível, você deve examinar o design do aplicativo e implementar uma abordagem diferente com uma frequência menor, o volume de dados e/ou menos instâncias de cliente consultando esses dados. No caso de aplicativos para os quais você tem acesso ao código fonte, este guia para [criando aplicativos eficientes de AD-Enabled](https://msdn.microsoft.com/library/ms808539.aspx) pode ajudá-lo a compreender a maneira ideal para aplicativos acessarem o AD.  
  
Se o comportamento da consulta não pode ser alterado, uma abordagem é também adicionando mais instâncias replicadas dos contextos de nomeação necessários para redistribuir os clientes e, eventualmente, reduzir a carga nos servidores LDAP individuais.  
  


