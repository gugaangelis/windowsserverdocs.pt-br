---
title: Atualizações de segurança estendidas do Windows Server 2008 e 2008 R2
description: Saiba como usar as ESUs (Atualizações de Segurança Estendidas) para o Windows Server 2008 e 2008 R2 após o término do ciclo de vida do suporte.
ms.prod: windows-server
ms.technology: server-general
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 02/21/2020
ms.openlocfilehash: 19a65f2a254fe14f7cddfbda2a84e9d00f47da56
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181842"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Como usar as ESUs (Atualizações de Segurança Estendidas) do Windows Server 2008 e 2008 R2

>Aplica-se a: Windows Server 2008 e Windows Server 2008 R2

O Windows Server 2008 e o Windows Server 2008 R2 chegaram ao fim do ciclo de vida do suporte em 14 de janeiro de 2020. O LTSC (Canal de Manutenção em Longo Prazo) do Windows Server tem um mínimo de dez anos de suporte: cinco anos para suporte base e cinco anos para suporte estendido. Esse suporte inclui as atualizações de segurança regulares.

O fim do suporte também significa o fim das atualizações de segurança. Esse cenário pode causar problemas de segurança ou de conformidade e colocar os aplicativos de negócios em risco. A Microsoft recomenda que você [atualize para a versão atual do Windows Server](modernize-windows-server-2008.md) com o objetivo de obter a segurança, o desempenho e a inovação mais avançados.

Caso você ainda não tenha atualizado seus servidores, as seguintes opções ajudam a proteger aplicativos e dados durante a transição:

* Migre as cargas de trabalho existentes do Windows Server 2008 e 2008 R2 no estado em que se encontram para as VMs (Máquinas Virtuais) do Azure.
  * Essa migração para o Azure fornecerá automaticamente três anos adicionais de ESUs (Atualizações de Segurança Estendidas). Não há nenhum custo adicional para as atualizações de segurança estendidas além do custo da VM do Azure e não há nenhuma configuração adicional necessária.
* Adquira uma assinatura de atualização de segurança estendida para seus servidores e mantenha a proteção até que esteja pronto para atualizar para uma versão mais recente do Windows Server.
  * Essas atualizações são fornecidas por até três anos após a data de término do ciclo de vida do suporte.

Após o período de três anos de atualizações estendidas, interromperemos a atualização do Windows Server 2008 e 2008 R2. Recomendamos atualizar sua versão do Windows Server para uma versão mais recente assim que possível.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Quais são as atualizações de segurança estendidas para o Windows Server?

As ESUs (Atualizações de Segurança Estendidas) para o Windows Server incluem atualizações de segurança e boletins classificados como *críticos* e *importantes*, por no máximo três anos após 14 de janeiro de 2020. As atualizações de segurança estendidas não incluem o seguinte:

* Novos recursos
* Hotfixes não relacionados à segurança solicitados pelo cliente
* Solicitações de alteração de design

