---
title: Configure Features on Demand in Windows Server
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c77bd088a02d405b17a4f7decd6093785296d5e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818927"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configure Features on Demand in Windows Server

>Aplica-se a: Windows Server 2016

Este tópico descreve como remover arquivos de recursos em uma configuração de Recursos sob Demanda usando o cmdlet Uninstall-WindowsFeature.

Recursos sob demanda é um recurso, introduzido no Windows 8 e Windows Server 2012, que permite remover arquivos de funções e recursos (às vezes chamado de recurso *carga*) do sistema operacional para preservar espaço em disco e instalar as funções e os recursos de locais remotos ou mídia de instalação, em vez da partir de computadores locais. Você pode remover arquivos de recursos de computadores físicos ou virtuais em execução. Também é possível adicionar ou remover arquivos de recursos de arquivos WIM (Imagem do Windows) ou de VHDs (discos rígidos virtuais) offline para criar uma cópia das configurações de Recursos sob Demanda que pode ser reproduzida.

Em uma configuração recursos sob demanda, quando os arquivos de recursos não estão disponíveis em um computador, se a instalação exige esses arquivos de recurso, Windows Server 2012 R2 ou Windows Server 2012 pode ser direcionado para obter os arquivos de um repositório de recursos de lado a lado (uma pasta compartilhada que contém arquivos de recursos e está disponível para o computador na rede), do Windows Update ou da mídia de instalação. Por padrão, quando os arquivos de recursos não estão disponíveis no servidor de destino, recursos sob demanda procura arquivos de recursos ausentes executando as seguintes tarefas, na ordem mostrada.

1.  Pesquisando em um local que foi especificado pelos usuários de adicionar funções e recursos do assistente ou comandos de instalação do DISM

2.  Avaliar a configuração da configuração de diretiva de grupo, **configuração do computador\Modelos administrativos\sistema\especificar configurações para instalação de componentes opcionais e reparo de componentes**

3.  Pesquisando o Windows Update

Você pode substituir o comportamento padrão de Recursos sob Demanda executando um dos procedimentos a seguir.

-   Especificando um caminho de origem alternativo como parte do cmdlet `Install-WindowsFeature` , adicionando o parâmetro `Source`

-   Especificando um caminho de origem alternativo na **Confirmar Opções de instalação** página enquanto instala recursos usando o Assistente de recursos e adicionar funções

-   Definindo a configuração da Política de Grupo, **Especificar configurações para instalação de componentes opcionais e reparo de componentes**

Este tópico contém as seguintes seções.

