---
title: Criar máquinas virtuais para a Área de Trabalho Remota
description: Crie VMs para hospedar componentes da Área de Trabalho Remota na nuvem.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: fa17c472e3311e4e34ac7b2176d0045886463274
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818459"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Criar máquinas virtuais para a Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Use as etapas a seguir para criar as máquinas virtuais no ambiente do locatário que será usado para executar as funções, os serviços e os recursos do Windows Server 2016 necessários para uma implantação de hospedagem de área de trabalho.   
  
Para este exemplo de uma implantação básica, o número mínimo de três máquinas virtuais será criado. Uma máquina virtual hospedará os serviços de função do Agente de Conexão de Área de Trabalho Remota (RD) e do Servidor de Licença, bem como um compartilhamento de arquivo para a implantação. Uma segunda máquina virtual hospedará os serviços de função do Gateway de Área de Trabalho Remota e Acesso via Web.  Uma terceira máquina virtual hospedará o serviço de função de Host da Sessão RD. Para implantações muito pequenas, você pode reduzir os custos de VM usando o Proxy de Aplicativo do AAD para eliminar todos os pontos de extremidade públicos da implantação e combinando todos os serviços de função em uma única VM. Para implantações maiores, você pode instalar os diferentes serviços de função em máquinas virtuais individuais para permitir um melhor dimensionamento.  
  
Esta seção descreve as etapas necessárias para implantar máquinas virtuais para cada função com base em imagens do Windows Server no [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Se precisar criar as máquinas virtuais usando uma imagem personalizada, o que requer o PowerShell, confira [Criar uma VM do Windows com o Resource Manager e o PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Em seguida, volte aqui para anexar discos de dados do Azure para o compartilhamento de arquivo e insira uma URL externa para sua implantação.  
  
1. [Crie Máquinas Virtuais do Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) para hospedar o Agente de Conexão de Área de Trabalho Remota, o Servidor de Licenças RD e o Servidor de Arquivos.  
  
   Para nosso objetivo, usamos as seguintes convenções de nomenclatura:  
   - Agente de Conexão de Área de Trabalho Remota, Servidor de Licenças de Área de Trabalho Remota e Servidor de Arquivos:   
       - VM: Contoso-Cb1  
       - Conjunto de disponibilidade: CbAvSet    
   - Acesso via Web da Área de Trabalho Remota e Servidor de Gateway de Área de Trabalho Remota:   
       - VM: Contoso-WebGw1  
       - Conjunto de disponibilidade: WebGwAvSet  
          
   - Host da Sessão RD:   
       - VM: Contoso-Sh1  
       - Conjunto de disponibilidade: ShAvSet  
          
   Cada VM usa o mesmo grupo de recursos.  
2. Crie e anexe um disco de dados do Azure para o compartilhamento do UPD (disco de perfil do usuário):  
   1.  No portal do Azure, clique em **Procurar > Grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique na VM criada para o Agente de Conexão de Área de Trabalho Remota (por exemplo, Contoso-Cb1).  
   2.  Clique em **Configurações > Discos > Anexar novo**.  
   3.  Aceite os padrões para nome e tipo.  
   4.  Insira um tamanho (em GB) que seja grande o suficiente para conter os compartilhamentos de rede para o ambiente do locatário, incluindo certificados e discos de perfil do usuário. Você pode usar o valor aproximado de 5 GB por usuário que planejar ter  
   5.  Aceite os padrões para localização e cache de host e, em seguida, clique em **OK**.  
3. Crie um balanceador de carga externo para acessar a implantação externamente:
   1. No portal do Azure, clique em **Procurar > Balanceadores de carga** e, em seguida, clique em **Adicionar**.
   2. Insira um **Nome**, selecione **Público** como **Tipo** do balanceador de carga e selecione os valores adequados para **Assinatura**, **Grupo de Recursos** e **Localização**.
   3. Selecione **Escolher um endereço IP público**, **Criar novo**, insira um nome e selecione **OK**.
   4. Selecione **Criar** para criar o balanceador de carga.
4. Configure o balanceador de carga externo para sua implantação
   1. No portal do Azure, clique em **Procurar > Grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique no balanceador de carga criado para a implantação.
   2. Adicione um pool de back-end para o qual o balanceador de carga enviará tráfego:
       1. Selecione **Pool de back-end** e **Adicionar**.
       2. Insira um **Nome** e selecione **\+ Adicionar uma máquina virtual**.
       3. Selecione **Conjunto de disponibilidade** e **WebGwAvSet**.
       4. Selecione **Máquinas virtuais**, **Contoso-WebGw1**, **Selecionar**, **OK** e **OK**.
   3. Adicione uma investigação para que o balanceador de carga saiba quais computadores estão ativos:
       1. Selecione **Investigações** e **Adicionar**.
       2. Insira um **Nome** (como HTTPS), selecione **TCP**, insira a **Porta** 443 e selecione **OK**.
   4. Insira as regras de balanceamento de carga para balancear o tráfego de entrada:
      1. Selecione **Regras de balanceamento de carga** e **Adicionar**
      2. Insira um **Nome** (como HTTPS), selecione **TCP** e 443 para a **Porta** e a **Porta de back-end**.
          - Para uma implantação com Windows 10 e Windows Server 2016, deixe **Persistência de sessão** como **Nenhum**, caso contrário, selecione **IP do cliente**.
      3. Selecione **OK** para aceitar a regra HTTPS.
      4. Crie uma nova regra selecionando **Adicionar**.
      5. Insira um **Nome** (como UDP), selecione **UDP** e 3391 para a <strong>Porta e a **Porta de back-end</strong>.
          - Para uma implantação com Windows 10 e Windows Server 2016, deixe **Persistência de sessão** como **Nenhum**, caso contrário, selecione **IP do cliente**.
      6. Selecione **OK** para aceitar a regra de UDP.
   5. Insira uma regra NAT de entrada para se conectar diretamente a Contoso-WebGw1
       1. Selecione **Regras NAT de Entrada** e **Adicionar**.
       2. Insira um **Nome** (como RDP-Contoso-WebGw1), selecione **Personalizado** para o serviço, **TCP** para o protocolo e insira 14000 para a **Porta**.
       3. Selecione **Escolha uma máquina virtual** e Contoso-WebGw1.
       4. Selecione **Personalizado** para o mapeamento de porta, digite 3389 para a **Porta de destino** e selecione **OK**.
5. Insira um nome DNS/URL externo para sua implantação para acessá-la externamente:  
   1.  No portal do Azure, clique em **Procurar > Grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique no endereço IP público criado para o Acesso via Web da Área de Trabalho Remota e o Gateway de Área de Trabalho Remota.  
   2.  Clique em **Configuração**, insira um rótulo de nome DNS (como contoso) e, em seguida, clique em **Salvar**. Este rótulo de nome DNS (contoso.westus.cloudapp.azure.com) é o nome DNS que você usará para se conectar ao seu servidor de Acesso via Web da Área de Trabalho Remota e de Gateway de Área de Trabalho Remota.  

