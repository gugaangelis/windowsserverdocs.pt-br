---
title: Planeje NPS como um proxy RADIUS
description: Este tópico fornece informações sobre a implantação de proxy de rede política servidor RADIUS planejamento no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planeje NPS como um proxy RADIUS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando você implanta o servidor de política de rede (NPS) como um proxy de autenticação remota Dial-In User Service \(RADIUS\), o NPS recebe solicitações de conexão de clientes RADIUS, como servidores de acesso à rede ou outros proxies RADIUS e encaminha essas solicitações de conexão com servidores que executam NPS ou outros servidores RADIUS. Você pode usar essas diretrizes de planejamento para simplificar a implantação de RAIO.

Estas diretrizes de planejamento não incluem circunstâncias em que você deseja implantar o NPS como um servidor RADIUS. Quando você implanta o NPS como um servidor RADIUS, o NPS realiza autenticação, autorização e estatísticas para solicitações de conexão para o domínio local e para domínios que confiam no domínio local.

Antes de implantar NPS como um proxy RADIUS em sua rede, use as seguintes diretrizes para planejar sua implantação.

- Planeje a configuração do servidor NPS.

- Planeje clientes RADIUS.

- Planeje os grupos de servidores remotos RADIUS.

- Planeje regras de manipulação de atributo de encaminhamento de mensagem.

- Planeje as políticas de solicitação de conexão.

- Planeje contabilidade NPS.

## <a name="plan-nps-server-configuration"></a>Planejar configuração do servidor NPS

Quando você usa o NPS como um proxy RADIUS, o NPS encaminha as solicitações de conexão para um servidor NPS ou outros servidores RADIUS para processamento. Por isso, a participação no domínio do proxy NPS é irrelevante. O proxy não precisa ser registrada no Active Directory Domain Services \(AD DS\) porque ela não precisa ter acesso às propriedades de discagem rápida de contas de usuário. Além disso, você não precisa configurar políticas de rede em um proxy NPS porque o proxy não executa a autorização para solicitações de conexão. O proxy NPS pode ser um membro do domínio ou pode ser um servidor independente com nenhuma associação ao domínio.

NPS deve ser configurado para se comunicar com os clientes RADIUS, também chamados de servidores de acesso à rede, usando o protocolo RADIUS. Além disso, você pode configurar os tipos de eventos que registra NPS no caso de log e você pode inserir uma descrição para o servidor.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para a configuração de proxy NPS, você pode usar as etapas a seguir.

- Determine as portas de RAIO proxy NPS usa para receber mensagens de RAIO de clientes RADIUS e enviar mensagens de RAIO para membros de grupos de servidores remotos RADIUS. As portas de protocolo UDP (User Datagram) padrão são 1812 e 1645 para mensagens de autenticação RADIUS e portas UDP 1813 e 1646 para mensagens de estatísticas RADIUS.

- Se o proxy NPS é configurado com vários adaptadores de rede, determine os adaptadores por meio da qual você deseja que o tráfego de RAIO seja permitido.

- Determine os tipos de eventos que você deseja NPS para gravar no Log de eventos. Você pode fazer solicitações de conexão rejeitadas, solicitações de conexão bem-sucedida ou ambos.

- Determine se você estiver implantando mais de um proxy NPS. Para fornecer tolerância, use pelo menos dois proxies NPS. Um proxy NPS é usado como o proxy RADIUS primário e o outro é usado como um backup. Cada cliente RADIUS é configurado, em seguida, em ambos os proxies NPS. Se o proxy NPS primário ficar indisponível, clientes RADIUS enviam mensagens de solicitação de acesso para o proxy NPS alternativo.

- Planeje o script usado para copiar uma configuração de proxy NPS para outros proxies NPS para salvar na sobrecarga administrativa e impedir que a configuração incorreta de um servidor. NPS fornece os comandos Netsh que permitem que você pode copiar todo ou parte de uma configuração de proxy NPS para importação em outro proxy NPS. Você pode executar os comandos manualmente no prompt do Netsh. No entanto, se você salvar sua sequência de comando como um script, você pode executar o script em uma data posterior se você decidir alterar suas configurações de proxy.

## <a name="plan-radius-clients"></a>Planeje clientes RADIUS

Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, servidores \(VPN\) de rede virtual privada, 802.1 comutadores compatíveis com X e servidores dial-up. Proxies RADIUS, o qual conexão encaminhar mensagens de solicitação para servidores RADIUS, também são clientes RADIUS. NPS dá suporte a todos os servidores de acesso à rede e proxies RADIUS compatíveis com o protocolo RADIUS, conforme descrito em RFC 2865, "autenticação discagem usuário remoto serviço \(RADIUS\)," e RFC 2866, "RADIUS Accounting".

