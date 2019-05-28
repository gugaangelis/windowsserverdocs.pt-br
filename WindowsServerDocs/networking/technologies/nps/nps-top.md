---
title: NPS (Servidor de Políticas de Rede)
description: Este tópico fornece uma visão geral do servidor de diretivas de rede no Windows Server 2016 e Windows Server 2019 e inclui links para diretrizes adicionais sobre o NPS.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 0439c0f45a604f6b3ef90369f5fe77a59568d9d7
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222582"
---
# <a name="network-policy-server-nps"></a>NPS (Servidor de Políticas de Rede)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server de 2019

Você pode usar este tópico para uma visão geral do servidor de diretivas de rede no Windows Server 2016 e Windows Server 2019. O NPS é instalado quando você instala o recurso de serviços de acesso (NPAS) e a política de rede no Windows Server 2016 e o servidor de 2019.

>[!NOTE]
>Além deste tópico, a seguinte documentação do NPS está disponível.
> - [Práticas recomendadas do servidor de diretiva de rede](nps-best-practices.md)
> - [Introdução ao servidor de políticas de rede](nps-getstart-top.md)
> - [Planejar o servidor de políticas de rede](nps-plan-top.md)
> - [Implantar o servidor de políticas de rede](nps-deploy.md)
> - [Gerenciar o servidor de políticas de rede](nps-manage-top.md)
> - [Cmdlets NPS (servidor) de diretiva no Windows PowerShell de rede](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2016 e Windows 10
> - [Cmdlets NPS (servidor) de diretiva no Windows PowerShell de rede](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 R2 e Windows 8.1
> - [Cmdlets NPS no Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 e Windows 8

O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão.

Você também pode configurar o NPS como um proxy RADIUS Remote Authentication Dial-In usuário Service () para encaminhar solicitações de conexão para um NPS remoto ou outro servidor RADIUS, para que você possa carregar solicitações de conexão de balanceamento e encaminhá-los para o domínio correto para autenticação e a autorização.

O NPS permite que você configurar centralmente e gerenciar a autenticação, autorização e contabilização com os seguintes recursos:

- **Servidor RADIUS**. NPS executa autenticação centralizada, autorização e contabilização para rede sem fio, autenticando switch, dial-up de acesso remoto e conexões de rede virtual privada (VPN). Ao usar o NPS como servidor RADIUS, você configura servidores de acesso à rede (como pontos de acesso sem fio e servidores VPN) como clientes RADIUS no NPS. Você também configura as políticas de rede que o NPS usa para autorizar solicitações de conexão. Você pode configurar a contabilização RADIUS para que o NPS registre as informações de contabilização em arquivos de log no disco rígido local ou em um banco de dados do Microsoft SQL Server. Para obter mais informações, consulte [servidor RADIUS](#radius-server).
- **Proxy RADIUS**. Quando você usa o NPS como um proxy RADIUS, você pode configurar políticas de solicitação de conexão que informam o NPS no qual conexão solicitações para encaminhar para outros servidores RADIUS e a quais servidores RADIUS você deseja encaminhar solicitações de conexão. Também é possível configurar o NPS para que encaminhe dados de estatística a serem registrados por um ou mais computadores de um grupo de servidores RADIUS remotos. Para configurar o NPS como um servidor proxy RADIUS, consulte os tópicos a seguir. Para obter mais informações, consulte [proxy RADIUS](#radius-proxy).
    - [Configurar políticas de solicitação de Conexão](nps-crp-configure.md)
- **Contabilidade RADIUS**. Você pode configurar o NPS para registrar eventos em um arquivo de log local ou para uma instância local ou remota do Microsoft SQL Server. Para obter mais informações, consulte [log NPS](#nps-logging).

>[!IMPORTANT]
>Proteção de acesso de rede \(NAP\), autoridade de registro de integridade \(HRA\)e o Host Credential Authorization Protocol \(HCAP\) foram preteridas no Windows Server 2012 R2 e não estão disponíveis no Windows Server 2016. Se você tiver uma implantação de NAP usando sistemas operacionais anteriores ao Windows Server 2016, você não pode migrar sua implantação de NAP para o Windows Server 2016.

Você pode configurar o NPS com qualquer combinação desses recursos. Por exemplo, você pode configurar um NPS como servidor RADIUS para conexões VPN e também como um proxy RADIUS para encaminhar algumas solicitações de conexão para membros de um grupo de servidores remotos RADIUS para autenticação e autorização em outro domínio.

## <a name="windows-server-editions-and-nps"></a>O NPS e edições do Windows Server

NPS fornece uma funcionalidade diferente dependendo da edição do Windows Server que você instala.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 ou Windows Server 2019 Standard/Datacenter Edition

Com o NPS no Windows Server 2016 Standard ou Datacenter, você pode configurar um número ilimitado de clientes RADIUS e grupos de servidores RADIUS remotos. Além disso, você pode configurar clientes RADIUS especificando um intervalo de endereços IP.

>[!NOTE]
>O recurso Serviços de acesso e diretiva de rede do WIndows não está disponível nos sistemas instalados com uma opção de instalação Server Core.

As seções a seguir fornecem informações mais detalhadas sobre o NPS como um servidor RADIUS e proxy.

## <a name="radius-server-and-proxy"></a>Servidor e proxy RADIUS

Você pode usar o NPS como um servidor RADIUS, proxy RADIUS ou ambos.

### <a name="radius-server"></a>Servidor RADIUS

O NPS é a implementação da Microsoft do padrão RADIUS especificado pela Internet Engineering Task Force \(IETF\) nas RFCs 2865 e 2866. Como um servidor RADIUS, o NPS executa autenticação centralizada de conexão, autorização e contabilidade para muitos tipos de acesso à rede, incluindo o comutador de autenticação, sem fio, dial-up e rede virtual privada \(VPN\) remoto o acesso e conexões de roteador a roteador.

>[!NOTE]
>Para obter informações sobre como implantar o NPS como servidor RADIUS, consulte [implantar servidor de políticas de rede](nps-deploy.md).

O NPS permite o uso de um conjunto heterogêneo de sem fio, comutador, acesso remoto ou equipamentos de VPN. Você pode usar o NPS com o serviço de acesso remoto, que está disponível no Windows Server 2016.

O NPS usa um Active Directory Domain Services \(AD DS\) domínio ou as contas de usuário do Gerenciador de contas de segurança (SAM) local do banco de dados para autenticar as credenciais do usuário para tentativas de conexão. Quando um servidor que executa o NPS é um membro de um domínio AD DS, o NPS usa o serviço de diretório como seu banco de dados de conta de usuário e é parte de uma solução de logon única. O mesmo conjunto de credenciais é usado para controle de acesso de rede \(autenticando e autorizando o acesso a uma rede\) e fazer logon em um domínio AD DS.

>[!NOTE]
>NPS usa as propriedades de discagem das políticas de rede e conta de usuário para autorizar uma conexão.

Provedores de serviços de Internet \(ISPs\) e as organizações que mantêm o acesso à rede têm o maior desafio do gerenciamento de todos os tipos de acesso à rede de um único ponto de administração, independentemente do tipo de acesso à rede equipamento usado. O padrão RADIUS dá suporte a essa funcionalidade em ambientes homogêneos e heterogêneos. RADIUS é um protocolo de cliente-servidor que permite que o equipamento de acesso à rede (usado como clientes RADIUS) para enviar solicitações de autenticação e estatísticas para um servidor RADIUS.

Um servidor RADIUS tem acesso às informações de conta de usuário e pode verificar as credenciais de autenticação de acesso de rede. Se as credenciais do usuário são autenticadas e a tentativa de conexão for autorizada, o servidor RADIUS autoriza o acesso de usuário com base nas condições especificadas e, em seguida, efetua a conexão de acesso de rede no log de estatísticas. O uso do RADIUS permite a autenticação de usuário de acesso de rede, autorização e dados de estatísticas a serem coletados e mantidos em um local central, em vez de em cada servidor de acesso.

#### <a name="using-nps-as-a-radius-server"></a>Usando o NPS como servidor RADIUS

Você pode usar o NPS como servidor RADIUS quando:

- Você está usando um domínio AD DS ou o banco de dados de contas de usuário SAM local do banco de dados de conta de usuário para clientes de acesso. 
- Você estiver usando o acesso remoto em vários servidores de dial-up, servidores VPN, ou roteadores de discagem por demanda e você deseja centralizar a configuração de diretivas de rede e de log e estatísticas de conexão. 
- Estiver terceirizando a sua rede dial-up, VPN ou acesso sem fio para um provedor de serviços. Os servidores de acesso usam o RADIUS para autenticar e autorizar conexões feitas por membros da sua organização.
- Você deseja centralizar a autenticação, autorização e contabilização para um conjunto heterogêneo de servidores de acesso.

A ilustração a seguir mostra o NPS como servidor RADIUS para uma variedade de clientes de acesso. 

![NPS como servidor RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Como um proxy RADIUS, o NPS encaminha mensagens de contabilização e autenticação ao NPS e outros servidores RADIUS. Você pode usar o NPS como um proxy RADIUS para fornecer o roteamento de RAIO de mensagens entre clientes RADIUS \(também chamado de servidores de acesso à rede\) e servidores RADIUS que executam a autenticação de usuário, autorização e contabilização para o tentativa de conexão. 

Quando usado como um proxy RADIUS, o NPS é um ponto de roteamento por meio do qual fluxo de mensagens RADIUS estatísticas e acesso ou de comutação central. NPS registra as informações no log de estatísticas sobre as mensagens que são encaminhados.

#### <a name="using-nps-as-a-radius-proxy"></a>Usando o NPS como um proxy RADIUS

Você pode usar o NPS como um proxy RADIUS quando:

- Você é um provedor de serviço que oferece serviços de acesso à rede sem fio, VPN ou dial-up terceirizado para vários clientes. Seus NASs enviam solicitações de conexão para o proxy RADIUS do NPS. Com base no realm do nome de usuário na solicitação de conexão, o proxy RADIUS NPS encaminha a solicitação de conexão para um servidor RADIUS que é mantido pelo cliente e pode autenticar e autorizar a tentativa de conexão.
- Você deseja fornecer autenticação e autorização para contas de usuário que não são membros do domínio no qual o NPS é um membro ou de outro domínio que tenha uma relação de confiança bidirecional com o domínio no qual o NPS é um membro. Isso inclui contas em outras florestas, domínios confiáveis unidirecionais e domínios não confiáveis. Em vez de configurar seus servidores de acesso para enviar suas solicitações de conexão para um servidor NPS RADIUS, você pode configurá-los para enviar suas solicitações de conexão para um proxy RADIUS do NPS. O proxy RADIUS NPS usa a parte do nome do território do nome de usuário e encaminha a solicitação para um NPS no domínio correto ou na floresta. Tentativas de Conexão de usuário contas em um domínio ou floresta podem ser autenticadas para NASs em outro domínio ou floresta.
- Você deseja realizar a autenticação e autorização usando um banco de dados que não é um banco de dados de conta do Windows. Nesse caso, as solicitações de conexão que correspondem a um nome de realm especificado são encaminhadas para um servidor RADIUS, que tem acesso a um banco de dados de diferente contas de usuário e dados de autorização. Exemplos de outros bancos de dados de usuário incluem bancos de dados dos serviços de diretório da Novell (NDS) e SQL Structured Query Language ().
- Você deseja processar um grande número de solicitações de conexão. Nesse caso, em vez de configurar os clientes RADIUS para tentar equilibrar suas solicitações de estatísticas e a conexão entre vários servidores RADIUS, você pode configurá-los para enviar suas solicitações de estatísticas e conexão para um proxy RADIUS do NPS. O proxy RADIUS NPS dinamicamente equilibra a carga de conexão e contabilidade solicitações entre vários servidores RADIUS e aumenta o processamento de grandes números de clientes RADIUS e autenticações por segundo.
- Você deseja fornecer autenticação e autorização para provedores de serviços terceirizado RADIUS e minimizar a configuração de firewall da intranet. Um firewall de intranet está entre a rede de perímetro (a rede entre a intranet e Internet) e a intranet. Colocando um NPS em sua rede de perímetro, o firewall entre a rede de perímetro e a intranet deve permitir o tráfego flua entre o NPS e vários controladores de domínio. Substituindo o NPS por um proxy do NPS, o firewall deve permitir apenas o tráfego RADIUS flua entre o proxy NPS e um ou vários NPSs dentro de sua intranet.

A ilustração a seguir mostra o NPS como um proxy RADIUS entre clientes RADIUS e servidores RADIUS.

![NPS como um Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Com o NPS, as organizações podem também terceirizar a infraestrutura de acesso remoto para um provedor de serviços, mantendo o controle sobre a autenticação de usuário, autorização e contabilidade.

Configurações do NPS podem ser criadas para os seguintes cenários:

- Acesso sem fio
- Acesso remoto da organização dial-up ou rede privada (VPN)
- Acesso sem fio ou discada terceirizada
- Acesso à rede e à Internet
- Acesso autenticado a recursos da extranet para parceiros de negócios

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Servidor RADIUS e exemplos de configuração de proxy RADIUS

Os exemplos de configuração a seguir demonstram como você pode configurar o NPS como um servidor RADIUS e proxy RADIUS.

**NPS como servidor RADIUS**. Neste exemplo, o NPS é configurado como um servidor RADIUS, a diretiva de solicitação de conexão padrão é a única política configurada e todas as solicitações de conexão são processadas pelo NPS local. O NPS pode autenticar e autorizar os usuários cujas contas estejam no domínio do NPS e em domínios confiáveis.

**NPS como um proxy RADIUS**. Neste exemplo, o NPS está configurado como um proxy RADIUS que encaminha as solicitações de conexão para grupos de servidores remotos RADIUS em dois domínios não confiáveis. A diretiva de solicitação de conexão padrão é excluída e duas novas diretivas de solicitação de conexão são criadas para encaminhar solicitações para cada um dos dois domínios não confiáveis. Neste exemplo, o NPS não processa quaisquer solicitações de conexão no servidor local. 

**NPS como servidor RADIUS e proxy RADIUS**. Além da política de solicitação de conexão padrão, que determina que as solicitações de conexão são processadas localmente, uma nova diretiva de solicitação de conexão é criada que encaminha solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Essa segunda diretiva é chamada a diretiva de Proxy. Neste exemplo, a diretiva de Proxy aparece primeira na lista ordenada de políticas. Se a solicitação de conexão corresponde a diretiva de Proxy, a solicitação de conexão é encaminhada para o servidor RADIUS no grupo de servidores RADIUS remotos. Se a solicitação de conexão não coincide com a diretiva de Proxy, mas coincide com a diretiva de solicitação de conexão padrão, o NPS processa a solicitação de conexão no servidor local. Se a solicitação de conexão não coincide com a política, ela será descartada.

**NPS como servidor RADIUS com servidores remotos de estatísticas**. Neste exemplo, o NPS local não está configurado para executar a contabilização e a diretiva de solicitação de conexão padrão é revisada para que as mensagens de contabilização RADIUS sejam encaminhadas para um NPS ou outro servidor RADIUS de um grupo de servidores RADIUS remotos. Embora as mensagens de estatística são encaminhadas, mensagens de autenticação e autorização não são encaminhadas e o NPS local executa essas funções para o domínio local e todos os domínios confiáveis.

**NPS com RADIUS remoto para mapeamento de usuário do Windows**. Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy RADIUS para cada solicitação de conexão individuais, encaminhar a solicitação de autenticação para um servidor RADIUS remoto ao usar uma conta de usuário do Windows local para autorização. Essa configuração é implementada por meio da configuração de RADIUS remoto para o atributo de mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. \(Além disso, uma conta de usuário deve ser criada localmente no servidor RADIUS que tem o mesmo nome que a conta de usuário remoto em relação ao qual autenticação é realizada pelo servidor RADIUS remoto.\)

## <a name="configuration"></a>Configuração

Para configurar o NPS como servidor RADIUS, você pode usar a configuração padrão ou a configuração avançada, no console do NPS ou no Gerenciador do servidor. Para configurar o NPS como um proxy RADIUS, você deve usar a configuração avançada.

### <a name="standard-configuration"></a>Configuração padrão

Com a configuração padrão, os assistentes são fornecidos para ajudá-lo a configurar o NPS para os seguintes cenários:

- Servidor RADIUS para conexões de VPN ou dial-up
- Servidor RADIUS para conexões 802.1X com ou sem fio

Para configurar o NPS usando um assistente, abra o console do NPS, selecione um dos cenários anteriores e, em seguida, clique no link que abre o assistente.

### <a name="advanced-configuration"></a>Configuração avançada

Quando você usa a configuração avançada, configure manualmente o NPS como servidor RADIUS ou proxy RADIUS. 

Para configurar o NPS usando a configuração avançada, abra o console do NPS e, em seguida, clique na seta ao lado **configuração avançada** para expandir essa seção.

Os seguintes itens de configuração avançada são fornecidos.

#### <a name="configure-radius-server"></a>Configurar o servidor RADIUS

Para configurar o NPS como servidor RADIUS, você deve configurar a contabilização RADIUS, de clientes RADIUS e de política de rede.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar políticas de rede](nps-np-configure.md)
- [Configurar a contabilização de servidor de diretiva de rede](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar o proxy RADIUS

Para configurar o NPS como um proxy RADIUS, você deve configurar clientes RADIUS, grupos de servidores remotos RADIUS e as diretivas de solicitação de conexão.

Para obter instruções sobre como fazer essas configurações, consulte os tópicos a seguir.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar grupos de servidores remotos RADIUS](nps-crp-rrsg-configure.md)
- [Configurar políticas de solicitação de Conexão](nps-crp-configure.md)

## <a name="nps-logging"></a>Registro em log do NPS

Registro em log o NPS também é chamado de contabilização RADIUS. Configure o NPS log aos seus requisitos se o NPS é usado como um servidor RADIUS, proxy ou qualquer combinação dessas configurações.

Para configurar o log do NPS, você deve configurar quais eventos registrados e exibidos com visualizador de eventos e, em seguida, determinar quais outras informações que você deseja registrar. Além disso, você deve decidir se deseja fazer o logon de autenticação de usuário e informações de estatísticas para arquivos de log de texto armazenados no computador local ou para um banco de dados do SQL Server no computador local ou em um computador remoto.

Para obter mais informações, consulte [configurar política de servidor de contabilidade de rede](nps-accounting-configure.md).
