---
title: Planeje NPS como um servidor RADIUS
description: Este tópico fornece informações sobre a implantação de servidor RADIUS de servidor de política de rede planejamento no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9bb95428c17e5d7bde588693e84288ddfe64531
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-server"></a>Planeje NPS como um servidor RADIUS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando você implanta \(NPS\) servidor de política de rede como um servidor remoto Authentication Dial-In User Service (RADIUS), o NPS realiza autenticação, autorização e estatísticas para solicitações de conexão para o domínio local e para domínios que confiam no domínio local. Você pode usar essas diretrizes de planejamento para simplificar a implantação de RAIO.

Estas diretrizes de planejamento não incluem circunstâncias em que você deseja implantar o NPS como um proxy RADIUS. Quando você implanta o NPS como um proxy RADIUS, o NPS encaminha as solicitações de conexão para um servidor que executa NPS ou outros servidores RADIUS em domínios remotos, domínios não confiáveis ou ambos. 

Antes de implantar NPS como um servidor RADIUS em sua rede, use as seguintes diretrizes para planejar sua implantação.

- Planeje a configuração do servidor NPS.

- Planeje clientes RADIUS.

- Planeje o uso dos métodos de autenticação.

- Planeje políticas de rede.

- Planeje contabilidade NPS.

## <a name="plan-nps-server-configuration"></a>Planejar configuração do servidor NPS

Você deve decidir em quais domínio servidor NPS é um membro. Para ambientes de vários domínios, um servidor NPS pode autenticar credenciais de contas de usuário no domínio do qual ele é um membro e para todos os domínios que confiam no domínio local do servidor NPS. Para permitir que o servidor NPS para ler as propriedades de discagem rápida de contas de usuário durante o processo de autorização, você deve adicionar a conta de computador do servidor NPS ao grupo de servidores RAS e NPS para cada domínio.

Depois de ter determinado a participação no domínio do servidor NPS, o servidor deve ser configurado para se comunicar com os clientes RADIUS, também chamados de servidores de acesso à rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que registra NPS no caso de log e você pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para configuração do servidor NPS, você pode usar as etapas a seguir.

- Determine as portas RADIUS que o servidor NPS usa para receber mensagens de RAIO de clientes RADIUS. As portas padrão são portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e portas 1813 e 1646 para mensagens de estatísticas RADIUS.

- Se o servidor NPS está configurado com vários adaptadores de rede, determine os adaptadores por meio da qual você deseja que o tráfego de RAIO seja permitido.

- Determine os tipos de eventos que você deseja NPS para gravar no Log de eventos. Você pode fazer solicitações de autenticação rejeitadas, solicitações de autenticação bem-sucedida ou ambos os tipos de solicitações.

- Determine se você estiver implantando mais de um servidor NPS. Para fornecer tolerância para autenticação baseada em RADIUS e estatísticas, use pelo menos dois servidores NPS. Um servidor NPS é usado como o servidor RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS é configurado, em seguida, em ambos os servidores NPS. Se o servidor NPS principal ficar indisponível, clientes RADIUS enviam mensagens de solicitação de acesso para o servidor NPS alternativo.

- Planeje o script usado para copiar uma configuração do servidor NPS para outros servidores NPS para salvar na sobrecarga administrativa e impedir que o cofiguration incorreta de um servidor. NPS fornece os comandos Netsh que permitem que você pode copiar todo ou parte de uma configuração do servidor NPS para importação em outro servidor NPS. Você pode executar os comandos manualmente no prompt do Netsh. No entanto, se você salvar sua sequência de comando como um script, você pode executar o script em uma data posterior se você decidir alterar suas configurações de servidor.

## <a name="plan-radius-clients"></a>Planeje clientes RADIUS

Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, servidores de rede virtual privada (VPN), 802.1 comutadores compatíveis com X e servidores dial-up. Proxies RADIUS, o qual conexão encaminhar mensagens de solicitação para servidores RADIUS, também são clientes RADIUS. NPS dá suporte a todos os servidores de acesso de rede e proxies RADIUS compatíveis com o RAIO de protocolo conforme descrito em RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)" e RFC 2866, "RADIUS Accounting".

>[!IMPORTANT]
>Clientes de acesso, como computadores cliente, não são clientes RADIUS. Somente os servidores de acesso à rede e servidores proxy que suportam o protocolo RADIUS são clientes RADIUS.

