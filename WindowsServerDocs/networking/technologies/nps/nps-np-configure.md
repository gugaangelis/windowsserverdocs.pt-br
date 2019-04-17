---
title: Configurar as políticas de rede
description: Este tópico fornece uma visão geral da configuração de política de rede para o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abcd9343f10100884f96d17d82704382e2af760
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policies"></a>Configurar as políticas de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para configurar políticas de rede no NPS.

## <a name="add-a-network-policy"></a>Adicionar uma política de rede

Rede o servidor de política \(NPS\) usa políticas de rede e as propriedades de discagem rápida de contas de usuário para determinar se uma solicitação de conexão tem autorização para se conectar à rede.

Você pode usar este procedimento para configurar uma nova política de rede no console do NPS ou o console de acesso remoto.

### <a name="performing-authorization"></a>Executar autorização

Quando o NPS realiza a autorização de uma solicitação de conexão, ele compara a solicitação com cada política de rede na lista ordenada de políticas, começando com a política de primeiro e, em seguida, mova para baixo a lista de políticas configuradas. Se o NPS encontrar uma diretiva cujas condições correspondam a solicitação de conexão, o NPS usa a diretiva correspondente e as propriedades de discagem rápida da conta de usuário para realizar autorização. Se as propriedades de discagem da conta do usuário são configuradas para conceder acesso ou controle de acesso por meio da política de rede e está autorizada a solicitação de conexão, o NPS aplica as configurações que estão configuradas na política de rede para a conexão.

Se o NPS não encontra uma política de rede que corresponda à solicitação de conexão, a solicitação de conexão será rejeitada, a menos que as propriedades de discagem na conta de usuário são definidas para conceder acesso.

Se as propriedades de discagem rápida da conta de usuário estiverem definidas como negar o acesso, a solicitação de conexão será rejeitada pelo NPS.

### <a name="key-settings"></a>Configurações de chave

Quando você usa o Assistente de nova política de rede para criar uma política de rede, o valor que você especifica na **método de conexão de rede** é usado para configurar automaticamente o **tipo de política** condição: 

- Se você mantiver o valor padrão não especificado, a política de rede que você cria é avaliada pelo NPS para todos os tipos de conexão de rede que estão usando qualquer tipo de servidor de acesso de rede (NAS).
- Se você especificar um método de conexão de rede, o NPS avalia a política de rede somente se a solicitação de conexão originada do tipo de servidor de acesso de rede que você especificar.

Sobre o **permissão de acesso** página, você deve selecionar **acesso concedido** se você quiser que a política para permitir que os usuários se conectem a sua rede. Se você quiser que a política para impedir que os usuários se conecte à rede, selecione **acesso negado**. 

Se você quiser que a permissão de acesso a ser determinado pelo usuário dial-propriedades da conta no Active Directory&reg; \(AD DS\) serviços de domínio, você pode selecionar o **acesso é determinado pelo usuário propriedades de discagem** caixa de seleção.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-add-a-network-policy"></a>Para adicionar uma política de rede 

1. Abra o console do NPS e clique duas vezes em **políticas**.

2. Na árvore de console, clique com botão direito **políticas de rede**e clique em **nova**. Abre o Assistente de nova política de rede.

3. Use o Assistente de nova política de rede para criar uma política.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Criar políticas de rede Dial-Up ou VPN com um assistente

Você pode usar este procedimento para criar as políticas de solicitação de conexão e políticas de rede necessárias para implantar servidores dial-up ou servidores \(VPN\) de rede virtual privada como clientes Remote Authentication Dial-In User Service \(RADIUS\) para o servidor RADIUS de NPS.

>[!NOTE]
>Computadores cliente, como laptops e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, comutadores de autenticação 802.1 X, servidores \(VPN\) de rede virtual privada e servidores dial-up — porque esses dispositivos usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidores NPS.

Este procedimento explica como abrir o novo assistente Dial-up ou conexões de rede Virtual privada no NPS.

Depois de executar o assistente, as políticas a seguir são criadas:

- Política de solicitação de uma conexão
- Uma diretiva de rede

Você pode executar o Assistente para Dial-up ou conexões de rede Virtual privada sempre que você precisa criar novas políticas para servidores dial-up e servidores VPN.

Executar o Assistente de nova Dial-up ou conexões de rede Virtual privada não é a única etapa necessária para implantar dial-up ou servidores VPN como clientes RADIUS no servidor NPS. Ambos os métodos de acesso de rede exigem que você implantar componentes adicionais de hardware e software.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para criar políticas para dial-up ou VPN com um assistente

1. Abra o console NPS. Se ainda não estiver selecionada, clique em **NPS \(Local\)**. Se você quiser criar políticas em um servidor NPS remoto, selecione o servidor.

2. Em **Introdução** e **configuração padrão**, selecione **servidor RADIUS para conexões Dial-Up ou VPN**. O texto e os links abaixo do texto mudam para refletir sua seleção.

3. Clique em **configurar VPN ou Dial-Up com um assistente**. O novo assistente Dial-up ou conexões de rede Virtual privada é aberta.

