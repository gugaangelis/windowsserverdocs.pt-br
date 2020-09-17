---
title: Escolha entre pontos de verificação padrão ou de produção no Hyper-V
description: Fornece instruções para configurar uma máquina virtual para usar pontos de verificação padrão ou de produção
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: 00a3c8d94fc18d180faa8927b33b90dc98854da0
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746471"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Escolha entre pontos de verificação padrão ou de produção no Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019


A partir do Windows Server 2016 e do Windows 10, você pode escolher entre pontos de verificação padrão e de produção para cada máquina virtual. Os pontos de verificação de produção são o padrão para novas máquinas virtuais.

- Os pontos de verificação de produção são imagens "pontuais" de uma máquina virtual, que podem ser restauradas posteriormente de uma forma que tenha suporte completo para todas as cargas de trabalho de produção. Isso é obtido usando a tecnologia de backup no convidado para criar o ponto de verificação, em vez de usar a tecnologia de estado salvo.

- Os pontos de verificação padrão capturam o estado, os dados e a configuração de hardware de uma máquina virtual em execução e devem ser usados em cenários de desenvolvimento e teste. Pontos de verificação padrão podem ser úteis se você precisar recriar um Estado ou condição específica de uma máquina virtual em execução para que você possa solucionar um problema.

  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Alterar pontos de verificação para os pontos de verificação padrão ou de produção

1.  No **Gerenciador do Hyper-V**, clique com o botão direito do mouse na máquina virtual e clique em **configurações**.

2.  Na seção **Gerenciamento** , selecione **pontos de verificação**.

3.  Selecione pontos de verificação de produção ou padrão.

    Se você escolher pontos de verificação de produção, também poderá especificar se o host deve usar um ponto de verificação padrão se um ponto de verificação de produção não puder ser obtido. Se você desmarcar essa caixa de seleção e um ponto de verificação de produção não puder ser obtido, nenhum ponto de verificação será obtido.

4.  Se você quiser armazenar os arquivos de configuração de ponto de verificação em um local diferente, altere-os na seção **local do arquivo de ponto de verificação** .

5.  Clique em **aplicar** para salvar as alterações. Se terminar, clique em **OK** para fechar a caixa de diálogo.

> [!NOTE]
> Somente **pontos de verificação de produção** têm suporte em convidados que executam Active Directory Domain Services função (controlador de domínio) ou Active Directory Lightweight Directory Services função.

## <a name="additional-references"></a>Referências adicionais

-   [Pontos de verificação de produção](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)

-   [Habilitar ou desabilitar pontos de verificação](Enable-or-disable-checkpoints-in-Hyper-V.md)



