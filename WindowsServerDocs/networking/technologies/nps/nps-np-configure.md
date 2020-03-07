---
title: Configurar políticas de rede
description: Este tópico fornece uma visão geral da configuração de política de rede para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2bde42ba9b9489ddcd8fb3673ec5ddf1fd4d970
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371883"
---
# <a name="configure-network-policies"></a>Configurar políticas de rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para configurar políticas de rede no NPS.

## <a name="add-a-network-policy"></a>Adicionar uma política de rede

O servidor de diretivas de rede \(\) NPS usa políticas de rede e as propriedades de discagem de contas de usuário para determinar se uma solicitação de conexão está autorizada a se conectar à rede.

Você pode usar este procedimento para configurar uma nova política de rede no console do NPS ou no console de acesso remoto.

### <a name="performing-authorization"></a>Executando autorização

Quando o NPS executa a autorização de uma solicitação de conexão, ele compara a solicitação com cada política de rede na lista ordenada de políticas, começando pela primeira política e, em seguida, movendo a lista de políticas configuradas. Se o NPS encontrar uma política cujas condições correspondam à solicitação de conexão, o NPS usará a política de correspondência e as propriedades de discagem da conta de usuário para executar a autorização. Se as propriedades de discagem da conta de usuário estiverem configuradas para conceder acesso ou controlar o acesso por meio da diretiva de rede e a solicitação de conexão for autorizada, o NPS aplicará as configurações definidas na política de rede para a conexão.

Se o NPS não encontrar uma política de rede que corresponda à solicitação de conexão, a solicitação de conexão será rejeitada, a menos que as propriedades de discagem na conta de usuário estejam definidas para conceder acesso.

Se as propriedades de discagem da conta de usuário estiverem definidas para negar acesso, a solicitação de conexão será rejeitada pelo NPS.

### <a name="key-settings"></a>Configurações de chave

Quando você usa o assistente de nova política de rede para criar uma política de rede, o valor que você especifica no **método de conexão de rede** é usado para configurar automaticamente a condição de tipo de **política** : 

- Se você mantiver o valor padrão de não especificado, a política de rede que você criar será avaliada pelo NPS para todos os tipos de conexão de rede que estão usando qualquer tipo de NAS (servidor de acesso à rede).
- Se você especificar um método de conexão de rede, o NPS avaliará a diretiva de rede somente se a solicitação de conexão for proveniente do tipo de servidor de acesso à rede que você especificar.

Na página **permissão de acesso** , você deve selecionar **acesso concedido** se desejar que a política permita que os usuários se conectem à sua rede. Se você quiser que a política impeça que os usuários se conectem à sua rede, selecione **acesso negado**. 

Se você quiser que a permissão de acesso seja determinada pelas propriedades de discagem da conta de usuário no Active Directory&reg; serviços de domínio \(AD DS\), marque a caixa de seleção o **acesso é determinado por propriedades de discagem do usuário** .

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

### <a name="to-add-a-network-policy"></a>Para adicionar uma política de rede 

1. Abra o console do NPS e clique duas vezes em **políticas**.

2. Na árvore de console, clique com o botão direito do mouse em **políticas de rede**e clique em **novo**. O assistente Nova Diretiva de Rede é exibido.

3. Use o assistente de nova política de rede para criar uma política.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Criar políticas de rede para dial-up ou VPN com um assistente

Você pode usar este procedimento para criar as políticas de solicitação de conexão e as políticas de rede necessárias para implantar servidores dial-up ou rede virtual privada \(servidores VPN\) como serviço RADIUS \(RADIUS\) clientes para o servidor RADIUS NPS.

>[!NOTE]
>Computadores cliente, como computadores laptop e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede — como pontos de acesso sem fio, comutadores de autenticação 802.1 X, rede virtual privada \(servidores VPN\) e servidores dial-up — porque esses dispositivos usam o protocolo RADIUS para se comunicar com servidores RADIUS, como NPSs.

Este procedimento explica como abrir o novo assistente de conexões de rede virtual privada ou dial-up no NPS.

Depois de executar o assistente, as seguintes políticas são criadas:

- Uma política de solicitação de conexão
- Uma política de rede

Você pode executar o novo assistente de conexões de rede virtual privada ou dial-up toda vez que precisar criar novas políticas para servidores de conexão discada e servidores VPN.

A execução do novo assistente de conexões de rede virtual privada ou dial-up não é a única etapa necessária para implantar servidores VPN ou discadas como clientes RADIUS para o NPS. Os dois métodos de acesso à rede exigem que você implante componentes de hardware e software adicionais.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para criar políticas para dial-up ou VPN com um assistente

1. Abra o console do NPS. Se ainda não estiver selecionado, clique em **NPS \(Local\)** . Se você quiser criar políticas em um NPS remoto, selecione o servidor.

2. Em **introdução** e **configuração padrão**, selecione **servidor RADIUS para conexões dial-up ou VPN**. O texto e os links sob o texto são alterados para refletir sua seleção.

3. Clique em **Configurar VPN ou dial-up com um assistente**. O novo assistente para conexão discada ou rede virtual privada é aberto.

