---
title: Configurar a Réplica do Hyper-V
ms.technology: compute-hyper-v
description: Fornece instruções para configurar a réplica, teste de failover e fazer uma replicação primeiro.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: KBDAzure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: 04066c5b645c0be641c2ba76fadac032d5a91420
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843207"
---
# <a name="set-up-hyper-v-replica"></a>Configurar a Réplica do Hyper-V

>Aplica-se a: Windows Server 2016

A Réplica do Hyper-V é uma parte integrante da função do Hyper-V. Ele contribui para sua estratégia de recuperação de desastres ao replicar máquinas virtuais de um servidor de host do Hyper-V para outro para manter suas cargas de trabalho disponíveis.  Réplica do Hyper-V cria uma cópia de uma máquina virtual ao vivo para uma máquina virtual offline de réplica. Observe o seguinte:  

-   **Hosts Hyper-V**: Servidores de host primário e secundário podem estar fisicamente colocalizados ou em locais geograficamente separados com replicação em uma WAN link. Hosts Hyper-V podem ser autônomos, em cluster, ou uma mistura de ambos. Não há nenhuma dependência do Active Directory entre os servidores e não precisam ser membros do domínio.  

-   **Replicação e controle de alterações**: Quando você habilita a réplica do Hyper-V para uma máquina virtual específica, a replicação inicial cria uma máquina virtual de réplica idêntico em um servidor de host secundário. Depois que isso acontece, o controle de alterações de réplica do Hyper-V cria e mantém um arquivo de log que captura alterações em um VHD de máquina virtual. O arquivo de log é reproduzido na ordem inversa para a réplica de que VHD com base nas configurações de frequência de replicação. Isso significa que as alterações mais recentes são armazenadas e replicadas de forma assíncrona. A replicação pode ser via HTTP ou HTTPS.  

-   **Estendidos de replicação (encadeada)**: Isso lhe permite replicar uma máquina virtual de um host primário para um host secundário e replicar o secundário de host para hospedar um terceiro. Observe que você não pode replicar do host primário diretamente para o segundo e o terceiro.  

    Esse recurso torna mais robusta para recuperação de desastres réplica do Hyper-V, pois se ocorrer uma interrupção permite a recuperação da réplica primária e estendida.  Você pode fazer failover para a réplica estendida se os locais primários e secundários ficam inativos. Observe que a réplica estendida não dá suporte a replicação consistente com o aplicativo e deve usar os mesmos VHDs que a réplica secundária está usando.  

-   **Failover**: Se ocorrer uma interrupção em seu servidor primário (ou secundário no caso de estendidos) local, você pode iniciar manualmente um teste, planejado ou failover não planejado.  

    ||Testar|Planejado|Não planejado|  
    |-|--------|-----------|-------------|  
    |Quando devo executar?|Verifique se uma máquina virtual pode fazer failover e iniciar no site secundário<br /><br />Útil para teste e treinamento|Durante as interrupções e tempo de inatividade planejado|Durante a eventos inesperados|  
    |Uma máquina virtual duplicada é criada?|Sim|Não|Não|  
    |Onde é iniciada?|Na máquina virtual de réplica|Iniciada na primária e concluída na secundária|Na máquina virtual de réplica|  
    |A frequência com que deve executar?|É recomendável uma vez por mês para teste|Uma vez a cada seis meses ou de acordo com requisitos de conformidade|No caso de desastre, quando a máquina virtual primária estiver indisponível|  
    |A máquina virtual primária continuam a replicar?|Sim|Sim. Quando a interrupção for resolva replica a replicação inversa que as alterações de volta para o site primário para que a primária e secundária são sincronizados.|Não|  
    |Há perda de dados?|Nenhuma|Nenhum. Após o failover de réplica do Hyper-V Replica o último conjunto de alterações controladas no principal para garantir que a perda de dados.|Depende dos evento e pontos de recuperação|  
    |Há algum tempo de inatividade?|Nenhum. Ele não afeta seu ambiente de produção. Ele cria uma máquina virtual de teste duplicada durante o failover. Após a conclusão do failover você seleciona **Failover** na réplica de máquina virtual e ele tem automaticamente limpos e excluído.|Durante a interrupção planejada|A duração de interrupção não planejada|  