Além disso, os pontos de acesso sem fio e comutadores devem ser capazes de autenticação 802.1 X. Se você deseja implantar Extensible Authentication Protocol \(EAP\) ou \(PEAP\) Protected Extensible Authentication Protocol, pontos de acesso e comutadores devem suportar o uso de EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação baseada em PPP adicionais, como PEAP, até que você testou aqueles que você pretende usar para acesso à rede.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específica do fornecedor (VSAs), que você deve configurar no NPS. Se os servidores de acesso de rede exigem VSAs, registra as informações de VSA para uso posterior quando você configurar as políticas de rede no NPS. 

- Documente os endereços IP dos clientes RADIUS e o servidor NPS para simplificar a configuração de todos os dispositivos. Quando você implanta os clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP do servidor NPS inserido como o servidor de autenticação. E quando você configura o NPS para se comunicar com os clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in NPS.

- Crie segredos compartilhados para configuração nos clientes RADIUS e o snap-in NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou a senha, que você também entrará no snap-in NPS ao configurar clientes RADIUS de NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planeje o uso dos métodos de autenticação

NPS dá suporte a ambos os métodos de autenticação baseada em senha e baseada em certificado. Entretanto, nem todos os servidores de acesso à rede suportam os mesmos métodos de autenticação. Em alguns casos, convém implantar um método de autenticação diferentes com base no tipo de acesso à rede.

Por exemplo, você talvez queira implantar o acesso VPN para sua organização e acesso sem fio, mas use um método de autenticação diferentes para cada tipo de acesso: EAP-TLS para conexões VPN, devido a segurança forte que fornece EAP com Transport Layer Security (EAP-TLS) e PEAP-MS-CHAP v2 para conexões sem fio 802.1 X.

Reconexão PEAP com Microsoft Challenge Handshake Authentication Protocol versão 2 (PEAP-MS-CHAP v2) fornece um recurso chamado rápido que é projetado especificamente para uso com computadores portáteis e outros dispositivos sem fio. Reconexão rápida permite que os clientes sem fio movam entre os pontos de acesso sem fio na mesma rede sem ser autenticado novamente cada vez que você se associar um novo ponto de acesso. Isso proporciona uma melhor experiência para os usuários sem fio e permite que eles movam entre os pontos de acesso sem precisar digitar novamente suas credenciais.
Devido a fast Reconecte e a segurança que fornece PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 é uma opção lógica como um método de autenticação para conexões sem fio.

Para conexões VPN, EAP-TLS é um método de autenticação baseada em certificado que fornece segurança forte que protege o tráfego de rede, mesmo quando os que são transmitidos pela Internet de computadores domésticos ou móveis para os servidores VPN da organização.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticação baseada em certificado

Métodos de autenticação baseada em certificado têm a vantagem de fornecimento de alta segurança; e eles têm a desvantagem de ser mais difícil implantar o que os métodos de autenticação baseada em senha.

PEAP-MS-CHAP v2 e EAP-TLS são métodos de autenticação baseada em certificado, mas existem muitas diferenças entre eles e a maneira na qual eles são implantados.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa certificados para autenticação de cliente e servidor e requer que você implante uma infraestrutura de chave pública (PKI) em sua organização. Implantação de uma PKI podem ser complexo e requer uma fase de planejamento é independente de planejamento para o uso do NPS como um servidor RADIUS.

Com EAP-TLS, o servidor NPS registra-se um certificado de servidor de uma autoridade de certificação \(CA\) e o certificado é salvo no computador local no repositório de certificados. Durante o processo de autenticação, autenticação de servidor ocorre quando o servidor NPS envia seu certificado do servidor para o cliente de acesso para provar sua identidade para o cliente de acesso. O cliente de acesso examina várias propriedades de certificado para determinar se o certificado é válido e é adequado para uso durante a autenticação de servidor. Se o certificado do servidor atenda aos requisitos de certificado do servidor mínimo e é emitido por uma autoridade de certificação que o cliente de acesso confie, o servidor NPS for autenticado com êxito pelo cliente.

