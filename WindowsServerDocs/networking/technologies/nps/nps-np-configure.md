---
title: Configurar políticas de rede
description: Este tópico fornece uma visão geral da configuração de diretiva de rede para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a03a6c67ddd59549cefc9742ca92e0702f92a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841997"
---
# <a name="configure-network-policies"></a>Configurar políticas de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para configurar políticas de rede no NPS.

## <a name="add-a-network-policy"></a>Adicione uma diretiva de rede

Servidor de diretivas de rede \(NPS\) usa políticas e as propriedades de discagem das contas de usuário para determinar se uma solicitação de conexão está autorizada a conectar-se à rede de rede.

Você pode usar este procedimento para configurar uma nova política de rede no console do NPS ou o console de acesso remoto.

### <a name="performing-authorization"></a>Execução de autorização

Quando o NPS realiza a autorização de uma solicitação de conexão, ele compara a solicitação com cada diretiva de rede na lista ordenada de políticas, começando com a primeira diretiva e, em seguida, movendo para baixo na lista de políticas configuradas. Se o NPS encontrar uma diretiva cujas condições correspondem à solicitação de conexão, o NPS usa a política de correspondência e as propriedades de discagem da conta de usuário para executar a autorização. Se as propriedades de discagem da conta de usuário são configuradas para conceder acesso ou controlar o acesso por meio da diretiva de rede e a solicitação de conexão for autorizada, o NPS aplica as configurações que são configuradas na política de rede para a conexão.

Se o NPS não encontrar uma diretiva de rede que corresponda à solicitação de conexão, a solicitação de conexão é rejeitada, a menos que as propriedades de discagem na conta de usuário são definidas para conceder acesso.

Se as propriedades de discagem da conta de usuário são definidas para negar acesso, a solicitação de conexão é rejeitada pelo NPS.

### <a name="key-settings"></a>Configurações de chave

Quando você usa o Assistente de nova diretiva de rede para criar uma política de rede, o valor que você especificar na **método de conexão de rede** é usado para configurar automaticamente o **tipo de política** condição: 

- Se você mantiver o valor padrão não especificado, a política de rede que você criar será avaliada pelo NPS para todos os tipos de conexão de rede que estão usando qualquer tipo de servidor de acesso de rede (NAS).
- Se você especificar um método de conexão de rede, o NPS avalia a política de rede somente se a solicitação de conexão se origina do tipo de rede do servidor de acesso que você especificar.

Sobre o **permissão de acesso** página, você deve selecionar **acesso concedido** se desejar que a política para permitir que os usuários se conectem à sua rede. Se você quiser impedir que os usuários se conectem à sua rede, selecione a política **acesso negado**. 

Se você quiser que a permissão de acesso seja determinado pelo usuário dial-in de propriedades da conta no Active Directory&reg; serviços de domínio \(AD DS\), você pode selecionar o **acesso é determinado pelas propriedades de usuário Dial-in** caixa de seleção.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-add-a-network-policy"></a>Para adicionar uma política de rede 

1. Abra o console do NPS e, em seguida, clique duas vezes **políticas**.

2. Na árvore de console, clique com botão direito **diretivas de rede**e clique em **New**. Abre o Assistente de nova diretiva de rede.

3. Use o Assistente de nova diretiva de rede para criar uma política.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Criar políticas de rede para conexões discadas ou VPN com um assistente

Você pode usar este procedimento para criar a conexão de diretivas de solicitação e necessárias para implantar servidores dial-up ou rede virtual privada de políticas de rede \(VPN\) servidores como Remote Authentication Dial-In User Service \(RADIUS\) clientes para o servidor NPS RADIUS.

>[!NOTE]
>Computadores cliente, como laptops e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede — como pontos de acesso sem fio, comutadores de autenticação 802.1X, rede virtual privada \(VPN\) servidores e os servidores de discagem — porque esses dispositivos usam o protocolo RADIUS para se comunicar com servidores RADIUS, como NPSs.

Este procedimento explica como abrir o novo Assistente de Dial-up ou conexões de rede Virtual privada no NPS.

Depois de executar o assistente, as políticas a seguir são criadas:

- Diretiva de solicitação de uma conexão
- Uma diretiva de rede

Você pode executar o novo Assistente de Dial-up ou conexões de rede Virtual privada toda vez que você precisa criar novas políticas para servidores dial-up e VPN.

Executar o Assistente de nova Dial-up ou conexões de rede Virtual privada não é a única etapa necessária para implantar servidores VPN ou dial-up como clientes RADIUS para o NPS. Ambos os métodos de acesso de rede exigem que você implante os componentes de hardware e software adicionais.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para criar diretivas para dial-up ou VPN com um assistente

1. Abra o console do NPS. Se ainda não estiver selecionado, clique em **NPS \(Local\)**. Se você quiser criar políticas em um NPS remoto, selecione o servidor.

2. Na **guia de Introdução** e **configuração padrão**, selecione **servidor RADIUS para conexões de VPN ou Dial-Up**. O texto e os links abaixo do texto mudam para refletir sua seleção.

