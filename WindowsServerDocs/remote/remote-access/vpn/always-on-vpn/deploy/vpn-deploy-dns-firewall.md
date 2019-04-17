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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066968"
---
# Etapa 5. Definir as configurações de DNS e firewall

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)<br>
& #187;  [ **Próximo:** etapa 6. Configurar o cliente do Windows 10 sempre em conexões VPN](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você configura DNS e o Firewall configurações de conectividade VPN.

## Configurar resolução de nomes DNS

Quando os clientes remotos VPN se conecta, eles usam os mesmos servidores DNS que usam a seus clientes internos, que permite que eles resolver nomes da mesma maneira como o restante do seus estações de trabalho internas. 

Por isso, você deve garantir que o nome de computador que clientes externos usam para se conectar ao servidor VPN corresponde o nome alternativo do assunto definido em certificados emitidos para o servidor VPN.

Para garantir que os clientes remotos podem se conectar ao servidor VPN, você pode criar um registro DNS A (Host) na zona DNS externa. O registro deve usar o nome alternativo de assunto do certificado para o servidor VPN.


### Para adicionar um host \ (A ou AAAA\) registro de recurso a uma zona

1. Em um servidor DNS, no Gerenciador do servidor, clique em **Ferramentas**e, em seguida, clique em **DNS**. Abre o Gerenciador DNS.
2. Na árvore de console do Gerenciador DNS, clique no servidor que você deseja gerenciar.
3. No painel de detalhes, em **nome**, clique double\ **Zonas de pesquisa** para expandir a exibição.
4. Nos detalhes de **Zonas de pesquisa direta** , clique right\ a zona de pesquisa direta para o qual você deseja adicionar um registro e, em seguida, clique em **Novo Host \(A or AAAA\)**. Abre a caixa de diálogo **Novo Host** .
5. No **Novo Host**, em **nome**, digite o nome alternativo de assunto do certificado para o servidor VPN.
6. No endereço IP, digite o endereço IP para o servidor VPN. Você pode digitar o endereço IP versão 4 (IPv4) formato para adicionar um registro de recurso do host \(A\), ou IP versão 6 \(IPv6\) para adicionar um registro de recurso do host \(AAAA\).
7. Se você criou uma zona de pesquisa inversa para um intervalo de endereços IP, incluindo o endereço IP que você digitou, em seguida, marque a caixa de seleção **Criar registro de ponteiro associado** .  Esta opção cria um registro de recurso \(PTR\) de ponteiro adicional em uma zona inversa para esse host, com base nas informações fornecidas no **nome** e **endereço IP**.
8. Clique em **Adicionar Host**.

## Configurar o Firewall do Edge

O Edge Firewall separa a rede de perímetro externa da Internet pública. Para obter uma representação visual dessa separação, consulte a ilustração no tópico [Sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).

O Edge Firewall deve permitir e encaminhar portas específicas para o servidor VPN. Se você usar \(NAT\) conversão de endereços de rede em seu firewall de borda, você precisa ativar o encaminhamento de porta de ports500 User Datagram Protocol \(UDP\) e 4500. Encaminhe essas portas para o endereço IP atribuído à interface externa do seu servidor VPN.

Se você estiver roteamento de tráfego de entrada e executando a NAT na ou atrás do servidor VPN, em seguida, você deve abrir suas regras de firewall para permitir que ports500 UDP e 4500 de entrada para o endereço IP externo aplicado à interface pública no servidor VPN.

Em ambos os casos, se seu firewall dá suporte a profunda inspeção de pacotes e você tiver dificuldades para estabelecer conexões de cliente, você deve tentar relaxe ou desabilitar profunda inspeção de pacotes para sessões de IKE.

Para obter informações sobre como fazer essas alterações de configuração, consulte a documentação do firewall.

## Configurar o Firewall da rede de perímetro interno

O Firewall interno de rede de perímetro separa a rede da organização/corporativos da rede de perímetro interna. Para obter uma representação visual dessa separação, consulte a ilustração no tópico [Sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).

Nessa implantação, o servidor de acesso remoto VPN na rede de perímetro é configurado como um cliente RADIUS.  O servidor VPN envia o tráfego RADIUS para o NPS na rede corporativa e também recebe o tráfego RADIUS do NPS.

Configure o firewall para permitir o tráfego RADIUS fluam em ambas as direções.


>[!NOTE]
>O servidor NPS na rede corporativa/organização funciona como um servidor RADIUS para o servidor VPN, que é um cliente RADIUS. Para obter mais informações sobre a infraestrutura RADIUS, consulte o [Servidor de política de rede (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### Portas de tráfego RADIUS no servidor VPN e no servidor NPS

Por padrão, o NPS e VPN escutam tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se você habilitar o Firewall do Windows com segurança avançada ao instalar o NPS, exceções de firewall para essas portas são criadas automaticamente durante o processo de instalação para tráfego IPv4 e IPv6.

>[!IMPORTANT]
>Se seus servidores de acesso de rede são configurados para enviar o tráfego RADIUS pelas portas que não sejam esses padrões, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação do NPS e criar exceções para as portas de que você usa para Tráfego RADIUS.

### Usar as mesmas portas RADIUS para a configuração de Firewall de rede de perímetro interno

Se você usar a configuração de porta padrão RADIUS no servidor VPN e o servidor NPS, certifique-se de que você abra as seguintes portas no Firewall interno de rede de perímetro:

- Portas UDP1812, UDP1813, UDP1645 e UDP1646

Se você não estiver usando as portas RADIUS padrão na implantação NPS, você deve configurar o firewall para permitir o tráfego RADIUS nas portas que você está usando. Para obter mais informações, consulte [Configurar Firewalls para tráfego RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Próximas etapas
[Etapa 6. Configurar o Windows 10 sempre em conexões VPN cliente](vpn-deploy-client-vpn-connections.md): nesta etapa, você configura os computadores de clientes do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes VPN do Windows 10, incluindo o Windows PowerShell, System Center Configuration Manager e Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas. 

---
