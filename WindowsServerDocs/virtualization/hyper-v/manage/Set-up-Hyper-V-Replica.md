---
title: Configurar a Réplica do Hyper-V
description: Fornece instruções para configurar a réplica, testar o failover e fazer uma primeira replicação.
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: kbdazure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: 24fce3e0ebbfc51167a7e6e390de092433cceaff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941844"
---
# <a name="set-up-hyper-v-replica"></a>Configurar a Réplica do Hyper-V

>Aplica-se a: Windows Server 2016

A Réplica do Hyper-V é uma parte integrante da função do Hyper-V. Ele contribui para sua estratégia de recuperação de desastres replicando máquinas virtuais de um servidor de host Hyper-V para outro para manter suas cargas de trabalho disponíveis.  A réplica do Hyper-V cria uma cópia de uma máquina virtual ativa para uma máquina virtual offline de réplica. Observe o seguinte:

-   **Hosts do Hyper-V**: os servidores de host primário e secundário podem estar fisicamente posicionados ou em locais geográficos separados com replicação em um link de WAN. Os hosts do Hyper-V podem ser autônomos, clusterizados ou uma mistura de ambos. Não há nenhuma dependência Active Directory entre os servidores e eles não precisam ser membros do domínio.

-   **Replicação e controle de alterações**: quando você habilita a réplica do Hyper-V para uma máquina virtual específica, a replicação inicial cria uma máquina virtual de réplica idêntica em um servidor host secundário. Depois disso, o controle de alterações da réplica do Hyper-V cria e mantém um arquivo de log que captura as alterações em um VHD de máquina virtual. O arquivo de log é reproduzido na ordem inversa para o VHD de réplica com base nas configurações de frequência de replicação. Isso significa que as alterações mais recentes são armazenadas e replicadas de forma assíncrona. A replicação pode ser por HTTP ou HTTPS.

-   **Replicação estendida (encadeada)**: isso permite replicar uma máquina virtual de um host primário para um host secundário e, em seguida, replicar o host secundário para um terceiro host. Observe que você não pode replicar do host primário diretamente para o segundo e o terceiro.

    Esse recurso torna a réplica do Hyper-V mais robusta para recuperação de desastres porque, se ocorrer uma interrupção, você poderá recuperar da réplica primária e estendida.  Você pode fazer failover para a réplica estendida se os locais primários e secundários ficarem inativos. Observe que a réplica estendida não dá suporte à replicação consistente com o aplicativo e deve usar os mesmos VHDs que a réplica secundária está usando.

-   **Failover**: se ocorrer uma interrupção no local primário (ou secundário no caso de extensão estendida), você poderá iniciar manualmente um failover de teste, planejado ou não planejado.

    | Pergunta | Teste | Planejado | Não planejado |
    |--|--|--|--|
    | Quando devo executar? | Verificar se uma máquina virtual pode fazer failover e iniciar no site secundário<p>Útil para teste e treinamento | Durante o tempo de inatividade e as interrupções planejadas | Durante eventos inesperados |
    | Uma máquina virtual duplicada foi criada? | Sim | Não | Não |
    | Onde ele é iniciado? | Na máquina virtual de réplica | Iniciada na primária e concluída na secundária | Na máquina virtual de réplica |
    | Com que frequência devo executar? | É recomendável uma vez por mês para teste | Uma vez a cada seis meses ou de acordo com os requisitos de conformidade | Somente em caso de desastre quando a máquina virtual primária estiver indisponível |
    | A máquina virtual primária continua replicando? | Sim | Sim. Quando a interrupção é resolvida, a replicação inversa Replica as alterações de volta para o site primário para que o primário e o secundário sejam sincronizados. | Não |
    | Há alguma perda de dados? | Nenhum | Nenhum. Após o failover, a réplica do Hyper-V Replica o último conjunto de alterações controladas de volta para o primário para garantir zero perda de dados. | Depende do evento e dos pontos de recuperação |
    | Há algum tempo de inatividade? | Nenhum. Ele não afeta seu ambiente de produção. Ele cria uma máquina virtual de teste duplicada durante o failover. Após a conclusão do failover, selecione **failover** na máquina virtual de réplica e ele será limpo e excluído automaticamente. | A duração da interrupção planejada | A duração da interrupção não planejada |

