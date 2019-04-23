---
title: Configurar a contabilização do Servidor de Políticas de Rede
description: Este tópico fornece informações sobre o arquivo de texto e o registro em log para o servidor de políticas de rede no Windows Server 2016 do SQL Server.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: c732a9f42d942ad579468d1dd15d30324d6fea87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839697"
---
# <a name="configure-network-policy-server-accounting"></a>Configurar a contabilização do Servidor de Políticas de Rede

Há três tipos de log para o servidor de políticas de rede \(NPS\):

- **O log de eventos**. Usado principalmente para auditoria e solucionar problemas de tentativas de conexão. Você pode configurar eventos NPS em log, obtendo as propriedades do NPS no console do NPS.

- **Registro em log solicitações de contabilização e autenticação de usuário em um arquivo local**. Usado principalmente para fins de análise e a cobrança de conexão. Também é útil como ferramenta de investigação de segurança porque ele oferece um método de controle a atividade de um usuário mal-intencionado após um ataque. Você pode configurar o log de arquivo local usando o Assistente de configuração de contabilização.

- **Registro em log solicitações de contabilização e autenticação de usuário para um banco de dados compatível com o Microsoft SQL Server XML**. Usado para permitir que vários servidores executando o NPS para ter uma fonte de dados. Também fornece as vantagens de usar um banco de dados relacional. Você pode configurar o log do SQL Server usando o Assistente de configuração de contabilização.

## <a name="use-the-accounting-configuration-wizard"></a>Use o Assistente de configuração da contabilidade

Usando o Assistente de configuração da contabilidade, você pode configurar as configurações de estatísticas de quatro seguintes:

- **Somente o log do SQL**. Ao usar essa configuração, você pode configurar um link de dados para um SQL Server que permite que o NPS conectar e enviar dados de estatísticas para o SQL server. Além disso, o assistente pode configurar o banco de dados no SQL Server para garantir que o banco de dados é compatível com o log do servidor SQL do NPS.
- **Log de texto apenas**. Ao usar essa configuração, você pode configurar o NPS para registrar dados de estatísticas em um arquivo de texto.
- **Registro em log em paralelo**. Ao usar essa configuração, você pode configurar o link de dados do SQL Server e banco de dados. Você também pode configurar o log de arquivo de texto para que o NPS registre simultaneamente para o arquivo de texto e o banco de dados do SQL Server. 
- **Registro em log do SQL com o backup**. Ao usar essa configuração, você pode configurar o link de dados do SQL Server e banco de dados. Além disso, você pode configurar o log de arquivo de texto que o NPS usa se o log do SQL Server falhará.

Além dessas configurações, log do SQL Server e o log de texto permitem que você especifique se o NPS continua a processar solicitações de conexão, se houver falha no log. Você pode especificar isso na **seção de ação de falha de registro em log** nas propriedades de log de arquivo local, nas propriedades de log do servidor SQL e enquanto você estiver executando o Assistente de configuração da contabilidade.

### <a name="to-run-the-accounting-configuration-wizard"></a>Para executar o Assistente de configuração da contabilidade

Para executar o Assistente de configuração da contabilidade, conclua as seguintes etapas:

1. Abra o console do NPS ou o snap-in do NPS Microsoft Management Console (MMC).
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, na **contabilização**, clique em **configurar a contabilização**.

## <a name="configure-nps-log-file-properties"></a>Configurar as propriedades de arquivo de Log do NPS

Você pode configurar o servidor de diretivas de rede (NPS) para executar autenticação Dial-In usuário serviço RADIUS (Remote) periódicas e contabilidade para solicitações de autenticação de usuário, mensagens de aceitação de acesso, mensagens de rejeição de acesso, as solicitações de contabilidade e respostas atualizações de status. Você pode usar este procedimento para configurar os arquivos de log no qual você deseja armazenar os dados de estatísticas.

