---
title: Políticas de solicitação de conexão
description: Este tópico fornece uma visão geral das diretivas de solicitação de conexão do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21330b3838064c7b31e78ef70bdc04f0db0f50db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872937"
---
# <a name="connection-request-policies"></a>Políticas de solicitação de conexão

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar as diretivas de solicitação de conexão de NPS para configurar o NPS como um servidor RADIUS, proxy RADIUS ou ambos.

>[!NOTE]
>Além deste tópico, a seguinte documentação de diretiva de solicitação de conexão está disponível.
> - [Configurar políticas de solicitação de Conexão](nps-crp-configure.md)
> - [Configurar grupos de servidores remotos RADIUS](nps-crp-rrsg-configure.md)

Diretivas de solicitação de Conexão são conjuntos de condições e configurações que permitem que os administradores de rede designar quais servidores RADIUS Remote Authentication Dial-In usuário Service () executam a autenticação e solicitações de autorização de conexão que o servidor que executa o servidor de diretivas de rede (NPS) recebe de clientes RADIUS. Políticas de solicitação de Conexão podem ser configuradas para designar quais servidores RADIUS são usados para contabilização RADIUS.

Você pode criar conexão diretivas de solicitação para que algumas mensagens de solicitação RADIUS enviadas de clientes RADIUS sejam processadas localmente (o NPS é usado como um servidor RADIUS) e outros tipos de mensagens são encaminhados para outro servidor RADIUS (NPS for usado como um proxy RADIUS).

Com as diretivas de solicitação de conexão, você pode usar o NPS como um servidor RADIUS ou como um proxy RADIUS, com base em fatores como o seguinte: 

- A hora do dia e dia da semana
- O nome de realm na solicitação de conexão
- O tipo de conexão que está sendo solicitado
- O endereço IP do cliente RADIUS

Mensagens de solicitação de acesso RADIUS são processadas ou encaminhadas pelo NPS somente se as configurações da mensagem de entrada corresponderem a pelo menos uma das diretivas de solicitação de conexão configuradas no NPS. 

Se corresponderem as configurações de política e a política exige que o NPS processar a mensagem, o NPS atua como um servidor RADIUS, autenticar e autorizar a solicitação de conexão. Se corresponderem as configurações de política e a política exige que o NPS encaminha a mensagem, o NPS atua como um proxy RADIUS e encaminha a solicitação de conexão para um servidor RADIUS remoto para processamento.

Se as configurações de uma mensagem de solicitação de acesso RADIUS de entrada não corresponder a pelo menos uma das diretivas de solicitação de conexão, uma mensagem de rejeição de acesso é enviada ao cliente RADIUS e o usuário ou computador tentar se conectar à rede tem acesso negado.

## <a name="configuration-examples"></a>Exemplos de configuração

Os exemplos de configuração a seguir demonstram como você pode usar as diretivas de solicitação de conexão.

### <a name="nps-as-a-radius-server"></a>NPS como um servidor RADIUS

A diretiva de solicitação de conexão padrão é a única política configurada. Neste exemplo, o NPS é configurado como um servidor RADIUS e todas as solicitações de conexão são processadas pelo NPS local. O NPS pode autenticar e autorizar os usuários cujas contas estejam no domínio do domínio NPS e em domínios confiáveis.


### <a name="nps-as-a-radius-proxy"></a>NPS como um proxy RADIUS

A diretiva de solicitação de conexão padrão é excluída e duas novas diretivas de solicitação de conexão são criadas para encaminhar solicitações para dois domínios diferentes. Neste exemplo, o NPS é configurado como um proxy RADIUS. NPS não processar as solicitações de conexão no servidor local. Em vez disso, ele encaminha solicitações de conexão para o NPS ou outros servidores RADIUS que são configurados como membros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS e proxy RADIUS

Além da política de solicitação de conexão padrão, uma nova diretiva de solicitação de conexão é criada que encaminha solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Neste exemplo, a diretiva de proxy aparece primeira na lista ordenada de políticas. Se a solicitação de conexão corresponde a diretiva de proxy, a solicitação de conexão é encaminhada para o servidor RADIUS no grupo de servidores RADIUS remotos. Se a solicitação de conexão não coincide com a diretiva de proxy, mas coincide com a diretiva de solicitação de conexão padrão, o NPS processa a solicitação de conexão no servidor local. Se a solicitação de conexão não coincide com a política, ela será descartada.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS com servidores remotos de estatísticas

Neste exemplo, o NPS local não está configurado para executar a contabilização e a diretiva de solicitação de conexão padrão é revisada para que as mensagens de contabilização RADIUS sejam encaminhadas para um NPS ou outro servidor RADIUS de um grupo de servidores RADIUS remotos. Embora as mensagens de estatística são encaminhadas, mensagens de autenticação e autorização não são encaminhadas e o NPS local executa essas funções para o domínio local e todos os domínios confiáveis.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS com RADIUS remoto para mapeamento de usuário do Windows

Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy RADIUS para cada solicitação de conexão individuais, encaminhar a solicitação de autenticação para um servidor RADIUS remoto ao usar uma conta de usuário do Windows local para autorização. Essa configuração é implementada por meio da configuração de RADIUS remoto para o atributo de mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. (Além disso, uma conta de usuário deve ser criada localmente que tem o mesmo nome que a conta de usuário remoto em relação ao qual autenticação é realizada pelo servidor RADIUS remoto.)

## <a name="connection-request-policy-conditions"></a>Condições de política de solicitação de Conexão

Condições de política de solicitação de Conexão são um ou mais atributos RADIUS que são comparados com os atributos da mensagem de solicitação de acesso RADIUS de entrada. Se houver várias condições, em seguida, todas as condições em que a conexão a mensagem de solicitação e na solicitação de conexão política deve corresponder para que a diretiva seja imposta pelo NPS.


A seguir estão os atributos de condição disponíveis que você pode configurar políticas de solicitação de conexão.

### <a name="connection-properties-attribute-group"></a>Grupo de atributos de propriedades de Conexão

O grupo de atributos de propriedades de Conexão contém os atributos a seguir.

