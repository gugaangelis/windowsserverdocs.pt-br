---
title: Configurar clientes RADIUS
description: Este tópico fornece informações sobre como configurar clientes RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1992470a643a1633479940c1a0f98f8e7e67f5c1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951957"
---
# <a name="configure-radius-clients"></a>Configurar clientes RADIUS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para configurar servidores de acesso à rede como clientes RADIUS no NPS.

Ao adicionar um novo servidor VPN do servidor de acesso à rede \( , ponto de acesso sem fio, comutador de autenticação ou servidor dial-up \) à sua rede, você deve adicionar o servidor como um cliente RADIUS no NPS e, em seguida, configurar o cliente RADIUS para se comunicar com o NPS.

>[!IMPORTANT]
>Os computadores e dispositivos cliente, como computadores laptop, tablets, telefones e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, comutadores compatíveis com 802.1 X, servidores de rede virtual privada (VPN) e servidores dial-up, porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidores NPS de servidor de políticas de rede \( \) .

Essa etapa também é necessária quando o NPS é membro de um grupo de servidores RADIUS remotos configurado em um proxy NPS. Nessa circunstância, além de executar as etapas nesta tarefa no proxy NPS, você deve fazer o seguinte:

- No proxy NPS, configure um grupo de servidores RADIUS remoto que contenha o NPS.
- No NPS remoto, configure o proxy NPS como um cliente RADIUS.

Para executar os procedimentos deste tópico, você deve ter pelo menos um servidor VPN do servidor de acesso à rede \( , ponto de acesso sem fio, comutador de autenticação ou servidor de conexão discada \) ou proxy do NPS fisicamente instalado em sua rede.

## <a name="configure-the-network-access-server"></a>Configurar o servidor de acesso à rede

Use este procedimento para configurar servidores de acesso à rede para uso com o NPS. Ao implantar servidores de acesso à rede (NASs) como clientes RADIUS, você deve configurar os clientes para se comunicarem com o NPSs onde os NASs são configurados como clientes.

Este procedimento fornece diretrizes gerais sobre as configurações que você deve usar para configurar seus NASs; para obter instruções específicas sobre como configurar o dispositivo que você está implantando em sua rede, consulte a documentação do produto NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar o servidor de acesso à rede

1. No NAS, em **configurações de RADIUS**, selecione **autenticação RADIUS** na porta UDP (User datagram Protocol) **1812** e **contabilização RADIUS** na porta UDP **1813**.
2. No **servidor de autenticação** ou **servidor RADIUS**, especifique o NPS por endereço IP ou FQDN (nome de domínio totalmente qualificado), dependendo dos requisitos do nas.
3. Em **segredo** ou **segredo compartilhado**, digite uma senha forte. Ao configurar o NAS como um cliente RADIUS no NPS, você usará a mesma senha, portanto, não a esqueça.
4. Se você estiver usando PEAP ou EAP como um método de autenticação, configure o NAS para usar a autenticação EAP.
5. Se você estiver configurando um ponto de acesso sem fio, em **SSID**, especifique um identificador de conjunto de serviços \( SSID \) , que é uma cadeia de caracteres alfanumérica que serve como o nome da rede. Esse nome é transmitido por pontos de acesso para clientes sem fio e é visível para os usuários em seus \( hotspots Wi-Fi sem fio de fidelidade \) .
6. Se você estiver configurando um ponto de acesso sem fio, no **802.1 x e no WPA**, habilite a **autenticação IEEE 802.1 x** se desejar implantar PEAP-MS-CHAP v2, PEAP-TLS ou EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Adicionar o servidor de acesso à rede como um cliente RADIUS no NPS

