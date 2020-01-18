---
title: Ferramentas de Administração de Servidor Remoto
description: Tópico de nível superior para Ferramentas de Administração de Servidor Remoto
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: e8e8206531e0939a1b6d6dfd17f5c5dd59947c81
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259131"
---
# <a name="remote-server-administration-tools"></a>Ferramentas de Administração de Servidor Remoto

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico oferece suporte a Ferramentas de Administração de Servidor Remoto para Windows 10.

> [!IMPORTANT]
> A partir da atualização do Windows 10 de outubro de 2018, o RSAT é incluído como um conjunto de **recursos sob demanda** no próprio Windows 10. Confira **quando usar qual versão do RSAT** abaixo para obter instruções de instalação.

O RSAT permite que os administradores de ti gerenciem funções e recursos do Windows Server de um PC com Windows 10.

O Ferramentas de Administração de Servidor Remoto inclui Gerenciador do Servidor, snap-ins do MMC (console de gerenciamento Microsoft), consoles do, cmdlets e provedores do Windows PowerShell e algumas ferramentas de linha de comando para gerenciar funções e recursos que são executados no Windows Server.

Ferramentas de Administração de Servidor Remoto inclui módulos de cmdlet do Windows PowerShell que podem ser usados para gerenciar funções e recursos que estão sendo executados em servidores remotos. Embora o gerenciamento remoto do Windows PowerShell esteja habilitado por padrão no Windows Server 2016, ele não é habilitado por padrão no Windows 10. Para executar os cmdlets que fazem parte do Ferramentas de Administração de Servidor Remoto em um servidor remoto, execute `Enable-PSremoting` em uma sessão do Windows PowerShell que tenha sido aberta com direitos de usuário elevados (ou seja, executar como administrador) no computador cliente Windows depois de instalar o Ferramentas de Administração de Servidor Remoto.

## <a name="BKMK_Thresh"></a>Ferramentas de Administração de Servidor Remoto para Windows 10
Use Ferramentas de Administração de Servidor Remoto para o Windows 10 para gerenciar tecnologias específicas em computadores que executam o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 e, em casos limitados, Windows Server 2012 ou Windows Server 2008 R2.

O Ferramentas de Administração de Servidor Remoto para Windows 10 inclui suporte para o gerenciamento remoto de computadores que executam a opção de instalação Server Core ou a configuração mínima da interface do servidor do Windows Server 2016, do Windows Server 2012 R2 e em limitada casos, as opções de instalação Server Core do Windows Server 2012. No entanto, Ferramentas de Administração de Servidor Remoto para Windows 10 não podem ser instaladas em nenhuma versão do sistema operacional Windows Server.

