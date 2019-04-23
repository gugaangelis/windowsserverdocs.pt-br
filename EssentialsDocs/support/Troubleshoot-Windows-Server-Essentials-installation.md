---
title: Solucionar problemas de instalação do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862017"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Solucionar problemas de instalação do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece uma solução de problemas para problemas que ocorrem durante a instalação do Windows Server Essentials. Serão fornecidas diretrizes nas áreas a seguir:  
  

-   [Etapas de solução de problemas gerais](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Etapas de solução de problemas gerais](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Para obter as informações mais recentes para solução de problemas da comunidade do Windows Server Essentials, sugerimos que você visite o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). O fórum do Windows Server Essentials é uma boa oportunidade para obter ajuda ou para fazer uma pergunta.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a> Etapas de solução de problemas gerais  
 Se a instalação do Windows Server Essentials falhar, siga estas etapas para ajudar a identificar o problema que causou a falha.  
  
> [!IMPORTANT]
>  É importante não reiniciar manualmente o seu servidor durante a instalação do Windows Server Essentials. O servidor é reiniciado automaticamente durante a instalação e a configuração inicial. Se você reiniciou o servidor manualmente antes de ver a mensagem **Instalação do servidor bem-sucedida**, isso pode ter interrompido a instalação e feito com que ela falhasse.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Para identificar problemas em uma falha na instalação do Windows Server Essentials  
  
1.  Garanta que o hardware de seu servidor cumpra os requisitos mínimos. Para obter informações sobre requisitos de hardware, consulte [requisitos de sistema para o Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Se você recebeu o DVD de instalação do Windows Server Essentials do MSDN, verifique se o DVD é válido, verificando a soma de SHA1. Para obter mais informações, consulte [disponibilidade e descrição do utilitário File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Verifique se o adaptador de rede no servidor está conectado a um roteador por um cabo de rede.  
  
4.  Se o computador tiver mais de um adaptador de rede, confirme que apenas um deles esteja habilitado.  
  
    > [!IMPORTANT]
    >  Não desconecte o cabo de rede nem reinicie o roteador durante a instalação do Windows Server Essentials.  
  
5.  Examine "instalação do servidor e implantação" em [Release Documentation for Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Se você receber a mensagem de erro ocorreu um erro enquanto a configuração do servidor durante a instalação, use o DVD de recuperação do servidor e as instruções fornecidas pelo fabricante do seu hardware para restaurar o servidor para configurações padrão de fábrica.  
  
##  <a name="BKMK_TroubleshootDrivers"></a> Solucionar problemas de driver  
 O problema mais comum ao instalar o Windows Server Essentials é armazenar controladores que precisam ter drivers instalados manualmente. O Windows inclui drivers para muitos controladores de armazenamento, mas talvez não inclua drivers para o hardware específico.  
  
 Também pode ser necessário instalar manualmente os drivers de placa de rede para seu hardware específico.  
  
###  <a name="BKMK_StorageDrivers"></a> Adicionando drivers para controladores de armazenamento  
 Se o seu hardware exige drivers de armazenamento que não estão incluídos no Windows Server Essentials, use as informações a seguir para concluir a instalação.  
  
 Se você vir a mensagem a seguir durante a instalação, é necessário que você adicione manualmente os drivers para seu controlador de armazenamento:  
  
 **Erro de instalação do Windows Server**  
  
 Unidade de disco rígido capaz de hospedar o Windows Server Essentials não foi encontrada. Deseja carregar drivers de armazenamento adicionais?  
  
 Use o procedimento a seguir para instalar um driver de controlador de armazenamento.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Para instalar manualmente um driver de controlador de armazenamento  
  
1.  Localize os drivers para seu controlador de armazenamento. Eles são fornecidos pelo fabricante do hardware e podem estar disponíveis também no site desse fabricante.  
  
2.  Crie uma pasta chamada DRIVERS em um disquete ou uma unidade flash USB e, em seguida, copie os drivers para a pasta.  
  
3.  Anexe a unidade de disquete ou unidade flash USB contendo os drivers ao computador.  
  
4.  Inicialize o computador a partir do DVD do Windows Server Essentials.  
  
     Se os drivers de controlador de armazenamento estiverem ausentes, a caixa de diálogo de erro de instalação do Windows Server Essentials será exibida.  
  
5.  Na caixa de diálogo de erro de instalação do Windows Server Essentials, clique em **Sim** para carregar os drivers de armazenamento adicional.  
  
6.  No prompt **Selecione o arquivo inf de seu driver** , navegue até o arquivo .inf na pasta DRIVERS em sua unidade de disquete ou unidade flash USB, selecione o arquivo, clique com o botão direito do mouse no nome do arquivo e, por fim, clique em **Abrir**. Isso carrega o driver.  
  
    > [!NOTE]
    >  Antes de tentar carregar o arquivo, verifique se a extensão de nome de arquivo (.inf) está em letras minúsculas. Essa operação diferencia maiúsculas de minúsculas, portanto, um arquivo não será carregado se a extensão de nome de arquivo estiver em letras maiúsculas.  
  
7.  No prompt, clique em **Sim** para tornar o driver de armazenamento disponível durante a fase de modo texto da instalação.  
  
 A instalação agora deve prosseguir normalmente.  
  
###  <a name="BKMK_AddingNICdrivers"></a> Adicionando drivers para adaptadores de rede  
 Se um adaptador de rede no computador não é suportado pelo Windows Server Essentials, o servidor não terá conectividade de rede após a conclusão da instalação, e você não poderá conectar computadores ao seu servidor.  
  
 No final da instalação do Windows Server Essentials, você será informado quando um driver de adaptador de rede não foi instalado automaticamente. Você também pode usar **Conexões de Rede**, no Painel de Controle, para verificar um driver de adaptador de rede ausente. Se você não vir uma conexão de rede associada ao adaptador de rede em seu servidor, será necessário que você instale um driver.  
  
 Se está faltando um driver com suporte para qualquer adaptador de rede no computador, é necessário que você instale manualmente o driver de adaptador de rede correto e, então, reinicie o servidor. Use o procedimento a seguir.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Para instalar um driver de adaptador de rede  
  
1.  Obtenha o driver faltante com o fabricante do adaptador de rede.  
  
2.  Siga as instruções de instalação do fabricante para instalar o driver.  
  
3.  Reinicie o computador.  
  
    > [!IMPORTANT]
    >  Se você não reiniciar o servidor depois de instalar o driver de adaptador de rede ausente, a instalação do software Windows Server Essentials connector nos computadores cliente poderá falhar.