3. Clique em **configurar VPN ou Dial-Up com um assistente**. O novo Assistente de Dial-up ou conexões de rede privada Virtual é aberto.

4. Siga as instruções no Assistente para concluir a criação de suas políticas de novo.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Criar políticas de rede de 802.1 X com ou sem fio com um assistente

Você pode usar este procedimento para criar a diretiva de solicitação de conexão e a diretiva de rede que são necessários para implantar comutadores de autenticação 802.1X ou pontos de acesso sem fio 802.1 X como clientes RADIUS Remote Authentication Dial-In usuário Service () para o NPS Servidor RADIUS.

Este procedimento explica como iniciar o Assistente de novo IEEE 802.1 X com fio e seguras conexões sem fio no NPS.

Depois de executar o assistente, as políticas a seguir são criadas:

- Diretiva de solicitação de uma conexão
- Uma diretiva de rede

Você pode executar o Assistente de novo IEEE 802.1 X com fio e seguras conexões sem fio toda vez que você precisa criar novas políticas de acesso 802.1 X.

Executar o Assistente de novo IEEE 802.1 X com fio e seguras conexões sem fio não é a única etapa necessária para implantar autenticação 802.1 X comutadores e pontos de acesso sem fio como clientes RADIUS para o NPS. Ambos os métodos de acesso de rede exigem que você implante os componentes de hardware e software adicionais.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para criar diretivas para 802.1 X com ou sem fio com um assistente

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS. 

2. Se ainda não estiver selecionado, clique em **NPS \(Local\)**. Se você quiser criar políticas em um NPS remoto, selecione o servidor.

3. Na **guia de Introdução** e **configuração padrão**, selecione **servidor RADIUS para conexões com fio ou de 802.1X sem fio**. O texto e os links abaixo do texto mudam para refletir sua seleção.

4. Clique em **configurar 802.1 X usando um assistente**. Abre o Assistente de novo IEEE 802.1 X com fio e seguras conexões sem fio.

5. Siga as instruções no Assistente para concluir a criação de suas políticas de novo.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurar o NPS para ignorar as propriedades de discagem da conta de usuário

Use este procedimento para configurar uma política de rede do NPS para ignorar as propriedades de discagem das contas de usuário no Active Directory durante o processo de autorização. Contas de usuário no Active Directory Users and Computers têm propriedades de discagem que NPS avalia durante o processo de autorização, a menos que o **permissão de acesso de rede** propriedade da conta de usuário é definida como **controle acesso por meio da diretiva de rede do NPS**. 

Há duas circunstâncias em que você talvez queira configurar o NPS para ignorar as propriedades de discagem das contas de usuário no Active Directory:

- Quando você deseja simplificar a autorização do NPS usando a diretiva de rede, mas não todas as suas contas de usuário tem o **permissão de acesso de rede** propriedade definida como **controlar o acesso por meio da diretiva de rede do NPS**. Por exemplo, algumas contas de usuário podem ter o **permissão de acesso de rede** propriedade da conta de usuário definida como **negar acesso** ou **permitir o acesso**.

- Quando outras propriedades de discagem das contas de usuário não são aplicáveis para o tipo de conexão que é configurado na política de rede. Por exemplo, as propriedades que o **permissão de acesso de rede** configuração são aplicáveis somente a dial-in ou conexões VPN, mas a política de rede que você está criando é para conexões de comutador de autenticação ou sem fio.

Você pode usar este procedimento para configurar o NPS para ignorar as propriedades de discagem da conta de usuário. Se uma solicitação de conexão corresponder a política de rede em que essa caixa de seleção está marcada, o NPS não usa as propriedades de discagem da conta de usuário para determinar se o computador ou usuário está autorizado a acessar a rede; somente as configurações na diretiva de rede são usadas para determinar a autorização.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.

2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo a **visão geral** guia **permissão de acesso**, selecione o **ignorar conta de usuário dial-in propriedades**caixa de seleção e, em seguida, clique em **Okey**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Para configurar o NPS para ignorar as propriedades de discagem da conta de usuário



## <a name="configure-nps-for-vlans"></a>Configurar o NPS para VLANs

Usando servidores de acesso de rede com suporte a VLAN e o NPS no Windows Server 2016, você pode fornecer a grupos de usuários com acesso somente aos recursos da rede que são apropriadas para suas permissões de segurança. Por exemplo, você pode fornecer os visitantes com acesso sem fio à Internet sem permitir o acesso à rede da sua organização. 

Além disso, VLANs permitem logicamente recursos de rede grupo que existem em diferentes locais físicos ou em diferentes sub-redes físicas. Por exemplo, os membros do departamento de vendas e seus recursos de rede, como computadores cliente, servidores e impressoras, podem estar localizados em vários prédios diferentes da sua organização, mas você pode colocar todos esses recursos em uma VLAN que usa o mesmo IP intervalo de endereços. As funções, da perspectiva do usuário final, como uma única sub-rede e VLAN.

Quando você deseja separar uma rede entre diferentes grupos de usuários, você também pode usar VLANs. Depois de determinar como você deseja definir seus grupos, você pode criar grupos de segurança no snap-in computadores e usuários do Active Directory e, em seguida, adicionar membros aos grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configurar uma política de rede para VLANs

