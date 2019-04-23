---
title: Conceitos de autenticação do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee3fa19efa8c5098008749a467e649edcb5bb716
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876497"
---
# <a name="windows-authentication-concepts"></a>Conceitos de autenticação do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de visão geral de referência descreve os conceitos no qual a autenticação do Windows se baseia.

A autenticação é um processo de verificação da identidade de um objeto ou uma pessoa. Quando você autentica um objeto, a meta é verificar se ele é genuíno. Quando você autentica uma pessoa, a meta é verificar se a pessoa não é um impostor.

Em um contexto de rede, a autenticação é o ato de fornecer identidade para um aplicativo ou recurso de rede. Normalmente, identidade está comprovada por uma operação criptográfica que usa uma chave somente o usuário conhece (assim como acontece com criptografia de chave pública) ou uma chave compartilhada. O lado do servidor da troca de autenticação compara os dados assinados com uma chave de criptografia conhecida para validar a tentativa de autenticação.

O armazenamento das chaves criptográficas em um local central seguro torna o processo de autenticação escalonável e passível de manutenção. O Active Directory é a recomendada e a tecnologia padrão para armazenar informações de identidade, que incluem a criptografia de chaves que são as credenciais do usuário. O Active Directory é exigido para as implementações de Kerberos e NTLM padrão.

Técnicas de autenticação de intervalo de um logon simples para um sistema operacional ou de uma entrada para um serviço ou aplicativo, que identifica os usuários com base em algo que apenas o usuário conhece, como uma senha, para os mecanismos de segurança mais eficientes que usam algo que has'such o usuário, como tokens, certificados de chave pública, imagens ou atributos biológicos. Em um ambiente de negócios, os usuários podem acessar vários aplicativos em muitos tipos de servidores em um único local ou em vários locais. Por esses motivos, a autenticação deve dar suporte a ambientes de outras plataformas e de outros sistemas operacionais Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticação e autorização: Uma analogia da viagem
Uma analogia da viagem pode ajudar a explicar como funciona a autenticação. Algumas tarefas preparatórias são geralmente necessárias para iniciar a jornada. A viagem deve provar sua identidade true suas autoridades de host. Essa verificação pode ser na forma de prova de cidadania, Naturalidade, um comprovante de pessoal, fotografias, ou tudo o que é exigido por lei do país de host. Identidade da viagem foi validada pela emissão de um passaporte, que é análogo a uma conta de sistema emitido e administrados por uma organização – a entidade de segurança. O passport e o destino pretendido com base em um conjunto de regras e regulamentos emitidos pela autoridade de governamental.

**A jornada**

Quando o Viajante chega na borda internacional, uma proteção de borda solicitará as credenciais e a viagem apresenta seu passaporte. O processo é composto por duas etapas:

-   O protetor autentica o passport, verificando se ele foi emitido por uma autoridade de segurança que o governo local confia (relações de confiança, pelo menos, para emitir passaportes) e verificando se o passport não foi modificado.

-   O protetor autentica a viagem, verificando se a face corresponde a face da pessoa mostrada no passport e que outras credenciais necessárias estão em boas condições.

Se o passport prova seja válida e a viagem prova ser seu proprietário, a autenticação é bem-sucedida e a viagem pode receber permissão de acesso entre a borda.

Relação de confiança transitiva entre as autoridades de segurança é a base de autenticação; o tipo de autenticação que ocorre em viagens internacionais baseia-se a relação de confiança. O governo local não sabe a viagem, mas ele confia que o governo host faz. Quando o governo host emitido do passport, ele não sabia o Viajante tanto. Ele confiável a agência que emitiu o certificado de nascimento ou outras documentações. A agência que emitiu o certificado de nascimento, por sua vez, confiável médico que assinou o certificado. O médico testemunhamos nascimento da viagem e identificado o certificado com direto de prova de identidade, nesse caso, com a superfície do recém-nascido. Relação de confiança que é transferida dessa forma, por meio de intermediários confiáveis, é transitiva.

Relação de confiança transitiva é a base para a segurança de rede na arquitetura de cliente/servidor do Windows. Uma relação de confiança percorrem um conjunto de domínios, como uma árvore de domínio e forma uma relação entre um domínio e todos os domínios que confiam nesse domínio. Por exemplo, se o domínio A tem uma relação de confiança transitiva com o domínio B, e se o domínio B confia no domínio C, em seguida, domínio A confia no domínio C.

Há uma diferença entre autenticação e autorização. Com a autenticação, o sistema de prova que você é quem diz ser. Com a autorização, o sistema verifica se você tem direitos para fazer o que você deseja fazer. Para aproveitar a analogia de borda para a próxima etapa, autenticar apenas que a viagem é o proprietário adequado de um passport válido não necessariamente autoriza o Viajante para inserir um país. Os residentes de um país específico tem permissão para inserir outro país, simplesmente apresentando um passaporte somente em situações em que o país que está sendo inserido concede permissão ilimitada para todos os cidadãos desse país específico para entrar.

