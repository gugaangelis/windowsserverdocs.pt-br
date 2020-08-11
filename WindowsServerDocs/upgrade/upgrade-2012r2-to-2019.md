---
title: Atualizar o Windows Server 2012 R2 para o Windows Server 2019 | Microsoft Docs
description: Saiba como executar uma atualização in-loco para passar do Windows Server 2012 R2 para o Windows Server 2019.
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: c23c04682fe796a5d76f487b5ed6d91e81ac3ad1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995780"
---
# <a name="upgrade-windows-server-2012-r2-to-windows-server-2019"></a>Atualizar o Windows Server 2012 R2 para o Windows Server 2019

Se você desejar manter o mesmo hardware e todas as funções de servidor que já configurou sem mesclar o servidor, faça uma atualização in-loco. Uma atualização in-loco permite ir de um sistema operacional antigo para um mais recente, mantendo suas configurações, funções de servidor e dados intactos. Este artigo ajuda você a migrar do Windows Server 2012 R2 para o Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de começar sua atualização in-loco

Antes de iniciar a atualização do Windows Server, recomendamos coletar algumas informações de seus dispositivos para fins de diagnóstico e solução de problemas. Como essas informações deverão ser usadas somente se a atualização falhar, armazene-as em algum lugar em que possa acessá-las do seu dispositivo.

### <a name="to-collect-your-info"></a>Para coletar suas informações

1. Abra um prompt de comando, acesse `c:\Windows\system32` e digite **systeminfo.exe**.

2. Copie, cole e armazene as informações do sistema resultantes em algum lugar do seu dispositivo.

3. Digite **ipconfig /all** no prompt de comando e, em seguida, copie e cole as informações de configuração resultantes na mesma localização acima.

4. Abra o editor do Registro, vá para o hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e copie e cole o Windows Server **BuildLabEx** (versão) e **EditionID** (edição) na mesma localização acima.

Depois de coletar todas as suas informações relacionadas ao Windows Server, é altamente recomendável fazer um backup do sistema operacional, dos aplicativos e das máquinas virtuais. Você também deve **Desligar**, realizar uma **Migração rápida** ou **Migrar em tempo real** as máquinas virtuais em execução no momento no servidor. Você não pode ter máquinas virtuais em execução durante a atualização in-loco.

## <a name="to-perform-the-upgrade"></a>Para executar a atualização

1. Verifique se o valor **BuildLabEx** informa que você está executando o Windows Server 2012 R2.

2. Localize a mídia de instalação do Windows Server 2019 e, em seguida, selecione **setup.exe**.

    ![Windows Explorer mostrando o arquivo setup.exe](media/upgrade-2012r2-2019/setup-2019.png)

3. Selecione **Sim** para iniciar o processo de instalação.

    ![Controle de Conta de Usuário solicitando permissão para iniciar a instalação](media/upgrade-2012r2-2019/start-setup-uac-box.png)

4. Para dispositivos conectados à Internet, marque a opção **Baixar atualizações, drivers e recursos opcionais (recomendado)** e, em seguida, selecione **Avançar**.

    ![Tela para escolher ficar online para obter atualizações importantes do Windows](media/upgrade-2012r2-2019/online-updates-win-setup.png)

5. A instalação verifica a configuração do dispositivo, você deve aguardar até que ela seja concluída e, em seguida, selecionar **Avançar**.

6. Dependendo do canal de distribuição do qual você recebeu a mídia do Windows Server (Varejo, Licença de Volume, OEM, ODM etc.) e da licença do servidor, talvez você deva inserir uma chave de licenciamento para continuar.

7. Selecione a edição do Windows Server 2019 que você deseja instalar e selecione **Avançar**.

    ![Tela para escolher o Windows Server 2012 R2 Edition a ser instalado](media/upgrade-2012r2-2019/select-os-edition.png)

8. Selecione **Aceitar** para aceitar os termos do seu contrato de licenciamento com base no seu canal de distribuição (como Varejo, Licença de Volume, OEM, ODM e assim por diante).

    ![Tela para aceitar seu contrato de licença](media/upgrade-2012r2-2019/license-terms.png)

9. A instalação recomendará que você remova o Microsoft Endpoint Protection usando **Adicionar/Remover Programas**.

    Esse recurso não é compatível com o Windows Server 2019.

10. Selecione **Manter arquivos e aplicativos pessoais** para optar por fazer uma atualização in-loco e, em seguida, selecione **Avançar**.

    ![Tela para escolher seu tipo de instalação](media/upgrade-2012r2-2019/choose-install-upgrade.png)

11. Após a Instalação analisar seu dispositivo, ela solicitará que você prossiga com a atualização selecionando **Instalar**.

    ![Tela mostrando que você está pronto para iniciar a atualização](media/upgrade-2012r2-2019/ready-to-install.png)

    A atualização in-loco é iniciada, mostrando a tela **Atualizando o Windows** com o progresso. Após a conclusão da atualização, o servidor será reiniciado.

    ![Tela mostrando o progresso da atualização](media/upgrade-2012r2-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Após a conclusão da atualização

Após a conclusão da atualização, você deverá verificar se a atualização para o Windows Server 2019 foi bem-sucedida.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para verificar se a atualização foi bem-sucedida

1. Abra o Editor do Registro, acesse o hive HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion e veja o **ProductName**. Você deverá ver sua edição do Windows Server 2019, por exemplo **Windows Server 2019 Datacenter**.

2. Verifique se todos os aplicativos estão em execução e se as conexões do cliente com os aplicativos foram bem-sucedidas.

Se acreditar que possa ter ocorrido um erro durante a atualização, copie e compacte o diretório `%SystemRoot%\Panther` (geralmente `C:\Windows\Panther`) e entre em contato com o Suporte da Microsoft.

## <a name="related-articles"></a>Artigos relacionados

- Para obter mais detalhes e informações sobre o Windows Server 2019, confira [Introdução ao Windows Server 2019](../get-started-19/get-started-19.md).