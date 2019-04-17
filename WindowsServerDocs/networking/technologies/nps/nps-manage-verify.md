---
title: Verifique se a configuração depois de alterações no servidor NPS
description: Você pode usar este tópico para verificar a configuração do servidor de política de rede do Windows Server 2016 após mudança de um endereço IP ou nome para o servidor.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>Verifique se a configuração depois de alterações no servidor NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para verificar a configuração do servidor NPS após mudança de um endereço IP ou nome para o servidor.

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>Verifique se a configuração após uma alteração de endereço IP de servidor NPS

Pode haver situações em que você precisa alterar o endereço IP de um proxy, como quando você move o servidor para uma sub-rede IP diferente ou servidor NPS. 

Se você alterar um endereço IP de proxy ou servidor NPS, é necessário reconfigurar partes da implantação do NPS. 

Use as seguintes diretrizes gerais para ajudá-lo a verificar que uma alteração de endereço IP não interrompam contabilidade em sua rede para servidores RADIUS de NPS e servidores proxy RADIUS, autorização ou autenticação acesso à rede.

Você deve ser um membro do **administradores**, ou equivalente, para executar esses procedimentos.

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>Para verificar a configuração após um NPS da alteração do endereço IP de servidor

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do servidor NPS.

2. Se o servidor NPS é um membro de um grupo de servidores remotos RADIUS, reconfigure o proxy NPS com o novo endereço IP do servidor NPS.

3. Se você tiver configurado o servidor NPS para usar o log do SQL Server, verifique se a conectividade entre o computador que esteja executando o SQL Server e o servidor NPS ainda está funcionando corretamente.

4. Se você tiver implantado o IPsec para proteger o tráfego RADIUS entre o servidor NPS e um proxy NPS ou outros servidores ou dispositivos, reconfigure a diretiva IPsec ou a regra de segurança de conexão no Firewall do Windows com segurança avançada para usar o novo endereço IP do servidor NPS.

5. Se o servidor NPS for de hospedagem múltipla e você tiver configurado o servidor para associar a um adaptador de rede específica, redefinir as configurações de porta NPS com o novo endereço IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para verificar a configuração após um NPS da alteração do endereço IP de proxy

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do proxy NPS.

2. Se o proxy NPS for de hospedagem múltipla e você tiver configurado o proxy para associar a um adaptador de rede específica, redefinir as configurações de porta NPS com o novo endereço IP.

3. Reconfigure todos os membros de todos os grupos de servidores RADIUS remotos com o endereço IP do servidor proxy. Para realizar essa tarefa, em cada servidor NPS que tem o proxy NPS configurado como um cliente RADIUS:

    um. Clique duas vezes em **NPS (Local)**, clique duas vezes em **clientes e servidores RADIUS**, clique em **clientes RADIUS**e, em seguida, no painel de detalhes, clique duas vezes o cliente RADIUS que você deseja alterar.

    b. No cliente RADIUS **propriedades**, na **endereço \(IP or DNS\)**, digite o novo endereço IP do proxy NPS.

4. Se você tiver configurado o proxy NPS para usar o log do SQL Server, verifique se a conectividade entre o computador que esteja executando o SQL Server e o proxy NPS ainda está funcionando corretamente.

## <a name="verify-configuration-after-renaming-an-nps-server"></a>Verifique se a configuração depois renomeando um servidor NPS

Pode haver situações quando você precisa alterar o nome de um servidor NPS ou proxy, como quando você reprojetar as convenções de nomenclatura para seus servidores.

Se você alterar um nome de proxy ou servidor NPS, é necessário reconfigurar partes da implantação do NPS. 

Use as seguintes diretrizes gerais para ajudá-lo a verificar que uma alteração de nome de servidor não interrompam contabilidade, autorização ou autenticação acesso à rede.

Você deve ser um membro do **administradores**, ou equivalente, para executar este procedimento.

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>Para verificar a configuração após uma alteração de nome de servidor ou proxy NPS

1. Se o servidor NPS é um membro de um grupo de servidores remotos RADIUS e o grupo está configurado com nomes de computador em vez de endereços IP, reconfigure o grupo de servidores remotos RADIUS com o novo nome do servidor NPS.

2. Se os métodos de autenticação baseada em certificado são implantados no servidor NPS, a alteração de nome invalida o certificado do servidor. Você pode solicitar um novo certificado com o administrador de autoridade de certificação ou, se o computador for um computador membro do domínio e certificados de registro automático para membros do domínio, você pode atualizar a política de grupo para obter um novo certificado através do registro automático. Para atualizar a política de grupo:

    um. Abra o Prompt de comando ou o Windows PowerShell.

    b. Tipo **gpupdate**, e pressione ENTER.


3. Depois de ter um novo certificado de servidor, solicite que o administrador da autoridade de certificação revogar o certificado antigo. 

     Depois que o certificado antigo é revogado, o NPS continua a usá-lo até que o certificado antigo expire. Por padrão, o certificado antigo permanece válido por um tempo máximo de uma semana e 10 horas. Esse período de tempo pode ser diferente dependendo se o vencimento da lista de revogação de certificados (CRL) e a expiração de tempo de cache Transport Layer Security (TLS) foram modificados de seus padrões. A expiração CRL padrão é uma semana; o padrão TLS cache tempo de expiração é de 10 horas. 

     Se você deseja configurar NPS para usar o novo certificado imediatamente, no entanto, você pode reconfigurar manualmente políticas de rede com o novo certificado.

4. Depois que o certificado antigo expira, o NPS começa automaticamente usando o novo certificado. 

5. Se você tiver configurado o servidor NPS para usar o log do SQL Server, verifique se a conectividade entre o computador que esteja executando o SQL Server e o servidor NPS ainda está funcionando corretamente.