-   **Pontos de recuperação**: Quando você configura as configurações de replicação para uma máquina virtual, você pode especificar os pontos de recuperação que você deseja armazenar a partir dele. Pontos de recuperação representam um instantâneo no tempo do qual você pode recuperar uma máquina virtual. Obviamente, menos dados serão perdidos se recuperar de um ponto de recuperação mais recente. Você pode acessar os pontos de recuperação até 24 horas atrás.  

## <a name="deployment-prerequisites"></a>Pré-requisitos de implantação  
Aqui está o que você deve verificar antes de começar:  

-   **Descobrir quais VHDs precisam ser replicados**. Em particular, VHDs que contêm dados que mudam rapidamente e não usadas pelo servidor de réplica após o failover, tais como discos de arquivo de página, deve ser excluído da replicação para poupar largura de banda de rede. Anote os VHDs podem ser excluídos.  

-   **Decidir a frequência com que você precisa sincronizar os dados**:  Os dados no servidor de réplica são sincronizados atualizado de acordo com a frequência de replicação que você configurar (30 segundos, 5 minutos ou 15 minutos). A frequência em que você escolher deve considerar o seguinte: As máquinas virtuais estão executando dados críticos com um RPO baixo? O que você está considerações de largura de banda?  As máquinas virtuais altamente críticos, obviamente, precisará de replicação mais frequente.  

-   **Decidir como recuperar dados**:  Por padrão a réplica do Hyper-V armazena apenas um ponto de recuperação único que será a replicação mais recente enviada do primário para o secundário. No entanto se você desejar que a opção para recuperar dados para um ponto anterior no tempo você pode especificar que os pontos de recuperação adicionais devem ser armazenados (até um máximo de 24 pontos por hora). Se você precisa de pontos de recuperação adicionais, você deve observar que isso exige mais sobrecarga em recursos de processamento e armazenamento.  

-   **Descobrir quais cargas de trabalho que você replicará**: Replicação de réplica do Hyper-V padrão mantém a consistência no estado do sistema de operacional de máquina virtual após um failover, mas não o estado dos aplicativos que está em execução na máquina virtual. Se você quiser ser capaz de recuperação de pontos de seu estado de carga de trabalho, você pode criar recuperação consistente do aplicativo. Observe que a recuperação consistente com o aplicativo não está disponível no site de réplica estendida se você estiver usando a replicação estendida (encadeada).  

-   **Decidir como fazer a replicação inicial de dados da máquina virtual**: Replicação é iniciada, transferindo as necessidades para transferir o estado atual das máquinas virtuais. Esse estado inicial pode ser transmitido diretamente através da rede existente, imediatamente ou em um momento posterior configurado por você. Você também pode usar uma máquina de virtual pré-existente restaurada (por exemplo, se você tiver restaurado um backup anterior da máquina virtual no servidor de réplica) como cópia inicial. Ou então, você pode poupar largura de banda da rede copiando a cópia inicial para mídia externa e depois entregando fisicamente a mídia ao site de Réplica.  Se você quiser usar um preexistente máquina virtual excluir todos os instantâneos anteriores associados a ele.  

## <a name="deployment-steps"></a>Etapas de implantação  

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Etapa 1: Configurar os hosts Hyper-V  
Você precisará de pelo menos dois hosts Hyper-V com um ou mais máquinas virtuais em cada servidor. [Introdução ao Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). O servidor de host que você replicar máquinas virtuais para precisará ser definido como o servidor de réplica.  

1.  As configurações de Hyper-V para o servidor, você replicará as máquinas virtuais para, em **configuração de replicação**, selecione **habilitar este computador como um servidor de réplica**.  

2.  Você pode replicar via HTTP ou HTTPS criptografado. Selecione **usar Kerberos (HTTP)** ou **usar a autenticação baseada em certificado (HTTPS**). Por padrão 80 de HTTP e HTTPS 443 são habilitados como exceções de firewall no servidor de réplica de Hyper-V. Se você alterar as configurações de porta, você precisará alterar também a exceção de firewall. Se você estiver replicando via HTTPS, você precisará selecionar um certificado e você deve ter a configuração de autenticação de certificado.  

