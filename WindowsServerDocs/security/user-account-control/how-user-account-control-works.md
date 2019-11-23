---
title: Como funciona o Controle de Conta de Usuário
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f12a690c06a37a6e1c01674bcdb96d0a9010a245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403338"
---
# <a name="how-user-account-control-works"></a>Como funciona o Controle de Conta de Usuário

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O UAC (Controle de Conta de Usuário) ajuda a impedir que programas mal-intencionados (também chamados de malware) danifiquem um computador. Além disso, ele ajuda as organizações a implantarem uma área de trabalho com melhor gerenciamento. Com o UAC, os aplicativos e as tarefas são sempre executados no contexto de segurança de uma conta que não é de administrador, a não ser que um administrador autorize especificamente o acesso no nível do administrador ao sistema. O UAC pode bloquear a instalação automática de aplicativos não autorizados e evitar alterações involuntárias nas configurações do sistema.

## <a name="uac-process-and-interactions"></a>Processo e interações do UAC
Cada aplicativo que requer o token de acesso de administrador deve solicitar o consentimento ao administrador. A única exceção é a relação existente entre o processo pai e os processos filho. Os processos filho herdam o token de acesso de usuário do processo pai. Porém, tanto o processo pai como os processos filho devem ter o mesmo nível de integridade. O Windows Server 2012 protege os processos marcando seus níveis de integridade. Esses níveis são medidas de confiabilidade. Um aplicativo de "alta" integridade é aquele que executa tarefas que modificam dados do sistema, como um aplicativo de particionamento de disco; já um aplicativo de "baixa" integridade é aquele que executa tarefas que têm o potencial de comprometer o sistema operacional, como um navegador da Web. Os aplicativos com níveis de integridade mais baixos não podem modificar dados dos aplicativos com níveis de integridade mais altos. Quando um usuário padrão tenta executar um aplicativo que requer um token de acesso de administrador, o UAC requer que o usuário forneça credenciais de administrador válidas.

Para entender melhor como esse processo acontece é importante examinar os detalhes do processo de logon do Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processo de logon do Windows Server 2012
A ilustração a seguir demonstra como o processo de logon difere para um administrador e para um usuário padrão.

