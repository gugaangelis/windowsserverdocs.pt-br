---
title: Transição do Windows Server Essentials para o Windows Server 2012 Standard
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 445472822de09263b84821e552c931ca19f14b2b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432536"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transição do Windows Server Essentials para o Windows Server 2012 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials oferece suporte a até 25 usuários e 50 dispositivos. Quando as necessidades de sua empresa excedem o limite, você pode realizar uma transição de licença in loco do Windows Server Essentials para Windows Server 2012 Standard para manter a conformidade da licença.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Como a transição afeta os limites de usuários e dispositivos  
 Após a transição para o Windows Server 2012 Standard, os limites de conta e dispositivos de usuário são removidos, mas os recursos que são exclusivos para o Windows Server Essentials (como o painel de acesso via Web remoto e backup do computador cliente), continuam disponíveis. Porém, por limitações técnicas, esses recursos oferecem suporte no máximo a 75 contas de usuário e 75 dispositivos. Se for necessário adicionar mais de 75 contas de usuário ou dispositivos, você deve desativar os recursos do Windows Server Essentials e usar as ferramentas nativas do Windows Server 2012 Standard para gerenciar dispositivos e contas de usuário.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard requer uma licença de acesso de cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo de CAL e não vem com CALs.  Quando a transição do Windows Server Essentials para Windows Server 2012 Standard, você precisará comprar o número e tipo corretos de CALs para seu ambiente (a maioria dos clientes adquire CALs de usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de fazer a transição do Windows Server Essentials para Windows Server 2012 Standard, você deve fazer o backup completo de dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, não é possível restaurar o servidor ao estado que estava antes da transição.  
  
-   Além disso, certifique-se de que você leia e entenda o contrato de licença de usuário final (EULA) do Windows Server 2012 Standard. Para ler o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         Onde **caminho do eula** representa o local no qual deseja salvar o arquivo do EULA. Por exemplo, C:\ws8std_eula.rtf.  Use .rtf como extensão de nome de arquivo.  
  
    3.  Abra o local onde salvou o arquivo e clique duas vezes nele para abri-lo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transição para o Windows Server 2012 Standard  
 Após decidir fazer a transição do Windows Server Essentials para Windows Server 2012 Standard, completa essas duas etapas:  
  
1. Compre uma licença para o Windows Server 2012 Standard e o número apropriado de usuário e/ou dispositivo licenças de acesso para cliente para o seu ambiente.  
  
    Você pode adquirir uma licença para o Windows Server 2012 Standard em uma loja, um distribuidor ou com a Ajuda de um [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se você adquiriu o Windows Server 2012 Standard inicialmente e utilizado seus direitos de downgrade para instalar uma das duas instâncias virtuais como Windows Server Essentials, não é necessário aquirir mais nada.  
   >   
   >  Se você comprar o Windows Server 2012 Standard por meio do canal de licenciamento por Volume, você pode baixar uma imagem ISO e uma chave de produto para o Windows Server 2012 Standard do Volume Licensing Service Center (VLSC).  
   >   
   >  Se você comprar o Windows Server 2012 Standard de todos os outros canais pode baixar uma imagem ISO e uma chave de produto de avaliação do Windows Server Essentials a partir de [Centro de avaliação TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). A execução da transição descrita na etapa a seguir converterá o produto de avaliação em um produto com licença e suporte completos.  
  
2. Abra o Windows PowerShell como administrador e execute o comando a seguir.  
  
    **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Chave do produto*  
  
    Em que *chave do produto* é a chave do produto de sua cópia do Windows Server 2012 Standard.  
  
    O servidor é reiniciado para concluir o processo de transição.  
  
   Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 75 usuários e 75 dispositivos. Se você exceder qualquer um desses limites, você deve usar as ferramentas nativas do Windows Server 2012 Standard para gerenciar dispositivos e contas de usuário.  
  
   Além disso, após a transição para o Windows Server 2012 Standard, os recursos de mídia do Windows Server Essentials não estão mais disponíveis. Isso inclui o Acesso via Web Remoto e as configurações de mídia no Painel.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Desativar recursos do Windows Server Essentials  
 Se você não precisar mais do painel do Windows Server Essentials ou outros recursos para gerenciar o servidor, você pode desativar os recursos e removê-los do seu servidor.  
  
 O **desativar Assistente de recursos do Windows Server Essentials** ajuda a desinstalar os recursos. Ele também remove do servidor de arquivos que foram criados pelo software do servidor Windows Server Essentials.  Algumas operações de limpeza são realizadas imediatamente, enquanto outras são iniciadas após a reinicialização do servidor.  
  
 O **desativar Assistente de recursos do Windows Server Essentials** exige que você desinstale manualmente todos os suplementos antes de concluir o assistente. Para exibir uma lista dos suplementos instalados, abra a página Aplicativo do Painel. O assistente o avisará se encontrar suplementos instalados e solicitará sua desinstalação.  
  
 O **desativar Assistente de recursos do Windows Server Essentials** permite que você escolha se deseja manter os computadores de arquivos de backup do cliente após desativar os recursos do Windows Server Essentials.  
  
 Há duas maneiras de executar o **desativar Assistente de recursos do Windows Server Essentials** do painel:  
  
#### <a name="from-the-alert"></a>A partir do alerta  
  
1.  No Painel, abra o Visualizador de Alertas.  
  
2.  Na lista organizar, selecione o alerta que relata informações sobre como desativar recursos do Windows Server Essentials após a transição.  
  
3.  No alerta, clique em **desativar recursos do Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>No painel de ajuda e suporte  
  
1. Na Home page, clique em Obter Ajuda e Suporte.  
  
2. Clique em **desativar Assistente de recursos do Windows Server Essentials**.  
  
   É possível que algumas tarefas executadas pelo **desativar Assistente de recursos do Windows Server Essentials** não será concluída com êxito. Em alguns casos, isso pode impedir a execução do Painel. Caso isso ocorra, você poderá iniciar o assistente manualmente executando o arquivo:  
  
   **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Consulte também  
  

-   [Transição para o Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transição para o Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