4. Siga as instruções no Assistente para concluir a criação de seu novas políticas.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Criar políticas de rede para 802.1 X com ou sem fio com um assistente

Você pode usar este procedimento para criar a política de solicitação de conexão e a política de rede são necessárias para implantar comutadores de autenticação 802.1 X ou pontos de acesso sem fio 802.1 X como clientes de autenticação discagem usuário serviço RADIUS (Remote) para o servidor RADIUS de NPS.

Este procedimento explica como iniciar o Assistente de nova IEEE 802.1 X com fio e seguras conexões sem fio no NPS.

Depois de executar o assistente, as políticas a seguir são criadas:

- Política de solicitação de uma conexão
- Uma diretiva de rede

Você pode executar o Assistente de nova IEEE 802.1 X com fio e seguras conexões sem fio sempre que você precisa criar novas políticas de acesso 802.1 X.

Executar o Assistente de nova IEEE 802.1 X com fio e seguras conexões sem fio não é a única etapa necessária para implantar autenticação 802.1 X opções e pontos de acesso sem fio como clientes RADIUS no servidor NPS. Ambos os métodos de acesso de rede exigem que você implantar componentes adicionais de hardware e software.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para criar políticas para 802.1 X com ou sem fio com um assistente

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS. 

2. Se ainda não estiver selecionada, clique em **NPS \(Local\)**. Se você quiser criar políticas em um servidor NPS remoto, selecione o servidor.

3. Em **Introdução** e **configuração padrão**, selecione **servidor RADIUS para 802.1 X acesso sem fio ou conexões de rede com fio**. O texto e os links abaixo do texto mudam para refletir sua seleção.

4. Clique em **configurar 802.1 X usando um assistente**. Abre o Assistente de nova IEEE 802.1 X com fio e seguras conexões sem fio.

5. Siga as instruções no Assistente para concluir a criação de seu novas políticas.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurar o NPS para ignorar as propriedades de discagem rápida da conta de usuário

Use este procedimento para configurar uma política de rede NPS para ignorar as propriedades de discagem rápida de contas de usuário no Active Directory durante o processo de autorização. Contas de usuário no Active Directory Users and Computers têm propriedades discagem que NPS avalia durante o processo de autorização, a menos que o **permissão de acesso de rede** propriedade da conta de usuário é definida como **controlar o acesso por meio da política de rede NPS**. 

Há duas circunstâncias em que convém configurar NPS para ignorar as propriedades de discagem rápida de contas de usuário no Active Directory:

- Quando você quer simplificar a autorização de NPS usando a política de rede, mas não todas as suas contas de usuário têm o **permissão de acesso de rede** propriedade definida como **controlar o acesso por meio da política de rede NPS**. Por exemplo, algumas contas de usuário podem ter o **permissão de acesso de rede** propriedade da conta do usuário definida como **negar acesso** ou **permitir acesso**.

- Quando as outras propriedades de discagem rápida de contas de usuário não são aplicáveis ao tipo de conexão é configurado na política de rede. Por exemplo, as propriedades diferente do **permissão de acesso de rede** configuração são aplicáveis somente a discagem ou conexões VPN, mas é a política de rede que você está criando conexões de comutador de autenticação ou sem fio.

Você pode usar este procedimento para configurar o NPS para ignorar as propriedades de discagem rápida da conta de usuário. Se uma solicitação de conexão corresponder a política de rede onde essa caixa de seleção for marcada, o NPS não usa as propriedades de discagem rápida da conta de usuário para determinar se o usuário ou computador está autorizado a acessar a rede; somente as configurações na política de rede são usadas para determinar a autorização.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.

2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo, pela **visão geral** guia, **permissão de acesso**, selecione o **Ignorar propriedades de discagem rápida da conta de usuário** caixa de seleção e, em seguida, clique em **Okey**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>A configuração do NPS para ignorar as propriedades de discagem rápida da conta de usuário



## <a name="configure-nps-for-vlans"></a>Configurar NPS para VLANs

Usando servidores de acesso à rede VLAN reconhecimento e NPS no Windows Server 2016, você pode fornecer grupos de usuários com acesso apenas aos recursos de rede que são apropriados para suas permissões de segurança. Por exemplo, você pode fornecer os visitantes acesso sem fio à Internet sem permitir o acesso à rede da organização. 

Além disso, VLANs permitem logicamente recursos de rede grupo que existe em diferentes locais físicos ou em várias sub-redes físicos. Por exemplo, membros do seu departamento de vendas e seus recursos de rede, como computadores cliente, servidores e impressoras, podem estar localizados em vários edifícios diferentes em sua organização, mas você pode colocar todos esses recursos em uma VLAN que usa o mesmo intervalo de endereços IP. O VLAN e funções, da perspectiva do usuário final, como uma única sub-rede.

Você também pode usar VLANs quando desejar separar a uma rede entre diferentes grupos de usuários. Depois de determinar como deseja definir seus grupos, você pode criar grupos de segurança no Active Directory usuários e computadores snap-in e adicionar membros dos grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configurar uma política de rede para VLANs

