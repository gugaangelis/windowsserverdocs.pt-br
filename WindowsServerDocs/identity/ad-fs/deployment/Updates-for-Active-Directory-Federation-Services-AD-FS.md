---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: "Atualizações necessárias para serviços de Federação do Active Directory (AD FS)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Atualizações necessárias para serviços de Federação do Active Directory (AD FS) e o Proxy de aplicativo Web (WAP)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1

A partir de outubro de 2016, todos os componentes do Windows Server, todas as atualizações são liberadas apenas via Windows Update (WU).  Há mais nenhuma hotfixes ou downloads individuais.
Isso se aplica ao Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 SP1.

Esta página lista os pacotes cumulativos de atualizações de interesse específico para o AD FS e WAP, bem como a lista histórica de atualizações de hotfix recomendado para o AD FS e WAP.

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Atualizações para o AD FS e WAP no Windows Server 2016
Atualizações para o Windows Server 2016 são entregues mensalmente via Windows Update e são cumulativas. O pacote de atualização listado abaixo é recomendado para todos os servidores do AD FS e WAP 2016 e inclui todas as atualizações necessárias anteriormente, bem como as correções mais recentes.

|KB # |Descrição|Data do lançamento
|----- | ----- |-----
|[4041688 (Build SO 14393.1794)](https://support.microsoft.com/kb/4041688)|Essa correção resolve um problema que intermitentemente misdirects solicitações de anúncio autoridade para o provedor de identidade errado por causa de comportamento de cache incorreto. Isso pode afetar a recursos de autenticação como Multi fator de autenticação. </br></br>Adicionada a capacidade AAD conectar integridade relatório de integridade do servidor ADFS com fidelidade correta (usando auditoria detalhada) em farms WS2012R2 e WS2016 ADFS mistos.</br></br>Corrigido um problema onde durante a atualização do 2012 R2 ADFS farm para o AD FS 2016, o cmdlet do powershell para elevar o nível de comportamento farm falha com um tempo limite quando há muitos terceira relações de confiança de terceiros.</br></br>Abordados um problema onde o AD FS causa falhas de autenticação modificando o valor do parâmetro wct enquanto federar as solicitações para outro servidor STS (Security Token).|Outubro de 2017|
|[4038801 (Build SO 14393.1737)](https://support.microsoft.com/kb/4038801)|Suporte adicionado para log OIDC usando LDPs federados. Isso permitirá que "Cenários de quiosque" onde vários usuários podem ser registrados serialmente em um único dispositivo onde há federação com um LDP.</br></br>Corrigido um problema onde o CEP/CES com base em certificados não funcionam com contas gMSA de WinHello.</br></br>Corrige um problema em que o banco de dados interno de Windows (trabalho) em servidores do Windows Server 2016 ADFS falha sincronizar algumas configurações, como as colunas ApplicationGroupId das tabelas IdentityServerPolicy.Scopes e IdentityServerPolicy.Clients) devido a uma chave externa restrição. Essas falhas de sincronização pode causar reivindicação diferente, reivindicar experiências de provedor e o aplicativo entre servidores ADFS principais para secundário. Além disso, se a função principal do trabalho é movida para um nó secundário, grupos de aplicativos não estará mais gerenciáveis no gerenciamento ADFS experiência do usuário.</br></br>Esta atualização corrige problemas onde Multi fator de autenticação não funciona corretamente com dispositivos móveis que utilizam as definições de cultura personalizada|Setembro de 2017|
|[4034661 (Build SO 14393.1613)](https://support.microsoft.com/kb/4034661)|Corrige um problema onde o endereço IP do chamador é nog registrado pelos 411 eventos no log de eventos de segurança do AD FS 4.0 \ servidores do Windows Server 2016 RS1 ADFS mesmo depois de habilitar "auditorias de êxitos" e "auditorias de falhas".</br></br>Essa correção resolve um problema com o Azure Multi Factor Authentication (MFA) quando um servidor ADFX é configurado para usar um HTTP Proxy.</br></br>"Abordados um problema onde a apresentação de um certificado expirado ou revogado para o servidor Proxy ADFS não retornam um erro para o usuário".|Agosto de 2017|
|[4034658 (Build SO 14393.1593)](https://support.microsoft.com/kb/4034658)|Correção para AD 2016 servidor FS para dar suporte ao registro de certificado MFA para Windows Hello para empresas para em implantações locais|Agosto de 2017| 
|[4025334 (Build SO 14393.1532)](https://support.microsoft.com/kb/4025334)|Abordados um problema onde o manipulador de token PkeyAuth pode falhar uma autenticação se a solicitação de pkeyauth contiver dados incorretos. A autenticação ainda deve continuar sem executar a autenticação de dispositivo|Julho de 2017|
|[4022723 (Build SO 14393.1378)](https://support.microsoft.com/kb/4022723)|[Proxy de aplicativo web] Valor da propriedade de configuração DisableHttpOnlyCookieProtection não será escolhido pelo WAP 2016 em implantação mista 2012R2/2016 </br></br>[Proxy de aplicativo web] Não é possível obter o token de acesso do usuário do AD FS em cenários EAS pré-auth.</br></br>AD FS 2016: WSFED saída leva a uma exceção|Junho de 2017 
|[3213986](https://support.microsoft.com/kb/3213986)|Atualização cumulativa para o Windows Server 2016 para sistemas baseados em x64 (KB3213986)| Janeiro de 2017

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Atualizações para o AD FS e WAP no Windows Server 2012 R2
A seguir está a lista de pacotes cumulativos de hotfixes e update que foram lançadas para os serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2.

|KB # |Descrição|Data do lançamento
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|Abordados um problema do AD FS onde MSISConext cookies em cabeçalhos de solicitação eventualmente podem estourar o limite de tamanho de cabeçalhos e causar falha autenticar com o código de status HTTP 400 "Ruim solicitação – cabeçalho muito tempo".</br></br>Corrigido um problema onde ADFS não podem mais ignorar "prompt = logon" durante a autenticação. Uma opção "Desabilitado" foi adicionada para restaurar cenários em que a autenticação de senha não é usada.|Visualização de outubro de 2017 cumulativo|
|[4019217](https://support.microsoft.com/kb/4019217)|Os clientes usando o token broker não funciona quando se usa um servidor Server 2012 R2 AD FS de pastas de trabalho|Pacote cumulativo de maio de 2017 Preview| 
|[4015550](https://support.microsoft.com/kb/4015550)|Corrigido um problema com o AD FS não autenticar usuários externos e AD FS WAP falha aleatoriamente encaminhar solicitação|Pacote cumulativo de abril de 2017| 
|[4015547](https://support.microsoft.com/kb/4015547)|Corrigido um problema com o AD FS não autenticar usuários externos e AD FS WAP falha aleatoriamente encaminhar solicitação|Atualização de segurança de abril de 2017| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 esta atualização de segurança resolve uma vulnerabilidade no Active Directory federação Services (AD FS). A vulnerabilidade pode permitir a divulgação de informações se um invasor envia uma solicitação específica para um servidor do AD FS, permitindo que o invasor ler informações importantes sobre o sistema de destino.|Pacote cumulativo de março de 2017| 
|[3179574](https://support.microsoft.com/kb/3179574)|Corrigido o problema com a atualização de senha extranet AD FS. |Pacote cumulativo de agosto de 2016
|[3172614](https://support.microsoft.com/kb/3172614)|Prompt introduzido = logon [suporte](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7), corrigido o problema com a configuração de AlwaysRequireAuthentication e o console de gerenciamento do AD FS. |Pacote cumulativo de julho de 2016
|[3163306](https://support.microsoft.com/kb/3163306)|Os serviços de Federação do Active Directory (AD FS) 3.0 não consegue se conectar às lojas de atributo Lightweight Directory Access Protocol (LDAP) que estão configuradas para usar Secure Sockets Layer (SSL) porta 636 ou 3269 na cadeia de caracteres de conexão. |Pacote cumulativo de junho de 2016
|[3148533](https://support.microsoft.com/kb/3148533)|Autenticação de fallback MFA falhar por meio de Proxy do AD FS no Windows Server 2012 R2 |Maio de 2016
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS logs não contêm o endereço IP do cliente para cenários de bloqueio de conta no Windows Server 2012 R2 |Fevereiro de 2016
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020: Atualização de segurança para serviços de Federação do Active Directory a negação de endereço de serviço: 9 de fevereiro de 2016|Fevereiro de 2016
|[3105881](https://support.microsoft.com/kb/3105881)|Não pode acessar aplicativos quando a autenticação de dispositivo é habilitada no servidor baseado em Windows Server 2012 R2 AD FS|Outubro de 2015
|[3092003](https://support.microsoft.com/kb/3092003)|A página é carregada repetidamente e a autenticação falhar quando os usuários utilizam MFA no Windows Server 2012 R2 AD FS|Agosto de 2015
|[3080778](https://support.microsoft.com/kb/3080778)|AD FS não chamar OnError quando o adaptador MFA lança uma exceção no Windows Server 2012 R2|Julho de 2015
|[3075610](https://support.microsoft.com/kb/3075610)|Relações de confiança são perdidas no servidor do AD FS secundário depois de adicionar ou remover o provedor de declarações no Windows Server 2012 R2|Julho de 2015
|[3070080](https://support.microsoft.com/kb/3070080)|Descobrindo doméstica território não está funcionando corretamente não declarações ciente dependência terceiros confiar|Junho de 2015
|[3052122](https://support.microsoft.com/kb/3052122)|Atualização adiciona suporte para composta declarações de ID em tokens do AD FS no Windows Server 2012 R2|Maio de 2015
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Vulnerabilidade nos serviços de Federação do Active Directory pode permitir a divulgação de informações|Abril de 2015
|[3042127](https://support.microsoft.com/kb/3042127)|"HTTP 400 - Solicitação incorreta" erro quando você abre uma caixa de correio compartilhada por meio de WAP no Windows Server 2012 R2|Março de 2015
|[3042121](https://support.microsoft.com/kb/3042121)|Proteção de repetição de token do AD FS para tokens de autenticação de Proxy de aplicativo Web no Windows Server 2012 R2|Março de 2015
|[3035025](https://support.microsoft.com/kb/3035025)|Hotfix senha de atualização de recursos para que os usuários não são necessárias para usar o dispositivo registrado no Windows Server 2012 R2|Janeiro de 2015
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS não pode processar a resposta SAML no Windows Server 2012 R2|Janeiro de 2015
|[3025080](https://support.microsoft.com/kb/3025080)|Operação falhar quando você tenta salvar um arquivo do Office por meio de Proxy de aplicativo Web no Windows Server 2012 R2|Janeiro de 2015
|[3025078](https://support.microsoft.com/kb/3025078)|Você não é apresentada para nome de usuário novamente quando você usar um nome de usuário incorreto para fazer logon no Windows Server 2012 R2|Janeiro de 2015
|[3020813](https://support.microsoft.com/kb/3020813)|Você será solicitado para autenticação quando você executa um aplicativo web no Windows Server 2012 R2 AD FS|Janeiro de 2015
|[3020773](https://support.microsoft.com/kb/3020773)|Falhas de tempo limite após a implantação inicial do serviço de registro de dispositivo no Windows Server 2012 R2|Janeiro de 2015
|[3018886](https://support.microsoft.com/kb/3018886)|Você for solicitado um nome de usuário e senha duas vezes quando você acessar o servidor Windows Server 2012 R2 AD FS de intranet|Janeiro de 2015
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 Update acumulados|Dezembro de 2014
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 Update acumulados|Novembro de 2014
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 Update acumulados|Agosto de 2014
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 Update acumulados|Julho de 2014
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 Update acumulados|Junho de 2014
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 Update acumulados|Maio de 2014
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 Update acumulados|Abril de 2014

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Atualizações do AD FS no Windows Server 2012 (AD FS 2.1) e AD FS 2.0
A seguir está a lista de pacotes cumulativos de hotfixes e atualização foram lançadas para o AD FS 2.0 e 2.1.

|KB # |Descrição|Data do lançamento|Aplica-se a:
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|Autenticação por meio do proxy falhar no Windows Server 2012 (Este é o lançamento geral do hotfix 3094446)|Cumulativo de atualizações de qualidade de novembro de 2016|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|Autenticação por meio do proxy falhar no Windows Server 2008 R2 SP1 (Este é o lançamento geral do hotfix 3094446)|Cumulativo de atualizações de qualidade de novembro de 2016|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|Autenticação por meio do proxy falhar no Windows Server 2012 ou Windows Server 2008 R2 SP1|Setembro de 2015|AD FS 2.0 e 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|AD FS 2.1 gera uma exceção quando você autentica usando um certificado de criptografia no Windows Server 2012|Julho de 2015|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062: Vulnerabilidade nos serviços de Federação do Active Directory pode permitir a elevação de privilégio|Junho de 2015|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Uma vulnerabilidade nos serviços de Federação do Active Directory pode permitir a divulgação de informações: 14 de abril de 2015|Novembro de 2014|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|Uso de memória de servidor de Federação do AD FS continua aumentando quando muitos usuários fazem logon em um aplicativo web no Windows Server 2012|Julho de 2014|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|A terceira confiança de terceiros no AD FS é interrompida quando é feita uma solicitação para o AD FS para um token delegado|Maio de 2014|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|Implantação de farm ADFS SQL falhará se você não tiver permissões de SQL|Outubro de 2014|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713) ou [2989956](https://support.microsoft.com/kb/2989956)|Atualização está disponível para resolver vários problemas depois de instalar a atualização de segurança 2843638 em um servidor do AD FS|Novembro de 2013</br></br>Setembro de 2014|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|Atualização permite que você use um certificado para várias partes Relying confie em um AD FS farm 2.1|Outubro de 2013|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|CORREÇÃO: Um erro ocorre quando você usar um CSP e o HSM de terceiros e, em seguida, configurar uma relação de confiança do provedor de declarações na atualização de Rollup 3 do AD FS 2.0 no Windows Server 2008 R2 Service Pack 1|Setembro de 2013|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|Uma vírgula no nome da entidade de um certificado de criptografia gera uma exceção no Windows Server 2008 R2 SP1|Agosto de 2013|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Security] Vulnerabilidade nos serviços de Federação do Active Directory pode permitir a divulgação de informações|Novembro de 2013|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066: Descrição da atualização de segurança do Active Directory federação Services 2.0: 13 de agosto de 2013|Agosto de 2013|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Arquivo federationmetadata.XML não contém as informações de ponto de extremidade MEX para os pontos de extremidade WS-Trust e WS-Federation no Windows Server 2012|Maio de 2013|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|Descrição do pacote cumulativo de atualizações 3 para serviços de Federação do Active Directory (AD FS) 2.0|Março de 2013|AD FS 2.0