Para obter mais informações sobre como interpretar os arquivos de log, consulte [interpretar arquivos de Log de formato de banco de dados de NPS](https://technet.microsoft.com/library/cc771748.aspx).

Para impedir que os arquivos de log ocupem o disco rígido, é altamente recomendável mantê-los em uma partição separada da partição do sistema. O exemplo a seguir fornece mais informações sobre como configurar a contabilização do NPS:

- Para enviar os dados de arquivo de log para coleta por outro processo, você pode configurar o NPS para gravar em um pipe nomeado. Para usar pipes nomeados, defina a pasta de arquivos de log como \\. \pipe ou \\ComputerName\pipe. O programa de servidor de pipe nomeado cria um pipe nomeado chamado \\.\pipe\iaslog.log para aceitar os dados. Na caixa de diálogo Propriedades de arquivo Local, em criar um novo arquivo de log, selecione nunca (tamanho de arquivo ilimitado) quando você usa pipes nomeados.

- O diretório de arquivos de log pode ser criado usando variáveis de ambiente do sistema (em vez de variáveis de usuário), como % systemdrive %, % systemroot % e % windir %. Por exemplo, o caminho a seguir, usando o ambiente variável % windir %, localiza o arquivo de log no diretório de sistema na subpasta \System32\Logs (ou seja, %windir%\System32\Logs\).

- Alternar os formatos de arquivo de log não faz com que um novo log a ser criado. Se você alterar formatos de arquivo de log, o arquivo que está ativo no momento da alteração conterá uma mistura dos dois formatos (registros no início do log terá o formato anterior e registros no final do log terão o novo formato).

- Se a contabilização RADIUS falhar devido a uma unidade de disco rígido completa ou outras causas, NPS para o processamento solicitações de conexão, impedindo que os usuários acessem recursos da rede.

- NPS fornece a capacidade de registrar um banco de dados do Microsoft® SQL Server™ além ou em vez de registro em log em um arquivo local.

Associação a **Admins. do domínio** grupo é o mínimo necessário para executar este procedimento.


### <a name="to-configure-nps-log-file-properties"></a>Para configurar propriedades de arquivo de log do NPS

1. Abra o console do NPS ou o snap-in do NPS Microsoft Management Console (MMC).
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, na **propriedades do arquivo de Log**, clique em **propriedades do arquivo de Log de alteração**. O **propriedades do arquivo de Log** caixa de diálogo é aberta.
4. No **propriedades do arquivo de Log**, na **configurações** guia **as seguintes informações de Log**, certifique-se de que você optar por registrar informações suficientes para alcançar suas metas de contabilidade. Por exemplo, se precisarem de seus logs realizar a correlação de sessão, marque todas as caixas de seleção.
5. Na **log de ação de falha**, selecione **se houver falha no log, descarte as solicitações de conexão** se você quiser que o NPS para parar o processamento de mensagens de solicitação de acesso quando os arquivos de log estão completa ou indisponível por algum motivo. Se você quiser que o NPS para continuar o processamento de solicitações de conexão se houver falha no log, não selecione essa caixa de seleção.
6. No **propriedades do arquivo de Log** caixa de diálogo, clique o **arquivo de Log** guia.
7. Sobre o **arquivo de Log** guia de **diretório**, digite o local onde você deseja armazenar arquivos de log do NPS. O local padrão é a pasta systemroot\System32\LogFiles.<br>Se você não fornecer uma instrução de caminho completo no **diretório de arquivo de Log**, o caminho padrão é usado. Por exemplo, se você digitar **NPSLogFile** na **diretório de arquivo de Log**, o arquivo está localizado em systemroot%\System32\NPSLogFile.
8. Na **formato**, clique em **compatível com DTS**. Se você preferir, você pode em vez disso, selecione um formato de arquivo herdados, como **ODBC \(herdados\)**  ou **IAS \(herdados\)**.<br>**ODBC** e **IAS** tipos de arquivo herdado contêm um subconjunto das informações que o NPS envia para seu banco de dados do SQL Server. O **compatível com DTS** o formato XML do tipo de arquivo é idêntico ao formato XML que o NPS usa para importar dados para seu banco de dados do SQL Server. Portanto, o **compatível com DTS** formato de arquivo fornece uma transferência mais eficiente e completa de dados no banco de dados do SQL Server padrão para o NPS.
9. Na **criar um novo arquivo de log**, para configurar o NPS para iniciar novos arquivos de log em intervalos especificados, clique no intervalo que você deseja usar:
    - Para um volume de transações e atividades de registro em log, clique em **diária**.
    - Para volumes de transações menor e atividades de log, clique em **semanal** ou **mensal**.
    - Para armazenar todas as transações em um arquivo de log, clique em **nunca \(tamanho de arquivo ilimitado\)**.
    - Para limitar o tamanho de cada arquivo de log, clique em **quando o arquivo de log atinge esse tamanho**e, em seguida, digite um tamanho de arquivo, após o qual um novo log é criado. O tamanho padrão é 10 megabytes (MB).
10. Se você quiser que o NPS para excluir arquivos de log antigos para criar o espaço em disco para novos arquivos de log quando o disco rígido esteja próximo à capacidade, certifique-se de que **quando o disco está completo excluir arquivos de log antigos** está selecionado. Essa opção não estiver disponível, no entanto, se o valor de **criar um novo arquivo de log** é **Never \(tamanho de arquivo ilimitado\)**. Além disso, se o arquivo de log mais antigo for o arquivo de log atual, ele não é excluído.

## <a name="configure-nps-sql-server-logging"></a>Configurar o log do servidor SQL do NPS

Você pode usar este procedimento para dados de contabilização de RADIUS do log para um banco de dados local ou remoto executando o Microsoft SQL Server.

>[!NOTE]
>O NPS formata os dados de estatísticas como um documento XML que ele envia para o **report_event** procedimento armazenado no banco de dados do SQL Server que você designar no NPS. Para o log do SQL Server para funcionar corretamente, você deve ter um procedimento armazenado denominado **report_event** no banco de dados do SQL Server que possa receber e analisar documentos XML do NPS.

A associação em Admins. do domínio, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-configure-sql-server-logging-in-nps"></a>Para configurar o log no NPS do SQL Server

1. Abra o console do NPS ou o snap-in do NPS Microsoft Management Console (MMC).
2. Na árvore de console, clique em **contabilidade**.
3. No painel de detalhes, na **propriedades de registro em log do SQL Server**, clique em **alterar propriedades servidor SQL log**. O **propriedades de registro em log do SQL Server** caixa de diálogo é aberta.
4. Na **as seguintes informações de Log**, selecione as informações que você deseja registrar: 
    - Para registrar em log todas as solicitações de contabilidade, clique em **as solicitações de contabilidade**.
    - Para registrar solicitações de autenticação, clique em **solicitações de autenticação**.
    - Para registrar o status de contabilização periódica, clique em **status de contabilização periódica**.
    - Para registrar o status periódicas, como solicitações de contabilidade provisório, clique em **status periódico**.
5. Para configurar o número de sessões simultâneas permitido entre o servidor que executa o NPS e o SQL Server, digite um número em **o número máximo de sessões simultâneas**.
6. Para configurar a fonte de dados do SQL Server, no **log do SQL Server**, clique em **configurar**. O **propriedades de vínculo de dados** caixa de diálogo é aberta. Sobre o **Conexão** guia, especifique o seguinte: 
    - Para especificar o nome do servidor no qual o banco de dados está armazenado, digite ou selecione um nome na **selecione ou insira um nome de servidor**.
    - Para especificar o método de autenticação com o qual fazer logon servidor, clique em **a segurança integrada Use Windows NT**. Ou, clique em **usar um determinado nome de usuário e senha**e, em seguida, digite as credenciais no **nome de usuário** e **senha**.
    - Para permitir que uma senha em branco, clique em **senha em branco**.
    - Para armazenar a senha, clique em **permitir salvamento de senha**.
    - Para especificar qual banco de dados para conectar-se para o computador executando o SQL Server, clique em **selecione o banco de dados no servidor**e, em seguida, selecione um nome de banco de dados da lista.
7. Para testar a conexão entre o SQL Server e o NPS, clique em **Testar Conexão**. Clique em **Okey** para fechar **propriedades de vínculo de dados**.
8. Na **log de ação de falha**, selecione **habilitar registro em log de arquivo de texto para o failover** se você quiser que o NPS para continuar com o log de arquivo de texto se falhar de log do SQL Server. 
9. Na **log de ação de falha**, selecione **se houver falha no log, descarte as solicitações de conexão** se você quiser que o NPS para parar o processamento de mensagens de solicitação de acesso quando os arquivos de log estão completa ou indisponível por algum motivo. Se você quiser que o NPS para continuar o processamento de solicitações de conexão se houver falha no log, não selecione essa caixa de seleção.

## <a name="ping-user-name"></a>Nome de usuário de ping

Alguns servidores proxy RADIUS e servidores de acesso à rede periodicamente enviam solicitações de autenticação e contabilidade (conhecidas como solicitações de ping) para verificar se o NPS está presente na rede. Essas solicitações ping incluem nomes de usuário fictício. Quando o NPS processa essas solicitações, os logs de eventos e contabilidade fique cheio de registros de rejeição de acesso, tornando mais difícil de controlar registros válidos.

Quando você configura uma entrada do registro **executar o ping de nome de usuário**, NPS corresponde ao valor de entrada de registro com o valor de nome de usuário em solicitações de ping por outros servidores. Um **executar o ping de nome de usuário** entrada de registro Especifica o nome de usuário fictício (ou um nome padrão do usuário, com variáveis, que corresponde ao nome de usuário fictício) enviada por servidores proxy RADIUS e servidores de acesso de rede. Quando o NPS recebe solicitações de ping que correspondem a **executar o ping de nome de usuário** valor de entrada do registro, o NPS rejeita as solicitações de autenticação sem processar a solicitação. NPS não registrar transações que envolvem o nome de usuário fictício em quaisquer arquivos de log, que torna mais fácil interpretar o log de eventos.

**Executar o ping de nome de usuário** não está instalado por padrão. Você deve adicionar **executar o ping de nome de usuário** no registro. Você pode adicionar uma entrada no registro usando o Editor do registro.

>[!CAUTION]
>a edição incorreta do Registro pode danificar gravemente o sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

### <a name="to-add-ping-user-name-to-the-registry"></a>Para adicionar o nome de usuário do ping para o registro

Nome de usuário do ping pode ser adicionado a seguinte chave do registro como um valor de cadeia de caracteres por um membro do grupo Administradores local:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Dados**:  *Nome de usuário*

>[!TIP]
>Para indicar a mais de um nome de usuário para um **executar o ping de nome de usuário** valor, insira um padrão de nome, como um nome DNS, incluindo caracteres curinga, em **dados**.
