---
title: Ferramentas de Administração de Servidor Remoto
description: Tópico de nível superior para ferramentas de administração de servidor remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 30ca0a1e8a2f17f54a8f05d7270bf9512be7a8dc
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805177"
---
# <a name="remote-server-administration-tools"></a>Ferramentas de Administração de Servidor Remoto

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico dá suporte ao Remote Server Administration ferramentas para Windows 10.

> [!IMPORTANT]
> Começando com o Windows 10 de outubro de 2018 Update, o RSAT é incluído como um conjunto de **recursos sob demanda** no Windows 10 em si. Ver **quando usar qual versão do RSAT** abaixo para obter instruções de instalação.

RSAT permite que os administradores de TI gerenciar recursos e funções do Windows Server a partir de um computador Windows 10.

Ferramentas de administração de servidor remoto inclui o Gerenciador do servidor, snap-ins do Microsoft Management Console (mmc), consoles, cmdlets do Windows PowerShell e provedores, bem como algumas ferramentas de linha de comando para gerenciar funções e recursos que são executados no Windows Server.

Ferramentas de administração de servidor remoto inclui módulos de cmdlet do Windows PowerShell que podem ser usados para gerenciar funções e recursos que são executados em servidores remotos. Embora o gerenciamento remoto do Windows PowerShell é habilitado por padrão no Windows Server 2016, ele não está habilitado por padrão no Windows 10. Para executar os cmdlets que fazem parte das ferramentas de administração de servidor remoto em um servidor remoto, execute `Enable-PSremoting` em uma sessão do Windows PowerShell que tenha sido aberta com direitos de usuário elevados (ou seja, executar como administrador) em seu computador de cliente do Windows após Instalando ferramentas de administração de servidor remoto.

## <a name="BKMK_Thresh"></a>Ferramentas de administração de servidor remoto para Windows 10
Use o Remote Server Administration ferramentas para Windows 10 para gerenciar tecnologias específicas em computadores que executam o Windows Server 2016, Windows Server 2012 R2, e em casos limitados, Windows Server 2012 ou Windows Server 2008 R2.

Remote Server Administration ferramentas para Windows 10 inclui suporte para gerenciamento remoto de computadores que estão executando a opção de instalação Server Core ou a configuração de Interface mínima do servidor do Windows Server 2016, Windows Server 2012 R2 e em limited casos, as opções de instalação Server Core do Windows Server 2012. No entanto, o Remote Server Administration ferramentas para Windows 10 não pode ser instalado em todas as versões do sistema operacional Windows Server.

