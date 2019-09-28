---
title: Configure Features on Demand in Windows Server
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f834ca2e4c4acd045ccaeb4f46142dcc0e86f674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383271"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configure Features on Demand in Windows Server

>Aplica-se a: Windows Server 2016

Este tópico descreve como remover arquivos de recursos em uma configuração de Recursos sob Demanda usando o cmdlet Uninstall-WindowsFeature.

Os recursos sob demanda são um recurso, introduzido no Windows 8 e no Windows Server 2012, que permite remover arquivos de função e recurso (às vezes chamado de *carga*de recursos) do sistema operacional para conservar espaço em disco e instalar funções e recursos do locais remotos ou mídia de instalação em vez de computadores locais. Você pode remover arquivos de recursos de computadores físicos ou virtuais em execução. Também é possível adicionar ou remover arquivos de recursos de arquivos WIM (Imagem do Windows) ou de VHDs (discos rígidos virtuais) offline para criar uma cópia das configurações de Recursos sob Demanda que pode ser reproduzida.

Em uma configuração de recursos sob demanda, quando os arquivos de recurso não estão disponíveis em um computador, se uma instalação exigir esses arquivos de recurso, o Windows Server 2012 R2 ou o Windows Server 2012 pode ser direcionado para obter os arquivos de um repositório de recursos lado a lado (uma pasta compartilhada que contém arquivos de recursos e está disponível para o computador na rede), do Windows Update ou da mídia de instalação. Por padrão, quando os arquivos de recurso não estão disponíveis no servidor de destino, os recursos sob demanda procuram por arquivos de recursos ausentes executando as seguintes tarefas, na ordem mostrada.

1.  Pesquisando em um local que foi especificado por usuários do assistente para adicionar funções e recursos ou comandos de instalação do DISM

2.  Avaliando a configuração da configuração de Política de Grupo, **configurações do computador \ Configuração do Templates\System\Specify para instalação de componente opcional e reparo de componentes**

3.  Pesquisando o Windows Update

Você pode substituir o comportamento padrão de Recursos sob Demanda executando um dos procedimentos a seguir.

-   Especificando um caminho de origem alternativo como parte do cmdlet `Install-WindowsFeature` , adicionando o parâmetro `Source`

-   Especificando um caminho de origem alternativo na página **confirmar opções de instalação** enquanto você está instalando recursos usando o assistente para adicionar funções e recursos

-   Definindo a configuração da Política de Grupo, **Especificar configurações para instalação de componentes opcionais e reparo de componentes**

Este tópico contém as seguintes seções.

