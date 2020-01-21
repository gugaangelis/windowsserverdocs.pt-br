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
ms.date: 12/16/2019
ms.openlocfilehash: 83ab3663b2c03017ba1bf613a49c394be0511002
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162497"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Como usar as ESUs (Atualizações de Segurança Estendidas) do Windows Server 2008 e 2008 R2

>Aplica-se a: Windows Server 2008/2008 R2

O Windows Server 2008 e o Windows Server 2008 R2 chegarão ao fim do ciclo de vida do suporte em 14 de janeiro de 2020. O LTSC (Canal de Manutenção em Longo Prazo) do Windows Server tem no mínimo dez anos de suporte, cinco anos para suporte base e cinco anos para suporte estendido. Esse suporte inclui as atualizações de segurança regulares.

O fim do suporte também significa o fim das atualizações de segurança. Esse cenário pode causar problemas de segurança ou de conformidade e colocar os aplicativos de negócios em risco. A Microsoft recomenda que você [atualize para a versão atual do Windows Server](modernize-windows-server-2008.md) com o objetivo de obter a segurança, o desempenho e a inovação mais avançados.

Caso não possa atualizar todos os servidores até o prazo final do ciclo de vida do suporte, as opções a seguir ajudarão a proteger os aplicativos e os dados durante a transição de atualização:

* Migre as cargas de trabalho existentes do Windows Server 2008 e 2008 R2 no estado em que se encontram para as VMs (Máquinas Virtuais) do Azure.
    * Essa migração para o Azure fornecerá automaticamente três anos adicionais de ESUs (Atualizações de Segurança Estendidas). Não há nenhum custo adicional para as atualizações de segurança estendidas além do custo da VM do Azure e não há nenhuma configuração adicional necessária.
* Adquira uma assinatura de atualização de segurança estendida para seus servidores e mantenha a proteção até que esteja pronto para atualizar para uma versão mais recente do Windows Server.
    * Essas atualizações são fornecidas por até três anos após a data de término do ciclo de vida do suporte.

Após o período de três anos de atualizações estendidas, não haverá nenhuma opção para que os computadores recebam atualizações adicionais.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Quais são as atualizações de segurança estendidas para o Windows Server?

As ESUs (Atualizações de Segurança Estendidas) para o Windows Server incluem atualizações de segurança e boletins classificados como *críticos* e *importantes*, por no máximo três anos após 14 de janeiro de 2020. As atualizações de segurança estendidas não incluem o seguinte:

* Novos recursos
* Hotfixes não relacionados à segurança solicitados pelo cliente
* Solicitações de alteração de design

