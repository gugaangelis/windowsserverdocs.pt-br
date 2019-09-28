---
title: NPS (Servidor de Políticas de Rede)
description: Este tópico fornece uma visão geral do servidor de políticas de rede no Windows Server 2016 e no Windows Server 2019 e inclui links para diretrizes adicionais sobre o NPS.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 5685d4dae742245c131ff2fee3114e905e94e9bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396011"
---
# <a name="network-policy-server-nps"></a>NPS (Servidor de Políticas de Rede)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2019

Você pode usar este tópico para obter uma visão geral do servidor de políticas de rede no Windows Server 2016 e no Windows Server 2019. O NPS é instalado quando você instala o recurso de serviços de acesso e política de rede (NPAS) no Windows Server 2016 e no Server 2019.

> [!NOTE]
> Além deste tópico, a seguinte documentação do NPS está disponível.
> - [Práticas recomendadas do servidor de políticas de rede](nps-best-practices.md)
> - [Introdução com o servidor de políticas de rede](nps-getstart-top.md)
> - [Planejar servidor de políticas de rede](nps-plan-top.md)
> - [Implantar servidor de políticas de rede](nps-deploy.md)
> - [Gerenciar servidor de políticas de rede](nps-manage-top.md)
> - [Cmdlets do servidor de diretivas de rede (NPS) no Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2016 e Windows 10
> - [Cmdlets do servidor de diretivas de rede (NPS) no Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2012 R2 e Windows 8.1
> - [Cmdlets do NPS no Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2012 e Windows 8

O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão.

Você também pode configurar o NPS como um proxy de serviço RADIUS (RADIUS) para encaminhar solicitações de conexão para um NPS remoto ou outro servidor RADIUS para que você possa balancear a carga de solicitações de conexão e encaminhá-las ao domínio correto para autenticação e autorização.

O NPS permite que você configure e gerencie centralmente a autenticação, autorização e contabilização de acesso à rede com os seguintes recursos:

- **Servidor RADIUS**. O NPS executa autenticação centralizada, autorização e contabilidade para conexões sem fio, comutador de autenticação, dial-up de acesso remoto e VPN (rede virtual privada). Ao usar o NPS como servidor RADIUS, você configura servidores de acesso à rede (como pontos de acesso sem fio e servidores VPN) como clientes RADIUS no NPS. Você também configura as políticas de rede que o NPS usa para autorizar solicitações de conexão. Você pode configurar a contabilização RADIUS para que o NPS registre as informações de contabilização em arquivos de log no disco rígido local ou em um banco de dados do Microsoft SQL Server. Para obter mais informações, consulte [servidor RADIUS](#radius-server).
- **Proxy RADIUS**. Ao usar o NPS como um proxy RADIUS, você configura as políticas de solicitação de conexão que dizem ao NPS quais solicitações de conexão devem ser encaminhadas para outros servidores RADIUS e para quais servidores RADIUS você deseja encaminhar as solicitações de conexão. Também é possível configurar o NPS para que encaminhe dados de estatística a serem registrados por um ou mais computadores de um grupo de servidores RADIUS remotos. Para configurar o NPS como um servidor proxy RADIUS, consulte os tópicos a seguir. Para obter mais informações, consulte [proxy RADIUS](#radius-proxy).
    - [Configurar políticas de solicitação de conexão](nps-crp-configure.md)
- **Contabilização RADIUS**. Você pode configurar o NPS para registrar eventos em um arquivo de log local ou em uma instância local ou remota do Microsoft SQL Server. Para obter mais informações, consulte [log do NPS](#nps-logging).

> [!IMPORTANT]
> A proteção de acesso à rede \(NAP @ no__t-1, a autoridade de registro de integridade \(HRA @ no__t-3 e o protocolo de autorização de credenciais de host \(HCAP @ no__t-5 foram preteridos no Windows Server 2012 R2 e não estão disponíveis no Windows Server 2016. Se você tiver uma implantação de NAP usando sistemas operacionais anteriores ao Windows Server 2016, não será possível migrar sua implantação de NAP para o Windows Server 2016.

Você pode configurar o NPS com qualquer combinação desses recursos. Por exemplo, você pode configurar um NPS como um servidor RADIUS para conexões VPN e também como um proxy RADIUS para encaminhar algumas solicitações de conexão para membros de um grupo de servidores remotos RADIUS para autenticação e autorização em outro domínio.

## <a name="windows-server-editions-and-nps"></a>Edições do Windows Server e NPS

O NPS fornece uma funcionalidade diferente, dependendo da edição do Windows Server que você instalar.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 ou Windows Server 2019 Standard/Datacenter Edition

Com o NPS no Windows Server 2016 Standard ou Datacenter, você pode configurar um número ilimitado de clientes RADIUS e grupos de servidores RADIUS remotos. Além disso, você pode configurar clientes RADIUS especificando um intervalo de endereços IP.

> [!NOTE]
> O recurso serviços de acesso e política de rede do WIndows não está disponível em sistemas instalados com uma opção de instalação Server Core.

As seções a seguir fornecem informações mais detalhadas sobre o NPS como um servidor RADIUS e proxy.

## <a name="radius-server-and-proxy"></a>Servidor RADIUS e proxy

Você pode usar o NPS como um servidor RADIUS, um proxy RADIUS ou ambos.

### <a name="radius-server"></a>Servidor RADIUS

O NPS é a implementação da Microsoft do padrão RADIUS especificado pela Internet Engineering Task Force \(IETF @ no__t-1 nas RFCs 2865 e 2866. Como um servidor RADIUS, o NPS executa autenticação de conexão centralizada, autorização e contabilidade para muitos tipos de acesso à rede, incluindo sem fio, comutador de autenticação, dial-up e rede privada virtual \(VPN @ no__t-1 acesso remoto e conexões de roteador para roteador.

> [!NOTE]
> Para obter informações sobre como implantar o NPS como um servidor RADIUS, consulte [implantar o servidor de políticas de rede](nps-deploy.md).

O NPS permite o uso de um conjunto heterogêneo de equipamentos sem fio, de comutador, de acesso remoto ou de VPN. Você pode usar o NPS com o serviço de acesso remoto, que está disponível no Windows Server 2016.

O NPS usa um domínio Active Directory Domain Services \(AD DS @ no__t-1 ou o banco de dados de contas de usuário SAM (Gerenciador de contas de segurança) local para autenticar credenciais de usuário para tentativas de conexão. Quando um servidor que executa o NPS é membro de um AD DS domínio, o NPS usa o serviço de diretório como seu banco de dados de conta de usuário e faz parte de uma solução de logon único. O mesmo conjunto de credenciais é usado para o controle de acesso à rede \(authenticating e autorizar o acesso a uma rede @ no__t-1 e fazer logon em um domínio AD DS.

> [!NOTE]
> O NPS usa as propriedades de discagem da conta de usuário e das políticas de rede para autorizar uma conexão.

Os provedores de serviços de Internet \(ISPs @ no__t-1 e organizações que mantêm o acesso à rede têm o maior desafio de gerenciar todos os tipos de acesso à rede a partir de um único ponto de administração, independentemente do tipo de equipamento de acesso à rede usado. O padrão RADIUS oferece suporte a essa funcionalidade em ambientes homogêneos e heterogêneos. O RADIUS é um protocolo cliente-servidor que permite que o equipamento de acesso à rede (usado como clientes RADIUS) Envie solicitações de autenticação e contabilização para um servidor RADIUS.

Um servidor RADIUS tem acesso às informações da conta de usuário e pode verificar as credenciais de autenticação de acesso à rede. Se as credenciais do usuário forem autenticadas e a tentativa de conexão for autorizada, o servidor RADIUS autorizará o acesso do usuário com base nas condições especificadas e, em seguida, registrará a conexão de acesso à rede em um log de estatísticas. O uso do RADIUS permite que os dados de autenticação, autorização e contabilização do usuário de acesso à rede sejam coletados e mantidos em um local central, e não em cada servidor de acesso.

#### <a name="using-nps-as-a-radius-server"></a>Usando o NPS como um servidor RADIUS

Você pode usar o NPS como um servidor RADIUS quando:

- Você está usando um domínio AD DS ou o banco de dados de contas de usuário SAM local como seu banco de dados de conta de usuário para clientes de acesso. 
- Você está usando o acesso remoto em vários servidores de conexão discada, servidores VPN ou roteadores de discagem por demanda e deseja centralizar a configuração de políticas de rede e registro em log de conexão e contabilidade. 
- Você está terceirizando seu acesso dial-up, VPN ou sem fio a um provedor de serviços. Os servidores de acesso usam o RADIUS para autenticar e autorizar conexões feitas por membros da sua organização.
- Você deseja centralizar a autenticação, a autorização e a contabilidade para um conjunto heterogêneo de servidores de acesso.

A ilustração a seguir mostra o NPS como um servidor RADIUS para uma variedade de clientes de acesso. 

![NPS como um servidor RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Como um proxy RADIUS, o NPS encaminha as mensagens de autenticação e contabilização para o NPS e outros servidores RADIUS. Você pode usar o NPS como um proxy RADIUS para fornecer o roteamento de mensagens RADIUS entre clientes RADIUS \(also chamado servidores de acesso à rede @ no__t-1 e servidores RADIUS que executam autenticação de usuário, autorização e contabilidade para a tentativa de conexão. 

Quando usado como um proxy RADIUS, o NPS é um ponto de roteamento ou comutador central por meio do qual o acesso RADIUS e as mensagens de contabilização fluem. O NPS registra informações em um log de contabilidade sobre as mensagens que são encaminhadas.

#### <a name="using-nps-as-a-radius-proxy"></a>Usando o NPS como um proxy RADIUS

Você pode usar o NPS como um proxy RADIUS quando:

- Você é um provedor de serviços que oferece serviços de acesso à rede dial-up, VPN ou sem fio terceirizados a vários clientes. Seus NASs enviam solicitações de conexão para o proxy RADIUS do NPS. Com base na parte do realm do nome de usuário na solicitação de conexão, o proxy RADIUS do NPS encaminha a solicitação de conexão para um servidor RADIUS mantido pelo cliente e pode autenticar e autorizar a tentativa de conexão.
- Você deseja fornecer autenticação e autorização para contas de usuário que não são membros do domínio no qual o NPS é um membro ou outro domínio que tenha uma relação de confiança bidirecional com o domínio no qual o NPS é membro. Isso inclui contas em domínios não confiáveis, domínios confiáveis unidirecionais e outras florestas. Em vez de configurar seus servidores de acesso para enviar suas solicitações de conexão para um servidor RADIUS NPS, você pode configurá-los para enviar suas solicitações de conexão para um proxy RADIUS NPS. O proxy RADIUS do NPS usa a parte do nome de realm do nome de usuário e encaminha a solicitação para um NPS no domínio ou floresta corretos. As tentativas de conexão para contas de usuário em um domínio ou floresta podem ser autenticadas para NASs em outro domínio ou floresta.
- Você deseja executar a autenticação e a autorização usando um banco de dados que não seja um banco de dados de conta do Windows. Nesse caso, as solicitações de conexão que correspondem a um nome de realm especificado são encaminhadas para um servidor RADIUS, que tem acesso a um banco de dados diferente de contas de usuário e de data de autorização. Exemplos de outros bancos de dados de usuário incluem os bancos de dados do Novell Directory Services (NDS) e linguagem SQL (SQL).
- Você deseja processar um grande número de solicitações de conexão. Nesse caso, em vez de configurar seus clientes RADIUS para tentar balancear a conexão e as solicitações de contabilização entre vários servidores RADIUS, você pode configurá-los para enviar suas solicitações de conexão e de contabilização para um proxy RADIUS NPS. O proxy RADIUS do NPS equilibra dinamicamente a carga de solicitações de conexão e de contabilização entre vários servidores RADIUS e aumenta o processamento de um grande número de clientes RADIUS e autenticações por segundo.
- Você deseja fornecer autenticação e autorização RADIUS para provedores de serviço terceirizados e minimizar a configuração de firewall da intranet. Um firewall de intranet está entre a rede de perímetro (a rede entre a intranet e a Internet) e a intranet. Ao colocar um NPS em sua rede de perímetro, o firewall entre a rede de perímetro e a intranet deve permitir que o tráfego flua entre o NPS e vários controladores de domínio. Ao substituir o NPS por um proxy NPS, o firewall deve permitir que somente o tráfego RADIUS flua entre o proxy NPS e um ou vários NPSs em sua intranet.

A ilustração a seguir mostra o NPS como um proxy RADIUS entre clientes RADIUS e servidores RADIUS.

![NPS como um proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)

Com o NPS, as organizações também podem terceirizar a infraestrutura de acesso remoto para um provedor de serviços e, ao mesmo tempo, manter o controle sobre a autenticação do usuário, autorização e contabilidade.

As configurações de NPS podem ser criadas para os seguintes cenários:

- Acesso sem fio
- Acesso remoto de rede virtual privada (VPN) ou dial-up da organização
- Dial-up ou acesso sem fio terceirizado
- Acesso à rede e à Internet
- Acesso autenticado a recursos de extranet para parceiros de negócios

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Exemplos de configuração de proxy RADIUS e servidor RADIUS

Os exemplos de configuração a seguir demonstram como você pode configurar o NPS como um servidor RADIUS e um proxy RADIUS.

**NPS como um servidor RADIUS**. Neste exemplo, o NPS é configurado como um servidor RADIUS, a diretiva de solicitação de conexão padrão é a única política configurada e todas as solicitações de conexão são processadas pelo NPS local. O NPS pode autenticar e autorizar usuários cujas contas estejam no domínio do NPS e em domínios confiáveis.

**NPS como um proxy RADIUS**. Neste exemplo, o NPS é configurado como um proxy RADIUS que encaminha solicitações de conexão para grupos de servidores remotos RADIUS em dois domínios não confiáveis. A política de solicitação de conexão padrão é excluída e duas novas políticas de solicitação de conexão são criadas para encaminhar solicitações para cada um dos dois domínios não confiáveis. Neste exemplo, o NPS não processa nenhuma solicitação de conexão no servidor local. 

**NPS como servidor RADIUS e proxy RADIUS**. Além da diretiva de solicitação de conexão padrão, que designa que as solicitações de conexão são processadas localmente, é criada uma nova política de solicitação de conexão que encaminha as solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Essa segunda política é chamada de política de proxy. Neste exemplo, a política de proxy aparece primeiro na lista ordenada de políticas. Se a solicitação de conexão corresponder à política de proxy, a solicitação de conexão será encaminhada para o servidor RADIUS no grupo de servidores remotos RADIUS. Se a solicitação de conexão não corresponder à política de proxy, mas corresponder à política de solicitação de conexão padrão, o NPS processará a solicitação de conexão no servidor local. Se a solicitação de conexão não corresponder a nenhuma política, ela será descartada.

**NPS como um servidor RADIUS com servidores de contabilidade remoto**. Neste exemplo, o NPS local não está configurado para executar a contabilidade e a política de solicitação de conexão padrão é revisada para que as mensagens de contabilização RADIUS sejam encaminhadas para um NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS. Embora as mensagens de contabilização sejam encaminhadas, as mensagens de autenticação e autorização não são encaminhadas, e o NPS local executa essas funções para o domínio local e todos os domínios confiáveis.

**NPS com RADIUS remoto para o mapeamento de usuários do Windows**. Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy RADIUS para cada solicitação de conexão individual encaminhando a solicitação de autenticação para um servidor RADIUS remoto ao usar uma conta de usuário do Windows local para autorização. Essa configuração é implementada com a configuração do atributo Remote RADIUS para o mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. \(In, uma conta de usuário deve ser criada localmente no servidor RADIUS que tem o mesmo nome que a conta de usuário remoto na qual a autenticação é executada pelo servidor RADIUS remoto. \)

## <a name="configuration"></a>Configuração

Para configurar o NPS como um servidor RADIUS, você pode usar a configuração padrão ou a configuração avançada no console do NPS ou no Gerenciador do Servidor. Para configurar o NPS como um proxy RADIUS, você deve usar a configuração avançada.

### <a name="standard-configuration"></a>Configuração padrão

Com a configuração padrão, os assistentes são fornecidos para ajudá-lo a configurar o NPS para os seguintes cenários:

- Servidor RADIUS para conexões dial-up ou VPN
- Servidor RADIUS para conexões 802.1X com ou sem fio

Para configurar o NPS usando um assistente, abra o console do NPS, selecione um dos cenários anteriores e clique no link que abre o assistente.

### <a name="advanced-configuration"></a>Configuração avançada

Ao usar a configuração avançada, você configura manualmente o NPS como um servidor RADIUS ou proxy RADIUS. 

Para configurar o NPS usando a configuração avançada, abra o console do NPS e clique na seta ao lado de **Configuração avançada** para expandir esta seção.

Os seguintes itens de configuração avançada são fornecidos.

#### <a name="configure-radius-server"></a>Configurar servidor RADIUS

Para configurar o NPS como um servidor RADIUS, você deve configurar clientes RADIUS, a política de rede e a contabilização RADIUS.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar políticas de rede](nps-np-configure.md)
- [Configurar a contabilização do servidor de políticas de rede](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar proxy RADIUS

Para configurar o NPS como um proxy RADIUS, você deve configurar clientes RADIUS, grupos de servidores remotos RADIUS e políticas de solicitação de conexão.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)
- [Configurar políticas de solicitação de conexão](nps-crp-configure.md)

## <a name="nps-logging"></a>Log do NPS

O log do NPS também é chamado de contabilidade RADIUS. Configure o log do NPS para seus requisitos se o NPS for usado como um servidor RADIUS, proxy ou qualquer combinação dessas configurações.

Para configurar o log do NPS, você deve configurar os eventos que deseja que sejam registrados e exibidos com Visualizador de Eventos e, em seguida, determinar quais outras informações deseja registrar em log. Além disso, você deve decidir se deseja registrar em log as informações de autenticação e estatísticas de usuário em arquivos de log de texto armazenados no computador local ou em um banco de dados SQL Server no computador local ou em um computador remoto.

Para obter mais informações, consulte [Configure Network Policy Server Accounting](nps-accounting-configure.md).
