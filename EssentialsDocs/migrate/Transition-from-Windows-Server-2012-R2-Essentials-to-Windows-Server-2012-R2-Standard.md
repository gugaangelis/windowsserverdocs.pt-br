---
title: "Transição do Windows Server Essentials para Windows Server 2012 R2 Standard"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transição do Windows Server Essentials para Windows Server 2012 R2 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 é o sistema operacional prontos para a nuvem que dá suporte a suas cargas de trabalho atuais enquanto apresentando novas tecnologias que tornam mais fácil fazer a transição para a computação em nuvem. Windows Server 2016 conteúdo ajuda prepará-lo.

 Windows Server Essentials dá suporte a até 25 usuários e 50 dispositivos. Quando sua empresa precisa de exceder o limite, você pode executar uma transição de licença in-loco do Windows Server Essentials para Windows Server 2012 R2 Standard permaneça licença compatível.  
  
 Depois que você faça a transição para o Windows Server 2012 R2 Standard, os limites de conta e seus dispositivos do usuário são removidos, mas os recursos que são exclusivos para Windows Server Essentials (por exemplo, o painel, o acesso via Web remoto e o backup do computador cliente) permanecerão disponíveis. No entanto, as limitações técnicas para que esses recursos dão suporte a um máximo de 200 dispositivos e 100 contas de usuário. A funcionalidade de backup do computador cliente darão suporte a backup de até 75 dispositivos.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requer uma licença de acesso do cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo de CAL e não vem com qualquer CALs. Durante a transição do Windows Server Essentials para o Windows Server 2012 R2 Standard, você precisará comprar o número apropriado e o tipo de CALs para seu ambiente (a maioria dos clientes adquirir CALs de usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de fazer a transição do Windows Server Essentials para Windows Server 2012 R2 Standard, você deve fazer o backup completo os dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, você não poderá restaurar o servidor para o estado em que estava antes da transição.  
  
-   Além disso, certifique-se de que você leia e entenda o contrato de licença de usuário final (EULA) para Windows Server 2012 R2 Standard. Para exibir o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **DISM / online/definido-edition: ServerStandard /geteula:** *caminho de eula* (onde *caminho de eula* representa o local ao qual você deseja salvar o arquivo de EULA; por exemplo: C:\ws8std_eula.rtf). Certifique-se de usar. rtf como a extensão de nome de arquivo.  
  
    3.  Abra o local onde você salvou o arquivo e, em seguida, clique duas vezes no arquivo para abri-lo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transição para o Windows Server 2012 R2 Standard  
 Depois que você decidiu fazer a transição do Windows Server Essentials para Windows Server 2012 R2 Standard, completa essas duas etapas:  
  
1.  Compre uma licença para o Windows Server 2012 R2 Standard e o número apropriado de usuário e/ou as licenças de acesso do dispositivo cliente para seu ambiente.  
  
     Você pode adquirir uma licença para o Windows Server 2012 R2 Standard de uma tomada de varejo, um distribuidor, ou com a Ajuda de um [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se você adquiriu o Windows Server 2012 R2 Standard inicialmente e exercidos seus direitos de downgrade para instalar uma de suas duas instâncias virtuais como Windows Server Essentials, você não precisa adquirir algum outro item.  
    >   
    >  Se você comprar o Windows Server 2012 R2 Standard através do canal de licenciamento por Volume, você pode baixar uma imagem ISO e uma chave do produto para o Windows Server 2012 R2 Standard do Volume Licensing Service Center (VLSC).  
    >   
    >  Se você comprar o Windows Server 2012 R2 Standard de qualquer outro canal, você pode baixar uma imagem ISO e uma chave do produto de avaliação do Windows Server Essentials do [Centro de avaliação TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Realizar a transição, conforme descrito na próxima etapa converterá o produto de avaliação em um produto totalmente licenciado e com suporte.  
  
2.  Abra o Windows PowerShell como administrador e execute o seguinte comando:  
  
     **DISM / online/definido-edition: ServerStandard /accepteula /productkey:** *chave do produto* (onde *chave do produto* é a chave do produto para sua cópia do Windows Server 2012 R2 Standard).  
  
     O servidor for reiniciado para concluir o processo de transição.  
  
 Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 100 usuários e 200 dispositivos.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Migrar dados do servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar dados do servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

