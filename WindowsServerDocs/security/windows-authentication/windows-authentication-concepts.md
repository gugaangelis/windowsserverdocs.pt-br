---
title: "Conceitos de autenticação do Windows"
description: "Segurança do Windows Server"
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
ms.openlocfilehash: 27e124c971926edd33f102fe6009a0c552c6d814
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-concepts"></a>Conceitos de autenticação do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de visão geral de referência descreve os conceitos no qual a autenticação do Windows se baseia.

Autenticação é um processo para verificar a identidade de um objeto ou a pessoa. Quando você autentica um objeto, o objetivo é verificar se o objeto é original. Quando você autentica uma pessoa, o objetivo é verificar se a pessoa não é um impostor.

Em um contexto de rede, a autenticação é o ato de provar a identidade de um aplicativo de rede ou recurso. Normalmente, identidade provou por uma operação criptográfica que usa uma chave somente o usuário sabe (assim como com criptografia de chave pública) ou uma chave compartilhada. O lado do servidor do exchange a autenticação compara os dados assinados com uma chave criptográfica conhecida para validar a tentativa de autenticação.

Armazenando as chaves de criptografia em um local central seguro torna o processo de autenticação pode ser mantido e escalonável. Active Directory é recomendado e tecnologia padrão para armazenar informações de identidade, que incluem a criptografia chaves que são as credenciais do usuário. Active Directory é necessário para implementações de Kerberos e NTLM padrão.

Intervalo de técnicas de autenticação de um logon simple para um sistema operacional ou uma entrada para um serviço ou aplicativo, que identifica os usuários com base em algo que somente o usuário sabe, como uma senha, mais potentes mecanismos de segurança que usam algo que has'such o usuário como tokens, certificados de chave pública, imagens ou atributos biológicos. Em um ambiente de negócios, os usuários podem acessar vários aplicativos em vários tipos de servidores em um único local ou em vários locais. Por esses motivos, autenticação deve dar suporte a ambientes para outras plataformas e para outros sistemas operacionais Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticação e autorização: uma analogia de viagem
Uma analogia viagem pode ajudar a explicar como funciona a autenticação. Algumas tarefas preparatórias são geralmente necessárias para iniciar o trajeto. A viagem deve provar sua identidade para seus autoridades de host. Essa prova pode ser na forma de comprovação de sua cidadania, Naturalidade, um comprovantes pessoais, fotografias ou tudo o que é exigido por lei do país host. Identidade do Viajante é validada pela emissão de um passport, que é semelhante a uma conta do sistema emitido e administrado por uma organização – a entidade de segurança. O passport e o destino pretendido são baseados em um conjunto de regras e os regulamentos aplicáveis emitidos pela autoridade governamental.

**A jornada**

Quando chega a viagem na borda internacional, um protetor de borda solicita credenciais e o Viajante apresenta jid passport. O processo é dupla:

-   O protetor autentica o passport, verificando se ele foi emitido por uma autoridade de segurança do governo local confia (relações de confiança, pelo menos, para emitir passports) e verificando se o passport não foi modificado.

-   O protetor autentica a viagem, verificando se o rosto coincide com a face da pessoa como mostrada no passport e que outras credenciais necessárias estão em boas condições.

Se o passport prova seja válido e a viagem prova para ser seu proprietário, autenticação for bem-sucedida, e a viagem pode receber permissão de acesso entre a borda.

Confiança transitiva entre autoridades de segurança é a base de autenticação; o tipo de autenticação que ocorre em viagens internacionais baseia-se na relação de confiança. O governo local não sabe a viagem, mas ele confia que faz o governo de host. Quando o governo host emitiu o passport, ele não sabia a viagem ou. Ele confiável a agência que emitiu o certificado de nascimento ou outra documentação. Agência que emitiu o certificado de nascimento, por sua vez, confiável o médico quem assinou o certificado. O médico testemunhamos nascimento da viagem e estampou o certificado com direta comprovação de identidade, nesse caso, com o volume do recém-nascido. Confiança que é transferida dessa forma, por meio de intermediários confiáveis, é transitiva.

Relação de confiança transitiva é a base de segurança de rede na arquitetura de cliente/servidor do Windows. Uma relação de confiança flui ao longo de um conjunto de domínios, como uma árvore de domínio e forma uma relação entre um domínio e todos os domínios que confiam nesse domínio. Por exemplo, se um domínio tem uma relação de confiança transitiva com o domínio B, e o domínio B confia no domínio C, em seguida, domínio A confia no C.

Há uma diferença entre autenticação e autorização. Com a autenticação, o sistema comprova que você é quem diz que ser. Com a autorização, o sistema verifica se você tem direitos de fazer o que você deseja fazer. Para tirar a analogia border para a próxima etapa, simplesmente autenticar o Viajante é o proprietário adequado de um passport válido não necessariamente autoriza a viagem para inserir um país. Residentes de um país específico têm permissão para inserir outro país simplesmente apresentando um passport apenas em situações em que país/região está sendo inserido concede permissão ilimitado para todos os cidadãos desse país específico para inserir.

