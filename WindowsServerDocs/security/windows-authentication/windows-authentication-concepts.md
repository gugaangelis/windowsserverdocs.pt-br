---
title: Conceitos de autenticação do Windows
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7ef3b9fde0ca9b9364a1fdcc99690b58f4f14f68
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989999"
---
# <a name="windows-authentication-concepts"></a>Conceitos de autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de visão geral de referência descreve os conceitos nos quais a autenticação do Windows é baseada.

A autenticação é um processo de verificação da identidade de um objeto ou uma pessoa. Quando você autentica um objeto, a meta é verificar se ele é genuíno. Quando você autentica uma pessoa, o objetivo é verificar se a pessoa não é uma impostor.

Em um contexto de rede, a autenticação é o ato de fornecer identidade para um aplicativo ou recurso de rede. Normalmente, a identidade é comprovada por uma operação criptográfica que usa uma chave somente o usuário sabe (assim como a criptografia de chave pública) ou uma chave compartilhada. O lado do servidor da troca de autenticação compara os dados assinados com uma chave de criptografia conhecida para validar a tentativa de autenticação.

O armazenamento das chaves criptográficas em um local central seguro torna o processo de autenticação escalonável e passível de manutenção. Active Directory é a tecnologia recomendada e padrão para armazenar informações de identidade, que incluem as chaves de criptografia que são as credenciais do usuário. O Active Directory é exigido para as implementações de Kerberos e NTLM padrão.

As técnicas de autenticação variam de um logon simples para um sistema operacional ou de uma entrada a um serviço ou aplicativo, que identifica os usuários com base em algo que apenas o usuário sabe, como uma senha, para mecanismos de segurança mais poderosos que usam algo que o usuário has'such como tokens, certificados de chave pública, imagens ou atributos biológicos. Em um ambiente de negócios, os usuários podem acessar vários aplicativos em muitos tipos de servidores em um único local ou em vários locais. Por esses motivos, a autenticação deve dar suporte a ambientes de outras plataformas e de outros sistemas operacionais Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticação e autorização: uma analogia de viagem
Uma analogia de viagem pode ajudar a explicar como funciona a autenticação. Algumas tarefas preparatórias geralmente são necessárias para iniciar a jornada. O viagem deve provar sua identidade verdadeira para suas autoridades de host. Essa prova pode estar na forma de prova de cidadania, local de nascimento, comprovante pessoal, fotografias ou o que for exigido pela lei do país do host. A identidade do viagem é validada pela emissão de um passaporte, que é análoga a uma conta de sistema emitida e administrada por uma organização – a entidade de segurança. O Passport e o destino pretendido são baseados em um conjunto de regras e regulamentos emitidos pela autoridade governamental.

**A jornada**

Quando o viagem chega à borda internacional, uma proteção de borda solicita credenciais e o viagem apresenta seu passaporte. O processo é de duas dobras:

-   A proteção autentica o Passport verificando se ele foi emitido por uma autoridade de segurança que o governo local confia (confia, pelo menos, para emitir Passports) e verificando se o Passport não foi modificado.

-   A proteção autentica o viagem verificando se a face corresponde à face da pessoa configurada no Passport e se outras credenciais necessárias estão em uma boa ordem.

Se o passaporte comprova ser válido e o viagem comprova ser seu proprietário, a autenticação é bem-sucedida e o acesso pode ter permissão para acessar toda a borda.

A confiança transitiva entre as autoridades de segurança é a base da autenticação; o tipo de autenticação que ocorre em uma borda internacional baseia-se na confiança. O governo local não conhece o viagem, mas confia que o governo do host faz. Quando o governo do host emitiu o Passport, ele também não sabia a viagem. Ele é confiável para a agência que emitiu o certificado de nascimento ou outra documentação. A agência que emitiu o certificado de nascimento, por sua vez, é confiável para o médico que assinou o certificado. O médico testemunhou o nascimento do viagem e carimbava o certificado com a prova direta da identidade, nesse caso, com a superfície do newborn. A confiança que é transferida dessa forma, por meio de intermediários confiáveis, é transitiva.