Da mesma forma, autenticação de cliente ocorre durante o processo de autenticação quando o cliente envia seu certificado de cliente para o servidor NPS para provar sua identidade para o servidor NPS. O servidor NPS examina o certificado e se o certificado cliente atende aos requisitos de certificado de cliente mínimo e é emitido por uma autoridade de certificação que o servidor NPS confie, o cliente de acesso for autenticado com êxito pelo servidor NPS.

Embora seja necessário que o certificado do servidor é armazenado no repositório de certificados no servidor NPS, o certificado de cliente ou usuário pode ser armazenado em um repositório de certificados no cliente ou em um cartão inteligente.

Para esse processo de autenticação seja bem-sucedida, é necessário que todos os computadores têm o certificado da CA da sua organização no repositório de certificados de autoridades de certificação confiáveis para o computador Local e o usuário atual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 usa um certificado de autenticação de servidor e credenciais baseadas em senha para autenticação do usuário. Como certificados são usados apenas para autenticação de servidor, você não é necessária para implantar uma PKI para usar PEAP-MS-CHAP v2. Quando você implanta PEAP-MS-CHAP v2, você poderá obter um certificado de servidor para o servidor NPS em uma das seguintes maneiras:

- Você pode instalar os serviços de certificados do Active Directory (AD CS) e, em seguida, registre automaticamente certificados para servidores NPS. Se você usar esse método, você também deve registrar o certificado de autoridade de certificação para computadores clientes conectados à sua rede para que eles confiam no certificado emitido para o servidor NPS.

- Você pode adquirir um certificado de servidor de uma autoridade de certificação pública, como VeriSign. Se você usar esse método, certifique-se de que você selecionar uma autoridade de certificação que já é confiável por computadores cliente. Para determinar se os computadores cliente confiar em uma autoridade de certificação, abra o snap-in do Console de gerenciamento Microsoft (MMC) de certificados em um computador cliente e, em seguida, exibir o armazenamento de autoridades de certificação confiáveis para o computador Local e para o usuário atual. Se houver um certificado da CA nesses repositórios de certificados, o computador cliente confia a autoridade de certificação e, portanto, confiarão qualquer certificado emitido pela autoridade de certificação.

Durante o processo de autenticação com PEAP-MS-CHAP v2, autenticação de servidor ocorre quando o servidor NPS envia seu certificado do servidor para o computador cliente. O cliente de acesso examina várias propriedades de certificado para determinar se o certificado é válido e é adequado para uso durante a autenticação de servidor. Se o certificado do servidor atenda aos requisitos de certificado do servidor mínimo e é emitido por uma autoridade de certificação que o cliente de acesso confie, o servidor NPS for autenticado com êxito pelo cliente.

Autenticação do usuário ocorre quando um usuário tentar se conectar às credenciais rede tipos baseada em senha e tenta fazer logon. NPS recebe as credenciais e realiza a autenticação e autorização. Se o usuário é autenticado e autorizado com êxito e se o computador cliente autenticado com êxito o servidor NPS, é concedida a solicitação de conexão.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para o uso dos métodos de autenticação, você pode usar as etapas a seguir.

- Identifica os tipos de acesso à rede que você planeja oferecer, por exemplo, sem fio, VPN, 802.1 switch compatível com X e acesso dial-up.

- Determine o método de autenticação ou métodos que você deseja usar para cada tipo de acesso. É recomendável que você use os métodos de autenticação baseada em certificado que fornecem forte segurança; No entanto, talvez não seja prático para implantar uma PKI, para que outros métodos de autenticação podem fornecer um melhor equilíbrio entre o que você precisa para sua rede.

- Se você estiver implantando EAP-TLS, planeje a implantação de PKI. Isso inclui o planejamento os modelos de certificado que você pretende usar para certificados de servidor e certificados de computador cliente. Ele também inclui determinar como certificados de membro do domínio e computadores membro do domínio não e determinar se você deseja usar cartões inteligentes.

- Se você estiver implantando PEAP-MS-CHAP v2, determine se você deseja instalar o AD CS para emitir certificados de servidor para seus servidores NPS ou se você deseja comprar certificados de servidor de uma autoridade de certificação pública, como VeriSign.

### <a name="plan-network-policies"></a>Plano de políticas de rede

Políticas de rede são usadas pelo NPS para determinar se as solicitações de conexão recebidas de clientes RADIUS são autorizadas. NPS também usa as propriedades de discagem rápida da conta de usuário para tomar uma decisão de autorização.

