---
title: Guia de laboratório de teste – demonstrar o DirectAccess em um cluster com o NLB do Windows
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c39111755af922a477475de180302da226966859
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951071"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guia de Teste de Laboratório: demonstrar o DirectAccess em um cluster com Windows NLB

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O acesso remoto é uma função de servidor nos sistemas operacionais Windows Server 2016, Windows Server 2012 R2 e Server 2012 que permite que os usuários remotos acessem com segurança recursos de rede internos usando DirectAccess ou VPN RRAS. Este guia contém instruções passo a passo para a extensão do [Guia de Teste de Laboratório: demonstrar a instalação de servidor único de DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar o Balanceamento de Carga de Rede do DirectAccess e a configuração de cluster.

## <a name="about-this-guide"></a>Sobre este guia
Este guia contém instruções para a configuração e demonstração do Acesso Remoto usando seis servidores e dois computadores clientes. O teste de laboratório de Acesso Remoto concluído com NLB simula uma intranet, a Internet e uma rede doméstica e demonstra a funcionalidade de Acesso Remoto em diferentes cenários de conexão à Internet.

> [!IMPORTANT]
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.

## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conhecidos
Os problemas a seguir são conhecidos quando se configura um cenário de cluster:

-   Depois de configurar o DirectAccess em uma implantação somente IPv4 com um único adaptador de rede e depois que o DNS64 padrão (o endereço IPv6 que contém ":3333::") for configurado automaticamente no adaptador de rede, a tentativa de habilitar o balanceamento de carga por meio do console de Gerenciamento de Acesso Remoto faz com que um prompt para o usuário forneça um DIP IPv6. Se for fornecido um DIP IPv6, ocorrerá falha na configuração depois de clicar em **Confirmar** com o erro: O parâmetro está incorreto.

    Para resolver o problema:

    1.  Baixe os scripts de backup e restauração em [Fazer Backup e Restauração da Configuração de Acesso Remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).

    2.  Faça backup dos seus GPOs de acesso remoto usando o script Backup-RemoteAccess.ps1 baixado

    3.  Tente habilitar o balanceamento de carga até a etapa em que ocorre a falha. Na caixa de diálogo Habilitar Balanceamento de Carga, expanda a área de detalhes, clique com o botão direito do mouse na área de detalhes e, em seguida, clique em **Copiar Script**.

    4.  Abra o Bloco de Notas e cole o conteúdo da área de transferência. Por exemplo:

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    5.  Feche todas as caixas de diálogo de Acesso Remoto abertas e feche o console de Gerenciamento de Acesso Remoto.

    6.  Edite o texto colado e remova os endereços IPv6. Por exemplo:

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    7.  Em uma janela elevada do PowerShell, execute o comando da etapa anterior.

    8.  Se o cmdlet falhar durante a execução (não devido a valores de entrada incorretos), execute o comando Restore-RemoteAccess.ps1 e siga as instruções para certificar-se de que a integridade da sua configuração original seja mantida.

    9. Agora é possível abrir o console de Gerenciamento de Acesso Remoto novamente.