Você pode usar este procedimento para configurar uma política de rede que atribui os usuários obtenham uma. Quando você usa o reconhecimento de VLAN hardware de rede, como roteadores, comutadores e controladores de acesso, você pode configurar a política de rede para instruir os servidores de acesso para colocar os membros dos grupos específicos do Active Directory em VLANs específicas. Essa capacidade de recursos de rede de grupo logicamente com VLANs oferece flexibilidade ao projetar e implementar soluções de rede.

Quando você define as configurações de uma política de rede NPS para uso com VLANs, configure os atributos **túnel-Medium-Type**, **túnel-Pvt-Group-ID**, **tipo de encapsulamento**, e **túnel-Tag**. 

Esse procedimento é fornecido como uma diretriz; a configuração de rede pode exigir configurações diferentes daquelas descritas abaixo.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar uma política de rede para VLANs

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.

2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo, clique no **configurações** guia.

4. Na política **propriedades**, na **configurações**, na **atributos RADIUS**, certifique-se de que **padrão** é selecionado.

5. No painel de detalhes, em **atributos**, o **Service-Type** atributo está configurado com um valor padrão de **enquadramento**. Por padrão, para políticas com métodos de acesso de VPN e dial-up, o **enquadramento protocolo** atributo está configurado com um valor de **PPP**. Para especificar os atributos de conexão adicionais necessários para VLANs, clique em **adicionar**. O **Adicionar atributo de RAIO padrão** caixa de diálogo é aberta.

6. Em **Adicionar atributo de RAIO padrão**, em atributos, role para baixo e adicione os seguintes atributos:

    - **Tipo de mídia de túnel**. Selecione um valor apropriado para as seleções anteriores que você fez para a política. Por exemplo, se você estiver configurando a política de rede é uma diretiva sem fio, selecione **valor: 802 (mídia inclui todos os 802 mais formato canônico Ethernet)**.

    - **Túnel-Pvt-grupo-ID**. Insira o número inteiro que representa o número à qual serão atribuídos a membros do grupo. 

    - **Tipo de encapsulamento**. Selecione **LANs virtuais (VLAN)**.


7. Em **Adicionar atributo de RAIO padrão**, clique em **fechar**. 

8. Se o seu servidor de acesso de rede (NAS) requer o uso do **túnel-Tag** atributo, use as seguintes etapas para adicionar o **túnel-Tag** atributo para a política de rede. Se a documentação do NAS não mencionar esse atributo, não adicioná-lo à política. Se necessário, adicione os atributos da seguinte maneira:

    - Na política **propriedades**, na **configurações**, na **atributos RADIUS**, clique em **específica do fornecedor**. 

    - No painel de detalhes, clique em **adicionar**. O **Adicionar atributo específico do fornecedor** caixa de diálogo é aberta.

    - Em **atributos**, role para baixo e selecione **túnel-Tag**e clique em **adicionar**. O **informações de atributo** caixa de diálogo é aberta. 

    - Em **valor do atributo**, digite o valor que você obteve de documentação de hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar o tamanho da carga EAP

Em alguns casos, roteadores ou firewalls soltar pacotes porque eles são configurados para descartar pacotes que exigem a fragmentação.

Quando você implanta NPS com políticas de rede que usam o protocolo de autenticação extensível \(EAP\) com Transport Layer Security \(TLS\) ou EAP-TLS, como um método de autenticação, a unidade de transmissão máxima padrão \(MTU\) NPS usa para cargas EAP é 1500 bytes. 

Esse tamanho máximo de carga EAP pode criar mensagens de RAIO que exigem fragmentação por um roteador ou firewall entre o servidor NPS e um cliente RADIUS. Se esse for o caso, um roteador ou firewall posicionado entre o cliente RADIUS e o servidor NPS silenciosamente pode descartar algumas fragmentos, resultando em falhas de autenticação e a incapacidade de se conectar à rede do cliente de acesso.

Use o procedimento a seguir para reduzir o tamanho máximo que NPS usa para cargas EAP, ajustando o atributo MTU de enquadramento em uma política de rede para um valor não maior que 1344.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar o atributo MTU de enquadramento

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.
 
2. Clique duas vezes em **políticas**, clique em **políticas de rede**e, em seguida, no painel de detalhes, clique duas vezes na política que você deseja configurar.

3. Na política **propriedades** caixa de diálogo, clique no **configurações** guia.

4. Em **configurações**, na **atributos RADIUS**, clique em **padrão**. No painel de detalhes, clique em **adicionar**. O **Adicionar atributo de RAIO padrão** caixa de diálogo é aberta.

5. Em **atributos**, role para baixo e clique em **MTU de enquadramento**e clique em **adicionar**. O **informações de atributo** caixa de diálogo é aberta.

6. Em **valor de atributo**, digite um valor menor ou igual a que **1344**. Clique em **Okey**, clique em **fechar**e clique em **Okey**.



Para obter mais informações sobre políticas de rede, consulte [políticas de rede](nps-np-overview.md).

Para obter exemplos de sintaxe de padrões coincidentes para especificar atributos de política de rede, consulte [Use expressões regulares no NPS](nps-crp-reg-expressions.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
