---
title: Manage the Local Server and the Server Manager Console
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f22578cc54a22464fe5d9208731fe681be30481
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832977"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, o Gerenciador do servidor permite que você gerencie o servidor local (se você está executando o Gerenciador do servidor no Windows Server e não em um sistema operacional cliente com base em Windows) e servidores remotos que executam o Windows Server 2008 e versões mais recentes do que o Windows Sistema operacional de servidor.

O **servidor Local** página no Gerenciador de servidores exibe os dados de contador de propriedades, eventos, serviços e desempenho do servidor e resultados do analisador de práticas recomendadas (BPA) para o servidor local. Os blocos de eventos, serviços, BPA e desempenho funcionam da mesma maneira que em páginas de grupo de servidores e função. Para obter mais informações sobre como configurar os dados que são exibidos nesses blocos, consulte [Exibir e configurar dados de desempenho, eventos e serviços](view-and-configure-performance-event-and-service-data.md) e [Executar verificações do Analisador de Práticas Recomendadas e gerenciar os resultados da verificação](run-best-practices-analyzer-scans-and-manage-scan-results.md).

Comandos de menu e as configurações nas barras de título do console Gerenciador do servidor se aplicam globalmente a todos os servidores no pool de servidores e permitem que você use o Gerenciador de servidores para gerenciar o pool de servidores inteiro.

Este tópico contém as seguintes seções.