-   **Pontos de recuperação**: ao definir as configurações de replicação para uma máquina virtual, você especifica os pontos de recuperação que deseja armazenar a partir dele. Os pontos de recuperação representam um instantâneo no tempo no qual você pode recuperar uma máquina virtual. Obviamente, menos dados serão perdidos se você se recuperar de um ponto de recuperação muito recente. Você pode acessar pontos de recuperação até 24 horas atrás.

## <a name="deployment-prerequisites"></a>Pré-requisitos de implantação
Veja o que você deve verificar antes de começar:

-   Descubra **quais VHDs precisam ser replicados**. Em particular, os VHDs que contêm dados que são alterados rapidamente e não são usados pelo servidor de réplica após o failover, como discos de arquivo de paginação, devem ser excluídos da replicação para conservar a largura de banda da rede. Anote quais VHDs podem ser excluídos.

-   **Decida com que frequência você precisa sincronizar dados**: os dados no servidor de réplica são sincronizados atualizados de acordo com a frequência de replicação configurada (30 segundos, 5 minutos ou 15 minutos). A frequência que você escolher deve considerar o seguinte: as máquinas virtuais que executam dados críticos com um RPO baixo? Quais são as considerações sobre largura de banda?  Suas máquinas virtuais altamente críticas certamente precisarão de replicação mais frequente.

-   **Decida como recuperar dados**: por padrão, a réplica do Hyper-V só armazena um único ponto de recuperação que será a replicação mais recente enviada do primário para o secundário. No entanto, se você quiser a opção de recuperar dados para um ponto anterior no tempo, poderá especificar que pontos de recuperação adicionais devem ser armazenados (até um máximo de 24 pontos por hora). Se você precisar de pontos de recuperação adicionais, observe que isso requer mais sobrecarga no processamento e nos recursos de armazenamento.

-   Descubra **quais cargas de trabalho serão replicadas**: a replicação de réplica padrão do Hyper-V mantém a consistência no estado do sistema operacional da máquina virtual após um failover, mas não o estado dos aplicativos em execução na máquina virtual. Se você quiser ser capaz de recuperar seu estado de carga de trabalho, poderá criar pontos de recuperação consistentes com o aplicativo. Observe que a recuperação consistente com o aplicativo não está disponível no site de réplica estendida se você estiver usando replicação estendida (encadeada).

-   **Decida como fazer a replicação inicial dos dados da máquina virtual**: a replicação começa transferindo as necessidades para transferir o estado atual das máquinas virtuais. Esse estado inicial pode ser transmitido diretamente através da rede existente, imediatamente ou em um momento posterior configurado por você. Você também pode usar uma máquina virtual restaurada pré-existente (por exemplo, se você restaurou um backup anterior da máquina virtual no servidor de réplica) como a cópia inicial. Ou então, você pode poupar largura de banda da rede copiando a cópia inicial para mídia externa e depois entregando fisicamente a mídia ao site de Réplica.  Se você quiser usar uma máquina virtual preexistente, exclua todos os instantâneos anteriores associados a ela.

## <a name="deployment-steps"></a>Etapas de implantação.

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Etapa 1: configurar os hosts do Hyper-V
Você precisará de pelo menos dois hosts Hyper-V com uma ou mais máquinas virtuais em cada servidor. [Introdução ao Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). O servidor host no qual você replicará máquinas virtuais precisará ser configurado como o servidor de réplica.

1.  Nas configurações do Hyper-V para o servidor em que você replicará máquinas virtuais, em **configuração de replicação**, selecione **habilitar este computador como um servidor de réplica**.

2.  Você pode replicar por HTTP ou HTTPS criptografado. Selecione **usar Kerberos (http)** ou **usar autenticação baseada em certificado (https**). Por padrão, HTTP 80 e HTTPS 443 são habilitados como exceções de firewall no servidor de réplica do Hyper-V. Se você alterar as configurações de porta padrão, precisará também alterar a exceção de firewall. Se estiver replicando por HTTPS, você precisará selecionar um certificado e deverá ter a autenticação do certificado configurada.

