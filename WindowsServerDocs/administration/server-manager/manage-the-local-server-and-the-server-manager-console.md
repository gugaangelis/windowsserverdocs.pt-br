---
title: Manage the Local Server and the Server Manager Console
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45bb0efcaf989cadd717ddbfde27230b76901113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383111"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, Gerenciador do Servidor permite que você gerencie o servidor local (se você estiver executando o Gerenciador do Servidor no Windows Server, e não em um sistema operacional cliente baseado no Windows) e servidores remotos que executam o Windows Server 2008 e versões mais recentes do Windows Sistema operacional do servidor.

A página **servidor local** em Gerenciador do servidor exibe as propriedades do servidor, os eventos, os dados do contador de desempenho e do serviço e os resultados de analisador de práticas recomendadas (BPA) para o servidor local. Os blocos de eventos, serviços, BPA e desempenho funcionam da mesma maneira que em páginas de grupo de servidores e função. Para obter mais informações sobre como configurar os dados exibidos nesses blocos, consulte [View e Configure Performance, Event, e Service Data](view-and-configure-performance-event-and-service-data.md) e [Run Best Practices Analyzer Scans e Manage Scan Results](run-best-practices-analyzer-scans-and-manage-scan-results.md).

Os comandos de menu e as configurações nas barras de título do console do Gerenciador do Servidor se aplicam globalmente a todos os servidores em seu pool de servidores e permitem que você use Gerenciador do Servidor para gerenciar todo o pool de servidores.

Este tópico contém as seguintes seções.

