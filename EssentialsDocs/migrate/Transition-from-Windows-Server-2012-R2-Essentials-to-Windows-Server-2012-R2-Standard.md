---
title: Transição do Windows Server Essentials para o Windows Server 2012 R2 Standard
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840077"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transição do Windows Server Essentials para o Windows Server 2012 R2 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 é o sistema operacional pronto para a nuvem que oferece suporte a suas cargas de trabalho atuais durante a introdução de novas tecnologias que facilitam a transição para a computação em nuvem. Conteúdo do Windows Server 2016 ajuda a prepará-lo.

 Windows Server Essentials oferece suporte a até 25 usuários e 50 dispositivos. Quando as necessidades de sua empresa excedem o limite, você pode realizar uma transição de licença in loco do Windows Server Essentials para Windows Server 2012 R2 Standard para manter a conformidade da licença.  
  
 Após a transição para o Windows Server 2012 R2 Standard, os limites de conta e dispositivos de usuário são removidos, mas os recursos que são exclusivos para o Windows Server Essentials (como o painel de acesso via Web remoto e backup do computador cliente) continuam disponíveis. Porém, por limitações técnicas, esses recursos oferecem suporte no máximo a 100 contas de usuário e 200 dispositivos. A funcionalidade de backup do computador cliente dará suporte ao backup de até 75 dispositivos.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requer uma licença de acesso para cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo de CAL e não vem com CALs. Quando a transição do Windows Server Essentials para Windows Server 2012 R2 Standard, você precisará comprar o número e tipo corretos de CALs para seu ambiente (a maioria dos clientes adquire CALs de usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de fazer a transição do Windows Server Essentials para Windows Server 2012 R2 Standard, você deve fazer o backup completo de dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, não é possível restaurar o servidor ao estado que estava antes da transição.  
  
-   Além disso, certifique-se de que você leia e entenda o contrato de licença de usuário final (EULA) do Windows Server 2012 R2 Standard. Para ler o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **DISM /online -/Set-Edition: ServerStandard /geteula:** *caminho do eula* (onde *caminho do eula* representa o local ao qual você deseja salvar o arquivo; por exemplo: C:\ws8std_eula.rtf). Use .rtf como extensão de nome de arquivo.  
  
    3.  Abra o local onde salvou o arquivo e clique duas vezes nele para abri-lo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transição para o Windows Server 2012 R2 Standard  
 Após decidir fazer a transição do Windows Server Essentials para Windows Server 2012 R2 Standard, completa essas duas etapas:  
  
1.  Compre uma licença para o Windows Server 2012 R2 Standard e o número apropriado de usuário e/ou licenças de acesso de cliente de dispositivo para o seu ambiente.  
  
     Você pode adquirir uma licença para o Windows Server 2012 R2 Standard em uma loja, um distribuidor ou com a Ajuda de um [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se você adquiriu o Windows Server 2012 R2 Standard inicialmente e utilizado seus direitos de downgrade para instalar uma das duas instâncias virtuais como Windows Server Essentials, não é necessário aquirir mais nada.  
    >   
    >  Se você comprar o Windows Server 2012 R2 Standard por meio do canal de licenciamento por Volume, você pode baixar uma imagem ISO e uma chave de produto para o Windows Server 2012 R2 Standard do Volume Licensing Service Center (VLSC).  
    >   
    >  Se você comprar o Windows Server 2012 R2 Standard em qualquer outro canal, você pode baixar uma imagem ISO e uma chave de produto de avaliação do Windows Server Essentials a partir de [Centro de avaliação TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). A execução da transição descrita na etapa a seguir converterá o produto de avaliação em um produto com licença e suporte completos.  
  
2.  Abra o Windows PowerShell como administrador e execute o comando a seguir:  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Chave do produto* (onde *chave do produto* é a chave do produto de sua cópia do Windows Server 2012 R2 Standard).  
  
     O servidor é reiniciado para concluir o processo de transição.  
  
 Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 100 usuários e 200 dispositivos.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

