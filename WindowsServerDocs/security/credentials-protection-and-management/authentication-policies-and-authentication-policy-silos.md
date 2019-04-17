---
title: "Políticas de autenticação e Silos de política de autenticação"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Políticas de autenticação e Silos de política de autenticação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve silos de política de autenticação e as políticas que podem restringir contas para esses silos. Ele também explica como políticas de autenticação podem ser usadas para restringir o escopo de contas.

Silos de política de autenticação e as políticas de acompanhamento fornecem uma maneira para conter as credenciais de alto privilégio para sistemas que só estão relacionadas ao selecionado de usuários, computadores ou serviços. Silos podem ser definidos e gerenciados nos serviços de domínio do Active Directory (AD DS) usando o Centro Administrativo do Active Directory e os cmdlets do Active Directory do Windows PowerShell.

Silos de política de autenticação são contêineres para que os administradores podem atribuir contas de usuário, contas de computador e contas de serviço. Conjuntos de contas, em seguida, podem ser gerenciados pelas políticas de autenticação que foram aplicadas a esse contêiner. Isso reduz a necessidade de administrador controlar o acesso a recursos para contas individuais e ajuda a impedir que usuários mal-intencionados acessem outros recursos por meio de roubo de credenciais.

Recursos introduzidos no Windows Server 2012 R2, permitem que você crie autenticação silos de política, que hospedam um conjunto de usuários de alto privilégio. Em seguida, você pode atribuir políticas de autenticação para esse contêiner para limitar onde contas privilegiadas podem ser usadas no domínio. Quando contas estão no grupo de segurança usuários protegido, controles adicionais são aplicados, como o uso exclusivo do protocolo Kerberos.

Com esses recursos, você pode limitar o uso da conta de alto valor para hosts de alto valor. Por exemplo, você poderia criar um silo administradores nova floresta que contém o enterprise, esquema e os administradores de domínio. Em seguida, você pode configurar silo com uma política de autenticação para que a senha e a autenticação baseada em cartão inteligente de sistemas que não sejam controladores de domínio e consoles de administrador do domínio falharia.

Para obter informações sobre como configurar silos de política de autenticação e políticas de autenticação, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Sobre silos de política de autenticação
Um silo de política de autenticação controla quais contas podem ser proibidas por silo e define as políticas de autenticação para aplicar aos membros. Você pode criar silo com base nos requisitos da sua organização. Os silos são objetos do Active Directory para os usuários, computadores e serviços, conforme definido pelo esquema na tabela a seguir.

**Esquema do Active Directory para silos de política de autenticação**

|Nome de exibição|Descrição|
|--------|--------|
|Silo de política de autenticação|Uma instância dessa classe define políticas de autenticação e os comportamentos relacionados para os usuários atribuídos, computadores e serviços.|
|Silos de política de autenticação|Um contêiner dessa classe pode conter objetos de silo de política de autenticação.|
|Autenticação Silo de política imposta|Especifica se a silo de política de autenticação é imposta.<br /><br />Quando não impostas, a política por padrão é no modo de auditoria. Os eventos que indicam possíveis sucessos e falhas são gerados, mas proteções não são aplicadas ao sistema.|
|Autenticação atribuído política Silo Backlink|Esse atributo é o link voltar para msDS-AssignedAuthNPolicySilo.|
|Membros de Silo de política de autenticação|Especifica quais entidades são designadas para o AuthNPolicySilo.|
|Autenticação política Silo membros Backlink|Esse atributo é o link voltar para msDS-AuthNPolicySiloMembers.|

Silos de política de autenticação podem ser configurados usando o Console administrativo do Active Directory ou o Windows PowerShell. Para obter mais informações, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Sobre políticas de autenticação
Uma política de autenticação define as propriedades de tempo de vida do Kerberos protocolo tíquete de concessão de tíquete (TGT) e condições de controle de acesso de autenticação para um tipo de conta. A política se baseia e controla o contêiner do AD DS conhecido como silo de política de autenticação.

As políticas de autenticação controlam o seguinte:

-   O tempo de vida do TGT para a conta, que é definida para ser não renovável.

