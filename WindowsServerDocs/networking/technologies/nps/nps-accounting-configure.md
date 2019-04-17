---
title: Configurar as estatísticas de servidor de política de rede
description: Este tópico fornece informações sobre o arquivo de texto e registro em log para o servidor de política de rede no Windows Server 2016 do SQL Server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69341e30d90ee1be29c40d835a4f71fe433c11dc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policy-server-accounting"></a>Configurar as estatísticas de servidor de política de rede

Há três tipos de log para o servidor de política de rede \(NPS\):

- **Log de eventos**. Usado principalmente para auditoria e solucionar problemas de tentativas de conexão. Você pode configurar Obtendo as propriedades do servidor NPS no console do NPS de log de eventos NPS.

- **Log de solicitações de contabilidade e autenticação do usuário em um arquivo local**. Usado principalmente para fins de faturamento e análise de conexão. Também é útil como uma ferramenta de investigação de segurança porque ele fornece um método de controle das atividades de um usuário mal-intencionado após um ataque. Você pode configurar o log de arquivo local usando o Assistente de configuração de contabilização.

- **Registrando solicitações de estatísticas e autenticação do usuário de um banco de dados compatível com o Microsoft SQL Server XML**. Usado para permitir que vários servidores que executam NPS para ter uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o log do SQL Server usando o Assistente de configuração de contabilização.

## <a name="use-the-accounting-configuration-wizard"></a>Use o Assistente de configuração de contabilização

Usando o Assistente de configuração de contabilidade, você pode configurar as seguintes configurações de estatísticas de quatro:

- **SQL apenas log**. Usando essa configuração, você pode configurar um link de dados para um SQL Server que permite que o NPS conectem e enviar dados de contabilidade para o SQL server. Além disso, o assistente pode configurar o banco de dados no SQL Server para garantir que o banco de dados é compatível com o log do servidor NPS SQL.
- **Log de texto somente**. Usando essa configuração, você pode configurar o NPS para registrar dados contábeis em um arquivo de texto.
- **Paralelas log**. Usando essa configuração, você pode configurar o banco de dados e vinculação de dados do SQL Server. Você também pode configurar o log de arquivo de texto para que o NPS registre simultaneamente o arquivo de texto e o banco de dados do SQL Server. 
- **Registro em log SQL com backup**. Usando essa configuração, você pode configurar o banco de dados e vinculação de dados do SQL Server. Além disso, você pode configurar o log de arquivo de texto que NPS usa se falhar de log do SQL Server.

Essas configurações, além de log do SQL Server e o log de texto permitem que você especifique se NPS continua a processar solicitações de conexão se houver falha no log. Você pode especificar isso no **seção de ação de falha de registro em log** nas propriedades de registro em log de arquivo local, nas propriedades de log do servidor SQL e enquanto você estiver executando o Assistente de configuração de contabilidade.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para executar o Assistente de configuração Contabilidade

Para executar o Assistente de configuração Contabilidade, conclua as seguintes etapas:

1. Abra o console do NPS ou o snap-in do Console de gerenciamento NPS Microsoft (MMC).
2. Na árvore de console, clique em **Accounting**.
3. No painel de detalhes, em **Accounting**, clique em **configurar Accounting**.

## <a name="configure-nps-log-file-properties"></a>Configurar as propriedades de arquivo de Log do NPS

Você pode configurar o servidor de política de rede (NPS) para realizar as estatísticas de autenticação discagem usuário serviço RADIUS (Remote) para solicitações de autenticação de usuário, mensagens de aceitação de acesso, mensagens de rejeição de acesso, contabilidade solicitações e respostas e atualizações periódicas de status. Você pode usar este procedimento para configurar os arquivos de log em que você deseja armazenar os dados de contabilidade.

