---
title: Verificar a configuração após as alterações NPS
description: Você pode usar este tópico para verificar a configuração de servidor de políticas de rede do Windows Server 2016 depois de alterar a um endereço IP ou nome para o servidor.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880667"
---
# <a name="verify-configuration-after-nps-changes"></a>Verificar a configuração após as alterações NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para verificar a configuração do NPS depois que um endereço IP ou nome de alterar para o servidor.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Verifique se a configuração após uma alteração de endereço IP do NPS

Pode haver circunstâncias em que você precisa alterar o endereço IP de um proxy, como quando você move o servidor para uma sub-rede IP diferente ou NPS. 

Se você alterar um endereço IP de proxy ou NPS, é necessário reconfigurar as partes da sua implantação do NPS. 

Use as seguintes diretrizes gerais para ajudar a verificar que uma alteração de endereço IP não interrompe a autenticação de acesso de rede, autorização ou contabilidade em sua rede para servidores NPS RADIUS e servidores de proxy RADIUS.

Você deve ser um membro da **administradores**, ou equivalente, para executar esses procedimentos.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Para verificar a configuração após uma alteração de endereço de IP de NPS

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do NPS.

2. Se o NPS é um membro de um grupo de servidores remotos RADIUS, reconfigure o proxy NPS com o novo endereço IP do NPS.

3. Se você tiver configurado o NPS para usar o log do SQL Server, verifique se a conectividade entre o computador executando o SQL Server e o NPS ainda está funcionando corretamente.

4. Se você tiver implantado o IPsec para proteger o tráfego do RADIUS entre o NPS e um proxy NPS ou outros servidores ou dispositivos, reconfigure a política de IPsec ou a regra de segurança de conexão no Firewall do Windows com segurança avançada para usar o novo endereço IP do NPS.

5. Se o NPS for multihomed e você tiver configurado o servidor para associar a um adaptador de rede específico, reconfigure as configurações de porta do NPS com o novo endereço IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para verificar a configuração após um NPS da alteração do endereço IP de proxy

1. Reconfigure todos os clientes RADIUS, como pontos de acesso sem fio e servidores VPN, com o novo endereço IP do proxy do NPS.

2. Se o proxy NPS for multihomed e configurou o proxy para associar a um adaptador de rede específico, reconfigure as configurações de porta do NPS com o novo endereço IP.

3. Reconfigure todos os membros de todos os grupos de servidores RADIUS remotos com o endereço IP do servidor proxy. Para realizar essa tarefa, em cada NPS com o proxy NPS configurado como um cliente RADIUS:

    a. Clique duas vezes em **NPS (Local)**, clique duas vezes em **clientes e servidores RADIUS**, clique em **clientes RADIUS**e, em seguida, no painel de detalhes, clique duas vezes o cliente RADIUS que você deseja alterar.

    b. No cliente RADIUS **propriedades**, na **endereço \(IP ou DNS\)**, digite o novo endereço IP do proxy do NPS.

4. Se você tiver configurado o proxy do NPS para usar o log do SQL Server, verifique se a conectividade entre o computador executando o SQL Server e o proxy NPS ainda está funcionando corretamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Verificar a configuração depois de renomear um NPS

Pode haver circunstâncias quando você precisar alterar o nome de um proxy, como quando você recriar as convenções de nomenclatura para seus servidores ou o NPS.

Se você alterar um nome de proxy ou NPS, é necessário reconfigurar as partes da sua implantação do NPS. 

Use as seguintes diretrizes gerais para ajudar a verificar que uma alteração de nome de servidor não interrompe a autenticação de acesso de rede, autorização ou contabilidade.

Você deve ser um membro da **administradores**, ou equivalente, para executar este procedimento.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Para verificar a configuração após uma alteração de nome de proxy ou NPS

1. Se o NPS é um membro de um grupo de servidores remotos RADIUS e o grupo é configurado com nomes de computador em vez de endereços IP, reconfigure o grupo de servidores remotos RADIUS com o novo nome do NPS.

2. Se os métodos de autenticação baseada em certificado são implantados no NPS, a alteração do nome invalida o certificado do servidor. Você pode solicitar um novo certificado do administrador de autoridade de certificação ou, se o computador for um computador membro do domínio e certificados de registro automático para membros do domínio, você pode atualizar a política de grupo para obter um novo certificado por meio do registro automático . Para atualizar a política de grupo:

    a. Abra o Prompt de comando ou o Windows PowerShell.

    b. Digite **gpupdate** e pressione ENTER.


3. Depois de ter um novo certificado do servidor, solicite que o administrador de autoridade de certificação revogar o certificado antigo. 

     Depois que o antigo certificado é revogado, o NPS continua a usá-lo até que o certificado antigo expira. Por padrão, o certificado antigo permanece válido por um período máximo de uma semana e 10 horas. Esse período de tempo pode ser diferente dependendo se a expiração da lista de revogação de certificados (CRL) e a expiração de tempo de cache de segurança de camada de transporte (TLS) foram modificados em relação aos padrões. A expiração CRL padrão é de uma semana; o padrão TLS em cache tempo de expiração é de 10 horas. 

     Se você quiser configurar o NPS para usar o novo certificado imediatamente, no entanto, você pode reconfigurar manualmente as diretivas de rede com o novo certificado.

4. Depois que o antigo certificado expira, o NPS inicia automaticamente usando o novo certificado. 

5. Se você tiver configurado o NPS para usar o log do SQL Server, verifique se a conectividade entre o computador executando o SQL Server e o NPS ainda está funcionando corretamente.

