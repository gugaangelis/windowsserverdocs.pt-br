---
title: Transição do Windows Server Essentials para o Windows Server 2012 R2 Standard
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f7914ff205382ed2c74cb130061f850e2c0675f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852289"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transição do Windows Server Essentials para o Windows Server 2012 R2 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O Windows Server 2016 é o sistema operacional pronto para a nuvem que dá suporte às suas cargas de trabalho atuais ao introduzir novas tecnologias que facilitam a transição para a computação em nuvem. O conteúdo do Windows Server 2016 ajuda você a se preparar.

 O Windows Server Essentials dá suporte a até 25 usuários e 50 dispositivos. Quando suas necessidades de negócios excederem o limite, você poderá executar uma transição de licença in-loco do Windows Server Essentials para o Windows Server 2012 R2 Standard para permanecer em conformidade com a licença.  
  
 Depois de fazer a transição para o Windows Server 2012 R2 Standard, os limites de conta de usuário e dispositivos são removidos, mas os recursos que são exclusivos do Windows Server Essentials (como o painel, Acesso via Web remota e backup do computador cliente) ainda permanecem disponíveis. Porém, por limitações técnicas, esses recursos oferecem suporte no máximo a 100 contas de usuário e 200 dispositivos. A funcionalidade de backup do computador cliente dará suporte ao backup de até 75 dispositivos.  
  
> [!IMPORTANT]
>   O Windows Server 2012 R2 Standard requer uma licença de acesso para cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo CAL e não vem com nenhuma Cal. Ao fazer a transição do Windows Server Essentials para o Windows Server 2012 R2 Standard, você precisará comprar o número e o tipo de CALs apropriados para o seu ambiente (a maioria dos clientes adquire as CALs do usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de passar do Windows Server Essentials para o Windows Server 2012 R2 Standard, você deve fazer backup completo dos dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, não é possível restaurar o servidor ao estado que estava antes da transição.  
  
-   Além disso, certifique-se de ler e entender o EULA (contrato de licença de usuário final) do Windows Server 2012 R2 Standard. Para ler o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **DISM/online/Set-Edition: ServerStandard/geteula:** *caminho do EULA* (em que o *caminho do EULA* representa o local no qual você deseja salvar o arquivo de eula; por exemplo: c:\ ws8std_eula. rtf). Use .rtf como extensão de nome de arquivo.  
  
    3.  Abra o local onde salvou o arquivo e clique duas vezes nele para abri-lo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transição para o Windows Server 2012 R2 Standard  
 Depois de ter decidido fazer a transição do Windows Server Essentials para o Windows Server 2012 R2 Standard, conclua estas duas etapas:  
  
1. Adquira uma licença para o Windows Server 2012 R2 Standard e o número apropriado de licenças de acesso para cliente de usuário e/ou dispositivo para seu ambiente.  
  
    Você pode comprar uma licença para o Windows Server 2012 R2 Standard por meio de uma loja, um distribuidor ou com a ajuda de um [parceiro da Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se você comprou o Windows Server 2012 R2 Standard inicialmente e preteve seus direitos de downgrade para instalar uma de suas duas instâncias virtuais como Windows Server Essentials, não é necessário comprar nada adicional.  
   >   
   >  Se você comprar o Windows Server 2012 R2 Standard por meio do canal de licenciamento por volume, poderá baixar uma imagem ISO e uma chave do produto (Product Key) do Windows Server 2012 R2 Standard no centro de serviços de licenciamento por volume (VLSC).  
   >   
   >  Se você comprar o Windows Server 2012 R2 Standard de qualquer outro canal, poderá baixar uma imagem ISO e uma chave de produto de avaliação para o Windows Server Essentials do [centro de avaliação do TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). A execução da transição descrita na etapa a seguir converterá o produto de avaliação em um produto com licença e suporte completos.  
  
2. Abra o Windows PowerShell como administrador e execute o comando a seguir:  
  
    **DISM/online/Set-Edition: ServerStandard/AcceptEula/ProductKey:** *chave do produto* (em que *chave do produto* é a chave do produto para sua cópia do Windows Server 2012 R2 Standard).  
  
    O servidor é reiniciado para concluir o processo de transição.  
  
   Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 100 usuários e 200 dispositivos.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