3.  Para autorização, selecione **permitem que a replicação de qualquer servidor autenticado** para permitir que o servidor de réplica aceitar o tráfego de replicação de máquina virtual de qualquer servidor primário que é autenticado com êxito. Selecione **permitem que a replicação dos servidores especificados** para aceitar o tráfego apenas de servidores primários selecionar especificamente.  

    Para ambas as opções, você pode especificar onde os VHDs replicados devem ser armazenados no servidor de réplica de Hyper-V.  

4.  Clique em **OK** ou **Aplicar**.  

### <a name="step-2-set-up-the-firewall"></a>Etapa 2: Configurar o firewall  
Para permitir a replicação entre os servidores primários e secundários, o tráfego deve obter por meio do firewall do Windows (ou quaisquer outros firewalls de terceiros). Quando você instalou a função Hyper-V nos servidores por exceções padrão para HTTP (80) e HTTPS (443) são criados. Se você estiver usando essas portas padrão, você precisará apenas habilitar as regras:  

-  Para habilitar as regras em um servidor de host autônomo:  

    1. Abra o Firewall do Windows com segurança avançada e clique em **regras de entrada**.  

    2. Para habilitar a autenticação HTTP (Kerberos), clique com botão direito **ouvinte HTTP da réplica do Hyper-V (TCP-In)** >**Habilitar regra.** Para habilitar a autenticação baseada em certificado HTTPS, clique com botão direito **Ouvinte HTTPS da réplica do Hyper-V (TCP-In)** > eletrônico**Habilitar regra**.  

-  Para habilitar as regras em um cluster do Hyper-V, abra uma sessão do Windows PowerShell usando **executar como administrador**, em seguida, execute um destes comandos:  

    -   Para HTTP:  `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`  

    -   Para HTTPS: `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`  

### <a name="enable-virtual-machine-replication"></a>Habilitar replicação de máquinas virtuais  
Faça o seguinte em cada máquina virtual que você deseja replicar:  

1.  No **detalhes** painel do Gerenciador do Hyper-V, selecione uma máquina virtual clicando nele.  
    Clique com botão direito na máquina virtual selecionada e clique em **habilitar replicação** para abrir o assistente habilitar replicação.  

2.  Na página **Antes de Começar** , clique em **Avançar**.  

3.  Sobre o **especificar servidor de réplica** página, na caixa de servidor de réplica, insira NetBIOS ou FQDN do servidor de réplica. Se o servidor de réplica for parte de um cluster de failover, insira o nome do agente de réplica do Hyper-V. Clique em **Avançar**.  

4.  Sobre o **especificar parâmetros de Conexão** página, a réplica do Hyper-V recupera automaticamente as configurações de autenticação e a porta que você configurou para o servidor de réplica. Se os valores não estão sendo recuperados a verificação de que o servidor está configurado como servidor de réplica, e que está registrado no DNS. Se for necessário tipo na configuração manualmente.  

5.  Sobre o **escolher VHDs de replicação** página, verifique se os VHDs que você deseja replicar estão selecionados e desmarque as caixas de seleção para quaisquer VHDs que você deseja excluir da replicação. Em seguida, clique em **Avançar**.  

6.  Sobre o **configurar a frequência de replicação** , especifique a frequência com que as alterações devem ser sincronizadas do primário para o secundário. Em seguida, clique em **Avançar**.  

7.  Sobre o **configurar pontos de recuperação adicionais** , selecione se você deseja manter somente o último ponto de recuperação ou para criar pontos adicionais.    Se você deseja recuperar consistentemente a aplicativos e cargas de trabalho que têm seus próprios gravadores VSS, recomendamos que você selecione **frequenc Volume Shadow Copy Service (VSS)** y e especifique com que frequência criar instantâneos consistentes por aplicativo. Observe que o serviço do Hyper-V VMM solicitante deve estar em execução em ambos os servidores de Hyper-V primários e secundários. Em seguida, clique em **Avançar**.  

