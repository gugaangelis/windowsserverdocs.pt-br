---
title: Definir as configurações de Firewall e DNS
description: Este tópico fornece instruções detalhadas para a implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886247"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Etapa 5. Definir configurações de DNS e firewall

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)<br>
&#187;  [**Next:** Etapa 6. Configurar o cliente do Windows 10 sempre em conexões VPN](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você configura o DNS e o Firewall configurações para conectividade VPN.

## <a name="configure-dns-name-resolution"></a>Configurar a resolução DNS

Quando se conectar a clientes remotos VPN, eles usam os mesmos servidores DNS que usam seus clientes internos, que lhes permite resolver os nomes da mesma maneira que o restante das suas estações de trabalho internos. 

Por isso, você deve verificar se o nome do computador que os clientes externos utilizam para se conectar ao servidor VPN corresponde ao nome alternativo da entidade definido em certificados emitidos para o servidor VPN.

Para garantir que clientes remotos podem se conectar ao seu servidor VPN, você pode criar um registro DNS A (Host) em sua zona DNS externa. O registro deve usar o nome alternativo de assunto do certificado para o servidor VPN.


### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Para adicionar um host \(A ou AAAA\) registro de recurso a uma zona

1. Em um servidor DNS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **DNS**. O Gerenciador de DNS é aberto.
2. Na árvore de console Gerenciador DNS, clique no servidor que você deseja gerenciar.
3. No painel de detalhes, na **nome**, double\-clique em **zonas de pesquisa direta** para expandir a exibição.
4. Na **zonas de pesquisa direta** de detalhes direito\-clique na zona de pesquisa direta para o qual você deseja adicionar um registro e, em seguida, clique em **novo Host \(A ou AAAA\)**. O **novo Host** caixa de diálogo é aberta.
5. Na **novo Host**, na **nome**, digite o nome alternativo de assunto do certificado para o servidor VPN.
6. Em endereço IP, digite o endereço IP para o servidor VPN. Você pode digitar o endereço IP versão 4 (IPv4) formato para adicionar um host \(um\) registro de recurso ou IP versão 6 \(IPv6\) formato para adicionar um host \(AAAA\) registro de recurso.
7. Se você criou uma zona de pesquisa inversa para um intervalo de endereços IP, incluindo o endereço IP que você digitou, em seguida, selecione a **criar registro de ponteiro (PTR) associado** caixa de seleção.  Esta opção cria um ponteiro adicional \(PTR\) registro de recurso em uma ordem inversa de zona para esse host, com base nas informações você inseriu no **nome** e **endereço IP**.
8. Clique em **Adicionar Host**.

## <a name="configure-the-edge-firewall"></a>Configurar o Firewall de borda

Firewall de borda separa a rede de perímetro externo da Internet pública. Para obter uma representação visual dessa separação, consulte a ilustração no tópico [sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).

Seu Firewall de borda deve permitir e encaminhar portas específicas para o servidor VPN. Se você usar a conversão de endereços de rede \(NAT\) em seu firewall de borda, talvez você precise habilitar o encaminhamento de porta para o protocolo de datagrama de usuário \(UDP\) portas 500 e 4500. Encaminhe essas portas para o endereço IP que é atribuído à interface externa do seu servidor VPN.

Se você estiver roteando o tráfego de entrada e executando a NAT no ou por trás do servidor VPN, em seguida, você deve abrir suas regras de firewall para permitir o UDP de entrada portas 500 e 4500 para o endereço IP externo aplicado à interface pública no servidor VPN.

Em ambos os casos, se seu firewall dá suporte à inspeção profunda de pacotes e você tiver dificuldade para estabelecer conexões de cliente, você deve tentar relaxar ou desabilitar as inspeções profundas para sessões de IKE.

Para obter informações sobre como fazer essas alterações de configuração, consulte a documentação do firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurar o Firewall de rede de perímetro interna

O Firewall interno de rede de perímetro separa da rede corporativa/organização da rede de perímetro interna. Para obter uma representação visual dessa separação, consulte a ilustração no tópico [sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).

Nessa implantação, o servidor de acesso remoto VPN na rede de perímetro é configurado como um cliente RADIUS.  O servidor VPN envia o tráfego RADIUS para o NPS na rede corporativa e também recebe o tráfego RADIUS do NPS.

Configure o firewall para permitir o tráfego RADIUS fluir em ambas as direções.


>[!NOTE]
>O servidor NPS na rede corporativa/organização funciona como um servidor RADIUS para o servidor VPN, que é um cliente RADIUS. Para obter mais informações sobre a infraestrutura RADIUS, consulte [servidor de diretivas de rede (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Portas de tráfego do RADIUS no servidor VPN e no servidor NPS

Por padrão, o NPS e VPN escutam tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se você habilitar o Firewall do Windows com segurança avançada ao instalar o NPS, as exceções de firewall para essas portas são criadas automaticamente durante o processo de instalação para o tráfego IPv6 e IPv4.

>[!IMPORTANT]
>Se seus servidores de acesso de rede estiverem configurados para enviar tráfego RADIUS em portas que não sejam essa padrão, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação do NPS e crie exceções para as portas que você usa para Tráfego RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Use as mesmas portas RADIUS para a configuração de Firewall de rede de perímetro interna

Se você usar a configuração de porta padrão do RADIUS no servidor VPN e o servidor NPS, certifique-se de que você abrir as seguintes portas no Firewall interno de rede de perímetro:

- Portas UDP1812, UDP1813, UDP1645 e UDP1646

Se você não estiver usando as portas RADIUS padrão em sua implantação do NPS, você deve configurar o firewall para permitir o tráfego RADIUS nas portas que você está usando. Para obter mais informações, consulte [configurar Firewalls para o tráfego RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-step"></a>Próximas etapas
[Etapa 6. Configurar o cliente do Windows 10 sempre em conexões VPN](vpn-deploy-client-vpn-connections.md): Nesta etapa, você deve configurar os computadores de cliente do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes de VPN do Windows 10, incluindo o Windows PowerShell, o System Center Configuration Manager e o Intune. Todos os três exigem um perfil VPN XML para definir as configurações de VPN apropriadas. 

---