- **Enquadrados protocolo**. Usado para designar o tipo de enquadramento para pacotes de entrada. Exemplos são o protocolo (PPP), protocolo de Internet de linha Serial (SLIP), Frame Relay e X.25.
- **Tipo de serviço**. Usado para designar o tipo de serviço que está sendo solicitado. Os exemplos incluem estruturado (por exemplo, as conexões PPP) e logon (por exemplo, conexões de Telnet). Para obter mais informações sobre os tipos de serviço RADIUS, consulte RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo de Encapsulamento**. Usado para designar o tipo de túnel que está sendo criado pelo cliente solicitante. Tipos de encapsulamento incluem o protocolo de encapsulamento ponto a ponto (PPTP) e o Layer Two Tunneling Protocol (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restrições de dia e hora 

O grupo de atributos de restrições de dia e hora contém o atributo de restrições de dia e hora. Com esse atributo, você pode designar o dia da semana e a hora do dia da tentativa de conexão. O dia e hora é relativo ao dia e hora do NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de gateway

O grupo de atributos de Gateway contém os atributos a seguir.

- **ID de estação chamada**. Usado para designar o número de telefone do servidor de acesso de rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.
- **Identificador NAS**. Usado para designar o nome do servidor de acesso de rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar identificadores NAS.
- **Endereço de IPv4 NAS**. Usado para designar o protocolo IP versão 4 (IPv4) o endereço do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Endereço IPv6 do NAS**. Usado para designar o protocolo IP versão 6 (IPv6) o endereço do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Tipo de porta NAS**. Usado para designar o tipo de mídia usado pelo cliente de acesso. Os exemplos são linhas telefônicas analógicas (conhecido como assíncrono), Integrated Services Digital Network (ISDN), túneis ou redes virtuais privadas (VPNs), IEEE 802.11 sem fio e switches Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidade do computador

O grupo de atributos de identidade da máquina contém o atributo de identidade da máquina. Ao usar esse atributo, você pode especificar o método com o qual os clientes são identificados na política.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propriedades do cliente RADIUS

O grupo de atributos de propriedades do cliente RADIUS contém os atributos a seguir.

- **ID de estação chamada**. Usado para designar o número de telefone usado pelo chamador (o cliente de acesso). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.  No 802.1X autenticações o endereço MAC costuma ser preenchido e podem ser correspondido do cliente.  Esse campo é normalmente usado para cenários de Bypass de endereço Mac quando a diretiva de solicitação de conexão é configurada para 'Aceitar usuários sem validar as credenciais'.  
- **Nome amigável do cliente**. Usado para designar o nome do computador cliente RADIUS que está solicitando a autenticação. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar nomes de clientes.
- **Endereço IPv4 do cliente**. Usado para designar o endereço IPv4 do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Endereço IPv6 do cliente**. Usado para designar o endereço IPv6 do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Fornecedor do cliente**. Usado para designar o fornecedor do servidor de acesso de rede que está solicitando a autenticação. Um computador que executa o serviço Roteamento e acesso remoto é o fabricante da Microsoft. Você pode usar esse atributo para configurar diretivas separadas para fabricantes de NAS diferentes. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nome de usuário

O grupo de atributos de nome de usuário contém o atributo de nome de usuário. Ao usar esse atributo, você pode designar o nome de usuário ou uma parte do nome de usuário, que deve corresponder ao nome de usuário fornecido pelo cliente de acesso na mensagem RADIUS. Esse atributo é uma cadeia de caracteres que geralmente contém um nome de realm e um nome de conta de usuário. Você pode usar a sintaxe de correspondência de padrões para especificar nomes de usuário.

## <a name="connection-request-policy-settings"></a>Configurações de diretiva de solicitação de Conexão

Configurações de diretiva de solicitação de Conexão são um conjunto de propriedades que são aplicadas a uma mensagem RADIUS de entrada. Configurações consistem em grupos de propriedades a seguir.

- Autenticação
- Contabilização
- Manipulação de atributos
- Solicitação de encaminhamento
- Avançado

As seções a seguir fornecem mais detalhes sobre essas configurações.

### <a name="authentication"></a>Autenticação

Ao usar essa configuração, você pode substituir as configurações de autenticação são configuradas em todas as diretivas de rede e você pode designar os métodos de autenticação e os tipos que são necessárias para se conectar à sua rede.

>[!IMPORTANT]
>Se você configurar um método de autenticação na diretiva de solicitação de conexão que é menos segura do que o método de autenticação que você definir na diretiva de rede, o método de autenticação mais seguro que você configurar na política de rede é substituído. Por exemplo, se você tiver uma diretiva de rede que requer o uso de Protected Extensible autenticação Protocol-Microsoft Challenge Handshake Authentication Protocol versão 2 \(PEAP-MS-CHAP v2\), que é um baseado em senha método de autenticação para rede sem fio segura e você também configurar uma diretiva de solicitação de conexão para permitir o acesso não autenticado, o resultado é que nenhum cliente é necessário para autenticar usando PEAP-MS-CHAP v2. Neste exemplo, todos os clientes que se conectam à sua rede recebem acesso não autenticado.

### <a name="accounting"></a>Contabilização

Ao usar essa configuração, você pode configurar a diretiva de solicitação de conexão para encaminhar informações de estatísticas para um NPS ou outro servidor RADIUS de um grupo de servidores remotos RADIUS, para que o grupo de servidores remotos RADIUS realiza a contagem.

>[!NOTE]
>Se você tiver vários servidores RADIUS e você deseja obter informações de estatísticas para todos os servidores armazenados em um RAIO contabilidade banco de dados central, você pode usar a configuração de estatísticas de diretiva de solicitação conexão em uma política em cada servidor RADIUS para encaminhar os dados contábeis do todos os servidores para um NPS ou outro servidor RADIUS que é designado como um servidor de estatísticas.

As configurações de contabilização de política de função independente da configuração de contabilidade do NPS local de solicitação de Conexão. Em outras palavras, se você configurar o NPS local para registrar em log informações de contabilidade do RADIUS para um arquivo local ou para um banco de dados do Microsoft SQL Server, ele fará isso independentemente se você configura uma diretiva de solicitação de conexão para encaminhar mensagens de estatísticas para um RADIUS remoto grupo de servidores.

Se você quiser informações contábeis registradas remotamente, mas não localmente, você deve configurar o NPS local para não executar a contabilização, enquanto também Configurando as estatísticas em uma diretiva de solicitação de conexão para encaminhar dados de estatísticas para um grupo de servidores RADIUS remotos.

### <a name="attribute-manipulation"></a>Manipulação de atributos

Você pode configurar um conjunto de regras de localizar e substituir que manipulam as cadeias de caracteres de texto de um dos seguintes atributos.

- Nome do Usuário
- ID de estação chamada
- ID de estação chamada

O processamento da regra de localizar e substituir ocorre por um dos atributos precedentes, antes da mensagem RADIUS está sujeito a configurações de contabilização e autenticação. Regras de manipulação de atributos se aplicam somente a um único atributo. Você não pode configurar regras de manipulação de atributos para cada atributo. Além disso, a lista de atributos que podem ser manipulados é uma lista estática; Você não pode adicionar à lista de atributos disponíveis para manipulação.

>[!NOTE]
>Se você estiver usando o protocolo de autenticação MS-CHAP v2, você não pode manipular o atributo de nome de usuário se a diretiva de solicitação de conexão é usada para encaminhar a mensagem RADIUS. A única exceção ocorre quando uma barra invertida (\) caractere é usado e a manipulação afeta apenas as informações à esquerda dele. Normalmente, um caractere de barra invertida é usado para indicar um nome de domínio (as informações à esquerda do caractere de barra invertida) e um nome de conta de usuário dentro do domínio (as informações à direita do caractere de barra invertida). Nesse caso, são permitidas apenas regras de manipulação atributos que modificam ou substituem o nome de domínio.

Para obter exemplos de como manipular o nome do território no atributo de nome de usuário, consulte a seção "Exemplos de manipulação do nome do território no atributo de nome de usuário" no tópico [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Solicitação de encaminhamento

Você pode configurar as seguintes opções de solicitação que são usadas para mensagens de solicitação de acesso RADIUS de encaminhamento:

- **Autenticar solicitações neste servidor**. Ao usar essa configuração, o NPS usa um domínio do Windows NT 4.0, Active Directory ou o banco de dados de contas de usuário gerente de contas de segurança (SAM) local para autenticar a solicitação de conexão. Essa configuração também especifica que a diretiva de rede correspondente configurada no NPS, juntamente com as propriedades de discagem da conta de usuário, são usadas pelo NPS para autorizar a solicitação de conexão. Nesse caso, o NPS é configurado para executar como um servidor RADIUS.

- **Encaminhar solicitações para o seguinte grupo de servidores remotos RADIUS**. Ao usar essa configuração, o NPS encaminha solicitações de conexão para o grupo de servidores remotos RADIUS que você especificar. Se o NPS recebe uma mensagem de aceitação de acesso válida que corresponde à mensagem de solicitação de acesso, a tentativa de conexão será considerada autenticados e autorizados. Nesse caso, o NPS atua como um proxy RADIUS.

- **Aceitar usuários sem validar as credenciais**. Ao usar essa configuração, o NPS não verifica a identidade do usuário de tentar se conectar à rede e NPS não tenta verificar se o usuário ou computador tem o direito de se conectar à rede. Quando NPS está configurado para permitir o acesso não autenticado e receber uma solicitação de conexão, o NPS envia imediatamente que uma mensagem de aceitação de acesso para o RAIO do cliente e o usuário ou computador é concedida o acesso à rede. Essa configuração é usada para alguns tipos de túnel compulsório em que o cliente de acesso é encapsulado antes que as credenciais do usuário são autenticadas.

>[!NOTE]
>Essa opção de autenticação não pode ser usada quando o protocolo de autenticação do cliente de acesso é MS-CHAP v2 ou Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), que fornecem autenticação mútua. Na autenticação mútua, o cliente de acesso prova que é um cliente de acesso válido para o servidor de autenticação (NPS) e o servidor de autenticação prova que é um servidor de autenticação válido para o cliente de acesso. Quando essa opção de autenticação é usada, a mensagem de aceitação de acesso é retornada. No entanto, o servidor de autenticação não fornece validação para o cliente de acesso e Falha na autenticação mútua.

Para obter exemplos de como usar expressões regulares para criar regras de roteamento que encaminham mensagens RADIUS com um nome de realm especificado para um grupo de servidores RADIUS remotos, consulte a seção "Exemplo de encaminhamento de mensagens RADIUS por um servidor proxy" no tópico [usar Expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avançado

Você pode definir propriedades avançadas para especificar a série de atributos RADIUS que são:

- Adicionado à mensagem de resposta RADIUS quando NPS está sendo usado como um servidor de estatísticas ou a autenticação RADIUS. Quando há atributos especificados em uma diretiva de rede e a diretiva de solicitação de conexão, os atributos que são enviados na mensagem de resposta RADIUS são a combinação dos dois conjuntos de atributos.
- Adicionado à mensagem RADIUS quando NPS está sendo usado como autenticação RADIUS ou proxy de contabilidade. Se o atributo já existe na mensagem que é encaminhada, ele será substituído com o valor do atributo especificado na diretiva de solicitação de conexão. 

Além disso, alguns atributos que estão disponíveis para a configuração em que a conexão solicitam a política **as configurações** guia o **avançado** categoria oferecem funções especializadas. Por exemplo, você pode configurar o **RADIUS remoto para mapeamento de usuário do Windows** quando você deseja dividir a autenticação e autorização de uma solicitação de conexão entre o usuário duas contas de bancos de dados de atributo.

O **RADIUS remoto para mapeamento de usuário do Windows** atributo especifica que a autorização do Windows ocorre para os usuários que são autenticados por um servidor RADIUS remoto. Em outras palavras, um servidor RADIUS remoto executa a autenticação em relação a uma conta de usuário em um banco de dados de contas de usuário remoto, mas o NPS local autoriza a solicitação de conexão para uma conta de usuário em um banco de dados de contas de usuário local. Isso é útil quando você deseja permitir que os visitantes acesso à sua rede.

Por exemplo, os visitantes de organizações parceiras podem ser autenticados por seu próprio servidor RADIUS da organização parceira e, em seguida, usam uma conta de usuário do Windows na sua organização para acessar uma convidado rede local (LAN) em sua rede.

Outros atributos que oferecem funções especializadas são:

- **MS-Quarantine-IPFilter e MS-quarentena--tempo limite da sessão**. Esses atributos são usados quando você implantar o controle de quarentena de acesso de rede (NAQC) com sua implantação de roteamento e acesso remoto VPN.
- **Passport-User-Mapping-UPN-Suffix**. Este atributo permite autenticar solicitações de conexão com credenciais de conta de usuário do Windows Live™ ID.
- **Tunnel-Tag**. Esse atributo designa o número de ID de VLAN para o qual a conexão deve ser atribuído pelo ao implantar redes locais virtuais (VLANs).

## <a name="default-connection-request-policy"></a>Diretiva de solicitação de conexão padrão

Uma diretiva de solicitação de conexão padrão é criada quando você instala o NPS. Essa política tem a seguinte configuração.

- Autenticação não está configurada.
- Contabilidade não está configurada para encaminhar informações de contabilidade a um grupo de servidores RADIUS remotos.
- Atributo não está configurado com regras de manipulação de atributo que encaminham as solicitações de conexão para grupos de servidores RADIUS remotos.
- Solicitação de encaminhamento está configurado para que as solicitações de conexão são autenticadas e autorizadas no NPS local.
- Atributos avançados não estão configurados.

A diretiva de solicitação de conexão padrão usa o NPS como servidor RADIUS. Para configurar um servidor que executa o NPS para atuar como um proxy RADIUS, você também deve configurar um grupo de servidores RADIUS remotos. Enquanto você estiver criando uma nova diretiva de solicitação de conexão usando o Assistente de nova diretiva de solicitação de Conexão, você pode criar um novo grupo de servidores RADIUS remoto. Você pode excluir a política de solicitação de conexão padrão ou verifique se a diretiva de solicitação de conexão padrão é a última diretiva processada pelo NPS, colocando-o pela última vez na lista ordenada de políticas.

>[!NOTE]
>Se o NPS e o serviço de acesso remoto são instalados no mesmo computador e o acesso remoto do serviço está configurado para autenticação do Windows e contabilidade, é possível que solicitações de estatísticas a serem encaminhadas para um servidor RADIUS e autenticação de acesso remoto . Isso pode ocorrer quando as solicitações de contabilização e autenticação de acesso remoto correspondem a uma diretiva de solicitação de conexão que está configurada para encaminhá-las a um grupo de servidores RADIUS remotos.
