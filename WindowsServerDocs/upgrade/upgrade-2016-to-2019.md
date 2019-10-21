---
title: Atualizar o Windows Server 2016 para o Windows Server 2019 | Microsoft Docs
description: Saiba como executar uma atualização in-loco para ir do Windows Server 2016 para o Windows Server 2019.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 62fe4f00cef121e6241a403ee339047cda9488b5
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591085"
---
# <a name="upgrade-windows-server-2016-to-windows-server-2019"></a>Atualizar o Windows Server 2016 para o Windows Server 2019

Se desejar manter o mesmo hardware e todas as funções de servidor que você já configurou sem mesclar o servidor, você desejará fazer uma atualização in-loco. Uma atualização in-loco permite que você vá de um sistema operacional mais antigo para um mais recente, ao mesmo tempo em que mantém suas configurações, funções de servidor e dados intactos. Este artigo ajuda você a migrar do Windows Server 2016 para o Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de começar sua atualização in-loco

Antes de iniciar a atualização do Windows Server, recomendamos que você colete algumas informações de seus dispositivos para fins de diagnóstico e solução de problemas. Como essas informações se destinam ao uso somente se a atualização falhar, você deverá certificar-se de armazenar as informações em algum lugar que possa acessá-las do seu dispositivo.

### <a name="to-collect-your-info"></a>Para coletar suas informações

1. Abra um prompt de comando, vá para `c:\Windows\system32` e digite **SystemInfo. exe**.

2. Copie, Cole e armazene as informações do sistema resultantes em algum lugar do seu dispositivo.

3. Digite **ipconfig/all** no prompt de comando e, em seguida, copie e cole as informações de configuração resultantes no mesmo local acima.

4. Abra o editor do registro, vá para a chave de `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` e, em seguida, copie e cole o **BuildlabEx** do Windows Server (versão) e o **EditionID** (edição) no mesmo local acima.

Depois de coletar todas as suas informações relacionadas ao Windows Server, é altamente recomendável que você faça backup de seu sistema operacional, aplicativos e máquinas virtuais. Você também deve **desligar**, **migrar rapidamente**ou **migrar ao vivo** todas as máquinas virtuais em execução no momento no servidor. Você não pode ter máquinas virtuais em execução durante a atualização in-loco.

## <a name="to-perform-the-upgrade"></a>Para executar a atualização

1. Verifique se o valor **BuildlabEx** diz que você está executando o Windows Server 2016.

2. Localize a mídia de instalação do Windows Server 2019 e, em seguida, selecione **Setup. exe**.

    ![Windows Explorer mostrando o arquivo setup. exe](media/upgrade-2016-2019/setup-2019.png)

3. Selecione **Sim** para iniciar o processo de instalação.

    ![Controle de conta de usuário solicitando permissão para iniciar a instalação](media/upgrade-2016-2019/start-setup-uac-box.png)

4. Para dispositivos conectados à Internet, selecione a opção **baixar atualizações, drivers e recursos opcionais (recomendado)** e, em seguida, selecione **Avançar**.

    ![Tela para escolher ficar online para obter atualizações importantes do Windows](media/upgrade-2016-2019/online-updates-win-setup.png)

5. A instalação verifica a configuração do dispositivo, você deve aguardar até que ela seja concluída e, em seguida, selecionar **Avançar**.

6. Dependendo do canal de distribuição do qual você recebeu a mídia do Windows Server (varejo, licença de volume, OEM, ODM, etc.) e da licença do servidor, talvez seja solicitado que você insira uma chave de licenciamento para continuar.

7. Selecione a edição do Windows Server 2019 que você deseja instalar e, em seguida, selecione **Avançar**.

    ![Tela para escolher qual edição do Windows Server 2016 instalar](media/upgrade-2016-2019/select-os-edition.png)

8. Selecione **aceitar** para aceitar os termos do seu contrato de licenciamento, com base no seu canal de distribuição (como, varejo, licença de volume, OEM, ODM e assim por diante).

    ![Tela para aceitar seu contrato de licença](media/upgrade-2016-2019/license-terms.png)

9. Selecione **manter arquivos pessoais e aplicativos** para optar por fazer uma atualização in-loco e, em seguida, selecione **Avançar**.

    ![Tela para escolher o tipo de instalação](media/upgrade-2016-2019/choose-install-upgrade.png)

10. Após a instalação analisar seu dispositivo, ele solicitará que você prossiga com a atualização selecionando **instalar**.

    ![Tela mostrando que você está pronto para iniciar a atualização](media/upgrade-2016-2019/ready-to-install.png)

    A atualização in-loco é iniciada, mostrando a tela **atualizando o Windows** com seu progresso. Após a conclusão da atualização, o servidor será reiniciado.

    ![Tela mostrando o andamento da atualização](media/upgrade-2016-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Após a conclusão da atualização

Após a conclusão da atualização, você deve verificar se a atualização para o Windows Server 2019 foi bem-sucedida.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para verificar se a atualização foi bem-sucedida

1. Abra o editor do registro, vá para a chave de `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` e exiba o **NomeDoProduto**. Você deve ver sua edição do Windows Server 2019, por exemplo, **Windows server 2019 datacenter**.

2. Verifique se todos os seus aplicativos estão em execução e se as conexões do cliente com os aplicativos foram bem-sucedidas.

Se você acredita que algo pode ter ficado errado durante a atualização, copie e compacte o diretório `%SystemRoot%\Panther` (geralmente `C:\Windows\Panther`) e entre em contato com o suporte da Microsoft.

## <a name="related-articles"></a>Artigos relacionados

- Para obter mais detalhes e informações sobre o Windows Server 2019, consulte Introdução [ao Windows server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19).