Para obter mais informações sobre como interpretar os arquivos de log, consulte [interpretar arquivos de Log de formato de banco de dados de NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para impedir que os arquivos de log ocupem o disco rígido, é altamente recomendável mantê-los em uma partição separada da partição do sistema. A seguir fornece mais informações sobre a configuração de contabilidade para NPS:

- Para enviar os dados do arquivo de log para a coleta por outro processo, você pode configurar o NPS gravar em um pipe nomeado. Para usar pipes nomeados, defina a pasta de arquivos de log para \\.\pipe ou \\ComputerName\pipe. O programa de servidor de pipe nomeado cria um pipe nomeado denominado \\.\pipe\iaslog.log para aceitar os dados. Na caixa de diálogo Propriedades de arquivo Local, em criar um novo arquivo de log, selecione nunca (tamanho do arquivo ilimitado) quando você usa pipes nomeados.

- O diretório do arquivo de log pode ser criado usando variáveis de ambiente do sistema (em vez de variáveis de usuário), como % systemdrive %, % systemroot % e % windir %. Por exemplo, o caminho a seguir, usando o ambiente variável % windir %, localiza o arquivo de log no diretório do sistema na subpasta \System32\Logs (ou seja, % windir%\System32\Logs\).

- Mudança de formatos de arquivo de log não faz com que um novo log seja criado. Se você alterar formatos de arquivo de log, o arquivo que está ativo no momento da alteração conterá uma mistura dos dois formatos (registros no início do log terão o formato anterior e registros no final do log terão o novo formato).

- Se estatísticas RADIUS falhar devido a uma unidade de disco rígido completa ou outras causas, NPS para processar solicitações de conexão, impedindo que os usuários acessem recursos de rede.

- NPS fornece a capacidade de fazer logon para um banco de dados do Microsoft® SQL Server™ além ou em vez de fazer logon em um arquivo local.

A associação a **Admins. do domínio** grupo é o requisito mínimo para executar este procedimento.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar propriedades de arquivo de log do NPS

1. Abra o console do NPS ou o snap-in do Console de gerenciamento NPS Microsoft (MMC).
2. Na árvore de console, clique em **Accounting**.
3. No painel de detalhes, em **propriedades de arquivo de Log**, clique em **alterar propriedades do arquivo de Log**. O **propriedades de arquivo de Log** caixa de diálogo é aberta.
4. Em **propriedades de arquivo de Log**diante do **configurações** guia, **registrar as seguintes informações**, certifique-se de que você optar por registrar informações suficientes para atingir suas metas de contabilidade. Por exemplo, se os logs de precisarem realizar correlação de sessão, marque todas as caixas de seleção.
5. Em **registrando falha ação**, selecione **se houver falha no log, descarte solicitações de conexão** se você quiser NPS para parar o processamento de mensagens de solicitação de acesso quando os arquivos de log são inteira ou não está disponível por algum motivo. Se você quiser NPS para continuar a processar solicitações de conexão se houver falha no log, não marque essa caixa de seleção.
6. No **propriedades de arquivo de Log** caixa de diálogo, clique no **arquivo de Log** guia.
7. Sobre o **arquivo de Log** guia, **diretório**, digite o local onde deseja armazenar arquivos de log do NPS. O local padrão é a pasta systemroot\System32\LogFiles.
    >[!NOTE]
    >Se você não fornecer uma declaração de caminho completo na **diretório do arquivo de Log**, o caminho padrão é usado. Por exemplo, se você digitar **NPSLogFile** em **diretório do arquivo de Log**, o arquivo está localizado em systemroot%\System32\NPSLogFile.
8. Em **formato**, clique em **compatíveis com o DTS**. Se você preferir, você pode em vez disso, selecionar um formato de arquivo herdado, como **ODBC \(Legacy\)** ou **IAS \(Legacy\)**.
    >[!NOTE]
    >**ODBC** e **IAS** tipos de arquivo herdado contêm um subconjunto das informações que NPS envia ao seu banco de dados do SQL Server. O **compatíveis com o DTS** formato XML do tipo de arquivo é idêntico ao formato XML que NPS usa para importar dados para seu banco de dados do SQL Server. Portanto, o **compatíveis com o DTS** formato de arquivo fornece uma transferência mais eficiente e completa de dados para o banco de dados padrão do SQL Server para NPS.
9. Em **criar um novo arquivo de log**, a configuração do NPS para iniciar novos arquivos de log em intervalos específicos, clique no intervalo que você deseja usar:
    - Para um volume de transações e atividades de registro em log, clique em **diária**.
    - Para volumes de transações e atividades de registro em log menos intensos, clique em **semanais** ou **mensal**.
    - Para armazenar todas as transações em um arquivo de log, clique em **nunca \(unlimited file size\)**.
    - Para limitar o tamanho de cada arquivo de log, clique em **quando o arquivo de log atinge esse tamanho**e, em seguida, digite um tamanho de arquivo, após o qual um novo registro é criado. O tamanho padrão é 10 megabytes (MB).
10. Se você quiser NPS para excluir arquivos de log antigos para criar espaço em disco para novos arquivos de log quando o disco rígido está próximo a capacidade, certifique-se de que **quando o disco estiver completo excluir arquivos de log antigos** é selecionado. Essa opção não estiver disponível, no entanto, se o valor de **criar um novo arquivo de log** é **nunca \(unlimited file size\)**. Além disso, se o arquivo de log mais antigo é o arquivo de log atual, ele não será excluído.

## <a name="configure-nps-sql-server-logging"></a>Configurar o log do servidor NPS SQL

Você pode usar este procedimento para dados de contabilização de RAIO de log para um banco de dados local ou remoto executando o Microsoft SQL Server.

>[!NOTE]
>NPS formata os dados contábeis como um documento XML que ele envia para o **report_event** procedimento armazenado no banco de dados SQL Server que você designar no NPS. Para o SQL Server de registro em log para funcionar corretamente, você deve ter um procedimento armazenado denominado **report_event** no banco de dados SQL Server que possa receber e analisar documentos XML do NPS.

A associação ao grupo Admins. do domínio, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar registro em log no NPS do SQL Server

1. Abra o console do NPS ou o snap-in do Console de gerenciamento NPS Microsoft (MMC).
2. Na árvore de console, clique em **Accounting**.
3. No painel de detalhes, em **propriedades de log do SQL Server**, clique em **propriedades de log do alteração SQL Server**. O **propriedades de log do SQL Server** caixa de diálogo é aberta.
4. Em **registrar as seguintes informações**, selecione as informações que você deseja fazer: 
    - Para registrar em log todas as solicitações de contabilidade, clique em **solicitações de contabilidade**.
    - Para registrar solicitações de autenticação, clique em **solicitações de autenticação**.
    - Para registrar status periódicos de contabilidade, clique em **periódicas status de contabilidade**.
    - Para registrar status periódicas, como solicitações de estatísticas provisórias, clique em **status periódico**.
5. Para configurar o número de sessões simultâneas permitido entre o servidor que executa o NPS e o SQL Server, digite um número no **número máximo de sessões simultâneas**.
6. Para definir a fonte de dados do SQL Server, em **log do SQL Server**, clique em **configurar**. O **Data Link Properties** caixa de diálogo é aberta. Sobre o **Conexão** guia, especifique o seguinte: 
    - Para especificar o nome do servidor no qual o banco de dados está armazenado, digite ou selecione um nome na **selecione ou insira um nome de servidor**.
    - Para especificar o método de autenticação com o qual deseja fazer logon no servidor, clique em **usar Windows segurança integrada NT**. Ou, clique em **usar um determinado nome de usuário e senha**e, em seguida, digite as credenciais no **nome de usuário** e **senha**.
    - Para permitir que uma senha em branco, clique em **senha em branco**.
    - Para armazenar a senha, clique em **permitir salvar senha**.
    - Para especificar o banco de dados para conectar-se para o computador executando o SQL Server, clique em **selecione o banco de dados no servidor**e, em seguida, selecione um nome de banco de dados da lista.
7. Para testar a conexão entre o NPS e SQL Server, clique em **Testar Conexão**. Clique em **Okey** fechar **Data Link Properties**.
8. Em **registrando falha ação**, selecione **habilitar o log de arquivo de texto de failover** se você quiser NPS para continuar com o log de arquivo de texto se falhar de log do SQL Server. 
9. Em **registrando falha ação**, selecione **se houver falha no log, descarte solicitações de conexão** se você quiser NPS para parar o processamento de mensagens de solicitação de acesso quando os arquivos de log são inteira ou não está disponível por algum motivo. Se você quiser NPS para continuar a processar solicitações de conexão se houver falha no log, não marque essa caixa de seleção.

## <a name="ping-user-name"></a>Nome de usuário ping

Alguns servidores proxy RADIUS e servidores de acesso à rede periodicamente enviam solicitações de autenticação e estatísticas (conhecidas como solicitações ping) para verificar se o servidor NPS está presente na rede. Essas solicitações ping incluem os nomes de usuário fictício. Quando NPS processa essas solicitações, os logs de eventos e contabilidade são preenchidos com registros de rejeição de acesso, tornando mais difícil de controlar registros válidos.

Quando você configura uma entrada do registro **executar ping nome de usuário**, NPS corresponde ao valor de entrada do registro com o valor de nome de usuário em solicitações ping por outros servidores. A **executar ping nome de usuário** entrada do registro Especifica o nome de usuário fictício (ou um nome padrão do usuário, com variáveis, que corresponde ao nome de usuário fictício) enviados por servidores proxy RADIUS e servidores de acesso à rede. Quando o NPS recebe solicitações ping que correspondem a **executar ping nome de usuário** valor da entrada do registro, o NPS rejeita as solicitações de autenticação sem processamento da solicitação. NPS não registra transações envolvendo o nome de usuário fictício em quaisquer arquivos de log, que facilita o log de eventos interpretar.

**Executar ping nome de usuário** não é instalado por padrão. Você deve adicionar **executar ping nome de usuário** no registro. Você pode adicionar uma entrada ao registro usando o Editor do registro.

>[!CAUTION]
>Edição incorreta do registro pode danificar gravemente o sistema. Antes de fazer alterações no registro, você deve fazer backup de todos os dados importantes no computador.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para adicionar o nome de usuário ping ao registro

Nome de usuário ping pode ser adicionada a seguinte chave do registro como um valor de cadeia de caracteres por um membro do grupo Administradores local:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Dados**: *nome de usuário*

>[!TIP]
>Para indicar mais de um nome de usuário para um **executar ping nome de usuário** valor, insira um nome padrão, como um nome DNS, incluindo caracteres curinga, em **dados**.