Use este procedimento para adicionar um servidor de acesso à rede como um cliente RADIUS no NPS. Você pode usar este procedimento para configurar um NAS como um cliente RADIUS usando o console do NPS.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso à rede como um cliente RADIUS no NPS

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.
2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**. Clique com o botão direito do mouse em **clientes RADIUS**e clique em **novo cliente RADIUS**.
3. Em **novo cliente RADIUS**, verifique se a caixa de seleção **habilitar este cliente RADIUS** está marcada.
4. Em **novo cliente RADIUS**, em **nome amigável**, digite um nome de exibição para o nas. Em **Endereço (IP ou DNS)**, digite o endereço IP do nas ou o FQDN (nome de domínio totalmente qualificado). Se você inserir o FQDN, clique em **verificar** se deseja verificar se o nome está correto e se mapeia para um endereço IP válido.
5. Em **novo cliente RADIUS**, em **fornecedor**, especifique o nome do fabricante do nas. Se você não tiver certeza do nome do fabricante do NAS, selecione **RADIUS padrão**.
6. Em **novo cliente RADIUS**, em **segredo compartilhado**, siga um destes procedimentos:
    - Verifique se a seleção **manual** está selecionada e, em **segredo compartilhado**, digite a senha forte que também é inserida no nas. Digite novamente o segredo compartilhado em **confirmar segredo compartilhado**.
    - Selecione **gerar**e, em seguida, clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração no NAS para que ele possa se comunicar com o NPS.
7. Em **novo cliente RADIUS**, em **Opções adicionais**, se você estiver usando qualquer método de autenticação diferente de EAP e PEAP e se o seu nas der suporte ao uso do atributo autenticador de mensagem, selecione **as mensagens de solicitação de acesso devem conter o atributo autenticador de mensagem**.
8. Clique em **OK**. Seu NAS aparece na lista de clientes RADIUS configurados no NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar clientes RADIUS por intervalo de endereços IP no Windows Server 2016 datacenter

Se você estiver executando o Windows Server 2016 datacenter, poderá configurar clientes RADIUS no NPS por intervalo de endereços IP. Isso permite que você adicione um grande número de clientes RADIUS (como pontos de acesso sem fio) ao console do NPS ao mesmo tempo, em vez de adicionar cada cliente RADIUS individualmente.

Você não poderá configurar clientes RADIUS por intervalo de endereços IP se estiver executando o NPS no Windows Server 2016 Standard.

Use este procedimento para adicionar um grupo de servidores de acesso à rede (NASs) como clientes RADIUS que são todos configurados com endereços IP do mesmo intervalo de endereços IP.

Todos os clientes RADIUS no intervalo devem usar a mesma configuração e segredo compartilhado.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar clientes RADIUS por intervalo de endereços IP

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.
2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**. Clique com o botão direito do mouse em **clientes RADIUS**e clique em **novo cliente RADIUS**.
3. Em **novo cliente RADIUS**, em **nome amigável**, digite um nome de exibição para a coleção de Nass.
4. Em **endereço \( IP ou DNS \) **, digite o intervalo de endereços IP para os clientes RADIUS usando a \( notação CIDR de roteamento entre domínios sem classificação \) . Por exemplo, se o intervalo de endereços IP para os NASs for 10.10.0.0, digite **10.10.0.0/16**.
5. Em **novo cliente RADIUS**, em **fornecedor**, especifique o nome do fabricante do nas. Se você não tiver certeza do nome do fabricante do NAS, selecione **RADIUS padrão**.
6. Em **novo cliente RADIUS**, em **segredo compartilhado**, siga um destes procedimentos:
    - Verifique se a seleção **manual** está selecionada e, em **segredo compartilhado**, digite a senha forte que também é inserida no nas. Digite novamente o segredo compartilhado em **confirmar segredo compartilhado**.
    - Selecione **gerar**e, em seguida, clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração no NAS para que ele possa se comunicar com o NPS.
7. Em **novo cliente RADIUS**, em **Opções adicionais**, se você estiver usando qualquer método de autenticação diferente de EAP e PEAP, e se todos os seus Nass oferecerem suporte ao uso do atributo autenticador de mensagem, selecione **as mensagens de solicitação de acesso devem conter o atributo autenticador de mensagem**.
8. Clique em **OK**. Seus NASs aparecem na lista de clientes RADIUS configurados no NPS.

Para obter mais informações, consulte [clientes RADIUS](nps-radius-clients.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).