8.  Sobre o **escolher replicação inicial** , selecione o método de replicação inicial a ser usado.  A configuração padrão para Enviar cópia inicial pela rede copiará o arquivo de configuração de máquina virtual primária (VMCX) e os arquivos de disco rígido virtual (VHD e VHDX) que você selecionou ao longo de sua conexão de rede. Verificar disponibilidade de largura de banda de rede, se você pretende usar essa opção. Se a máquina virtual primária já estiver configurada no site secundário como uma máquina virtual replicar pode ser útil selecionar **usar uma máquina virtual existente no servidor de replicação como cópia inicial**. Você pode usar Hyper-V exportação para exportar a máquina virtual primária e importá-lo como uma máquina virtual de réplica no servidor secundário. Para máquinas virtuais maiores ou largura de banda limitada você pode optar por ter a replicação inicial através da rede ocorrer em um momento posterior e, em seguida, configure o horário de pico, ou para enviar as informações de replicação inicial como mídia offline.  

    Se você fizer a replicação offline será de transporte a cópia inicial para o servidor secundário, usando uma mídia de armazenamento externo, como um disco rígido ou unidade USB. Para fazer isso, que você precisará conectar o externo armazenamento para o servidor primário (ou em um cluster de nó do proprietário) e, em seguida, quando você seleciona enviam cópia inicial usando a mídia externa, que você pode especificar um local localmente ou em sua mídia externa em que a cópia inicial pode ser armazenada.  Uma máquina virtual do espaço reservado é criada no site de réplica. Após a conclusão de replicação inicial de armazenamento externo pode ser enviado ao site de réplica. Lá, você conectará a mídia externa para o servidor secundário ou para o nó do proprietário do cluster secundário. Em seguida, você vai importar a réplica inicial para um local especificado e mesclá-lo para a máquina virtual de espaço reservado.  

9. Sobre o **Concluindo habilitar replicação** página, examine as informações no resumo e, em seguida, clique em **concluir.**. Os dados da máquina virtual serão transferidos de acordo com as configurações escolhidas. e uma caixa de diálogo é exibida indicando que a replicação foi habilitada com êxito.  

10. Se você quiser configurar a replicação estendida (encadeada), abra o servidor de réplica e a máquina virtual que você deseja replicar com o botão direito. Clique em **replicação** > **replicação estendida** e especifique as configurações de replicação.  

## <a name="run-a-failover"></a>Executar um failover  
Depois de concluir essas etapas de implantação a seu ambiente replicado está em execução. Agora você pode executar failovers conforme necessário.  

**Failover de teste**:  Se você quiser executar a máquina virtual primária de um failover de teste de clique com botão direito e selecione **replicação** > **Failover de teste**. Escolha o ponto de recuperação mais recente ou, se configurado. Uma nova máquina virtual de teste será criada e iniciada no site secundário. Depois de concluir o teste, selecione **parar Failover de teste** na máquina de virtual de réplica para limpar. Observe que para uma máquina virtual que você só pode executar um failover de teste por vez. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).  

**Failover planejado**: Para executar o botão direito do mouse um failover planejado da máquina virtual primária e selecione **replicação** > **Failover planejado**. Failover planejado executa verificações de pré-requisitos para garantir que a perda de dados. Ele verifica se a máquina virtual primária está desligada antes de iniciar o failover. Após o failover da máquina virtual, ele começa a replicar as alterações de volta para o site primário quando ele estiver disponível. Observe que, para este trabalho de servidor primário, deve ser configurado para replicação recebida do servidor secundário ou do agente de réplica do Hyper-V no caso de um cluster primário. Planejado envios de failover, o último conjunto de alterações controladas. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).  

**Failover não planejado**: Para executar um failover não planejado, o botão direito do mouse na máquina virtual de réplica e selecione **replicação** > **Failover não planejado** do Gerenciador do Hyper-V ou o Gerenciador de cluster de Failover. Se essa opção estiver habilitada, você pode recuperar do último ponto de recuperação ou de pontos de recuperação anteriores. Após o failover, verifique se tudo está funcionando conforme o esperado em com falha na máquina virtual, em seguida, clique em **concluir** na máquina virtual de réplica. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).  