-   Os critérios que contas de dispositivo precisam atender para entrar com uma senha ou um certificado.

-   Os critérios que os usuários e dispositivos precisam atender para autenticar os serviços executados como parte da conta.

O tipo de conta do Active Directory determina a função do chamador como um destes procedimentos:

-   **Usuário**

    Os usuários sempre devem ser membros do grupo de segurança usuários protegido, que, por padrão, rejeita tentativas de autenticação usando NTLM.

    As políticas podem ser configuradas para definir o tempo de vida do TGT de uma conta de usuário como um valor menor ou restringir os dispositivos para o qual uma conta de usuário pode entrar. Expressões avançada podem ser configuradas na política de autenticação para controlar os critérios que os usuários e seus dispositivos precisam atender para autenticar para o serviço.

    Para obter mais informações, consulte [protegido por grupo de segurança de usuários](protected-users-security-group.md).

-   **Serviço**

    Autônomo gerenciado contas de serviço, contas de serviço gerenciado do grupo ou um objeto de conta personalizado é derivado desses dois tipos de contas são usadas de serviço. Políticas podem definir acesso do dispositivo condições de controle, que são usadas para restringir as credenciais de conta de serviço gerenciado para dispositivos específicos com uma identidade do Active Directory. Serviços nunca devem ser membros do grupo de segurança usuários protegido porque todos os autenticação entrada falhará.

-   **Computador**

    O objeto de conta de computador ou o objeto de conta personalizada que é derivado do objeto de conta de computador é usado. Políticas podem definir o acesso condições de controle que são necessárias para permitir que a autenticação para a conta com base nas propriedades do dispositivo e usuário. Computadores nunca devem ser membros do grupo de usuários protegido de segurança porque todos os autenticação entrada falhará. Por padrão, as tentativas de usar autenticação NTLM serão rejeitadas. Um tempo de vida do TGT não deve ser configurado para contas de computador.

> [!NOTE]
> É possível definir uma política de autenticação em um conjunto de contas sem associar a política para um silo de política de autenticação. Você pode usar essa estratégia quando você tem uma única conta para proteger.

**Esquema do Active Directory para políticas de autenticação**

As políticas para objetos do Active Directory para os usuários, computadores e serviços são definidas pelo esquema na tabela a seguir.

|Tipo|Nome de exibição|Descrição|
|----|--------|--------|
|Política|Política de autenticação|Uma instância dessa classe define comportamentos de política de autenticação para entidades atribuídas.|
|Política|Políticas de autenticação|Um contêiner dessa classe pode conter objetos de política de autenticação.|
|Política|Política de autenticação aplicada|Especifica se a política de autenticação é aplicada.<br /><br />Quando não impostas, a política por padrão está em modo de auditoria e eventos que indicam possíveis sucessos e falhas são gerados, mas proteções não são aplicadas ao sistema.|
|Política|Autenticação atribuído política Backlink|Esse atributo é o link voltar para msDS-AssignedAuthNPolicy.|
|Política|Política de autenticação atribuído|Especifica que AuthNPolicy devem ser aplicadas a essa entidade de segurança.|
|Usuário|Política de autenticação de usuário|Especifica que AuthNPolicy devem ser aplicadas a usuários atribuídos a esse objeto silo.|
|Usuário|Backlink de política de autenticação do usuário|Esse atributo é o link voltar para msDS-UserAuthNPolicy.|
|Usuário|ms-DS-User-Allowed-To-Authenticate-To|Esse atributo é usado para determinar o conjunto de objetos de permissão para autenticar a um serviço em execução sob a conta de usuário.|
|Usuário|ms-DS-User-Allowed-To-Authenticate-From|Esse atributo é usado para determinar o conjunto de dispositivos para o qual uma conta de usuário tem permissão para entrar.|
|Usuário|O tempo de vida do usuário TGT|Especifica a duração máxima de um TGT Kerberos que é emitida para um usuário (expressado em segundos). TGTs resultantes são não renovável.|
|Computador|Política de autenticação do computador|Especifica que AuthNPolicy devem ser aplicadas a computadores que são atribuídos a esse objeto silo.|
|Computador|Backlink de política de autenticação do computador|Esse atributo é o link voltar para msDS-ComputerAuthNPolicy.|
|Computador|ms-DS-Computer-Allowed-To-Authenticate-To|Esse atributo é usado para determinar o conjunto de objetos que têm permissão para se autenticar em um serviço em execução sob a conta do computador.|
|Computador|O tempo de vida do computador TGT|Especifica a duração máxima de um TGT Kerberos emitido para um computador (expressado em segundos). Não é recomendável alterar essa configuração.|
|Serviço|Política de autenticação do serviço|Especifica que AuthNPolicy devem ser aplicadas a serviços que são atribuídos a esse objeto silo.|
|Serviço|Backlink de política de autenticação do serviço|Esse atributo é o link voltar para msDS-ServiceAuthNPolicy.|
|Serviço|ms-DS-Service-Allowed-To-Authenticate-To|Esse atributo é usado para determinar o conjunto de objetos que têm permissão para se autenticar em um serviço em execução sob a conta de serviço.|
|Serviço|ms-DS-Service-Allowed-To-Authenticate-From|Esse atributo é usado para determinar o conjunto de dispositivos para o qual uma conta de serviço tem permissão para entrar.|
|Serviço|O tempo de vida do serviço TGT|Especifica a duração máxima de um TGT Kerberos emitido para um serviço (expressado em segundos).|