Além disso, os pontos de acesso sem fio e comutadores devem ser capazes de autenticação 802.1 X. Se você deseja implantar EAP Extensible Authentication Protocol () ou Protected Extensible Authentication Protocol (PEAP), pontos de acesso e comutadores devem suportar o uso de EAP.

Para testar a interoperabilidade básica para conexões PPP para pontos de acesso sem fio, configure o ponto de acesso e o cliente de acesso para usar o protocolo de autenticação de senha (PAP). Use protocolos de autenticação baseada em PPP adicionais, como PEAP, até que você testou aqueles que você pretende usar para acesso à rede.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para clientes RADIUS, você pode usar as etapas a seguir.

- Documente os atributos específica do fornecedor (VSAs), que você deve configurar no NPS. Se seu NASs requerem VSAs, registra as informações de VSA para uso posterior quando você configurar as políticas de rede no NPS.

- Documente os endereços IP dos clientes RADIUS e o proxy NPS para simplificar a configuração de todos os dispositivos. Quando você implanta os clientes RADIUS, você deve configurá-los para usar o protocolo RADIUS, com o endereço IP do proxy NPS inserido como o servidor de autenticação. E quando você configura o NPS para se comunicar com os clientes RADIUS, você deve inserir os endereços IP do cliente RADIUS no snap-in NPS.

- Crie segredos compartilhados para configuração nos clientes RADIUS e o snap-in NPS. Você deve configurar clientes RADIUS com um segredo compartilhado, ou a senha, que você também entrará no snap-in NPS ao configurar clientes RADIUS de NPS.

## <a name="plan-remote-radius-server-groups"></a>Planejar grupos de servidores RADIUS remotos

Quando você configura um grupo de servidores remotos RADIUS em um proxy de NPS, dizem o proxy NPS onde enviar conexão algumas ou todas as mensagens de solicitação que ele recebe do servidores de acesso à rede e proxies NPS ou outros proxies RADIUS.

Você pode usar o NPS como um proxy RADIUS para encaminhar conexão solicita a um ou mais remotos grupos de servidores RADIUS e cada grupo podem conter um ou mais servidores RADIUS. Quando você quiser que o proxy NPS para encaminhar mensagens para vários grupos, configure uma política de solicitação de conexão por grupo. A política de solicitação de conexão contém informações adicionais, como regras de manipulação de atributo, que informam o proxy NPS quais as mensagens enviadas para o grupo de servidores remotos RADIUS especificado na diretiva.

Você pode configurar grupos de servidores RADIUS remotos usando os comandos Netsh para NPS, configurando grupos diretamente no snap-in NPS em grupos de servidores RADIUS remotos ou executando o Assistente de nova diretiva de solicitação de Conexão.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para grupos de servidores RADIUS remotos, você pode usar as etapas a seguir.

- Determine os domínios que contêm os servidores RADIUS ao qual você deseja que o proxy NPS para encaminhar as solicitações de conexão. Esses domínios contêm as contas de usuário para os usuários que se conectam à rede por meio de clientes RADIUS que implantar.

- Determine se você precisa adicionar novos servidores RADIUS em domínios onde RADIUS já não estiver implantado.

- Documente os endereços IP dos servidores RADIUS que você deseja adicionar a grupos de servidores remotos RADIUS.

- Determine quantos grupos de servidores RADIUS remotos você precisa criar. Em alguns casos, é melhor criar um grupo de servidores RADIUS remotos por domínio e, em seguida, adicione os servidores RADIUS para o domínio ao grupo. No entanto, pode haver casos em que você tem uma grande quantidade de recursos em um domínio, incluindo um grande número de usuários com contas de usuário em um grande número de servidores RADIUS, um grande número de controladores de domínio e o domínio. Ou seu domínio pode abranger uma grande área geográfica, fazendo com que você tenha servidores de acesso à rede e servidores RADIUS em locais que estão distantes uns dos outros. Nestes e possivelmente outros casos, você pode criar vários grupos de servidores RADIUS remotos por domínio.

- Crie segredos compartilhados para a configuração no proxy NPS e nos servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planeje regras de manipulação de atributo de encaminhamento de mensagem

Regras de manipulação de atributo, que são configuradas em políticas de solicitação de conexão, permitem que você identificar as mensagens de solicitação de acesso que você deseja encaminhar para um grupo de servidores remotos RADIUS específico.