-   [Desligar o servidor local](#BKMK_shutdown)

-   [Configurar propriedades de Gerenciador do Servidor](#BKMK_props)

-   [Gerenciar o console do Gerenciador do Servidor](#BKMK_managesm)

-   [Personalizar as ferramentas exibidas no menu ferramentas](#BKMK_tools)

-   [Gerenciar funções em home pages de função](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Desligar o servidor local
O menu **tarefas** no bloco **Propriedades** do servidor local permite que você inicie uma sessão do Windows PowerShell no servidor local, abra o snap-in **Gerenciamento do computador** do MMC ou abra snap-ins do MMC para funções ou recursos que estão instalados no servidor local. Você também pode desligar o servidor local usando o comando **Desligar Servidor Local** nesse menu **Tarefas** . O comando **Desligar Servidor Local** também está disponível para o servidor local no bloco **Servidores** na página **Todos os Servidores** ou na página de qualquer função ou grupo na qual o servidor local esteja representado.

Desligar o servidor local usando esse método, ao contrário de desligar o Windows Server 2016 da tela **inicial** , abre a caixa de diálogo **desligar o Windows** , que permite especificar os motivos de desligamento na área do **rastreador de eventos de desligamento** .

> [!NOTE]
> Somente os membros do grupo Administradores podem desligar ou reiniciar um servidor. Os usuários padrão não podem desligar ou reiniciar um servidor. Ao clicar no comando **Desligar Servidor Local** , é feito logoff dos usuários padrão das sessões do servidor local. Isso corresponde à experiência de um usuário padrão que executa o comando de desligamento **Alt+F4** na área de trabalho do servidor.

## <a name="BKMK_props"></a>Configurar propriedades de Gerenciador do Servidor
Você pode exibir ou alterar as seguintes configurações no bloco **Propriedades** na página **Servidor Local** . Para alterar o valor de uma configuração, clique no valor de hipertexto da configuração.

> [!NOTE]
> Normalmente, as propriedades exibidas no bloco **Propriedades** do Servidor Local podem ser alteradas apenas no servidor local. Você não pode alterar as propriedades do servidor local de um computador remoto usando Gerenciador do Servidor porque o bloco **Propriedades** só pode obter informações sobre o computador local, não para computadores remotos.
> 
> Como muitas propriedades exibidas no bloco **Propriedades** são controladas por ferramentas que não fazem parte do Gerenciador do servidor (painel de controle, por exemplo), as alterações nas configurações de **Propriedades** nem sempre são exibidas no bloco **Propriedades** imediatamente. Por padrão, os dados do bloco **Propriedades** são atualizadas a cada dois minutos. Para atualizar os dados do bloco de **Propriedades** imediatamente, clique em **Atualizar** na barra de endereços do Gerenciador do servidor.

|Configuração|Descrição|
|------|--------|
|nome do computador|Exibe o nome amigável do computador e abre a caixa de diálogo **Propriedades do sistema** , que permite alterar o nome do servidor, a associação do domínio e outras configurações do sistema, como perfis de usuário.|
|Domínio (ou Grupo de Trabalho, se o servidor não tiver ingressado em um domínio)|Exibe o domínio ou o grupo de trabalho do qual o servidor é membro. Abre a caixa de diálogo **Propriedades do sistema** , que permite alterar o nome do servidor, a associação do domínio e outras configurações do sistema, como perfis de usuário.|
|Firewall do Windows|Exibe o status do Firewall do Windows para o servidor local. Abre **Painel de Controle\Sistema e Segurança\Firewall do Windows**. Para obter mais informações sobre como configurar o firewall do Windows, consulte [Firewall do Windows com segurança avançada e IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|gerenciamento remoto|Exibe Gerenciador do Servidor e o status de gerenciamento remoto do Windows PowerShell. Abre a caixa de diálogo **Configurar gerenciamento remoto** . Para obter mais informações sobre gerenciamento remoto, consulte [Configurar o gerenciamento remoto no Gerenciador do servidor](configure-remote-management-in-server-manager.md).|
|Área de Trabalho Remota|Mostra se os usuários podem se conectar ao servidor remotamente usando sessões da Área de Trabalho Remota. Abre a guia **remoto** da caixa de diálogo **Propriedades do sistema** .|
|Agrupamento NIC|Mostra se o servidor local está participando do Agrupamento NIC. Abre a caixa de diálogo **Agrupamento NIC** e permite o ingresso no servidor local para um Agrupamento NIC, se desejado. Para obter mais informações sobre Agrupamento NIC, consulte o [White paper Agrupamento NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Exibe o status de rede do servidor. Abre **Painel de Controle\Rede e Internet\Conexões de Rede**.|
|Versão do sistema operacional|Este campo somente leitura exibe o número da versão do sistema operacional Windows em execução no servidor local.|
|Informações de hardware|Este campo somente leitura exibir o fabricante, o nome e o número do modelo do hardware do servidor.|
|Últimas atualizações instaladas|Exibe o dia e a hora em que atualizações do Windows foram instaladas pela última vez. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Windows Update|Exibe as configurações do Windows Update para o servidor local. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Última verificação de atualizações|Exibe o dia e a hora em que o servidor verificou atualizações disponíveis do Windows pela última vez. Abre **Painel de Controle\Sistema e Segurança\Windows Update**.|
|Relatório de erros do Windows|Exibe o status de aceite do Relatório de Erros do Windows. Abre a caixa de diálogo **Configuração do Relatório de Erros do Windows** . Para obter mais informações sobre Relatórios de Erros do Windows, seus benefícios, políticas de privacidade e configurações de aceite, consulte [Relatórios de erros do Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programa de Aperfeiçoamento da Experiência do Usuário|Exibe o status de aceite do Programa de Aperfeiçoamento da Experiência do Usuário do Windows. Abre a caixa de diálogo **Configuração do Programa de Aperfeiçoamento da Experiência do Usuário** . Para obter mais informações sobre o Programa de Aperfeiçoamento da Experiência do Usuário, seus benefícios e suas configurações de aceite, consulte [Programa de Aperfeiçoamento da Experiência do Usuário do Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuração de Segurança Aprimorada do Internet Explorer (IE)|Mostra se a Configuração de Segurança Aprimorada do IE (também conhecida como fortalecimento do IE ou IE ESC) está ativada ou desativada. Abre a caixa de diálogo **Configuração de Segurança Aprimorada do Internet Explorer** . A Configuração de Segurança Aprimorada do IE é uma medida de segurança para servidores que impede que páginas da Web sejam abertas no Internet Explorer. Para mais informações sobre Configuração de segurança avançada do IE, seus benefícios e configurações, consulte [Internet Explorer: Configuração de segurança aprimorada](https://go.microsoft.com/fwlink/?LinkId=253461).|
|fuso horário|Exibe o fuso horário do servidor local. Abre a caixa de diálogo **data e hora** .|
|ID do Produto|Exibe o status de ativação do Windows e o número da ID do produto (se o Windows tiver sido ativado) do sistema operacional Windows Server 2016. Esse número não é o mesmo que a chave do produto do Windows. Abre a caixa de diálogo **Ativação do Windows** .|
|Processadores|Esse campo somente leitura exibe informações de fabricante, nome do modelo e velocidade sobre os processadores do servidor local.|
|Memória instalada (RAM)|Este campo somente leitura exibe a quantidade de RAM disponível, em gigabytes.|
|Espaço total em disco|Este campo somente leitura exibe a quantidade de espaço em disco disponível, em gigabytes.|

## <a name="BKMK_managesm"></a>Gerenciar o console do Gerenciador do Servidor
As configurações globais que se aplicam a todo o console de Gerenciador do Servidor e a todos os servidores remotos que foram adicionados ao pool de Servidores Gerenciador do Servidor, são encontradas nas barras de título na parte superior da janela do console do Gerenciador do Servidor.

### <a name="add-servers-to-server-manager"></a>adicionar servidores a Gerenciador do Servidor
O comando que abre a caixa de diálogo **adicionar servidores** e permite adicionar servidores físicos ou virtuais remotos ao pool de servidores do Gerenciador do servidor, está no menu **gerenciar** do console do Gerenciador do servidor. Para obter informações detalhadas sobre como adicionar servidores, consulte [adicionar servidores a Gerenciador do servidor](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Atualizar os dados exibidos no Gerenciador do Servidor
Você pode configurar o intervalo de atualização para dados exibidos no Gerenciador do Servidor na caixa de diálogo **Propriedades do Gerenciador do servidor** , que você abre no menu **gerenciar** .

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Para configurar o intervalo de atualização no Gerenciador do Servidor

1.  No menu **gerenciar** do console do Gerenciador do servidor, clique em **Propriedades de Gerenciador do servidor**.

2.  Na caixa de diálogo **Propriedades do Gerenciador do servidor** , especifique um período de tempo, em minutos, para a quantidade de tempo decorrido que você deseja entre as atualizações dos dados exibidos no Gerenciador do servidor. O padrão é de 10 minutos. Ao concluir, clique em OK.

#### <a name="refresh-limitations"></a>Limitações de atualização
A atualização se aplica globalmente aos dados de todos os servidores que você adicionou ao pool de servidores do Gerenciador do Servidor. Não é possível atualizar dados ou configurar intervalos de atualização diferentes para servidores, funções ou grupos individuais.

Quando servidores que estão em um cluster são adicionados a Gerenciador do Servidor, sejam computadores físicos ou máquinas virtuais, a primeira atualização de dados pode falhar ou exibir dados somente para o servidor host para objetos clusterizados. As atualizações subsequentes mostram dados precisos de servidores físicos ou virtuais em um cluster de servidores.

Os dados exibidos nas páginas iniciais da função no Gerenciador do Servidor para Serviços de Área de Trabalho Remota, o gerenciamento de endereços IP e os serviços de arquivo e armazenamento não são atualizados automaticamente. Atualize os dados exibidos nessas páginas manualmente, pressionando **F5** ou clicando em **atualizar** no cabeçalho Gerenciador do servidor console enquanto estiver nessas páginas.

### <a name="add-or-remove-roles-or-features"></a>Adicionar ou remover funções ou recursos
Os comandos que abrem o assistente para adicionar funções e recursos e removem o assistente de funções e recursos e permitem adicionar ou remover funções, serviços de função e recursos para servidores no pool de servidores, estão no menu **gerenciar** do console do Gerenciador do servidor e no menu **tarefas** do bloco **funções e recursos** em páginas de função ou de grupo. Para obter informações detalhadas sobre como adicionar ou remover funções ou recursos, consulte [Install or Uninstall Roles, Role Services, or Features](install-or-uninstall-roles-role-services-or-features.md).

No Gerenciador do Servidor, os dados de função e de recurso são exibidos no idioma base do sistema, também chamado de idioma de GUI padrão do sistema ou no idioma selecionado durante a instalação do sistema operacional.

### <a name="create-server-groups"></a>criar grupos de servidores
O comando que abre a caixa de diálogo **Criar grupo** de servidores e permite que você crie grupos de servidores personalizados, está no menu **gerenciar** do console do Gerenciador do servidor. Para obter informações detalhadas sobre como criar grupos de servidores, consulte [criar e gerenciar grupos de servidores](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedir que o Gerenciador do Servidor seja aberto automaticamente no logon
A caixa de seleção não **iniciar Gerenciador do servidor automaticamente no logon** na caixa de diálogo **Propriedades do Gerenciador do Servidor** controla se Gerenciador do servidor é aberto automaticamente no logon para membros do grupo Administradores em um servidor local. Essa configuração não afeta o comportamento de Gerenciador do Servidor quando ele está em execução no Windows 10 como parte do Ferramentas de Administração de Servidor Remoto. Para obter mais informações sobre como definir essa configuração, consulte [Gerenciador do servidor](server-manager.md).

### <a name="zoom-in-or-out"></a>Ampliar ou reduzir
Para ampliar ou reduzir a exibição do console do Gerenciador do Servidor, você pode usar os comandos de **zoom** no menu **Exibir** ou pressionar **Ctrl + mais (+)** para ampliar e **Ctrl + menos (-)** para reduzir.

## <a name="BKMK_tools"></a>Personalizar as ferramentas exibidas no menu ferramentas
O menu **ferramentas** do Gerenciador do servidor inclui links suaves para atalhos na pasta **Ferramentas administrativas** no **painel de controle/sistema e segurança**. A pasta **Ferramentas administrativas** contém uma lista de atalhos ou arquivos lnk para as ferramentas de gerenciamento disponíveis, como snap-ins do mmc. Gerenciador do servidor popula o menu **ferramentas** com links para esses atalhos e copia a estrutura de pastas da pasta **Ferramentas administrativas** para o menu **ferramentas** . Por padrão, as ferramentas na pasta Ferramentas Administrativas são organizadas em uma lista simples, classificadas por tipo e por nome. No menu**ferramentas** Gerenciador do servidor, os itens são classificados somente por nome, não por tipo.

Para personalizar o menu **Ferramentas** , copie os atalhos das ferramentas ou dos scripts que você deseja usar para a pasta **Ferramentas Administrativas** . Você também pode organizar seus atalhos em pastas, o que cria menus em cascata no menu **Ferramentas** . Além disso, se você quiser restringir o acesso às ferramentas personalizadas no menu **ferramentas** , poderá definir direitos de acesso de usuário em suas pastas de ferramentas personalizadas em ferramentas administrativas ou diretamente na ferramenta original ou nos arquivos de script.

É recomendável não reorganizar as ferramentas administrativas e do sistema, nem as ferramentas de gerenciamento associadas a funções e recursos instalados no servidor local. A movimentação de ferramentas de gerenciamento de funções e recursos pode impedir a desinstalação com êxito dessas ferramentas de gerenciamento, quando necessário. Depois de desinstalar uma função ou um recurso, um link não funcional para uma ferramenta cujo atalho foi movido pode permanecer no menu **Ferramentas**. Se você reinstalar a função, será criado um link duplicado da mesma ferramenta no menu **Ferramentas**, mas um dos links não funcionará.

Contudo, as ferramentas de funções e recursos instaladas como parte das Ferramentas de Administração de Servidor Remoto em um computador baseado no cliente Windows podem ser organizadas em pastas personalizadas. A desinstalação da função ou do recurso pai não tem nenhum efeito sobre os atalhos de ferramenta que estão disponíveis em um computador remoto que esteja executando o Windows 10.

O procedimento a seguir descreve como criar uma pasta de exemplo chamada *MyTools*e mover atalhos para dois scripts do Windows PowerShell para a pasta que, em seguida, é acessível no menu ferramentas do Gerenciador do servidor.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Para personalizar o menu Ferramentas adicionando atalhos em Ferramentas Administrativas

1.  Crie uma nova pasta chamada *MyTools* em um local conveniente.

    > [!NOTE]
    > Devido a direitos de acesso restritos na pasta **Ferramentas Administrativas** , você não tem permissão para criar uma nova pasta diretamente na pasta **Ferramentas Administrativas** ; crie uma nova pastas em outro lugar (como a Área de Trabalho) e copie a nova pasta para a pasta **Ferramentas Administrativas** .

2.  Mova ou copie *MyTools* para **painel de controle/sistema e ferramentas administrativas/de segurança**. Por padrão, você deve ser membro do grupo Administradores no computador para fazer alterações à pasta **Ferramentas Administrativas** .

3.  Se você não precisar restringir os direitos de acesso de usuário aos seus atalhos de ferramenta personalizada, vá para a etapa 6. Caso contrário, clique com o botão direito do mouse no arquivo da ferramenta (ou na pasta *MinhasFerramentas*) e clique em **Propriedades**.

4.  Na guia **segurança** da caixa de diálogo **Propriedades** do arquivo, clique em **Editar**.

5.  para os usuários para os quais você deseja restringir o acesso à ferramenta, desmarque as caixas de seleção para **ler & executar**, **ler**e **gravar** permissões. Essas permissões são herdadas pelo atalho da ferramenta na pasta **Ferramentas Administrativas**.

    Se você editar direitos de acesso para um usuário enquanto o usuário estiver usando Gerenciador do Servidor (ou enquanto Gerenciador do Servidor estiver aberto), as alterações não serão mostradas no menu **ferramentas** até que o usuário reinicie Gerenciador do servidor.

    > [!NOTE]
    > Se você restringir o acesso a uma pasta inteira que você copiou para ferramentas administrativas, os usuários restritos poderão ver nem a pasta nem seu conteúdo no menu**ferramentas** do Gerenciador do servidor.
    > 
    > Edite permissões para a pasta na pasta **Ferramentas administrativas** . Como arquivos ocultos e pastas em ferramentas administrativas são sempre exibidos no menu**ferramentas** de Gerenciador do servidor, não use a configuração **oculta** em uma caixa de diálogo **Propriedades** de arquivo ou pasta para restringir o acesso de usuário aos seus atalhos de ferramenta personalizada.
    > 
    > As permissões **Negar** sempre substituem as permissões **Permitir**.

6.  Clique com o botão direito do mouse na ferramenta original, no script ou no arquivo executável para o qual você deseja adicionar entradas no menu **ferramentas** e clique em **criar atalho**.

7.  Mova o atalho para a pasta *MyTools* em ferramentas administrativas.

8.  Atualize ou reinicie Gerenciador do Servidor, se necessário, para ver o atalho da ferramenta personalizada no menu **ferramentas** .

## <a name="BKMK_roles"></a>Gerenciar funções em home pages de função
Depois de adicionar servidores ao pool de servidores do Gerenciador do Servidor e Gerenciador do Servidor coletar dados de inventário sobre servidores em seu pool, Gerenciador do Servidor adiciona páginas ao painel de navegação para funções que são descobertas em servidores gerenciados. O bloco **Servidores** nas páginas de funções lista os servidores gerenciados que executam a função. Por padrão, os blocos **Eventos**, **Analisador de Práticas Recomendadas**, **Serviços**e **Desempenho** exibem dados de todos os serviços que executam a função. A seleção de um servidor específico no bloco **Servidores** limita o escopo dos resultados de eventos, serviços, contadores de desempenho e BPA somente aos servidores selecionados. As ferramentas de gerenciamento normalmente estão disponíveis no menu de **ferramentas** do console Gerenciador do servidor, depois que uma função ou um recurso tiver sido instalado ou descoberto em um servidor gerenciado. Você também pode clicar com o botão direito do mouse nas entradas do servidor no bloco **Servidores** de uma função ou um grupo e então iniciar a ferramenta de gerenciamento que deseja usar.

No Windows Server 2016, as funções e recursos a seguir têm ferramentas de gerenciamento integradas ao console do Gerenciador do Servidor como páginas.

-   **Serviços de arquivo e armazenamento.** As páginas de serviços de arquivo e armazenamento incluem blocos e comandos personalizados para gerenciar volumes, compartilhamentos, discos virtuais iSCSI e pools de armazenamento. Quando você abre a função serviços de arquivo e armazenamento home page no Gerenciador do Servidor, um painel de cancelamento é aberto e exibe páginas de gerenciamento personalizadas para serviços de arquivo e armazenamento. Para obter mais informações sobre como implantar e gerenciar Serviços de arquivo e armazenamento, consulte [Serviços de arquivo e armazenamento](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Serviços de Área de Trabalho Remota.** As páginas de Serviços de Área de Trabalho Remota incluem blocos e comandos personalizados para gerenciar sessões, licenças, gateways e áreas de trabalho virtuais. Para obter mais informações sobre como implantar e gerenciar Serviços de Área de Trabalho Remota, consulte [serviços de área de trabalho remota (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **IPAM (gerenciamento de endereços IP).** A página da função IPAM inclui um bloco **Bem-Vindo** personalizado que contém links para tarefas comuns de configuração e gerenciamento do IPAM, incluindo um assistente para o provisionamento de um servidor IPAM. A home page do IPAM também inclui blocos para exibir a rede gerenciada, o resumo de configuração e tarefas agendadas.

    Há algumas limitações para o gerenciamento de IPAM no Gerenciador do Servidor. Diferentemente das páginas de funções e grupos típicas, o IPAM não tem os blocos **Servidores**, **Eventos**, **Desempenho**, **Analisador de Práticas Recomendadas** ou **Serviços**. Não há um modelo de Analisador de Práticas Recomendadas disponível para o IPAM; Não há suporte para verificações de Analisador de Práticas Recomendadas no IPAM. Para acessar servidores em seu pool de servidores com o IPAM, crie um grupo personalizado desses servidores com o IPAM e acesse a lista de servidores no bloco **Servidores** na página do grupo personalizado. Alternativamente, acesse os servidores IPAM no bloco **Servidores** na página do grupo **Todos os Servidores**.

    As miniaturas do dashboard também exibem linhas limitadas para o IPAM, em comparação com as miniaturas de outras funções e grupos. Ao clicar nas linhas de miniaturas do IPAM, é possível exibir eventos, dados de desempenho e alertas de status de gerenciabilidade dos servidores com o IPAM. Os serviços relacionados ao IPAM podem ser gerenciados nas páginas dos grupos de servidores que contêm servidores IPAM, como a página do grupo **Todos os Servidores** .

    para obter mais informações sobre como implantar e gerenciar o IPAM, consulte [IPAM (gerenciamento de endereço IP)](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Consulte também
[Gerenciador do Servidor](server-manager.md)
[adicionar servidores a Gerenciador do servidor](add-servers-to-server-manager.md)
[criar e gerenciar grupos de servidores](create-and-manage-server-groups.md)
[Exibir e configurar dados de desempenho, eventos e serviços](view-and-configure-performance-event-and-service-data.md)
de [serviços de arquivo e armazenamento](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[serviços de área de trabalho remota (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532)
[Gerenciamento de endereço IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