Como políticas de rede são processadas na ordem em que aparecem no snap-in de NPS, planeje colocar as políticas mais restritivas primeiro na lista de políticas. Para cada solicitação de conexão, o NPS tenta comparar as condições da política com as propriedades de solicitação de conexão. NPS examina cada política de rede em ordem até encontrar uma correspondência. Se ele não encontra uma correspondência, a solicitação de conexão será rejeitada.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para políticas de rede, você pode usar as etapas a seguir.

- Determine a ordem de processamento de NPS preferencial de políticas de rede, da mais restritiva para menos restritivas.

- Determine o estado de política. O estado de política pode ter o valor de habilitado ou desabilitado. Se a política estiver habilitada, o NPS avalia a política durante a execução de autorização. Se a política não estiver habilitada, ele não será avaliado.

- Determine o tipo de política. Você deve determinar se a política destina-se para conceder acesso quando as condições da política são atendidas, a solicitação de conexão ou se a política é projetada para negar acesso quando as condições da política são atendidas pela solicitação de conexão. Por exemplo, se você quiser explicitamente negar acesso sem fio para os membros de um grupo do Windows, você pode criar uma política de rede que especifica o grupo, o método de conexão sem fio e que tem uma política digite configuração de negar acesso.

- Determine se você deseja NPS para ignorar as propriedades de discagem rápida de contas de usuário que são membros do grupo no qual baseia-se a política. Quando essa configuração não está habilitada, as propriedades de discagem rápida de contas de usuário substituem as configurações que são definidas em políticas de rede. Por exemplo, se uma política de rede está configurada que concede acesso a um usuário, mas as propriedades de discagem da conta de usuário para esse usuário são definidas para negar o acesso, o usuário acesso será negado. Mas se você habilitar as propriedades de configuração dial-in de conta de usuário Ignorar do tipo de política, o mesmo usuário recebe acesso à rede.

- Determine se a política usa a configuração de política de origem. Esta configuração permite especificar facilmente uma fonte para todas as solicitações de acesso. Possíveis fontes são um Gateway de serviços de Terminal (Gateway TS), um servidor de acesso remoto (dial-up ou VPN), um servidor DHCP, um ponto de acesso sem fio e um servidor de autoridade de registro de saúde. Como alternativa, você pode especificar uma origem específica do fornecedor.

- Determine as condições que devem ser atendidas para que a política de rede sejam aplicadas.

- Determine as configurações que são aplicadas se as condições da diretiva de rede correspondem à solicitação de conexão.

- Determine se você deseja usar, modificar ou excluir as políticas de rede padrão.

## <a name="plan-nps-accounting"></a>Planejar Contabilização de NPS

NPS fornece a capacidade de registrar os dados de contabilidade RADIUS, como autenticação do usuário e solicitações de contabilidade, em três formatos: formato IAS, formato compatível com o banco de dados e registro em log do Microsoft SQL Server. 

Formato IAS e formato compatível com o banco de dados criar arquivos de log no servidor NPS local no formato de arquivo de texto. 

Log do SQL Server fornece a capacidade de fazer logon para um banco de dados compatíveis com o SQL Server 2005 XML, estendendo estatísticas RADIUS para aproveitar as vantagens de registro em log para um banco de dados relacional ou SQL Server 2000.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento de contabilização de NPS, você pode usar as etapas a seguir.

- Determine se você deseja armazenar os dados de contabilização de NPS em arquivos de log ou em um banco de dados do SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilização de NPS usando arquivos de log local

Solicitações de autenticação e estatísticas de usuário de gravação nos arquivos de log é usado principalmente para fins de faturamento e análise de conexão e também é útil como uma ferramenta de investigação de segurança, proporcionando-lhe um método para controlar a atividade de um usuário mal-intencionado após um ataque.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento de contabilização de NPS usando arquivos de log local, você pode usar as etapas a seguir.

- Determine o formato de arquivo de texto que você deseja usar para seus arquivos de log do NPS.

- Escolha o tipo de informação que você deseja fazer logon. Você pode fazer solicitações de contabilidade, solicitações de autenticação e status periódico.

- Determine o local do disco rígido onde deseja armazenar seus arquivos de log.

