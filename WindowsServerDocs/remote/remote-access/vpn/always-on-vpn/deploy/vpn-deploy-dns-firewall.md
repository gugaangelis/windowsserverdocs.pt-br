---
title: Definir as configurações de Firewall e DNS
description: Este tópico fornece instruções detalhadas para a implantação de Always On VPN no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: b1efa918971208bb9819de189de7298b4de144a5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871321"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Etapa 5. Definir configurações de DNS e firewall

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior** Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)
- [**Última** Etapa 6. Configurar as conexões da VPN Always On do cliente Windows 10](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você define as configurações de DNS e de firewall para conectividade VPN.

## <a name="configure-dns-name-resolution"></a>Configurar a resolução de nomes DNS

Quando os clientes VPN remotos se conectam, eles usam os mesmos servidores DNS que seus clientes internos usam, o que permite que eles resolvam nomes da mesma maneira que o restante das estações de trabalho internas.

Por isso, você deve garantir que o nome do computador que os clientes externos usam para se conectar ao servidor VPN corresponda ao nome alternativo da entidade definido em certificados emitidos para o servidor VPN.

Para garantir que os clientes remotos possam se conectar ao seu servidor VPN, você pode criar um registro (host) do DNS em sua zona DNS externa. O registro a deve usar o nome alternativo da entidade do certificado para o servidor VPN.

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Para adicionar um registro de recurso de host (A ou AAAA) a uma zona

1. Em um servidor DNS, em Gerenciador do Servidor, selecione **ferramentas**e, em seguida, selecione **DNS**. O Gerenciador DNS é aberto.
2. Na árvore de console do Gerenciador de DNS, selecione o servidor que você deseja gerenciar.
3. No painel de detalhes, em **nome**, clique duas vezes em **zonas de pesquisa direta** para expandir a exibição.
4. Em detalhes de **zonas de pesquisa direta** , clique com o botão direito do mouse na zona de pesquisa direta à qual você deseja adicionar um registro e, em seguida, selecione **novo host (a ou aaaa)** . A caixa de diálogo **novo host** é aberta.
5. Em **novo host**, em **nome**, insira o nome alternativo da entidade do certificado para o servidor VPN.
6. Em endereço IP, insira o endereço IP para o servidor VPN. Você pode inserir o endereço no formato IP versão 4 (IPv4) para adicionar um registro de recurso de host (A) ou formato de IP versão 6 (IPv6) para adicionar um registro de recurso de host (AAAA).
7. Se você criou uma zona de pesquisa inversa para um intervalo de endereços IP, incluindo o endereço IP que você inseriu, marque a caixa de seleção **criar registro de ponteiro associado (PTR)** .  A seleção dessa opção cria um registro de recurso de ponteiro (PTR) adicional em uma zona inversa para esse host, com base nas informações inseridas em **nome** e **endereço IP**.
8. Selecione **Adicionar host**.

## <a name="configure-the-edge-firewall"></a>Configurar o Firewall do Edge

O Firewall do Edge separa a rede de perímetro externa da Internet pública. Para uma representação visual dessa separação, consulte a ilustração no tópico [Always on visão geral da tecnologia VPN](../always-on-vpn-technology-overview.md).

O Firewall do Edge deve permitir e encaminhar portas específicas para o servidor VPN. Se você usar a NAT (conversão de endereços de rede) no firewall do Edge, talvez seja necessário habilitar o encaminhamento de porta para as portas 500 e 4500 do protocolo UDP. Encaminhe essas portas para o endereço IP atribuído à interface externa do servidor VPN.

Se você estiver roteando o tráfego de entrada e executando o NAT no servidor VPN ou atrás dele, deverá abrir suas regras de firewall para permitir as portas UDP 500 e 4500 de entrada para o endereço IP externo aplicado à interface pública no servidor VPN.

Em ambos os casos, se o firewall der suporte à inspeção profunda de pacotes e você tiver dificuldade para estabelecer conexões de cliente, você deverá tentar relaxar ou desabilitar a inspeção profunda de pacotes para sessões IKE.

Para obter informações sobre como fazer essas alterações de configuração, consulte a documentação do firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurar o firewall de rede de perímetro interno

O firewall de rede de perímetro interno separa a rede corporativa/organizacional da rede de perímetro interna. Para uma representação visual dessa separação, consulte a ilustração no tópico [Always on visão geral da tecnologia VPN](../always-on-vpn-technology-overview.md).

Nessa implantação, o servidor VPN de acesso remoto na rede de perímetro é configurado como um cliente RADIUS.  O servidor VPN envia o tráfego RADIUS para o NPS na rede corporativa e também recebe o tráfego RADIUS do NPS.

Configure o firewall para permitir que o tráfego RADIUS flua em ambas as direções.

>[!NOTE]
>O servidor NPS na organização/rede corporativa funciona como um servidor RADIUS para o servidor VPN, que é um cliente RADIUS. Para obter mais informações sobre a infraestrutura RADIUS, consulte [servidor de diretivas de rede (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Portas de tráfego RADIUS no servidor VPN e no servidor NPS

Por padrão, o NPS e a VPN escutam o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se você habilitar o Firewall do Windows com segurança avançada ao instalar o NPS, as exceções de firewall para essas portas serão criadas automaticamente durante o processo de instalação para o tráfego IPv6 e IPv4.

>[!IMPORTANT]
>Se os servidores de acesso à rede estiverem configurados para enviar tráfego RADIUS por portas diferentes desses padrões, remova as exceções criadas no firewall do Windows com segurança avançada durante a instalação do NPS e crie exceções para as portas que você usa para Tráfego RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Usar as mesmas portas RADIUS para a configuração de firewall de rede de perímetro interno

Se você usar a configuração de porta RADIUS padrão no servidor VPN e no servidor NPS, certifique-se de abrir as seguintes portas no firewall de rede de perímetro interno:

- Portas UDP1812, UDP1813, UDP1645 e UDP1646

Se você não estiver usando as portas RADIUS padrão em sua implantação do NPS, deverá configurar o firewall para permitir o tráfego RADIUS nas portas que você está usando. Para obter mais informações, consulte [Configurar firewalls para tráfego RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-steps"></a>Próximas etapas

[Etapa 6. Configurar conexões](vpn-deploy-client-vpn-connections.md)de VPN Always on cliente do Windows 10: Nesta etapa, você configura os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes de VPN do Windows 10, incluindo o Windows PowerShell, o System Center Configuration Manager e o Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas.