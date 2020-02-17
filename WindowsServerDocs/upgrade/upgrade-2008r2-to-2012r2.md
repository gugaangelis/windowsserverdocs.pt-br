---
title: Atualizar o Windows Server 2008 R2 para o Windows Server 2012 R2 | Microsoft Docs
description: Saiba como executar uma atualização in-loco para passar do Windows Server 2008 R2 para o Windows Server 2012 R2.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: d5051239f7269eb4b6361187121ac960e06f6d9e
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125076"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>Atualizar o Windows Server 2008 R2 para o Windows Server 2012 R2

Se você desejar manter o mesmo hardware e todas as funções de servidor que já configurou sem mesclar o servidor, faça uma atualização in-loco. Uma atualização in-loco permite ir de um sistema operacional antigo para um mais recente, mantendo suas configurações, funções de servidor e dados intactos. Este artigo ajuda você a migrar do Windows Server 2008 R2 para o Windows Server 2012 R2.

Para atualizar para o Windows Server 2019, use este tópico primeiro para atualizar para o Windows Server 2012 R2 e, em seguida, [atualizar do Windows Server 2012 R2 para o Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de começar sua atualização in-loco

Antes de iniciar a atualização do Windows Server, recomendamos coletar algumas informações de seus dispositivos para fins de diagnóstico e solução de problemas. Como essas informações deverão ser usadas somente se a atualização falhar, armazene-as em algum lugar em que possa acessá-las do seu dispositivo.

### <a name="to-collect-your-info"></a>Para coletar suas informações

1. Abra um prompt de comando, acesse `c:\Windows\system32` e digite **systeminfo.exe**.

2. Copie, cole e armazene as informações do sistema resultantes em algum lugar do seu dispositivo.

3. Digite **ipconfig /all** no prompt de comando e, em seguida, copie e cole as informações de configuração resultantes na mesma localização acima.

4. Abra o editor do Registro, vá para o hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e copie e cole o Windows Server **BuildLabEx** (versão) e **EditionID** (edição) na mesma localização acima.

Depois de coletar todas as suas informações relacionadas ao Windows Server, é altamente recomendável fazer um backup do sistema operacional, dos aplicativos e das máquinas virtuais. Você também deve **Desligar**, realizar uma **Migração rápida** ou **Migrar em tempo real** as máquinas virtuais em execução no momento no servidor. Você não pode ter máquinas virtuais em execução durante a atualização in-loco.

## <a name="to-perform-the-upgrade"></a>Para executar a atualização

1. Verifique se o valor **BuildLabEx** informa que você está executando o Windows Server 2008 R2.

2. Localize a mídia de instalação do Windows Server 2012 R2 e, em seguida, selecione **setup.exe**.

    ![Windows Explorer mostrando o arquivo setup.exe](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. Selecione **Sim** para iniciar o processo de instalação.

    ![Controle de Conta de Usuário solicitando permissão para iniciar a instalação](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. Na tela do Windows Server 2012 R2, selecione **Instalar agora**.

5. Para dispositivos conectados à Internet, selecione **Ficar online para instalar atualizações agora (recomendado)** .

    ![Tela para escolher ficar online para obter atualizações importantes do Windows](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. Selecione a edição do Windows Server 2012 R2 que você deseja instalar e selecione **Avançar**.

    ![Tela para escolher o Windows Server 2012 R2 Edition a ser instalado](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. Selecione **Eu aceito os termos de licença** para aceitar os termos do seu contrato de licenciamento com base no seu canal de distribuição (como Varejo, Licença de Volume, OEM, ODM e assim por diante), então selecione **Avançar**.

    ![Tela para aceitar seu contrato de licença](media/upgrade-2008r2-2012r2/license-terms.png)

8. Selecione **Atualizar: instale o Windows e mantenha arquivos, configurações e aplicativos** para optar por fazer uma atualização in-loco.

    ![Tela para escolher seu tipo de instalação](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. A instalação lembra você de verificar se os aplicativos são compatíveis com o Windows Server 2012 R2 usando as informações no artigo [Instalação e atualização do Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade) e, em seguida, selecionar **Avançar**.

    ![Tela lembrando você de verificar a compatibilidade de seu aplicativo](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. Se você vir uma página que informa que a atualização não é recomendada, ignore-a e selecione **Confirmar**. Foi colocado em vigor para solicitar instalações limpas, mas não é obrigatório.

    ![Tela mostrando o progresso da atualização](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    A atualização in-loco é iniciada, mostrando a tela **Atualizando o Windows** com o progresso. Após a conclusão da atualização, o servidor será reiniciado.

## <a name="after-your-upgrade-is-done"></a>Após a conclusão da atualização

Após a conclusão da atualização, você deverá verificar se a atualização para o Windows Server 2012 R2 foi bem-sucedida.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para verificar se a atualização foi bem-sucedida

1. Abra o Editor do Registro, acesse o hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e veja o **ProductName**. Você deverá ver sua edição do Windows Server 2012 R2, por exemplo **Windows Server 2012 R2 Datacenter**.

2. Verifique se todos os aplicativos estão em execução e se as conexões do cliente com os aplicativos foram bem-sucedidas.

Se acreditar que possa ter ocorrido um erro durante a atualização, copie e compacte o diretório `%SystemRoot%\Panther` (geralmente `C:\Windows\Panther`) e entre em contato com o Suporte da Microsoft.

## <a name="next-steps"></a>Próximas etapas

Você pode executar mais uma atualização para ir do Windows Server 2012 R2 para o Windows Server 2019. Para obter instruções detalhadas, confira [Atualizar o Windows Server 2012 R2 para o Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="related-articles"></a>Artigos relacionados

- Para obter mais detalhes e informações sobre o Windows Server 2012 R2, confira [Windows Server 2012 R2 e Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11)).