Da mesma forma, você pode conceder todos os usuários de um determinadas permissões de domínio para acessar um recurso. Qualquer usuário que pertence ao domínio que tenha acesso ao recurso, assim como Canadá vamos cidadãos americanos insira Canadá. No entanto, cidadãos americanos tentar inserir Brasil ou Índia encontrar que não podem inserir nesses países simplesmente, apresentando um passport porque ambos nesses países exigem visitando cidadãos americanos para ter um visa válido. Assim, a autenticação não garante acesso a recursos ou autorização para usar recursos.

## <a name="credentials"></a>Credenciais
Um passport e possivelmente associados visas são as credenciais aceitas para uma viagem. No entanto, essas credenciais podem não permitir que uma viagem inserir ou acessar todos os recursos dentro de um país. Por exemplo, as credenciais adicionais são necessárias para participar da conferência. No Windows, as credenciais podem ser gerenciadas para possibilitar para titulares de contas acessar recursos pela rede sem ter que fornecer suas credenciais repetidamente. Esse tipo de acesso permite que os usuários ser autenticado avulso pelo sistema para acessar todos os aplicativos e fontes de dados que eles estão autorizados a utilizar sem inserir outro identificador da conta ou senha. A plataforma Windows explora a capacidade de usar uma identidade de usuário único (mantida pelo Active Directory) através da rede por localmente em cache as credenciais do usuário do sistema operacional segurança LSA (autoridade Local). Quando um usuário fizer logon no domínio, pacotes de autenticação do Windows utilizar de forma transparente as credenciais para fornecer o logon único ao autenticar as credenciais aos recursos de rede. Para saber mais sobre as credenciais, consulte [processos de credenciais de autenticação do Windows](credentials-processes-in-windows-authentication.md).

Uma forma de autenticação multifator para a viagem pode ser o requisito de carregar e apresentar vários documentos para autenticar sua identidade, como uma informações de registro do passport e conferência. O Windows implementa esse formulário ou autenticação por meio de cartões inteligentes, cartões inteligentes virtuais e tecnologias biométricas. 

