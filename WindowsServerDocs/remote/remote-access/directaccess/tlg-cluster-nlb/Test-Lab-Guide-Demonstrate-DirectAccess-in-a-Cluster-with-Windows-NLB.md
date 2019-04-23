---
title: 'Guia de laboratório de teste: demonstrar o DirectAccess em um Cluster com NLB do Windows'
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 910afa78553c828aff954f7677869569068198aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869517"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guia do laboratório de teste: Demonstrar o DirectAccess em um Cluster com NLB do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Acesso remoto é uma função de servidor no Windows Server 2016, Windows Server 2012 R2 e sistemas operacionais Server 2012 que permite que usuários remotos acessem com segurança os recursos da rede interna usando DirectAccess ou RRAS VPN. Este guia contém instruções passo a passo para estender o [guia do laboratório de teste: Demonstrar a instalação de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar o balanceamento de carga de rede do DirectAccess e a configuração de cluster.  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia contém instruções para a configuração e demonstração do Acesso Remoto usando seis servidores e dois computadores clientes. O teste de laboratório de Acesso Remoto concluído com NLB simula uma intranet, a Internet e uma rede doméstica e demonstra a funcionalidade de Acesso Remoto em diferentes cenários de conexão à Internet.  
  
> [!IMPORTANT]  
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.  
  
## <a name="KnownIssues"></a>Problemas conhecidos  
Os problemas a seguir são conhecidos quando se configura um cenário de cluster:  
  
-   Depois de configurar o DirectAccess em uma implantação somente IPv4 com um único adaptador de rede e depois que o DNS64 padrão (o endereço IPv6 que contém ":3333::") for configurado automaticamente no adaptador de rede, a tentativa de habilitar o balanceamento de carga por meio do console de Gerenciamento de Acesso Remoto faz com que um prompt para o usuário forneça um DIP IPv6. Se for fornecido um DIP IPv6, ocorrerá falha na configuração depois de clicar em **Confirmar** com o erro: O parâmetro está incorreto.  
  
    Para resolver esse problema:  
  
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
  