### <a name="tools-available-in-this-release"></a>Ferramentas disponíveis nesta versão
Para obter uma lista das ferramentas disponíveis no Ferramentas de Administração de Servidor Remoto para Windows 10, consulte a tabela em [ferramentas de administração de servidor remoto (RSAT) para sistemas operacionais Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos de sistema
Ferramentas de Administração de Servidor Remoto para Windows 10 pode ser instalado somente em computadores que executam o Windows 10. O Ferramentas de Administração de Servidor Remoto não pode ser instalado em computadores que executam o Windows RT 8,1 ou outros dispositivos do sistema em chip.

O Ferramentas de Administração de Servidor Remoto para Windows 10 é executado nas edições baseadas em x86 e x64 do Windows 10.

> [!IMPORTANT]
> O Ferramentas de Administração de Servidor Remoto para Windows 10 não deve ser instalado em um computador que esteja executando pacotes de ferramentas de administração para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Remova todas as versões mais antigas do pacote de ferramentas de administração ou Ferramentas de Administração de Servidor Remoto, incluindo versões anteriores de pré-lançamento e versões das ferramentas para diferentes idiomas ou localidades do computador antes de instalar a administração do servidor remoto Ferramentas para Windows 10.

Para usar esta versão do Gerenciador do Servidor para acessar e gerenciar servidores remotos que executam o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2, você deve instalar várias atualizações para tornar os sistemas operacionais Windows Server mais antigos gerenciáveis usando o se Gerenciador de servido. Para obter informações detalhadas sobre como preparar o Windows Server 2012 R2, o Windows Server 2012 e o Windows Server 2008 R2 para gerenciamento usando Gerenciador do Servidor no Ferramentas de Administração de Servidor Remoto para Windows 10, consulte [gerenciar vários servidores remotos com o Gerenciador do servidor](https://technet.microsoft.com/library/hh831456.aspx).

O Windows PowerShell e o gerenciamento remoto do Gerenciador do Servidor devem ser habilitados em servidores remotos para gerenciá-los usando ferramentas que fazem parte do Ferramentas de Administração de Servidor Remoto para Windows 10. O gerenciamento remoto é habilitado por padrão em servidores que executam o Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012. Para obter mais informações sobre como habilitar o gerenciamento remoto se ele tiver sido desabilitado, consulte [Gerenciar vários servidores remotos com o Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalar, desinstalar e desativar/em ferramentas do RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Usar os recursos sob demanda (FoD) para instalar ferramentas do RSAT específicas no Windows 10 de outubro de 2018 atualização ou posterior

A partir da atualização do Windows 10 de outubro de 2018, o RSAT está incluído como um conjunto de **recursos sob demanda** diretamente do Windows 10. Agora, em vez de baixar um pacote do RSAT, você pode ir apenas para **gerenciar recursos opcionais** em **configurações** e clicar em **Adicionar um recurso** para ver a lista de ferramentas do RSAT disponíveis. Selecione e instale as ferramentas do RSAT específicas de que você precisa. Para ver o progresso da instalação, clique no botão **voltar** para exibir o status na página **gerenciar recursos opcionais** .

Consulte a [lista de ferramentas do RSAT disponíveis por meio **de recursos sob demanda**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além de instalar o por meio do aplicativo de **configurações** gráficas, você também pode instalar ferramentas do RSAT específicas por meio de linha de comando ou automação usando o [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Um benefício dos recursos sob demanda é que os recursos instalados persistem entre atualizações de versão do Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar as ferramentas do RSAT específicas no Windows 10 de outubro de 2018 atualização ou posterior (após a instalação com FoD)

No Windows 10, abra o aplicativo **configurações** , vá para **gerenciar recursos opcionais**, selecione e desinstale as ferramentas do RSAT específicas que você deseja remover. Observe que, em alguns casos, será necessário desinstalar manualmente as dependências. Especificamente, se a ferramenta RSAT A for necessária pela ferramenta RSAT B, a escolha de desinstalar a ferramenta RSAT A falhará se a ferramenta do RSAT B ainda estiver instalada. Nesse caso, desinstale A ferramenta B do RSAT primeiro e, em seguida, desinstale A ferramenta do RSAT A. Observe também que, em alguns casos, a desinstalação de uma ferramenta RSAT pode parecer ter sucesso, embora a ferramenta ainda esteja instalada. Nesse caso, reiniciar o PC concluirá a remoção da ferramenta.

Consulte a [lista de ferramentas do RSAT, incluindo dependências](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além de desinstalar o por meio do aplicativo de configurações gráficas, você também pode desinstalar ferramentas do RSAT específicas por meio de linha de comando ou automação usando o [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usar qual versão do RSAT

Se você tiver uma versão do Windows 10 anterior à atualização de outubro de 2018 (1809), não poderá usar **recursos sob demanda**. Será necessário baixar e instalar o pacote do RSAT.

- **Instale o RSAT FODs diretamente do Windows 10, conforme descrito acima**: ao instalar no Windows 10 de outubro de 2018 atualização (1809) ou posterior, para gerenciar o windows Server 2019 ou versões anteriores.

- **Baixe e instale o pacote WS_1803 RSAT, conforme descrito abaixo**: ao instalar o Windows 10 abril de 2018 atualização (1803) ou anterior, para gerenciar o Windows Server, versão 1803 ou Windows Server, versão 1709.

- **Baixe e instale o pacote do WS2016 RSAT, conforme descrito abaixo**: ao instalar na atualização do Windows 10 de abril de 2018 (1803) ou anterior, para gerenciar o windows Server 2016 ou versões anteriores.

#### <a name="BKMK_installthresh"></a>Baixe o pacote do RSAT para instalar o Ferramentas de Administração de Servidor Remoto para Windows 10

1.  Baixe o pacote Ferramentas de Administração de Servidor Remoto para Windows 10 do [centro de download da Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Você também pode executar o instalador do site do Centro de Download ou salvar o pacote de download em um computador ou compartilhamento local.

    > [!IMPORTANT]
    > Você só pode instalar Ferramentas de Administração de Servidor Remoto para Windows 10 em computadores que executam o Windows 10. O Ferramentas de Administração de Servidor Remoto não pode ser instalado em computadores que executam o Windows RT 8,1 ou outros dispositivos do sistema em chip.

2.  Se você salvar o pacote de download em um computador ou compartilhamento local, clique duas vezes no programa instalador, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, dependendo da arquitetura do computador no qual você deseja instalar as ferramentas.

3.  Quando a caixa de diálogo **Instalador Autônomo do Windows Update** perguntar se você deseja instalar a atualização, clique em **Sim**.

4.  Leia e aceite os termos de licença. Clique em **Aceito**.

5.  A instalação requer alguns minutos para ser concluída.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Para desinstalar o Ferramentas de Administração de Servidor Remoto para Windows 10 (após a instalação do pacote do RSAT)

1. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Painel de Controle**.

2. Em **Programas**, clique em **Desinstalar um programa**.

3. Clique em **Exibir atualizações instaladas**.

4. Clique com o botão direito do mouse em **Atualizar para Microsoft Windows (KB2693643)** e clique em **Desinstalar**.

5. Ao ser perguntado se tem certeza de que deseja desinstalar a atualização, clique em **Sim**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Para desativar ferramentas específicas (após a instalação do pacote do RSAT)

6. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Painel de Controle**.

7. Clique em **Programas**; em seguida, em **Programas e Recursos** , clique em **Ativar ou desativar recursos do Windows**.

8. Na caixa de diálogo **Recursos do Windows** , expanda **Ferramentas de Administração de Servidor Remoto**e depois expanda **Ferramentas de Administração de Funções** ou **Ferramentas de Administração de Recursos**.

9. Limpe as caixas de seleção para qualquer ferramenta que você deseja desativar.

   > [!NOTE]
   > Se você desativar Gerenciador do Servidor, o computador deverá ser reiniciado e as ferramentas que estavam acessíveis no menu **ferramentas** do Gerenciador do servidor deverão ser abertas na pasta **Ferramentas administrativas** .

10. Quando terminar de desabilitar as ferramentas que não deseje mais usar, clique em **OK**.

### <a name="run-remote-server-administration-tools"></a>Executar Ferramentas de Administração de Servidor Remoto

> [!NOTE]
> Depois de instalar o Ferramentas de Administração de Servidor Remoto para Windows 10, a pasta **Ferramentas administrativas** é exibida no menu **Iniciar** . Você pode acessar as ferramentas nos seguintes locais.
>
> -   O menu **ferramentas** no console do Gerenciador do servidor.
> -   **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas**.
> -   Um atalho salvo na área de trabalho da pasta **Ferramentas Administrativas** (para fazer isso, clique com o botão direito do mouse no link **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas** e clique em **Criar Atalho**).

As ferramentas instaladas como parte do Ferramentas de Administração de Servidor Remoto para Windows 10 não podem ser usadas para gerenciar o computador cliente local. Independentemente da ferramenta executada, você deve especificar um servidor remoto ou vários servidores remotos, nos quais executar a ferramenta. Como a maioria das ferramentas está integrada com o Gerenciador do Servidor, você adiciona servidores remotos que deseja gerenciar ao pool de servidores do Gerenciador do Servidor antes de gerenciar o servidor usando as ferramentas no menu **ferramentas** . Para obter mais informações sobre como adicionar servidores ao pool de servidores e criar grupos personalizados de servidores, consulte [Adicionar servidores ao Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Criar e gerenciar grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

No Ferramentas de Administração de Servidor Remoto para Windows 10, todas as ferramentas de gerenciamento de servidor baseadas em GUI, como snap-ins do MMC e caixas de diálogo, são acessadas no menu **ferramentas** do console do Gerenciador do servidor. Embora o computador que executa o Ferramentas de Administração de Servidor Remoto para Windows 10 execute um sistema operacional baseado em cliente, depois de instalar as ferramentas, Gerenciador do Servidor, incluído com o Ferramentas de Administração de Servidor Remoto para Windows 10, é aberto automaticamente por padrão no computador cliente. Observe que não há nenhuma página de **servidor local** no console do Gerenciador do servidor que é executado em um computador cliente.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar o Gerenciador do Servidor em um computador cliente

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**e, em seguida, clique em **Ferramentas Administrativas**.

2.  Na pasta **Ferramentas Administrativas** , clique em **Gerenciador do Servidor**.

Embora eles não estejam listados no menu **ferramentas** do console Gerenciador do servidor, os cmdlets do Windows PowerShell e as ferramentas de gerenciamento de prompt de comando também são instalados para funções e recursos como parte do ferramentas de administração de servidor remoto. Por exemplo, se você abrir uma sessão do Windows PowerShell com direitos de usuário elevados (executar como administrador) e executar o cmdlet `Get-Command -Module RDManagement`, os resultados incluirão uma lista de cmdlets de serviços de área de trabalho remota que agora estão disponíveis para execução no computador local após a instalação do Ferramentas de Administração de Servidor Remoto, desde que os cmdlets sejam direcionados a um servidor remoto que esteja executando toda ou parte da função de serviços

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar o Windows PowerShell com direitos de usuário elevados (Executar como administrador)

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Windows PowerShell**.

2.  Para executar o Windows PowerShell como administrador na área de trabalho, clique com o botão direito do mouse no atalho do **Windows PowerShell** e clique em **Executar como administrador**.

> [!NOTE]
> Você também pode iniciar uma sessão do Windows PowerShell que é destinada a um servidor específico clicando com o botão direito do mouse em um servidor gerenciado em uma página de função ou grupo no Gerenciador do Servidor e, em seguida, clicando em **Windows PowerShell**.


## <a name="known-issues"></a>Problemas conhecidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: a instalação do RSAT fod falha com o código de erro 0x800f0954

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018) em ambientes WSUS/SCCM
> 
> **Resolução**: para instalar o FODs em um computador ingressado em domínio que recebe atualizações por meio do WSUS ou SCCM, você precisará alterar uma configuração de política de grupo para habilitar o download do FODs diretamente do Windows Update ou de um compartilhamento local. Para obter mais detalhes e instruções sobre como alterar essa configuração, consulte [como tornar recursos sob demanda e pacotes de idiomas disponíveis quando você estiver usando o WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: a instalação do RSAT fod por meio de configurações do aplicativo não mostra o status/progresso

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: para ver o progresso da instalação, clique no botão **voltar** para exibir o status na página **gerenciar recursos opcionais** .

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: a desinstalação do RSAT fod por meio de configurações do aplicativo pode falhar

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: em alguns casos, as falhas de desinstalação são devido à necessidade de desinstalar manualmente as dependências. Especificamente, se a ferramenta RSAT A for necessária pela ferramenta RSAT B, a escolha de desinstalar a ferramenta RSAT A falhará se a ferramenta do RSAT B ainda estiver instalada. Nesse caso, desinstale A ferramenta B do RSAT primeiro e, em seguida, desinstale A ferramenta do RSAT A. Consulte a lista de FODs do RSAT, incluindo dependências.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: a desinstalação do RSAT fod parece ter sucesso, mas a ferramenta ainda está instalada

> **Impacto**: RSAT FODs no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: reiniciar o PC concluirá a remoção da ferramenta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: RSAT ausente após a atualização do Windows 10

> **Impacto**: qualquer RSAT. Instalação do pacote MSU (antes do RSAT FODs) não reinstalado automaticamente
> 
> **Resolução**: uma instalação do RSAT não pode ser persistida nas atualizações do sistema operacional devido ao RSAT. MSU sendo entregue como um pacote de Windows Update. Instale o RSAT novamente após a atualização do Windows 10. Observe que essa limitação é um dos motivos pelos quais mudamos para FODs a partir do Windows 10 1809. O RSAT FODs, que estão instalados, persistirá em atualizações futuras de versão do Windows 10.

## <a name="see-also"></a>Veja também
>- [Ferramentas de Administração de Servidor Remoto para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Ferramentas de Administração de Servidor Remoto (RSAT) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