4. Siga as instruções no Assistente para concluir a criação de suas novas políticas.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Criar políticas de rede para 802.1 X com ou sem fio com um assistente

Você pode usar este procedimento para criar a política de solicitação de conexão e a política de rede que são necessárias para implantar comutadores de autenticação 802.1 X ou pontos de acesso sem fio 802.1 X como clientes RADIUS (serviço RADIUS) para o NPS Servidor RADIUS.

Este procedimento explica como iniciar o novo assistente de conexões sem fio e com fio IEEE 802.1 X seguro no NPS.

Depois de executar o assistente, as seguintes políticas são criadas:

- Uma política de solicitação de conexão
- Uma política de rede

Você pode executar o novo assistente de conexões com fio e sem fio IEEE 802.1 X seguras toda vez que precisar criar novas políticas para acesso 802.1 X.

A execução do novo assistente de conexões com e sem fio IEEE 802.1 X seguro não é a única etapa necessária para implantar comutadores de autenticação 802.1 X e pontos de acesso sem fio como clientes RADIUS para o NPS. Os dois métodos de acesso à rede exigem que você implante componentes de hardware e software adicionais.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para criar políticas para 802.1 X com ou sem fio com um assistente

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto. 

2. Se ainda não estiver selecionado, clique em **NPS \(Local\)** . Se você quiser criar políticas em um NPS remoto, selecione o servidor.

3. Em **introdução** e **configuração padrão**, selecione **servidor RADIUS para conexões 802.1 x sem fio ou com fio**. O texto e os links sob o texto são alterados para refletir sua seleção.

4. Clique em **Configurar 802.1 x usando um assistente**. O novo assistente de conexões com e sem fio IEEE 802.1 X seguro é aberto.

5. Siga as instruções no Assistente para concluir a criação de suas novas políticas.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurar o NPS para ignorar as propriedades de discagem da conta de usuário

Use este procedimento para configurar uma política de rede NPS para ignorar as propriedades de discagem de contas de usuário no Active Directory durante o processo de autorização. As contas de usuário no Active Directory usuários e computadores têm propriedades de discagem que o NPS avalia durante o processo de autorização, a menos que a propriedade **permissão de acesso à rede** da conta de usuário esteja definida para controlar o **acesso por meio da política de rede do NPS**. 

Há duas circunstâncias em que você pode desejar configurar o NPS para ignorar as propriedades de discagem de contas de usuário no Active Directory:

- Quando você deseja simplificar a autorização do NPS usando a política de rede, mas nem todas as suas contas de usuário têm a propriedade de **permissão de acesso à rede** definida para controlar o **acesso por meio da política de rede do NPS**. Por exemplo, algumas contas de usuário podem ter a propriedade **permissão de acesso à rede** da conta de usuário definida para **negar acesso** ou **permitir acesso**.

- Quando outras propriedades de discagem de contas de usuário não são aplicáveis ao tipo de conexão que está configurado na política de rede. Por exemplo, propriedades diferentes da configuração de **permissão de acesso à rede** são aplicáveis somente a conexões dial-in ou VPN, mas a política de rede que você está criando é para conexões de comutador sem fio ou de autenticação.

Você pode usar este procedimento para configurar o NPS para ignorar as propriedades de discagem da conta de usuário. Se uma solicitação de conexão corresponder à política de rede em que essa caixa de seleção está marcada, o NPS não usará as propriedades de discagem da conta de usuário para determinar se o usuário ou o computador está autorizado a acessar a rede; somente as configurações na política de rede são usadas para determinar a autorização.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.

2. Clique duas vezes em **políticas**, em **políticas de rede**e, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na caixa de **diálogo Propriedades** da política, na guia **visão geral** , em **permissão de acesso**, marque a caixa de seleção **ignorar Propriedades de discagem da conta de usuário** e clique em **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Para configurar o NPS para ignorar as propriedades de discagem da conta de usuário



## <a name="configure-nps-for-vlans"></a>Configurar NPS para VLANs

Ao usar servidores de acesso à rede com reconhecimento de VLAN e NPS no Windows Server 2016, você pode fornecer grupos de usuários com acesso apenas aos recursos de rede apropriados para suas permissões de segurança. Por exemplo, você pode fornecer aos visitantes acesso sem fio à Internet sem permitir acesso à rede da sua organização. 

Além disso, as VLANs permitem que você agrupe logicamente os recursos de rede que existem em diferentes locais físicos ou em sub-redes físicas diferentes. Por exemplo, os membros do seu departamento de vendas e seus recursos de rede, como computadores cliente, servidores e impressoras, podem estar localizados em vários prédios diferentes na sua organização, mas você pode posicionar todos esses recursos em uma VLAN que usa o mesmo IP intervalo de endereços. A VLAN, em seguida, funciona, da perspectiva do usuário final, como uma única sub-rede.

Você também pode usar VLANs quando quiser separar uma rede entre diferentes grupos de usuários. Depois de determinar como deseja definir seus grupos, você pode criar grupos de segurança no snap-in Active Directory usuários e computadores e, em seguida, adicionar membros aos grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configurar uma política de rede para VLANs