-   [Criar um arquivo de recurso ou um repositório lado a lado](#BKMK_store)

-   [Métodos de remoção de arquivos de recursos](#BKMK_methods)

-   [Remover arquivos de recursos usando Uninstall-WindowsFeature](#BKMK_remove)

## <a name="BKMK_store"></a>Criar um arquivo de recurso ou um repositório lado a lado
Esta seção descreve como configurar uma recurso remoto pasta compartilhada de arquivos (também chamada de repositório lado a lado) que armazena os arquivos necessários para instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2012 R2 ou Windows Server 2012. Depois de configurar um repositório de recursos, você pode instalar funções, serviços de função e recursos em servidores que executam esses sistemas operacionais e especificar o repositório de recursos como o local dos arquivos de origem de instalação.

#### <a name="to-create-a-feature-file-store"></a>Para criar um repositório de arquivos de recursos

1.  Crie uma pasta compartilhada em um servidor em sua rede. Por exemplo,  *\\\network\share\sxs*.

2.  Verifique se você tem as permissões corretas atribuídas ao repositório de recursos. O compartilhamento de arquivo ou caminho de origem deve conceder **leitura** permissões para o **Everyone** grupo (não recomendado por motivos de segurança), ou para as contas de computador (*domínio* \\ *SERverNAME*$) dos servidores nos quais você planeja instalar recursos usando esse repositório de recursos; concedendo acesso de conta de usuário não é suficiente.

    Você pode acessar o compartilhamento de arquivos e as configurações de permissões executando um dos procedimentos a seguir na área de trabalho do Windows.

    -   Clique com o botão direito do mouse na pasta compartilhada, clique em **Propriedades** e altere os usuários permitidos e seus direitos de acesso à pasta na guia **Segurança**.

    -   Clique com o botão direito do mouse na pasta compartilhada, aponte para **Compartilhar com**e clique em **Pessoas específicas**.

    > [!NOTE]
    > Os servidores que estão em grupos de trabalho não podem acessar compartilhamentos de arquivos externos, mesmo que a conta do computador do servidor do grupo tenha permissões **Leitura** no compartilhamento externo. Locais de origem alternativos que funcionam para servidores de grupos de trabalho incluem mídias de instalação, o Windows Update e arquivos VHD ou WIM armazenados no servidor de grupo de trabalho local.

3.  Copie a pasta **Sources\SxS** da mídia de instalação do Windows Server para a pasta compartilhada que você criou na etapa 1.

## <a name="BKMK_methods"></a>Métodos de remoção de arquivos de recursos
Há dois métodos disponíveis para remover arquivos de recursos do Windows Server em uma configuração de Recursos sob Demanda.

-   O `remove` parâmetro do `Uninstall-WindowsFeature` cmdlet permite excluir arquivos de recursos de um servidor ou disco rígido virtual offline (VHD) que está executando o Windows Server 2012 R2 ou Windows Server 2012. Os valores válidos para o `remove` parâmetro são os nomes das funções, serviços de função e recursos.

-   Os comandos DISM (Gerenciamento e Manutenção de Imagens de Implantação) permitem criar arquivos WIM personalizados que preservam espaço em disco omitindo arquivos de recursos que não são necessários ou que podem ser obtidos de outras fontes remotas. Para obter mais informações sobre como usar o DISM para preparar imagens personalizadas, consulte [Como habilitar ou desabilitar os recursos do Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="BKMK_remove"></a>Remover arquivos de recursos usando Uninstall-WindowsFeature
Você pode usar o cmdlet Uninstall-WindowsFeature para desinstalar funções, serviços de função e recursos de servidores e VHDs offline que executam o Windows Server 2012 R2 ou Windows Server 2012 e excluir arquivos de recurso. Você pode desinstalar e excluir as funções, serviços de função e recursos no mesmo comando mesmo se desejado.

> [!IMPORTANT]
> Quando você exclui arquivos de recurso para uma função, serviço de função ou recurso, funções, serviços de função e recursos que dependem dos arquivos que estão sendo removidos também são excluídos. Se você está excluindo arquivos de recursos de um serviço de função ou sub-recurso e nenhum outro serviço de função ou sub-recurso da função ou do recurso pai permanecer instalado, os arquivos de toda a função ou recurso pai serão excluídos. Para exibir todos os arquivos de recursos que seriam excluídos pelo comando `Uninstall-WindowsFeature -remove`, adicione o parâmetro `whatif` ao comando para executá-lo e exibir os resultados sem realmente excluir os arquivos de recursos.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Para remover arquivos de funções e recursos usando Uninstall-WindowsFeature

1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.

    > [!NOTE]
    > Se você estiver desinstalando funções e recursos de um servidor remoto, você precisa executar o Windows PowerShell com direitos de usuário elevados.

    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.

    -   Sobre o Windows **inicie** tela, clique com botão direito no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.

    -   Em um servidor que está executando a opção de instalação Server Core, digite **PowerShell** em um prompt de comando e pressione **Enter**.

2.  Digite o seguinte e pressione **Enter**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Exemplo:** o Licenciamento de Área de Trabalho Remota é o último serviço de função restante dos Serviços de Área de Trabalho Remota que é instalado. O comando desinstala o Licenciamento de Área de Trabalho Remota e exclui os arquivos de recursos de toda a função Serviços de Área de Trabalho Remota do servidor especificado, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Exemplo:** No exemplo a seguir, o comando remove os serviços de domínio do Active Directory e o gerenciamento de diretiva de grupo de um VHD offline. A função e o recurso são desinstalados primeiro, depois seus arquivos de recursos são removidos completamente do VHD offline, *Contoso.vhd*.

    > [!NOTE]
    > Você deve adicionar o `computerName` parâmetro se você estiver executando o cmdlet em um computador que está executando o Windows 8.1 ou Windows 8.
    > 
    > Se você inserir o nome de um arquivo VHD de um compartilhamento de rede, esse compartilhamento deverá conceder **leitura** e **gravar** permissões para a conta de computador do servidor que você selecionou para montar o VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Consulte também
[Instalar ou desinstalar funções, serviços de função ou recursos](install-or-uninstall-roles-role-services-or-features.md)
[opções de instalação do Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[como habilitar ou desabilitar os recursos do Windows](https://technet.microsoft.com/library/hh824822.aspx) 
 [Implantação visão geral do DISM (gerenciamento) e a manutenção de imagens](https://technet.microsoft.com/library/hh825236.aspx)


