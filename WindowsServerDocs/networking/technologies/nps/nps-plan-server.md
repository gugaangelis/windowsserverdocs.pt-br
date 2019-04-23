---
title: Planejar NPS como um servidor RADIUS
description: Este tópico fornece informações sobre a implantação de servidor RADIUS do servidor de diretiva de rede no Windows Server 2016 de planejamento.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5fd89ef8d95735b8cbe1334ba51ed0059595dbc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839167"
---
# <a name="plan-nps-as-a-radius-server"></a>Planejar NPS como um servidor RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando você implanta o servidor de políticas de rede \(NPS\) como um servidor RADIUS Remote Authentication Dial-In usuário Service (), o NPS executa autenticação, autorização e contabilização para solicitações de conexão para o domínio local e para domínios que confiam no domínio local. Você pode usar essa diretrizes de planejamento para simplificar a implantação do RADIUS.

Essa diretrizes de planejamento não incluem as circunstâncias em que você deseja implantar o NPS como um proxy RADIUS. Quando você implanta o NPS como um proxy RADIUS, o NPS encaminha as solicitações de conexão para um servidor que executa o NPS ou outros servidores RADIUS em domínios remotos, / ou domínios não confiáveis. 

Antes de implantar o NPS como servidor RADIUS em sua rede, use as seguintes diretrizes para planejar a implantação.

- Planeje a configuração do NPS.

- Plano de clientes RADIUS.

- Planeje o uso de métodos de autenticação.

- Planeje as políticas de rede.

- Planeje a contabilidade NPS.

## <a name="plan-nps-configuration"></a>Planejar a configuração do NPS

Você deve decidir em qual domínio o NPS é um membro. Para ambientes de vários domínios, um NPS pode autenticar credenciais de contas de usuário de domínio do qual ele é um membro e para todos os domínios que confiam no domínio local do NPS. Para permitir que o NPS ler as propriedades de discagem das contas de usuário durante o processo de autorização, você deve adicionar a conta de computador do NPS para o grupo de RAS e NPSs para cada domínio.

Depois de determinar a associação de domínio do NPS, o servidor deve ser configurado para se comunicar com clientes RADIUS, também chamados de servidores de acesso de rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que registra o NPS no evento de log e você pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a configuração do NPS, você pode usar as etapas a seguir.

- Determine as portas RADIUS que o NPS usa para receber mensagens RADIUS dos clientes RADIUS. As portas padrão são as portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e as portas 1813 e 1646 para mensagens de contabilização RADIUS.

- Se o NPS é configurado com vários adaptadores de rede, determine os adaptadores sobre o qual você deseja que o tráfego RADIUS seja permitido.

- Determine os tipos de eventos que você deseja que o NPS para gravar no Log de eventos. Você pode registrar solicitações de autenticação rejeitados, solicitações de autenticação bem-sucedidas ou ambos os tipos de solicitações.

- Determine se você estiver implantando mais de um NPS. Para fornecer tolerância a falhas para contabilização e autenticação RADIUS, use pelo menos dois NPSs. Um NPS é usado como o servidor RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS está configurado, em seguida, em ambos os NPSs. Se o NPS primário ficar indisponível, os clientes RADIUS, em seguida, enviam mensagens de solicitação de acesso ao NPS alternativo.

- Planeje o script usado para copiar uma configuração do NPS para outros NPSs para economizar em sobrecarga administrativa e evitar a configuração incorreta de um servidor. NPS fornece os comandos Netsh que permitem copiar todo ou parte de uma configuração do NPS para importação em outro NPS. Você pode executar manualmente os comandos no prompt do Netsh. No entanto, se você salvar sua sequência de comando como um script, você pode executar o script em uma data posterior se você decidir alterar suas configurações de servidor.

## <a name="plan-radius-clients"></a>Planejar clientes RADIUS

Os clientes RADIUS são servidores de acesso de rede, como pontos de acesso sem fio, servidores de rede virtual privada (VPN), 802.1X comutadores compatíveis e servidores dial-up. Os proxies RADIUS que encaminham mensagens de solicitação em servidores RADIUS de conexão, também são clientes RADIUS. O NPS dá suporte a todos os servidores de acesso de rede e proxies RADIUS que estão em conformidade com o RAIO de protocolo conforme descrito no RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)," e RFC 2866, "Contabilidade do RADIUS".