![Ilustração que demonstra como o processo de logon de um administrador difere do processo de logon para um usuário padrão](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Por padrão, os usuários padrão e os administradores acessam recursos e executam aplicativos no contexto de segurança dos usuários padrão. Quando um usuário faz logon em um computador, o sistema cria um token de acesso para ele. O token de acesso contém informações sobre o nível de acesso que é concedido ao usuário, incluindo SIDs (identificadores de segurança) e privilégios do Windows específicos.

Quando um administrador faz logon, dois tokens de acesso separados são criados para o usuário: um de usuário padrão e outro de administrador. O token de acesso de usuário padrão contém as mesmas informações específicas ao usuário que o token de acesso de administrador, mas os SIDs e os privilégios administrativos do Windows são removidos. O token de acesso de usuário padrão é utilizado para iniciar aplicativos que não realizam tarefas administrativas (aplicativos de usuário padrão). Em seguida, ele é utilizado para exibir a área de trabalho (Explorer.exe). Explorer.exe é o processo pai do qual todos os outros processos iniciados pelo usuário herdam o token de acesso. Como resultado, todos os aplicativos são executados como um usuário padrão, a não ser que um usuário tenha consentimento ou credenciais para aprovar um aplicativo para usar um token de acesso administrativo completo.

Um usuário que seja membro do grupo Administradores pode fazer logon, navegar na Web e ler emails enquanto usam um token de acesso de usuário padrão. Quando o administrador precisa executar uma tarefa que requer o token de acesso de administrador, o Windows Server 2012 solicita automaticamente a aprovação do usuário. Essa solicitação é chamada de solicitação de elevação e seu comportamento pode ser configurado por meio do snap-in de Política de Segurança Local (Secpol.msc) ou da Política de Grupo.

> [!NOTE]
> O termo "elevar" é usado para fazer referência ao processo no Windows Server 2012 que solicita ao usuário consentimento ou credenciais para usar um token de acesso de administrador completo.

### <a name="the-uac-user-experience"></a>A experiência do usuário do UAC
Quando o UAC está habilitado, a experiência dos usuários padrão é diferente daquela dos administradores no Modo de Aprovação de Administrador. O método recomendado e mais seguro de executar o Windows Server 2012 é tornar sua conta de usuário principal uma conta de usuário padrão. A execução como um usuário padrão ajuda a maximizar a segurança em um ambiente gerenciado. Com o componente interno de elevação do UAC, os usuários padrão podem facilmente realizar uma tarefa administrativa inserindo as credenciais válidas de uma conta de administrador local. O componente interno padrão de elevação do UAC para os usuários padrão é a solicitação de credenciais.

A alternativa à execução como um usuário padrão é a execução como um administrador no Modo de Aprovação de Administrador. Com o componente de elevação do UAC interno, membros do grupo Administradores local podem realizar facilmente uma tarefa administrativa dando a aprovação. O componente interno padrão de elevação do UAC para uma conta de administrador no Modo de Aprovação de Administrador é chamado de solicitação de consentimento. O comportamento da solicitação de elevação do UAC pode ser configurado por meio do snap-in de Política de Segurança Local (Secpol.msc) ou da Política de Grupo.

**As solicitações de consentimento e credencial**

Com o UAC habilitado, o Windows Server 2012 solicita consentimento ou solicita credenciais de uma conta de administrador local válida antes de iniciar um programa ou uma tarefa que exija um token de acesso de administrador completo. Essa solicitação assegura que nenhum software mal-intencionado possa ser instalado silenciosamente.

**A solicitação de consentimento**

A solicitação de consentimento é apresentada quando um usuário tenta realizar uma tarefa que requer um token de acesso administrativo do usuário. A imagem a seguir é uma captura de tela da solicitação de consentimento do UAC.

![Captura de tela do prompt de consentimento do UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**A solicitação de credenciais**

A solicitação de credenciais é apresentada quando um usuário padrão tenta realizar uma tarefa que requer um token de acesso administrativo do usuário. O comportamento dessa solicitação padrão do usuário pode ser configurado por meio do snap-in de Política de Segurança Local (Secpol.msc) ou da Política de Grupo. Os administradores também podem ser solicitados a fornecer suas credenciais Configurando o controle de conta de usuário: comportamento do prompt de elevação para administradores no modo de aprovação de administrador valor da configuração de política para solicitar credenciais.

A captura de tela a seguir é um exemplo da solicitação de credenciais do UAC.

![Captura de tela mostrando um exemplo do prompt de credenciais do UAC](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Prompts de elevação do UAC**

As solicitações de elevação do UAC são codificadas por cores para serem específicas ao aplicativo, o que permite a identificação imediata do risco de segurança potencial de um aplicativo. Quando um aplicativo tenta ser executado com um token de acesso completo do administrador, o Windows Server 2012 primeiro analisa o arquivo executável para determinar seu editor. Os aplicativos são primeiro separados em três categorias com base no editor do arquivo executável: Windows Server 2012, Publicador verificado (assinado) e Publicador não verificado (não assinado). O diagrama a seguir ilustra como o Windows Server 2012 determina qual prompt de elevação de cor deve ser apresentado ao usuário.

A codificação de cores das solicitações de elevação é a seguinte:

-   Fundo vermelho com um ícone de escudo vermelho: o aplicativo está bloqueado pela Política de Grupo ou é proveniente de um editor que está bloqueado.

-   Plano de fundo azul com um ícone de escudo azul e dourado: o aplicativo é um aplicativo administrativo do Windows Server 2012, como um item do painel de controle.

-   Fundo azul com um ícone de escudo azul: o aplicativo está assinado por meio de Authenticode e é confiável para o computador local.

-   Fundo amarelo com um ícone de escudo amarelo: o aplicativo não está assinado, ou está assinado mas ainda não é confiável para o computador local.

**Ícone de escudo**

Alguns itens do Painel de Controle, como **Propriedades de data e hora**, contêm uma combinação de operações de administrador e de usuário padrão. Os usuários padrão podem ver o relógio e alterar o fuso horário, mas um token de acesso de administrador completo é necessário para alterar a hora local do sistema. A imagem a seguir é uma captura de tela do item do Painel de Controle **Propriedades de data e hora**.

![Captura de tela mostrando o * * Propriedades de data e hora * * painel de controle](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

O ícone de escudo no botão **Alterar data e hora** indica que o processo requer um token de acesso de administrador completo e exibirá uma solicitação de elevação do UAC.

**Protegendo a solicitação de elevação**

O processo de elevação tem uma proteção extra com o direcionamento da solicitação para a área de trabalho protegida. Os prompts de consentimento e credencial são exibidos na área de trabalho segura por padrão no Windows Server 2012. Somente os processos do Windows podem acessar a área de trabalho protegida. Para níveis mais altos de segurança, é recomendável manter a configuração de política **Controle de Conta de Usuário: alternar para a área de trabalho segura ao pedir elevação** habilitada.

Quando um arquivo executável solicita elevação, a área de trabalho interativa, também chamada de área de trabalho do usuário, é trocada pela área de trabalho protegida. Esta esmaece a área de trabalho do usuário e exibe uma solicitação de elevação que deverá ser respondida para o processo continuar. Quando o usuário clica em Sim ou não, a área de trabalho muda para a área de trabalho do usuário.

O malware pode apresentar uma imitação da área de trabalho segura, mas quando a configuração de diretiva de usuário controle de conta de usuários: comportamento da solicitação de elevação para administradores no modo de aprovação de administrador estiver definida como solicitar consentimento, o malware não obterá elevação se o usuário clicar em Sim no imitação. Se a configuração de política for definida como solicitar credenciais, o malware que imita a solicitação de credenciais poderá coletar as credenciais do usuário. Porém, o malware não ganhará privilégios elevados, e o sistema tem outras proteções que reduzem a possibilidade de o malware assumir o controle da interface do usuário mesmo com uma senha coletada.

Embora um malware possa apresentar uma imitação da área de trabalho protegida, esse problema não poderá ocorrer a menos que um usuário tenha instalado o malware previamente no computador. Como os processos que requerem um token de acesso de administrador não podem ser instalados silenciosamente quando o UAC está habilitado, o usuário deverá providenciar o consentimento explicitamente clicando em **Sim** ou fornecendo credenciais de administrador. O comportamento específico da solicitação de elevação do UAC depende da Política de Grupo.

## <a name="uac-architecture"></a>Arquitetura do UAC
O diagrama a seguir fornece detalhes sobre a arquitetura do UAC.

![Diagrama detalhando a arquitetura do UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Para entender melhor cada componente, examine a tabela abaixo:

|Componente|Descrição|
|-------|--------|
|**Usuário**||
|Usuário realiza uma operação que exige um privilégio|Se o operador alterar o sistema de arquivos ou o registro, Virtualization será chamado. Todas as outras operações chamam ShellExecute.|
|ShellExecute|ShellExecute chama CreateProcess. ShellExecute procura o erro ERROR_ELEVATION_REQUIRED em CreateProcess. Se receber o erro, ShellExecute chamará o serviço de Informações de Aplicativos para tentar realizar a tarefa solicitada com o prompt de privilégios elevados.|
|CreateProcess|Se o aplicativo exigir elevação, CreateProcess rejeitará a chamada com ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Serviço de Informações de Aplicativos|Um serviço do sistema que ajuda a iniciar aplicativos que exigem um ou mais direitos de usuário ou privilégios elevados para serem executados, como tarefas administrativas locais, e aplicativos que exigem níveis maiores de integridade. O Serviço de Informações de Aplicativos ajuda a iniciar esses aplicativos criando novos processos para eles com o token de acesso completo de um usuário administrativo quando a elevação é necessária e (dependendo da Política de Grupo) e quando o usuário deu o seu consentimento para isso.|
|Elevação de uma instalação do ActiveX|Se o ActiveX não estiver instalado, o sistema verificará o nível do controle deslizante do UAC. Se o ActiveX estiver instalado, a configuração de Política de Grupo **Controle de Conta de Usuário: alternar para a área de trabalho segura ao pedir elevação** estará marcada.|
|Verificar nível do controle deslizante do UAC|Agora, o UAC tem quatro níveis de notificação para seleção, além de um controle deslizante que deve ser usado para escolher o nível de notificação:<br /><br /><ul><li>Alto<br /><br />    Se o controle deslizante estiver definido como **Sempre notificar**, o sistema verificará se a área de trabalho protegida está habilitada.</li><li>Médio<br /><br />    Se o controle deslizante estiver definido como **Padrão - Notificar-me somente quando os programas tentarem fazer alterações no meu computador**, a configuração de política **Controle de Conta de Usuário: elevar somente executáveis assinados e validados** estará marcada:<br /><br /><ul><li>Se essa configuração de política estiver habilitada, a validação do caminho de certificação de PKI (infraestrutura de chave pública) será imposta para um arquivo executável específico antes que ele possa ser executado.</li><li>Se essa configuração de política não estiver habilitada, a validação do caminho de certificação de PKI não será imposta para que um arquivo executável específico possa ser executado. A configuração de Política de Grupo **Controle de Conta de Usuário: alternar para a área de trabalho segura ao pedir elevação** está marcada.</li></ul></li><li>Baixa<br /><br />    Se o controle deslizante estiver definido como **Notificar somente quando programas tentarem fazer alterações no meu computador (não esmaecer a área de trabalho)** , CreateProcess será chamado.</li><li>Nunca notificar<br /><br />    Se o controle deslizante estiver definido como **nunca notificar quando**, o prompt do UAC nunca notificará quando um programa estiver tentando instalar ou tentar fazer qualquer alteração no computador. **Importante:**     Essa configuração não é recomendada. Essa configuração é igual à configuração da política **Controle de Conta de Usuário: comportamento da solicitação de elevação de administradores no Modo de Aprovação de Administrador** como **Elevate sem aviso**.</li></ul>|
|Área de trabalho protegida habilitada|A configuração de política **Controle de Conta de Usuário: alternar para a área de trabalho segura ao pedir elevação** está marcada:<br /><br />-Se a área de trabalho segura estiver habilitada, todas as solicitações de elevação vão para a área de trabalho segura independentemente das configurações de política de comportamento de prompt para administradores e usuários padrão.<br />-Se a área de trabalho segura não estiver habilitada, todas as solicitações de elevação vão para a área de trabalho do usuário interativo e as configurações por usuário para administradores e usuários padrão são usadas.|
|CreateProcess|CreateProcess chama AppCompat, Fusion e a detecção de Instalador para avaliar se o aplicativo requer elevação. Em seguida, o arquivo executável é inspecionado para determinar seu nível de execução solicitado, que é armazenado no manifesto do aplicativo para o arquivo executável. CreateProcess falhará se o nível de execução solicitado que foi especificado no manifesto não corresponder ao token de acesso, retornando um erro (ERROR_ELEVATION_REQUIRED) para ShellExecute. |
|AppCompat|O banco de dados AppCompat armazena informações nas entradas de correção de compatibilidade de um aplicativo.|
|Fusão|O banco de dados Fusion armazena informações de manifestos de aplicativo que descrevem os aplicativos. O esquema de manifestos é atualizado para adicionar um novo campo de nível de execução solicitado.|
|Detecção do instalador|A detecção de Instalador detecta arquivos executáveis de uma instalação, ajudando a impedir que instalações sejam executadas sem o conhecimento e o consentimento do usuário.|
|**Kernel**||
|Virtualização|A tecnologia de Virtualização garante que aplicativos incompatíveis não apresentem falhas de execução silenciosas ou cuja causa raiz não possa ser determinada. O UAC também fornece virtualização e registro em log de arquivos e do Registro para aplicativos que gravam em áreas protegidas.|
|Sistema de arquivos e Registro|A virtualização de arquivos e do Registro por usuário redireciona solicitações de gravação de arquivos e do Registro por computador para localizações equivalentes por usuário. Solicitações de leitura são redirecionadas primeiramente à localização por usuário virtualizada e depois à localização por computador.|

Há uma alteração no UAC do Windows Server 2012 de versões anteriores do Windows. O novo controle deslizante nunca desativará completamente o UAC. A nova configuração:

-   Manter o serviço do UAC em execução.

-   Fazer toda a solicitação de elevação iniciada por administradores ser aprovada automaticamente sem mostrar uma solicitação do UAC.

-   Negar automaticamente todas as solicitações de elevação para usuários padrão.

> [!IMPORTANT]
> Para desabilitar por completo o UAC, você deve desabilitar a política **Controle de Conta de Usuário: executar todos os administradores no Modo de Aprovação de Administrador**.

> [!WARNING]
> Os aplicativos personalizados não funcionarão no Windows Server 2012 quando o UAC estiver desabilitado.

### <a name="virtualization"></a>Virtualização
Como os administradores de sistemas em ambientes corporativos tentam proteger seus sistemas, muitos aplicativos de LOB (linha de negócios) são projetados para usarem apenas um token de acesso de usuário padrão. Como resultado, os administradores de ti não precisam substituir a maioria dos aplicativos ao executarem o Windows Server 2012 com o UAC habilitado.

 O Windows Server 2012 inclui a tecnologia de virtualização de arquivo e registro para aplicativos que não são compatíveis com o UAC e que exigem o token de acesso de um administrador para serem executados corretamente. A virtualização garante que até mesmo aplicativos que não são compatíveis com o UAC são compatíveis com o Windows Server 2012. Quando um aplicativo administrativo incompatível com o UAC tenta gravar em um diretório protegido, como a pasta Arquivos de Programas, o UAC apresenta a esse aplicativo sua própria exibição virtualizada do recurso que ele está tentando alterar. A cópia virtualizada é mantida no perfil do usuário. Essa estratégia cria uma cópia separada do arquivo virtualizado para cada usuário que executa o aplicativo incompatível.

A maioria das tarefas de aplicativos consegue operar corretamente com o uso de recursos de virtualização. Embora a virtualização permita a execução de uma ampla maioria de aplicativos, trata-se de uma correção a curto prazo, e não de uma solução a longo prazo. Os desenvolvedores de aplicativos devem modificar seus aplicativos para que estejam em conformidade com o programa de logotipo do Windows Server 2012 assim que possível, em vez de depender do arquivo, da pasta e da virtualização do registro.

A virtualização não é uma opção nos seguintes cenários:

1.  A virtualização não é válida para aplicativos que possuem privilégios elevados e são executados com um token de acesso administrativo completo.

2.  A virtualização apenas oferece suporte para aplicativos de 32 bits. Aplicativos de 64 bits sem privilégios elevados simplesmente recebem uma mensagem de acesso negado quando tentam adquirir um identificador exclusivo para um objeto do Windows. Aplicativos de 64 bits nativos do Windows devem ser compatíveis com o UAC e gravar dados nas localizações corretas.

3.  A virtualização ficará desabilitada para um aplicativo se este incluir um manifesto de aplicativos com um atributo de nível de execução solicitado.

### <a name="request-execution-levels"></a>Níveis de execução de solicitação
Um manifesto de aplicativo é um arquivo XML que descreve e identifica os assemblies lado a lado particulares e compartilhados aos quais um aplicativo deve se associar em tempo de execução. No Windows Server 2012, o manifesto do aplicativo inclui entradas para fins de compatibilidade de aplicativos do UAC. Aplicativos administrativos que incluem uma entrada no manifesto de aplicativos solicitam a permissão do usuário para obter o token de acesso desse usuário. Embora eles não tenham uma entrada no manifesto de aplicativos, a maioria desses aplicativos administrativos pode ser executada sem modificação, com o uso de correções de compatibilidade de aplicativo. Correções de compatibilidade de aplicativos são entradas de banco de dados que permitem que aplicativos que não são compatíveis com o UAC funcionem corretamente com o Windows Server 2012.

Todos os aplicativos compatíveis com o UAC devem ter um nível de execução solicitado adicionado ao manifesto de aplicativos. Se o aplicativo exigir acesso administrativo ao sistema, marcá-lo com um nível de execução solicitado de "exigir administrador" garantirá que o sistema consiga identificar esse programa como um aplicativo administrativo e realizar as etapas de elevação necessárias. Níveis de execução solicitados especificam os privilégios necessários para um aplicativo.

### <a name="installer-detection-technology"></a>Tecnologia de detecção de instalador
Programas de instalação são aplicativos projetados para implantar softwares. A maioria dos programas de instalação grava em diretórios do sistema e chaves do Registro. Em geral, essas localizações protegidas do sistema somente podem ser gravadas por um administrador na tecnologia de detecção de Instalador, o que significa que os usuários padrão não têm acesso suficiente para instalar programas. O Windows Server 2012 detecta heurísticamente programas de instalação e solicita credenciais de administrador ou a aprovação do usuário administrador para executar com privilégios de acesso. O Windows Server 2012 também detecta heurísticamente atualizações e programas que desinstalam aplicativos. Uma das metas de design do UAC é impedir que instalações sejam executadas sem o conhecimento e o consentimento do usuário, uma vez que os programas de instalação gravam em áreas protegidas do sistema de arquivos e do registro.

A detecção de Instalador apenas se aplica ao seguinte:

-   Arquivos executáveis de 32 bits.

-   Aplicativos sem um atributo de nível de execução solicitado.

-   Processos interativos em execução como um usuário padrão com o UAC habilitado.

Antes de um processo de 32 bits ser criado, os seguintes atributos são verificados para determinar se ele é um instalador:

-   O nome do arquivo inclui palavras-chave, como "instalação", "configuração" ou "atualização".

-   Campos de Recursos de Controle de Versão contêm as seguintes palavras-chave: Fornecedor, Nome da Empresa, Nome do Produto, Descrição do Arquivo, Nome do Arquivo Original, Nome Interno e Nome de Exportação.

-   Palavras-chave no manifesto lado a lado estão inseridas no arquivo executável.

-   Palavras-chave em entradas StringTable específicas estão vinculadas no arquivo executável.

-   Os atributos de chave nos dados de script de recurso são vinculados no arquivo executável.

-   Existem sequências de bytes direcionadas no arquivo executável.

> [!NOTE]
> As palavras-chave e as sequências de butes foram derivadas de características comuns observadas em várias tecnologias de Instalador.

> [!NOTE]
> A configuração de política Controle de Conta de Usuário: detectar instalações de aplicativos e perguntar se deseja elevar deve estar habilitada para a detecção do instalador detectar programas de instalação. Essa configuração está habilitada por padrão e pode ser definida localmente com o uso do snap-in Política de Segurança Local (Secpol.msc) ou definida para o domínio, a unidade organizacional ou grupos específicas pela Política de Grupo (Gpedit.msc).


