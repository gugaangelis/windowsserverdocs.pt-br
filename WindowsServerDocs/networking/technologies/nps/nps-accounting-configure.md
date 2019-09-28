---
title: Configurar a contabilização do Servidor de Políticas de Rede
description: Este tópico fornece informações sobre o arquivo de texto e o log de SQL Server para o servidor de políticas de rede no Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: 0c154d4d4534f4c343107eecd158974b92903e39
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405567"
---
# <a name="configure-network-policy-server-accounting"></a>Configurar a contabilização do Servidor de Políticas de Rede

Há três tipos de log para NPS \(\)do servidor de políticas de rede:

- **Log de eventos**. Usado principalmente para auditoria e solução de problemas de tentativas de conexão. Você pode configurar o log de eventos do NPS obtendo as propriedades do NPS no console do NPS.

- **Registrar em log a autenticação de usuário e as solicitações de contabilização em um arquivo local**. Usado principalmente para fins de cobrança e análise de conexão. Também é útil como uma ferramenta de investigação de segurança, pois fornece um método de acompanhar a atividade de um usuário mal-intencionado após um ataque. Você pode configurar o registro em log de arquivo local usando o assistente de configuração de contabilidade.

- **Registrar em log a autenticação de usuário e solicitações de contabilização em um Microsoft SQL Server banco de dados em conformidade com XML**. Usado para permitir que vários servidores executando o NPS tenham uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o log de SQL Server usando o assistente de configuração de contabilidade.

## <a name="use-the-accounting-configuration-wizard"></a>Usar o assistente de configuração de contabilidade

Usando o assistente de configuração de contabilidade, você pode definir as quatro configurações de contabilidade a seguir:

- **Somente log SQL**. Ao usar essa configuração, você pode configurar um link de dados para um SQL Server que permite que o NPS se conecte e envie dados de contabilidade para o SQL Server. Além disso, o assistente pode configurar o banco de dados no SQL Server para garantir que o banco de dados seja compatível com o log NPS do SQL Server.
- **Somente registro em log de texto**. Ao usar essa configuração, você pode configurar o NPS para registrar dados de estatísticas em um arquivo de texto.
- **Log paralelo**. Ao usar essa configuração, você pode configurar o SQL Server e o link de dados do. Você também pode configurar o log de arquivo de texto para que o NPS faça logs simultaneamente no arquivo de texto e no banco de dados de SQL Server. 
- **Log de SQL com backup**. Ao usar essa configuração, você pode configurar o SQL Server e o link de dados do. Além disso, você pode configurar o log de arquivos de texto que o NPS usa se SQL Server log falhar.

Além dessas configurações, os SQL Server log e o log de texto permitem que você especifique se o NPS continuará a processar as solicitações de conexão se houver falha no log. Você pode especificar isso na **seção Ação de falha de log** em Propriedades de log de arquivo local, em Propriedades de log do SQL Server e enquanto estiver executando o assistente de configuração de contabilidade.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para executar o assistente de configuração de contabilidade

Para executar o assistente de configuração de contabilidade, conclua as seguintes etapas:

1. Abra o console do NPS ou o snap-in MMC (console de gerenciamento Microsoft) do NPS.
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, em **contabilidade**, clique em **Configurar contabilidade**.

## <a name="configure-nps-log-file-properties"></a>Configurar propriedades do arquivo de log do NPS

Você pode configurar o NPS (servidor de políticas de rede) para executar a contabilidade de serviço RADIUS (RADIUS) para solicitações de autenticação de usuário, mensagens de aceitação de acesso, mensagens de recusa de acesso, solicitações e respostas de estatísticas e atividades periódicas atualizações de status. Você pode usar este procedimento para configurar os arquivos de log nos quais você deseja armazenar os dados de contabilidade.