### <a name="tools-available-in-this-release"></a>Ferramentas disponíveis nesta versão
Para obter uma lista das ferramentas disponíveis no Remote Server Administration ferramentas para Windows 10, consulte a tabela [remoto administração de servidor Tools (RSAT) para sistemas operacionais Windows de](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos de sistema
Remote Server Administration ferramentas para Windows 10 pode ser instalado somente em computadores que executam o Windows 10. Ferramentas de administração de servidor remoto não pode ser instaladas em computadores que executam o Windows RT 8.1 ou outros dispositivos de sistema no chip.

Remote Server Administration ferramentas para Windows 10 é executado em edições com base em x86 e baseadas em x64 do Windows 10.

> [!IMPORTANT]
> Remote Server Administration ferramentas para Windows 10 não deve ser instalado em um computador que execute pacotes de ferramentas de administração para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Remover todas as versões mais antigas do pacote de ferramentas de administração ou ferramentas de administração de servidor remoto, inclusive versões de pré-lançamento anteriores e as versões das ferramentas para diferentes idiomas e localidades do computador antes de instalar a administração de servidor remoto Ferramentas para Windows 10.

Para usar esta versão do Gerenciador do servidor para acessar e gerenciar servidores remotos que estão executando o Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, você deve instalar várias atualizações para tornar os sistemas operacionais Windows Server mais antigos gerenciáveis usando Se Gerenciador de servidores. Para obter informações detalhadas sobre como preparar o Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 para o gerenciamento usando o Gerenciador do servidor no Remote Server Administration ferramentas para Windows 10, consulte [Manage Multiple, Remote Servers com o Gerenciador do servidor](https://technet.microsoft.com/library/hh831456.aspx).

Gerenciamento remoto do Windows PowerShell e Gerenciador do servidor deve ser habilitado em servidores remotos para gerenciá-los usando as ferramentas que fazem parte do Remote Server Administration ferramentas para Windows 10. Gerenciamento remoto é habilitado por padrão nos servidores que executam o Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Para obter mais informações sobre como habilitar o gerenciamento remoto se ele tiver sido desabilitado, consulte [Gerenciar vários servidores remotos com o Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalar, desinstalar e ativar/desativar ferramentas RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Atualizar o uso de recursos sob demanda (FoD) para instalar ferramentas do RSAT específicas no Windows 10 de outubro de 2018, ou posterior

Começando com o Windows 10 de outubro de 2018 Update, o RSAT é incluído como um conjunto de **recursos sob demanda** diretamente do Windows 10. Agora, em vez de baixar um pacote RSAT você pode simplesmente acessar **gerenciar recursos opcionais** na **configurações** e clique em **adicionar um recurso** para ver a lista de ferramentas do RSAT disponíveis. Selecione e instale as ferramentas RSAT específicas que necessárias. Para ver o progresso da instalação, clique o **volta** botão para exibir o status de **gerenciar recursos opcionais** página.

Consulte a [disponível por meio das ferramentas de lista do RSAT **recursos sob demanda**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além da instalação por meio do gráfico **as configurações** aplicativo, você também pode instalar as ferramentas RSAT específicas por meio da linha de comando ou usando automação [ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Uma vantagem de recursos sob demanda é que os recursos instalados persistirem entre as atualizações de versão do Windows 10!

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar o RSAT específico ferramentas no Windows 10 de outubro de 2018 atualização ou posterior (após a instalação com FoD)

No Windows 10, abra o **as configurações** aplicativo, vá para **gerenciar recursos opcionais**, selecione e desinstalar as ferramentas RSAT específicas que você deseja remover. Observe que em alguns casos, você precisará desinstalar manualmente as dependências. Especificamente, se A ferramenta de RSAT for necessário pela ferramenta RSAT B, escolhendo desinstalar RSAT ferramenta um falhará se a ferramenta RSAT B ainda está instalada. Nesse caso, desinstale primeiro a ferramenta RSAT B e, em seguida, desinstale A. ferramenta RSAT Além disso, observe que em alguns casos, desinstalar uma ferramenta RSAT poderá parecer ter êxito mesmo que a ferramenta ainda está instalada. Nesse caso, reiniciar o computador concluir a remoção da ferramenta.

Consulte a [lista de ferramentas do RSAT incluindo dependências](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além de desinstalação por meio do aplicativo configurações gráfico, você também pode desinstalar as ferramentas RSAT específicas por meio da linha de comando ou usando automação [ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usar qual versão do RSAT

Se você tiver uma versão do Windows 10 antes de outubro de 2018 (1809) de atualização, você não poderá usar **recursos sob demanda**. Você precisará baixar e instalar o pacote RSAT.

- **Instalar o RSAT FODs diretamente do Windows 10, conforme descrito acima**: Ao instalar no Windows 10 de outubro de 2018 atualizar (1809) ou posterior, para gerenciar o Windows Server 2019 ou versões anteriores.

- **Baixe e instale o pacote RSAT WS_1803, conforme descrito a seguir**: Ao instalar no Windows 10 de abril de 2018 atualizar (1803) ou anterior, para gerenciar o Windows Server, versão 1803 ou Windows Server, versão 1709.

- **Baixe e instale o pacote RSAT WS2016, conforme descrito a seguir**: Ao instalar no Windows 10 de abril de 2018 atualizar (1803) ou anterior, para gerenciar o Windows Server 2016 ou versões anteriores.

#### <a name="BKMK_installthresh"></a>Baixe o pacote do RSAT para instalar o Remote Server Administration ferramentas para Windows 10

1.  Baixe o pacote Remote Server Administration Tools para Windows 10 a [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). Você também pode executar o instalador do site do Centro de Download ou salvar o pacote de download em um computador ou compartilhamento local.

    > [!IMPORTANT]
    > Você só pode instalar o Remote Server Administration ferramentas para Windows 10 em computadores que executam o Windows 10. Ferramentas de administração de servidor remoto não pode ser instaladas em computadores que executam o Windows RT 8.1 ou outros dispositivos de sistema no chip.

2.  Se você salvar o pacote de download em um computador ou compartilhamento local, clique duas vezes no programa instalador, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, dependendo da arquitetura do computador no qual você deseja instalar as ferramentas.

3.  Quando a caixa de diálogo **Instalador Autônomo do Windows Update** perguntar se você deseja instalar a atualização, clique em **Sim**.

4.  Leia e aceite os termos de licença. Clique em **Aceito**.

5.  A instalação requer alguns minutos para ser concluída.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Para desinstalar as ferramentas de administração de servidor remoto para Windows 10 (após a instalação do pacote RSAT)

1. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows** e, em seguida, clique em **Painel de Controle**.

2. Em **Programas**, clique em **Desinstalar um programa**.

3. Clique em **Exibir atualizações instaladas**.

4. Clique com o botão direito do mouse em **Atualizar para Microsoft Windows (KB2693643)** e clique em **Desinstalar**.

5. Ao ser perguntado se tem certeza de que deseja desinstalar a atualização, clique em **Sim**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Para desativar ferramentas específicas (depois de instalar o pacote RSAT)

6. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows** e, em seguida, clique em **Painel de Controle**.

7. Clique em **Programas**; em seguida, em **Programas e Recursos** , clique em **Ativar ou desativar recursos do Windows**.

8. Na caixa de diálogo **Recursos do Windows**, expanda **Ferramentas de Administração de Servidor Remoto** e depois expanda **Ferramentas de Administração de Funções** ou **Ferramentas de Administração de Recursos**.

9. Limpe as caixas de seleção para qualquer ferramenta que você deseja desativar.

   > [!NOTE]
   > Se você desativar o Gerenciador de servidor, o computador deve ser reiniciado e as ferramentas antes acessíveis a partir o **ferramentas** menu do Gerenciador do servidor deve ser aberto a partir de **ferramentas administrativas** pasta.

10. Quando terminar de desabilitar as ferramentas que não deseje mais usar, clique em **OK**.

### <a name="run-remote-server-administration-tools"></a>Executar Ferramentas de Administração de Servidor Remoto

> [!NOTE]
> Depois de instalar o Remote Server Administration ferramentas para Windows 10, o **ferramentas administrativas** pasta é exibida na **iniciar** menu. Você pode acessar as ferramentas nos seguintes locais.
>
> -   O **ferramentas** menu no console do Gerenciador do servidor.
> -   **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas**.
> -   Um atalho salvo na área de trabalho da pasta **Ferramentas Administrativas** (para fazer isso, clique com o botão direito do mouse no link **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas** e clique em **Criar Atalho**).

As ferramentas instaladas como parte do Remote Server Administration ferramentas para Windows 10 não podem ser usadas para gerenciar o computador cliente local. Independentemente da ferramenta que você execute, você deve especificar um servidor remoto ou vários servidores remotos, no qual executar a ferramenta. Como a maioria das ferramentas são integradas ao Gerenciador do servidor, adicione os servidores remotos que você deseja gerenciar ao pool de servidores do Gerenciador do servidor antes de gerenciar o servidor usando as ferramentas na **ferramentas** menu. Para obter mais informações sobre como adicionar servidores ao pool de servidores e criar grupos personalizados de servidores, consulte [Adicionar servidores ao Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Criar e gerenciar grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

Nas ferramentas de administração de servidor remoto para Windows 10, todas as ferramentas de gerenciamento de servidor baseado em GUI, como snap-ins do mmc e caixa de diálogo caixas, são acessados a partir de **ferramentas** menu do console do Gerenciador do servidor. Embora o computador que executa o Remote Server Administration ferramentas para Windows 10 executa um sistema operacional baseado no cliente, depois de instalar as ferramentas, o Gerenciador do servidor, incluído com o Remote Server Administration ferramentas para Windows 10, é aberto automaticamente por padrão no computador cliente. Observe que não há nenhuma **servidor Local** página no console do Gerenciador do servidor que executa em um computador cliente.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar o Gerenciador do Servidor em um computador cliente

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**e, em seguida, clique em **Ferramentas Administrativas**.

2.  Na pasta **Ferramentas Administrativas** , clique em **Gerenciador do Servidor**.

Embora não estejam listados no console do Gerenciador do servidor **ferramentas** menu, cmdlets do Windows PowerShell e ferramentas de gerenciamento do prompt de comando também são instaladas para funções e recursos como parte das ferramentas de administração de servidor remoto. Por exemplo, se você abre uma sessão do Windows PowerShell com direitos de usuário elevados (Executar como administrador) e execute o cmdlet `Get-Command -Module RDManagement`, os resultados incluem uma lista dos cmdlets de serviços de área de trabalho remotas que agora estão disponíveis para execução no computador local depois de Instalando ferramentas de administração de servidor remoto, desde que os cmdlets são destinados a um servidor remoto que está executando toda ou parte da função de serviços de área de trabalho remota.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar o Windows PowerShell com direitos de usuário elevados (Executar como administrador)

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Windows PowerShell**.

2.  Para executar o Windows PowerShell como administrador na área de trabalho, clique com botão direito do **Windows PowerShell** atalho e clique **executar como administrador**.

> [!NOTE]
> Você também pode iniciar uma sessão do Windows PowerShell destinado a um servidor específico clicando duas vezes a um servidor gerenciado em uma página de função ou grupo no Gerenciador do servidor e, em seguida, clicando em **Windows PowerShell**.


## <a name="known-issues"></a>Problemas conhecidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: Instalação de RSAT FOD falha com código de erro 0x800f0954

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018) em ambientes de WSUS/SCCM
> 
> **Resolução**: Para instalar o FODs em um PC ingressado no domínio que recebe atualizações através do WSUS ou SCCM, você precisará alterar uma configuração de diretiva de grupo para habilitar o download FODs diretamente do Windows Update ou um compartilhamento local. Para obter mais detalhes e instruções sobre como alterar essa configuração, consulte [como criar recursos na demanda e pacotes de idiomas disponíveis quando você estiver usando o WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: Instalação de RSAT FOD por meio do aplicativo de configurações não mostra o andamento do status

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Para ver o progresso da instalação, clique o **volta** botão para exibir o status de **gerenciar recursos opcionais** página.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: Desinstalação do RSAT FOD por meio do aplicativo de configurações pode falhar

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Em alguns casos, as falhas de desinstalação são devido à necessidade de desinstalar manualmente as dependências. Especificamente, se A ferramenta de RSAT for necessário pela ferramenta RSAT B, escolhendo desinstalar RSAT ferramenta um falhará se a ferramenta RSAT B ainda está instalada. Nesse caso, desinstale primeiro a ferramenta RSAT B e, em seguida, desinstale A. ferramenta RSAT Consulte a lista de FODs RSAT incluindo dependências.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: Desinstalação do RSAT FOD parece ser bem-sucedida, mas a ferramenta ainda está instalada

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Reiniciar o computador será concluída a remoção da ferramenta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: RSAT ausente após atualização do Windows 10

> **Impacto**: Qualquer RSAT. Instalação do pacote MSU (antes do RSAT FODs) reinstalada automaticamente
> 
> **Resolução**: Uma instalação de RSAT não pode ser persistentes entre as atualizações de sistema operacional devido a RSAT. MSU sendo entregue como um pacote de atualização do Windows. Instale o RSAT novamente depois de atualizar o Windows 10. Observe que essa limitação é um dos motivos por que mudamos para FODs começando com o Windows 10 1809. FODs RSAT instaladas persistirá em futuras atualizações de versão do Windows 10.

## <a name="see-also"></a>Consulte também
>- [Ferramentas de administração de servidor remoto para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Ferramentas de administração de servidor remoto (RSAT) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