Da mesma forma, você pode conceder um determinadas permissões de domínio para acessar um recurso de todos os usuários. Qualquer usuário que pertence ao domínio que tem acesso ao recurso, assim como no Canadá permite que cidadãos americanos insira Canadá. No entanto, cidadãos americanos tentar inserir Brasil ou Índia descobrir que eles não possam inserir esses países simplesmente apresentando um passaporte porque ambos esses países exigem visitando cidadãos americanos para ter um visa válido. Assim, autenticação não garante o acesso a recursos ou autorização para usar os recursos.

## <a name="credentials"></a>Credenciais
Um passaporte e visas possivelmente associados são as credenciais aceitas para uma viagem. No entanto, essas credenciais talvez não permitam uma viagem Insira ou acessar todos os recursos dentro de um país. Por exemplo, credenciais adicionais são necessários para participar de uma conferência. No Windows, as credenciais podem ser gerenciadas para tornar possível que os proprietários das contas acessar recursos através da rede sem a necessidade de fornecer suas credenciais repetidamente. Esse tipo de acesso permite que os usuários a ser autenticado uma vez pelo sistema para acessar todos os aplicativos e fontes de dados que eles estão autorizados a usar sem inserir outro identificador de conta ou senha. A plataforma Windows explora a capacidade de usar uma identidade de usuário único (mantida pelo Active Directory) em toda a rede por localmente em cache as credenciais do usuário do sistema operacional segurança LSA (autoridade Local). Quando um usuário faz logon no domínio, pacotes de autenticação do Windows utilizar de forma transparente as credenciais para fornecer logon único ao autenticar as credenciais aos recursos da rede. Para obter mais informações sobre credenciais, consulte [processos de credenciais na autenticação do Windows](credentials-processes-in-windows-authentication.md).

Uma forma de autenticação multifator para a viagem pode ser o requisito para transportar e apresentar vários documentos para autenticar sua identidade, como informações de registro do passport e conferência. Windows implementa esse formulário ou a autenticação por meio de tecnologias biométricas, cartões inteligentes virtuais e cartões inteligentes. 