A confiança transitiva é a base para a segurança de rede na arquitetura de cliente/servidor do Windows. Uma relação de confiança flui em um conjunto de domínios, como uma árvore de domínio, e forma uma relação entre um domínio e todos os domínios que confiam nesse domínio. Por exemplo, se o domínio A tiver uma relação de confiança transitiva com o domínio B, e se o domínio B confiar no domínio C, o domínio A confiará no domínio C.

Há uma diferença entre autenticação e autorização. Com a autenticação, o sistema comprova que você é quem diz que está. Com a autorização, o sistema verifica se você tem direitos para fazer o que deseja fazer. Para levar a borda à analogia para a próxima etapa, simplesmente Autenticando que o viagem é o proprietário adequado de um Passport válido, não necessariamente autoriza o viagem a inserir um país. Os residentes de um determinado país têm permissão para inserir outro país simplesmente apresentando um passaporte apenas em situações em que o país inserido concede permissão ilimitada para todos os cidadãos do país específico a serem inseridos.

Da mesma forma, você pode conceder a todos os usuários de determinadas permissões de domínio para acessar um recurso. Qualquer usuário que pertença a esse domínio tem acesso ao recurso, assim como o Canadá permite que os cidadãos dos EUA insiram o Canadá. No entanto, os cidadãos americanos que tentam entrar no Brasil ou na Índia acham que não podem inserir esses países simplesmente apresentando um passaporte, pois esses dois países precisam visitar cidadãos americanos para ter uma visa válida. Portanto, a autenticação não garante o acesso a recursos ou autorização para usar recursos.

## <a name="credentials"></a>Credenciais
Um passaporte e, possivelmente, as visas associadas são as credenciais aceitas para um viagem. No entanto, essas credenciais podem não permitir que um viagem Insira ou acesse todos os recursos em um país. Por exemplo, credenciais adicionais são necessárias para participar de uma conferência. No Windows, as credenciais podem ser gerenciadas para possibilitar que os proprietários da conta acessem recursos pela rede sem ter que fornecer repetidamente suas credenciais. Esse tipo de acesso permite que os usuários sejam autenticados uma vez pelo sistema para acessar todos os aplicativos e fontes de dados que eles estão autorizados a usar sem inserir outro identificador de conta ou senha. A plataforma Windows aproveita a capacidade de usar uma identidade de usuário único (mantida por Active Directory) pela rede armazenando em cache localmente as credenciais de usuário na LSA (autoridade de segurança local) do sistema operacional. Quando um usuário faz logon no domínio, os pacotes de autenticação do Windows usam de modo transparente as credenciais para fornecer logon único ao autenticar as credenciais para recursos de rede. Para obter mais informações sobre credenciais, consulte [processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md).

Uma forma de autenticação multifator para o viagem pode ser o requisito de executar e apresentar vários documentos para autenticar sua identidade, como as informações de registro do Passport e da conferência. O Windows implementa esse formulário ou autenticação por meio de cartões inteligentes, cartões inteligentes virtuais e tecnologias biométricas.

## <a name="security-principals-and-accounts"></a>Entidades de segurança e contas
No Windows, qualquer usuário, serviço, grupo ou computador que possa iniciar a ação é uma entidade de segurança. As entidades de segurança têm contas, que podem ser locais para um computador ou ser baseadas em domínio. Por exemplo, computadores ingressados no domínio do cliente do Windows podem participar de um domínio de rede comunicando-se com um controlador de domínio, mesmo quando nenhum usuário humano está conectado. Para iniciar as comunicações, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a autoridade de segurança local no controlador de domínio autentica a identidade do computador e, em seguida, define o contexto de segurança do computador da mesma forma que faria com uma entidade de segurança humana. Esse contexto de segurança define a identidade e os recursos de um usuário ou serviço em um computador específico ou um usuário, serviço, grupo ou computador em uma rede. Por exemplo, ele define os recursos, como um compartilhamento de arquivos ou impressora, que pode ser acessado e as ações, como leitura, gravação ou modificação, que podem ser executadas por um usuário, serviço ou computador nesse recurso. Para obter mais informações, consulte [entidades de segurança](/windows/security/identity-protection/access-control/security-principals).

