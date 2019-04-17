---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: "Solução de problemas de replicação do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solução de problemas de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Problemas de replicação do Active Directory podem ter várias fontes diferentes. Por exemplo, problemas, problemas de rede ou problemas de segurança do sistema de nome de domínio (DNS) podem ocasionar replicação do Active Directory falhar. 

O restante deste tópico explica ferramentas e uma metodologia geral para corrigir erros de replicação do Active Directory. Para um laboratório prático que demonstra como solucionar problemas de replicação do Active Directory, consulte [laboratório Virtual da TechNet: solução de problemas de erros de replicação de diretório ativo](https://go.microsoft.com/?linkid=9844718)

Os seguintes subtópicos abrangem sintomas, as causas e como solucionar erros de replicação específico:
   
[Corrigir problemas de objeto remanescente da replicação (IDs de evento 1388, 1988, 2042)](https://technet.microsoft.com/library/cc949124.aspx)
  
[Corrigindo problemas de replicação de segurança](https://technet.microsoft.com/library/cc949132.aspx)

[Corrigindo problemas de pesquisa de DNS replicação (IDs de evento 1925, 2087, 2088)](https://technet.microsoft.com/library/cc949133.aspx)
  
[Corrigindo problemas de replicação de conectividade (ID de evento 1925)](https://technet.microsoft.com/library/cc949131.aspx)
  
[Corrigindo problemas de replicação de topologia (ID de evento 1311)](https://technet.microsoft.com/library/cc949129.aspx)
  
[Verifique se a funcionalidade DNS para dar suporte a replicação de diretório](https://technet.microsoft.com/library/dd728017.aspx)
  
[Erro de replicação 8614 o Active Directory não pode replicar com este servidor porque o tempo desde a última replicação com este servidor excedeu o tempo de desativação](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[Operação de 8524 DSA a replicação erro não for capaz de continuar devido a uma falha de pesquisa DNS](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[Erro de replicação 8456 ou 8457 a fonte | servidor de destino está rejeitando pedidos de replicação](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[Replicação erro 8453 replicação acesso foi negado](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[Erros de replicação 8452 o contexto de nomenclatura está sendo removido ou não é replicado do servidor especificado](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[Erro de replicação 5 acesso negado](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[Erro de replicação-2146893022 o nome da entidade de destino está incorreto](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[Erro de replicação 1753 lá não são nenhum ponto de extremidade mais disponível o mapeador](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[Replicação erro 1722 o servidor RPC não está disponível](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[Erro de replicação 1396 Logon falha no nome da conta de destino está incorreta](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[Erro de replicação 1256 o sistema remoto não está disponível](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[Erro de replicação 1127 acesso ao disco rígido, uma operação de disco falha mesmo após várias tentativas](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[Erro de replicação 8451 a operação de replicação encontrou um erro de banco de dados](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[Replicação erro 8606 insuficiente atributos foram dada para criar um objeto](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introdução e recursos para solução de problemas de replicação do Active Directory

Entrada ou falha na replicação saída faz com que os objetos do Active Directory que representam a topologia de replicação, agenda de replicação, controladores de domínio, os usuários, computadores, senhas, grupos de segurança, associações de grupo e política de grupo para ser inconsistente entre controladores de domínio. Falha de inconsistência e replicação do diretório causar falhas operacionais ou resultados inconsistentes, dependendo do controlador de domínio é contatado para a operação e pode impedir que o aplicativo de política de grupo e permissões de controle de acesso. Os serviços de domínio do Active Directory (AD DS) depende de conectividade de rede, resolução de nomes, autenticação e autorização, o banco de dados de diretório, a topologia de replicação e o mecanismo de replicação. Quando a causa raiz de um problema de replicação não é imediatamente óbvia, determinar a causa entre o número de causas possíveis requer sistemática eliminação das causas mais provável. 

Para uma ferramenta baseada na interface do usuário ajudar a monitorar a replicação e diagnosticar erros, consulte [ferramenta de Status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005). Há também uma [laboratório prático](https://go.microsoft.com/?linkid=9844718) que demonstra como usar o Active Status de replicação de pasta e outras ferramentas para solucionar problemas de erros. 

Para um documento abrangente que descreve como você pode usar a ferramenta Repadmin para solucionar problemas do Active Directory replicação está disponível. consulte [monitoramento e solução de problemas Active Directory replicação usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obter informações sobre como funciona a replicação do Active Directory, consulte as seguintes referências técnicas:


- [Referência técnica do modelo do Active Directory replicação](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referência técnica da topologia de replicação do Diretor de ativo](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recomendações de solução de evento e a ferramenta

Idealmente, o vermelho (erro) e amarelos eventos (aviso) no log de eventos de serviço de diretório sugerem a restrição específica que está causando falha na replicação no controlador de domínio de origem ou de destino. Se a mensagem de evento sugere etapas para uma solução, tente as etapas descritas no evento. A ferramenta Repadmin e outras ferramentas de diagnóstico também fornecem informações que podem ajudá-lo a resolver falhas de replicação. 

Para obter informações detalhadas sobre como usar Repadmin para solucionar problemas de replicação, consulte [monitoramento e solução de problemas Active Directory replicação usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Dispensando interrupções intencionais ou falhas de hardware

Às vezes, ocorrer erros de replicação devido a interrupções intencionais. Por exemplo, ao solucionar problemas de replicação do Active Directory, descartar desconexões intencionais e falhas de hardware ou atualizações pela primeira vez.

### <a name="intentional-disconnections"></a>Desconexões intencionais

Se os erros de replicação são relatados por um controlador de domínio que está tentando replicação com um controlador de domínio que foi criado em um site de preparo e está offline aguardando sua implantação no site do final de produção (um site remoto, como uma filial), você pode considerar esses erros de replicação. Para evitar separar um controlador de domínio da topologia de replicação por períodos prolongados, que faz com que os erros contínuos até que o controlador de domínio é reconectado, considere adicionar esses computadores inicialmente como servidores membro e usando a instalação a partir do método de mídia (IFM) para instalar os serviços de domínio do Active Directory (AD DS). Você pode usar a ferramenta de linha de comando Ntdsutil para criar mídia de instalação que você pode armazenar em uma mídia removível (CD, DVD ou outra mídia) e enviar para o site de destino. Em seguida, você pode usar a mídia de instalação para instalar o AD DS nos controladores de domínio no site, sem o uso de replicação. 

### <a name="hardware-failures-or-upgradestitle"></a>Falhas de hardware ou atualizações</title>

Se ocorrerem problemas de replicação como resultado de falha de hardware (por exemplo, uma falha de uma placa-mãe, o subsistema de disco ou o disco rígido), notifica o proprietário do servidor para que o problema de hardware pode ser resolvido.

Atualizações periódicas de hardware também podem causar controladores de domínio ser fora de serviço. Certifique-se de que seus proprietários de servidor têm um sistema BOM essas paradas para comunicar antecipadamente.

### <a name="firewall-configuration"></a>Configuração do firewall

Por padrão, chamadas de procedimento remoto de replicação do Active Directory (RPCs) dinamicamente ocorrerem em uma porta disponível por meio de mapeador de ponto de extremidade de RPC (RPCSS) na porta 135. Certifique-se de que o Firewall do Windows com segurança avançada e em outros firewalls estão configuradas corretamente para permitir para replicação. Para obter informações sobre como especificar a porta para replicação do Active Directory e configurações de porta, consulte [artigo na Base de Conhecimento Microsoft 224196](https://go.microsoft.com/fwlink/?LinkId=22578). 

Para obter informações sobre as portas que usa replicação do Active Directory, consulte [ferramentas de replicação do Active Directory e configurações](https://go.microsoft.com/fwlink/?LinkId=123774). 

Para obter informações sobre o gerenciamento de replicação do Active Directory sobre firewalls, consulte [replicação do Active Directory sobre Firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Respondendo a falha de um servidor desatualizado executando o Windows 2000 Server

Se um controlador de domínio que executam o Windows 2000 Server falhou por mais tempo do que o número de dias em que o tempo de desativação, a solução é sempre o mesmo: 

1. Mova o servidor da rede corporativa a uma rede privada.
2. Obrigatoriamente remova do Active Directory ou reinstalar o sistema operacional.
3. Remova os metadados de servidor do Active Directory para que o objeto de servidor não pode ser reativado. 

Você pode usar um script para limpar os metadados do servidor na maioria dos sistemas operacionais Windows. Para obter informações sobre como usar esse script, consulte [remover Active Directory domínio controlador metadados](https://go.microsoft.com/fwlink/?LinkID=123599). 

Por padrão, os objetos de configurações NTDS que são excluídos são reativados automaticamente por um período de 14 dias. Portanto, se você não remover os metadados de servidor (use Ntdsutil ou o script mencionado anteriormente para executar a limpeza de metadados), os metadados do servidor é restabelecido no diretório, que solicita a replicação tentativas de ocorrer. Nesse caso, erros serão registrados persistentemente devido a incapacidade de replicar com o controlador de domínio ausente.

## <a name="root-causes"></a>Causas raiz

Se você descarta desconexões intencionais, falhas de hardware e desatualizados controladores de domínio do Windows 2000, o restante dos problemas de replicação quase sempre tem uma das seguintes causas raiz: 


- Conectividade de rede: A conexão de rede talvez não esteja disponível, ou as configurações de rede não estão configuradas corretamente.
- Resolução de nomes: configurações de DNS incorretas são uma causa comum de falhas de replicação.
- Autenticação e autorização: problemas de autenticação e autorização causam erros de "Acesso negado" quando um controlador de domínio tenta se conectar ao seu parceiro de replicação.
- Banco de dados do diretório (loja): O banco de dados do diretório não pode ser capaz de processar transações rápido o suficiente para acompanhar o tempo limite de replicação.
- Mecanismo de replicação: se agendas de replicação entre sites são muito curtas, filas de replicação podem ser grandes demais para processar o tempo que é exigido pelo agendamento de duplicação de saída. Nesse caso, a replicação de algumas alterações pode ser indefinitelypotentially travada, grande o suficiente para exceder o tempo de desativação.
- Topologia de replicação: controladores de domínio devem ter links entre sites no AD DS que correspondem às conexões de rede virtual privada (VPN) ou real longa distância WAN (rede). Se você criar objetos no AD DS para a topologia de replicação que não são compatíveis com a topologia de site real da rede, falha na replicação que exige a topologia configurado incorretamente.

## <a name="general-approach-to-fixing-problems"></a>Abordagem geral para corrigir problemas

Use a seguinte abordagem geral para corrigir problemas de replicação: 


1. Monitorar a integridade da replicação diariamente, ou use Repadmin.exe para recuperar o status da replicação diariamente.
2. Tente resolver qualquer falha informada em tempo hábil usando os métodos descritos em mensagens de evento e neste guia. Se o software pode estar causando o problema, desinstale o software antes de continuar com outras soluções.
3. Se não puder ser resolvido o problema que está causando a replicação falhar por qualquer métodos conhecidos, remover o AD DS do servidor e reinstale o AD DS. Para obter mais informações sobre a reinstalação do AD DS, consulte [encerrar um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se o AD DS não podem ser removidos normalmente enquanto o servidor está conectado à rede, use um dos métodos a seguir para resolver o problema:
 

    - Forçar a remoção do AD DS no diretório serviços de modo de restauração (DSRM), limpar metadados de servidor, e, em seguida, reinstale o AD DS.
    - Reinstalar o sistema operacional e recriar o controlador de domínio.

Para obter mais informações sobre como forçar a remoção do AD DS, consulte [forçando a remoção de um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Usando Repadmin para recuperar o status da replicação</title>

Status de replicação é uma maneira importante para você a avaliar o status do serviço de diretório. Se a replicação está funcionando sem erros, você sabe os controladores de domínio que estão online. Você também sabe que os seguintes sistemas e serviços estão funcionando:


- Infraestrutura DNS
- Protocolo de autenticação Kerberos
- Serviço de tempo do Windows (W32time)
- Chamada de procedimento remoto (RPC)
- Conectividade de rede

Use Repadmin para monitorar o status de replicação diariamente ao executar um comando que avalia o status de replicação de todos os controladores de domínio na floresta. O procedimento gera um arquivo. csv que você pode abrir no Microsoft Excel e filtrar por falhas de replicação.

Você pode usar o procedimento a seguir para recuperar o status de replicação de todos os controladores de domínio na floresta. 
      
Requisitos
      
A associação ao grupo **administradores corporativos**, ou equivalente, é o requisito mínimo para concluir este procedimento. 

Ferramentas: 


- Repadmin.exe 
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para gerar uma planilha de /showrepl repadmin para controladores de domínio



1. Abra um Prompt de comando como administrador: no menu Iniciar, clique com botão direito Prompt de comando e clique em Executar como administrador. Se a caixa de diálogo User Account Control for exibida, forneça credenciais de administrador corporativo, se necessário e, em seguida, clique em Continue. 
2. No prompt de comando, digite o seguinte comando e pressione ENTER:`repadmin /showrepl * /csv &gt;showrepl.csv`
3. Abra o Excel.
4. Clique no botão do Office, clique em Abrir, navegue até Showrepl e, em seguida, clique em Abrir.
5. Ocultar ou excluir uma coluna, bem como a coluna de tipo de transporte, da seguinte maneira:
6. Selecione uma coluna que você deseja ocultar ou excluir.
      

    - Para ocultar a coluna, clique com botão direito a coluna e, em seguida, clique em Ocultar.
    - Para excluir a coluna, clique com botão direito a coluna selecionada e clique em Excluir.
7. Selecione a linha 1 abaixo da linha de título de coluna. Na guia Exibir, clique em Congelar painéis e, em seguida, clique em Congelar linha superior.
8. Selecione toda a planilha. Na guia dados, clique em filtro.
9. Na coluna última vez de sucesso, clique na seta para baixo e clique em Classificar em ordem crescente.
10. Na coluna DC de origem, clique no filtro seta para baixo, aponte para filtros de texto e, em seguida, clique em filtro personalizado.
11. Na caixa de diálogo Personalizar AutoFiltro, em linhas de mostrar onde, clique em não contém. Na caixa de texto adjacente, digite <userInput>del</userInput> eliminar esmaecendo os resultados para excluído controladores de domínio.
12. Repita a etapa 11 para a coluna de última hora de falha, mas use o valor não iguais e, em seguida, digite o valor 0.
13. Resolva falhas de replicação.

Para cada controlador de domínio na floresta, a planilha mostra o parceiro de replicação de origem, o tempo que a replicação última ocorreu e a hora da última replicação falha para cada contexto de nomenclatura (partição de diretório). Usando AutoFiltro no Excel, você pode exibir a integridade da replicação para funcionar somente, controladores de domínio falha somente controladores de domínio ou controladores de domínio que são menos ou mais atual e você pode ver os parceiros de replicação que são replicando com êxito.

## <a name="replication-problems-and-resolutions"></a>Resoluções e problemas de replicação
    
Problemas de replicação são relatados em mensagens de evento e em diversas mensagens de erro que ocorrem quando um aplicativo ou serviço tenta uma operação. Idealmente, essas mensagens são coletadas pelo seu aplicativo monitoramento ou quando você recupera o status de replicação.

A maioria dos problemas de replicação são identificados nas mensagens de evento que são registradas no log de eventos do serviço de diretório. Problemas de replicação também podem ser identificados na forma de mensagens de erro na saída do <system>repadmin /showrepl</system> comando. 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>Repadmin /showrepl mensagens de erro indicam problemas de replicação

Para identificar problemas de replicação do Active Directory, use o <system>repadmin /showrepl</system> de comando, conforme descrito na seção anterior. A tabela a seguir mostra mensagens de erro que gera esse comando, juntamente com as causas dos erros e links para tópicos que fornecem soluções para os erros.


|Erro Repadmin|Causa raiz|Solução|
| --- | --- | --- |
|O tempo decorrido desde a última replicação com este servidor excedeu o tempo de desativação.|Um controlador de domínio falhou duplicação de entrada com o controlador de domínio de origem nomeado longa o suficiente para uma exclusão ter sido marcado para exclusão, replicados e coleta de lixo do AD DS.|ID de evento: 2042: Foi muito tempo desde essa máquina replicada|
|Nenhuma entrados vizinhos.|Se nenhum item aparecer na seção "Vizinhos de entrada" da saída que é gerada pelo repadmin /showrepl, o controlador de domínio não foi possível estabelecer links de replicação com outro controlador de domínio.|Corrigindo problemas de replicação de conectividade (ID de evento 1925)| 
|Acesso será negado.|Existe um link de replicação entre dois controladores de domínio, mas a duplicação não pode ser executada corretamente como resultado de uma falha de autenticação.|Corrigindo problemas de replicação de segurança| 
|Última tentativa em < data - tempo > falha com o "nome de conta de destino está incorreta".|Esse problema pode estar relacionado a conectividade, DNS ou problemas de autenticação. Se esse for um erro de DNS, o controlador de domínio local não foi possível resolver o identificador globalmente exclusivo (GUID)-com base em nome DNS do seu parceiro de replicação.|Corrigindo replicação DNS Lookup problemas (IDs de evento 1925, 2087, 2088) corrigindo segurança problemas de replicação de corrigir problemas de conectividade de replicação (ID de evento 1925)| 
|Erro LDAP 49.|A conta de computador do controlador de domínio não pode ser sincronizada com o Centro de distribuição de chaves (KDC).|Corrigindo problemas de replicação de segurança| 
|Não é possível abrir conexão LDAP para host local|A ferramenta de administração não pôde contatar o AD DS.|Corrigindo problemas de pesquisa de DNS replicação (IDs de evento 1925, 2087, 2088)| 
|Replicação do Active Directory tem foram preterida.|O progresso de duplicação de entrada foi interrompido por uma solicitação de replicação de prioridade mais alta, como uma solicitação que tenha sido gerada manualmente com o comando de /sync repadmin.|Aguarde a conclusão da replicação. Esta mensagem informativa indica a operação normal.| 
|Replicação lançada, esperando.| O controlador de domínio postou uma solicitação de replicação e está aguardando uma resposta. Replicação está em andamento dessa fonte.|Aguarde a conclusão da replicação. Esta mensagem informativa indica a operação normal.| 

A tabela a seguir lista os eventos comuns que podem indicar problemas de replicação, juntamente com raiz causas de problemas e links para tópicos que fornecem soluções para os problemas do Active Directory. 

|ID de evento e a fonte|Causa raiz|Solução|
| --- | --- | --- | 
|1311 NTDS KCC|As informações de configuração de replicação no AD DS não refletem precisamente a topologia física da rede.|Corrigindo problemas de replicação de topologia (ID de evento 1311)| 
|1388 NTDS replicação|Consistência de replicação estrito não está em vigor e um objeto remanescente foi replicado para o controlador de domínio.|Corrigir problemas de objeto remanescente da replicação (IDs de evento 1388, 1988, 2042)|
|1925 NTDS KCC|Falha ao tentar estabelecer um link de replicação para uma partição de diretório gravável. Esse evento pode ter causas diferentes, dependendo do erro.| Corrigindo conectividade problemas (ID de evento 1925) corrigindo replicação DNS Lookup problemas de replicação (IDs de evento 1925, 2087, 2088)| 
|1988 NTDS replicação|O controlador de domínio local tiver tentado replicar um objeto de um controlador de domínio de origem que não está presente no controlador de domínio local porque ele pode foram excluído e já coleta de lixo. Replicação não continua para essa partição de diretório com esse parceiro até que a situação seja resolvida.|Corrigir problemas de objeto remanescente da replicação (IDs de evento 1388, 1988, 2042)|
|2042 NTDS replicação|Replicação não ocorreu com esse parceiro para uma desativação e replicação não pode continuar.|Corrigir problemas de objeto remanescente da replicação (IDs de evento 1388, 1988, 2042)| 
|2087 NTDS replicação|AD DS não foi possível resolver o nome de host DNS do controlador de domínio de origem para um endereço IP e replicação falhou.|Corrigindo problemas de pesquisa de DNS replicação (IDs de evento 1925, 2087, 2088)| 
|2088 NTDS replicação |AD DS não foi possível resolver o nome de host DNS do controlador de domínio de origem para um endereço IP, mas replicação foi bem-sucedida.|Corrigindo problemas de pesquisa de DNS replicação (IDs de evento 1925, 2087, 2088)|
|Logon de rede 5805|Uma conta de computador falhou em autenticar, que normalmente é causado por qualquer uma das várias instâncias do mesmo nome de computador ou o nome do computador não replicam para todos os controladores de domínio.|Corrigindo problemas de replicação de segurança| 

Para obter mais informações sobre os conceitos de replicação, consulte [tecnologias de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
