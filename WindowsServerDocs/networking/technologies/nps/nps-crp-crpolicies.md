---
title: Políticas de solicitação de Conexão
description: Este tópico fornece uma visão geral das políticas de solicitação de conexão do servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496ba87597b0be6cad1109269d2f72f52de017f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-policies"></a>Políticas de solicitação de Conexão

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para aprender a usar políticas de solicitação de conexão de NPS para configurar o servidor NPS como um servidor RADIUS, um proxy RADIUS ou ambos.

>[!NOTE]
>Além de neste tópico, a seguinte documentação de política de solicitação de conexão está disponível.
> - [Configurar as políticas de solicitação de Conexão](nps-crp-configure.md)
> - [Definir grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

As políticas de solicitação de Conexão são conjuntos de configurações que permitem aos administradores de rede designar quais servidores remotos Authentication Dial-In User Service (RADIUS) fazer a autenticação e autorização de solicitações de conexão que o servidor que executa o servidor de política de rede (NPS) recebe de clientes RADIUS e condições. Políticas de solicitação de Conexão podem ser configuradas para designar quais servidores RADIUS são usados para estatísticas RADIUS.

Você pode criar conexão políticas de solicitação para que algumas mensagens de solicitação RADIUS enviadas de clientes RADIUS são processadas localmente (NPS é usado como um servidor RADIUS) e outros tipos de mensagens são encaminhados para outro servidor RADIUS (NPS é usado como um proxy RADIUS).

Com as políticas de solicitação de conexão, você pode usar o NPS como um servidor RADIUS ou como um proxy RADIUS, com base em fatores como o seguinte: 

- A hora do dia e o dia da semana
- O nome do território na solicitação de conexão
- O tipo de conexão sendo solicitada
- O endereço IP do cliente RADIUS

Mensagens de solicitação de acesso RADIUS são processadas ou encaminhadas pelo NPS somente se as configurações da mensagem de entrada correspondem a pelo menos uma das políticas de solicitação de conexão configuradas no servidor NPS. 

Se corresponderem as configurações de política e a política exige que o servidor NPS processar a mensagem, o NPS atua como um servidor RADIUS, autenticar e autorizar a solicitação de conexão. Se corresponderem as configurações de política e a política exige que o servidor NPS encaminha a mensagem, o NPS atua como um proxy RADIUS e encaminha a solicitação de conexão a um servidor remoto RADIUS para processamento.

Se as configurações de uma mensagem de solicitação de acesso RADIUS recebida não corresponderem a pelo menos uma das diretivas de solicitação de conexão, uma mensagem de rejeição de acesso é enviada para o cliente RADIUS e o usuário ou computador tentando se conectar à rede tem acesso negado.

## <a name="configuration-examples"></a>Exemplos de configuração

Os exemplos de configuração a seguir demonstram como você pode usar políticas de solicitação de conexão.

### <a name="nps-as-a-radius-server"></a>NPS como um servidor RADIUS

A política de solicitação de conexão padrão é a única diretiva configurada. Neste exemplo, o NPS está configurado como um servidor RADIUS e todas as solicitações de conexão são processadas pelo servidor NPS local. O servidor NPS pode autenticar e autorizar usuários cujas contas estão no domínio do domínio do servidor NPS e em domínios confiáveis.


### <a name="nps-as-a-radius-proxy"></a>NPS como um proxy RADIUS

A diretiva de solicitação de conexão padrão é excluída e duas novas políticas de solicitação de conexão são criadas para encaminhar as solicitações para dois domínios diferentes. Neste exemplo, o NPS está configurado como um proxy RADIUS. NPS não processa todas as solicitações de conexão no servidor local. Em vez disso, ele encaminha as solicitações de conexão para NPS ou outros servidores RADIUS configurados como membros de grupos de servidores remotos RADIUS.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS e proxy RADIUS

Além da política de solicitação de conexão padrão, uma nova política de solicitação de conexão é criada para encaminhar as solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Neste exemplo, a política de proxy será exibida primeira na lista ordenada de políticas. Se a solicitação de conexão corresponder a política de proxy, a solicitação de conexão é encaminhada para o servidor RADIUS no grupo de servidores remotos RADIUS. Se a solicitação de conexão não coincide com a política de proxy, mas coincide com a política de solicitação de conexão padrão, o NPS processará a solicitação de conexão no servidor local. Se a solicitação de conexão não coincide com qualquer política, ele será descartado.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS com servidores de contabilidade remotos

Neste exemplo, o servidor NPS local não está configurado para realizar as estatísticas e a diretiva de solicitação de conexão padrão é revisada para que as mensagens de estatísticas RADIUS são encaminhadas para um NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS. Embora as mensagens de contabilidade são encaminhadas, as mensagens de autenticação e autorização não são encaminhadas e o servidor NPS local executa essas funções para o domínio local e todos os domínios confiáveis.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS com RADIUS remoto para mapeamento de usuário do Windows

Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy de RAIO para cada solicitação de conexão individuais, encaminhando a solicitação de autenticação para um servidor remoto RADIUS enquanto estiver usando uma conta de usuário do Windows local para autorização. Essa configuração é implementada Configurando o RAIO remoto ao atributo de mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. (Além disso, uma conta de usuário deve ser criada localmente com o mesmo nome que a conta de usuário remoto em relação ao qual a autenticação é realizada pelo servidor RADIUS remoto.)

## <a name="connection-request-policy-conditions"></a>Condições de política de solicitação de Conexão

Condições de política de solicitação de Conexão são um ou mais atributos RADIUS que são comparados com os atributos da mensagem de solicitação de acesso RADIUS de entrada. Se houver várias condições, todas as condições em que a conexão mensagem de solicitação e a solicitação de conexão política deve coincidir com para que a política seja imposta pelo NPS.

A seguir estão os atributos de condição disponível que você pode configurar em políticas de solicitação de conexão.

### <a name="connection-properties-attribute-group"></a>Grupo de atributos de propriedades de Conexão

O grupo de atributos de propriedades de Conexão contém os seguintes atributos.

- **Enquadrada protocolo**. Usado para designar o tipo de enquadramento para pacotes recebidos. Exemplos são protocolo (PPP), protocolo de Internet de linha Serial (SLIP), retransmissão e x. 25.
- **Tipo de serviço**. Usado para designar o tipo de serviço que está sendo solicitado. Os exemplos incluem enquadrada (por exemplo, conexões PPP) e logon (por exemplo, conexões Telnet). Para obter mais informações sobre os tipos de serviço RADIUS, consulte RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo de encapsulamento**. Usado para designar o tipo de encapsulamento que está sendo criado pelo cliente solicitante. Tipos de encapsulamento incluem o protocolo de encapsulamento de ponto a ponto (PPTP) e o protocolo de encapsulamento de camada 2 (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restrições de dia e hora 

O grupo de atributos de restrições de dia e hora contém o atributo de restrições de dia e hora. Com esse atributo, você pode designar o dia da semana e a hora do dia da tentativa de conexão. O dia e hora é relativa do dia e hora do servidor NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de gateway

O grupo de atributos de Gateway contém os seguintes atributos.

- **ID de estação chamada**. Usado para designar o número de telefone do servidor de acesso da rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar códigos de área.
- **Identificador NAS**. Usado para designar o nome do servidor de acesso da rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar os identificadores NAS.
- **Endereço IPv4 NAS**. Usado para designar o protocolo IP versão 4 (IPv4) endereço do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar redes IP.
- **Endereço IPv6 do NAS**. Usado para designar o protocolo IP versão 6 (IPv6) endereço do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar redes IP.
- **Tipo de porta NAS**. Usado para designar o tipo de mídia usado pelo cliente de acesso. Os exemplos são linhas telefônicas analógicas (conhecida como async), rede Digital de serviços integrados (ISDN), túneis ou redes virtuais privadas (VPNs), IEEE 802.11 sem fio e switches Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidade do computador

O grupo de atributos de identidade de máquina contém o atributo de identidade do computador. Usando esse atributo, você pode especificar o método com o qual os clientes são identificados na política.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propriedades de cliente RADIUS

O grupo de atributos de propriedades do cliente RADIUS contém os seguintes atributos.

- **ID de estação chamada**. Usado para designar o número de telefone usado pelo chamador (o cliente de acesso). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar códigos de área.
- **Nome amigável do cliente**. Usado para designar o nome do computador cliente RADIUS que está solicitando a autenticação. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar nomes de clientes.
- **Endereço IPv4 do cliente**. Usado para designar o endereço IPv4 do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar redes IP.
- **Endereço IPv6 do cliente**. Usado para designar o endereço IPv6 do servidor de acesso de rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar redes IP.
- **Fornecedor do cliente**. Usado para designar o fornecedor do servidor de acesso da rede que está solicitando a autenticação. Um computador executando o serviço de roteamento e acesso remoto é o fabricante da Microsoft. Você pode usar esse atributo para configurar políticas separadas para diferentes fabricantes de NAS. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nome de usuário

O grupo de atributos de nome de usuário contém o atributo de nome de usuário. Usando esse atributo, você pode designar o nome de usuário ou uma parte do nome do usuário, que deve corresponder ao nome de usuário fornecido pelo cliente de acesso na mensagem RADIUS. Esse atributo é uma cadeia de caracteres que normalmente contém um nome de território e um nome de conta de usuário. Você pode usar a sintaxe de padrões coincidentes para especificar nomes de usuário.

## <a name="connection-request-policy-settings"></a>Configurações de política de solicitação de Conexão

Configurações de política de solicitação de Conexão são um conjunto de propriedades que são aplicadas a uma mensagem recebida do RAIO. Configurações consistem em grupos de propriedades a seguir.

- Autenticação
- Contabilidade
- Manipulação de atributos
- Encaminhamento de solicitação
- Avançadas

As seguintes seções fornecem detalhes adicionais sobre essas configurações.

### <a name="authentication"></a>Autenticação

Usando essa configuração, você pode substituir as configurações de autenticação que estão configuradas em todas as políticas de rede e você pode designar os métodos de autenticação e os tipos que são necessárias para se conectar à rede.

>[!IMPORTANT]
>Se você definir um método de autenticação na diretiva de solicitação de conexão é menos segura que o método de autenticação que você configurar a política de rede, o método de autenticação mais seguro que você configurar em política de rede é substituído. Por exemplo, se você tiver uma política de rede que requer o uso de protegido extensível autenticação Protocol-Microsoft Challenge Handshake Authentication Protocol versão 2 \ (PEAP-MS-CHAP v2\), que é um método de autenticação baseada em senha para sem fio protegida, e você também configurar uma política de solicitação de conexão para permitir o acesso não autenticado, o resultado é que nenhum cliente é necessário para se autenticar usando PEAP-MS-CHAP v2. Neste exemplo, todos os clientes que se conectam à rede são concedidos acesso não autenticado.

### <a name="accounting"></a>Contabilidade

Usando essa configuração, você pode configurar a política de solicitação de conexão para encaminhar as informações de contabilidade para um NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS para que o grupo de servidores remotos RADIUS realiza a contagem.

>[!NOTE]
>Se você tiver vários servidores RADIUS, e você deseja que as informações de contabilidade para todos os servidores armazenados em um RAIO contabilidade banco de dados central, você pode usar a configuração de estatísticas de política de solicitação conexão em uma política em cada servidor RADIUS para encaminhar os dados contábeis de todos os servidores para um NPS ou outro servidor RADIUS designado como um servidor de contabilidade.

Configurações de política de solicitação de Conexão estatísticas funcionam independente da configuração Contabilidade do servidor NPS local. Em outras palavras, se você definir o servidor NPS local para registrar informações de contabilização de RAIO em um arquivo local ou um banco de dados do Microsoft SQL Server, irá fazer isso, independentemente de se você configurar uma política de solicitação de conexão para encaminhar mensagens de contabilidade para um grupo de servidores remotos RADIUS.

Se você quiser saber contabilidade conectado remotamente, mas não localmente, configure o servidor NPS local para não executar contabilidade, enquanto também configura a contabilização em uma diretiva de solicitação de conexão para encaminhar os dados contábeis para um grupo de servidores remotos RADIUS.

### <a name="attribute-manipulation"></a>Manipulação de atributos

Você pode configurar um conjunto de regras de localização e substituição que manipulam as cadeias de caracteres de texto de um dos seguintes atributos.

- Nome de usuário
- ID de estação chamada
- ID de estação chamada

Processamento de regra de localização e substituição ocorre para um dos atributos anteriores antes da mensagem RADIUS está sujeita às configurações de autenticação e estatísticas. Regras de manipulação de atributo se aplicam somente a um único atributo. Você não pode configurar as regras de manipulação de atributo para cada atributo. Além disso, a lista de atributos que você pode manipular é uma lista estática; Você não pode adicionar à lista de atributos disponíveis para manipulação.

>[!NOTE]
>Se você estiver usando o protocolo de autenticação MS-CHAP v2, você não poderá manipular o atributo de nome de usuário se a diretiva de solicitação de conexão é usada para encaminhar a mensagem RADIUS. A única exceção ocorre quando um caractere de barra invertida (\ \ \) é usado e a manipulação afeta apenas as informações à esquerda dele. Um caractere de barra invertida normalmente é usado para indicar um nome de domínio (as informações à esquerda do caractere de barra invertida) e um nome de conta de usuário dentro do domínio (as informações à direita do caractere de barra invertida). Nesse caso, são permitidas apenas regras de manipulação atributo que modificar ou substituem o nome de domínio.

Para obter exemplos de como manipular o nome do território no atributo de nome de usuário, consulte a seção "Exemplos para manipulação do nome do território no atributo de nome de usuário" no tópico [Use expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Encaminhamento de solicitação

Você pode definir as seguintes opções de solicitação que são usadas para mensagens de solicitação de acesso RADIUS de encaminhamento:

- **Autenticar solicitações nesse servidor**. Usando essa configuração, o NPS usa um domínio do Windows NT 4.0, Active Directory ou o banco de dados de contas de usuário Gerenciador de contas de segurança (SAM) local para autenticar a solicitação de conexão. Essa configuração também especifica que a política de rede correspondente configurada no NPS, juntamente com as propriedades de discagem rápida da conta de usuário, são usados pelo NPS para autorizar a solicitação de conexão. Nesse caso, o servidor NPS é configurado para executar como um servidor RADIUS.

- **Encaminhar as solicitações para o grupo de servidores remotos RADIUS seguir**. Usando essa configuração, o NPS encaminha as solicitações de conexão para o grupo de servidores remotos RADIUS que você especificar. Se o servidor NPS recebe uma mensagem de aceitação de acesso válida que corresponde à mensagem de solicitação de acesso, a tentativa de conexão será considerada autenticados e autorizados. Nesse caso, o servidor NPS atua como um proxy RADIUS.

- **Aceitar usuários sem validar as credenciais**. Usando essa configuração, NPS não verifica a identidade do usuário que está tentando se conectar à rede e NPS não tenta Verifique se o usuário ou o computador tem o direito de se conectar à rede. Quando NPS está configurado para permitir o acesso não autenticado e receber uma solicitação de conexão, o NPS imediatamente envia que uma mensagem para o RAIO do cliente e o usuário ou computador aceitar o acesso é concedida acesso à rede. Essa configuração é usada para alguns tipos de encapsulamento compulsório em que o cliente de acesso é encapsulado antes que as credenciais do usuário são autenticadas.

>[!NOTE]
>Essa opção de autenticação não pode ser usada quando o protocolo de autenticação do cliente de acesso é MS-CHAP v2 ou Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), que fornecem autenticação mútua. Na autenticação mútua, o cliente de acesso prova que é um cliente de acesso válido para o servidor de autenticação (o servidor NPS), e o servidor de autenticação prova que se trata de um servidor de autenticação válido para o cliente de acesso. Quando esta opção de autenticação é usada, a mensagem de aceitação de acesso é retornada. No entanto, o servidor de autenticação não fornece validação para o cliente de acesso e autenticação mútua falhar.

Para obter exemplos de como usar expressões regulares para criar regras de roteamento que encaminham mensagens RADIUS com um nome de domínio especificado para um grupo de servidores remotos RADIUS, consulte a seção "Exemplo de encaminhamento de mensagens RADIUS por um servidor proxy" no tópico [Use expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avançadas

Você pode definir propriedades avançadas para especificar a série de atributos RADIUS:

- Adicionado à mensagem de resposta RADIUS quando o servidor NPS está sendo usado como um servidor de contabilidade ou autenticação RADIUS. Quando há atributos especificados em uma política de rede e a política de solicitação de conexão, os atributos que são enviados na mensagem de resposta RADIUS são a combinação dos dois conjuntos de atributos.
- Adicionado à mensagem RADIUS quando o servidor NPS está sendo usado como um proxy estatística ou autenticação RADIUS. Se o atributo já existe na mensagem que é encaminhada, ele será substituído com o valor do atributo especificado na diretiva de solicitação de conexão. 

Além disso, alguns atributos que estão disponíveis para configuração sobre a conexão solicitam política **configurações** guia a **avançado** categoria fornecer funcionalidade especializada. Por exemplo, você pode configurar o **RADIUS remoto para Windows User Mapping** atributo quando você deseja dividir a autenticação e autorização de uma solicitação de conexão entre o usuário duas contas bancos de dados.

O **RADIUS remoto para Windows User Mapping** atributo especifica que a autorização do Windows ocorre para usuários autenticados por um servidor remoto RADIUS. Em outras palavras, um servidor RADIUS remoto executa a autenticação contra uma conta de usuário em um banco de dados de contas de usuário remoto, mas o servidor NPS local autoriza a solicitação de conexão com uma conta de usuário em um banco de dados de contas de usuário local. Isso é útil quando você deseja permitir que os visitantes acesso à rede.

Por exemplo, visitantes de organizações parceiras podem ser autenticados por seu próprio servidor de RAIO de organização de parceiro e então usam uma conta de usuário do Windows em sua organização para acessar uma convidado rede local (LAN) em sua rede.

Outros atributos que fornecem funcionalidade especializada são:

- **MS-quarentena-IPFilter e MS-quarentena-sessão-Timeout**. Esses atributos são usados quando você implanta o controle de quarentena de acesso de rede (NAQC) com a implantação de roteamento e acesso remoto VPN.
- **Passport-usuário-mapeamento--sufixo**. Esse atributo permite autenticar solicitações de conexão com credenciais de conta de usuário do Windows Live™ ID.
- **Marca de túnel**. Esse atributo designa o número de ID de VLAN para que a conexão deve ser atribuída pelo ao implantar redes locais virtuais (VLANs).

## <a name="default-connection-request-policy"></a>Política de solicitação de conexão padrão

Uma diretiva de solicitação de conexão padrão é criada quando você instala o NPS. Esta configuração de política tem a seguinte configuração.

- Autenticação não está configurada.
- Contabilidade não está configurada para encaminhar informações de contabilização a um grupo de servidores remotos RADIUS.
- Atributo não está configurado com as regras de manipulação de atributos que encaminhe solicitações de conexão para grupos de servidores remotos RADIUS.
- Solicitação de encaminhamento é configurado para que as solicitações de conexão são autenticadas e autorizadas no servidor NPS local.
- Atributos avançados não estão configurados.

A política de solicitação de conexão padrão usa NPS como um servidor RADIUS. Para configurar um servidor NPS para atuar como um proxy RADIUS, você também deverá configurar um grupo de servidores remotos RADIUS. Você pode criar um novo grupo de servidores remotos RADIUS enquanto você estiver criando uma nova política de solicitação de conexão usando o Assistente de nova diretiva de solicitação de Conexão. Você pode excluir a política de solicitação de conexão padrão ou verificar se a política de solicitação de conexão padrão é a última diretiva processada pelo NPS colocando-a última na lista ordenada de políticas.

>[!NOTE]
>Se o NPS e o serviço de acesso remoto estão instalados no mesmo computador e o acesso remoto serviço está configurado para autenticação do Windows e contabilidade, é possível para autenticação de acesso remoto e solicitações de contabilidade para ser encaminhada para um servidor RADIUS. Isso pode ocorrer quando a autenticação de acesso remoto e solicitações de contabilidade correspondem a uma política de solicitação de conexão que está configurada para encaminhá-las a um grupo de servidores remotos RADIUS.