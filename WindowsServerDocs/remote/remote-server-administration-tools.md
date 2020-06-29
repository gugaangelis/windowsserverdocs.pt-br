---
title: Ferramentas de Administração de Servidor Remoto
description: Tópico de nível superior das Ferramentas de Administração de Servidor Remoto
ms.prod: windows-server
ms.technology: manage-rsat
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 69b31c8ef0ce093604ee9fd8fe382d75f7f88595
ms.sourcegitcommit: aeefdf7814a4672b2dcd7537204205bb7ee5f9a0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84514906"
---
# <a name="remote-server-administration-tools"></a>Ferramentas de Administração de Servidor Remoto

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico dá suporte às Ferramentas de Administração de Servidor Remoto para Windows 10.

> [!IMPORTANT]
> Começando na atualização do Windows 10 de outubro de 2018, as Ferramentas de Administração de Servidor Remoto estão incluídas como um conjunto de **Recursos Sob Demanda** no próprio Windows 10. Confira **Quando usar qual versão das Ferramentas de Administração de Servidor Remoto** abaixo para obter instruções de instalação.

A Ferramentas de Administração de Servidor Remoto permite que os administradores de TI gerenciem funções e recursos do Windows Server de um PC com Windows 10.

As Ferramentas de Administração de Servidor Remoto incluem o Gerenciador do Servidor, snap-ins do MMC (Console de Gerenciamento Microsoft), consoles, os cmdlets do Windows PowerShell e provedores e algumas ferramentas de linha de comando para gerenciamento de funções e recursos executados no Windows Server.

As Ferramentas de Administração de Servidor Remoto incluem módulos de cmdlet do Windows PowerShell que podem ser usados para gerenciar funções e recursos que estejam em execução em servidores remotos. Embora o gerenciamento remoto do Windows PowerShell seja habilitado por padrão no Windows Server 2016, ele não é habilitado por padrão no Windows 10. Para executar os cmdlets que fazem parte das Ferramentas de Administração de Servidor Remoto em um servidor remoto, execute `Enable-PSremoting` em uma sessão do Windows PowerShell aberta com direitos elevados de usuário (ou seja, executar como administrador) no computador cliente Windows depois de instalar as Ferramentas de Administração de Servidor Remoto.

## <a name="remote-server-administration-tools-for-windows-10"></a><a name="BKMK_Thresh"></a>Ferramentas de Administração de Servidor Remoto para Windows 10
Use as Ferramentas de Administração de Servidor Remoto para Windows 10 para gerenciar tecnologias específicas em computadores que usam o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 e, em casos limitados, Windows Server 2012 ou Windows Server 2008 R2.

As Ferramentas de Administração de Servidor Remoto para Windows 10 incluem a compatibilidade com o gerenciamento remoto de computadores que estão executando a opção de instalação Server Core ou a configuração de Interface de Servidor Mínima do Windows Server 2016, do Windows Server 2012 R2 e, em casos limitados, as opções de instalação Server Core do Windows Server 2012. Entretanto, as Ferramentas de Administração de Servidor Remoto para Windows 10 não podem ser instaladas em nenhuma versão do sistema operacional Windows Server.