3.  Para autorização, selecione **permitir replicação de qualquer servidor autenticado** para permitir que o servidor de réplica aceite o tráfego de replicação da máquina virtual de qualquer servidor primário autenticado com êxito. Selecione **permitir replicação dos servidores especificados** para aceitar o tráfego somente dos servidores primários que você selecionar especificamente.

    Para ambas as opções, você pode especificar onde os VHDs replicados devem ser armazenados no servidor Hyper-V de réplica.

4.  Clique em **OK** ou em **Aplicar**.

### <a name="step-2-set-up-the-firewall"></a>Etapa 2: configurar o firewall
Para permitir a replicação entre os servidores primário e secundário, o tráfego deve ser obtido por meio do firewall do Windows (ou de qualquer outro firewall de terceiros). Quando você instalou a função Hyper-V nos servidores, por padrão, as exceções para HTTP (80) e HTTPS (443) são criadas. Se você estiver usando essas portas padrão, precisará apenas habilitar as regras:

-  Para habilitar as regras em um servidor host autônomo:

    1. Abra o Firewall do Windows com Segurança Avançada e clique em **Regras de Entrada**.

    2. Para habilitar a autenticação http (Kerberos), clique com o botão direito do mouse em **ouvinte HTTP de réplica do Hyper-V (TCP-in)**  > **habilitar regra.** Para habilitar a autenticação baseada em certificado HTTPS, clique com o botão direito do mouse em **ouvinte HTTPS de réplica do Hyper-V (TCP-in)** > E**habilitar a regra**.

-  Para habilitar regras em um cluster Hyper-V, abra uma sessão do Windows PowerShell usando **Executar como administrador**e execute um destes comandos:

    -   Para HTTP:`get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`

    -   Para HTTPS:`get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`

### <a name="enable-virtual-machine-replication"></a>Habilitar replicação de máquina virtual
Faça o seguinte em cada máquina virtual que você deseja replicar:

1.  No painel de **detalhes** do Gerenciador do Hyper-V, selecione uma máquina virtual clicando nela.
    Clique com o botão direito do mouse na máquina virtual selecionada e clique em **habilitar replicação** para abrir o assistente para habilitar replicação.

2.  Na página **Antes de Começar**, clique em **Avançar**.

3.  Na página **especificar servidor de réplica** , na caixa servidor de réplica, insira o NetBIOS ou o FQDN do servidor de réplica. Se o servidor de réplica fizer parte de um cluster de failover, insira o nome do agente de réplica do Hyper-V. Clique em **Próximo**.

4.  Na página **especificar parâmetros de conexão** , a réplica do Hyper-V recupera automaticamente as configurações de porta e autenticação que você configurou para o servidor de réplica. Se os valores não estiverem sendo recuperados, verifique se o servidor está configurado como um servidor de réplica e se ele está registrado no DNS. Se necessário, digite manualmente a configuração.

5.  Na página **escolher VHDs de replicação** , verifique se os VHDs que você deseja replicar estão selecionados e desmarque as caixas de seleção de todos os VHDs que você deseja excluir da replicação. Em seguida, clique em **Próximo**.

6.  Na página **Configurar frequência de replicação** , especifique a frequência com que as alterações devem ser sincronizadas do primário para o secundário. Em seguida, clique em **Próximo**.

7.  Na página **Configurar pontos de recuperação adicionais** , selecione se deseja manter apenas o ponto de recuperação mais recente ou criar pontos adicionais.    Se você quiser recuperar consistentemente aplicativos e cargas de trabalho que têm seus próprios gravadores VSS, recomendamos que você selecione a **frequência de serviço de cópias de sombra de volume (VSS)** y e especifique a frequência de criação de instantâneos consistentes com o aplicativo. Observe que o serviço solicitante do VMM do Hyper-V deve estar em execução nos servidores Hyper-V primário e secundário. Em seguida, clique em **Próximo**.

