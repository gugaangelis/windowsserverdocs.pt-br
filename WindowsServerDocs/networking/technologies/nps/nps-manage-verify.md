---
title: Verificar a configuração após as alterações do NPS
description: Você pode usar este tópico para verificar a configuração do servidor de diretivas de rede do Windows Server 2016 depois que um endereço IP ou nome mudar para o servidor.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ba1f5f494228f6bdba22b300a2336fa6eb37690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405356"
---
# <a name="verify-configuration-after-nps-changes"></a>Verificar a configuração após as alterações do NPS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para verificar a configuração do NPS após a alteração de um endereço IP ou nome para o servidor.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Verificar a configuração após a alteração de um endereço IP do NPS

Pode haver circunstâncias em que você precisa alterar o endereço IP de um NPS ou proxy, como quando você move o servidor para uma sub-rede IP diferente. 

Se você alterar um endereço IP de proxy ou NPS, será necessário reconfigurar partes de sua implantação do NPS. 

Use as diretrizes gerais a seguir para ajudá-lo a verificar se uma alteração de endereço IP não interrompe a autenticação, autorização ou contabilização de acesso à rede em sua rede para servidores RADIUS NPS e servidores proxy RADIUS.

Você deve ser um membro de **Administradores**, ou equivalente, para executar esses procedimentos.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Para verificar a configuração após a alteração de um endereço IP do NPS

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do NPS.

2. Se o NPS for membro de um grupo de servidores RADIUS remotos, reconfigure o proxy NPS com o novo endereço IP do NPS.

3. Se você configurou o NPS para usar SQL Server registro em log, verifique se a conectividade entre o computador que está executando o SQL Server e o NPS ainda está funcionando corretamente.

4. Se você tiver implantado o IPsec para proteger o tráfego RADIUS entre o NPS e um proxy NPS ou outros servidores ou dispositivos, reconfigure a política IPsec ou a regra de segurança de conexão no firewall do Windows com segurança avançada para usar o novo endereço IP do NPS.

5. Se o NPS for de hospedagem múltipla e você tiver configurado o servidor para associar a um adaptador de rede específico, redefina as configurações de porta do NPS com o novo endereço IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para verificar a configuração após a alteração de um endereço IP de proxy NPS

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do proxy NPS.

2. Se o proxy NPS for de hospedagem múltipla e você tiver configurado o proxy para associar a um adaptador de rede específico, redefina as configurações de porta do NPS com o novo endereço IP.

3. Reconfigure todos os membros de todos os grupos de servidores remotos RADIUS com o endereço IP do servidor proxy. Para realizar essa tarefa, em cada NPS que tem o proxy NPS configurado como um cliente RADIUS:

    a. Clique duas vezes em **NPS (local)** , clique duas vezes em **clientes e servidores RADIUS**, clique em **clientes RADIUS**e, no painel de detalhes, clique duas vezes no cliente RADIUS que você deseja alterar.

    b. Em **Propriedades**do cliente RADIUS, em **endereço \(IP ou DNS @ no__t-3**, digite o novo endereço IP do proxy NPS.

4. Se você tiver configurado o proxy NPS para usar o log de SQL Server, verifique se a conectividade entre o computador que executa o SQL Server e o proxy NPS ainda está funcionando corretamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Verificar a configuração após renomear um NPS

Pode haver circunstâncias em que você precisa alterar o nome de um NPS ou proxy, como quando você remodela as convenções de nomenclatura para seus servidores.

Se você alterar um nome de proxy ou NPS, será necessário reconfigurar partes de sua implantação do NPS. 

Use as diretrizes gerais a seguir para ajudá-lo a verificar se uma alteração de nome de servidor não interrompe a autenticação, autorização ou contabilização de acesso à rede.

Você deve ser um membro de **Administradores**, ou equivalente, para executar esse procedimento.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Para verificar a configuração após a alteração de um NPS ou de um nome de proxy

1. Se o NPS for membro de um grupo de servidores remotos RADIUS e o grupo estiver configurado com nomes de computador em vez de endereços IP, reconfigure o grupo de servidores remotos RADIUS com o novo nome do NPS.

2. Se os métodos de autenticação baseados em certificado forem implantados no NPS, a alteração de nome invalida o certificado do servidor. Você pode solicitar um novo certificado do administrador da autoridade de certificação (CA) ou, se o computador for um computador membro do domínio e registrar automaticamente os certificados para membros do domínio, você poderá atualizar Política de Grupo para obter um novo certificado por meio do registro automático . Para atualizar Política de Grupo:

    a. Abra o prompt de comando ou o Windows PowerShell.

    b. Digite **gpupdate** e pressione ENTER.


3. Depois de ter um novo certificado de servidor, solicite que o administrador da autoridade de certificação revogue o certificado antigo. 

     Depois que o certificado antigo for revogado, o NPS continuará a usá-lo até que o certificado antigo expire. Por padrão, o certificado antigo permanece válido por um tempo máximo de uma semana e 10 horas. Esse período de tempo pode ser diferente dependendo se a expiração da CRL (lista de certificados revogados) e a expiração do tempo de cache da TLS (segurança da camada de transporte) foram modificadas a partir de seus padrões. A expiração da CRL padrão é de uma semana; a expiração do tempo de cache TLS padrão é de 10 horas. 

     Se você quiser configurar o NPS para usar o novo certificado imediatamente, no entanto, você pode reconfigurar manualmente as políticas de rede com o novo certificado.

4. Depois que o certificado antigo expira, o NPS começa automaticamente a usar o novo certificado. 

5. Se você configurou o NPS para usar SQL Server registro em log, verifique se a conectividade entre o computador que está executando o SQL Server e o NPS ainda está funcionando corretamente.

