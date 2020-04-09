---
title: Transição do Windows Server Essentials para o Windows Server 2012 Standard
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: aace7849244bb65ec0042971e6ec899f554a62d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852299"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transição do Windows Server Essentials para o Windows Server 2012 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 O Windows Server&reg; 2012 Essentials dá suporte a até 25 usuários e 50 dispositivos. Quando suas necessidades de negócios excederem o limite, você poderá executar uma transição de licença in-loco do Windows Server Essentials para o Windows Server 2012 Standard para permanecer em conformidade com a licença.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Como a transição afeta os limites de usuários e dispositivos  
 Depois de fazer a transição para o Windows Server 2012 Standard, os limites de conta de usuário e dispositivos são removidos, mas os recursos que são exclusivos do Windows Server Essentials (como o painel, Acesso via Web remota e backup do computador cliente) ainda permanecem disponíveis. Porém, por limitações técnicas, esses recursos oferecem suporte no máximo a 75 contas de usuário e 75 dispositivos. Se for necessário adicionar mais de 75 contas de usuário ou dispositivos, desative os recursos do Windows Server Essentials e use as ferramentas nativas do Windows Server 2012 Standard para gerenciar contas de usuário e dispositivos.  
  
> [!IMPORTANT]
>   O Windows Server 2012 Standard requer uma licença de acesso para cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo CAL e não vem com nenhuma Cal.  Ao fazer a transição do Windows Server Essentials para o Windows Server 2012 Standard, você precisará comprar o número e o tipo de CALs apropriados para o seu ambiente (a maioria dos clientes adquire as CALs do usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de passar do Windows Server Essentials para o Windows Server 2012 Standard, você deve fazer backup completo dos dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, não é possível restaurar o servidor ao estado que estava antes da transição.  
  
-   Além disso, certifique-se de ler e entender o contrato de licença de usuário final (EULA) para o Windows Server 2012 Standard. Para ler o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **dism/online/Set-Edition: ServerStandard/geteula: caminho do EULA**  
  
         Onde **caminho do eula** representa o local no qual deseja salvar o arquivo do EULA. Por exemplo, C:\ws8std_eula.rtf.  Use .rtf como extensão de nome de arquivo.  
  
    3.  Abra o local onde salvou o arquivo e clique duas vezes nele para abri-lo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transição para o Windows Server 2012 Standard  
 Depois de ter decidido fazer a transição do Windows Server Essentials para o Windows Server 2012 Standard, conclua estas duas etapas:  
  
1. Adquira uma licença para o Windows Server 2012 Standard e o número apropriado de licenças de acesso para cliente de usuário e/ou dispositivo para seu ambiente.  
  
    Você pode comprar uma licença do Windows Server 2012 Standard por meio de uma loja de varejo, um distribuidor ou com a ajuda de um [parceiro da Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se você comprou inicialmente o Windows Server 2012 Standard e preparava seus direitos de downgrade para instalar uma de suas duas instâncias virtuais como Windows Server Essentials, não é necessário comprar nada adicional.  
   >   
   >  Se você comprar o Windows Server 2012 Standard por meio do canal de licenciamento por volume, poderá baixar uma imagem ISO e uma chave do produto para o Windows Server 2012 Standard no centro de serviços de licenciamento por volume (VLSC).  
   >   
   >  Se você comprar o Windows Server 2012 Standard de todos os outros canais, o poderá baixar uma imagem ISO e uma chave de produto de avaliação para o Windows Server Essentials do [centro de avaliação do TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). A execução da transição descrita na etapa a seguir converterá o produto de avaliação em um produto com licença e suporte completos.  
  
2. Abra o Windows PowerShell como administrador e execute o comando a seguir.  
  
    **DISM/online/Set-Edition: ServerStandard/AcceptEula/ProductKey:** *Product Key*  
  
    Em que *chave do produto* é a chave do produto (Product Key) para sua cópia do Windows Server 2012 Standard.  
  
    O servidor é reiniciado para concluir o processo de transição.  
  
   Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 75 usuários e 75 dispositivos. Se você exceder qualquer um desses limites, deverá usar as ferramentas nativas padrão do Windows Server 2012 para gerenciar contas de usuário e dispositivos.  
  
   Além disso, depois de fazer a transição para o Windows Server 2012 Standard, os recursos de mídia do Windows Server Essentials não estão mais disponíveis. Isso inclui o Acesso via Web Remoto e as configurações de mídia no Painel.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Desativar os recursos do Windows Server Essentials  
 Se você não precisar mais do painel do Windows Server Essentials ou de outros recursos de adição de valor para gerenciar o servidor, poderá desativar os recursos e removê-los do servidor.  
  
 O **Assistente para desligar recursos do Windows Server Essentials** ajuda a desinstalar os recursos. Ele também limpa o servidor de arquivos que foram criados pelo software do servidor do Windows Server Essentials.  Algumas operações de limpeza são realizadas imediatamente, enquanto outras são iniciadas após a reinicialização do servidor.  
  
 O **Assistente para desligar recursos do Windows Server Essentials** requer que você desinstale manualmente todos os suplementos antes de poder concluir o assistente. Para exibir uma lista dos suplementos instalados, abra a página Aplicativo do Painel. O assistente o avisará se encontrar suplementos instalados e solicitará sua desinstalação.  
  
 O **Assistente para desligar recursos do Windows Server Essentials** permite que você escolha se deseja manter os arquivos de backup para computadores cliente depois de desativar os recursos do Windows Server Essentials.  
  
 Há duas maneiras de executar o **Assistente para desativar recursos do Windows Server Essentials** no painel:  
  
#### <a name="from-the-alert"></a>A partir do alerta  
  
1.  No Painel, abra o Visualizador de Alertas.  
  
2.  Na lista organizar, selecione o alerta que relata informações sobre como desativar os recursos do Windows Server Essentials após a transição.  
  
3.  No alerta, clique em **desativar recursos do Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>No painel de ajuda e suporte  
  
1. Na Home page, clique em Obter Ajuda e Suporte.  
  
2. Clique em **desativar o assistente de recursos do Windows Server Essentials**.  
  
   É possível que algumas tarefas executadas pelo desativar o **Assistente de recursos do Windows Server Essentials** não sejam concluídas com êxito. Em alguns casos, isso pode impedir a execução do Painel. Caso isso ocorra, você poderá iniciar o assistente manualmente executando o arquivo:  
  
   **%Systemdrive%\Arquivos de Programas\windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Consulte também  
  

-   [Transição para o Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transição para o Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

