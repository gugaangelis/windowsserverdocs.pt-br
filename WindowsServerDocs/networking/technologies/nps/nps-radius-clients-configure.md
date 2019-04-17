---
title: Configurar clientes RADIUS
description: Este tópico fornece informações sobre como configurar clientes RADIUS para o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9af3189199c8282394ca34f181a90b4290c55044
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-radius-clients"></a>Configurar clientes RADIUS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para configurar os servidores de acesso à rede como clientes RADIUS no NPS.

Quando você adiciona um novo servidor de acesso de rede \ (servidor VPN, ponto de acesso sem fio, chave de autenticação ou servidor \ conexões discadas) à sua rede, você deve adicionar o servidor como um cliente RADIUS de NPS e, em seguida, configure o cliente RADIUS para se comunicar com o servidor NPS.

>[!IMPORTANT]
>Computadores cliente e dispositivos, como laptops, tablets, telefones e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede - como pontos de acesso sem fio 802.1 X capaz alterna, virtuais privadas rede servidores (VPN) e servidores dial-up - porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidores NPS \(NPS\).

Esta etapa também é necessária quando seu servidor NPS for membro de um grupo de servidores remotos RADIUS configurado em um proxy NPS. Neste caso, além de executar as etapas nesta tarefa no proxy NPS, você deve fazer o seguinte:

- No proxy NPS, configure um grupo de servidores RADIUS remoto que contém o servidor NPS.
- No servidor NPS remoto, configure o proxy NPS como um cliente RADIUS.

Para executar os procedimentos neste tópico, você deve ter pelo menos um servidor de acesso de rede \ (servidor VPN, ponto de acesso sem fio, chave de autenticação ou servidor \ dial-up) ou proxy NPS fisicamente instalado em sua rede.

## <a name="configure-the-network-access-server"></a>Configurar o servidor de acesso de rede

Use este procedimento para configurar os servidores de acesso à rede para uso com NPS. Quando você implanta servidores de acesso à rede (NASs) como clientes RADIUS, você deve configurar os clientes para se comunicar com os servidores NPS onde os NASs são configurados como clientes.

Esse procedimento fornece diretrizes gerais sobre as configurações que você deve usar para configurar seus NASs; Para obter instruções específicas sobre como configurar o dispositivo que você estiver implantando em sua rede, consulte a documentação do produto NAS.

### <a name="to-configure-the-network-access-server"></a>Para configurar o servidor de acesso de rede

1. NAS, em **configurações de RAIO**, selecione **autenticação RADIUS** na porta User Datagram Protocol (UDP) **1812** e **estatísticas RADIUS** na porta UDP **1813**.
2. Em **servidor de autenticação** ou **servidor RADIUS**, especifique seu servidor NPS pelo endereço IP ou nome de domínio totalmente qualificado (FQDN), dependendo dos requisitos da NAS. 
3. Em **segredo** ou **segredo compartilhado**, digite uma senha forte. Quando você configurar NAS como um cliente RADIUS de NPS, você usará a mesma senha, portanto, não se esqueça de-lo.
4. Se você estiver usando PEAP ou EAP como um método de autenticação, configure o para usar autenticação de EAP.
5. Se você estiver configurando um ponto de acesso sem fio, em **SSID**, especifique um \(SSID\) identificador do conjunto de serviço, que é uma cadeia de caracteres alfanumérica que serve como o nome da rede. Esse nome é transmitida por pontos de acesso para clientes sem fio e fica visível para os usuários em seus pontos de acesso sem fio fidelidade \(Wi-Fi\).
6. Se você estiver configurando um ponto de acesso sem fio, em **802.1 X e WPA**, habilitar **autenticação IEEE 802.1 X** se você deseja implantar PEAP-MS-CHAP v2, PEAP-TLS e EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Adicione o servidor de acesso de rede como um cliente RADIUS de NPS

Use este procedimento para adicionar um servidor de acesso de rede como um cliente RADIUS de NPS. Você pode usar este procedimento para configurar NAS como um cliente RADIUS usando o console NPS.