Você pode configurar o NPS para encaminhar todas as solicitações de conexão para um grupo de servidores remotos RADIUS sem usar as regras de manipulação de atributo.

Se você tiver mais de um local para o qual você deseja encaminhar as solicitações de conexão, no entanto, você deve criar uma política de solicitação de conexão para cada local e configurar a política com o grupo de servidores remotos RADIUS à qual você deseja encaminhar mensagens, assim como com as regras de manipulação de atributos que informam NPS quais mensagens para encaminhar.

Você pode criar regras para os seguintes atributos.

- Chamado-estação-ID. O número de telefone do servidor de acesso à rede (NAS). O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar códigos de área.

- Chamada-estação-ID. O número de telefone usado pelo chamador. O valor desse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de padrões coincidentes para especificar códigos de área.

- Nome do usuário. O nome de usuário que é fornecido pelo cliente de acesso e que está incluído por na mensagem de solicitação de acesso RADIUS. O valor desse atributo é uma cadeia de caracteres que normalmente contém um nome de território e um nome de conta de usuário.

Para substituir ou converter nomes de territórios no nome do usuário de uma solicitação de conexão corretamente, você deve configurar as regras de manipulação de atributo para o atributo de nome de usuário na diretiva de solicitação de conexão apropriada.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para regras de manipulação de atributo, você pode usar as etapas a seguir.

- Planeje o roteamento de mensagens de por meio do proxy para os servidores RADIUS remotos para verificar se você tem um caminho lógico com o qual deseja encaminhar mensagens para os servidores RADIUS.

- Determine um ou mais atributos que você deseja usar para cada política de solicitação de conexão.

- Documentar as regras de manipulação de atributos que você pretende usar para cada política de solicitação de conexão e correspondem às regras para o grupo de servidores remotos RADIUS ao qual as mensagens são encaminhadas.

## <a name="plan-connection-request-policies"></a>Planeje as políticas de solicitação de conexão

A política de solicitação de conexão padrão está configurada para NPS quando ele é usado como um servidor RADIUS. Políticas de solicitação de conexão adicionais podem ser usadas para definir condições mais específicas, criar regras de manipulação que informam NPS qual mensagens para encaminhar mensagens para grupos de servidores RADIUS remotos e para especificar atributos avançados de atributo. Use o Assistente de nova diretiva de solicitação de Conexão para criar políticas de solicitação de conexão comuns ou personalizada.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento para políticas de solicitação de conexão, você pode usar as etapas a seguir.

- Exclua a diretiva de solicitação de conexão padrão em cada servidor que executa o NPS que funciona exclusivamente como um proxy RADIUS.

- Planeje condições adicionais e configurações que são necessárias para cada política, combinando essas informações com o grupo de servidores remotos RADIUS e as regras de manipulação de atributo planejadas para a política.

- Projete o plano para distribuir políticas de solicitação de conexão comuns para todos os proxies NPS. Criar políticas comuns a vários proxies NPS em um servidor NPS e, em seguida, use os comandos Netsh para NPS para importar a configuração de políticas e o servidor da solicitação de conexão em todos os outros proxies.

## <a name="plan-nps-accounting"></a>Planejar Contabilização de NPS

Quando você configura o NPS como um proxy RADIUS, você pode configurá-la para executar RADIUS accounting usando NPS SQL Server, arquivos de log do formato compatível com o banco de dados ou arquivos de log de formato NPS registro em log.

Você também pode encaminhar mensagens de contabilidade para um grupo de servidores remotos RADIUS que realiza a contagem usando um desses formatos de registro em log.

### <a name="key-steps"></a>Etapas principais

Durante o planejamento de contabilização de NPS, você pode usar as etapas a seguir.

- Determine se você deseja que o proxy NPS para executar serviços de contabilidade ou para encaminhar mensagens de contabilização de um grupo de servidores remotos RADIUS de contabilidade.

- Planeje desabilitar considerando se você pretende encaminhar mensagens de estatísticas para outros servidores de proxy NPS local.

- Planeje as etapas de configuração de política de solicitação de conexão se você pretende encaminhar mensagens de estatísticas para outros servidores. Se você desabilitar contabilidade local para o proxy NPS, cada política de solicitação de conexão que você configurar nesse proxy deve ter o encaminhamento de mensagem contabilidade habilitado e configurado corretamente.

- Determinar o formato de log que você deseja usar: arquivos de log de formato IAS, arquivos de log do formato compatível com o banco de dados ou registro em log NPS SQL Server.

Para configurar o balanceamento de carga para NPS como um proxy RADIUS, consulte [NPS Proxy Server balanceamento de carga de](nps-manage-proxy-lb.md).