## <a name="security-principals-and-accounts"></a>Contas e entidades de segurança
No Windows, qualquer usuário, serviço, grupo ou computador que pode iniciar ação é uma entidade de segurança. Entidades de segurança têm contas, que podem ser locais para um computador ou ser baseado em domínio. Por exemplo, computadores de domínio de cliente do Windows podem participar em um domínio de rede se comunicando com um controlador de domínio, mesmo quando nenhum usuário humano é conectado. Para iniciar as comunicações, o computador deve ter uma conta ativa no domínio. Antes de aceitar comunicações do computador, a autoridade de segurança local no controlador de domínio autentica a identidade do computador e, em seguida, define o contexto de segurança do computador como faria para uma entidade de segurança humana. Esse contexto de segurança define a identidade e os recursos de um usuário ou um serviço em um computador específico ou um usuário, serviço, grupo ou computador em uma rede. Por exemplo, ele define os recursos, como um compartilhamento de arquivos ou impressora, que pode ser acessada e as ações, como leitura, gravação ou modificar, que pode ser executada por um usuário, serviço ou computador nesse recurso. Para obter mais informações, consulte [entidades de segurança](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Uma conta é uma maneira de identificar um reclamante – o usuário humano ou serviço – solicitando acesso ou recursos. A viagem que mantém o passport autêntico possui uma conta com o país do host. Os usuários, grupos de usuários, objetos e serviços podem ter contas individuais ou compartilhar contas. Contas podem ser membros de grupos e podem ser atribuídas específicos direitos e permissões. Contas podem ser restritas para o computador local, o grupo de trabalho, rede ou ser atribuídas a associação a um domínio.

Contas internas e os grupos de segurança, dos quais eles são membros, são definidos em cada versão do Windows. Usando grupos de segurança, você pode atribuir as mesmas permissões de segurança para muitos usuários que foram autenticados com êxito, o que simplifica a administração de acesso. Regras para a emissão de passports podem exigir que o Viajante ser atribuído a determinados grupos, como empresa, ou turísticos ou governo. Esse processo garante permissões de segurança consistente entre todos os membros de um grupo. Usando grupos de segurança para atribuir permissões significa que o controle de recursos de acesso permanece constante e fácil de gerenciar e auditoria. Adicionando e removendo usuários que precisam de acesso dos grupos de segurança adequadas conforme necessário, você pode minimizar a frequência das alterações para acessar listas de controle (ACLs).

Autônomo gerenciado contas de serviço e contas virtuais foram introduzidas no Windows Server 2008 R2 e Windows 7 para fornecer aplicativos necessários, como o Microsoft Exchange Server e o Internet Information Services (IIS), com o isolamento de suas próprias contas de domínio, enquanto eliminando a necessidade de um administrador manualmente administrar o nome de entidade de serviço (SPN) e credenciais para essas contas. Contas de serviço gerenciado grupo foram introduzidas no Windows Server 2012 e fornece a mesma funcionalidade dentro do domínio, mas também se estende essa funcionalidade ao longo de vários servidores. Ao se conectar a um serviço hospedado em um farm de servidores, como balanceamento de carga de rede, os protocolos de autenticação dando suporte à autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança.

Para obter mais informações sobre as contas, consulte:

-   [Contas do Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Grupos de segurança do Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Contas locais](https://technet.microsoft.com/itpro/windows/keep-secure/local-accounts)

-   [Contas da Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Contas de serviço](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identidades especiais](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticação delegada
Para usar a analogia de viagens, países podem emitir o mesmo acesso a todos os membros de uma delegação governamental oficial, desde que os representantes são bem conhecidos. Esta delegação vamos um membro agir na autoridade de outro membro. No Windows, a autenticação delegada ocorre quando um serviço de rede aceita uma solicitação de autenticação de um usuário e assume a identidade do usuário para iniciar uma nova conexão com um serviço de rede a segunda. Para dar suporte à autenticação delegada, você deve estabelecer servidores front-end ou de primeiro nível, como servidores web, que serão responsáveis por manipular solicitações de autenticação de cliente e servidores fileiras ou back-end, como bancos de dados grandes, que serão responsáveis por armazenar informações. Você pode delegar o direito de configurar a autenticação delegada para os usuários em sua organização para reduzir a carga administrativa em seus administradores.

Estabelecendo um computador ou um serviço como confiáveis para delegação, você permitir que esse serviço ou o computador concluir a autenticação delegada, receber um tíquete de usuário que está fazendo a solicitação e acessar informações para que o usuário. Esse modelo restringe o acesso de dados em servidores back-end apenas para os usuários ou serviços que credenciais presente com os tokens de controle de acesso correto. Além disso, ele permite a auditoria de acesso desses recursos de back-end. Exigindo que todos os dados ser acessada por meio de credenciais delegadas para o servidor para uso em nome do cliente, você pode garantir que o servidor não pode ser comprometido e que você pode obter acesso a informações confidenciais que é armazenado em outros servidores. Autenticação delegada é útil para aplicativos de várias camados que são projetados para usar recursos de logon únicos em vários computadores.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticação em relações de confiança entre domínios
A maioria das organizações que têm mais de um domínio tem uma verdadeira necessário para que os usuários acessem recursos compartilhados que estão localizados em um domínio diferente, assim como a viagem é permitida viagens para diferentes regiões no país. Controlar esse acesso requer que os usuários em um domínio também podem ser autenticados e autorizados a utilizar os recursos em outro domínio. Para fornecer recursos de autenticação e autorização entre clientes e servidores em domínios diferentes, deve haver uma relação de confiança entre os dois domínios. Relações de confiança são a tecnologia subjacente pelo qual comunicações seguras do Active Directory ocorrem e são um componente de segurança integral da arquitetura de rede do Windows Server.

Quando existe uma relação de confiança entre dois domínios, os mecanismos de autenticação para cada domínio confiar as autenticações provenientes de outro domínio. Relações de confiança ajudam a oferecer acesso controlado a recursos compartilhados em um domínio do recurso – do domínio confiante – verificando que a autenticação entrada solicitações provenientes de uma autoridade confiável – o domínio confiável. Dessa forma, relações de confiança atuam como pontes que permitem apenas validado viagem de solicitações de autenticação entre domínios.

Como uma relação de confiança específica passa solicitações de autenticação depende de como ele está configurado. Relações de confiança podem ser, fornecendo acesso do domínio confiável aos recursos do domínio confiante unidirecional ou bidirecional, fornecendo acesso de cada domínio aos recursos em outro domínio. Confie também são ambos intransitiva, no qual confiança maiusculas existe apenas entre os domínios de parceiros de dois confiança, ou transitiva, nesse caso confiança automaticamente se estende para quaisquer outros domínios que qualquer um dos parceiros confie.

Para obter informações sobre como funciona uma relação de confiança, consulte [como domínio e o trabalho de relações de confiança de floresta](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transição de protocolo
Transição de protocolo ajuda os designers de aplicativos, permitindo que os aplicativos oferecem suporte a mecanismos de autenticação diferentes no nível de autenticação de usuário e alternando para o protocolo Kerberos para recursos de segurança, como autenticação mútua e delegação restrita, nas camadas do aplicativo subsequentes.

Para obter mais informações sobre a transição de protocolo, consulte [transição de protocolo Kerberos e delegação restrita](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delegação restrita
Delegação restrita oferece aos administradores a capacidade de especificar e impor limites de confiança do aplicativo, limitando o escopo em que os serviços de aplicativo podem agir em nome de um usuário. Você pode especificar determinados serviços em que um computador que é confiável para delegação pode solicitar recursos. A flexibilidade para restringir os direitos de autorização para serviços ajuda a melhorar o design de segurança do aplicativo, reduzindo as chances de comprometimento por serviços não confiáveis.

Para saber mais sobre a delegação restrita, consulte [visão geral de delegação restrita Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Consulte também
[Visão geral técnica de Logon do Windows e autenticação](https://technet.microsoft.com/library/dn269029.aspx)


