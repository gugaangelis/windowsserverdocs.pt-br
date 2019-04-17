---
title: Servidor de política de rede (NPS)
description: Este tópico fornece uma visão geral do servidor de política de rede no Windows Server 2016 e inclui links para orientação adicional sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>Servidor de política de rede (NPS)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do servidor de política de rede no Windows Server 2016.

>[!NOTE]
>Além de neste tópico, a seguinte documentação de NPS está disponível.
> - [Práticas recomendadas de servidor de política de rede](nps-best-practices.md)
> - [Introdução ao servidor de políticas de rede](nps-getstart-top.md)
> - [Planeje o servidor de políticas de rede](nps-plan-top.md)
> - [Implantar o servidor de políticas de rede](nps-deploy.md)
> - [Gerenciar o servidor de políticas de rede](nps-manage-top.md)
> - [Cmdlets NPS (servidor) da política de rede](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2016 e Windows 10
> - [Cmdlets da diretiva de NPS (servidor) no Windows PowerShell de rede](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 R2 e Windows 8.1
> - [NPS Cmdlets do Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 e Windows 8

Servidor de política de rede (NPS) permite que você criar e aplicar políticas de acesso de rede em toda a organização para autenticação de solicitação de conexão e autorização.

Você também pode configurar o NPS como um proxy de autenticação discagem usuário serviço RADIUS (Remote) para encaminhar as solicitações de conexão para um NPS remoto ou outro servidor RADIUS para que você pode carregar o saldo solicitações de conexão e encaminhá-las ao domínio correto para autenticação e autorização.

NPS permite que você centralmente a configurar e gerenciar autenticação de acesso de rede, autorização e contabilidade com os seguintes recursos:

- **Servidor RADIUS**. NPS realiza autenticação centralizada, autorização e estatísticas para sem fio, autenticação switch, acesso remoto dial-up e conexões de rede virtual privada (VPN). Quando você usa o NPS como um servidor RADIUS, configurar servidores de acesso à rede, como pontos de acesso sem fio e servidores VPN, como clientes RADIUS no NPS. Você também configurar políticas de rede NPS usa para autorizar solicitações de conexão, e você pode configurar estatísticas RADIUS para que o NPS registra informações de estatísticas para registrar arquivos no disco rígido local ou em um banco de dados do Microsoft SQL Server. Para obter mais informações, consulte [servidor RADIUS](#bkmk_server).
- **Proxy RADIUS**. Quando você usa o NPS como um proxy RADIUS, você pode configurar políticas de solicitação de conexão que informam ao servidor NPS qual conexão solicita para encaminhar para outros servidores RADIUS e quais servidores RADIUS você deseja encaminhar as solicitações de conexão. Você também pode configurar o NPS para encaminhar os dados contábeis deve ser registrada por um ou mais computadores em um grupo de servidores remotos RADIUS. Para configurar o NPS como um servidor proxy RADIUS, consulte os tópicos a seguir. Para obter mais informações, consulte [proxy RADIUS](#bkmk_proxy).
    - [Configurar as políticas de solicitação de Conexão](nps-crp-configure.md)
- **Estatísticas RADIUS**. Você pode configurar o NPS para registrar eventos para um arquivo de log local ou para uma instância local ou remota do Microsoft SQL Server. Para obter mais informações, consulte [registro em log NPS](#bkmk_logging).

>[!IMPORTANT]
>Rede proteção de acesso à \(NAP\), \(HRA\) autoridade de registro de saúde e protocolo de autorização de credencial Host \(HCAP\) foram preteridas no Windows Server 2012 R2 e não estão disponíveis no Windows Server 2016. Se você tiver uma implantação NAP usando sistemas operacionais anteriores ao Windows Server 2016, você não pode migrar sua implantação NAP para Windows Server 2016.

Você pode configurar o NPS com qualquer combinação desses recursos. Por exemplo, você pode configurar um servidor NPS como um servidor RADIUS para conexões VPN e também como um proxy RADIUS para encaminhar algumas solicitações de conexão para membros de um grupo de servidores remotos RADIUS para autenticação e autorização em outro domínio.

## <a name="windows-server-editions-and-nps"></a>NPS e as edições do Windows Server

NPS fornece uma funcionalidade diferente dependendo da edição do Windows Server que você instalar.

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server 2016 Datacenter Edition

Com o NPS no Windows Server 2016 Datacenter, você pode configurar um número ilimitado de clientes RADIUS e grupos de servidores remotos RADIUS. Além disso, você pode configurar clientes RADIUS especificando um intervalo de endereços IP.

### <a name="windows-server-2016-standard-edition"></a>Windows Server 2016 Standard Edition

Com o NPS no Windows Server 2016 Standard, você pode configurar um máximo, 50 clientes RADIUS e 2 grupos de servidores remotos RADIUS. Você pode definir um cliente RADIUS usando um nome de domínio totalmente qualificado ou um endereço IP, mas não é possível definir grupos de clientes RADIUS especificando um intervalo de endereços IP. Se o nome de domínio totalmente qualificado de um cliente RADIUS é resolvido para vários endereços IP, o servidor NPS usa o primeiro endereço IP retornado na consulta de sistema de nome de domínio (DNS).

As seções a seguir fornecem informações mais detalhadas sobre NPS como um servidor e proxy RADIUS.

## <a name="radius-server-and-proxy"></a>Servidor e proxy RADIUS

Você pode usar o NPS como um servidor RADIUS, um proxy RADIUS ou ambos.

### <a name="bkmk_server"></a>Servidor RADIUS

NPS é a implementação Microsoft do RAIO padrão especificada pela \(IETF\) a Internet Engineering Task Force no RFCs 2865 e 2866. Como um servidor RADIUS, o NPS realiza autenticação de conexão centralizada, autorização e estatísticas para muitos tipos de acesso à rede, incluindo acesso sem fio, chave, dial-up de autenticação e acesso remoto de \(VPN\) de rede virtual privada e conexões de roteador para roteador.

>[!NOTE]
>Para obter informações sobre como implantar o NPS como um servidor RADIUS, consulte [servidor de políticas de rede implantar](nps-deploy.md).

NPS permite o uso de um conjunto heterogêneo de acesso sem fio, switch, acesso remoto ou equipamento de VPN. Você pode usar o NPS com o serviço de acesso remoto, que está disponível no Windows Server 2016.

NPS usa um domínio do Active Directory Domain Services \(AD DS\) ou tentativas de Gerenciador de contas de segurança (SAM) usuário contas banco de dados local para autenticar as credenciais do usuário para conexão. Quando um servidor NPS é um membro de um domínio do AD DS, o NPS usa o serviço de diretório como seu banco de dados de conta de usuário e faz parte de uma solução. O mesmo conjunto de credenciais é usado para o controle de acesso de rede \ (autenticar e autorizar o acesso a uma rede \) e para fazer logon em um domínio do AD DS.

>[!NOTE]
>NPS usa as propriedades de discagem rápida das políticas de rede e de conta de usuário para autorizar uma conexão.

Internet serviço provedores \(ISPs\) e as organizações que mantêm o acesso à rede têm o maior desafio de gerenciamento de todos os tipos de acesso à rede de um único ponto de administração, independentemente do tipo de equipamento de acesso de rede usado. O padrão RADIUS dá suporte a essa funcionalidade em ambientes homogêneas e heterogêneos. RADIUS é um protocolo de cliente-servidor que permite que o equipamento de acesso de rede (usado como clientes RADIUS) para enviar solicitações de autenticação e estatísticas para um servidor RADIUS.

Um servidor RADIUS tem acesso às informações de conta de usuário e pode verificar as credenciais de autenticação de acesso de rede. Se as credenciais do usuário são autenticadas, e está autorizada a tentativa de conexão, o servidor RADIUS autoriza o acesso de usuário com base nas condições especificadas e, em seguida, registra a conexão de acesso de rede em um log de contabilidade. O uso de RAIO permite a autenticação do usuário de acesso de rede, autorização e dados de contabilidade para ser coletadas e mantidas em um local central, em vez de em cada servidor de acesso.

#### <a name="using-nps-as-a-radius-server"></a>Usando o NPS como um servidor RADIUS

Você pode usar o NPS como um servidor RADIUS quando:

- Você está usando um domínio do AD DS ou o banco de dados de contas de usuário SAM local como seu banco de dados de conta de usuário para clientes de acesso. 
- Você estiver usando o acesso remoto em vários servidores de dial-up, servidores VPN, ou roteadores de discagem e você quer centralizar a configuração de políticas de rede e conexão log e estatísticas. 
- Você está terceirizando seu dial-up, VPN ou acesso sem fio a um provedor de serviços. Os servidores de acesso usam RADIUS para autenticar e autorizar conexões feitas por membros de sua organização.
- Você quer centralizar a autenticação, autorização e estatísticas para um conjunto heterogênea de servidores de acesso.

A ilustração a seguir mostra o NPS como um servidor RADIUS para uma variedade de clientes de acesso. 

![NPS como um servidor RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>Proxy RADIUS

Como um proxy RADIUS, o NPS encaminha autenticação e mensagens de contabilização NPS e outros servidores RADIUS. Você pode usar o NPS como um proxy RADIUS para fornecer o roteamento de RAIO mensagens entre clientes RADIUS \ (também chamado de servers\ de acesso de rede) e servidores RADIUS que executam a autenticação do usuário, autorização e estatísticas para a tentativa de conexão. 

Quando usado como um proxy RADIUS, o NPS é um ponto roteamento pelo qual fluem de mensagens de acesso e estatísticas de RAIO ou a central de alternância. NPS registra as informações no log de estatísticas sobre as mensagens que são encaminhados.

#### <a name="using-nps-as-a-radius-proxy"></a>Usando o NPS como um proxy RADIUS

Você pode usar o NPS como um proxy RADIUS quando:

- Você é um provedor de serviço que oferece acesso dial-up terceirizado, VPN ou serviços de acesso de rede sem fio para diversos clientes. Seus NASs enviam solicitações de conexão para o proxy RADIUS de NPS. Com base na parte do território do nome do usuário na solicitação de conexão, o proxy RADIUS de NPS encaminha a solicitação de conexão para um servidor RADIUS que é mantido pelo cliente e pode autenticar e autorizar a tentativa de conexão.
- Você deseja fornecer autenticação e autorização para contas de usuário que não sejam membros do domínio no qual o servidor NPS é um membro ou de outro domínio que tenha uma relação de confiança bidirecional com o domínio em que o servidor NPS é um membro. Isso inclui contas em domínios não confiáveis, domínios confiáveis unidirecionais e outras florestas. Em vez de configurar os servidores de acesso para enviar suas solicitações de conexão a um servidor RADIUS de NPS, você pode configurá-los para enviar suas solicitações de conexão para um proxy RADIUS de NPS. O proxy RADIUS de NPS usa a parte do nome do território do nome do usuário e encaminha a solicitação para um servidor NPS no domínio correto ou floresta. Tentativas de Conexão de usuário contas em um domínio ou floresta podem ser autenticadas para NASs em outro domínio ou floresta.
- Você deseja executar autenticação e autorização usando um banco de dados que não é um banco de dados de conta do Windows. Nesse caso, as solicitações de conexão que correspondem a um nome de domínio especificado são encaminhadas para um servidor RADIUS, que tem acesso a um banco de dados diferente de contas de usuário e dados de autorização. Exemplos de outros bancos de dados do usuário incluem bancos de dados de serviços de diretório Novell (NDS) e linguagem de consulta estruturada (SQL).
- Você deseja processar um grande número de solicitações de conexão. Nesse caso, em vez de configurar seus clientes RADIUS para tentar equilibrar suas solicitações de estatísticas e conexão por diversos servidores RADIUS, você pode configurá-los para enviar suas solicitações de estatísticas e conexão a um proxy RADIUS de NPS. O proxy RADIUS de NPS equilibra a carga de conexão e contabilidade solicitações por diversos servidores RADIUS e aumenta o processamento de um grande número de clientes RADIUS e autenticações por segundo.
- Você deseja fornecer autenticação e autorização para provedores de serviço terceirizado RADIUS e minimizar a configuração de firewall da intranet. Um firewall de intranet está entre o perímetro (a rede entre a intranet e a Internet) e da intranet. Colocando um servidor NPS em sua rede de perímetro, o firewall entre a rede do perímetro e a intranet deve permitir o tráfego a fluir entre o servidor NPS e vários controladores de domínio. Substituindo o servidor NPS por um proxy de NPS, o firewall deve permitir que apenas o tráfego RADIUS flua entre o proxy NPS e um ou vários servidores NPS dentro de sua intranet.

A ilustração a seguir mostra o NPS como um proxy RADIUS entre clientes RADIUS e servidores RADIUS.

![NPS como um Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Com o NPS, as organizações podem também terceirizar infraestrutura de acesso remoto a um provedor de serviço, mantendo o controle sobre a autenticação do usuário, autorização e estatísticas.

Configurações de NPS podem ser criadas para os seguintes cenários:

- Acesso sem fio
- Acesso remoto de dial-up ou VPN (rede privada) de organização
- Terceirizado dial-up ou acesso sem fio
- Acesso à Internet
- Acesso autenticado aos recursos extranet para parceiros de negócios

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Servidor RADIUS e exemplos de configuração de proxy RADIUS

Os exemplos de configuração a seguir demonstram como você pode configurar o NPS como um servidor RADIUS e um proxy RADIUS.

**NPS como um servidor RADIUS**. Neste exemplo, NPS está configurado como um servidor RADIUS, a política de solicitação de conexão padrão é a única diretiva configurada e todas as solicitações de conexão são processadas pelo servidor NPS local. O servidor NPS pode autenticar e autorizar usuários cujas contas estão no domínio do servidor NPS e em domínios confiáveis.

**NPS como um proxy RADIUS**. Neste exemplo, o servidor NPS está configurado como um proxy RADIUS que encaminha as solicitações de conexão para grupos de servidores remotos RADIUS em dois domínios não confiáveis. A diretiva de solicitação de conexão padrão é excluída e duas novas políticas de solicitação de conexão são criadas para encaminhar as solicitações para cada um dos dois domínios não confiáveis. Neste exemplo, o NPS não processa todas as solicitações de conexão no servidor local. 

**NPS como servidor RADIUS e proxy RADIUS**. Além da política de solicitação de conexão padrão, que designa que as solicitações de conexão são processadas localmente, uma nova política de solicitação de conexão é criada para encaminhar as solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Essa segunda diretiva é chamada a política de Proxy. Neste exemplo, a política de Proxy será exibida primeira na lista ordenada de políticas. Se a solicitação de conexão corresponder a política de Proxy, a solicitação de conexão é encaminhada para o servidor RADIUS no grupo de servidores remotos RADIUS. Se a solicitação de conexão não coincide com a política de Proxy, mas coincide com a política de solicitação de conexão padrão, o NPS processará a solicitação de conexão no servidor local. Se a solicitação de conexão não coincide com qualquer política, ele será descartado.

**NPS como um servidor RADIUS com servidores remotos de contabilidade**. Neste exemplo, o servidor NPS local não está configurado para realizar as estatísticas e a diretiva de solicitação de conexão padrão é revisada para que as mensagens de contabilização de RADIUS são encaminhadas para um servidor NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS. Embora as mensagens de contabilidade são encaminhadas, as mensagens de autenticação e autorização não são encaminhadas e o servidor NPS local executa essas funções para o domínio local e todos os domínios confiáveis.

**NPS com RADIUS remoto para mapeamento de usuário do Windows**. Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy de RAIO para cada solicitação de conexão individuais, encaminhando a solicitação de autenticação para um servidor remoto RADIUS enquanto estiver usando uma conta de usuário do Windows local para autorização. Essa configuração é implementada Configurando o RAIO remoto ao atributo de mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. \ (Além disso, uma conta de usuário deve ser criada localmente no servidor RADIUS que tem o mesmo nome que a conta de usuário remoto em relação ao qual a autenticação é realizada pelo servidor RADIUS remoto. \)

## <a name="configuration"></a>Configuração

Para configurar o NPS como um servidor RADIUS, você pode usar a configuração padrão ou configuração avançada no console do NPS ou no Gerenciador do servidor. Para configurar o NPS como um proxy RADIUS, você deve usar configuração avançada.

### <a name="standard-configuration"></a>Configuração padrão

Com a configuração padrão, assistentes são fornecidos para ajudá-lo a configurar o NPS para os seguintes cenários:

- Servidor RADIUS para conexões dial-up ou VPN
- Servidor RADIUS para conexões com ou sem fio 802.1 X

Para configurar o NPS usando um assistente, abra o console NPS, selecione um dos cenários acima e, em seguida, clique no link que abre o assistente.

### <a name="advanced-configuration"></a>Configuração avançada

Ao usar configuração avançada, configure manualmente o NPS como um servidor RADIUS ou proxy RADIUS. 

Para configurar o NPS usando a configuração avançada, abra o console NPS e, em seguida, clique na seta ao lado de **configuração avançada** para expandir essa seção.

Os seguintes itens de configuração avançada são fornecidos.

#### <a name="configure-radius-server"></a>Configurar servidor RADIUS

Para configurar o NPS como um servidor RADIUS, você deve configurar clientes RADIUS, a política de rede e estatísticas RADIUS.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar as políticas de rede](nps-np-configure.md)
- [Configurar as estatísticas de servidor de política de rede](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar o proxy RADIUS

Para configurar o NPS como um proxy RADIUS, você deve configurar clientes RADIUS, grupos de servidores RADIUS remotos e políticas de solicitação de conexão.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Definir grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)
- [Configurar as políticas de solicitação de Conexão](nps-crp-configure.md)

## <a name="bkmk_logging"></a>Registro em log NPS

Registro em log NPS também é chamado de contabilização de RAIO. Configure o log de NPS às suas necessidades se NPS é usado como um servidor RADIUS, proxy ou qualquer combinação dessas configurações.

Para configurar o log de NPS, você deve configurar os eventos que você deseja registrados e exibidos com o Visualizador de eventos e, em seguida, determine quais outras informações que você deseja fazer logon. Além disso, você deve decidir se deseja fazer o logon de autenticação do usuário e informações de contabilidade para arquivos de log de texto armazenados no computador local ou para um banco de dados do SQL Server no computador local ou em um computador remoto.

Para obter mais informações, consulte [configurar rede política servidor Accounting](nps-accounting-configure.md).