Para concluir este procedimento, você deve ser um membro do **administradores** grupo.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para adicionar um servidor de acesso de rede como um cliente RADIUS de NPS

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.
2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**. Clique com botão direito **clientes RADIUS**e clique em **novo cliente RADIUS**. 
3. Em **novo cliente RADIUS**, verifique o **habilitar este cliente RADIUS** caixa de seleção está marcada.
4. Em **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para NAS. Em **endereço (IP ou DNS)**, digite o endereço IP do NAS ou nome de domínio totalmente qualificado (FQDN). Se você inserir o FQDN, clique em **verificar** se você deseja verificar se o nome está correto e é mapeado para um endereço IP válido. 
5. Em **novo cliente RADIUS**, na **fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.
6. Em **novo cliente RADIUS**, na **segredo compartilhado**, siga um destes procedimentos:
    - Certifique-se de que **Manual** é selecionado e, em seguida, em **segredo compartilhado**, digite a senha forte que também é inserida no NAS. Digite novamente o segredo compartilhado no **confirmar o segredo compartilhado**.
    - Selecione **gerar**e clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração no para que ele possa se comunicar com o servidor NPS.
7. Em **novo cliente RADIUS**, na **opções adicionais**, se você estiver usando qualquer método de autenticação diferentes de EAP e PEAP e se o seu NAS permite o uso do atributo autenticador mensagem, selecione **mensagens de solicitação de acesso devem conter o atributo autenticador de mensagem**.
8. Clique em **Okey**. Seu NAS aparece na lista de clientes RADIUS configurados no servidor NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurar clientes RADIUS por intervalo de endereços IP no Windows Server 2016 Datacenter

Se você estiver executando o Windows Server 2016 Datacenter, você pode configurar clientes RADIUS de NPS por intervalo de endereços IP. Isso permite que você adicionar um grande número de clientes RADIUS (como pontos de acesso sem fio) para o console NPS ao mesmo tempo, em vez de adicionar cada cliente RADIUS individualmente.

Você não pode configurar clientes RADIUS por intervalo de endereços IP, se você estiver executando o NPS no Windows Server 2016 Standard.

Use este procedimento para adicionar um grupo de servidores de acesso à rede (NASs) como clientes RADIUS configurados com endereços IP do mesmo intervalo de endereços IP.

Todos os clientes RADIUS no intervalo devem usar a mesma configuração e segredo compartilhado.

Para concluir este procedimento, você deve ser um membro do **administradores** grupo.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Para configurar clientes RADIUS por intervalo de endereços IP

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.
2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**. Clique com botão direito **clientes RADIUS**e clique em **novo cliente RADIUS**.
3. Em **novo cliente RADIUS**, na **nome amigável**, digite um nome de exibição para a coleção de NASs.
4. Em **endereço \(IP or DNS\)**, digite o intervalo de endereços IP para os clientes RADIUS usando a notação CIDR \(CIDR\). Por exemplo, se o intervalo de endereços IP para os NASs 10.10.0.0, digite **10.10.0.0/16**.
5. Em **novo cliente RADIUS**, na **fornecedor**, especifique o nome do fabricante NAS. Se você não tiver certeza do nome do fabricante NAS, selecione **padrão RADIUS**.
6. Em **novo cliente RADIUS**, na **segredo compartilhado**, siga um destes procedimentos:
    - Certifique-se de que **Manual** é selecionado e, em seguida, em **segredo compartilhado**, digite a senha forte que também é inserida no NAS. Digite novamente o segredo compartilhado no **confirmar o segredo compartilhado**.
    - Selecione **gerar**e clique em **gerar** para gerar automaticamente um segredo compartilhado. Salve o segredo compartilhado gerado para a configuração no para que ele possa se comunicar com o servidor NPS.
7. Em **novo cliente RADIUS**, na **opções adicionais**, se você estiver usando qualquer método de autenticação diferentes de EAP e PEAP e se todos os seus NASs suportam o uso do atributo autenticador mensagem, selecione **mensagens de solicitação de acesso devem conter o atributo autenticador de mensagem**.
8. Clique em **Okey**. Seus NASs aparecem na lista de clientes RADIUS configurados no servidor NPS.

Para obter mais informações, consulte [clientes RADIUS](nps-radius-clients.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).