>[!IMPORTANT]
>Clientes de acesso, como computadores cliente, não são clientes RADIUS. Somente os servidores de acesso de rede e servidores proxy que suportam o protocolo RADIUS são clientes RADIUS.

Além disso, os pontos de acesso sem fio e comutadores devem ser capazes de autenticação 802.1 X. Se você quiser implantar Extensible Authentication Protocol \(EAP\) ou Protected Extensible Authentication Protocol \(PEAP\), comutadores e pontos de acesso devem dar suporte o uso de EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação adicionais baseadas em PPP, como PEAP, até que você tenha testado aqueles que você pretende usar para acesso à rede.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específicos do fornecedor (VSAs), que você deve configurar no NPS. Se seus servidores de acesso de rede exigem VSAs, as VSA informações de log para uso posterior quando você configurar suas políticas de rede no NPS. 

- Documente os endereços IP de clientes RADIUS e o NPS para simplificar a configuração de todos os dispositivos. Quando você implanta os clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP de NPS inserido como o servidor de autenticação. E, quando você configura o NPS para se comunicar com os clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in do NPS.

- Crie os segredos compartilhados para configuração nos clientes RADIUS e o snap-in NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou senha, o que você também vai inserir no snap-in NPS ao configurar clientes RADIUS no NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planejar o uso de métodos de autenticação

O NPS dá suporte a ambos os métodos de autenticação baseada em certificado e baseada em senha. No entanto, nem todos os servidores de acesso de rede dão suporte os mesmos métodos de autenticação. Em alguns casos, você talvez queira implantar um método de autenticação diferente com base no tipo de acesso à rede.

Por exemplo, você talvez queira implantar o acesso VPN para sua organização e sem fio, mas usar um método de autenticação diferentes para cada tipo de acesso: EAP-TLS para conexões VPN, devido à alta segurança em que fornece EAP com Transport Layer Security (EAP-TLS) e o PEAP-MS-CHAP v2 para conexões sem fio 802.1 X.

PEAP com Microsoft Challenge Handshake Authentication Protocol versão 2 (PEAP-MS-CHAP v2) fornece um recurso chamado fast reconectar-se que é projetado especificamente para uso com computadores portáteis e outros dispositivos sem fio. A reconexão rápida permite que clientes sem fio para se mover entre pontos de acesso sem fio na mesma rede sem serem autenticados novamente cada vez que se associam a um novo ponto de acesso. Isso proporciona uma melhor experiência para usuários sem fio e lhes permite mover entre os pontos de acesso sem ter que digitar novamente suas credenciais.
Por causa de rápida reconexão e a segurança que fornece o PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 é uma escolha lógica como um método de autenticação para conexões sem fio.

Para conexões VPN, o EAP-TLS é um método de autenticação baseada em certificado que fornece alta segurança que protege o tráfego de rede, até mesmo quando eles são transmitidos entre a Internet a partir de computadores domésticos ou móveis aos seus servidores VPN de organização.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticação baseada em certificado

Métodos de autenticação baseada em certificado tem a vantagem de fornecer segurança forte; e eles têm a desvantagem de ser mais difíceis de implantar que os métodos de autenticação baseada em senha.

PEAP-MS-CHAP v2 e EAP-TLS são métodos de autenticação baseada em certificado, mas há muitas diferenças entre eles e a maneira na qual eles são implantados.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa certificados para autenticação de cliente e servidor e requer que você implante uma infraestrutura de chave pública (PKI) em sua organização. Implantar uma PKI pode ser complexo e requer uma fase de planejamento que é independente de planejamento para o uso de NPS como servidor RADIUS.

Com o EAP-TLS, o NPS registra um certificado do servidor de uma autoridade de certificação \(autoridade de certificação\), e o certificado é salvo no computador local no repositório de certificados. Durante o processo de autenticação, autenticação do servidor ocorre quando o NPS envia seu certificado de servidor para o cliente de acesso para provar sua identidade para o cliente de acesso. O cliente de acesso examina várias propriedades do certificado para determinar se o certificado é válido e é adequado para uso durante a autenticação de servidor. Se o certificado do servidor atende aos requisitos de certificado de servidor mínima e é emitido por uma CA de confiança do cliente de acesso, o NPS é autenticado com êxito pelo cliente.

