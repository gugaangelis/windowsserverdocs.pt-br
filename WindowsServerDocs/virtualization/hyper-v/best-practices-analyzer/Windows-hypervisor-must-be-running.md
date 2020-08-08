---
title: O hipervisor do Windows deve estar em execução
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: aa89f3735151b2dee795c1a325e22446e3770fa0
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994537"
---
# <a name="windows-hypervisor-must-be-running"></a>O hipervisor do Windows deve estar em execução

>Aplica-se a: Windows Server 2016

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Pré-requisitos|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*O hipervisor do Windows não está em execução.*

## <a name="impact"></a>Impacto

*As máquinas virtuais não podem ser iniciadas até que o hipervisor do Windows esteja em execução.*

## <a name="resolution"></a>Resolução

*Verifique o catálogo do Windows Server para ver se este servidor está qualificado para executar o Hyper-V. Em seguida, verifique se o BIOS está habilitado para virtualização assistida por hardware e prevenção de execução de dados imposta por hardware. Em seguida, verifique o log de eventos do hipervisor do Hyper-V.*

Para verificar o catálogo, consulte [catálogo do Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) ( https://go.microsoft.com/fwlink/?LinkId=111228) .

> [!CAUTION]
> A alteração de determinados parâmetros no BIOS do sistema de um computador pode fazer com que esse computador pare de carregar o sistema operacional, ou pode fazer com que os dispositivos de hardware, como unidades de disco rígido, não estejam disponíveis. Sempre consulte o manual do usuário para o computador para determinar a maneira apropriada de configurar o BIOS do sistema. Além disso, é sempre uma boa ideia manter o controle dos parâmetros que você modifica e seu valor original para que você possa restaurá-los posteriormente, se necessário. Se você tiver problemas após a alteração dos parâmetros no BIOS do sistema, tente carregar as configurações padrão (uma opção geralmente está disponível no utilitário de configuração do BIOS) ou entre em contato com o fabricante do computador para obter assistência.

#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Para verificar o suporte de virtualização no BIOS ou UEFI

1.  Reinicie o computador e acesse o BIOS ou o UEFI por meio da ferramenta de configuração. O acesso a essa ferramenta geralmente está disponível quando o computador passa por um processo de inicialização. Imediatamente depois de ativar a maioria dos computadores, uma mensagem é exibida por alguns segundos que lista a chave ou a combinação de teclas a serem pressionadas para abrir a ferramenta de configuração.

2.  Encontre as configurações de virtualização e DEP (prevenção de execução de dados) imposta por hardware e verifique se elas estão ativadas. A seguir estão os locais de menu comuns para essas configurações na ferramenta de configuração e exemplos de como eles podem ser nomeados:

    -   Suporte à virtualização:

        -   Geralmente disponível nas configurações do processador principal ou do desempenho. Às vezes, ele está sob as configurações de segurança.

        -   Procure nomes de parâmetro que incluam "virtualização" ou "tecnologia de virtualização".

    -   DEP imposta por hardware:

        -   Geralmente disponível nas configurações de segurança ou memória.

        -   Procure nomes de parâmetro que incluam "Execution", "Execute" ou "Prevention".

3.  Se necessário, ative as configurações seguindo as instruções na ferramenta de configuração do. Salve as alterações e saia.

4.  Se você fez alguma alteração, desative e ligue novamente para concluir.

    > [!IMPORTANT]
    > Recomendamos que você desligue e ligue novamente (às vezes chamado de ciclo de energia) porque as alterações não são aplicadas em alguns computadores até que isso aconteça.

Em seguida, verifique o log de eventos do hipervisor do Hyper-V. Se houver problemas, você também verificará o log do sistema.

#### <a name="to-check-the-event-logs"></a>Para verificar os logs de eventos

1.  Visualizador de EventosAberto. Clique em **Iniciar**, em **Ferramentas administrativas**e em **Visualizador de eventos**.

2.  Abra o log de eventos do hipervisor do Hyper-V. No painel de navegação, expanda **logs de aplicativos e serviços**  >>  **Microsoft**  >>  **Windows**  >>  **Hyper-V-hipervisor**e, em seguida, clique em **operacional**.

3.  Se o hipervisor do Windows estiver em execução, nenhuma ação adicional será necessária. Se o hipervisor do Windows não estiver em execução, faça o seguinte:

4.  Abra o log do sistema. (No painel de navegação, expanda **logs do Windows** e, em seguida, selecione **sistema**.)

5.  Use um filtro para localizar eventos do hipervisor do Hyper-V:
    1. No painel **ações** , clique em **Filtrar log atual**. Para **fontes de evento**, especifique "Hyper-V-hipervisor".
    2. Procure eventos que relatam problemas. Por exemplo, a ID de evento 41 indica um problema com a configuração do BIOS: "falha na inicialização do Hyper-V; A VMX não está presente ou não está habilitada no BIOS. "

### <a name="see-also"></a>Consulte Também
Para obter detalhes sobre como usar o Hyper-V no Windows 10, incluindo como verificar se o computador pode executar o Hyper-v, consulte [requisitos de sistema do Hyper-v do Windows 10](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).