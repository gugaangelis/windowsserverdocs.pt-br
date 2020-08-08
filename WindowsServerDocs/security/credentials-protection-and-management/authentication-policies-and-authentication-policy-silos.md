---
title: Políticas de autenticação e silos de políticas de autenticação
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ad453c5581f966a2e21a5cd8b4ed1c9cb28fe9a8
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995899"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Políticas de autenticação e silos de políticas de autenticação

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico destinado a profissionais de TI descreve os silos de política de autenticação e as políticas que podem restringir contas a esses silos. Ele também explica como as políticas de autenticação podem ser usadas para restringir o escopo das contas.

Os silos de política de autenticação e as políticas de acompanhamento fornecem uma maneira de conter credenciais de alto privilégio para sistemas que somente são pertinentes aos usuários, computadores ou serviços selecionados. Os silos podem ser definidos e gerenciados no AD DS (Serviços de Domínio Active Directory) usando o Centro Administrativo do Active Directory e os cmdlets do Windows PowerShell do Active Directory.

Os silos de política de autenticação são contêiners aos quais os administradores podem atribuir contas de usuário, de computador e de serviço. Os conjuntos de contas podem, então, ser gerenciados pelas políticas de autenticação aplicadas àquele contêiner. Isso reduz a necessidade de o administrador rastrear o acesso aos recursos para contas individuais e ajuda a evitar que usuários mal-intencionados acessem outros recursos por meio do roubo de credenciais.

Os recursos introduzidos no Windows Server 2012 R2 permitem que você crie silos de política de autenticação, que hospedam um conjunto de usuários de alto privilégio. Você pode, então, atribuir as políticas de autenticação para este contêiner a fim de limitar onde as contas privilegiadas podem ser usadas no domínio. Quando as contas estiverem no grupo de segurança Usuários Protegidos, serão aplicados controles adicionais, como o uso exclusivo do protocolo Kerberos.

Com essas capacidades, você pode limitar o uso de contas de alto valor a hosts de alto valor. Por exemplo, você poderia criar um novo silo de Administradores de Floresta contendo os administradores corporativos, de esquema e de domínio. Depois, você poderia configurar o silo com uma política de autenticação para que a autenticação baseada em senha e cartão inteligente de sistemas diferentes dos controladores de domínio e consoles de administrador do domínio falhassem.