Você pode usar este procedimento para configurar uma política de rede que atribui usuários a uma VLAN. Quando você usa o hardware de rede de reconhecimento de VLAN, como roteadores, comutadores e acessar os controladores, você pode configurar a política de rede para instruir os servidores de acesso para colocar os membros de grupos do Active Directory em VLANs específicas. Essa capacidade de agrupar recursos de rede lógica com VLANs fornece flexibilidade ao projetar e implementar soluções de rede.

Quando você define as configurações de uma política de rede do NPS para uso com VLANs, você deve configurar os atributos **Tunnel-Medium-Type**, **Tunnel-Pvt-Group-ID**, **detipodetúnel**, e **túnel marca**. 

Esse procedimento é fornecido como uma diretriz; sua configuração de rede pode requerer configurações diferentes daqueles descritos abaixo.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar uma política de rede para VLANs

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.

2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo, clique o **configurações** guia.

4. Na diretiva **propriedades**, na **configurações**, na **atributos RADIUS**, certifique-se de que **padrão** está selecionado.

5. No painel de detalhes, na **atributos**, o **tipo de serviço** atributo estiver configurado com um valor padrão de **enquadramento**. Por padrão, para as políticas com métodos de acesso de VPN e dial-up, o **protocolo de enquadramento** atributo estiver configurado com um valor de **PPP**. Para especificar atributos de conexão adicionais necessários para VLANs, clique em **adicionar**. O **adicionar o atributo RADIUS padrão** caixa de diálogo é aberta.

6. Na **adicionar o atributo RADIUS padrão**, nos atributos, role para baixo e adicione os seguintes atributos:

    - **Tipo de túnel médio**. Selecione um valor apropriado para as seleções anteriores feitas para a política. Por exemplo, se você estiver configurando a política de rede for uma diretiva sem fio, selecione **valor: 802 (inclui todos os 802 mídia mais formato canônico de Ethernet)**.

    - **Tunnel-Pvt-Group-ID**. Insira o número inteiro que representa o número VLAN ao qual os membros do grupo serão atribuídos. 

    - **Tipo de túnel**. Selecione **LANs virtuais (VLAN)**.


7. Na **adicionar o atributo RADIUS padrão**, clique em **fechar**. 

8. Se seu servidor de acesso de rede (NAS) requer o uso do **marca de túnel** do atributo, use as etapas a seguir para adicionar o **túnel marca** de atributo para a política de rede. Se a documentação do NAS não menciona esse atributo, não o adicione à política. Se necessário, adicione os atributos da seguinte maneira:

    - Na diretiva **propriedades**, na **configurações**, na **atributos RADIUS**, clique em **específicas de fornecedor**. 

    - No painel de detalhes, clique em **adicionar**. O **Adicionar atributo específico do fornecedor** caixa de diálogo é aberta.

    - Na **atributos**, role para baixo e selecione **túnel marca**e, em seguida, clique em **adicionar**. O **informações de atributo** caixa de diálogo é aberta. 

    - Na **valor do atributo**, digite o valor que você obteve na documentação do hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar o tamanho da carga EAP

Em alguns casos, roteadores ou firewalls descartar pacotes porque eles são configurados para descartar pacotes que exigem a fragmentação.

Quando você implanta o NPS com as políticas de rede que usam o protocolo de autenticação extensível \(EAP\) com Transport Layer Security \(TLS\), ou EAP-TLS como método de autenticação, o máximo padrão unidade de transmissão \(MTU\) que o NPS usa para cargas EAP é 1500 bytes. 

Esse tamanho máximo para a carga EAP pode criar mensagens RADIUS que exigem a fragmentação por um roteador ou firewall entre o NPS e um cliente RADIUS. Se esse for o caso, um roteador ou firewall posicionado entre o cliente RADIUS e o NPS pode descartar silenciosamente alguns fragmentos, resultando em falha de autenticação e a incapacidade de se conectar à rede do cliente de acesso.

Use o procedimento a seguir para reduzir o tamanho máximo que o NPS usa para cargas EAP, ajustando o atributo de MTU de enquadramento em uma política de rede para um valor maior do que 1344.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar o atributo de MTU de enquadramento

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.
 
2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo, clique o **configurações** guia.

4. Na **as configurações**, na **atributos RADIUS**, clique em **padrão**. No painel de detalhes, clique em **adicionar**. O **adicionar o atributo RADIUS padrão** caixa de diálogo é aberta.

5. Na **atributos**, role para baixo e clique em **MTU de enquadramento**e, em seguida, clique em **adicionar**. O **informações de atributo** caixa de diálogo é aberta.

6. Na **valor do atributo**, digite um valor igual ou menor que **1344**. Clique em **Okey**, clique em **Close**e, em seguida, clique em **Okey**.



Para obter mais informações sobre as políticas de rede, consulte [diretivas de rede](nps-np-overview.md).

Para obter exemplos de sintaxe de correspondência para especificar atributos de política de rede, consulte [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