-   [Desligar servidor local](#BKMK_shutdown)

-   [Configurar propriedades do Gerenciador do servidor](#BKMK_props)

-   [Gerenciar o console do Gerenciador do servidor](#BKMK_managesm)

-   [Personalizar ferramentas que são exibidas no menu Ferramentas](#BKMK_tools)

-   [Gerenciar funções em home pages de funções](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Desligar servidor local
O **tarefas** menu no servidor local **propriedades** bloco permite a você inicia uma sessão do Windows PowerShell no servidor local, abra o **gerenciamento de computador** snap-in mmc, ou abra snap-ins do MMC para funções ou recursos que estão instalados no servidor local. Você também pode desligar o servidor local usando o comando **Desligar Servidor Local** nesse menu **Tarefas** . O comando **Desligar Servidor Local** também está disponível para o servidor local no bloco **Servidores** na página **Todos os Servidores** ou na página de qualquer função ou grupo na qual o servidor local esteja representado.

Desligar o servidor local usando esse método, diferente de desligar o Windows Server 2016 do **inicie** tela, abre o **desligar o Windows** caixa de diálogo que permite que você especifique os motivos do desligamento no **controlador de eventos de desligamento** área.

> [!NOTE]
> Somente os membros do grupo Administradores podem desligar ou reiniciar um servidor. Os usuários padrão não podem desligar ou reiniciar um servidor. Ao clicar no comando **Desligar Servidor Local** , é feito logoff dos usuários padrão das sessões do servidor local. Isso corresponde à experiência de um usuário padrão que executa o comando de desligamento **Alt+F4** na área de trabalho do servidor.

## <a name="BKMK_props"></a>Configurar propriedades do Gerenciador do servidor
Você pode exibir ou alterar as seguintes configurações no bloco **Propriedades** na página **Servidor Local** . Para alterar o valor de uma configuração, clique no valor de hipertexto da configuração.

> [!NOTE]
> Normalmente, as propriedades exibidas no bloco **Propriedades** do Servidor Local podem ser alteradas apenas no servidor local. Você não pode alterar as propriedades do servidor de local de um computador remoto usando o Gerenciador do servidor porque o **propriedades** lado a lado pode obter informações sobre o computador local, computadores remotos não somente.
> 
> Como muitas propriedades exibidas na **propriedades** lado a lado são controladas por ferramentas que não fazem parte do Gerenciador do servidor (painel de controle, por exemplo), é alterado para **propriedades** configurações nem sempre são exibido na **propriedades** bloco imediatamente. Por padrão, os dados do bloco **Propriedades** são atualizadas a cada dois minutos. Para atualizar **propriedades** bloco dados imediatamente, clique em **atualizar** na barra de endereços do Gerenciador do servidor.

|Configuração|Descrição|
|------|--------|
|Nome do computador|Exibe o nome amigável do computador e abre o **propriedades do sistema** caixa de diálogo que permite que você altere o nome do servidor, associação de domínio e outras configurações do sistema, como perfis de usuário.|
|Domínio (ou Grupo de Trabalho, se o servidor não tiver ingressado em um domínio)|Exibe o domínio ou o grupo de trabalho do qual o servidor é membro. Abre o **propriedades do sistema** caixa de diálogo que permite que você altere o nome do servidor, associação de domínio e outras configurações do sistema, como perfis de usuário.|
|Firewall do Windows|Exibe o status do Firewall do Windows para o servidor local. Abre **Painel de Controle\Sistema e Segurança\Firewall do Windows**. Para obter mais informações sobre como configurar o firewall do Windows, consulte [Firewall do Windows com segurança avançada e IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|Gerenciamento remoto|Exibe o status de gerenciamento remoto do Gerenciador do servidor e do Windows PowerShell. Abre o **configurar o gerenciamento remoto** caixa de diálogo. Para obter mais informações sobre o gerenciamento remoto, consulte [configurar o gerenciamento remoto no Gerenciador do servidor](configure-remote-management-in-server-manager.md).|
|Área de Trabalho Remota|Mostra se os usuários podem se conectar ao servidor remotamente usando sessões da Área de Trabalho Remota. Abre o **remoto** guia o **propriedades do sistema** caixa de diálogo.|
|Agrupamento NIC|Mostra se o servidor local está participando do Agrupamento NIC. Abre a caixa de diálogo **Agrupamento NIC** e permite o ingresso no servidor local para um Agrupamento NIC, se desejado. Para obter mais informações sobre Agrupamento NIC, consulte o [White paper Agrupamento NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Exibe o status de rede do servidor. Abre **Painel de Controle\Rede e Internet\Conexões de Rede**.|
|Versão do sistema operacional|Este campo somente leitura exibe o número da versão do sistema operacional Windows em execução no servidor local.|
|Informações de hardware|Este campo somente leitura exibir o fabricante, o nome e o número do modelo do hardware do servidor.|
|Últimas atualizações instaladas|Exibe o dia e a hora em que atualizações do Windows foram instaladas pela última vez. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Windows Update|Exibe as configurações do Windows Update para o servidor local. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Última verificação de atualizações|Exibe o dia e a hora em que o servidor verificou atualizações disponíveis do Windows pela última vez. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Relatório de erros do Windows|Exibe o status de aceite do Relatório de Erros do Windows. Abre a caixa de diálogo **Configuração do Relatório de Erros do Windows** . Para obter mais informações sobre Relatórios de Erros do Windows, seus benefícios, políticas de privacidade e configurações de aceite, consulte [Relatórios de erros do Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programa de Aperfeiçoamento da Experiência do Usuário|Exibe o status de aceite do Programa de Aperfeiçoamento da Experiência do Usuário do Windows. Abre a caixa de diálogo **Configuração do Programa de Aperfeiçoamento da Experiência do Usuário** . Para obter mais informações sobre o Programa de Aperfeiçoamento da Experiência do Usuário, seus benefícios e suas configurações de aceite, consulte [Programa de Aperfeiçoamento da Experiência do Usuário do Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuração de Segurança Aprimorada do Internet Explorer (IE)|Mostra se a Configuração de Segurança Aprimorada do IE (também conhecida como fortalecimento do IE ou IE ESC) está ativada ou desativada. Abre a caixa de diálogo **Configuração de Segurança Aprimorada do Internet Explorer** . A Configuração de Segurança Aprimorada do IE é uma medida de segurança para servidores que impede que páginas da Web sejam abertas no Internet Explorer. Para obter mais informações sobre a configuração de segurança reforçada do IE, seus benefícios e configurações, consulte [do Internet Explorer: Configuração de segurança reforçada](https://go.microsoft.com/fwlink/?LinkId=253461).|
|Fuso horário|Exibe o fuso horário do servidor local. Abre o **data e hora** caixa de diálogo.|
|ID do Produto|Exibe o número de ID do Windows ativação status e o produto (se o Windows tem sido ativado) do sistema operacional Windows Server 2016. Esse número não é o mesmo que a chave do produto do Windows. Abre a caixa de diálogo **Ativação do Windows** .|
|Processadores|Este campo somente leitura exibe o fabricante, nome do modelo e informações sobre a velocidade sobre processadores do servidor local.|
|Memória instalada (RAM)|Este campo somente leitura exibe a quantidade de RAM disponível, em gigabytes.|
|Espaço total em disco|Este campo somente leitura exibe a quantidade de espaço em disco disponível, em gigabytes.|

## <a name="BKMK_managesm"></a>Gerenciar o console do Gerenciador do servidor
Configurações globais que se aplicam a todo o console do Gerenciador do servidor e a todos os servidores remotos que foram adicionados ao pool de servidores do Gerenciador do servidor, encontram-se nas barras de título na parte superior da janela do console do Gerenciador do servidor.

### <a name="add-servers-to-server-manager"></a>Adicionar servidores ao Gerenciador do servidor
O comando que abre o **adicionar servidores** caixa de diálogo e permite que você adicione servidores remotos físicos ou virtuais ao pool de servidores do Gerenciador do servidor, está no **gerenciar** menu do console do Gerenciador do servidor. Para obter informações detalhadas sobre como adicionar servidores, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Atualizar os dados exibidos no Gerenciador do Servidor
Você pode configurar o intervalo de atualização de dados que é exibido no Gerenciador do servidor na **propriedades do Gerenciador do servidor** caixa de diálogo, que é aberta na **gerenciar** menu.

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Para configurar o intervalo de atualização no Gerenciador do Servidor

1.  Sobre o **gerenciar** menu no console do Gerenciador do servidor, clique em **propriedades do Gerenciador do servidor**.

2.  No **propriedades do Gerenciador do servidor** caixa de diálogo, especifique um período de tempo, em minutos, para o tempo decorrido desejado entre as atualizações dos dados que são exibidos no Gerenciador do servidor. O padrão é de 10 minutos. Ao concluir, clique em OK.

#### <a name="refresh-limitations"></a>Limitações de atualização
A atualização se aplica globalmente aos dados de todos os servidores que você adicionou ao pool de servidores do Gerenciador do servidor. Não é possível atualizar dados ou configurar intervalos de atualização diferentes para servidores, funções ou grupos individuais.

Quando servidores que estão em um cluster são adicionados ao Gerenciador do servidor, quer eles sejam computadores físicos ou máquinas virtuais, a primeira atualização dos dados pode falhar ou exibir dados somente para o servidor de host de objetos clusterizados. As atualizações subsequentes mostram dados precisos de servidores físicos ou virtuais em um cluster de servidores.

Dados que são exibidos em home pages de funções no Gerenciador do servidor para serviços de área de trabalho remota, gerenciamento de endereço de IP e serviços de arquivo e armazenamento não são atualizados automaticamente. Atualize os dados que são exibidos nessas páginas manualmente, pressionando **F5** ou clicando em **atualizar** no cabeçalho de console de Gerenciador do servidor enquanto está nessas páginas.

### <a name="add-or-remove-roles-or-features"></a>Adicionar ou remover funções ou recursos
Os comandos que abrem o Assistente de recursos e adicionar funções e remover funções e Assistente de recursos e permitem que você adicionar ou remover funções, serviços de função e recursos aos servidores no pool de servidores, estão na **gerenciar** menu do Gerenciador de servidores console e o **tarefas** menu das **funções e recursos** lado a lado em páginas de função ou grupo. Para obter informações detalhadas sobre como adicionar ou remover funções ou recursos, consulte [instalar ou desinstalar funções, serviços de função ou recursos](install-or-uninstall-roles-role-services-or-features.md).

No Gerenciador do servidor, os dados de funções e recursos são exibidos no idioma base do sistema, também chamado de idioma de GUI padrão do sistema ou o idioma selecionado durante a instalação do sistema operacional.

### <a name="create-server-groups"></a>criar grupos de servidores
O comando que abre o **criar grupo de servidores** caixa de diálogo e permite que você crie grupos de servidores personalizados, que está no **gerenciar** menu do console do Gerenciador do servidor. Para obter informações detalhadas sobre como criar grupos de servidores, consulte [criar e gerenciar grupos de servidores](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedir que o Gerenciador do Servidor seja aberto automaticamente no logon
O **não iniciar o Gerenciador do servidor automaticamente no logon** caixa de seleção a **propriedades do Gerenciador do servidor** caixa de diálogo controla se Gerenciador do servidor é aberto automaticamente no logon de membros da Grupo de administradores em um servidor local. Essa configuração não afeta o comportamento do Gerenciador do servidor quando ele está em execução no Windows 10 como parte das ferramentas de administração de servidor remoto. Para obter mais informações sobre essa configuração, consulte [Gerenciador do servidor](server-manager.md).

### <a name="zoom-in-or-out"></a>Ampliar ou reduzir
Para ampliar ou reduzir a exibição do console do Gerenciador do servidor, você pode usar o **Zoom** comandos na **exibição** menus ou pressione **Ctrl + sinal de mais (+)** para ampliar e **Ctrl + sinal de subtração (-)** para diminuir o zoom.

## <a name="BKMK_tools"></a>Personalizar ferramentas que são exibidas no menu Ferramentas
O **ferramentas** menu no Gerenciador do servidor inclui soft links para atalhos na **ferramentas administrativas** pasta **painel de controle/sistema e segurança**. O **ferramentas administrativas** pasta contém uma lista de atalhos ou arquivos LNK para as ferramentas de gerenciamento disponíveis, como snap-ins do mmc. Popula o Gerenciador do servidor a **ferramentas** menu com links para esses atalhos e copia a estrutura da pasta a **ferramentas administrativas** pasta para o **ferramentas** menu. Por padrão, as ferramentas na pasta Ferramentas Administrativas são organizadas em uma lista simples, classificadas por tipo e por nome. No Gerenciador de servidores**ferramentas** menu, itens são classificados somente por nome e não por tipo.

Para personalizar o menu **Ferramentas** , copie os atalhos das ferramentas ou dos scripts que você deseja usar para a pasta **Ferramentas Administrativas** . Você também pode organizar seus atalhos em pastas, o que cria menus em cascata no menu **Ferramentas** . Além disso, se você quiser restringir o acesso às ferramentas personalizadas na **ferramentas** menu, você pode definir direitos de acesso do usuário em ambas as pastas de ferramentas personalizadas nas ferramentas administrativas ou diretamente nos arquivos de script ou ferramenta originais.

É recomendável não reorganizar as ferramentas administrativas e do sistema, nem as ferramentas de gerenciamento associadas a funções e recursos instalados no servidor local. A movimentação de ferramentas de gerenciamento de funções e recursos pode impedir a desinstalação com êxito dessas ferramentas de gerenciamento, quando necessário. Depois de desinstalar uma função ou um recurso, um link não funcional para uma ferramenta cujo atalho foi movido pode permanecer no menu **Ferramentas**. Se você reinstalar a função, será criado um link duplicado da mesma ferramenta no menu **Ferramentas**, mas um dos links não funcionará.

Contudo, as ferramentas de funções e recursos instaladas como parte das Ferramentas de Administração de Servidor Remoto em um computador baseado no cliente Windows podem ser organizadas em pastas personalizadas. Desinstalando a função ou recurso pai não tem efeito sobre os atalhos de ferramentas que estão disponíveis em um computador remoto que esteja executando o Windows 10.

O procedimento a seguir descreve como criar uma pasta de exemplo chamada *Minhasferramentas*, e mover atalhos de dois scripts do Windows PowerShell para a pasta que então ficarão acessíveis no menu Ferramentas do Gerenciador do servidor.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Para personalizar o menu Ferramentas adicionando atalhos em Ferramentas Administrativas

1.  criar uma nova pasta denominada *Minhasferramentas* em um único local conveniente.

    > [!NOTE]
    > Devido a direitos de acesso restritos na pasta **Ferramentas Administrativas** , você não tem permissão para criar uma nova pasta diretamente na pasta **Ferramentas Administrativas** ; crie uma nova pastas em outro lugar (como a Área de Trabalho) e copie a nova pasta para a pasta **Ferramentas Administrativas** .

2.  Mover ou copiar *Minhasferramentas* à **painel de controle/sistema e segurança/ferramentas administrativas**. Por padrão, você deve ser membro do grupo Administradores no computador para fazer alterações à pasta **Ferramentas Administrativas** .

3.  Se você não precisar restringir direitos de acesso do usuário aos atalhos de suas ferramentas personalizadas, vá para a etapa 6. Caso contrário, clique com o botão direito do mouse no arquivo da ferramenta (ou na pasta *MinhasFerramentas*) e clique em **Propriedades**.

4.  Sobre o **segurança** guia do arquivo de **propriedades** caixa de diálogo, clique em **editar**.

5.  para os usuários para quem você deseja restringir o acesso à ferramenta, desmarque as caixas de seleção **ler & executar**, **leitura**, e **gravar** permissões. Essas permissões são herdadas pelo atalho da ferramenta na pasta **Ferramentas Administrativas**.

    Se você editar os direitos de acesso para um usuário enquanto o usuário está usando o Gerenciador do servidor (ou enquanto o Gerenciador do servidor é abre), suas alterações não são mostradas na **ferramentas** menu até que o usuário reinicia o Gerenciador do servidor.

    > [!NOTE]
    > Se você restringir o acesso a uma pasta inteira que copiou em Ferramentas administrativas, os usuários restritos não poderão ver a pasta nem seus conteúdos no Gerenciador de servidores**ferramentas** menu.
    > 
    > Editar permissões para a pasta na **ferramentas administrativas** pasta. Como arquivos ocultos e pastas em Ferramentas administrativas são sempre exibidas no Gerenciador de servidores**ferramentas** menu, não use o **Hidden** definição em um arquivo ou uma pasta **propriedades** caixa de diálogo para restringir o acesso de usuários aos atalhos de suas ferramentas personalizadas.
    > 
    > As permissões **Negar** sempre substituem as permissões **Permitir**.

6.  Clique com botão direito a ferramenta original, script ou arquivo executável para o qual você deseja adicionar entradas na **ferramentas** menu e clique **criar atalho**.

7.  Mova o atalho para o *Minhasferramentas* pasta em Ferramentas administrativas.

8.  Atualize ou reinicie o Gerenciador do servidor, se necessário, para ver o atalho de sua ferramenta personalizada na **ferramentas** menu.

## <a name="BKMK_roles"></a>Gerenciar funções em home pages de funções
Depois de adicionar servidores ao pool de servidores do Gerenciador de servidores e Gerenciador do servidor coleta dados de inventário sobre os servidores em seu pool, o Gerenciador do servidor adiciona páginas ao painel de navegação para as funções que são descobertos em servidores gerenciados. O bloco **Servidores** nas páginas de funções lista os servidores gerenciados que executam a função. Por padrão, os blocos **Eventos**, **Analisador de Práticas Recomendadas**, **Serviços**e **Desempenho** exibem dados de todos os serviços que executam a função. A seleção de um servidor específico no bloco **Servidores** limita o escopo dos resultados de eventos, serviços, contadores de desempenho e BPA somente aos servidores selecionados. Ferramentas de gerenciamento geralmente estão disponíveis no console do Gerenciador do servidor **ferramentas** menu, depois que uma função ou recurso foi instalado ou descoberto em um servidor gerenciado. Você também pode clicar com o botão direito do mouse nas entradas do servidor no bloco **Servidores** de uma função ou um grupo e então iniciar a ferramenta de gerenciamento que deseja usar.

No Windows Server 2016, as seguintes funções e recursos têm ferramentas de gerenciamento integradas ao console do Gerenciador do servidor como páginas.

-   **Serviços de arquivo e armazenamento.** As páginas de serviços de arquivo e armazenamento incluem blocos e comandos personalizados para gerenciar volumes, compartilhamentos, discos virtuais iSCSI e pools de armazenamento. Quando você abrir o arquivo e abre de página inicial da função Serviços de armazenamento no Gerenciador do servidor, um painel recolhível que exibe páginas de gerenciamento personalizado para serviços de arquivo e armazenamento. Para obter mais informações sobre como implantar e gerenciar Serviços de arquivo e armazenamento, consulte [Serviços de arquivo e armazenamento](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Serviços de Área de Trabalho Remota.** As páginas de Serviços de Área de Trabalho Remota incluem blocos e comandos personalizados para gerenciar sessões, licenças, gateways e áreas de trabalho virtuais. Para obter mais informações sobre como implantar e gerenciar serviços de área de trabalho remota, consulte [serviços de área de trabalho remota (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Endereço IP de IPAM (gerenciamento).** A página da função IPAM inclui um bloco **Bem-Vindo** personalizado que contém links para tarefas comuns de configuração e gerenciamento do IPAM, incluindo um assistente para o provisionamento de um servidor IPAM. A home page do IPAM também inclui blocos para exibir a rede gerenciada, o resumo de configuração e tarefas agendadas.

    Existem algumas limitações para o gerenciamento IPAM no Gerenciador do servidor. Diferentemente das páginas de funções e grupos típicas, o IPAM não tem os blocos **Servidores**, **Eventos**, **Desempenho**, **Analisador de Práticas Recomendadas** ou **Serviços**. Não há nenhum modelo de analisador de práticas recomendadas disponíveis para o IPAM; Não há suporte para verificações de analisador de práticas recomendadas no IPAM. Para acessar servidores em seu pool de servidores com o IPAM, crie um grupo personalizado desses servidores com o IPAM e acesse a lista de servidores no bloco **Servidores** na página do grupo personalizado. Alternativamente, acesse os servidores IPAM no bloco **Servidores** na página do grupo **Todos os Servidores**.

    As miniaturas do dashboard também exibem linhas limitadas para o IPAM, em comparação com as miniaturas de outras funções e grupos. Ao clicar nas linhas de miniaturas do IPAM, é possível exibir eventos, dados de desempenho e alertas de status de gerenciabilidade dos servidores com o IPAM. Os serviços relacionados ao IPAM podem ser gerenciados nas páginas dos grupos de servidores que contêm servidores IPAM, como a página do grupo **Todos os Servidores** .

    Para obter mais informações sobre como implantar e gerenciar o IPAM, consulte [endereço IP (IPAM) gerenciamento](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Consulte também
[Gerenciador de servidores](server-manager.md)
[adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md)
[criar e gerenciar grupos de servidores](create-and-manage-server-groups.md)
[exibir e configurar Desempenho, eventos e dados de serviço](view-and-configure-performance-event-and-service-data.md)
[serviços de arquivo e armazenamento](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[serviços de área de trabalho remota (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532) 
 [ Endereço IP IPAM (gerenciamento)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