-   [Criar um arquivo de recurso ou armazenamento lado a lado](#BKMK_store)

-   [Métodos de remoção de arquivos de recursos](#BKMK_methods)

-   [Remover arquivos de recurso usando Uninstall-WindowsFeature](#BKMK_remove)

## <a name="BKMK_store"></a>Criar um arquivo de recurso ou armazenamento lado a lado
Esta seção descreve como configurar uma pasta compartilhada de arquivo de recurso remoto (também chamada de armazenamento lado a lado) que armazena os arquivos necessários para instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2012 R2 ou o Windows Server 2012. Depois de configurar um repositório de recursos, você pode instalar funções, serviços de função e recursos em servidores que executam esses sistemas operacionais e especificar o repositório de recursos como o local dos arquivos de origem da instalação.

#### <a name="to-create-a-feature-file-store"></a>Para criar um repositório de arquivos de recursos

1.  Crie uma pasta compartilhada em um servidor em sua rede. Por exemplo, *\\ \ network\share\sxs*.

2.  Verifique se você tem as permissões corretas atribuídas ao repositório de recursos. O caminho de origem ou o compartilhamento de arquivos deve conceder permissões de **leitura** ao grupo **todos** (não recomendado por motivos de segurança) ou às contas de computador (*domínio*\\*ServerName*$) de servidores nos quais você planeja instalar recursos usando este repositório de recursos; conceder acesso à conta de usuário não é suficiente.

    Você pode acessar o compartilhamento de arquivos e as configurações de permissões executando um dos procedimentos a seguir na área de trabalho do Windows.

    -   Clique com o botão direito do mouse na pasta compartilhada, clique em **Propriedades** e altere os usuários permitidos e seus direitos de acesso à pasta na guia **Segurança**.

    -   Clique com o botão direito do mouse na pasta compartilhada, aponte para **Compartilhar com**e clique em **Pessoas específicas**.

    > [!NOTE]
    > Os servidores que estão em grupos de trabalho não podem acessar compartilhamentos de arquivos externos, mesmo que a conta do computador do servidor do grupo tenha permissões **Leitura** no compartilhamento externo. Locais de origem alternativos que funcionam para servidores de grupos de trabalho incluem mídias de instalação, o Windows Update e arquivos VHD ou WIM armazenados no servidor de grupo de trabalho local.

3.  Copie a pasta **Sources\SxS** da mídia de instalação do Windows Server para a pasta compartilhada que você criou na etapa 1.

## <a name="BKMK_methods"></a>Métodos de remoção de arquivos de recursos
Há dois métodos disponíveis para remover arquivos de recursos do Windows Server em uma configuração de Recursos sob Demanda.

-   O parâmetro `remove` do cmdlet `Uninstall-WindowsFeature` permite que você exclua arquivos de recursos de um servidor ou VHD (disco rígido virtual) offline que esteja executando o Windows Server 2012 R2 ou o Windows Server 2012. Os valores válidos para o parâmetro `remove` são os nomes de funções, serviços de função e recursos.

-   Os comandos DISM (Gerenciamento e Manutenção de Imagens de Implantação) permitem criar arquivos WIM personalizados que preservam espaço em disco omitindo arquivos de recursos que não são necessários ou que podem ser obtidos de outras fontes remotas. Para obter mais informações sobre como usar o DISM para preparar imagens personalizadas, consulte [Como habilitar ou desabilitar os recursos do Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="BKMK_remove"></a>Remover arquivos de recurso usando Uninstall-WindowsFeature
Você pode usar o cmdlet Uninstall-WindowsFeature para desinstalar funções, serviços de função e recursos de servidores e VHDs offline que executam o Windows Server 2012 R2 ou o Windows Server 2012 e para excluir arquivos de recursos. Você pode desinstalar e excluir as mesmas funções, serviços de função e recursos no mesmo comando, se desejado.

> [!IMPORTANT]
> Quando você exclui arquivos de recursos para uma função, serviço de função ou recurso, funções, serviços de função e recursos que dependem dos arquivos que você está removendo também são excluídos. Se você está excluindo arquivos de recursos de um serviço de função ou sub-recurso e nenhum outro serviço de função ou sub-recurso da função ou do recurso pai permanecer instalado, os arquivos de toda a função ou recurso pai serão excluídos. Para exibir todos os arquivos de recursos que seriam excluídos pelo comando `Uninstall-WindowsFeature -remove`, adicione o parâmetro `whatif` ao comando para executá-lo e exibir os resultados sem realmente excluir os arquivos de recursos.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Para remover arquivos de funções e recursos usando Uninstall-WindowsFeature

1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.

    > [!NOTE]
    > Se você estiver desinstalando funções e recursos de um servidor remoto, não será necessário executar o Windows PowerShell com direitos de usuário elevados.

    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.

    -   Na tela **inicial** do Windows, clique com o botão direito do mouse no bloco Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.

    -   Em um servidor que esteja executando a opção de instalação Server Core, digite **PowerShell** em um prompt de comando e pressione **Enter**.

2.  Digite o seguinte e pressione **Enter**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Exemplo:** o Licenciamento de Área de Trabalho Remota é o último serviço de função restante dos Serviços de Área de Trabalho Remota que é instalado. O comando desinstala o Licenciamento de Área de Trabalho Remota e exclui os arquivos de recursos de toda a função Serviços de Área de Trabalho Remota do servidor especificado, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Exemplo:** No exemplo a seguir, o comando remove os serviços de domínio Active Directory e o gerenciamento de Política de Grupo de um VHD offline. A função e o recurso são desinstalados primeiro, depois seus arquivos de recursos são removidos completamente do VHD offline, *Contoso.vhd*.

    > [!NOTE]
    > Você deve adicionar o parâmetro `computerName` se estiver executando o cmdlet de um computador que esteja executando o Windows 8.1 ou o Windows 8.
    > 
    > Se você inserir o nome de um arquivo VHD de um compartilhamento de rede, esse compartilhamento deverá conceder permissões de **leitura** e **gravação** à conta de computador do servidor que você selecionou para montar o VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Consulte também
[Instale ou desinstale funções, serviços de função ou recursos](install-or-uninstall-roles-role-services-or-features.md)
[Opções de instalação do Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[como habilitar ou desabilitar recursos do Windows](https://technet.microsoft.com/library/hh824822.aspx)
[visão geral do DISM (gerenciamento e manutenção de imagens de implantação)](https://technet.microsoft.com/library/hh825236.aspx)