## <a name="security-principals-and-accounts"></a>Contas e entidades de segurança
No Windows, qualquer usuário, serviço, grupo ou computador que pode iniciar a ação é uma entidade de segurança. Entidades de segurança têm contas, que podem ser local para um computador ou ser baseado em domínio. Por exemplo, computadores ingressados no domínio do cliente do Windows podem participar em um domínio de rede por meio da comunicação com um controlador de domínio, mesmo quando nenhum usuário humano é conectado. Para iniciar a comunicação, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a autoridade de segurança local no controlador de domínio autentica a identidade do computador e, em seguida, define o contexto de segurança do computador como faria para uma entidade de segurança humana. Este contexto de segurança define a identidade e recursos de um usuário ou serviço em um computador específico ou um usuário, serviço, grupo ou computador em uma rede. Por exemplo, ele define os recursos, como um compartilhamento de arquivos ou impressora, que pode ser acessada e as ações, como leitura, gravação ou modificar, que pode ser executada por um usuário, um serviço ou um computador nesse recurso. Para obter mais informações, consulte [as entidades de segurança](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Uma conta é um meio para identificar um requerente – o usuário humano ou serviço – solicitando acesso ou recursos. A viagem que detém o passport autêntica possui uma conta com o país do host. Os usuários, grupos de usuários, objetos e serviços podem ter contas individuais ou compartilhar contas. Contas podem ser membro dos grupos e podem ser atribuídas permissões e direitos específicos. Contas podem ser restritas para o computador local, o grupo de trabalho, a rede ou ser atribuídas a associação a um domínio.

Contas internas e os grupos de segurança, dos quais eles são membros, são definidos em cada versão do Windows. Usando grupos de segurança, você pode atribuir as mesmas permissões de segurança para vários usuários que são autenticados com êxito, o que simplifica a administração de acesso. Regras para a emissão de passaportes podem exigir que a viagem ser atribuídos a determinados grupos, como empresa, turísticas ou governamental. Esse processo garante as permissões de segurança consistente em todos os membros de um grupo. Usando grupos de segurança para atribuir permissões significa que o controle de recursos de acesso permanece constante e fácil de gerenciar e auditar. Adicionando e removendo os usuários que exigem acesso de grupos de segurança apropriados, conforme necessário, você pode minimizar a frequência das alterações para acessar listas de controle (ACLs).

Contas de serviço gerenciado de autônomo e contas virtuais foram introduzidas no Windows Server 2008 R2 e Windows 7 para fornecer aplicativos necessários, como o Microsoft Exchange Server e serviços de informações da Internet (IIS), com o isolamento de seu próprio domínio contas, eliminando a necessidade de um administrador administrar manualmente o nome da entidade de serviço (SPN) e as credenciais dessas contas. Contas de serviço gerenciado de grupo foram introduzidas no Windows Server 2012 e fornece a mesma funcionalidade dentro do domínio, mas também estende essa funcionalidade por vários servidores. Ao se conectarem a um serviço hospedado em um farm de servidores, como o Balanceamento de Carga de Rede, os protocolos de autenticação que oferecem suporte para autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança.

Para obter mais informações sobre contas, consulte:

-   [Contas do Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Grupos de segurança do Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Contas locais](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Contas da Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Contas de serviço](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identidades especiais](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticação delegada
Para usar a analogia da viagem, países podem emitir o mesmo acesso a todos os membros de uma delegação governamental oficial, desde que os delegados são conhecidos. Essa delegação permite que um membro funcione na autoridade de outro membro. No Windows, a autenticação delegada ocorre quando um serviço de rede aceita uma solicitação de autenticação de um usuário e assume a identidade de usuário para iniciar uma nova conexão a um segundo serviço de rede. Para dar suporte a autenticação delegada, você deve estabelecer servidores front-end ou a primeira camada, como servidores web, que serão responsáveis por lidar com solicitações de autenticação de cliente e servidores de back-end ou de n camadas, como grandes bancos de dados, que são responsáveis por armazenamento de informações. Você pode delegar o direito para configurar a autenticação delegada aos usuários em sua organização para reduzir a carga administrativa em seus administradores.

Estabelecendo um serviço ou um computador como confiáveis para delegação, você pode deixar esse serviço ou o computador para concluir a autenticação delegada, recebe um tíquete do usuário que está fazendo a solicitação e, em seguida, acessar as informações de que o usuário. Esse modelo restringe o acesso a dados nos servidores back-end apenas para esses usuários ou serviços que apresentar credenciais com os tokens de controle de acesso corretas. Além disso, ele permite a auditoria de acesso a um desses recursos de back-end. Ao exigir que todos os dados de ser acessado por meio de credenciais que são delegadas ao servidor para uso em nome do cliente, verifique se o servidor não pode ser comprometido e que você pode obter acesso a informações confidenciais que está armazenado em outros servidores. Autenticação delegada é útil para aplicativos de várias camados que são projetados para usar recursos de logon único em vários computadores.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticação em relações de confiança entre domínios
A maioria das organizações que têm mais de um domínio tiver uma verdadeira necessário para que os usuários acessem recursos compartilhados que estão localizados em um domínio diferente, assim como a viagem é permitida uma viagem para diferentes regiões no país. Controlar o acesso a este requer que os usuários em um domínio também podem ser autenticados e autorizados para usar os recursos em outro domínio. Para fornecer autenticação e autorização de recursos entre clientes e servidores em domínios diferentes, deve haver uma relação de confiança entre os dois domínios. Relações de confiança são a tecnologia subjacente pelo qual comunicações protegidas do Active Directory ocorrem e são um componente de segurança integral da arquitetura de rede do Windows Server.

Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio de confiança as autenticações provenientes de outro domínio. Relações de confiança ajudam a fornecer acesso controlado aos recursos compartilhados em um domínio de recurso – o domínio confiante – verificando-se que a autenticação entrada as solicitações vêm de uma autoridade confiável – o domínio confiável. Dessa forma, relações de confiança agem como pontes que permitem apenas validado viagem de solicitações de autenticação entre domínios.

Como uma relação de confiança específica passa as solicitações de autenticação depende de como ela é configurada. Relações de confiança podem ser unidirecional, fornecendo acesso do domínio confiável aos recursos no domínio confiante ou bidirecional, fornecendo acesso de cada domínio aos recursos em outro domínio. Relações de confiança também estão ambos intransitiva, no qual a confiança caso existe somente entre os domínios de parceiro de confiança de duas, ou transitiva, nesse caso, confiança estende automaticamente a quaisquer outros domínios que confie em qualquer um dos parceiros.

Para obter informações sobre o funcionamento de uma relação de confiança, consulte [como o domínio e o trabalho de relações de confiança de floresta](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transição de protocolo
Transição de protocolo ajuda os designers de aplicativos, permitindo que os aplicativos dão suporte a diferentes mecanismos de autenticação na camada de autenticação de usuário e alternando para o protocolo Kerberos para recursos de segurança, como a autenticação mútua e delegação restrita, nas camadas de aplicativo subsequente.

Para obter mais informações sobre a transição de protocolo, consulte [transição do protocolo Kerberos e delegação restrita](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delegação restrita
A delegação restrita fornece aos administradores a capacidade de especificar e impor limites de confiança do aplicativo restringindo o escopo em que os serviços de aplicativo podem atuar em nome do usuário. Você pode especificar os serviços específicos do qual um computador que seja confiável para delegação pode solicitar recursos. A flexibilidade para restringir direitos de autorização para serviços ajuda a melhorar o design de segurança de aplicativo, reduzindo as chances de comprometimento pelos serviços não confiáveis.

Para obter mais informações sobre a delegação restrita, consulte [visão geral de delegação restrita de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Consulte também
[Visão geral técnica de Logon do Windows e autenticação](https://technet.microsoft.com/library/dn269029.aspx)


