---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Solucionar problemas de replicação do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869417"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solucionar problemas de replicação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Problemas de replicação do Active Directory podem ter várias fontes diferentes. Por exemplo, problemas, problemas de rede ou problemas de segurança do sistema de nome de domínio (DNS) podem todos causar falha na replicação do Active Directory. 

O restante deste tópico explica as ferramentas e uma metodologia geral para corrigir erros de replicação do Active Directory. Os seguintes subtópicos abrangem os sintomas, causas e como resolver erros de replicação específico:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introdução e recursos para solucionar problemas de replicação do Active Directory

Entrada ou a falha de replicação de saída faz com que os objetos do Active Directory que representam a topologia de replicação, agendamento de replicação, controladores de domínio, os usuários, computadores, senhas, grupos de segurança, associações de grupo e política de grupo fiquem inconsistentes entre os controladores de domínio. Falha de inconsistência e a replicação de diretório causar falhas operacionais ou resultados inconsistentes, dependendo do controlador de domínio que é contatado para a operação e pode impedir a aplicação de diretiva de grupo e permissões de controle de acesso. Os serviços de domínio do Active Directory (AD DS) depende de conectividade de rede, resolução de nomes, autenticação e autorização, o banco de dados do diretório, a topologia de replicação e o mecanismo de replicação. Quando a causa raiz de um problema de replicação não é imediatamente óbvia, determinar a causa entre as muitas causas possíveis exige a eliminação sistemática de causas prováveis.