### <a name="tools-available-in-this-release"></a>Ferramentas disponíveis nesta versão
Para obter uma lista das ferramentas disponíveis nas Ferramentas de Administração de Servidor Remoto para Windows 10, confira a tabela em [RSAT (Ferramentas de Administração de Servidor Remoto) para sistemas operacionais Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos de sistema
As Ferramentas de Administração de Servidor Remoto para Windows 10 só podem ser instaladas em computadores que estejam usando o Windows 10. As Ferramentas de Administração de Servidor Remoto não podem ser instaladas em computadores que estão usando o Windows RT 8.1 ou em outros dispositivos de sistema-em-um-chip.

As Ferramentas de Administração de Servidor Remoto para Windows 10 são podem ser usadas em edições baseadas em x86 e x64 do Windows 10.

> [!IMPORTANT]
> As Ferramentas de Administração de Servidor Remoto para Windows 10 não devem ser instaladas em um computador que execute pacotes de ferramentas de administração para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Remova todas as versões anteriores do Pacote de Ferramentas de Administração ou das Ferramentas de Administração de Servidor Remoto do computador, inclusive as versões de pré-lançamento anteriores e as versões das ferramentas para diferentes idiomas ou localidades antes de instalar as Ferramentas de Administração de Servidor Remoto para Windows 10.

Para usar essa versão do Gerenciador do Servidor para acessar e gerenciar servidores remotos que estão usando o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2, você deve instalar várias atualizações para tornar os antigos sistemas operacionais Windows Server gerenciáveis usando o Gerenciador do Servidor. Para obter informações detalhadas sobre como preparar o Windows Server 2012 R2, o Windows Server 2012 e o Windows Server 2008 R2 para gerenciamento usando o Gerenciador do Servidor nas Ferramentas de Administração de Servidor Remoto para Windows 10, confira [Gerenciar vários servidores remotos com o Gerenciador do Servidor](https://technet.microsoft.com/library/hh831456.aspx).
        
O gerenciamento remoto do Windows PowerShell e do Gerenciador do Servidor deve ser habilitado em servidores remotos para gerenciá-los usando as ferramentas que fazem parte das Ferramentas de Administração de Servidor Remoto para Windows 10. O gerenciamento remoto é habilitado por padrão em servidores que usam o Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012. Para obter mais informações sobre como habilitar o gerenciamento remoto se ele tiver sido desabilitado, consulte [Gerenciar vários servidores remotos com o Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).
        
## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalar, desinstalar e desligar/ligar Ferramentas de Administração de Servidor Remoto        

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Use o FoD (Recursos Sob Demanda) para instalar ferramentas específicas das Ferramentas de Administração de Servidor Remoto na Atualização de outubro de 2018 para o Windows 10 ou posterior.

Começando na atualização do Windows 10 de outubro de 2018, a Ferramentas de Administração de Servidor Remoto está incluída como um conjunto de **Recursos Sob Demanda** diretamente do Windows 10. Agora, em vez de baixar um pacote das Ferramentas de Administração de Servidor Remoto, você pode apenas ir para **Gerenciar recursos opcionais** em **Configurações** e clicar em **Adicionar um recurso** para ver a lista de Ferramentas de Administração de Servidor Remoto disponíveis. Selecione e instale as Ferramentas de Administração de Servidor Remoto específicas de que você precisa. Para ver o progresso da instalação, clique no botão **Voltar** para exibir o status na página **Gerenciar recursos opcionais**.
        
Confira a [lista de Ferramentas de Administração de Servidor Remoto disponíveis por meio do **Recursos Sob Demanda**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além de instalar por meio do aplicativo gráfico de **Configurações**, você também pode instalar Ferramentas de Administração de Servidor Remoto específicas por meio de linha de comando ou automação usando [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Um benefício dos Recursos Sob Demanda é que os recursos instalados persistem entre atualizações de versão do Windows 10.        
        
#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar ferramentas específicas das Ferramentas de Administração de Servidor Remoto na Atualização de outubro de 2018 para o Windows 10 ou posterior (após a instalação com o FoD)        

No Windows 10, abra o aplicativo de **Configurações**, vá para **Gerenciar recursos opcionais**, selecione e desinstale as Ferramentas de Administração de Servidor Remoto específicas que você deseja remover. Observe que, em alguns casos, será necessário desinstalar manualmente as dependências. Especificamente, se a ferramenta A das Ferramentas de Administração de Servidor Remoto for exigida pela ferramenta B das Ferramentas de Administração de Servidor Remoto, a escolha de desinstalar a ferramenta A das Ferramentas de Administração de Servidor Remoto falhará se a ferramenta B das Ferramentas de Administração de Servidor Remoto ainda estiver instalada. Nesse caso, desinstale a ferramenta B de Ferramentas de Administração de Servidor Remoto primeiro e, em seguida, desinstale a ferramenta A de Ferramentas de Administração de Servidor Remoto. Observe também que, em alguns casos, a desinstalação de uma ferramenta de Ferramentas de Administração de Servidor Remoto pode parecer ter sucesso, embora a ferramenta ainda esteja instalada. Nesse caso, reiniciar o PC concluirá a remoção da ferramenta.

Confira a [lista de Ferramentas de Administração de Servidor Remoto, incluindo dependências](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Além de desinstalar por meio do aplicativo gráfico de Configurações, você também pode desinstalar Ferramentas de Administração de Servidor Remoto específicas por meio da linha de comando ou automação usando [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usar qual versão das Ferramentas de Administração de Servidor Remoto

Se você tiver uma versão do Windows 10 anterior à atualização de outubro de 2018 (1809), não poderá usar os **Recursos Sob Demanda**. Você precisará baixar e instalar o pacote das Ferramentas de Administração de Servidor Remoto.

- **Instale os FODs das Ferramentas de Administração de Servidor Remoto diretamente do Windows 10, conforme descrito acima**: Ao instalar na Atualização de outubro de 2018 para o Windows 10 (1809) ou posterior, para gerenciar o Windows Server 2019 ou versões anteriores.

- **Baixe e instale o pacote das Ferramentas de Administração de Servidor Remoto WS_1803 conforme descrito abaixo**: Ao instalar na Atualização de abril de 2018 para o Windows 10 (1803) ou anterior, para gerenciar o Windows Server, versão 1803 ou Windows Server, versão 1709.

- **Baixe e instale o pacote WS2016 das Ferramentas de Administração de Servidor Remoto, conforme descrito abaixo**: Ao instalar na Atualização de abril de 2018 para o Windows 10 (1803) ou anterior, para gerenciar o Windows Server 2016 ou versões anteriores.

#### <a name="download-the-rsat-package-to-install-remote-server-administration-tools-for-windows-10"></a><a name="BKMK_installthresh"></a>Baixar o pacote das Ferramentas de Administração de Servidor Remoto para instalar as Ferramentas de Administração de Servidor Remoto para Windows 10

1.  Baixe o pacote de Ferramentas de Administração de Servidor Remoto para Windows 10 do [Centro de Download da Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Você também pode executar o instalador do site do Centro de Download ou salvar o pacote de download em um computador ou compartilhamento local.

    > [!IMPORTANT]
    > Você pode instalar as Ferramentas de Administração de Servidor Remoto para Windows 10 apenas em computadores que estejam usando o Windows 10. As Ferramentas de Administração de Servidor Remoto não podem ser instaladas em computadores que estão usando o Windows RT 8.1 ou em outros dispositivos de sistema-em-um-chip.

2.  Se você salvar o pacote de download em um computador ou compartilhamento local, clique duas vezes no programa instalador, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, dependendo da arquitetura do computador no qual você deseja instalar as ferramentas.

3.  Quando a caixa de diálogo **Instalador Autônomo do Windows Update** perguntar se você deseja instalar a atualização, clique em **Sim**.

4.  Leia e aceite os termos de licença. Clique em **Aceito**.

5.  A instalação requer alguns minutos para ser concluída.    
        
##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Para desinstalar as Ferramentas de Administração de Servidor Remoto para Windows 10 (após a instalação do pacote das Ferramentas de Administração de Servidor Remoto)
        
1. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Painel de Controle**.

2. Em **Programas**, clique em **Desinstalar um programa**.

3. Clique em **Exibir atualizações instaladas**.

4. Clique com o botão direito do mouse em **Atualizar para Microsoft Windows (KB2693643)** e clique em **Desinstalar**.

5. Ao ser perguntado se tem certeza de que deseja desinstalar a atualização, clique em **Sim**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Para desligar ferramentas específicas (após a instalação do pacote das Ferramentas de Administração de Servidor Remoto)
        
6. Na área de trabalho, clique em **Iniciar**, clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Painel de Controle**.

7. Clique em **Programas**; em seguida, em **Programas e Recursos** , clique em **Ativar ou desativar recursos do Windows**.

8. Na caixa de diálogo **Recursos do Windows** , expanda **Ferramentas de Administração de Servidor Remoto**e depois expanda **Ferramentas de Administração de Funções** ou **Ferramentas de Administração de Recursos**.

9. Limpe as caixas de seleção para qualquer ferramenta que você deseja desativar.

   > [!NOTE]
   > Se você desligar o Gerenciador do Servidor, o computador deverá ser reiniciado e as ferramentas que estavam acessíveis pelo menu **Ferramentas** do Gerenciador do Servidor deverão ser abertas pela pasta **Ferramentas Administrativas**.
        
10. Quando terminar de desabilitar as ferramentas que não deseje mais usar, clique em **OK**.

### <a name="run-remote-server-administration-tools"></a>Executar Ferramentas de Administração de Servidor Remoto

> [!NOTE]
> Depois de instalar as Ferramentas de Administração de Servidor Remoto para Windows 10, a pasta **Ferramentas Administrativas** é exibida no menu **Iniciar**. Você pode acessar as ferramentas nos seguintes locais.
>
> -   O menu **Ferramentas** no console do Gerenciador do Servidor.
> -   **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas**.
> -   Um atalho salvo na área de trabalho da pasta **Ferramentas Administrativas** (para fazer isso, clique com o botão direito do mouse no link **Painel de Controle\Sistema e Segurança\Ferramentas Administrativas** e clique em **Criar Atalho**).

As ferramentas instaladas como parte das Ferramentas de Administração de Servidor Remoto para Windows 10 não podem ser usadas para gerenciar o computador cliente local. Independentemente da ferramenta que você execute, é necessário especificar um ou vários servidores remotos nos quais executar a ferramenta. Como a maioria das ferramentas são integradas ao Gerenciador do Servidor, adicione os servidores remotos que você deseja gerenciar ao pool de servidores do Gerenciador do Servidor antes de gerenciar o servidor usando as ferramentas no menu **Ferramentas**. Para obter mais informações sobre como adicionar servidores ao pool de servidores e criar grupos personalizados de servidores, consulte [Adicionar servidores ao Gerenciador do Servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Criar e gerenciar grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

Em Ferramentas de Administração de Servidor Remoto para Windows 10, todas as ferramentas de gerenciamento do servidor baseadas em GUI, como snap-ins do MMC e caixas de diálogo, são acessadas no menu **Ferramentas** do console do Gerenciador do Servidor. Embora o computador que executa as Ferramentas de Administração de Servidor Remoto para Windows 10 execute um sistema operacional baseado no cliente, depois de instalar as ferramentas, o Gerenciador do Servidor, incluído nas Ferramentas de Administração de Servidor Remoto para Windows 10, é aberto automaticamente por padrão no computador cliente. Observe que não há nenhuma página **Servidor Local** no console do Gerenciador do Servidor executado em um computador cliente.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar o Gerenciador do Servidor em um computador cliente

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**e, em seguida, clique em **Ferramentas Administrativas**.

2.  Na pasta **Ferramentas Administrativas** , clique em **Gerenciador do Servidor**.

Embora eles não estejam listados no menu **Ferramentas** do console do Gerenciador do Servidor, as ferramentas de gerenciamento do prompt de comando e os cmdlets do Windows PowerShell também são instalados para funções e recursos como parte das Ferramentas de Administração de Servidor Remoto. Por exemplo, se você abrir uma sessão do Windows PowerShell com direitos do usuário elevados (Executar como Administrador) e executar o cmdlet `Get-Command -Module RDManagement`, os resultados incluirão uma lista de cmdlets de Serviços de Área de Trabalho Remota que agora estão disponíveis para execução no computador local após a instalação das Ferramentas de Administração de Servidor Remoto, desde que os cmdlets sejam direcionados a um servidor remoto que esteja executando toda ou parte da função Serviços de Área de Trabalho Remota.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar o Windows PowerShell com direitos de usuário elevados (Executar como administrador)

1.  No menu **Iniciar** , clique em **Todos os Aplicativos**, clique em **Sistema Windows**e, em seguida, clique em **Windows PowerShell**.

2.  Para executar o Windows PowerShell como administrador na área de trabalho, clique com o botão direito do mouse no atalho **Windows PowerShell** e clique em **Executar como Administrador**.

> [!NOTE]
> Você também pode iniciar uma sessão do Windows PowerShell direcionada a um servidor específico clicando com o botão direito do mouse em um servidor gerenciado em uma página de função ou grupo no Gerenciador do Servidor e em **Windows PowerShell**.
        

## <a name="known-issues"></a>Problemas conhecidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: A instalação do FOD das Ferramentas de Administração de Servidor Remoto falha com o código de erro 0x800f0954

> **Impacto**: Os FODs das Ferramentas de Administração de Servidor Remoto no Windows 10 1809 (atualização de outubro de 2018) em ambientes WSUS/Configuration Manager
> 
> **Resolução**: Para instalar os FODs em um computador ingressado no domínio que recebe atualizações por meio do WSUS ou do Configuration Manager, você precisará alterar uma configuração da Política de Grupo para habilitar o download dos FODs diretamente do Windows Update ou de um compartilhamento local. Para obter mais detalhes e instruções sobre como alterar essa configuração, confira [Como disponibilizar os Recursos Sob Demanda e os pacotes de idiomas quando você estiver usando o WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: Instalação do FOD das Ferramentas de Administração de Servidor Remoto por meio do aplicativo Configurações não exibe o status/progresso

> **Impacto**: FODs das Ferramentas de Administração de Servidor Remoto no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Para ver o progresso da instalação, clique no botão **Voltar** para exibir o status na página **Gerenciar recursos opcionais**.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: A desinstalação do FOD das Ferramentas de Administração de Servidor Remoto por meio do aplicativo Configurações pode falhar

> **Impacto**: FODs das Ferramentas de Administração de Servidor Remoto no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Em alguns casos, as falhas de desinstalação são devido à necessidade de desinstalar manualmente as dependências. Especificamente, se a ferramenta A das Ferramentas de Administração de Servidor Remoto for exigida pela ferramenta B das Ferramentas de Administração de Servidor Remoto, a escolha de desinstalar a ferramenta A das Ferramentas de Administração de Servidor Remoto falhará se a ferramenta B das Ferramentas de Administração de Servidor Remoto ainda estiver instalada. Nesse caso, desinstale a ferramenta B de Ferramentas de Administração de Servidor Remoto primeiro e, depois, desinstale a ferramenta A de Ferramentas de Administração de Servidor Remoto. Confira a lista de FODs das Ferramentas de Administração de Servidor Remoto que incluem dependências.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: A desinstalação do FOD das Ferramentas de Administração de Servidor Remoto parece ter tido êxito, mas a ferramenta ainda está instalada

> **Impacto**: FODs das Ferramentas de Administração de Servidor Remoto no Windows 10 1809 (atualização de outubro de 2018)
> 
> **Resolução**: Reiniciar o PC concluirá a remoção da ferramenta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: Ferramentas de Administração de Servidor Remoto ausentes após a atualização do Windows 10

> **Impacto**: Algum pacote de instalação .MSU das Ferramentas de Administração de Servidor Remoto (antes dos FODs das Ferramentas de Administração de Servidor Remoto) não é reinstalado automaticamente
> 
> **Resolução**: Uma instalação das Ferramentas de Administração de Servidor Remoto não pode ser persistida nas atualizações do sistema operacional devido ao .MSU das Ferramentas de Administração de Servidor Remoto ser entregue como um pacote do Windows Update. Instale as Ferramentas de Administração de Servidor Remoto novamente após a atualização do Windows 10. Observe que essa limitação é um dos motivos pelos quais mudamos para os FODs do Windows 10 1809 em diante. Os FODs das Ferramentas de Administração de Servidor Remoto que estão instalados persistirão em atualizações de versão futuras do Windows 10.

## <a name="see-also"></a>Consulte Também
>- [Ferramentas de Administração de Servidor Remoto para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [RSAT (Ferramentas de Administração de Servidor Remoto) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055) '                                                                                                                                                                                                                                                                                                                                                                                    