Da mesma forma, autenticação de cliente ocorre durante o processo de autenticação quando o cliente envia seu certificado de cliente para o NPS para comprovar sua identidade para o NPS. O NPS examina o certificado e se o certificado do cliente atende aos requisitos de certificado de cliente mínimo e é emitido por uma autoridade de certificação que o NPS confia, o cliente de acesso for autenticado com êxito, o NPS.

Embora seja necessário que o certificado do servidor é armazenado no repositório de certificados no NPS, o certificado de cliente ou usuário pode ser armazenado em qualquer repositório de certificados no cliente ou em um cartão inteligente.

Para esse processo de autenticação seja bem-sucedida, é necessário que todos os computadores têm o certificado de autoridade de certificação da sua organização no repositório de certificados de autoridades de certificação raiz confiáveis para o computador Local e o usuário atual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 usa um certificado para autenticação de servidor e credenciais baseadas em senha para autenticação do usuário. Como os certificados são usados apenas para autenticação de servidor, não é necessário para implantar uma PKI para usar o PEAP-MS-CHAP v2. Quando você implantar PEAP-MS-CHAP v2, você pode obter um certificado de servidor para o NPS em uma das duas maneiras a seguir:

- Você pode instalar os serviços de certificados do Active Directory (AD CS) e, em seguida, certificados de registro automático para NPSs. Se você usar esse método, você também deve registrar o certificado de autoridade de certificação para computadores cliente se conectam à sua rede para que eles confiam no certificado emitido para o NPS.

- Você pode comprar um certificado do servidor de uma autoridade de certificação pública, como a VeriSign. Se você usar esse método, certifique-se de que você selecionar uma autoridade de certificação que já é confiável por computadores cliente. Para determinar se os computadores cliente confiar uma autoridade de certificação, abra o snap-in de certificados Microsoft Management Console (MMC) em um computador cliente e, em seguida, exibir o armazenamento de autoridades de certificação raiz confiáveis do computador Local e para o usuário atual. Se houver um certificado de autoridade de certificação nesses repositórios de certificado, o computador cliente confie na AC e, portanto, confiarão qualquer certificado emitido pela autoridade de certificação.

Durante o processo de autenticação com PEAP-MS-CHAP v2, a autenticação do servidor ocorre quando o NPS envia seu certificado de servidor para o computador cliente. O cliente de acesso examina várias propriedades do certificado para determinar se o certificado é válido e é adequado para uso durante a autenticação de servidor. Se o certificado do servidor atende aos requisitos de certificado de servidor mínima e é emitido por uma CA de confiança do cliente de acesso, o NPS é autenticado com êxito pelo cliente.

Autenticação do usuário ocorre quando um usuário tentar se conectar às credenciais de baseado em senha de tipos de rede e tenta fazer logon. O NPS recebe as credenciais e executa autenticação e autorização. Se o usuário é autenticado e autorizado com êxito e se o computador cliente autenticado com êxito o NPS, a solicitação de conexão é concedida.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para o uso de métodos de autenticação, você pode usar as etapas a seguir.

- Identifique os tipos de acesso à rede que você planeja oferecer, como sem fio, VPN, 802.1 comutador compatível com X e acesso dial-up.

- Determine o método de autenticação ou métodos que você deseja usar para cada tipo de acesso. É recomendável que você use os métodos de autenticação baseada em certificado que fornecem segurança forte; No entanto, ele pode não ser prático para você implantar uma PKI, portanto, outros métodos de autenticação podem fornecer um melhor equilíbrio entre o que você precisa para sua rede.

- Se você estiver implantando o EAP-TLS, planeje a implantação de PKI. Isso inclui o planejamento de modelos de certificado que você pretende usar para certificados de servidor e certificados de computador cliente. Ele também inclui a determinação de como registrar certificados para o membro do domínio e computadores não associados ao domínio e determinar se você deseja usar cartões inteligentes.

- Se você estiver implantando o PEAP-MS-CHAP v2, determine se você deseja instalar o AD CS para emitir certificados de servidor para seu NPSs ou se você deseja comprar certificados de servidor de uma autoridade de certificação pública, como a VeriSign.

### <a name="plan-network-policies"></a>Planejar as políticas de rede

Políticas de rede são usadas pelo NPS para determinar se as solicitações de conexão recebidas dos clientes RADIUS são autorizadas. O NPS também usa as propriedades de discagem da conta de usuário para tomar uma decisão de autorização.

