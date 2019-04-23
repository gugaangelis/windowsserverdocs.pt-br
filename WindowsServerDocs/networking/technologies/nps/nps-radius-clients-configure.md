---
title: Configurar clientes RADIUS
description: Este tópico fornece informações sobre como configurar clientes RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839487"
---
# <a name="configure-radius-clients"></a>Configurar clientes RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para configurar servidores de acesso à rede como clientes RADIUS no NPS.

Quando você adiciona um novo servidor de acesso de rede \(servidor VPN, o ponto de acesso sem fio, chave de autenticação ou servidor dial-up\) à sua rede, você deve adicionar o servidor como um cliente RADIUS no NPS e, em seguida, configure o cliente RADIUS se comunicar com o NPS.

>[!IMPORTANT]
>Computadores cliente e dispositivos, como laptops, tablets, telefones e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso de rede - como pontos de acesso sem fio, comutadores de compatíveis com 802.1X, virtuais privadas rede servidores (VPN) e servidores dial-up - porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como a diretiva de rede Servidor \(NPS\) servidores.

Esta etapa também é necessária quando o NPS é um membro de um grupo de servidores remotos RADIUS configurado em um proxy do NPS. Nessa circunstância, além de realizar as etapas nessa tarefa no proxy do NPS, faça o seguinte:

- No proxy do NPS, configure um grupo de servidores remotos RADIUS que contém o NPS.
- No NPS remoto, configure o proxy NPS como um cliente RADIUS.

Para executar os procedimentos neste tópico, você deve ter pelo menos um servidor de acesso de rede \(servidor VPN, o ponto de acesso sem fio, chave de autenticação ou servidor dial-up\) ou proxy NPS fisicamente instalado em sua rede.

## <a name="configure-the-network-access-server"></a>Configurar o servidor de acesso de rede

Use este procedimento para configurar os servidores de acesso de rede para uso com o NPS. Quando você implanta servidores de acesso de rede (NASs) como clientes RADIUS, você deve configurar os clientes para se comunicar com os NPSs onde NASs são configurados como clientes.

Esse procedimento fornece diretrizes gerais sobre as configurações que você deve usar para configurar seu NASs; Para obter instruções específicas sobre como configurar o dispositivo que você está implantando em sua rede, consulte a documentação do produto NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar o servidor de acesso de rede

1. NAS, na **configurações de RADIUS**, selecione **autenticação RADIUS** na porta de protocolo UDP (User Datagram) **1812** e **contabilidade RADIUS**na porta UDP **1813**.
2. Na **servidor de autenticação** ou **servidor RADIUS**, especifique seu endereço IP ou nome de domínio totalmente qualificado (FQDN), dependendo dos requisitos de do NPS. 
3. Na **segredo** ou **segredo compartilhado**, digite uma senha forte. Quando você configura NAS como um cliente RADIUS no NPS, você usará a mesma senha, portanto, não se esqueça de-lo.
4. Se você estiver usando PEAP ou EAP como um método de autenticação, configure as para usar a autenticação de EAP.
5. Se você estiver configurando um ponto de acesso sem fio, em **SSID**, especifique um identificador do conjunto de serviço \(SSID\), que é uma cadeia de caracteres alfanumérica que serve como o nome da rede. Esse nome é transmitida por pontos de acesso para clientes sem fio e é visível para os usuários com sua fidelidade sem fio \(Wi-Fi\) pontos problemáticos.
6. Se você estiver configurando um ponto de acesso sem fio, em **802.1 X e WPA**, habilite **autenticação IEEE 802.1X** se você deseja implantar o PEAP-MS-CHAP v2, PEAP-TLS ou EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Adicione o servidor de acesso de rede como um cliente RADIUS no NPS

