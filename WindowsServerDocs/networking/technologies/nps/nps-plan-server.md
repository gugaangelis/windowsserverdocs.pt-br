---
title: Planejar NPS como um servidor RADIUS
description: Este tópico fornece informações sobre o planejamento de implantação do servidor RADIUS do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbcf3338f2cd6d8662a84faf263b486e31b140e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405334"
---
# <a name="plan-nps-as-a-radius-server"></a>Planejar NPS como um servidor RADIUS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Quando você implanta o servidor de políticas de rede \(NPS @ no__t-1 como um servidor serviço RADIUS (RADIUS), o NPS executa autenticação, autorização e contabilização de solicitações de conexão para o domínio local e para domínios que confiam no domínio local. Você pode usar essas diretrizes de planejamento para simplificar a implantação do RADIUS.

Essas diretrizes de planejamento não incluem circunstâncias nas quais você deseja implantar o NPS como um proxy RADIUS. Quando você implanta o NPS como um proxy RADIUS, o NPS encaminha as solicitações de conexão para um servidor que executa o NPS ou outros servidores RADIUS em domínios remotos, domínios não confiáveis ou ambos. 

Antes de implantar o NPS como um servidor RADIUS em sua rede, use as diretrizes a seguir para planejar sua implantação.

- Planejar a configuração do NPS.

- Planejar clientes RADIUS.

- Planeje o uso de métodos de autenticação.

- Planejar políticas de rede.

- Planejar a contabilidade do NPS.

## <a name="plan-nps-configuration"></a>Planejar a configuração do NPS

Você deve decidir em qual domínio o NPS é membro. Para ambientes de vários domínios, um NPS pode autenticar credenciais para contas de usuário no domínio do qual é membro e para todos os domínios que confiam no domínio local do NPS. Para permitir que o NPS Leia as propriedades de discagem de contas de usuário durante o processo de autorização, você deve adicionar a conta de computador do NPS ao grupo RAS e NPSs para cada domínio.

Depois de determinar a associação de domínio do NPS, o servidor deve ser configurado para se comunicar com clientes RADIUS, também chamados de servidores de acesso à rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que o NPS registra no log de eventos e pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento da configuração do NPS, você pode usar as etapas a seguir.

- Determine as portas RADIUS que o NPS usa para receber mensagens RADIUS de clientes RADIUS. As portas padrão são as portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e portas 1813 e 1646 para mensagens de contabilização RADIUS.

- Se o NPS estiver configurado com vários adaptadores de rede, determine os adaptadores sobre os quais você deseja que o tráfego RADIUS seja permitido.

- Determine os tipos de eventos que você deseja que o NPS registre no log de eventos. Você pode registrar solicitações de autenticação rejeitadas, solicitações de autenticação bem-sucedidas ou ambos os tipos de solicitações.

- Determine se você está implantando mais de um NPS. Para fornecer tolerância a falhas para autenticação e contabilidade baseadas em RADIUS, use pelo menos dois NPSs. Um NPS é usado como o servidor RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS é então configurado em ambos os NPSs. Se o NPS primário ficar indisponível, os clientes RADIUS enviarão mensagens de solicitação de acesso para o NPS alternativo.

- Planeje o script usado para copiar uma configuração NPS para outras NPSs para economizar na sobrecarga administrativa e para evitar o configuração incorreto de um servidor. O NPS fornece os comandos netsh que permitem copiar toda ou parte de uma configuração NPS para importação em outro NPS. Você pode executar os comandos manualmente no prompt do netsh. No entanto, se você salvar a sequência de comandos como um script, poderá executar o script em uma data posterior se decidir alterar as configurações do servidor.

## <a name="plan-radius-clients"></a>Planejar clientes RADIUS

Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, servidores de rede virtual privada (VPN), comutadores compatíveis com 802.1 X e servidores dial-up. Os proxies RADIUS, que encaminham mensagens de solicitação de conexão para servidores RADIUS, também são clientes RADIUS. O NPS dá suporte a todos os servidores de acesso à rede e proxies RADIUS que estão em conformidade com o protocolo RADIUS, conforme descrito na RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)" e RFC 2866, "RADIUS Accounting".

>[!IMPORTANT]
>Clientes de acesso, como computadores cliente, não são clientes RADIUS. Somente os servidores de acesso à rede e os servidores proxy que dão suporte ao protocolo RADIUS são clientes RADIUS.