Como as diretivas de rede são processadas na ordem em que aparecem no snap-in do NPS, planeje para colocar suas políticas mais restritivas primeiro na lista de políticas. Para cada solicitação de conexão, o NPS tenta comparar as condições da política com as propriedades de solicitação de conexão. NPS examina cada diretiva de rede na ordem até que ele encontra uma correspondência. Se não encontrar uma correspondência, a solicitação de conexão é rejeitada.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para políticas de rede, você pode usar as etapas a seguir.

- Determine a ordem de processamento de NPS preferencial de diretivas de rede, do mais restritivo para o menos restritivo.

- Determine o estado da política. O estado da política pode ter o valor habilitado ou desabilitado. Se a política estiver habilitada, o NPS avalia a política durante a execução de autorização. Se a diretiva não estiver habilitada, ele não será avaliado.

- Determine o tipo de política. Você deve determinar se a política é projetada para conceder acesso quando as condições da política são correspondidas pela solicitação de conexão ou se a política foi projetada para negar o acesso quando as condições da política são atendidas pela solicitação de conexão. Por exemplo, se você quiser negar explicitamente o acesso sem fio para os membros de um grupo do Windows, você pode criar uma política de rede que especifica o grupo, o método de conexão sem fio e que tem uma política de configuração de negar acesso de tipo.

- Determine se você deseja que o NPS para ignorar as propriedades de discagem das contas de usuário que são membros do grupo no qual a política se baseia. Quando essa configuração não está habilitada, as propriedades de discagem das contas de usuário substituem as configurações que são configuradas nas políticas de rede. Por exemplo, se uma política de rede é configurada que concede acesso a um usuário, mas as propriedades de discagem da conta de usuário para esse usuário são definidas para negar acesso, o usuário tem acesso negado. Mas se você habilitar as propriedades de configuração dial-in de conta de usuário Ignorar do tipo de política, o mesmo usuário tenha acesso à rede.

- Determine se a política usa a configuração de origem da política. Essa configuração permite que você especifique facilmente uma código-fonte para todas as solicitações de acesso. Possíveis origens são um Gateway de serviços de Terminal (TS Gateway), um servidor de acesso remoto (VPN ou dial-up), um servidor DHCP, um ponto de acesso sem fio e um servidor de autoridade de registro de integridade. Como alternativa, você pode especificar uma fonte específicas do fornecedor.

- Determine as condições que devem ser atendidas para que a política de rede a ser aplicado.

- Determine as configurações que são aplicadas se as condições da diretiva de rede forem atendidas pela solicitação de conexão.

- Determine se você deseja usar, modificar ou excluir as políticas de rede padrão.

## <a name="plan-nps-accounting"></a>Planejar a contabilidade NPS

NPS fornece a capacidade de registrar os dados de contabilidade do RADIUS, como autenticação de usuário e as solicitações de contabilidade, em três formatos: Formato do IAS, formato compatível com o banco de dados e log do Microsoft SQL Server. 

Formato de IAS e formato compatível com o banco de dados criam arquivos de log no NPS local no formato de arquivo de texto. 

Log do SQL Server fornece a capacidade de fazer em um SQL Server 2000 ou o banco de dados em conformidade com o SQL Server 2005 XML, estendendo a contabilidade do RADIUS para aproveitar as vantagens do registro em log para um banco de dados relacional.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade NPS, você pode usar as etapas a seguir.

- Determine se você deseja armazenar os dados de estatísticas do NPS em arquivos de log ou em um banco de dados do SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilidade NPS usando arquivos de log local

Solicitações em arquivos de log de contabilização e autenticação de usuário de gravação é usado principalmente para fins de análise e a cobrança de conexão e também é útil como ferramenta de investigação de segurança, fornecendo a você um método para rastrear a atividade de um usuário mal-intencionado depois de um ataque.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade NPS usando arquivos de log local, você pode usar as etapas a seguir.

- Determine o formato de arquivo de texto que você deseja usar para seus arquivos de log do NPS.

- Escolha o tipo de informação que você deseja registrar. Você pode registrar as solicitações de contabilidade, as solicitações de autenticação e status periódico.

- Determine o local de disco rígido em que você deseja armazenar seus arquivos de log.