- Projete sua solução de backup de arquivos de log. O local do disco rígido em que você armazena seus arquivos de log deve ser um local que permite que você facilmente faça backup dos seus dados. Além disso, o local do disco rígido deve ser protegido, configurando a lista de controle de acesso (ACL) para a pasta onde os arquivos de log estão armazenados.

- Determine a frequência na qual você deseja novos arquivos de log seja criado. Se você quiser que os arquivos de log seja criado com base no tamanho do arquivo, determine o tamanho máximo de arquivo permitido antes que um novo arquivo de log é criado pelo NPS.

- Determine se você deseja NPS para excluir arquivos de log antigos se o disco rígido ficar sem espaço de armazenamento.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de contabilidade e geram relatórios.

### <a name="nps-sql-server-logging"></a>Registro em log NPS SQL Server

Registro em log NPS SQL Server é usado quando você precisa de informações de estado de sessão, para fins de análise de dados e criação de relatórios, bem como para centralizar e simplificar o gerenciamento de seus dados de contabilidade.

NPS fornece a capacidade de usar o SQL Server log para autenticação do usuário de registro e estatísticas pedidos recebidos de um ou mais servidores de acesso de rede para uma fonte de dados em um computador executando qualquer versão do SQL Server ou \(MSDE 2000\) o mecanismo de área de trabalho do Microsoft SQL Server posteriores ao SQL Server 2000.

Contabilidade dados forem repassados do NPS em formato XML para um procedimento armazenado no banco de dados, que oferece suporte a ambas as linguagem de consulta estruturada \(SQL\) e \(SQLXML\) XML. Gravação de autenticação do usuário e solicitações em um banco de dados do SQL Server compatível com XML de estatísticas permite que vários servidores NPS ter uma fonte de dados.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento de contabilização de NPS usando o log de NPS SQL Server, você pode usar as etapas a seguir.

- Determine se você ou outro membro da sua organização tem o desenvolvimento de banco de dados relacionais do SQL Server 2000 ou SQL Server 2005 a experiência e entender como usar esses produtos para criar, modificar, administrar e gerenciar bancos de dados do SQL Server.

- Determine se o SQL Server é instalado no servidor NPS ou em um computador remoto.

- Crie o procedimento armazenado que você usará em seu banco de dados do SQL Server para processar entrados arquivos XML que contêm dados de contabilização de NPS.

- Crie a estrutura de replicação de banco de dados do SQL Server e o fluxo.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de contabilidade e geram relatórios.

- Você pretende usar servidores de acesso à rede que envia o atributo Class em todas as solicitações de contabilidade. O atributo Class é enviado para o cliente RADIUS em uma mensagem de aceitação de acesso e é útil para correlacionar mensagens de solicitação de contabilização com sessões de autenticação. Se o atributo Class for enviado pelo servidor de acesso de rede nas mensagens de solicitação de contabilização, ele pode ser usado para corresponder os registros de estatísticas e autenticação. A combinação dos atributos exclusivos-número de série, Service-Reboot-Time e endereço do servidor deve ser uma identificação exclusiva para cada autenticação que o servidor aceita.

- Você pretende usar servidores de acesso à rede que dão suporte a estatísticas provisórias.

- Você pretende usar servidores de acesso à rede que enviam mensagens de ativação e desativação.

- Você pretende usar servidores de acesso à rede que dão suporte ao armazenamento e transferência de dados de contabilidade. Servidores de acesso à rede que dão suporte a esse recurso podem armazenar dados de contabilidade quando o servidor de acesso de rede não poderão se comunicar com o servidor NPS. Quando o servidor NPS estiver disponível, o servidor de acesso de rede encaminha os registros armazenados no servidor NPS, fornecendo maior confiabilidade em estatísticas sobre servidores de acesso à rede que não fornecem esse recurso.

- Planeje sempre configurar o atributo de conta-ínterim-Interval em políticas de rede. O atributo de conta-ínterim-Interval define o intervalo (em segundos) entre cada atualização que envia o servidor de acesso de rede. Acordo com RFC 2869, o valor do atributo conta-ínterim-Interval não deve ser menor que 60 segundos ou um minuto e não deve ser menor que 600 segundos ou 10 minutos. Para obter mais informações, consulte RFC 2869, "RADIUS extensões".

- Certifique-se de que o log de status periódico seja habilitado em seus servidores NPS.