Use este procedimento para adicionar um servidor de acesso de rede como um cliente RADIUS no NPS. Você pode usar este procedimento para configurar NAS como um cliente RADIUS usando o console do NPS.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso de rede como um cliente RADIUS no NPS

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.
2. No console do NPS, clique duas vezes **clientes e servidores RADIUS**. Clique com botão direito **clientes RADIUS**e, em seguida, clique em **novo cliente RADIUS**. 
3. No **novo cliente RADIUS**, verifique se que o **habilitar este cliente RADIUS** caixa de seleção está selecionada.
4. Na **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para NAS. Na **endereço (IP ou DNS)**, digite o endereço IP NAS ou o nome de domínio totalmente qualificado (FQDN). Se você inserir o FQDN, clique em **Verify** se você deseja verificar se o nome está correto e é mapeado para um endereço IP válido. 
5. Na **novo cliente RADIUS**, na **fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.
6. Na **novo cliente RADIUS**, na **segredo compartilhado**, faça o seguinte:
    - Certifique-se de que **Manual** está selecionada e, em seguida, na **segredo compartilhado**, digite a senha forte que também é inserida NAS. Digite novamente o segredo compartilhado no **Confirmar segredo compartilhado**.
    - Selecione **Generate**e, em seguida, clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração para que ele possa se comunicar com o NPS.
7. Na **novo cliente RADIUS**, na **opções adicionais**, se você estiver usando qualquer método de autenticação que não seja EAP e PEAP e se o seu NAS dá suporte ao uso do atributo de autenticador de mensagem, selecione **Mensagens de solicitação de acesso devem conter o atributo de autenticador de mensagem**.
8. Clique em **OK**. Seu NAS é exibida na lista de clientes RADIUS configurado no NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar clientes RADIUS por intervalo de endereços IP no Windows Server 2016 Datacenter

Se você estiver executando o Windows Server 2016 Datacenter, você pode configurar clientes RADIUS no NPS por intervalo de endereços IP. Isso permite que você adicione um grande número de clientes RADIUS (por exemplo, pontos de acesso sem fio) para o console do NPS ao mesmo tempo, em vez de adicionar cada cliente RADIUS individualmente.

Não é possível configurar clientes RADIUS por intervalo de endereços IP, se você estiver executando o NPS no Windows Server 2016 Standard.

Use este procedimento para adicionar um grupo de servidores de acesso de rede (NASs) como clientes RADIUS que são configurados com endereços IP do mesmo intervalo de endereços IP.

Todos os clientes RADIUS no intervalo devem usar a mesma configuração e o segredo compartilhado.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar clientes RADIUS por intervalo de endereços IP

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.
2. No console do NPS, clique duas vezes **clientes e servidores RADIUS**. Clique com botão direito **clientes RADIUS**e, em seguida, clique em **novo cliente RADIUS**.
3. Na **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para a coleção de NASs.
4. Na **endereço \(IP ou DNS\)**, digite o intervalo de endereços IP para os clientes RADIUS usando o roteamento entre domínios \(CIDR\) notação. Por exemplo, se o intervalo de endereços IP para os NASs for 10.10.0.0, digite **10.10.0.0/16**.
5. Na **novo cliente RADIUS**, na **fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.
6. Na **novo cliente RADIUS**, na **segredo compartilhado**, faça o seguinte:
    - Certifique-se de que **Manual** está selecionada e, em seguida, na **segredo compartilhado**, digite a senha forte que também é inserida NAS. Digite novamente o segredo compartilhado no **Confirmar segredo compartilhado**.
    - Selecione **Generate**e, em seguida, clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração para que ele possa se comunicar com o NPS.
7. Na **novo cliente RADIUS**, na **opções adicionais**, se você estiver usando qualquer método de autenticação que não seja EAP e PEAP e se todos os seus NASs suporte ao uso do atributo de autenticador de mensagem, selecione  **Mensagens de solicitação de acesso devem conter o atributo de autenticador de mensagem**.
8. Clique em **OK**. Seu NASs aparecem na lista de clientes RADIUS configurado no NPS.

Para obter mais informações, consulte [clientes RADIUS](nps-radius-clients.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).