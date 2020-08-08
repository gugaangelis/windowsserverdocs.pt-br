---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Solucionar problemas de replicação do Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 630ba90cd2e5c00753b707754d32530b38c8db8d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941615"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solucionar problemas de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory problemas de replicação podem ter várias fontes diferentes. Por exemplo, problemas de DNS (sistema de nomes de domínio), problemas de rede ou problemas de segurança podem fazer com que Active Directory replicação falhe.

O restante deste tópico explica as ferramentas e uma metodologia geral para corrigir Active Directory erros de replicação. Os subtópicos a seguir abrangem sintomas, causas e como resolver erros de replicação específicos:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introdução e recursos para solução de problemas de replicação de Active Directory

A falha de replicação de entrada ou saída faz com que Active Directory objetos que representam a topologia de replicação, o agendamento de replicação, os controladores de domínio, os usuários, os computadores, as senhas, os grupos de segurança, as associações de grupo e a Política de Grupo sejam inconsistentes entre os controladores de domínio A inconsistência de diretório e a falha de replicação causam falhas operacionais ou resultados inconsistentes, dependendo do controlador de domínio que é contatado para a operação e podem impedir o aplicativo de Política de Grupo e permissões de controle de acesso. O Active Directory Domain Services (AD DS) depende da conectividade de rede, da resolução de nomes, da autenticação e da autorização, do banco de dados de diretório, da topologia de replicação e do mecanismo de replicação. Quando a causa raiz de um problema de replicação não é imediatamente óbvia, determinar a causa entre as muitas causas possíveis requer uma eliminação sistemática das causas prováveis.

