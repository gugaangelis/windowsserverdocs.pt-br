---
title: Hipervisor do Windows deve estar em execução
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: f34e10918c60fb602c3a88ef3434cda619b11d8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884137"
---
# <a name="windows-hypervisor-must-be-running"></a>Hipervisor do Windows deve estar em execução

>Aplica-se a: Windows Server 2016
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Pré-requisitos|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Hipervisor do Windows não está em execução.*  
  
## <a name="impact"></a>Impacto  
  
*Máquinas virtuais não podem ser iniciadas até que o hipervisor do Windows está em execução.*  
  
## <a name="resolution"></a>Resolução  
  
*Verifique o catálogo do Windows Server para ver se esse servidor está qualificado para executar o Hyper-V. Em seguida, verifique se que o BIOS está habilitado para virtualização assistida por hardware e a prevenção de execução de dados imposta por hardware. Em seguida, verifique o log de eventos do hipervisor do Hyper-V.*  
  
Para verificar o catálogo, consulte [catálogo do Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> Alterar certos parâmetros no BIOS do sistema de um computador pode fazer com que o computador para interromper o carregamento do sistema operacional, ou pode tornar os dispositivos de hardware, como unidades de disco rígido disponível. Sempre consulte o manual do usuário para o computador para determinar o modo adequado para configurar o BIOS do sistema. Além disso, é sempre uma boa ideia para controlar os parâmetros que você modifique e do valor original para que você pode restaurá-los mais tarde, se necessário. Se você tiver problemas depois de alterar parâmetros no BIOS do sistema, tenta carregar as configurações padrão (uma opção é geralmente disponível no utilitário de configuração do BIOS) ou entre em contato com o fabricante do computador para obter assistência.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Para confirmar o suporte de virtualização no BIOS ou UEFI  
  
1.  Reinicie o computador e acessar o BIOS ou UEFI por meio da ferramenta de configuração. Acesso a essa ferramenta normalmente está disponível quando o computador passa por um processo de inicialização. Imediatamente depois de ativar a maioria dos computadores, uma mensagem é exibida por alguns segundos que lista a tecla ou combinação de teclas para abrir a ferramenta de configuração.  
  
2.  Localize as configurações para virtualização e prevenção de execução de dados (DEP) imposta por hardware e verifique se eles estão no. Estes são os locais de menu comuns para essas configurações na ferramenta de configuração e exemplos de como o que elas podem ser nomeadas:  
  
    -   Suporte de virtualização:  
  
        -   Geralmente disponível sob as configurações para o processador principal ou o desempenho. Às vezes, ele está sob as configurações de segurança.  
  
        -   Procure nomes de parâmetro que incluem "virtualização" ou "tecnologia de virtualização".  
  
    -   DEP imposta ao hardware:  
  
        -   Geralmente disponível em configurações de segurança ou de memória.  
  
        -   Procure nomes de parâmetro que incluem "execução", "execução" ou "Prevenção".  
  
3.  Se necessário, ative as configurações, seguindo as instruções na ferramenta de configuração. Salve as alterações e saia.  
  
4.  Se você fizer alterações, desligue a energia e, em seguida, novamente para concluir.  
  
    > [!IMPORTANT]  
    > É recomendável que você desligue a energia e ligar novamente (às vezes chamado de um ciclo de energia), porque as alterações não são aplicadas em alguns computadores até que isso ocorra.  
  
Em seguida, verifique o log de eventos do hipervisor do Hyper-V. Se houver problemas, você também vai verificar o log do sistema.  
  
#### <a name="to-check-the-event-logs"></a>Para verificar os logs de eventos  
  
1.  Abra o Visualizador de Eventos. Clique em **inicie**, clique em **ferramentas administrativas**e, em seguida, clique em **Visualizador de eventos**.  
  
2.  Abra o log de eventos do hipervisor do Hyper-V. No painel de navegação, expanda **Applications and Services Logs** >> **Microsoft** >> **Windows**  >>   **Hipervisor do Hyper-V**e, em seguida, clique em **operacional**.  
  
3.  Se o hipervisor do Windows está em execução, nenhuma ação adicional é necessária. Se o hipervisor do Windows não está em execução, fazer isso:  
  
4.  Abra o log do sistema. (No painel de navegação, expanda **Logs do Windows** e, em seguida, selecione **sistema**.)  
  
5.  Use um filtro para localizar eventos de hipervisor do Hyper-V:   
    1. No **ações** painel, clique em **Filtrar Log atual**. Para **origens do evento**, especifique "Hyper-V-hipervisor".   
    2. Procure eventos que relatam problemas. Por exemplo, o evento ID 41 indica um problema com a configuração do BIOS: "Falha na inicialização do Hyper-V; O VMX não está presente ou não está habilitado no BIOS."  
  
### <a name="see-also"></a>Consulte também  
Para obter detalhes sobre como usar o Hyper-V no Windows 10, incluindo como verificar que seu computador pode executar o Hyper-V, consulte [requisitos de sistema do Windows 10 Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility). 