Para uma ferramenta baseada em interface do usuário ajudar a monitorar a replicação e diagnosticar erros, consulte [ferramenta de Status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Para um documento abrangente que descreve como você pode usar a ferramenta Repadmin para solucionar problemas do Active Directory a replicação está disponível. ver [monitoramento e solução de problemas do Active Directory Replication usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obter informações sobre como funciona a replicação do Active Directory, consulte as seguintes referências técnicas:

- [Referência técnica de modelo de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referência técnica da topologia de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recomendações de solução de evento e a ferramenta

O ideal é que o vermelho (erro) e eventos de amarelo (aviso) no log de eventos do serviço de diretório sugerem a restrição específica que está causando a falha de replicação no controlador de domínio de origem ou destino. Se a mensagem de evento sugere etapas para uma solução, tente as etapas descritas no evento. A ferramenta Repadmin e outras ferramentas de diagnóstico também fornecem informações que podem ajudá-lo a resolver falhas de replicação. 

Para obter informações detalhadas sobre como usar Repadmin para solucionar problemas de replicação, consulte [monitoramento e solução de problemas do Active Directory Replication usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Descartando-interrupções intencionais ou falhas de hardware

Às vezes, erros de replicação ocorrem devido a interrupções intencionais. Por exemplo, ao solucionar problemas de replicação do Active Directory, elimine desconexões intencionais e falhas de hardware ou atualizações pela primeira vez.

### <a name="intentional-disconnections"></a>Desconexões intencionais

Se os erros de replicação são relatados por um controlador de domínio que está tentando fazer a replicação com um controlador de domínio que foi criado em um site de preparo e está offline no momento aguardando sua implantação no local de produção final (um site remoto, como uma filial ), você poderá considerar esses erros de replicação. Para evitar separar um controlador de domínio da topologia da replicação por longos períodos, que faz com que erros contínuos até que o controlador de domínio seja reconectado, considere adicionar tais computadores inicialmente como servidores membros e usando a instalação da mídia ( Método IFM) para instalar os serviços de domínio do Active Directory (AD DS). Você pode usar a ferramenta de linha de comando Ntdsutil para criar mídia de instalação que você pode armazenar em mídia removível (CD, DVD ou outra mídia) e enviar para o site de destino. Em seguida, você pode usar a mídia de instalação para instalar o AD DS nos controladores de domínio no site, sem o uso da replicação. 

### <a name="hardware-failures-or-upgradestitle"></a>Falhas de hardware ou atualizações</title>

Se ocorrerem problemas de replicação como resultado de falha de hardware (por exemplo, falha de uma placa-mãe, subsistema de disco ou unidade de disco rígido), notificar o proprietário do servidor para que o problema de hardware pode ser resolvido.

Atualizações de hardware periódica também podem causar a controladores de domínio para estar fora de serviço. Certifique-se de que os proprietários de seus servidores têm um bom sistema de comunicação, como interrupções de antemão.

### <a name="firewall-configuration"></a>Configuração de firewall

Por padrão, chamadas de procedimento remoto de replicação do Active Directory (RPCs) ocorrem dinamicamente ao longo de uma porta disponível por meio do mapeador de ponto de extremidade de RPC (RPCSS) na porta 135. Certifique-se de que o Windows Firewall com segurança avançada e outros firewalls estão configurados corretamente para permitir a replicação. Para obter informações sobre como especificar a porta para a replicação do Active Directory e as configurações de porta, consulte [224196 na Base de dados de Conhecimento da Microsoft do artigo](https://go.microsoft.com/fwlink/?LinkId=22578). 

Para obter informações sobre as portas que usa replicação do Active Directory, consulte [ferramentas de replicação do Active Directory e configurações](https://go.microsoft.com/fwlink/?LinkId=123774).

Para obter informações sobre como gerenciar a replicação do Active Directory através de firewalls, consulte [replicação do Active Directory através de Firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Respondendo à falha de um servidor desatualizado executando o Windows 2000 Server

Se um controlador de domínio executando o Windows 2000 Server tiver falhado por mais tempo que o número de dias no tempo de vida da marca para exclusão, a solução é sempre o mesmo: 

1. Mova o servidor da rede corporativa a uma rede privada.
2. Modo forçado remova do Active Directory ou reinstalar o sistema operacional.
3. Remova os metadados do servidor do Active Directory para que o objeto de servidor não pode ser reativado. 

Você pode usar um script para limpar os metadados do servidor na maioria dos sistemas operacionais Windows. Para obter informações sobre como usar esse script, consulte [remover a metadados controlador do domínio do diretório ativo](https://go.microsoft.com/fwlink/?LinkID=123599). 

Por padrão, os objetos de configurações NTDS que são excluídos são reativados automaticamente por um período de 14 dias. Portanto, se você não remover os metadados do servidor (use Ntdsutil ou o script mencionado anteriormente para executar a limpeza de metadados), os metadados do servidor seja restabelecido no diretório, que solicita a tentativas de replicação ocorrer. Nesse caso, erros serão registrados persistentemente como resultado a incapacidade de replicar com o controlador de domínio ausentes.

## <a name="root-causes"></a>Causas raiz

Se você descartar desconexões intencionais, falhas de hardware e desatualizados controladores de domínio do Windows 2000, o restante dos problemas de replicação quase sempre ter uma das seguintes causas:

- Conectividade de rede: A conexão de rede pode estar indisponível ou configurações de rede não estão configuradas corretamente.
- Resolução de nomes: Configurações de DNS incorretas são uma causa comum de falhas de replicação.
- Autenticação e autorização: Problemas de autenticação e autorização causam erros de "Acesso negado" quando um controlador de domínio tenta se conectar ao seu parceiro de replicação.
- Banco de dados do diretório (repositório): O banco de dados do diretório pode não ser capaz de processar transações rápido o suficiente para acompanhar tempos limite de replicação.
- Mecanismo de replicação: Se as agendas de replicação entre sites são muito curtas, filas de replicação podem ser muito grandes para processar a hora em que é necessário para o agendamento de replicação de saída. Nesse caso, a replicação de algumas alterações pode ser paralisada indefinidamente, potencialmente, tempo suficiente para exceder o tempo de vida da marca para exclusão.
- Topologia de replicação: Controladores de domínio devem ter links entre sites no AD DS que mapeiam para conexões de rede virtual privada (VPN) ou rede real de longa distância (WAN). Se você criar objetos no AD DS para a topologia de replicação que não são compatíveis com a topologia de site real da sua rede, a replicação que exige a topologia configuradas incorretamente falhará.

## <a name="general-approach-to-fixing-problems"></a>Abordagem geral para a correção de problemas

Use a seguinte abordagem geral para corrigir problemas de replicação: 

1. Monitorar a integridade da replicação diariamente ou usar Repadmin.exe para recuperar o status de replicação diária.
2. Tenta resolver qualquer falha relatada de maneira oportuna usando os métodos descritos neste guia e mensagens de evento. Se o software pode estar causando o problema, desinstale o software antes de continuar com outras soluções.
3. Se o problema que está causando a falha na replicação não pode ser resolvido por todos os métodos conhecidos, remover o AD DS do servidor e, em seguida, reinstalar o AD DS. Para obter mais informações sobre como reinstalar o AD DS, consulte [Descomissionando um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Se o AD DS não pode ser removido normalmente enquanto o servidor está conectado à rede, use um dos seguintes métodos para resolver o problema:

   - Forçar a remoção de AD DS no Directory Services Restore DSRM (modo), limpar metadados de servidor e, em seguida, reinstalar o AD DS.
   - Reinstale o sistema operacional e recrie o controlador de domínio.

Para obter mais informações sobre como forçar a remoção do AD DS, consulte [forçar a remoção de um controlador de domínio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Usando Repadmin para recuperar o status de replicação</title>

Status de replicação é uma maneira importante para você avaliar o status do serviço de diretório. Se a replicação está funcionando sem erros, você sabe os controladores de domínio que estiverem online. Você também pode saber se os seguintes sistemas e serviços estão funcionando:


- Infraestrutura de DNS
- Protocolo de autenticação Kerberos
- Serviço de tempo do Windows (W32time)
- Chamada de procedimento remoto (RPC)
- Conectividade de rede

Use Repadmin para monitorar o status de replicação diário ao executar um comando que avalia o status de replicação de todos os controladores de domínio na floresta. O procedimento gera um arquivo. csv que você pode abrir no Microsoft Excel e o filtro para falhas de replicação.

Você pode usar o procedimento a seguir para recuperar o status de replicação de todos os controladores de domínio na floresta. 

Requisitos

A associação no **Admins. de Empresa** ou equivalente é o requisito mínimo exigido para concluir este procedimento. 

Ferramentas:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para gerar uma planilha do repadmin /showrepl para controladores de domínio

1. Abra um prompt de comando como um administrador: No menu Iniciar, clique com botão direito o Prompt de comando e, em seguida, clique em Executar como administrador. Se a caixa de diálogo controle de conta de usuário for exibida, forneça as credenciais de administrador corporativo, se necessário e, em seguida, clique em continuar.
2. No prompt de comando, digite o seguinte comando e pressione ENTER: `repadmin /showrepl * /csv > showrepl.csv`
3. Abra o Excel.
4. Clique no botão Office, clique em Abrir, navegue até Showrepl e, em seguida, clique em Abrir.
5. Ocultar ou excluir a coluna A, bem como a coluna de tipo de transporte, da seguinte maneira:
6. Selecione uma coluna que você deseja ocultar ou excluir.

   - Para ocultar a coluna, a coluna com o botão direito e, em seguida, clique em Ocultar.
   - Para excluir a coluna, a coluna selecionada com o botão direito e, em seguida, clique em Excluir.

7. Selecione a linha 1 sob a linha de cabeçalho de coluna. Na guia Exibir, clique em Congelar painéis e, em seguida, clique em linha superior congelar.
8. Selecione a planilha inteira. Na guia dados, clique em filtro.
9. Na coluna de hora da última sucesso, clique na seta para baixo e, em seguida, clique em Classificar em ordem crescente.
10. Na coluna DC de origem, clique no filtro de seta para baixo, aponte para filtros de texto e, em seguida, clique em filtro personalizado.
11. Na caixa de diálogo Personalizar AutoFiltro, em linhas de mostrar onde, clique em não contém. Na caixa de texto adjacente, digite <userInput>/DEL</userInput> eliminar na exibição de resultados para excluído controladores de domínio.
12. Repita a etapa 11 para a coluna de hora da última falha, mas use o valor não iguais e, em seguida, digite o valor 0.
13. Resolva falhas de replicação.

Para cada controlador de domínio na floresta, a planilha mostra o parceiro de replicação de origem, o tempo que a replicação ocorreu pela última vez e a hora em que ocorreu a última falha de replicação para cada contexto de nomenclatura (partição de diretório). Ao usar o AutoFiltro no Excel, você pode exibir a integridade da replicação para controladores de domínio apenas, falhando somente controladores de domínio ou controladores de domínio que são menos ou mais atual e você pode ver os parceiros de replicação que estão replicando com êxito.

## <a name="replication-problems-and-resolutions"></a>Resoluções e problemas de replicação

Problemas de replicação são relatados em mensagens de eventos em várias mensagens de erro que ocorrem quando um aplicativo ou serviço tenta uma operação. O ideal é que essas mensagens são coletadas por seu aplicativo de monitoramento ou quando você recuperar o status de replicação.

A maioria dos problemas de replicação são identificados nas mensagens de evento são registradas no log de eventos do serviço de diretório. Problemas de replicação também podem ser identificados na forma de mensagens de erro na saída do <system>repadmin /showrepl</system> comando.

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>mensagens de erro do Repadmin /showrepl que indicam problemas de replicação

Para identificar problemas de replicação do Active Directory, use o <system>repadmin /showrepl</system> de comando, conforme descrito na seção anterior. A tabela a seguir mostra as mensagens de erro que este comando gera, juntamente com as causas dos erros e links para tópicos que fornecem soluções para os erros.

|Erro de Repadmin|Causa raiz|Solução|
| --- | --- | --- |
|O tempo decorrido desde a última replicação com este servidor excedeu o tempo de vida da marca para exclusão.|Um controlador de domínio falha na replicação de entrada com o controlador de domínio de origem nomeada longo o suficiente para uma exclusão ter sido marcado para exclusão, replicados e coleta de lixo do AD DS.|ID de evento 2042: Faz muito tempo que esta máquina replicou|
|Nenhuma entrados vizinhos.|Se nenhum item aparecer na seção da saída que é gerada pelo repadmin /showrepl "entrada vizinhos", o controlador de domínio não pôde estabelecer links de replicação com outro controlador de domínio.|Corrigir problemas de conectividade de replicação (ID do evento 1925)| 
|O acesso é negado.|Existe um link de replicação entre dois controladores de domínio, mas a replicação não pode ser executada corretamente, como resultado de uma falha de autenticação.|Corrigir problemas de segurança de replicação| 
|Última tentativa em < data - hora > falhou com o "nome da conta de destino está incorreto".|Esse problema pode estar relacionado à conectividade, DNS ou problemas de autenticação. Se esse for um erro de DNS, o controlador de domínio local não foi possível resolver o identificador global exclusivo (GUID)-com base no nome DNS de seu parceiro de replicação.|Corrigindo replicação problemas pesquisa de DNS problemas (ID do evento 1925, 2087, 2088) corrigindo replicação segurança corrigir problemas de conectividade de replicação (ID do evento 1925)| 
|Erro LDAP 49.|A conta de computador do controlador de domínio pode não ser sincronizada com o Centro de distribuição de chaves (KDC).|Corrigir problemas de segurança de replicação| 
|Não é possível abrir a conexão LDAP para o host local|A ferramenta de administração não pôde contatar o AD DS.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)| 
|Replicação do Active Directory foi substituída.|O andamento da replicação de entrada foi interrompido por uma solicitação de replicação de prioridade mais alta, como uma solicitação que foi gerada manualmente com o comando de /Sync. repadmin.|Aguarde a conclusão da replicação. Essa mensagem informativa indica que a operação normal.| 
|A replicação é lançada, esperando.| O controlador de domínio lançada uma solicitação de replicação e está aguardando uma resposta. A replicação está em andamento dessa fonte.|Aguarde a conclusão da replicação. Essa mensagem informativa indica que a operação normal.| 

A tabela a seguir lista os eventos comuns que podem indicar problemas com o Active Directory faz com que o replication, juntamente com a raiz dos problemas e links para tópicos que fornecem soluções para os problemas. 

|ID do evento e o código-fonte|Causa raiz|Solução|
| --- | --- | --- | 
|1311 NTDS KCC|As informações de configuração de replicação do AD DS não refletem com precisão a topologia física da rede.|Corrigir problemas de topologia de replicação (ID do evento 1311)| 
|Replicação de NTDS 1388|Consistência de replicação estrita não estiver em vigor, e um objeto remanescente foi replicado para o controlador de domínio.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)|
|1925 NTDS KCC|Falha na tentativa de estabelecer um vínculo de replicação para uma partição de diretório gravável. Esse evento pode ter causas diferentes, dependendo do erro.| Corrigindo conectividade problemas (ID do evento 1925) correção de replicação DNS Lookup problemas de replicação (ID do evento 1925, 2087, 2088)| 
|Replicação de NTDS 1988|O controlador de domínio local tentou replicar um objeto de um controlador de domínio de origem que não está presente no controlador de domínio local porque ele pode ter foi excluído e já coletado como lixo. A replicação não continuará para essa partição de diretório com esse parceiro até que a situação seja resolvida.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)|
|Replicação de NTDS 2042|Replicação não tiver ocorrido com esse parceiro para um tempo de vida da marca para exclusão e a replicação não pode continuar.|Corrigir problemas de objetos remanescentes de replicação (ID do evento 1388, 1988, 2042)| 
|Replicação de NTDS 2087|AD DS não foi possível resolver o nome de host DNS do controlador de domínio de origem para um endereço IP e a falha na replicação.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)| 
|Replicação de NTDS 2088 |AD DS não foi possível resolver o nome de host DNS do controlador de domínio de origem para um endereço IP, mas a replicação foi bem-sucedida.|Corrigir problemas de pesquisa de DNS de replicação (ID do evento 1925, 2087, 2088)|
|Logon de rede 5805|Uma conta de computador falhou ao autenticar, que geralmente é causado por várias instâncias do mesmo nome de computador ou o nome do computador não está replicando para cada controlador de domínio.|Corrigir problemas de segurança de replicação| 

Para obter mais informações sobre os conceitos de replicação, consulte [as tecnologias de replicação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, incluindo o suporte a artigos específicos para códigos de erro consulte o artigo de suporte: [Como solucionar erros comuns de replicação do Active Directory](https://support.microsoft.com/help/3108513)