Uma conta é um meio de identificar um solicitante – o usuário ou serviço humano solicitando acesso ou recursos. O viagem que mantém o Passport autêntico possui uma conta com o país do host. Os usuários, grupos de usuários, objetos e serviços podem ter contas individuais ou compartilhar contas. As contas podem ser membros de grupos e podem receber direitos e permissões específicas. As contas podem ser restritas ao computador local, grupo de trabalho, rede ou ser atribuídas à associação a um domínio.

As contas internas e os grupos de segurança, dos quais são membros, são definidos em cada versão do Windows. Usando grupos de segurança, você pode atribuir as mesmas permissões de segurança a muitos usuários que foram autenticados com êxito, o que simplifica a administração do acesso. As regras para a emissão de passaportes podem exigir que o viagem seja atribuído a determinados grupos, como Business ou Tourist ou governo. Esse processo garante permissões de segurança consistentes em todos os membros de um grupo. O uso de grupos de segurança para atribuir permissões significa que o controle de acesso dos recursos permanece constante e fácil de gerenciar e auditar. Ao adicionar e remover usuários que exigem acesso dos grupos de segurança apropriados, conforme necessário, você pode minimizar a frequência de alterações nas ACLs (listas de controle de acesso).

As contas de serviço gerenciado autônomo e as contas virtuais foram introduzidas no Windows Server 2008 R2 e no Windows 7 para fornecer os aplicativos necessários, como o Microsoft Exchange Server e o Serviços de Informações da Internet (IIS), com o isolamento de suas próprias contas de domínio, ao mesmo tempo que elimina a necessidade de um administrador administrar manualmente o SPN (nome da entidade de serviço) e as credenciais dessas contas. As contas de serviço gerenciado de grupo foram introduzidas no Windows Server 2012 e fornecem a mesma funcionalidade no domínio, mas também estende essa funcionalidade em vários servidores. Ao se conectarem a um serviço hospedado em um farm de servidores, como o Balanceamento de Carga de Rede, os protocolos de autenticação que oferecem suporte para autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança.

Para obter mais informações sobre contas, consulte:

-   [Contas do Active Directory](/windows/security/identity-protection/access-control/active-directory-accounts)

-   [Grupos de segurança do Active Directory](/windows/security/identity-protection/access-control/active-directory-security-groups)

-   [Contas locais](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Contas da Microsoft](/windows/security/identity-protection/access-control/microsoft-accounts)

-   [Contas de serviço](/windows/security/identity-protection/access-control/service-accounts)

-   [Identidades especiais](/windows/security/identity-protection/access-control/special-identities)

## <a name="delegated-authentication"></a>Autenticação delegada
Para usar a analogia de viagem, os países podem emitir o mesmo acesso a todos os membros de uma delegação governamental oficial, desde que os delegados sejam bem conhecidos. Essa delegação permite que um membro atue na autoridade de outro membro. No Windows, a autenticação delegada ocorre quando um serviço de rede aceita uma solicitação de autenticação de um usuário e assume a identidade desse usuário para iniciar uma nova conexão com um segundo serviço de rede. Para dar suporte à autenticação delegada, você deve estabelecer servidores front-end ou de primeira camada, como servidores Web, que são responsáveis por manipular solicitações de autenticação de cliente e servidores de back-end ou de n camadas, como bancos de dados grandes, que são responsáveis por armazenar informações. Você pode delegar o direito de configurar a autenticação delegada aos usuários em sua organização para reduzir a carga administrativa em seus administradores.

