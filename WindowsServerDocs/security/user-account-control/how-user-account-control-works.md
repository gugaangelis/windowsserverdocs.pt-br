---
title: "Como funciona o controle de conta de usuário"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7f5ad234959ebfe0687b8bc10f9e43f2092252f2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="how-user-account-control-works"></a>Como funciona o controle de conta de usuário

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Controle de conta de usuário (UAC) ajuda a evitar que programas mal-intencionados (também chamados de malware) danifiquem um computador e ajuda as organizações a implantar uma área de trabalho mais bem gerenciada. Com o UAC, os aplicativos e tarefas são sempre executados no contexto de segurança de uma conta não administrativa, a menos que um administrador especificamente conceda acesso de nível de administrador ao sistema. O UAC pode bloquear a instalação automática de aplicativos não autorizados e impedir alterações inadvertidas em configurações do sistema.

## <a name="uac-process-and-interactions"></a>Processo UAC e interações
Cada aplicativo que exija o token de acesso de administrador deve solicitar o administrador de consentimento. A única exceção é a relação que existe entre pai e filho processos. Os processos filho herdam o token de acesso do usuário do processo pai. Processos pai e filho, no entanto, devem ter o mesmo nível de integridade. Windows Server 2012 protege os processos marcando os níveis de integridade. Níveis de integridade são avaliações de confiança. Um aplicativo de integridade "alta" é aquele que realiza tarefas que modificam dados do sistema, como um disco de particionamento de aplicativo, enquanto um aplicativo de integridade "baixa" é aquele que realiza tarefas que poderiam comprometer o sistema operacional, como um navegador da Web. Aplicativos com níveis de integridade mais baixos não podem modificar dados em aplicativos com níveis de integridade mais altos. Quando um usuário padrão tenta executar um aplicativo que requer um token de acesso de administrador, o UAC exige que o usuário forneça credenciais de administrador válidas.

Para entender melhor como esse processo ocorre é importante examinar os detalhes do processo de logon do Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processo de Logon do Windows Server 2012
A ilustração a seguir demonstra como o processo de logon para um administrador difere do processo de logon para um usuário padrão.