8.  Na página **escolher replicação inicial** , selecione o método de replicação inicial a ser usado.  A configuração padrão para enviar a cópia inicial pela rede irá copiar o arquivo de configuração de máquina virtual primário (VMCX) e os arquivos de disco rígido virtual (VHDX e VHD) selecionados em sua conexão de rede. Verifique a disponibilidade da largura de banda da rede se você pretende usar essa opção. Se a máquina virtual primária já estiver configurada no site secundário como uma máquina virtual replicar, pode ser útil selecionar **usar uma máquina virtual existente no servidor de replicação como a cópia inicial**. Você pode usar a exportação do Hyper-V para exportar a máquina virtual primária e importá-la como uma máquina virtual de réplica no servidor secundário. Para máquinas virtuais maiores ou largura de banda limitada, você pode escolher que a replicação inicial na rede ocorra em um momento posterior e, em seguida, configurar horários de pico ou enviar as informações de replicação inicial como mídia offline.

    Se você fizer a replicação offline, você transportará a cópia inicial para o servidor secundário usando um meio de armazenamento externo, como um disco rígido ou uma unidade USB. Para fazer isso, você precisará conectar o armazenamento externo ao servidor primário (ou nó de proprietário em um cluster) e, em seguida, ao selecionar enviar cópia inicial usando a mídia externa, você pode especificar um local localmente ou em sua mídia externa na qual a cópia inicial pode ser armazenada.  Uma máquina virtual de espaço reservado é criada no site de réplica. Depois que a replicação inicial for concluída, o armazenamento externo poderá ser enviado para o site de réplica. Lá, você conectará a mídia externa ao servidor secundário ou ao nó proprietário do cluster secundário. Em seguida, você importará a réplica inicial para um local especificado e a mesclará na máquina virtual do espaço reservado.

9. Na página **concluindo a replicação** , examine as informações no resumo e clique em **concluir.**.. Os dados da máquina virtual serão transferidos de acordo com as configurações escolhidas. e uma caixa de diálogo é exibida indicando que a replicação foi habilitada com êxito.

10. Se você quiser configurar a replicação estendida (encadeada), abra o servidor de réplica e clique com o botão direito do mouse na máquina virtual que você deseja replicar. Clique em **replicação**  >  **estender replicação** e especifique as configurações de replicação.

## <a name="run-a-failover"></a>Executar um failover
Depois de concluir essas etapas de implantação, seu ambiente replicado estará ativo e em execução. Agora você pode executar failovers conforme necessário.

**Failover de teste**: se você quiser executar um failover de teste, clique com o botão direito do mouse na máquina virtual primária e selecione failover de teste de **replicação**  >  **Test Failover**. Escolha o mais recente ou outro ponto de recuperação, se estiver configurado. Uma nova máquina virtual de teste será criada e iniciada no site secundário. Depois de concluir o teste, selecione **parar failover de teste** na máquina virtual de réplica para limpá-lo. Observe que, para uma máquina virtual, você só pode executar um failover de teste por vez. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).

**Failover planejado**: para executar um failover planejado, clique com o botão direito do mouse na **Replication**máquina virtual primária e selecione  >  **failover planejado**de replicação. O failover planejado executa verificações de pré-requisitos para garantir zero perda de dados. Ele verifica se a máquina virtual primária está desligada antes de iniciar o failover. Após o failover da máquina virtual, ele começa a replicar as alterações de volta para o site primário quando ele está disponível. Observe que para isso funcionar, o servidor primário deve ser configurado para recebido a replicação do servidor secundário ou do agente de réplica do Hyper-V no caso de um cluster primário. O failover planejado envia o último conjunto de alterações controladas. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).

**Failover não planejado**: para executar um failover não planejado, clique com o botão direito do mouse na máquina virtual **Replication**de réplica e selecione  >  **failover não planejado** de replicação no Gerenciador do Hyper-V ou Gerenciador de cluster de failover. Você pode recuperar do ponto de recuperação mais recente ou de pontos de recuperação anteriores se essa opção estiver habilitada. Após o failover, verifique se tudo está funcionando conforme o esperado na máquina virtual com failover e clique em **concluir** na máquina virtual de réplica. [Leia mais](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).