Ao estabelecer um serviço ou computador como confiável para delegação, você permite que esse serviço ou computador conclua a autenticação delegada, receba um tíquete para o usuário que está fazendo a solicitação e, em seguida, acesse as informações para esse usuário. Esse modelo restringe o acesso a dados em servidores back-end apenas para os usuários ou serviços que apresentam credenciais com os tokens de controle de acesso corretos. Além disso, ele permite a auditoria de acesso desses recursos de back-end. Ao exigir que todos os dados sejam acessados por meio de credenciais que são delegadas ao servidor para uso em nome do cliente, você garante que o servidor não possa ser comprometido e que você possa obter acesso a informações confidenciais armazenadas em outros servidores. A autenticação delegada é útil para aplicativos multicamadas que são projetados para usar recursos de logon único em vários computadores.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticação em relações de confiança entre domínios
A maioria das organizações que têm mais de um domínio tem uma necessidade legítima para os usuários acessarem recursos compartilhados localizados em um domínio diferente, assim como o viagem é permitido viajar para regiões diferentes no país. O controle desse acesso exige que os usuários em um domínio também possam ser autenticados e autorizados a usar recursos em outro domínio. Para fornecer recursos de autenticação e autorização entre clientes e servidores em domínios diferentes, deve haver uma relação de confiança entre os dois domínios. As relações de confiança são a tecnologia subjacente pela qual ocorrem comunicações Active Directory seguras e são um componente de segurança integral da arquitetura de rede do Windows Server.

Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio confiam nas autenticações provenientes do outro domínio. As relações de confiança ajudam a fornecer acesso controlado a recursos compartilhados em um domínio de recurso – o domínio confiável, verificando se as solicitações de autenticação de entrada são provenientes de uma autoridade confiável – o domínio confiável. Dessa forma, as relações de confiança agem como pontes que permitem que apenas solicitações de autenticação validadas percorram entre domínios.

Como uma relação de confiança específica passa solicitações de autenticação depende de como ela está configurada. As relações de confiança podem ser unidirecionais, fornecendo acesso do domínio confiável a recursos no domínio confiante, ou bidirecional, fornecendo acesso de cada domínio aos recursos no outro domínio. As relações de confiança também são intransitivas; nesse caso, a confiança existe somente entre os dois domínios de parceiros de confiança, ou transitivos, caso em que a confiança se estende automaticamente a quaisquer outros domínios nos quais os parceiros confiam.

Para obter informações sobre como uma relação de confiança funciona, consulte [como as relações de confiança de domínio e floresta funcionam](/previous-versions/windows/it-pro/windows-server-2003/cc773178(v=ws.10)).

### <a name="protocol-transition"></a>Transição de protocolo
A transição de protocolo auxilia os designers de aplicativos, permitindo que os aplicativos ofereçam suporte a mecanismos de autenticação diferentes na camada de autenticação do usuário e alternando para o protocolo Kerberos para recursos de segurança, como autenticação mútua e delegação restrita, nas camadas de aplicativo subsequentes.

Para obter mais informações sobre a transição de protocolo, consulte [transição de protocolo Kerberos e delegação restrita](/previous-versions/windows/it-pro/windows-server-2003/cc758097(v=ws.10)).

### <a name="constrained-delegation"></a>Delegação restrita
A delegação restrita oferece aos administradores a capacidade de especificar e impor limites de confiança do aplicativo, limitando o escopo em que os serviços de aplicativo podem agir em nome de um usuário. Você pode especificar serviços específicos dos quais um computador confiável para delegação pode solicitar recursos. A flexibilidade para restringir os direitos de autorização dos serviços ajuda a melhorar o design de segurança do aplicativo, reduzindo as oportunidades de comprometimento por serviços não confiáveis.

Para obter mais informações sobre a delegação restrita, consulte [visão geral da delegação restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="additional-references"></a>Referências adicionais
[Visão geral técnica de logon e autenticação do Windows](https://technet.microsoft.com/library/dn269029.aspx)