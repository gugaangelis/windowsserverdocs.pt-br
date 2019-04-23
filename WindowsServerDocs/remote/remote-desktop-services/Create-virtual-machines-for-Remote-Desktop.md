---
title: Criar máquinas virtuais para a Área de Trabalho Remota
description: Crie VMs para hospedar os componentes da área de trabalho remota na nuvem.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: c5147636f73e8628aba549c4e05b9f23ab4ef9a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873927"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Criar máquinas virtuais para a Área de Trabalho Remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Use as etapas a seguir para criar as máquinas virtuais no ambiente do locatário que será usado para executar o Windows Server 2016 funções, serviços e recursos necessários para uma implantação de hospedagem de área de trabalho.   
  
Para este exemplo de uma implantação básica, o mínimo de 3 máquinas virtuais será criado. Uma máquina virtual hospedará os serviços de função agente de Conexão de área de trabalho remota (RD) e o servidor de licença e um compartilhamento de arquivos para a implantação. Uma segunda máquina virtual hospedará os serviços de função Gateway de área de trabalho remota e acesso via Web.  Uma terceira máquina virtual hospedar o serviço de função Host de sessão de área de trabalho remota. Para implantações pequenas, você pode reduzir os custos VM usando o Proxy de aplicativo do AAD para eliminar todos os pontos de extremidade públicos da implantação e combinar todos os serviços de função em uma única VM. Para implantações maiores, você pode instalar vários serviços de função em máquinas virtuais individuais para permitir melhor dimensionamento.  
  
Esta seção descreve as etapas necessárias para implantar máquinas virtuais para cada função com base em imagens do Windows Server no [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Se você precisa criar as máquinas virtuais de uma imagem personalizada, que requer o PowerShell, confira [criar uma VM do Windows com o Resource Manager e PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Em seguida, volte aqui para anexar discos de dados do Azure para o compartilhamento de arquivos e insira uma URL externa para sua implantação.  
  
1.  [Criar máquinas virtuais de Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) para hospedar o servidor do agente de Conexão de área de trabalho, o servidor de licenças de área de trabalho remota e o arquivo.  
  
    Para nosso objetivo, usamos as seguintes convenções de nomenclatura:  
    - Agente de Conexão de área de trabalho, o servidor de licença e o servidor de arquivos:   
        - VM: A Contoso Cb1  
        - Conjunto de disponibilidade: CbAvSet    
    - Acesso via Web RD e o servidor de Gateway de área de trabalho remota:   
        - VM: Contoso-WebGw1  
        - Conjunto de disponibilidade: WebGwAvSet  
          
    - Host de sessão de área de trabalho remota:   
        - VM: A Contoso Sh1  
        - Conjunto de disponibilidade: ShAvSet  
          
    Cada VM usa o mesmo grupo de recursos.  
2.  Criar e anexar um disco de dados do Azure para o compartilhamento de disco (UDP) do perfil de usuário:  
    1.  No portal do Azure clique **procurar > grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique em VM criada para o agente de Conexão de área de trabalho remota (por exemplo, Contoso-Cb1).  
    2.  Clique em **Configurações > discos > Anexar novo**.  
    3.  Aceite os padrões para o nome e tipo.  
    4.  Insira um tamanho (em GB) que seja grande o suficiente para manter os compartilhamentos de rede para o ambiente do locatário, incluindo certificados e discos de perfil do usuário. Você pode aproximar 5 GB por usuário que você planeja ter  
    5.  Aceite os padrões para o local e o cache de host e, em seguida, clique em **Okey**.  
3.  Crie um balanceador de carga externo para acessar a implantação externamente:
    1. No portal do Azure clique **procurar > balanceadores de carga**e, em seguida, clique em **Add**.
    2. Insira um **nome**, selecione **pública** como o **tipo** do balanceador de carga e selecione apropriado **assinatura**,  **Grupo de recursos**, e **local**.
    3. Selecione **escolher um endereço IP público**, **criar novo**, insira um nome e selecione **Okey**.
    4. Selecione **criar** para criar o balanceador de carga.
4.  Configure o balanceador externo de carga para sua implantação
    1. No portal do Azure clique **procurar > grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique o balanceador de carga que você criou para a implantação.
    2. Adicione um pool de back-end para o balanceador de carga enviar tráfego para:
        1. Selecione **pool de back-end** e **Add**.
        2. Insira um **nome** e selecione  **\+ adicionar uma máquina virtual**.
        3. Selecione **conjunto de disponibilidade** e **WebGwAvSet**.
        4. Selecione **máquinas virtuais**, **Contoso WebGw1**, **selecione**, **Okey**, e **Okey**.
    3. Adicione uma investigação para que o balanceador de carga sabe quais computadores estão ativos:
        1. Selecione **investigações** e **adicionar**.
        2. Insira um **nome** (como HTTPS), selecione **TCP**, insira **porta** 443 e selecione **Okey**.
    4. Insira as regras para equilibrar o tráfego de entrada de balanceamento de carga:
        1. Selecione **regras de balanceamento de carga** e **adicionar**
        2. Insira um **nome** (como HTTPS), selecione **TCP**e 443 para ambos os **porta** e o **porta de back-end**.
            - Para um Windows 10 e a implantação do Windows Server 2016, deixe **persistência de sessão** como **None**, caso contrário, selecione **IP do cliente**.
        3. Selecione **Okey** para aceitar a regra HTTPS.
        4. Criar uma nova regra selecionando **adicionar**.
        5. Insira um **nome** (como UDP), selecione **UDP**e 3391 para ambos o * * porta e o **porta de back-end**.
            - Para uma implantação do Windows 10 e Windows Server 2016, deixe **persistência de sessão** como **None**, caso contrário, selecione **IP do cliente**.
        6. Selecione **Okey** para aceitar a regra UDP.
    5. Insira uma regra NAT de entrada para se conectar diretamente à Contoso WebGw1
        1. Selecione **regras NAT de entrada** e **Add**.
        2. Insira um **nome** (como RDP-Contoso-WebGw1), selecione **Customm** para o serviço **TCP** para o protocolo e insira 14000 para o **porta**.
        3. Selecione **escolha uma máquina virtual** e WebGw1 Contoso.
        4. Selecione **personalizado** para o mapeamento de porta, digite 3389 para o **porta de destino**e selecione **Okey**.
5.  Insira um nome DNS/URL externo para sua implantação para acessá-lo externamente:  
    1.  No portal do Azure, clique em **procurar > grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique no endereço IP público que você criou para acesso via Web RD e o Gateway de área de trabalho remota.  
    2.  Clique em **Configuration**, insira um rótulo de nome DNS (como a contoso) e, em seguida, clique em **salvar**. Este rótulo de nome DNS (contoso.westus.cloudapp.azure.com) é o nome DNS que você usará para se conectar ao seu servidor de acesso via Web RD e o Gateway de área de trabalho remota.  