![Ilustração demonstrando como o processo de logon para um administrador difere do processo de logon para um usuário padrão](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Por padrão, administradores e usuários padrão acessam recursos e executam aplicativos no contexto de segurança de usuários padrão. Quando um usuário faz logon em um computador, o sistema cria um token de acesso para esse usuário. O token de acesso contém informações sobre o nível de acesso concedido ao usuário, inclusive identificadores de segurança específicos (SIDs) e privilégios do Windows.

Quando um administrador faz logon, dois tokens de acesso separados são criados para o usuário: um token de acesso de usuário padrão e um token de acesso de administrador. O token de acesso de usuário padrão contém as mesmas informações específicas do usuário que o token de acesso de administrador, mas os privilégios administrativos do Windows e os SIDs são removidos. O token de acesso de usuário padrão é usado para iniciar aplicativos que não executam tarefas administrativas (aplicativos de usuário padrão). O token de acesso de usuário padrão, em seguida, é usado para exibir a área de trabalho (Explorer.exe). Explorer.exe é o processo pai do qual todos os outros processos iniciados por usuário herdam o token de acesso. Como resultado, todos os aplicativos executados como usuário padrão, a menos que um usuário fornece consentimento ou credenciais para aprovar um aplicativo para usar um token de acesso administrativo completo.

Um usuário que seja um membro do grupo Administradores pode fazer logon, navegar na Web e ler emails enquanto usam um token de acesso de usuário padrão. Quando o administrador precisa realizar uma tarefa que exige o token de acesso de administrador, o Windows Server 2012 automaticamente solicita ao usuário para aprovação. Esse prompt é chamado uma solicitação de elevação, e seu comportamento pode ser configurado usando o snap-in política de segurança Local (secpol. msc) ou a política de grupo.

> [!NOTE]
> O termo "elevar" é usado para fazer referência ao processo no Windows Server 2012 que solicita que o usuário de consentimento ou credenciais para usar um token de acesso de administrador completo.

### <a name="the-uac-user-experience"></a>A experiência de usuário do UAC
Quando o UAC está habilitado, a experiência do usuário para usuários padrão é diferente daquela de administradores no modo de aprovação de administrador. O método recomendado e mais seguro de executar o Windows Server 2012 é fazer com que seu usuário principal uma conta de usuário padrão da conta. Execução como usuário padrão ajuda a maximizar a segurança de um ambiente gerenciado. Com o componente de elevação do UAC interno, os usuários padrão podem realizar facilmente uma tarefa administrativa inserindo credenciais válidas para uma conta de administrador local. O padrão, o componente de elevação do UAC interno para usuários padrão é a solicitação de credencial.

A alternativa à execução como um usuário padrão é executar como administrador no modo de aprovação de administrador. Com o componente de elevação do UAC interno, membros do grupo Administradores local podem realizar facilmente uma tarefa administrativa dando a aprovação. O padrão, o componente de elevação do UAC interno para uma conta de administrador no modo de aprovação de administrador é chamado a solicitação de consentimento. O comportamento de indicação de elevação do UAC pode ser configurado usando o snap-in política de segurança Local (secpol. msc) ou a política de grupo.

**As solicitações de consentimento e credencial**

Com o UAC habilitado, o Windows Server 2012 solicita consentimento ou solicita credenciais de uma conta de administrador local válida antes de iniciar um programa ou uma tarefa que exija um token de acesso de administrador completo. Essa solicitação garante que nenhum software mal-intencionado pode ser instalado silenciosamente.

**A solicitação de consentimento**

A solicitação de consentimento é apresentada quando um usuário tenta executar uma tarefa que exige o token de acesso administrativo de um usuário. A seguir está uma captura de tela de consentimento do UAC.

![Captura de tela de consentimento do UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**A solicitação de credencial**

A solicitação de credencial é apresentada quando um usuário padrão tenta executar uma tarefa que exige o token de acesso administrativo de um usuário. Esse comportamento de solicitação de padrão de usuário padrão pode ser configurado usando o snap-in política de segurança Local (secpol. msc) ou a política de grupo. Os administradores também podem ser necessários para fornecer suas credenciais definindo o controle de conta de usuário: comportamento da solicitação de elevação para administradores no modo de aprovação de administrador configuração de política valor para Prompt para credenciais.

A captura de tela a seguir está um exemplo de credencial do UAC.

![Captura de tela mostrando um exemplo de credencial do UAC](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Solicitações de elevação do UAC**

As solicitações de elevação do UAC são codificadas por cores para ser específica ao aplicativo, permitindo a identificação imediata de potencial risco à segurança de um aplicativo. Quando um aplicativo tenta executar com o token de acesso completo de um administrador, o Windows Server 2012 primeiro analisa o arquivo executável para determinar o Editor. Aplicativos são inicialmente separados em três categorias com base no Editor do arquivo executável: Windows Server 2012, editor verificado (assinado) e editor não verificado (não assinado). O diagrama a seguir ilustra como o Windows Server 2012 determina qual solicitação de elevação de cor para apresentar ao usuário.

A solicitação de elevação codificação é o seguinte:

-   Tela de fundo vermelha com um ícone de escudo vermelho: O aplicativo é bloqueado por política de grupo ou de um fornecedor que está bloqueado.

-   Tela de fundo com um ícone de escudo azul e dourado azul: O aplicativo é um aplicativo administrativo do Windows Server 2012, como um item do painel de controle.

-   Tela de fundo com um ícone de escudo azul azul: O aplicativo está assinado com Authenticode e é confiável segundo o computador local.

-   Amarelo em segundo plano com um ícone de escudo amarelo: O aplicativo é não assinado ou assinado, mas ainda não é confiável segundo o computador local.

**Ícone de escudo**

Alguns painel de controle de itens, como **propriedades de data e hora**, contêm uma combinação de administrador e operações de usuário padrão. Os usuários padrão podem exibir o relógio e alterar o fuso horário, mas um token de acesso de administrador completo é necessário para alterar a hora do sistema local. A seguir está uma captura de tela a **propriedades de data e hora** item do painel de controle.

![Tela mostrando a * * item Data e hora no painel de controle de propriedades * *](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

O ícone de escudo no **alterar data e hora** botão indica que o processo requer um token de acesso de administrador completo e exibirá um aviso de elevação do UAC.

**Protegendo a solicitação de elevação**

O processo de elevação fica ainda mais protegido direcionando a solicitação para a área de trabalho segura. As solicitações de consentimento e credencial são exibidas na área de trabalho segura por padrão no Windows Server 2012. Apenas processos do Windows podem acessar a área de trabalho segura. Para níveis superiores de segurança, é recomendável manter a **controle de conta de usuário: alternar para a área de trabalho segura ao pedir elevação** configuração de política habilitada.

Quando um arquivo executável solicita a elevação, área de trabalho interativa, também chamada de área de trabalho do usuário, é alternada para a área de trabalho segura. Área de trabalho protegida esmaece a área de trabalho do usuário e exibe uma solicitação de elevação que deve ser respondida antes de continuar. Quando o usuário clicar em Sim ou não, a área de trabalho alterna de volta para a área de trabalho do usuário.

Malware pode apresentar uma imitação da área de trabalho segura, mas quando o controle de conta de usuário: comportamento da solicitação de elevação para administradores no modo de aprovação de administrador é definido como Pedir consentimento, o malware não consegue a elevação se o usuário clicar em Sim na imitação. Se a configuração de política é definida como pedir credenciais, o malware imitando a solicitação de credencial pode ser capaz de coletar as credenciais do usuário. No entanto, o malware não consegue um privilégio elevado e o sistema tem outras proteções que aliviar o malware assuma o controle da interface do usuário mesmo com uma senha coletada.

Enquanto o malware possa apresentar uma imitação da área de trabalho protegida, esse problema não pode ocorrer, a menos que um usuário tenha instalado anteriormente o malware no computador. Porque processos que exigem um token de acesso de administrador não podem instalar silenciosamente quando o UAC está habilitado, o usuário deve dar explicitamente o consentimento clicando **Sim** ou fornecendo credenciais de administrador. O comportamento específico de elevação do UAC depende de política de grupo.

## <a name="uac-architecture"></a>Arquitetura do UAC
O diagrama a seguir detalha a arquitetura do UAC.

![Diagrama detalhando a arquitetura do UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Para compreender melhor cada componente, examine a tabela a seguir:

|Componente|Descrição|
|-------|--------|
|**Usuário**||
|Usuário realiza uma operação que exige um privilégio|Se a operação altere o sistema de arquivos ou o registro, a virtualização é chamada. Todas as outras operações chamam ShellExecute.|
|ShellExecute|ShellExecute chama CreateProcess. ShellExecute procura o erro ERROR_ELEVATION_REQUIRED de CreateProcess. Caso ele receba o erro, ShellExecute chama o serviço de informações do aplicativo para tentar realizar a tarefa solicitada usando o prompt com privilégios elevados.|
|CreateProcess|Se o aplicativo exija elevação, CreateProcess rejeita a chamada com ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Serviço de informações do aplicativo|Um serviço de sistema que ajuda a iniciar aplicativos que exigem um ou mais privilégios elevados ou direitos de usuário para executar, como tarefas administrativas locais e aplicativos que exigem níveis de integridade mais altos. As informações do aplicativo serviço ajuda a iniciar esses aplicativos, criando um novo processo para o aplicativo com o token de acesso completo do usuário administrativo quando a elevação é necessária e (dependendo da política de grupo) de consentimento é dado pelo usuário para fazer isso.|
|Elevando a uma instalação do ActiveX|Caso o ActiveX não estiver instalado, o sistema verifica o nível de controle deslizante do UAC. Se o ActiveX esteja instalado, o **controle de conta de usuário: alternar para a área de trabalho segura ao pedir elevação** configuração de política de grupo está marcada.|
|Verificar o nível de controle deslizante do UAC|Agora, o UAC tem quatro níveis de notificação a serem escolhidos e um controle deslizante usar para selecionar o nível de notificação:<br /><br /><ul><li>Alto<br /><br />    Se o controle deslizante estiver definido como **sempre notificar**, o sistema verifica se a área de trabalho segura está habilitada.</li><li>Médio<br /><br />    Se o controle deslizante estiver definido como **notificar padrão me somente quando programas tentarem fazer alterações no meu computador**, o **controle de conta de usuário: Elevar somente executáveis assinados e validados** configuração de política está marcada:<br /><br /><ul><li>Se a configuração de política estiver habilitada, a validação de caminho de certificação de infraestrutura de chave pública (PKI) é imposta para um determinado arquivo executável antes que ele tenha permissão para ser executado.</li><li>Se a configuração de política não estiver habilitado (padrão), a validação de caminho de certificação da PKI não é imposta antes que um determinado arquivo executável tenha permissão para ser executado. O **controle de conta de usuário: alternar para a área de trabalho segura ao pedir elevação** configuração de política de grupo está marcada.</li></ul></li><li>Baixo<br /><br />    Se o controle deslizante estiver definido como **notificar-me somente quando programas tentarem fazer alterações no meu computador (não esmaecer a área de trabalho)**, o CreateProcess é chamado.</li><li>Nunca notificar<br /><br />    Se o controle deslizante estiver definido como **nunca notificar-me quando**, o UAC jamais notificará quando um programa está tentando instalar ou tentando fazer qualquer alteração no computador. **Importante:** essa configuração não é recomendada. Essa configuração é a mesma configuração o **controle de conta de usuário: comportamento da solicitação de elevação para administradores no modo de aprovação de administrador** configuração de política **Elevate sem aviso**.</li></ul>|
|Área de trabalho protegida habilitada|O **controle de conta de usuário: alternar para a área de trabalho segura ao pedir elevação** configuração de política está marcada:<br /><br />-Se a área de trabalho segura estiver habilitada, todas as solicitações de elevação vão para a área de trabalho segura independentemente das configurações de política de comportamento de solicitação para administradores e usuários padrão.<br />-Se a área de trabalho protegida não esteja habilitada, todas as solicitações de elevação vão para área de trabalho interativa do usuário e as configurações por usuário para administradores e usuários padrão são usadas.|
|CreateProcess|CreateProcess chama a detecção de AppCompat, Fusion e instalador para avaliar se o aplicativo exija elevação. O arquivo executável acaba sendo inspecionado para determinar o nível de execução solicitado, que é armazenado no manifesto do aplicativo para o arquivo executável. CreateProcess falha caso o nível de execução solicitado especificado no manifesto não coincide com o token de acesso e retorna um erro (ERROR_ELEVATION_REQUIRED) para ShellExecute. |
|Compatibilidade de aplicativo|O banco de dados AppCompat armazena informações nas entradas de correção de compatibilidade de aplicativo para um aplicativo.|
|Fusion|O banco de dados Fusion armazena informações de manifestos de aplicativo que descrevem os aplicativos. Os esquemas de manifesto é atualizado para adicionar um novo campo de nível de execução solicitado.|
|Detecção do instalador|A detecção do instalador detecta arquivos executáveis de instalação, que ajuda a evitar que as instalações sejam executadas sem conhecimento e consentimento do usuário.|
|**Kernel**||
|Virtualização|A tecnologia de virtualização garante que aplicativos não compatíveis com não silenciosamente Falha ao executar ou falhem de maneira que a causa não pode ser determinada. O UAC também fornece virtualização de arquivos e do registro e log para aplicativos que gravam em áreas protegidas.|
|Sistema de arquivos e do registro|A virtualização de arquivos e do registro por usuário redireciona o registro por computador e as solicitações de gravação de arquivo para locais por usuário equivalente. Solicitações de leitura são redirecionadas para o local do usuário virtualizado primeiro e depois para o local do computador por segundo.|

Há uma alteração no Windows Server 2012 UAC das versões anteriores do Windows. O novo controle de deslizante jamais desativará UAC completamente. A nova configuração irá:

-   Manter o serviço do UAC em execução.

-   Causa a solicitação de elevação todos iniciada por administradores ser aprovada automaticamente sem mostrar um UAC prompt.

-   Nega automaticamente todas as solicitações de elevação para usuários padrão.

> [!IMPORTANT]
> Para desabilitar por completo o UAC, você deve desabilitar a política **controle de conta de usuário: executar todos os administradores no modo de aprovação de administrador**.

> [!WARNING]
> Aplicativos personalizados não funcionará no Windows Server 2012 quando o UAC está desabilitado.

### <a name="virtualization"></a>Virtualização
Como os administradores do sistema em ambientes corporativos tentam proteger sistemas, muitos aplicativos do linha de negócios (LOB) são projetados para usar somente um token de acesso de usuário padrão. Como resultado, os administradores de TI não precisa substituir a maioria dos aplicativos ao executar o Windows Server 2012 com o UAC habilitado.

 Windows Server 2012 inclui uma tecnologia de virtualização de arquivos e do registro para aplicativos que não são compatíveis com o UAC e que exigem o token de acesso do administrador para serem executados corretamente. Virtualização garante que até mesmo aplicativos que não são compatíveis com o UAC são compatíveis com o Windows Server 2012. Quando um aplicativo administrativo que não é compatível com o UAC tenta gravar em um diretório protegido, como arquivos de programas, o UAC dá ao aplicativo a própria exibição virtualizada do recurso que está tentando alterar. A cópia virtualizada é mantida no perfil do usuário. Essa estratégia cria uma cópia separada do arquivo virtualizado para cada usuário que executa o aplicativo não compatível.

A maioria das tarefas de aplicativo funciona corretamente usando recursos de virtualização. Embora a virtualização permite que a maioria dos aplicativos sejam executados, é uma correção a curto prazo e não uma solução a longo prazo. Os desenvolvedores de aplicativos devem modificar seus aplicativos para ser compatível com o programa de logotipo do Windows Server 2012 assim que possível, em vez de depender de virtualização de arquivos, pastas e registro.

A virtualização não estiver na opção nos seguintes cenários:

1.  A virtualização não se aplica a aplicativos elevados e que são executados com um token de acesso administrativo completo.

2.  Virtualização dá suporte a aplicativos de 32 bits somente. Aplicativos de 64 bits não elevados simplesmente recebem uma mensagem de acesso negado quando tentam adquirir um identificador (um identificador exclusivo) para um objeto Windows. Aplicativos nativos do Windows de 64 bits são necessários para ser compatível com o UAC e gravar dados nos locais corretos.

3.  A virtualização é desabilitada para um aplicativo se o aplicativo incluir um manifesto de aplicativo com um atributo de nível de execução solicitado.

### <a name="request-execution-levels"></a>Níveis de execução de solicitação
Um manifesto de aplicativo é um arquivo XML que descreve e identifica os assemblies lado a lado compartilhados e privados que um aplicativo deve se associar em tempo de execução. No Windows Server 2012, o manifesto do aplicativo inclui entradas para fins de compatibilidade de aplicativo do UAC. Aplicativos administrativos que incluam uma entrada no manifesto do aplicativo solicitam ao usuário permissão acessar o token de acesso do usuário. Embora eles não ter uma entrada no manifesto do aplicativo, a maioria dos aplicativos administrativos podem executar sem modificação usando correções de compatibilidade de aplicativos. Correções de compatibilidade do aplicativo são entradas de banco de dados que permitem que aplicativos que não são compatíveis com o UAC para funcionar corretamente com o Windows Server 2012.

Todos os aplicativos compatíveis com o UAC devem ter um nível de execução solicitado adicionado ao manifesto do aplicativo. Se o aplicativo exija acesso administrativo ao sistema, marcar o aplicativo com uma execução solicitado nível de "exigir administrador" garante que o sistema identifique esse programa como um aplicativo administrativo e executa as etapas de elevação necessárias. De execução solicitados níveis especificam os privilégios necessários para um aplicativo.

### <a name="installer-detection-technology"></a>Tecnologia de detecção do instalador
Programas de instalação são aplicativos projetados para implantar o software. A maioria dos programas de instalação gravam em diretórios do sistema e chaves do registro. Esses locais protegidos do sistema são costumam ser graváveis apenas por um administrador na tecnologia de detecção do instalador, o que significa que os usuários padrão não têm acesso suficiente para instalar programas. Windows Server 2012 detecta heuristicamente programas de instalação e solicita credenciais de administrador ou aprovação do usuário do administrador para executar com privilégios de acesso. Windows Server 2012 também detecta heuristicamente atualizações e programas que desinstalam aplicativos. Uma das metas de design do UAC é evitar que instalações sejam executadas sem o conhecimento do usuário e consentimento porque programas de instalação gravam em áreas protegidas do sistema de arquivos e do registro.

A detecção do instalador só se aplica a:

-   arquivos executáveis de 32 bits.

-   Aplicativos sem um atributo de nível de execução solicitado.

-   Processos interativos em execução como usuário padrão com o UAC habilitado.

Antes de um processo de 32 bits é criado, os seguintes atributos são verificados para determinar se ele é um instalador:

-   O nome do arquivo inclui palavras-chave como "instalar", "configuração" ou "atualização".

-   Campos do recurso de controle de versão contêm as seguintes palavras-chave: fornecedor, nome da empresa, nome do produto, descrição do arquivo, nome do arquivo Original, nome interno e nome de exportação.

-   Palavras-chave no manifesto lado a lado são incorporadas no arquivo executável.

-   Palavras-chave em entradas específicas de StringTable estão vinculadas no arquivo executável.

-   Os atributos-chave nos dados de script do recurso são vinculados no arquivo executável.

-   Há sequências de bytes dentro do arquivo executável direcionadas.

> [!NOTE]
> As palavras-chave e as sequências de bytes foram derivadas de características comuns observadas em diversas tecnologias de instalador.

> [!NOTE]
> O controle de conta de usuário: Detectar instalações de aplicativos e a solicitação de elevação deve ser habilitada para a detecção do instalador detectar programas de instalação. Essa configuração é habilitada por padrão e pode ser definida localmente usando o snap-in política de segurança Local (secpol. msc) ou configurada para o domínio, unidade Organizacional ou grupos específicos pela política de grupo (Gpedit.msc).