Para obter mais informações sobre como interpretar arquivos de log, consulte [interpretar arquivos de log de formato de banco de dados NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para impedir que os arquivos de log preencham o disco rígido, é altamente recomendável mantê-los em uma partição separada da partição do sistema. O seguinte fornece mais informações sobre como configurar a contabilidade para o NPS:

- Para enviar os dados do arquivo de log para coleta por outro processo, você pode configurar o NPS para gravar em um pipe nomeado. Para usar pipes nomeados, defina a pasta do \\arquivo de \\log como .\pipe ou ComputerName\pipe. O programa de servidor de pipe nomeado cria um pipe \\nomeado chamado .\pipe\iaslog.log para aceitar os dados. Na caixa de diálogo Propriedades do arquivo local, em criar um novo arquivo de log, selecione nunca (tamanho de arquivo ilimitado) ao usar pipes nomeados.

- O diretório do arquivo de log pode ser criado usando variáveis de ambiente do sistema (em vez de variáveis de usuário), como% systemdrive%,% SystemRoot% e% WINDIR%. Por exemplo, o caminho a seguir, usando a variável de ambiente% windir%, localiza o arquivo de log no diretório do sistema na subpasta \System32\Logs (ou seja,%windir%\System32\Logs @ no__t-0.

- Alternar formatos de arquivo de log não faz com que um novo log seja criado. Se você alterar os formatos de arquivo de log, o arquivo que estiver ativo no momento da alteração conterá uma combinação dos dois formatos (os registros no início do log terão o formato anterior e os registros no final do log terão o novo formato).

- Se a contabilização RADIUS falhar devido a uma unidade de disco rígido completa ou outras causas, o NPS para de processar as solicitações de conexão, impedindo que os usuários acessem os recursos da rede.

- O NPS fornece a capacidade de fazer logon em um banco de dados do Microsoft® SQL Server™ além de, ou em vez de, fazer logon em um arquivo local.

A associação no grupo **Admins** . do domínio é o mínimo necessário para executar esse procedimento.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar as propriedades do arquivo de log do NPS

1. Abra o console do NPS ou o snap-in MMC (console de gerenciamento Microsoft) do NPS.
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, em **Propriedades do arquivo de log**, clique em **alterar propriedades do arquivo de log**. A caixa de diálogo **Propriedades do arquivo de log** é aberta.
4. Em **Propriedades do arquivo de log**, na guia **configurações** , em **registrar as informações a seguir**, verifique se você optou por registrar informações suficientes para obter suas metas de contabilidade. Por exemplo, se os logs precisarem realizar a correlação de sessão, marque todas as caixas de seleção.
5. Em **ação de falha no log**, selecione **se o log falhar, descarte as solicitações de conexão** se você quiser que o NPS interrompa o processamento de mensagens de solicitação de acesso quando os arquivos de log estiverem cheios ou indisponíveis por algum motivo. Se você quiser que o NPS continue a processar solicitações de conexão se o log falhar, não marque essa caixa de seleção.
6. Na caixa de diálogo **Propriedades do arquivo de log** , clique na guia arquivo de **log** .
7. Na guia **arquivo de log** , em **diretório**, digite o local onde você deseja armazenar os arquivos de log do NPS. O local padrão é a pasta systemroot\System32\LogFiles.<br>Se você não fornecer uma instrução de caminho completo no **diretório do arquivo de log**, o caminho padrão será usado. Por exemplo, se você digitar **NPSLogFile** no **diretório do arquivo de log**, o arquivo estará localizado em%SystemRoot%\System32\NPSLogFile.
8. Em **formato**, clique em **compatível com DTS**. Se preferir, você pode selecionar um formato de arquivo herdado, como **ODBC \(Legacy\)**  ou **ias \(Legacy.\)**<br>Os tipos de arquivo herdados do **ODBC** e do **ias** contêm um subconjunto das informações que o NPS envia para seu banco de dados SQL Server. O formato XML do tipo de arquivo em **conformidade com DTS** é idêntico ao formato XML que o NPS usa para importar dados para seu SQL Server Database. Portanto, o formato de arquivo **compatível com DTS** fornece uma transferência mais eficiente e completa de dados para o banco de SQL Server padrão para NPS.
9. Em **criar um novo arquivo de log**, para configurar o NPS para iniciar novos arquivos de log em intervalos especificados, clique no intervalo que você deseja usar:
    - Para atividade de log e volume de transação pesada, clique em **diariamente**.
    - Para volumes de transações menores e atividade de registro em log, clique em **semanal** ou **mensalmente**.
    - Para armazenar todas as transações em um arquivo de log, clique em  **\(tamanho\)de arquivo nunca ilimitado**.
    - Para limitar o tamanho de cada arquivo de log, clique em **quando o arquivo de log atingir esse tamanho**e, em seguida, digite um tamanho de arquivo, após o qual um novo log será criado. O tamanho padrão é 10 megabytes (MB).
10. Se você quiser que o NPS exclua arquivos de log antigos para criar espaço em disco para novos arquivos de log quando o disco rígido estiver perto da capacidade, verifique se **quando o disco está cheio excluir arquivos de log mais antigos** está selecionado. No entanto, essa opção não estará disponível se o valor de **criar um novo arquivo de log** nunca  **\(for um\)tamanho de arquivo ilimitado**. Além disso, se o arquivo de log mais antigo for o arquivo de log atual, ele não será excluído.

## <a name="configure-nps-sql-server-logging"></a>Configurar o log de SQL Server do NPS

Você pode usar este procedimento para registrar em log os dados de contabilização RADIUS em um banco de dado local ou remoto executando o Microsoft SQL Server.

>[!NOTE]
>O NPS formata dados de contabilidade como um documento XML que ele envia para o procedimento armazenado **report_event** no banco de dados SQL Server que você designa no NPS. Para que SQL Server log funcione corretamente, você deve ter um procedimento armazenado chamado **report_event** no banco de dados SQL Server que pode receber e analisar os documentos XML do NPS.

A associação em admins. do domínio, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar o log de SQL Server no NPS

1. Abra o console do NPS ou o snap-in MMC (console de gerenciamento Microsoft) do NPS.
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, em **SQL Server Propriedades de log**, clique em **alterar SQL Server Propriedades de log**. A caixa de diálogo **Propriedades de log de SQL Server** é aberta.
4. Em **registrar em log as informações a seguir**, selecione as informações que você deseja registrar: 
    - Para registrar em log todas as solicitações de contabilização, clique em **solicitações de contabilização**.
    - Para registrar solicitações de autenticação, clique em **solicitações de autenticação**.
    - Para registrar o status de contabilidade periódica, clique em **status contábil periódico**.
    - Para registrar em log o status periódico, como solicitações de contabilidade provisória, clique em **status periódico**.
5. Para configurar o número de sessões simultâneas permitidas entre o servidor que executa o NPS e o SQL Server, digite um número no **número máximo de sessões simultâneas**.
6. Para configurar a fonte de dados SQL Server, em **registro em log de SQL Server**, clique em **Configurar**. A caixa de diálogo **Propriedades do link de dados** é aberta. Na guia **conexão** , especifique o seguinte: 
    - Para especificar o nome do servidor no qual o banco de dados está armazenado, digite ou selecione um nome em **selecionar ou insira um nome de servidor**.
    - Para especificar o método de autenticação com o qual fazer logon no servidor, clique em **usar segurança integrada do Windows NT**. Ou então, clique em **usar um nome de usuário e senha específicos**e, em seguida, digite credenciais em **nome de usuário** e **senha**.
    - Para permitir uma senha em branco, clique em **senha em branco**.
    - Para armazenar a senha, clique em **permitir salvar senha**.
    - Para especificar o banco de dados ao qual se conectar no computador que executa o SQL Server, clique em **selecionar o banco de dados no servidor**e selecione um nome de banco de dados na lista.
7. Para testar a conexão entre o NPS e o SQL Server, clique em **testar conexão**. Clique em **OK** para fechar **as propriedades do vínculo de dados**.
8. Em **ação de falha de log**, selecione **habilitar log de arquivo de texto para failover** se você quiser que o NPS continue com o log de arquivo de texto se SQL Server log falhar. 
9. Em **ação de falha no log**, selecione **se o log falhar, descarte as solicitações de conexão** se você quiser que o NPS interrompa o processamento de mensagens de solicitação de acesso quando os arquivos de log estiverem cheios ou indisponíveis por algum motivo. Se você quiser que o NPS continue a processar solicitações de conexão se o log falhar, não marque essa caixa de seleção.

## <a name="ping-user-name"></a>Ping de nome de usuário

Alguns servidores proxy RADIUS e servidores de acesso à rede enviam periodicamente solicitações de autenticação e contabilização (conhecidas como solicitações de ping) para verificar se o NPS está presente na rede. Essas solicitações de ping incluem nomes de usuário fictícios. Quando o NPS processa essas solicitações, os logs de eventos e de contabilidade se tornam preenchidos com registros de rejeição de acesso, tornando mais difícil manter o controle de registros válidos.

Quando você configura uma entrada de registro para **ping User-Name**, o NPS corresponde ao valor de entrada do registro em relação ao valor do nome de usuário em solicitações de ping por outros servidores. Uma entrada de registro de **nome de usuário de ping** especifica o nome de usuário fictício (ou um padrão de nome de usuário, com variáveis que corresponde ao nome de usuário fictício) enviado por servidores proxy RADIUS e servidores de acesso à rede. Quando o NPS recebe solicitações de ping que correspondem ao valor de entrada de registro de **nome de usuário de ping** , o NPS rejeita as solicitações de autenticação sem processar a solicitação. O NPS não registra as transações que envolvem o nome de usuário fictício em todos os arquivos de log, o que torna o log de eventos mais fácil de interpretar.

O **ping User-Name** não é instalado por padrão. Você deve adicionar **ping User-Name** ao registro. Você pode adicionar uma entrada ao registro usando o editor do registro.

>[!CAUTION]
>a edição incorreta do Registro pode danificar gravemente o sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para adicionar ping User-Name ao registro

O ping User-Name pode ser adicionado à seguinte chave do registro como um valor de cadeia de caracteres por um membro do grupo local de administradores:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**:`ping user-name`
- **Tipo**:`REG_SZ`
- **Dados**:  *Nome de usuário*

>[!TIP]
>Para indicar mais de um nome de usuário para um valor de **nome de usuário de ping** , insira um padrão de nome, como um nome DNS, incluindo caracteres curinga, em **dados**.
