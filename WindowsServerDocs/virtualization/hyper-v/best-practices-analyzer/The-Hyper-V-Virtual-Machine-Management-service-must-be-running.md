---
title: O serviço gerenciamento de máquinas virtuais do Hyper-V deve estar em execução
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
ms.date: 10/03/2016
ms.openlocfilehash: 6fe2f95821b23f98931b7cecc3c59dc58f0f5e37
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746231"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>O serviço gerenciamento de máquinas virtuais do Hyper-V deve estar em execução

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Pré-requisitos|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*O serviço necessário para gerenciar as máquinas virtuais não está em execução.*

## <a name="impact"></a>Impacto

*Nenhuma operação de gerenciamento de máquina virtual pode ser executada.*

As máquinas virtuais que estão em execução continuarão a ser executadas. No entanto, você não poderá gerenciar máquinas virtuais ou criá-las ou excluí-las até que o serviço esteja em execução.

## <a name="resolution"></a>Resolução

*Use o snap-in de serviços ou a ferramenta de linha de comando sc config para reconfigurar o serviço para iniciar automaticamente.*

> [!TIP]
> Se você não encontrar o serviço no aplicativo da área de trabalho ou a ferramenta de linha de comando informar que o serviço não existe, as ferramentas de gerenciamento do Hyper-V provavelmente não estão instaladas.
E se você não conseguir ver o console do MMC do Hyper-V no menu Iniciar, instale as ferramentas de gerenciamento do Hyper-V.

Para instalar as ferramentas de gerenciamento do Hyper-V:
>
> - No Windows Server, abra Gerenciador do Servidor e use o assistente para adicionar funções e recursos. Para obter mais detalhes, consulte [instalar a função Hyper-V no Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  Você também pode usar o PowerShell para instalar as ferramentas ( `Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell` )
> - No Windows, na área de trabalho, comece digitando **programas**, clique em **programas e recursos** (painel de controle) > **Ativar ou desativar recursos do Windows em**  >  **Hyper-V**  >  **ferramentas de gerenciamento**Hyper-v Hyper-v. Depois, clique em **OK**.

### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para reconfigurar o serviço para iniciar automaticamente usando o aplicativo de área de trabalho de serviços

1.  Abra o aplicativo de área de trabalho serviços. (Clique em **Iniciar**, clique na caixa **Iniciar pesquisa** , digite **Services. msc**e pressione Enter.)

2.  No painel de detalhes, clique com o botão direito do mouse em **Gerenciamento de máquinas virtuais do Hyper-V**e clique em **Propriedades**.

3.  Na guia **geral** , em tipo de **inicialização** , clique em **automático**.

4.  Para iniciar o serviço, clique em **Iniciar**.

### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Para reconfigurar o serviço para iniciar automaticamente usando SC config

1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)

2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.

3.  Para reconfigurar o serviço, digite:

    ```
    sc config vmms start=auto
    ```

4.  Para iniciar o serviço, digite:

    ```
    sc start vmms
    ```

Se o serviço já estiver configurado para iniciar automaticamente e você só precisar reiniciar o serviço, você poderá fazer isso no Gerenciador do Hyper-V ou no comando sc Start VMMS mostrado acima.

#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Para reiniciar o serviço do Gerenciador do Hyper-V

1.  Abra o Gerenciador do Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.

2.  No painel de navegação, clique no nome do servidor se ele ainda não estiver selecionado.

3.  No painel **ações** , clique em **Iniciar serviço**.