Políticas de autenticação podem ser configuradas para cada silo usando o Console administrativo do Active Directory ou o Windows PowerShell. Para obter mais informações, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Como funciona
Esta seção descreve como silos de política de autenticação e políticas de autenticação funcionam em conjunto com o grupo de segurança de usuários protegidos e a implementação do protocolo Kerberos no Windows.

-   [Como o protocolo Kerberos é usado com as políticas e silos de autenticação](#BKMK_HowKerbUsed)

-   [Restringindo como um usuário entrar funciona](#BKMK_HowRestrictingSignOn)

-   [Como funciona a restringir emissão de tíquete de serviço](#BKMK_HowRestrictingServiceTicket)

**Contas protegidas**

O grupo de segurança de usuários protegido dispara proteção não configuráveis em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1 e em controladores de domínio em domínios com um controlador de domínio primário executando o Windows Server 2012 R2. Dependendo do nível funcional de domínio da conta, membros do grupo de segurança usuários protegidos são ainda mais protegidos devido às alterações nos métodos de autenticação que têm suporte no Windows.

-   O membro do grupo de segurança protegido por usuários não pode se autenticar usando NTLM, a autenticação Digest ou delegação de credencial CredSSP padrão. Em um dispositivo executando o Windows 8.1 que usa qualquer um desses provedores de suporte de segurança (SSPs), a autenticação em um domínio falhará quando a conta é um membro do grupo de segurança usuários protegido.

-   O protocolo Kerberos não usará os tipos de criptografia DES ou RC4 mais fracos no processo de pré-autenticação. Isso significa que o domínio deve ser configurado para o tipo de criptografia AES com suporte mínimo.

-   A conta do usuário não pode ser recebida com Kerberos restrita ou estão irrestritos delegação. Isso significa que primeiro conexões com outros sistemas poderão falhar se o usuário é um membro do grupo de segurança usuários protegido.

-   A configuração de tempo de vida de Kerberos TGTs padrão de quatro horas é configurável por meio de políticas de autenticação e silos, que podem ser acessados por meio do Centro Administrativo do Active Directory. Isso significa que, após quatro horas, o usuário deve autenticar novamente.

Para obter mais informações sobre esse grupo de segurança, consulte [como os usuários protegidos agrupar funciona](protected-users-security-group.md#BKMK_HowItWorks).

**Políticas de autenticação e silos**

Silos de política de autenticação e políticas de autenticação aproveitam a infraestrutura de autenticação existente do Windows. O uso do protocolo NTLM for rejeitado e o protocolo Kerberos com tipos de criptografia mais recentes é usado. Políticas de autenticação complementam o grupo de segurança de usuários protegido, fornecendo uma maneira de aplicar as restrições podem ser definidas para contas, além de fornecer as restrições para contas para serviços e computadores. Políticas de autenticação são impostas durante o serviço de autenticação de protocolo Kerberos (AS) ou o exchange concessão de tíquete de serviço (TGS). Para obter mais informações sobre como o Windows usa o protocolo Kerberos e quais alterações foram feitas para dar suporte a políticas de autenticação e silos de política de autenticação, consulte:

-   [Como funciona o protocolo de autenticação 5 Kerberos versão](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Alterações na autenticação Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server 2008 R2 e Windows 7)

### <a name="BKMK_HowKerbUsed"></a>Como o protocolo Kerberos é usado com as políticas e silos de política de autenticação
Quando uma conta de domínio está vinculada a um silo de política de autenticação e o usuário entrar, o Gerenciador de contas de segurança adiciona o tipo de declaração de Silo de política de autenticação que inclui silo como o valor. Essa declaração na conta fornece o acesso a silo direcionada.

Quando uma política de autenticação é imposta e a solicitação de serviço de autenticação para uma conta de domínio for recebida no controlador de domínio, o controlador de domínio retorna um TGT não renováveis com o tempo de vida configurado (a menos que o tempo de vida do TGT de domínio for menor).

> [!NOTE]
> A conta de domínio deve ter um tempo de vida do TGT configurado e deve ser diretamente vinculado à política ou indiretamente vinculados através da associação a silo.

Quando uma política de autenticação estiver no modo de auditoria e a solicitação de serviço de autenticação para uma conta de domínio for recebida no controlador de domínio, o controlador de domínio verifica se a autenticação é permitida para o dispositivo para que ele possa fazer um aviso se houver uma falha. Uma política de autenticação auditados não altera o processo para que solicitações de autenticação não falhará se eles não atender aos requisitos da política.

> [!NOTE]
> A conta de domínio deve ser vinculada diretamente à política ou indiretamente vinculada através da associação a silo.

Quando uma política de autenticação é imposta e o serviço de autenticação é protegido, a solicitação de serviço de autenticação para uma conta de domínio for recebida no controlador de domínio, o controlador de domínio verifica se a autenticação é permitida para o dispositivo. Se ele falhar, o controlador de domínio retorna uma mensagem de erro e registra um evento.

> [!NOTE]
> A conta de domínio deve ser vinculada diretamente à política ou indiretamente vinculada através da associação a silo.

Quando uma política de autenticação estiver no modo de auditoria e uma solicitação de serviço de concessão de tíquete for recebida pelo controlador de domínio para uma conta de domínio, o controlador de domínio verifica se a autenticação é permitida com base em dados de certificado de atributo de privilégio (PAC) de tíquete da solicitação e registra uma mensagem de aviso se ele falhar. O PAC contém vários tipos de dados de autorização, incluindo grupos que o usuário seja um membro de direitos que o usuário tiver, e quais políticas se aplicam ao usuário. Essas informações são usadas para gerar um token de acesso do usuário. Se for uma política de autenticação imposta que permite a autenticação para um usuário, dispositivo ou serviço, as verificações de controlador de domínio se a autenticação é permitida com base em dados PAC de tíquete da solicitação. Se ele falhar, o controlador de domínio retorna uma mensagem de erro e registra um evento.

> [!NOTE]
> A conta de domínio deve estar diretamente vinculada ou vinculada através da participação em silo uma auditados na política de autenticação que permite a autenticação para um usuário, dispositivo ou serviço,

Você pode usar uma política de autenticação único para todos os membros de um silo, ou você pode usar políticas separadas para os usuários, computadores e contas de serviço gerenciado.

Políticas de autenticação podem ser configuradas para cada silo usando o Console administrativo do Active Directory ou o Windows PowerShell. Para obter mais informações, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Restringindo como um usuário entrar funciona
Porque essas políticas de autenticação são aplicadas a uma conta, ela também se aplica a contas que são usadas pelos serviços. Se você deseja limitar o uso de uma senha para um serviço específicos hosts, essa configuração é útil. Por exemplo, grupo contas estão configuradas onde os hosts são permitidos para recuperar a senha do Active Directory Domain Services de serviço gerenciado. No entanto, essa senha pode ser usada de qualquer host de autenticação inicial. Aplicando uma condição de controle de acesso, uma camada adicional de proteção pode ser feita, limitando a senha para apenas o conjunto de hosts que pode recuperar a senha.

Quando os serviços executados como sistema, serviço de rede ou outro identificador de serviço local se conectar aos serviços de rede, eles usam a conta de computador do host. Contas de computador não podem ser restritas. Por isso mesmo se o serviço está usando uma conta de computador que não é para um host do Windows, não pode ser restrito.

Restringir a entrada do usuário para hosts específicos requer o controlador de domínio para validar a identidade do host. Ao usar autenticação Kerberos com proteção Kerberos (que é parte do controle de acesso dinâmico), o Centro de distribuição de chaves é fornecido com o TGT do host do qual o usuário é autenticado. O conteúdo desse TGT com é usado para concluir uma verificação de acesso para determinar se o host é permitido.

Quando um usuário fizer logon no Windows ou insere suas credenciais de domínio em uma solicitação de credencial para um aplicativo, por padrão, o Windows envia um AS-REQ unarmored ao controlador de domínio. Se o usuário está enviando a solicitação de um computador que não oferece suporte a proteção, como computadores que executam o Windows 7 ou Windows Vista, a solicitação falhará.

A lista a seguir descreve o processo:

-   O controlador de domínio em um domínio que executam o Windows Server 2012 R2 consulta da conta de usuário e determina se ele está configurado com uma política de autenticação que restringe a autenticação inicial que exige com solicitações.

-   O controlador de domínio falhará a solicitação.

-   Como proteção é necessário, o usuário pode tentar entrar usando um computador com Windows 8.1 ou Windows 8, que é habilitado para dar suporte a proteção Kerberos para repetir o processo de entrada.

-   O Windows detecta que o domínio dá suporte a proteção Kerberos e envia um com AS-REQ para repetir a solicitação de entrada.

-   O controlador de domínio executa uma verificação de acesso, usando as condições de controle de acesso configuradas e informações de identidade do sistema operacional do cliente no que foi usada para armor a solicitação TGT.

-   Se a verificação de acesso falhar, o controlador de domínio rejeita a solicitação.

Mesmo quando os sistemas operacionais compatíveis com proteção Kerberos, requisitos de controle de acesso podem ser aplicados e devem ser atendidos antes que o acesso é concedido. Os usuários entrar no Windows ou inserir suas credenciais de domínio em uma solicitação de credencial para um aplicativo. Por padrão, o Windows envia um AS-REQ unarmored ao controlador de domínio. Se o usuário está enviando a solicitação de um computador que dá suporte à proteção, como Windows 8.1 ou Windows 8, políticas de autenticação são avaliadas da seguinte maneira:

1.  O controlador de domínio em um domínio que executam o Windows Server 2012 R2 consulta da conta de usuário e determina se ele está configurado com uma política de autenticação que restringe a autenticação inicial que exige com solicitações.

2.  O controlador de domínio executa uma verificação de acesso, usando as condições de controle de acesso configuradas e informações de identidade do sistema no que é usado para armor a solicitação TGT. A verificação de acesso é bem-sucedido.

    > [!NOTE]
    > Se forem definidas restrições de grupo de trabalho herdados, aqueles também precisam ser atendida.

3.  O controlador de domínio responde com uma resposta com (como representante) e continua a autenticação.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Como funciona a restringir emissão de tíquete de serviço
Quando uma conta não é permitida e um usuário que tem um TGT tenta se conectar ao serviço (como ao abrir um aplicativo que requer autenticação para um serviço que é identificado por nome da entidade do serviço serviço (SPN), ocorrerá o seguinte:

1.  Em uma tentativa de se conectar a SPN1 de SPN, o Windows envia uma TGS-REQ ao controlador de domínio que está solicitando um tíquete de serviço para SPN1.

2.  O controlador de domínio em um domínio que executam o Windows Server 2012 R2 procura SPN1 para encontrar a conta de serviços de domínio do Active Directory para o serviço e determina que a conta está configurada com uma política de autenticação que restringe a emissão de tíquete de serviço.

3.  O controlador de domínio executa uma verificação de acesso, usando as condições de controle de acesso configuradas e informações de identidade do usuário no TGT. A verificação de acesso falhar.

4.  O controlador de domínio rejeita a solicitação.

Quando uma conta é permitida porque a conta atenda as condições de controle de acesso que são definidas pela política de autenticação, e um usuário que tem um TGT tenta se conectar ao serviço (como abrindo um aplicativo que requer autenticação para um serviço que é identificado por meio SPN do serviço), ocorrerá o seguinte:

1.  Em uma tentativa de se conectar a SPN1, o Windows envia uma TGS-REQ ao controlador de domínio que está solicitando um tíquete de serviço para SPN1.

2.  O controlador de domínio em um domínio que executam o Windows Server 2012 R2 procura SPN1 para encontrar a conta de serviços de domínio do Active Directory para o serviço e determina que a conta está configurada com uma política de autenticação que restringe a emissão de tíquete de serviço.

3.  O controlador de domínio executa uma verificação de acesso, usando as condições de controle de acesso configuradas e informações de identidade do usuário no TGT. A verificação de acesso é bem-sucedido.

4.  O controlador de domínio responde à solicitação com uma resposta do serviço de concessão de tíquete (TGS representante).

## <a name="BKMK_ErrorandEvents"></a>Erro associadas e mensagens de evento informativo
A tabela a seguir descreve os eventos associados ao grupo de segurança de usuários protegidos e as políticas de autenticação que são aplicadas a silos de política de autenticação.

Os eventos são registrados nos Logs de aplicativos e serviços em **Microsoft\Windows\Authentication**.

Para solucionar problemas de etapas que usam esses eventos, consulte [Solucionar políticas de autenticação](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies) e [solucionar problemas de eventos relacionados aos usuários protegido](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents).

|Log e ID de evento|Descrição|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Uma falha de entrar NTLM ocorre porque a política de autenticação está configurada.<br /><br />Um evento é registrado no controlador de domínio para indicar que a autenticação NTLM falhou porque as restrições de controle de acesso são necessárias, e essas restrições não podem ser aplicadas para NTLM.<br /><br />Exibe a conta, dispositivo, política e silo nomes.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Uma falha de restrição de Kerberos ocorre porque a autenticação de um dispositivo específico não era permitida.<br /><br />Um evento é registrado no controlador de domínio para indicar que um TGT Kerberos foi negada porque o dispositivo não atendeu as restrições de controle de acesso imposta.<br /><br />Exibe a conta de dispositivo, política, silo nomes e tempo de vida do TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Uma falha de restrição de Kerberos potencial pode ocorrer devido a autenticação de um dispositivo específico não era permitida.<br /><br />No modo de auditoria, um evento informativo é registrado no controlador de domínio para determinar se um TGT Kerberos será negada porque o dispositivo não atendeu as restrições de controle de acesso.<br /><br />Exibe a conta de dispositivo, política, silo nomes e tempo de vida do TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Uma falha de restrição de Kerberos ocorre porque o usuário ou o dispositivo não teve permissão para realizar autenticação no servidor.<br /><br />Um evento é registrado no controlador de domínio para indicar que um tíquete de serviço Kerberos foi negado porque o usuário, dispositivo ou ambos não cumprir as restrições de controle de acesso imposta.<br /><br />Exibe o dispositivo, a política e silo nomes.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Uma falha de restrição de Kerberos pode ocorrer porque o usuário ou o dispositivo não teve permissão para realizar autenticação no servidor.<br /><br />No modo de auditoria, um evento informativo é registrado no controlador de domínio para indicar que um tíquete de serviço Kerberos será negado porque o usuário, dispositivo ou ambos não atender as restrições de controle de acesso.<br /><br />Exibe o dispositivo, a política e silo nomes.|

## <a name="see-also"></a>Consulte também
[Como configurar contas protegidas](how-to-configure-protected-accounts.md)

[Gerenciamento e proteção de credenciais](credentials-protection-and-management.md)

[Grupo de segurança de usuários protegido](protected-users-security-group.md)