Para obter mais informações, consulte as [Perguntas frequentes sobre atualizações de segurança estendidas](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="register-for-extended-security-updates"></a>Registrar-se para obter as atualizações de segurança estendidas

Para usar as atualizações de segurança estendidas, crie um MAK (chave de ativação múltipla) e aplique-o aos computadores com Windows Server 2008 e 2008 R2. Essa chave permite que os servidores do Windows Update saibam que você pode continuar recebendo atualizações de segurança. Registre-se para obter as atualizações de segurança estendidas e gerencie essas chaves usando o portal do Azure, mesmo que você use apenas computadores locais.

> [!NOTE]
> Caso execute as VMs do Windows Server 2008/2008 R2 no Azure, não será necessário executar as etapas a seguir. As VMs do Azure são habilitadas automaticamente para as atualizações de segurança estendidas. Não é preciso criar um recurso e uma chave de atualização de segurança estendida e não há nenhum custo adicional pelo uso de atualizações de segurança estendidas com as VMs do Azure.

> [!NOTE]
> Antes de seguir as etapas abaixo, envie um email para [winsvresuchamps@microsoft.com](mailto:winsvresuchamps@microsoft.com) com estas informações para aprovação na lista de permissões:
> * Nome do cliente:
> * Assinatura do Azure:
> * Número do Contrato Enterprise (para ESU):
> * Número de servidores ESU:
> 
> A equipe examinará as informações fornecidas e adicionará o usuário e a assinatura à lista de permissões.
> 
> Se o solicitante não estiver na lista de permissões, o seguinte erro poderá ocorrer: [Não foi possível encontrar o tipo de recurso no namespace 'Microsoft.WindowsESU'](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version)

Para registrar VMs não Azure para obter as atualizações de segurança estendidas e criar uma chave, conclua as seguintes etapas no portal do Azure:

1. Entre no [portal do Azure](https://portal.azure.com/).
1. Na caixa de pesquisa na parte superior do portal do Azure, procure e selecione **Atualizações de Segurança Estendidas**.

    ![Pesquise por atualizações de segurança estendidas no portal do Azure](media/extended-security-updates/esu-portal-search.png)

    Caso ainda não tenha usado as atualizações de segurança estendidas, primeiro escolha **+ Criar** um recurso de atualizações de segurança estendidas. Caso contrário, selecione o recurso na lista.

1. Em **Registrar-se para obter as Atualizações de Serviço Estendidas**, selecione **Introdução**.

    ![Introdução às Atualizações de Segurança Estendidas no portal do Azure](media/extended-security-updates/get-started-with-esu.png)

1. Para criar sua primeira chave, selecione **Obter chave**.

    ![Escolha criar uma chave no portal do Azure](media/extended-security-updates/get-key.png)

    > [!NOTE]
    > Será necessário ter uma assinatura do Azure associada à sua conta para criar o recurso e a chave de atualização de segurança estendida. Caso não tenha uma assinatura do Azure associada à sua conta, entre com uma conta de usuário diferente ou crie uma assinatura do Azure usando as etapas guiadas mostradas no portal.

1. Em **Detalhes do Azure**, selecione sua assinatura do Azure, um grupo de recursos e um local para sua chave.

    Em **Detalhes do registro**, insira as seguintes informações:

    | Configuração             | Valor |
    |---------------------|-------|
    | Nome da chave            | Um nome de exibição para sua chave, como *Agreement01*. |
    | Número de contrato    | Seu número de contrato gerado pelo sistema de gerenciamento de contratos de licenciamento por volume ou MSLicense para programas do Contrato Enterprise. |
    | Número de computadores | Escolha o número de computadores nos quais deseja instalar as Atualizações de Segurança Estendidas com essa chave. |
    | Sistema operacional    | Escolha o sistema operacional com o qual deseja usar essa chave, como o Windows Server 2008 ou o Windows Server 2008 R2. |

    Quando estiver pronto, selecione **Revisar + registrar**.

1. Após a validação bem-sucedida, um resumo de suas opções para o novo recurso de registro será mostrado. Se necessário, corrija os erros de validação ou atualize sua opção de configuração. Os [Termos de uso](https://azure.microsoft.com/support/legal/) e a [Política de privacidade](https://privacy.microsoft.com/privacystatement) do Azure estarão disponíveis.

    Marque a caixa para confirmar que tem computadores qualificados e que a chave será usada somente em sua organização:

    ![Confirme que a chave será usada somente por sua organização](media/extended-security-updates/confirm-key-usage.png)

    Quando estiver pronto, selecione **Criar** para gerar o MAK.

O registro das atualizações de segurança estendidas agora estará disponível para uso em seus computadores. A chave criada deve ser aplicada aos computadores com Windows Server 2008 e 2008 R2 que desejar manter qualificados para atualizações de segurança.

## <a name="download-and-apply-extended-security-updates"></a>Baixar e aplicar as atualizações de segurança estendidas

A entrega, o download e a aplicação das atualizações de segurança estendidas para o Windows Server não são diferentes dos processos de implantação existentes. As atualizações fornecidas por meio de atualizações de segurança estendidas são apenas para *Segurança* e são lançadas a cada Patch às terças-feiras.

É possível instalar as atualizações usando quaisquer ferramentas e processos que já estejam em vigor. A única diferença é que o sistema deve ser registrado usando a chave gerada na seção anterior para que as atualizações sejam baixadas e instaladas.

Para as VMs do Azure, o processo de habilitar o computador para as atualizações de segurança estendidas será automaticamente concluído para você. As atualizações devem ser baixadas e instaladas sem configurações adicionais.