Para obter mais informações, consulte as [Perguntas frequentes sobre atualizações de segurança estendidas](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="how-to-use-extended-security-updates"></a>Como usar as Atualizações de Segurança Estendidas

Se você executar VMs do Windows Server 2008 ou 2008 R2 no Azure, elas serão habilitadas automaticamente para as Atualizações de Segurança Estendidas. Você não precisa configurar nada, e não há nenhum encargo adicional pelo uso das Atualizações de Segurança Estendidas nas VMs do Azure. As Atualizações de Segurança Estendidas são entregues automaticamente às VMs do Azure quando elas estão configuradas para receber atualizações.

Para outros ambientes, como VMs locais ou servidores físicos, você precisará solicitar e configurar manualmente as Atualizações de Segurança Estendidas. Compre as Atualizações de Segurança Estendidas por meio de Programas de Licenciamento por Volume, como o EA (Contrato Enterprise), o EAS (Contrato Enterprise Subscription), o EES (Registro para Soluções Educacionais) ou o SCE (Registro de Servidor e de Nuvem).

Quando você comprar as Atualizações de Segurança Estendidas, use um dos seguintes métodos para obter suas chaves:

* Caso deseje obter chaves da Atualização de Segurança Estendida no portal do Azure, [registre-se para obter as Atualizações de Segurança Estendidas no portal do Azure](#register-for-extended-security-updates-on-azure-portal).
* Além disso, [entre no Centro de Serviços de Licenciamento por Volume da Microsoft](#sign-in-to-the-microsoft-volume-licensing-service-center) para obter suas chaves sem usar o portal do Azure.

### <a name="register-for-extended-security-updates-on-azure-portal"></a>Registre-se para obter as Atualizações de Segurança Estendidas no portal do Azure

Para usar as Atualizações de Segurança Estendidas em VMs que não são do Azure, crie uma MAK (chave de ativação múltipla) e aplique-a aos computadores Windows Server 2008 e 2008 R2. A chave MAK permite que os servidores do Windows Update saibam que você pode continuar recebendo atualizações de segurança. Registre-se para obter as Atualizações de Segurança Estendidas e gerencie essas chaves usando o portal do Azure, mesmo que você use apenas computadores locais.

> [!NOTE]
> Você não precisará se registrar para obter as Atualizações de Segurança Estendidas se estiver executando o Windows Server 2008 e 2008 R2 em VMs do Azure. Para outros ambientes, como VMs locais ou servidores físicos, [compre as Atualizações de Segurança Estendidas](https://www.microsoft.com/licensing/how-to-buy/how-to-buy) antes de tentar registrá-las e usá-las.

Para registrar sua VM a fim de obter as Atualizações de Segurança Estendidas e criar uma chave, abra o portal do Azure e siga estas instruções:

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Na caixa de pesquisa na parte superior do portal do Azure, procure e selecione **Atualizações de Segurança Estendidas**.

    ![Procure as Atualizações de Segurança Estendidas no portal do Azure](media/extended-security-updates/esu-portal-search.png)

    Caso ainda não tenha usado as Atualizações de Segurança Estendidas, selecione **+ Criar** para primeiro criar um recurso das Atualizações de Segurança Estendidas. Caso contrário, selecione o recurso na lista.

3. Em **Registrar-se para obter as Atualizações de Serviço Estendidas**, selecione **Introdução**.

    ![Introdução às Atualizações de Segurança Estendidas no portal do Azure](media/extended-security-updates/get-started-with-esu.png)

4. Para criar sua primeira chave, selecione **Obter chave**.

    ![Optar por criar uma chave no portal do Azure](media/extended-security-updates/get-key.png)

    Você precisará ter uma assinatura do Azure associada à sua conta para criar o recurso e a chave da Atualização de Segurança Estendida. Caso não tenha uma assinatura do Azure associada à sua conta, entre com outra conta de usuário ou crie uma assinatura do Azure no portal do Azure.

    A função Colaborador também precisará ser atribuída à sua assinatura do Azure para que a atualização de segurança funcione. Para verificar sua função, insira "Assinaturas" na caixa de pesquisa. Você verá uma tabela que mostrará sua função ao lado da ID e do nome da assinatura.

    Se você não for um Colaborador, solicite ao proprietário da assinatura que altere sua função. Para descobrir quem é o proprietário da sua assinatura, acesse a tabela de funções descrita no parágrafo anterior e selecione o nome da assinatura. Em seguida, acesse o menu no lado esquerdo da página e selecione **Controle de acesso (IAM)**  > **Atribuições de função** e procure a seção "Proprietários" na tabela.

5. Caso você veja uma página indicando "Registre-se para obter uma chave de ativação múltipla", isso significa que é necessário solicitar o acesso à versão prévia privada para usar as Atualizações de Segurança Estendidas. Caso não veja essa página, pule para a etapa 6.

   Para solicitar o acesso, selecione **Ingressar na versão prévia privada**. Uma janela de mensagem de email será aberta. Esse email é a sua solicitação de acesso à equipe do produto.

    Inclua as seguintes informações na solicitação:

    * Nome do cliente
    * ID da assinatura do Azure
    * Número do contrato (para ESU)
    * Número de servidores ESU

    Quando terminar, envie o email.

    A equipe examinará as informações fornecidas no seu email de solicitação. Se tudo estiver correto, ela adicionará você à lista aprovada.

    Se a equipe não aprovar a sua solicitação, você verá o seguinte erro:

    [Não foi possível encontrar o tipo de recurso no namespace 'Microsoft.WindowsESU'](https://docs.microsoft.com/windows-server/get-started/extended-security-updates)

6. Em **Detalhes do Azure**, selecione sua assinatura do Azure, um grupo de recursos e um local para sua chave.

    Em **Detalhes do registro**, insira as seguintes informações:

    | Configuração             | Valor |
    |---------------------|-------|
    | Nome da chave            | Um nome de exibição para sua chave, como *Agreement01*. |
    | Número de contrato    | Seu número de contrato gerado pelo sistema de gerenciamento de contratos de licenciamento por volume ou MSLicense para programas do Contrato Enterprise. |
    | Número de computadores | Escolha o número de computadores nos quais deseja instalar as Atualizações de Segurança Estendidas com essa chave. |
    | Sistema operacional    | Escolha o sistema operacional com o qual deseja usar essa chave, como o Windows Server 2008 ou o Windows Server 2008 R2. |

    Quando estiver pronto, selecione **Revisar + registrar**.

    >[!NOTE]
    >Verifique se você selecionou a assinatura do Azure com a qual ingressou na versão prévia privada no filtro global. Selecione o botão **Filtrar** na faixa de opções do portal do Azure para verificar o filtro de assinatura global.
    >
    > ![Uma imagem da faixa de opções do portal do Azure com o botão Filtrar selecionado](media/azure-ribbon-filter.png)

7. Após a validação bem-sucedida, um resumo de suas opções para o novo recurso de registro será mostrado. Se necessário, corrija os erros de validação ou atualize sua opção de configuração. Os [Termos de uso](https://azure.microsoft.com/support/legal/) e a [Política de privacidade](https://privacy.microsoft.com/privacystatement) do Azure estarão disponíveis.

    Marque a caixa para confirmar que tem computadores qualificados e que a chave será usada somente em sua organização:

    ![Confirme que a chave será usada somente por sua organização](media/extended-security-updates/confirm-key-usage.png)

    Quando estiver pronto, selecione **Criar** para gerar o MAK.

O registro das Atualizações de Segurança Estendidas agora está disponível para uso nos computadores. A chave criada deve ser aplicada aos computadores com Windows Server 2008 e 2008 R2 que desejar manter qualificados para atualizações de segurança.

### <a name="sign-in-to-the-microsoft-volume-licensing-service-center"></a>Entrar no Centro de Serviços de Licenciamento por Volume da Microsoft

Caso não tenha acesso ao portal do Azure, use o Centro de Serviços de Licenciamento por Volume para ver e baixar suas chaves de ativação.

Para obter suas chaves no Centro de Serviços de Licenciamento por Volume:

1. Acesse a [página do Centro de Serviços de Licenciamento por Volume](https://www.microsoft.com/vlsc) e entre com as suas credenciais do Azure.

2. Selecione **Licenças** > **Resumo de Relações** > **ID de Licenciamento** > **Chaves do Produto (Product Keys)** .

Para saber mais sobre como obter as Atualizações de Segurança Estendidas para dispositivos Windows qualificados, confira a [nossa postagem na Tech Community](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/obtaining-extended-security-updates-for-eligible-windows-devices/ba-p/1167091#).

## <a name="download-and-apply-extended-security-updates"></a>Baixar e aplicar as Atualizações de Segurança Estendidas

A entrega, o download e a aplicação das Atualizações de Segurança Estendidas para o Windows Server não são diferentes dos processos de implantação existentes. As atualizações fornecidas por meio das Atualizações de Segurança Estendidas destinam-se apenas à *Segurança* e são lançadas em cada Patch Tuesday.

É possível instalar as atualizações usando quaisquer ferramentas e processos que já estejam em vigor. A única diferença é que o sistema deve ser registrado usando a chave gerada na seção anterior para que as atualizações sejam baixadas e instaladas.

Para as VMs do Azure, o processo de habilitar o computador para as Atualizações de Segurança Estendidas é automaticamente concluído para você. As atualizações devem ser baixadas e instaladas sem configurações adicionais.