- Projetar sua solução de backup de arquivo de log. O local de disco rígido onde você armazena seus arquivos de log deve ser um local que permite que você facilmente fazer backup dos dados. Além disso, o local do disco rígido deve ser protegido, configurando a lista de controle de acesso (ACL) para a pasta onde os arquivos de log são armazenados.

- Determine a frequência em que deseja que os novos arquivos de log a ser criado. Se desejar que os arquivos de log a ser criado com base no tamanho de arquivo, determine o tamanho máximo permitido antes que um novo arquivo de log é criado pelo NPS.

- Determine se você deseja que o NPS para excluir arquivos de log antigos, se o disco rígido ficar sem espaço de armazenamento.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de estatísticas e produzir relatórios.

### <a name="nps-sql-server-logging"></a>Registro em log do SQL Server do NPS

Registro em log do SQL Server do NPS é usado quando você precisa de informações de estado de sessão, para fins de análise de dados e criação de relatório e para centralizar e simplificar o gerenciamento de seus dados contábeis.

NPS fornece a capacidade de usar o SQL Server log para a autenticação de usuário do registro e estatísticas de solicitações recebidas de um ou mais servidores de acesso de rede a uma fonte de dados em um computador executando o Microsoft SQL Server Desktop Engine \(MSDE 2000\), ou qualquer versão do SQL Server mais tarde do que o SQL Server 2000.

Dados de estatísticas são passados do NPS em formato XML para um procedimento armazenado no banco de dados, que dá suporte a ambos os linguagem de consulta estruturada \(SQL\) e XML \(SQLXML\). Solicitações em um banco de dados do SQL Server compatível com XML de contabilização e autenticação de usuário de gravação permite que vários NPSs ter uma fonte de dados.

### <a name="key-steps"></a>Principais etapas

Durante o planejamento para a contabilidade NPS usando o log do SQL Server do NPS, você pode usar as etapas a seguir.

- Determine se você ou outro membro da sua organização tem o desenvolvimento de banco de dados relacional do SQL Server 2000 ou SQL Server 2005 a experiência e você entende como usar esses produtos para criar, modificar, administrar e gerenciar bancos de dados do SQL Server.

- Determine se o SQL Server está instalado no NPS ou em um computador remoto.

- Crie o procedimento armazenado que você usará em seu banco de dados do SQL Server para processar arquivos de entrada XML que contêm dados de estatísticas do NPS.

- Projetar a estrutura de replicação de banco de dados do SQL Server e o fluxo.

- Determine o aplicativo ou aplicativos que você deseja usar para exibir dados de estatísticas e produzir relatórios.

- Planeje usar servidores de acesso de rede que enviam o atributo de classe em todas as solicitações de contabilidade. O atributo de classe é enviado ao cliente em uma mensagem de aceitação de acesso RADIUS e é útil para correlacionar mensagens de solicitação de estatísticas com sessões de autenticação. Se o atributo de classe é enviado pelo servidor de acesso de rede nas mensagens de solicitação de estatísticas, ele pode ser usado para corresponder os registros de contabilização e autenticação. A combinação dos atributos Unique-número de série, hora de reinicialização de serviço e o endereço do servidor deve ser uma identificação exclusiva para cada autenticação que o servidor aceita.

- Planeje usar servidores de acesso de rede que dão suporte a estatísticas provisórias.

- Planeje usar servidores de acesso de rede que enviam mensagens de ativação e desativação.

- Planeje usar servidores de acesso de rede que dão suporte ao armazenamento e encaminhamento de dados de estatísticas. Servidores de acesso de rede que dão suporte a esse recurso podem armazenar dados de estatísticas quando o servidor de acesso de rede não pode se comunicar com o NPS. Quando o NPS estiver disponível, o servidor de acesso de rede encaminha os registros armazenados para o NPS, fornecendo maior confiabilidade na contabilidade sobre os servidores de acesso de rede que não oferecem esse recurso.

- Planeje a configuração sempre o atributo Acct-Interim-Interval nas políticas de rede. O atributo Acct-Interim-Interval define o intervalo (em segundos) entre as atualizações provisórias que envia o servidor de acesso de rede. De acordo com a RFC 2869, o valor do atributo Acct-Interim-Interval não deve ser menor que 60 segundos, ou de um minuto e não deve ser menor do que 600 segundos ou 10 minutos. Para obter mais informações, consulte RFC 2869, "RADIUS Extensions".

- Certifique-se de que o log de status periódico está habilitado no seu NPSs.