Para uma ferramenta baseada em interface do usuário para ajudar a monitorar a replicação e diagnosticar erros, consulte a [ferramenta de status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Para obter um documento abrangente que descreve como você pode usar a ferramenta repadmin para solucionar problemas de replicação Active Directory está disponível; consulte [monitoramento e solução de problemas Active Directory replicação usando repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obter informações sobre como Active Directory replicação funciona, consulte as seguintes referências técnicas:

- [Referência técnica do modelo de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referência técnica da topologia de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recomendações de solução de eventos e ferramentas

O ideal é que os eventos vermelho (erro) e amarelo (aviso) no log de eventos do serviço de diretório sugerem a restrição específica que está causando falha de replicação no controlador de domínio de origem ou de destino. Se a mensagem de evento sugerir etapas para uma solução, tente as etapas descritas no evento. A ferramenta repadmin e outras ferramentas de diagnóstico também fornecem informações que podem ajudá-lo a resolver falhas de replicação.

Para obter informações detalhadas sobre como usar o repadmin para solucionar problemas de replicação, consulte [monitoramento e solução de problemas Active Directory replicação usando repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Desativando interrupções intencionais ou falhas de hardware

Às vezes, ocorrem erros de replicação devido a interrupções intencionais. Por exemplo, ao solucionar problemas de Active Directory de replicação, a regra sai das desconexões intencionales e falhas de hardware ou atualizações primeiro.

### <a name="intentional-disconnections"></a>Desconexões intencionales

Se os erros de replicação forem relatados por um controlador de domínio que está tentando fazer a replicação com um controlador de domínio que foi criado em um site de preparo e estiver offline no momento aguardando sua implantação no site de produção final (um site remoto, como uma filial), você poderá considerar esses erros de replicação. Para evitar a separação de um controlador de domínio da topologia de replicação por períodos estendidos, o que causa erros contínuos até que o controlador de domínio seja reconectado, considere adicionar esses computadores inicialmente como servidores membro e usar o método IFM (instalar da mídia) para instalar o Active Directory Domain Services (AD DS). Você pode usar a ferramenta de linha de comando Ntdsutil para criar uma mídia de instalação que você pode armazenar em mídia removível (CD, DVD ou outra mídia) e enviar para o site de destino. Em seguida, você pode usar a mídia de instalação para instalar AD DS nos controladores de domínio no site, sem o uso da replicação.

### <a name="hardware-failures-or-upgradestitle"></a>Falhas ou atualizações de hardware</title>

Se ocorrerem problemas de replicação como resultado de falha de hardware (por exemplo, falha de uma placa-mãe, subsistema de disco ou disco rígido), notifique o proprietário do servidor para que o problema de hardware possa ser resolvido.

As atualizações periódicas de hardware também podem fazer com que os controladores de domínio estejam fora do serviço. Verifique se os proprietários do seu servidor têm um bom sistema de comunicar tais interrupções com antecedência.

### <a name="firewall-configuration"></a>Configuração do firewall

Por padrão, Active Directory RPCs (chamadas de procedimento remoto) de replicação ocorrem dinamicamente em uma porta disponível por meio do mapeador de ponto de extremidade RPC (RPCSs) na porta 135. Verifique se o Firewall do Windows com segurança avançada e outros firewalls estão configurados corretamente para permitir a replicação. Para obter informações sobre como especificar a porta para Active Directory configurações de porta e replicação, consulte [o artigo 224196 na base de dados de conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=22578).

Para obter informações sobre as portas que Active Directory a replicação usa, consulte [Active Directory ferramentas e configurações de replicação](https://go.microsoft.com/fwlink/?LinkId=123774).

Para obter informações sobre como gerenciar a replicação de Active Directory sobre firewalls, consulte [Active Directory replicação sobre firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Respondendo à falha de um servidor desatualizado executando o Windows 2000 Server

Se um controlador de domínio que executa o Windows 2000 Server falhar por mais tempo do que o número de dias no tempo de vida da marca de exclusão, a solução será sempre a mesma:

1. Mova o servidor da rede corporativa para uma rede privada.
2. Force a remoção Active Directory ou reinstale o sistema operacional.
3. Remova os metadados do servidor de Active Directory para que o objeto de servidor não possa ser reativado.

Você pode usar um script para limpar os metadados do servidor na maioria dos sistemas operacionais Windows. Para obter informações sobre como usar esse script, consulte [remover metadados do controlador de domínio do Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599).

Por padrão, os objetos de configurações NTDS que são excluídos são reativados automaticamente por um período de 14 dias. Portanto, se você não remover os metadados do servidor (use Ntdsutil ou o script mencionado anteriormente para executar a limpeza de metadados), os metadados do servidor serão restabelecidos no diretório, o que solicitará que as tentativas de replicação ocorram. Nesse caso, os erros serão registrados em log persistentemente como resultado da incapacidade de replicar com o controlador de domínio ausente.

## <a name="root-causes"></a>Causas raiz

Se você descartar desconexões intencionales, falhas de hardware e controladores de domínio do Windows 2000 desatualizados, o restante dos problemas de replicação quase sempre terá uma das seguintes causas raiz:

- Conectividade de rede: a conexão de rede pode estar indisponível ou as configurações de rede não estão configuradas corretamente.
- Resolução de nomes: as configurações incorretas de DNS são uma causa comum de falhas de replicação.
- Autenticação e autorização: problemas de autenticação e autorização causam erros de "acesso negado" quando um controlador de domínio tenta se conectar a seu parceiro de replicação.
- Banco de dados de diretório (armazenamento): o banco de dados de diretório pode não ser capaz de processar transações com rapidez suficiente para acompanhar os tempos limite de replicação.
- Mecanismo de replicação: se os agendamentos de replicação entre sites forem muito curtos, as filas de replicação poderão ser muito grandes para serem processadas no momento necessário para a agenda de replicação de saída. Nesse caso, a replicação de algumas alterações pode ser interrompida indefinidamente, por tempo suficiente, para exceder o tempo de vida da marca de exclusão.
- Topologia de replicação: os controladores de domínio devem ter links entre sites no AD DS que mapeiam para WAN (rede de longa distância) ou conexões de rede virtual privada (VPN). Se você criar objetos no AD DS para a topologia de replicação que não tem suporte pela topologia de site real da sua rede, a replicação que requer a topologia configurada incorretamente falhará.

## <a name="general-approach-to-fixing-problems"></a>Abordagem geral para corrigir problemas

Use a seguinte abordagem geral para corrigir problemas de replicação:

1. Monitore a integridade da replicação diariamente ou use Repadmin.exe para recuperar o status de replicação diariamente.
2. Tente resolver qualquer falha relatada em tempo hábil usando os métodos descritos em mensagens de evento e este guia. Se o software puder estar causando o problema, desinstale o software antes de continuar com outras soluções.
3. Se o problema que está causando a falha de replicação não puder ser resolvido por nenhum método conhecido, remova AD DS do servidor e reinstale AD DS. Para obter mais informações sobre como reinstalar AD DS, consulte [encerramento de um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se AD DS não puder ser removido normalmente enquanto o servidor estiver conectado à rede, use um dos seguintes métodos para resolver o problema:

   - Force a remoção de AD DS no Modo de Restauração dos Serviços de Diretório (DSRM), limpe os metadados do servidor e reinstale AD DS.
   - Reinstale o sistema operacional e recrie o controlador de domínio.

Para obter mais informações sobre como forçar a remoção de AD DS, consulte [forçando a remoção de um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Usando repadmin para recuperar o status de replicação</title>

O status de replicação é uma maneira importante para avaliar o status do serviço de diretório. Se a replicação estiver funcionando sem erros, você saberá os controladores de domínio que estão online. Você também sabe que os seguintes sistemas e serviços estão funcionando:


- Infraestrutura de DNS
- Protocolo de autenticação Kerberos
- Serviço de tempo do Windows (W32time)
- RPC (Chamada de Procedimento Remoto)
- Conectividade de rede

Use repadmin para monitorar o status de replicação diariamente, executando um comando que avalia o status de replicação de todos os controladores de domínio em sua floresta. O procedimento gera um arquivo. csv que você pode abrir no Microsoft Excel e filtrar as falhas de replicação.

Você pode usar o procedimento a seguir para recuperar o status de replicação de todos os controladores de domínio na floresta.

Requisitos

A associação no **Admins. de Empresa** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

Ferramentas:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para gerar uma planilha repadmin/showrepl para controladores de domínio

1. Abra um prompt de comando como administrador: no menu Iniciar, clique com o botão direito do mouse em prompt de comando e clique em executar como administrador. Se a caixa de diálogo controle de conta de usuário for exibida, forneça as credenciais de administração de empresa, se necessário, e clique em continuar.
2. No prompt de comando, digite o seguinte comando e pressione ENTER:`repadmin /showrepl * /csv > showrepl.csv`
3. Abra o Excel.
4. Clique no botão Office, clique em abrir, navegue até showrepl.csv e clique em abrir.
5. Oculte ou exclua a coluna A, bem como a coluna de tipo de transporte, da seguinte maneira:
6. Selecione uma coluna que você deseja ocultar ou excluir.

   - Para ocultar a coluna, clique com o botão direito do mouse na coluna e clique em ocultar.
   - Para excluir a coluna, clique com o botão direito do mouse na coluna selecionada e clique em excluir.

7. Selecione a linha 1 abaixo da linha de título de coluna. Na guia Exibir, clique em congelar painéis e, em seguida, clique em congelar linha superior.
8. Selecione a planilha inteira. Na guia dados, clique em filtrar.
9. Na coluna último tempo de êxito, clique na seta para baixo e, em seguida, clique em classificar em ordem crescente.
10. Na coluna DC de origem, clique na seta filtrar para baixo, aponte para filtros de texto e clique em filtro personalizado.
11. Na caixa de diálogo Filtro automático personalizado, em mostrar linhas, clique em não contém. Na caixa de texto adjacente, digite <userInput>del</userInput> para eliminar de exibir os resultados de controladores de domínio excluídos.
12. Repita a etapa 11 para a última coluna de tempo de falha, mas use o valor não é igual e, em seguida, digite o valor 0.
13. Resolver falhas de replicação.

Para cada controlador de domínio na floresta, a planilha mostra o parceiro de replicação de origem, a hora em que a replicação ocorreu pela última vez e a hora em que a última falha de replicação ocorreu para cada contexto de nomenclatura (partição de diretório). Usando o AutoFiltro no Excel, você pode exibir a integridade da replicação somente para controladores de domínio de trabalho, somente controladores de domínio com falha ou controladores de domínio que são os menos ou mais atuais, e você pode ver os parceiros de replicação que estão replicando com êxito.

## <a name="replication-problems-and-resolutions"></a>Problemas e resoluções de replicação

Os problemas de replicação são relatados em mensagens de evento e em várias mensagens de erro que ocorrem quando um aplicativo ou serviço tenta uma operação. O ideal é que essas mensagens sejam coletadas pelo aplicativo de monitoramento ou quando você recupera o status de replicação.

A maioria dos problemas de replicação é identificada nas mensagens de evento que são registradas no log de eventos do serviço de diretório. Problemas de replicação também podem ser identificados na forma de mensagens de erro na saída do comando <system>repadmin/showrepl</system> .

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl mensagens de erro que indicam problemas de replicação

Para identificar Active Directory problemas de replicação, use o comando <system>repadmin/showrepl</system> , conforme descrito na seção anterior. A tabela a seguir mostra as mensagens de erro geradas por esse comando, juntamente com as causas raiz dos erros e links para tópicos que fornecem soluções para os erros.

|Erro de repadmin|Causa raiz|Solução|
| --- | --- | --- |
|O tempo desde a última replicação com este servidor excedeu a vida útil da marca de exclusão.|Um controlador de domínio apresentou falha na replicação de entrada com o controlador de domínio de origem nomeado longo o suficiente para que uma exclusão tenha sido marcada para exclusão, replicada e coletada por lixo de AD DS.|ID do evento 2042: faz muito tempo que esta máquina replicou|
|Não há vizinhos de entrada.|Se nenhum item aparecer na seção "vizinhos de entrada" da saída gerada pelo repadmin/showrepl, o controlador de domínio não conseguirá estabelecer links de replicação com outro controlador de domínio.|Corrigir problemas de conectividade de replicação (ID do evento 1925)|
|Acesso negado.|Existe um link de replicação entre dois controladores de domínio, mas a replicação não pode ser executada corretamente como resultado de uma falha de autenticação.|Corrigir problemas de segurança de replicação|
|Última tentativa em <data/hora> falhou com o "o nome da conta de destino está incorreto".|Esse problema pode estar relacionado a problemas de conectividade, DNS ou autenticação. Se esse for um erro DNS, o controlador de domínio local não poderá resolver o nome DNS baseado em GUID (identificador global exclusivo) de seu parceiro de replicação.|Corrigindo problemas de pesquisa de DNS de replicação (IDs de evento 1925, 2087, 2088) corrigindo problemas de segurança de replicação corrigindo problemas de conectividade de replicação (ID do evento 1925)|
|Erro de LDAP 49.|A conta do computador do controlador de domínio pode não ser sincronizada com o centro de distribuição de chaves (KDC).|Corrigir problemas de segurança de replicação|
|Não é possível abrir a conexão LDAP com o host local|A ferramenta de administração não pôde contatar o AD DS.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)|
|Active Directory a replicação foi admitida.|O progresso da replicação de entrada foi interrompido por uma solicitação de replicação de prioridade mais alta, como uma solicitação gerada manualmente com o comando repadmin/Sync.|Aguarde a conclusão da replicação. Essa mensagem informativa indica uma operação normal.|
|Replicação postada, aguardando.| O controlador de domínio postou uma solicitação de replicação e está aguardando uma resposta. A replicação está em andamento a partir desta origem.|Aguarde a conclusão da replicação. Essa mensagem informativa indica uma operação normal.|

A tabela a seguir lista os eventos comuns que podem indicar problemas com Active Directory replicação, juntamente com as causas raiz dos problemas e links para tópicos que fornecem soluções para os problemas.

|ID do evento e origem|Causa raiz|Solução|
| --- | --- | --- |
|1311 NTDS KCC|As informações de configuração de replicação no AD DS não refletem com precisão a topologia física da rede.|Corrigir problemas de topologia de replicação (ID do evento 1311)|
|1388 replicação de NTDS|A consistência de replicação estrita não está em vigor e um objeto remanescente foi replicado para o controlador de domínio.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)|
|1925 NTDS KCC|Falha na tentativa de estabelecer um link de replicação para uma partição de diretório gravável. Esse evento pode ter causas diferentes, dependendo do erro.| Corrigindo problemas de conectividade de replicação (ID de evento 1925) corrigindo problemas de pesquisa de DNS de replicação (IDs de evento 1925, 2087, 2088)|
|1988 replicação de NTDS|O controlador de domínio local tentou replicar um objeto de um controlador de domínio de origem que não está presente no controlador de domínio local porque ele pode ter sido excluído e já foi coletado como lixo. A replicação não prosseguirá para essa partição de diretório com esse parceiro até que a situação seja resolvida.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)|
|2042 replicação de NTDS|A replicação não ocorreu com esse parceiro por um tempo de vida de marca para exclusão e a replicação não pode continuar.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)|
|2087 replicação de NTDS|AD DS não pôde resolver o nome do host DNS do controlador de domínio de origem para um endereço IP e a replicação falhou.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)|
|2088 replicação de NTDS |AD DS não pôde resolver o nome do host DNS do controlador de domínio de origem para um endereço IP, mas a replicação foi bem-sucedida.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)|
|Logon de rede 5805|Falha na autenticação de uma conta de computador, que geralmente é causada por várias instâncias do mesmo nome do computador ou do nome do computador que não está replicando para cada controlador de domínio.|Corrigir problemas de segurança de replicação|

Para obter mais informações sobre os conceitos de replicação, consulte [Active Directory tecnologias de replicação](https://go.microsoft.com/fwlink/?LinkId=41950).

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, incluindo artigos de suporte específicos para códigos de erro, consulte o artigo de suporte: [como solucionar erros comuns de replicação de Active Directory](https://support.microsoft.com/help/3108513)