Além disso, os pontos de acesso sem fio e os comutadores devem ser compatíveis com a autenticação 802.1 X. Se você quiser implantar o protocolo de autenticação extensível \(EAP @ no__t-1 ou o protocolo de autenticação extensível protegido \(PEAP @ no__t-3, pontos de acesso e comutadores devem oferecer suporte ao uso de EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação adicionais baseados em PPP, como o PEAP, até que você tenha testado aqueles que pretende usar para acesso à rede.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específicos do fornecedor (VSAs) que você deve configurar no NPS. Se os servidores de acesso à rede exigirem VSAs, registre as informações de VSA para uso posterior ao configurar as políticas de rede no NPS. 

- Documente os endereços IP de clientes RADIUS e seu NPS para simplificar a configuração de todos os dispositivos. Ao implantar seus clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP do NPS inserido como o servidor de autenticação. E, ao configurar o NPS para se comunicar com seus clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in do NPS.

- Crie segredos compartilhados para configuração nos clientes RADIUS e no snap-in do NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou senha, que também será inserido no snap-in do NPS ao configurar clientes RADIUS no NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planejar o uso de métodos de autenticação

O NPS dá suporte a métodos de autenticação baseados em senha e em certificado. No entanto, nem todos os servidores de acesso à rede oferecem suporte aos mesmos métodos de autenticação. Em alguns casos, talvez você queira implantar um método de autenticação diferente com base no tipo de acesso à rede.

Por exemplo, talvez você queira implantar o acesso sem fio e VPN para sua organização, mas usar um método de autenticação diferente para cada tipo de acesso: EAP-TLS para conexões VPN, devido à forte segurança que o EAP com segurança de camada de transporte (EAP-TLS) fornece e PEAP-MS-CHAP v2 para conexões sem fio 802.1 X.

O PEAP com o protocolo de autenticação de handshake de desafio da Microsoft versão 2 (PEAP-MS-CHAP v2) fornece um recurso chamado reconexão rápida que é projetado especificamente para uso com computadores portáteis e outros dispositivos sem fio. A reconexão rápida permite que os clientes sem fio se movam entre pontos de acesso sem fio na mesma rede sem serem autenticados sempre que eles se associam a um novo ponto de acesso. Isso fornece uma experiência melhor para usuários sem fio e permite que eles se movam entre pontos de acesso sem precisar digitar novamente suas credenciais.
Devido à reconexão rápida e à segurança fornecida pelo PEAP-MS-CHAP v2, o PEAP-MS-CHAP v2 é uma opção lógica como um método de autenticação para conexões sem fio.

Para conexões VPN, o EAP-TLS é um método de autenticação baseado em certificado que fornece segurança forte que protege o tráfego de rede mesmo quando ele é transmitido pela Internet de computadores domésticos ou móveis para os servidores VPN da sua organização.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticação baseados em certificado

Os métodos de autenticação baseados em certificado têm a vantagem de fornecer forte segurança; e eles têm a desvantagem de serem mais difíceis de implantar do que os métodos de autenticação baseados em senha.

O PEAP-MS-CHAP v2 e o EAP-TLS são métodos de autenticação baseados em certificado, mas há muitas diferenças entre eles e a maneira como eles são implantados.

### <a name="eap-tls"></a>EAP-TLS

O EAP-TLS usa certificados para autenticação de cliente e de servidor e requer que você implante uma infraestrutura de chave pública (PKI) em sua organização. A implantação de uma PKI pode ser complexa e requer uma fase de planejamento que seja independente do planejamento para o uso do NPS como um servidor RADIUS.

Com o EAP-TLS, o NPS registra um certificado de servidor de uma autoridade de certificação \(CA @ no__t-1 e o certificado é salvo no computador local no repositório de certificados. Durante o processo de autenticação, a autenticação do servidor ocorre quando o NPS envia seu certificado de servidor ao cliente de acesso para provar sua identidade para o cliente de acesso. O cliente de acesso examina várias propriedades de certificado para determinar se o certificado é válido e é apropriado para uso durante a autenticação do servidor. Se o certificado do servidor atender aos requisitos mínimos de certificado do servidor e for emitido por uma AC que o cliente de acesso confia, o NPS será autenticado com êxito pelo cliente.

Da mesma forma, a autenticação de cliente ocorre durante o processo de autenticação quando o cliente envia seu certificado de cliente para o NPS para provar sua identidade para o NPS. O NPS examina o certificado e, se o certificado do cliente atender aos requisitos mínimos de certificado do cliente e for emitido por uma autoridade de certificação que o NPS confia, o cliente de acesso será autenticado com êxito pelo NPS.

Embora seja necessário que o certificado do servidor seja armazenado no repositório de certificados no NPS, o cliente ou o certificado do usuário podem ser armazenados no repositório de certificados no cliente ou em um cartão inteligente.

Para que esse processo de autenticação seja bem sucedido, é necessário que todos os computadores tenham o certificado de autoridade de certificação de sua organização no repositório de certificados das autoridades confiáveis para o computador local e o usuário atual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

O PEAP-MS-CHAP v2 usa um certificado para autenticação de servidor e credenciais baseadas em senha para autenticação de usuário. Como os certificados são usados apenas para autenticação de servidor, não é necessário implantar uma PKI para usar o PEAP-MS-CHAP v2. Ao implantar o PEAP-MS-CHAP v2, você pode obter um certificado de servidor para o NPS de uma das duas maneiras a seguir:

- Você pode instalar Active Directory serviços de certificados (AD CS) e, em seguida, registrar certificados automaticamente no NPSs. Se você usar esse método, também deverá registrar o certificado de autoridade de certificação nos computadores cliente que se conectam à sua rede para que eles confiem no certificado emitido para o NPS.

- Você pode comprar um certificado de servidor de uma CA pública, como a VeriSign. Se você usar esse método, certifique-se de selecionar uma autoridade de certificação que já seja confiável em computadores cliente. Para determinar se os computadores cliente confiam em uma autoridade de certificação, abra o snap-in do MMC (console de gerenciamento Microsoft) de certificados em um computador cliente e, em seguida, exiba o repositório de autoridades de certificação raiz confiáveis para o computador local e para o usuário atual. Se houver um certificado da autoridade de certificação nesses repositórios de certificados, o computador cliente confiará na autoridade de certificação e, portanto, confiará em qualquer certificado emitido pela autoridade de certificação.

Durante o processo de autenticação com PEAP-MS-CHAP v2, a autenticação do servidor ocorre quando o NPS envia seu certificado do servidor para o computador cliente. O cliente de acesso examina várias propriedades de certificado para determinar se o certificado é válido e é apropriado para uso durante a autenticação do servidor. Se o certificado do servidor atender aos requisitos mínimos de certificado do servidor e for emitido por uma AC que o cliente de acesso confia, o NPS será autenticado com êxito pelo cliente.

A autenticação de usuário ocorre quando um usuário tenta se conectar à rede digita credenciais baseadas em senha e tenta fazer logon. O NPS recebe as credenciais e executa a autenticação e a autorização. Se o usuário for autenticado e autorizado com êxito e se o computador cliente tiver autenticado com êxito o NPS, a solicitação de conexão será concedida.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para o uso de métodos de autenticação, você pode usar as etapas a seguir.

- Identifique os tipos de acesso à rede que você planeja oferecer, como sem fio, VPN, comutador com capacidade 802.1 X e acesso dial-up.

- Determine o método de autenticação ou os métodos que você deseja usar para cada tipo de acesso. É recomendável que você use os métodos de autenticação baseados em certificado que fornecem segurança forte; no entanto, talvez não seja prático implantar uma PKI, para que outros métodos de autenticação possam fornecer um equilíbrio melhor do que você precisa para sua rede.

- Se você estiver implantando o EAP-TLS, planeje sua implantação PKI. Isso inclui o planejamento dos modelos de certificado que você vai usar para certificados de servidor e certificados de computador cliente. Ele também inclui determinar como registrar certificados em computadores membro do domínio e não membros do domínio, e determinar se você deseja usar cartões inteligentes.

- Se você estiver implantando o PEAP-MS-CHAP v2, determine se deseja instalar o AD CS para emitir certificados de servidor para seu NPSs ou se deseja comprar certificados de servidor de uma AC pública, como a VeriSign.

### <a name="plan-network-policies"></a>Planejar políticas de rede

As políticas de rede são usadas pelo NPS para determinar se as solicitações de conexão recebidas de clientes RADIUS são autorizadas. O NPS também usa as propriedades de discagem da conta de usuário para fazer uma determinação de autorização.

Como as diretivas de rede são processadas na ordem em que aparecem no snap-in do NPS, planeje posicionar as políticas mais restritivas primeiro na lista de políticas. Para cada solicitação de conexão, o NPS tenta corresponder as condições da política com as propriedades de solicitação de conexão. O NPS examina cada política de rede na ordem até encontrar uma correspondência. Se não encontrar uma correspondência, a solicitação de conexão será rejeitada.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento de políticas de rede, você pode usar as etapas a seguir.

- Determine a ordem de processamento de NPS preferencial de políticas de rede, da mais restritiva para a menos restritiva.

- Determine o estado da política. O estado da política pode ter o valor de habilitado ou desabilitado. Se a política estiver habilitada, o NPS avaliará a política ao executar a autorização. Se a política não estiver habilitada, ela não será avaliada.

- Determine o tipo de política. Você deve determinar se a política foi criada para conceder acesso quando as condições da política forem correspondidas pela solicitação de conexão ou se a política for projetada para negar o acesso quando as condições da política forem correspondidas pela solicitação de conexão. Por exemplo, se você quiser negar explicitamente o acesso sem fio aos membros de um grupo do Windows, poderá criar uma política de rede que especifique o grupo, o método de conexão sem fio e que tenha uma configuração de tipo de política de negar acesso.

- Determine se você deseja que o NPS ignore as propriedades de discagem de contas de usuário que são membros do grupo no qual a política se baseia. Quando essa configuração não está habilitada, as propriedades de discagem das contas de usuário substituem as configurações definidas em políticas de rede. Por exemplo, se uma política de rede estiver configurada para conceder acesso a um usuário, mas as propriedades de discagem da conta de usuário para esse usuário estiverem definidas como negar acesso, o acesso ao usuário será negado. Mas se você habilitar a configuração de tipo de política ignorar Propriedades de discagem de conta de usuário, o mesmo usuário terá acesso à rede.

- Determine se a política usa a configuração de origem da política. Essa configuração permite que você especifique facilmente uma fonte para todas as solicitações de acesso. As fontes possíveis são um gateway de serviços de terminal (Gateway TS), um servidor de acesso remoto (VPN ou dial-up), um servidor DHCP, um ponto de acesso sem fio e um servidor de autoridade de registro de integridade. Como alternativa, você pode especificar uma fonte específica do fornecedor.

- Determine as condições que devem ser correspondidas para que a política de rede seja aplicada.

- Determine as configurações que serão aplicadas se as condições da política de rede forem correspondidas pela solicitação de conexão.

- Determine se você deseja usar, modificar ou excluir as políticas de rede padrão.

## <a name="plan-nps-accounting"></a>Planejar contabilidade do NPS

O NPS fornece a capacidade de registrar em log dados de estatísticas RADIUS, como autenticação de usuário e solicitações de contabilização, em três formatos: Formato do IAS, formato compatível com Banco de dados e log de Microsoft SQL Server. 

Formato do IAS e formato compatível com Banco de dados criar arquivos de log no NPS local no formato de arquivo de texto. 

SQL Server log fornece a capacidade de fazer logon em um banco de dados SQL Server 2000 ou SQL Server 2005 em conformidade com XML, estendendo a contabilidade RADIUS para aproveitar as vantagens do registro em log em um banco de dados relacional.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade do NPS, você pode usar as etapas a seguir.

- Determine se você deseja armazenar os dados de contabilidade do NPS em arquivos de log ou em um banco de dados SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilidade de NPS usando arquivos de log locais

A gravação de solicitações de estatísticas e autenticação de usuário em arquivos de log é usada principalmente para fins de análise e cobrança de conexão e também é útil como uma ferramenta de investigação de segurança, fornecendo um método para acompanhar a atividade de um usuário mal-intencionado após um invasão.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade do NPS usando arquivos de log locais, você pode usar as etapas a seguir.

- Determine o formato de arquivo de texto que você deseja usar para os arquivos de log do NPS.

- Escolha o tipo de informações que você deseja registrar em log. Você pode registrar em log solicitações de contabilização, solicitações de autenticação e status periódico.

- Determine o local do disco rígido onde você deseja armazenar os arquivos de log.

- Projete sua solução de backup de arquivo de log. O local do disco rígido onde você armazena os arquivos de log deve ser um local que permite que você faça backup dos seus dados com facilidade. Além disso, o local do disco rígido deve ser protegido Configurando a ACL (lista de controle de acesso) para a pasta em que os arquivos de log estão armazenados.

- Determine a frequência na qual você deseja que novos arquivos de log sejam criados. Se você quiser que os arquivos de log sejam criados com base no tamanho do arquivo, determine o tamanho máximo de arquivo permitido antes que um novo arquivo de log seja criado pelo NPS.

- Determine se você deseja que o NPS exclua arquivos de log mais antigos se o disco rígido ficar sem espaço de armazenamento.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de contabilidade e produzir relatórios.

### <a name="nps-sql-server-logging"></a>Log de SQL Server do NPS

O log de SQL Server do NPS é usado quando você precisa de informações de estado da sessão, para fins de criação de relatórios e análise de dados e para centralizar e simplificar o gerenciamento de seus dados contábeis.

O NPS fornece a capacidade de usar SQL Server registro em log para registrar a autenticação de usuário e as solicitações de contabilização recebidas de um ou mais servidores de acesso à rede para uma fonte de dados em um computador que executa o Microsoft SQL Server mecanismo de área de trabalho \(MSDE 2000 @ no__t-1 ou qualquer versão do SQL Server posterior a SQL Server 2000.

Os dados de contabilidade são passados do NPS em formato XML para um procedimento armazenado no banco de dados, que dá suporte à linguagem de consulta estruturada \(SQL @ no__t-1 e XML \(SQLXML @ no__t-3. A gravação de solicitações de estatísticas e autenticação de usuário em um banco de dados SQL Server em conformidade com XML permite que vários NPSs tenham uma fonte de dado.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade do NPS usando o log de SQL Server do NPS, você pode usar as etapas a seguir.

- Determine se você ou outro membro da sua organização tem SQL Server a experiência de desenvolvimento de banco de dados relacional 2000 ou SQL Server 2005 e você sabe como usar esses produtos para criar, modificar, administrar e gerenciar bancos de dados SQL Server.

- Determine se o SQL Server está instalado no NPS ou em um computador remoto.

- Crie o procedimento armazenado que você usará em seu banco de dados SQL Server para processar arquivos XML de entrada que contêm dados de contabilidade do NPS.

- Projete a estrutura e o fluxo de replicação de banco de dados SQL Server.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de contabilidade e produzir relatórios.

- Planeje usar os servidores de acesso à rede que enviam o atributo de classe em todas as solicitações de contabilidade. O atributo de classe é enviado para o cliente RADIUS em uma mensagem de aceitação de acesso e é útil para correlacionar mensagens de solicitação de contabilidade com sessões de autenticação. Se o atributo de classe for enviado pelo servidor de acesso à rede nas mensagens de solicitação de contabilização, ele poderá ser usado para corresponder os registros de estatísticas e de autenticação. A combinação dos atributos Unique-Serial-Number, Service-reboot-time e Server-address deve ser uma identificação exclusiva para cada autenticação que o servidor aceitar.

- Planeje usar os servidores de acesso à rede que dão suporte à contabilidade provisória.

- Planeje usar os servidores de acesso à rede que enviam mensagens de contabilidade e de contabilização.

- Planeje usar os servidores de acesso à rede que dão suporte ao armazenamento e encaminhamento de dados de contabilidade. Os servidores de acesso à rede que dão suporte a esse recurso podem armazenar dados contábeis quando o servidor de acesso à rede não pode se comunicar com o NPS. Quando o NPS está disponível, o servidor de acesso à rede encaminha os registros armazenados para o NPS, fornecendo maior confiabilidade na contabilidade de servidores de acesso à rede que não fornecem esse recurso.

- Planeje sempre configurar o atributo Acct-Interim-Interval em políticas de rede. O atributo Acct-Interim-Interval define o intervalo (em segundos) entre cada atualização provisória que o servidor de acesso à rede envia. De acordo com a RFC 2869, o valor do atributo Acct-Interim-Interval não deve ser menor que 60 segundos ou um minuto e não deve ser menor que 600 segundos ou 10 minutos. Para obter mais informações, consulte RFC 2869, "extensões RADIUS".

- Verifique se o log do status periódico está habilitado no seu NPSs.