Para mais informações sobre como configurar os silos da política de autenticação e as políticas de autenticação, consulte [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Sobre silos de política de autenticação
Um silo de política de autenticação controla quais contas podem ser restringidas pelo silo e define as políticas de autenticação a serem aplicadas aos membros. Você pode criar o silo com base nos requisitos da sua organização. Os silos são objetos do Active Directory para usuários, computadores e serviços, conforme definido pelo esquema na tabela a seguir.

**Esquema do Active Directory para silos de política de autenticação**

|Nome de exibição|Descrição|
|--------|--------|
|Silo de política de autenticação|Uma instância desta classe define as políticas de autenticação e comportamentos relacionados para usuários, computadores e serviços designados.|
|Silos de política de autenticação|Um contêiner desta classe pode conter objetos do silo de política de autenticação.|
|Silo de política de autenticação imposto|Especifica se o silo de política de autenticação foi imposto.<p>Quando não está imposto, a política fica no modo de auditoria por padrão. Eventos que indicam potenciais sucessos e falhas são gerados, mas as proteções não são aplicadas ao sistema.|
|Vínculo regressivo do silo de política de autenticação atribuído|Este atributo é o vínculo regressivo para msDS-AssignedAuthNPolicySilo.|
|Membros do silo de política de autenticação|Especifica quais entidades são atribuídas ao AuthNPolicySilo.|
|Vínculo regressivo dos membros do silo de política de autenticação|Este atributo é o vínculo regressivo para os msDS-AuthNPolicySiloMembers.|

Os silos de política de autenticação podem ser configurados usando o Console Administrativo do Active Directory ou o Windows PowerShell. Para mais informações, consulte [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Sobre políticas de autenticação
Uma política de autenticação define as propriedades de tempo de vida TGT (tíquete de concessão de tíquete) do protocolo Kerberos e as condições do controle de acesso de autenticação para um tipo de conta. A política é criada e controla o contêiner de AD DS conhecido como silo da política de autenticação.

As políticas de autenticação controlam o seguinte:

-   O tempo de vida de TGT para a conta, que é definido para não renovável.

-   Os critérios que as contas do dispositivo precisam cumprir para entrar com uma senha ou um certificado.

-   Os critérios que os usuários e os dispositivos precisam cumprir para se autenticarem nos serviços executados como parte da conta.

O tipo de conta Active Directory determina a função do chamador como um dos seguintes:

-   **Usuário**

    Os usuários sempre devem ser membros do grupo de segurança Usuários Protegidos, que rejeita por padrão as tentativas de entrar usando NTLM.

    As políticas podem ser configuradas para definir o tempo de vida de TGT de uma conta de usuário com um valor mais curto ou restringir os dispositivos nos quais uma conta de usuário pode entrar. As expressões avançadas podem ser configuradas na política de autenticação para controlar os critérios que os usuários e seus dispositivos precisam cumprir para se autenticarem no serviço.

    Para mais informações, consulte [Grupo de segurança de usuários protegidos](protected-users-security-group.md).

-   **Serviço**

    São usadas contas de serviço gerenciadas autônomas, contas de serviço gerenciadas de grupo ou um objeto de conta personalizado derivado de um desses dois tipos de contas de serviço. As políticas podem definir as condições de controle de acesso de um dispositivo, que são usadas para restringir as credenciais da conta de serviço gerenciada a dispositivos específicos com uma identidade de Active Directory. Os serviços nunca deverão ser membros do grupo de segurança Usuários Protegidos, pois todas as autenticações de entrada falharão.

-   **Computador**

    É usado um objeto de conta de computador ou um objeto de conta personalizado derivado de um objeto de conta de computador. As políticas podem definir as condições de controle de acesso necessárias para permitir a autenticação à conta com base nas propriedades do usuário e do dispositivo. Os computadores nunca deverão ser membros do grupo de segurança Usuários Protegidos, pois todas as autenticações de entrada falharão. Por padrão, as tentativas de usar autenticação NTLM são rejeitadas. Um tempo de vida de TGT não deve ser configurado para contas de computador.

> [!NOTE]
> É possível definir uma política de autenticação em um conjunto de contas sem associar a política a um silo de política de autenticação. Você pode usar esta estratégia quando uma única conta precisar ser protegida.

**Esquema do Active Directory para políticas de autenticação**

As políticas para os objetos do Active Directory para usuários, computadores e serviços são definidas pelo esquema na tabela a seguir.

|Type|Nome de exibição|Descrição|
|----|--------|--------|
|Política|Política de autenticação|Uma instância desta classe define os comportamentos da política de autenticação para as entidades designadas.|
|Política|Políticas de autenticação|Um contêiner desta classe pode conter objetos de política de autenticação.|
|Política|Política de autenticação imposta|Especifica se a política de autenticação foi imposta.<p>Quando não está imposta, a política fica no modo de auditoria por padrão e os eventos que indicam potenciais sucessos ou falhas são gerados, mas as proteções não são aplicadas ao sistema.|
|Política|Vínculo regressivo da política de autenticação atribuída|Este atributo é o vínculo regressivo para msDS-AssignedAuthNPolicy.|
|Política|Política de autenticação atribuída|Especifica qual AuthNPolicy deve ser aplicada a esta entidade.|
|Usuário|Política de autenticação de usuário|Especifica qual AuthNPolicy deve ser aplicada aos usuários atribuídos a este objeto de silo.|
|Usuário|Vínculo regressivo da política de autenticação de usuário|Este atributo é o vínculo regressivo para msDS-UserAuthNPolicy.|
|Usuário|ms-DS-User-Allowed-To-Authenticate-To|Este atributo é usado para determinar o conjunto de entidades permitidas para autenticação a um serviço executado sob uma conta de usuário.|
|Usuário|ms-DS-User-Allowed-To-Authenticate-From|Este atributo é usado para determinar o conjunto de dispositivos com os quais uma conta de usuário possui permissão para entrar.|
|Usuário|Tempo de vida de TGT de usuário|Especifica a duração máxima de um TGT de Kerberos emitido para um usuário (expressada em segundos). Os TGTs resultantes não são renováveis.|
|Computador|Política de autenticação de computador|Especifica qual AuthNPolicy deve ser aplicada aos computadores atribuídos a este objeto de silo.|
|Computador|Vínculo regressivo da política de autenticação de computador|Este atributo é o vínculo regressivo para msDS-ComputerAuthNPolicy.|
|Computador|ms-DS-Computer-Allowed-To-Authenticate-To|Este atributo é usado para determinar o conjunto de entidades permitidas para autenticação a um serviço executado sob uma conta de computador.|
|Computador|Tempo de vida de TGT de computador|Especifica a duração máxima de um TGT de Kerberos emitido para um computador (expressada em segundos). Não é recomendável alterar essa configuração.|
|Serviço|Política de autenticação de serviço|Especifica qual AuthNPolicy deve ser aplicada aos serviços atribuídos a este objeto de silo.|
|Serviço|Vínculo regressivo da política de autenticação de serviço|Este atributo é o vínculo regressivo para msDS-ServiceAuthNPolicy.|
|Serviço|ms-DS-Service-Allowed-To-Authenticate-To|Este atributo é usado para determinar o conjunto de entidades permitidas para autenticação a um serviço executado sob uma conta de serviço.|
|Serviço|ms-DS-Service-Allowed-To-Authenticate-From|Este atributo é usado para determinar o conjunto de dispositivos com os quais uma conta de serviço possui permissão para entrar.|
|Serviço|Tempo de vida de TGT de serviço|Especifica a duração máxima de um TGT de Kerberos emitido para um serviço (expressada em segundos).|

As políticas de autenticação podem ser configuradas para cada silo usando o Console Administrativo do Active Directory ou o Windows PowerShell. Para mais informações, consulte [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Como ele funciona
Esta seção explica como os silos de política de autenticação e as políticas de autenticação funcionam em conjunto com o grupo de segurança Usuários Protegidos, bem como a implementação do protocolo Kerberos no Windows.

-   [Como o protocolo Kerberos é usado com silos e políticas de autenticação](#BKMK_HowKerbUsed)

-   [Como funciona a restrição de entrada de usuário](#BKMK_HowRestrictingSignOn)

-   [Como funciona a restrição de emissão de tíquete de serviço](#BKMK_HowRestrictingServiceTicket)

**Contas protegidas**

O grupo de segurança usuários protegidos dispara proteção não configurável em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1 e em controladores de domínio em domínios com um controlador de domínio primário executando o Windows Server 2012 R2. Dependendo do nível funcional do domínio da conta, os membros do grupo de segurança Usuários Protegidos são protegidos ainda mais devido a mudanças nos métodos de autenticação com suporte no Windows.

-   O membro do grupo de segurança Usuários Protegidos não pode se autenticar usando NTLM, autenticação Digest ou delegação de credencial padrão CredSSP. Em um dispositivo que executa o Windows 8.1 que usa qualquer um desses SSPs (provedores de suporte de segurança), a autenticação em um domínio falhará quando a conta for membro do grupo de segurança usuários protegidos.

-   O protocolo Kerberos não usará os tipos de criptografia DES ou RC4 mais fracos no processo de pré-autenticação. Isso significa que o domínio deve ser configurado para dar suporte ao menos ao tipo de criptografia AES.

-   A conta do usuário não pode ser delegada com delegação restrita de Kerberos ou irrestrita. Isso significa que as conexões antigas aos outros sistemas podem falhar, caso o usuário seja um membro do grupo de segurança Usuários Protegidos.

-   A configuração de tempo de vida dos TGTs padrão de quatro horas é configurável usando políticas de autenticação e silos acessados pelo Centro Administrativo do Active Directory. Isso significa que passadas quatro horas, o usuário deve se autenticar novamente.

Para mais informações sobre este grupo de segurança, consulte [Como funciona o grupo Usuários Protegidos](protected-users-security-group.md#BKMK_HowItWorks).

**Silos e políticas de autenticação**

Os silos de política de autenticação e as políticas de autenticação aproveitam a infraestrutura de autenticação existente do Windows. O uso do protocolo NTLM é rejeitado, sendo usado o protocolo Kerberos com tipos de criptografia mais modernos. As políticas de autenticação complementam o grupo de segurança Usuários Protegidos ao oferecer uma maneira de aplicar restrições configuráveis a contas, além de fornecer restrições a contas para serviços e computadores. As políticas de autenticação são impostas durante o compartilhamento do protocolo Kerberos AS (serviço de autenticação) ou TGS (serviço de concessão de tíquete). Para saber mais sobre como o Windows usa o protocolo Kerberos e quais alterações foram feitas para dar suporte aos silos de política de autenticação e as políticas de autenticação, consulte:

-   [Como funciona o protocolo de autenticação Kerberos versão 5](/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10))

-   [Alterações na autenticação Kerberos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10)) (windows Server 2008 R2 e Windows 7)

### <a name="how-the-kerberos-protocol-is-used-with-authentication-policy-silos-and-policies"></a><a name="BKMK_HowKerbUsed"></a>Como o protocolo Kerberos é usado com silos de políticas e políticas de autenticação
Quando uma conta de domínio é ligada a um silo de política de autenticação e o usuário entra, o Gerenciador de contas de segurança adiciona o tipo de declaração do Silo de política de autenticação que inclui o silo como valor. Essa declaração na conta fornece o acesso ao silo de destino.

Quando uma política de autenticação é imposta e o serviço de autenticação solicita que uma conta de domínio seja recebida no controlador de domínio, este retorna um TGT não renovável com o tempo de vida configurado (exceto se o tempo de vida de TGT do domínio for mais breve).

> [!NOTE]
> A conta do domínio deve ter um tempo de vida TGT configurado e deverá estar diretamente ligada à política ou indiretamente vinculada pela associação ao silo.

Quando uma política de autenticação está no modo de auditoria e a solicitação de serviço de autenticação para a conta de domínio é recebida no controlador de domínio, este verifica se a autenticação possui permissão para o dispositivo, para que possa registrar um aviso em caso de falha. Uma política de autenticação auditada não altera o processo, por isso as solicitações de autenticação não falharão se não cumprirem os requisitos da política.

> [!NOTE]
> A conta do domínio deve estar direta ou indiretamente ligada à política pela associação ao silo.

Quando uma política de autenticação é imposta e o serviço de autenticação é protegido, esse serviço solicita que uma conta de domínio seja recebida no controlador de domínio. Este controlador, por sua vez, verifica se a autenticação é permitida para o dispositivo. Se falhar, o controlador de domínio retornará uma mensagem de erro e registrará um evento.

> [!NOTE]
> A conta do domínio deve estar direta ou indiretamente ligada à política pela associação ao silo.

Quando uma política de autenticação está em modo de auditoria e uma solicitação de serviço de concessão de tíquete é recebida pelo controlador de domínio para uma conta de domínio, o controlador de domínio verifica se a autenticação é permitida com base nos dados de PAC (certificado de atributo de privilégio) de tíquete de solicitação e registra uma mensagem de aviso se ela falhar. O PAC contém diversos tipos de dados de autorização, incluindo grupos dos quais o usuário é membro, direitos que possui e quais políticas se aplicam a ele. Essas informações são usadas para gerar o token de acesso do usuário. Se for uma política de autenticação imposta que permite a autenticação para um usuário, dispositivo ou serviço, o controlador de domínio verificará se a autenticação é permitida com base nos dados de PAC do tíquete da solicitação. Se falhar, o controlador de domínio retornará uma mensagem de erro e registrará um evento.

> [!NOTE]
> A conta de domínio deve estar ligada diretamente ou ligada por associação ao silo a uma política de autenticação auditada que permita a autenticação a um usuário, dispositivo ou serviço.

Você pode usar uma única política de autenticação para todos os membros de um silo ou usar políticas separadas para usuários, computadores e contas de serviço gerenciadas.

As políticas de autenticação podem ser configuradas para cada silo usando o Console Administrativo do Active Directory ou o Windows PowerShell. Para mais informações, consulte [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

### <a name="how-restricting-a-user-sign-in-works"></a><a name="BKMK_HowRestrictingSignOn"></a>Como funciona a restrição de entrada de usuário
Como essas políticas de autenticação são aplicadas a uma conta, elas também se aplicam às contas usadas pelos serviços. Esta configuração será muito útil se você desejar limitar o uso de uma senha para um serviço a um host específico. Isso ocorre, por exemplo, quando contas de serviço gerenciadas de grupo são configuradas onde os hosts têm permissão para recuperar a senha dos Serviços de Domínio Active Directory. Contudo, a senha pode ser usada de qualquer host para autenticação inicial. Ao aplicar uma condição de controle de acesso, uma camada de proteção adicional pode ser obtida ao limitar que a senha pode ser recuperada apenas por um determinado conjunto de hosts.

Quando os serviços executados como sistema, serviço de rede ou outra identidade de serviço local se conectam aos serviços de rede, eles usam a conta de computador do host. As contas de computador não podem ser restringidas. Por isso, mesmo se o serviço estiver usando uma conta de computador que não seja para um host do Windows, ela não poderá ser restrita.

Restringir a entrada do usuário a hosts específicos exige que o controlador de domínio valide a identidade do host. Ao usar autenticação Kerberos com proteção Kerberos (que faz parte do Controle de Acesso Dinâmico), o Centro de Distribuição de Chave é fornecido com o TGT do host ao qual o usuário está se autenticando. O conteúdo deste TGT protegido é usado para concluir uma verificação de acesso para determinar se o host é permitido.

Quando um usuário entra no Windows ou digita suas credenciais de domínio em uma solicitação de credenciais para um aplicativo, por padrão, o Windows envia um AS-REQ desprotegido para o controlador de domínio. Se o usuário estiver enviando a solicitação de um computador que não dá suporte à proteção, caso este computador esteja executando o Windows 7 ou o Windows Vista, por exemplo, a solicitação falhará.

A lista a seguir descreve este processo:

-   O controlador de domínio em um domínio que executa consultas do Windows Server 2012 R2 para a conta de usuário e determina se ele está configurado com uma política de autenticação que restringe a autenticação inicial que exige solicitações protegidas.

-   A solicitação falhará no controlador de domínio.

-   Como a proteção é necessária, o usuário pode tentar entrar usando um computador executando o Windows 8.1 ou o Windows 8, que está habilitado para dar suporte à proteção Kerberos para tentar novamente o processo de entrada.

-   O Windows detecta que o domínio dá suporte à proteção Kerberos e envia uma AS-REQ protegida para tentar efetuar novamente a solicitação de entrada.

-   O controlador de domínio executa uma verificação de acesso usando as condições de controle de acesso configuradas e as informações de identidade do sistema operacional do cliente no TGT que foi usado para proteger a solicitação.

-   Se a verificação de acesso falhar, o controlador de domínio rejeitará a solicitação.

Mesmo quando os sistemas operacionais oferecerem suporte à proteção Kerberos, os requisitos de controle de acesso podem ser aplicados e devem ser cumpridos para que o acesso seja concedido. Os usuários que entram no Windows ou digitam suas credenciais de domínio em um prompt de credenciais de um aplicativo. Por padrão, o Windows envia um AS-REQ desprotegido para o controlador de domínio. Se o usuário estiver enviando a solicitação de um computador que dá suporte à proteção, como Windows 8.1 ou Windows 8, as políticas de autenticação serão avaliadas da seguinte maneira:

1.  O controlador de domínio em um domínio que executa consultas do Windows Server 2012 R2 para a conta de usuário e determina se ele está configurado com uma política de autenticação que restringe a autenticação inicial que exige solicitações protegidas.

2.  O controlador de domínio executa uma verificação de acesso usando as condições de controle de acesso configuradas e as informações de identidade do sistema no TGT que é usado para proteger a solicitação. A verificação de acesso é concluída com êxito.

    > [!NOTE]
    > Se as restrições do grupo de trabalho legado estiverem configuradas, elas também precisarão ser cumpridas.

3.  O controlador de domínio devolve uma resposta protegida (AS-REP) e a autenticação prossegue.

### <a name="how-restricting-service-ticket-issuance-works"></a><a name="BKMK_HowRestrictingServiceTicket"></a>Como funciona a restrição de emissão de tíquete de serviço
Quando uma conta não é permitida e um usuário que tem um TGT tenta se conectar ao serviço (por exemplo, abrindo um aplicativo que requer autenticação para um serviço que é identificado pelo SPN (nome da entidade de serviço) do serviço, ocorre a seguinte sequência:

1.  Ao tentar se conectar ao SPN1 do SPN, o Windows envia um TGS-REQ para o controlador de domínio que está solicitando um tíquete de serviço para SPN1.

2.  O controlador de domínio em um domínio que executa o Windows Server 2012 R2 pesquisa o SPN1 para localizar a conta de Active Directory Domain Services para o serviço e determina que a conta está configurada com uma política de autenticação que restringe a emissão do tíquete de serviço.

3.  O controlador de domínio executa uma verificação de acesso usando as condições de controle de acesso configuradas e as informações de identidade do usuário no TGT. A verificação de acesso falha.

4.  O controlador de domínio rejeita a solicitação.

Quando uma conta é permitida porque a conta atende às condições de controle de acesso que são definidas pela política de autenticação e um usuário que tem uma TGT tenta se conectar ao serviço (por exemplo, abrindo um aplicativo que requer autenticação para um serviço que é identificado pelo SPN do serviço), ocorre a seguinte sequência:

1.  Ao tentar se conectar ao SPN1, o Windows envia um TGS-REQ para o controlador de domínio que está solicitando um tíquete de serviço para SPN1.

2.  O controlador de domínio em um domínio que executa o Windows Server 2012 R2 pesquisa o SPN1 para localizar a conta de Active Directory Domain Services para o serviço e determina que a conta está configurada com uma política de autenticação que restringe a emissão do tíquete de serviço.

3.  O controlador de domínio executa uma verificação de acesso usando as condições de controle de acesso configuradas e as informações de identidade do usuário no TGT. A verificação de acesso é concluída com êxito.

4.  O controlador de domínio responde à solicitação com um TGS-REP (resposta de serviço de concessão de tíquete).

## <a name="associated-error-and-informational-event-messages"></a><a name="BKMK_ErrorandEvents"></a>Mensagens de erro associadas e eventos informativos
A tabela a seguir descreve os eventos associados ao grupo de segurança Usuários Protegidos e as políticas de autenticação aplicadas aos silos de política de autenticação.

Os eventos são registrados nos Logs de Aplicativos e Serviços, em **Microsoft\Windows\Authentication**.

Para ver as etapas de solução de problemas que usam esses eventos, consulte as [Solução de problemas das políticas de autenticação](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicies) e [Solução de problemas de eventos relacionados a usuários protegidos](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents).

|ID e log de evento|Descrição|
|----------|--------|
|101<p>**AuthenticationPolicyFailures-DomainController**|Motivo: uma falha de conexão NTLM ocorre porque a política de autenticação está configurada.<p>Um evento é registrado no controlador de domínio para indicar que a autenticação de NTLM falhou, pois as restrições de controle de acesso são necessárias, e essas restrições não podem ser aplicadas ao NTLM.<p>Exibe os nomes da conta, dispositivo, política e silo.|
|105<p>**AuthenticationPolicyFailures-DomainController**|Motivo: uma falha de restrição de Kerberos ocorre porque a autenticação de um dispositivo específico não foi permitida.<p>Um evento é registrado no controlador de domínio para indicar que o TGT do Kerberos foi negado, pois o dispositivo não cumpre as restrições de controle de acesso impostas.<p>Exibe os nomes da conta, dispositivo, política e silo, bem como o tempo de vida de TGT.|
|305<p>**AuthenticationPolicyFailures-DomainController**|Motivo: uma possível falha de restrição de Kerberos pode ocorrer porque a autenticação de um dispositivo específico não era permitida.<p>No modo de auditoria, um evento informativo é registrado no controlador de domínio para determinar se um TGT do Kerberos será negado, pois o dispositivo não cumpre as restrições de controle de acesso.<p>Exibe os nomes da conta, dispositivo, política e silo, bem como o tempo de vida de TGT.|
|106<p>**AuthenticationPolicyFailures-DomainController**|Motivo: uma falha de restrição de Kerberos ocorre porque o usuário ou o dispositivo não tem permissão para se autenticar no servidor.<p>Um evento é registrado no controlador de domínio para indicar que o tíquete de serviço do Kerberos foi negado, pois o usuário, dispositivo ou ambos não cumprem as restrições de controle de acesso impostas.<p>Exibe os nomes do dispositivo, política e silo.|
|306<p>**AuthenticationPolicyFailures-DomainController**|Motivo: uma falha de restrição de Kerberos pode ocorrer porque o usuário ou o dispositivo não tem permissão para se autenticar no servidor.<p>No modo de auditoria, um evento informativo é registrado no controlador de domínio para indicar que o tíquete de serviço do Kerberos será negado, pois o usuário, o dispositivo ou ambos não cumprem as restrições de controle de acesso.<p>Exibe os nomes do dispositivo, política e silo.|

## <a name="additional-references"></a>Referências adicionais
[Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)

[Proteção e gerenciamento de credenciais](credentials-protection-and-management.md)

[Grupo de segurança Usuários protegidos](protected-users-security-group.md)