Você pode usar este procedimento para configurar uma política de rede que atribui usuários a uma VLAN. Ao usar o hardware de rede com reconhecimento de VLAN, como roteadores, comutadores e controladores de acesso, você pode configurar a política de rede para instruir os servidores de acesso a inserir membros de grupos de Active Directory específicos em VLANs específicas. Essa capacidade de agrupar recursos de rede logicamente com VLANs fornece flexibilidade ao projetar e implementar soluções de rede.

Ao definir as configurações de uma política de rede NPS para uso com VLANs, você deve configurar os atributos **Tunnel-Medium**, **Tunnel-Pvt-Group-ID**, **túnel-Type**e **Tunnel-Tag**. 

Esse procedimento é fornecido como uma diretriz; sua configuração de rede pode exigir configurações diferentes das descritas abaixo.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar uma política de rede para VLANs

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.

2. Clique duas vezes em **políticas**, em **políticas de rede**e, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na caixa de diálogo **Propriedades** da política, clique na guia **configurações** .

4. Em **Propriedades**da política, em **configurações**, em **atributos RADIUS**, verifique se **padrão** está selecionado.

5. No painel de detalhes, em **atributos**, o atributo **Service-Type** é configurado com um valor padrão de **Framed**. Por padrão, para políticas com métodos de acesso de VPN e dial-up, o atributo **Framed-Protocol** é configurado com um valor de **PPP**. Para especificar atributos de conexão adicionais necessários para VLANs, clique em **Adicionar**. A caixa de diálogo **Adicionar atributo RADIUS padrão** é aberta.

6. Em **Adicionar atributo RADIUS padrão**, em atributos, role para baixo e adicione os seguintes atributos:

    - **Tunnel-Medium-Type**. Selecione um valor apropriado para as seleções anteriores que você fez para a política. Por exemplo, se a política de rede que você está configurando for uma política sem fio, selecione **valor: 802 (inclui todas as mídias 802 e formato canônico Ethernet)** .

    - **Tunnel-Pvt-Group-ID**. Insira o inteiro que representa o número de VLAN ao qual os membros do grupo serão atribuídos. 

    - **Tipo de túnel**. Selecione **redes virtuais (VLAN)** .


7. Em **Adicionar atributo RADIUS padrão**, clique em **fechar**. 

8. Se o seu NAS (servidor de acesso à rede) exigir o uso do atributo **Tunnel-Tag** , use as etapas a seguir para adicionar o atributo **Tunnel-Tag** à política de rede. Se a documentação do NAS não mencionar esse atributo, não o adicione à política. Se necessário, adicione os atributos da seguinte maneira:

    - Em **Propriedades**da política, **em configurações**, em **atributos RADIUS**, clique em **específico do fornecedor**. 

    - No painel de detalhes, clique em **Adicionar**. A caixa de diálogo **Adicionar atributo específico do fornecedor** é aberta.

    - Em **atributos**, role para baixo até e selecione **Tunnel-Tag**e, em seguida, clique em **Adicionar**. A caixa de diálogo **informações de atributo** é aberta. 

    - Em **valor do atributo**, digite o valor que você obteve da documentação do hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar o tamanho da carga EAP

Em alguns casos, roteadores ou firewalls descartam pacotes porque eles são configurados para descartar pacotes que exigem fragmentação.

Quando você implanta o NPS com políticas de rede que usam o protocolo de autenticação extensível \(EAP\) com segurança de camada de transporte \(TLS\)ou EAP-TLS, como um método de autenticação, a unidade de transmissão máxima padrão \(MTU\) que o NPS usa para cargas de EAP é de 1500 bytes. 

Esse tamanho máximo para a carga EAP pode criar mensagens RADIUS que exigem fragmentação por um roteador ou firewall entre o NPS e um cliente RADIUS. Se esse for o caso, um roteador ou firewall posicionado entre o cliente RADIUS e o NPS pode descartar silenciosamente alguns fragmentos, resultando em falha de autenticação e a incapacidade do cliente de acesso para se conectar à rede.

Use o procedimento a seguir para reduzir o tamanho máximo que o NPS usa para cargas de EAP, ajustando o atributo frame-MTU em uma política de rede para um valor não maior que 1344.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar o atributo frame-MTU

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.
 
2. Clique duas vezes em **políticas**, em **políticas de rede**e, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na caixa de diálogo **Propriedades** da política, clique na guia **configurações** .

4. Em **configurações**, em **atributos RADIUS**, clique em **padrão**. No painel de detalhes, clique em **Adicionar**. A caixa de diálogo **Adicionar atributo RADIUS padrão** é aberta.

5. Em **atributos**, role para baixo e clique em **frame-MTU**e, em seguida, clique em **Adicionar**. A caixa de diálogo **informações de atributo** é aberta.

6. Em **valor do atributo**, digite um valor igual ou menor que **1344**. Clique em **OK**, em **fechar**e em **OK**.



Para obter mais informações sobre diretivas de rede, consulte [Network Policies](nps-np-overview.md).

Para obter exemplos de sintaxe de correspondência de padrões para especificar atributos de política de rede, consulte [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
