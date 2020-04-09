---
title: Solucionar problemas de instalação do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6d48b9bed429f3dda49847b8d5a771ee61090cd5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852209"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Solucionar problemas de instalação do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece solução de problemas que ocorrem durante a instalação do Windows Server Essentials. Serão fornecidas diretrizes nas áreas a seguir:  
  

-   [Etapas gerais de solução de problemas](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Etapas gerais de solução de problemas](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Para obter as informações de solução de problemas mais recentes da Comunidade do Windows Server Essentials, sugerimos que você visite o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). O fórum do Windows Server Essentials é uma boa oportunidade para obter ajuda ou para fazer uma pergunta.  
  
##  <a name="general-troubleshooting-steps"></a><a name="BKMK_GeneralTroubleshootingSteps"></a>Etapas gerais de solução de problemas  
 Se a instalação do Windows Server Essentials falhar, siga estas etapas para ajudar a identificar o problema que causou a falha.  
  
> [!IMPORTANT]
>  É importante que você não reinicie o servidor manualmente durante a instalação do Windows Server Essentials. O servidor é reiniciado automaticamente durante a instalação e a configuração inicial. Se você reiniciou o servidor manualmente antes de ver a mensagem **Instalação do servidor bem-sucedida**, isso pode ter interrompido a instalação e feito com que ela falhasse.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Para identificar problemas em uma instalação com falha do Windows Server Essentials  
  
1.  Garanta que o hardware de seu servidor cumpra os requisitos mínimos. Para obter informações sobre os requisitos de hardware, consulte [requisitos de sistema do Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Se você recebeu o DVD de instalação do Windows Server Essentials do MSDN, verifique se o DVD é válido verificando a soma SHA1. Para obter mais informações, consulte [disponibilidade e descrição do utilitário verificador de integridade de soma de verificação de arquivo](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Verifique se o adaptador de rede no servidor está conectado a um roteador por um cabo de rede.  
  
4.  Se o computador tiver mais de um adaptador de rede, confirme que apenas um deles esteja habilitado.  
  
    > [!IMPORTANT]
    >  Não desconecte o cabo de rede nem reinicie o roteador durante a instalação do Windows Server Essentials.  
  
5.  Examine a "instalação e implantação do servidor" na [documentação de lançamento do Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Se você receber a mensagem de erro ocorreu um erro durante a configuração do servidor durante a instalação, use o DVD de recuperação do servidor e as instruções fornecidas pelo fabricante do hardware para restaurar o servidor para as configurações padrão de fábrica.  
  
##  <a name="troubleshoot-driver-issues"></a><a name="BKMK_TroubleshootDrivers"></a>Solucionar problemas de driver  
 O problema mais comum ao instalar o Windows Server Essentials são os controladores de armazenamento que precisam ter drivers instalados manualmente. O Windows inclui drivers para muitos controladores de armazenamento, mas talvez não inclua drivers para o hardware específico.  
  
 Também pode ser necessário instalar manualmente os drivers de placa de rede para seu hardware específico.  
  
###  <a name="adding-drivers-for-storage-controllers"></a><a name="BKMK_StorageDrivers"></a>Adicionando drivers para controladores de armazenamento  
 Se o hardware exigir drivers de armazenamento que não estão incluídos no Windows Server Essentials, use as informações a seguir para concluir a instalação.  
  
 Se você vir a mensagem a seguir durante a instalação, é necessário que você adicione manualmente os drivers para seu controlador de armazenamento:  
  
 **Erro de instalação do Windows Server**  
  
 A unidade de disco rígido capaz de hospedar o Windows Server Essentials não foi encontrada. Deseja carregar drivers de armazenamento adicionais?  
  
 Use o procedimento a seguir para instalar um driver de controlador de armazenamento.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Para instalar manualmente um driver de controlador de armazenamento  
  
1. Localize os drivers para seu controlador de armazenamento. Eles são fornecidos pelo fabricante do hardware e podem estar disponíveis também no site desse fabricante.  
  
2. Crie uma pasta chamada DRIVERS em um disquete ou uma unidade flash USB e, em seguida, copie os drivers para a pasta.  
  
3. Anexe a unidade de disquete ou unidade flash USB contendo os drivers ao computador.  
  
4. Inicialize o computador do DVD do Windows Server Essentials.  
  
    Se algum driver de controlador de armazenamento estiver ausente, a caixa de diálogo de erro de instalação do Windows Server Essentials será exibida.  
  
5. Na caixa de diálogo erro de instalação do Windows Server Essentials, clique em **Sim** para carregar os drivers de armazenamento adicionais.  
  
6. No prompt **Selecione o arquivo inf de seu driver**, navegue até o arquivo .inf na pasta DRIVERS em sua unidade de disquete ou unidade flash USB, selecione o arquivo, clique com o botão direito do mouse no nome do arquivo e, por fim, clique em **Abrir**. Isso carrega o driver.  
  
   > [!NOTE]
   >  Antes de tentar carregar o arquivo, verifique se a extensão de nome de arquivo (.inf) está em letras minúsculas. Essa operação diferencia maiúsculas de minúsculas, portanto, um arquivo não será carregado se a extensão de nome de arquivo estiver em letras maiúsculas.  
  
7. No prompt, clique em **Sim** para tornar o driver de armazenamento disponível durante a fase de modo texto da instalação.  
  
   A instalação agora deve prosseguir normalmente.  
  
###  <a name="adding-drivers-for-network-adapters"></a><a name="BKMK_AddingNICdrivers"></a>Adicionando drivers para adaptadores de rede  
 Se um adaptador de rede no computador não tiver suporte do Windows Server Essentials, o servidor não terá conectividade de rede após a conclusão da instalação e você não poderá conectar computadores ao seu servidor.  
  
 No final da instalação do Windows Server Essentials, você será informado se um driver de adaptador de rede não tiver sido instalado automaticamente. Você também pode usar **Conexões de Rede**, no Painel de Controle, para verificar um driver de adaptador de rede ausente. Se você não vir uma conexão de rede associada ao adaptador de rede em seu servidor, será necessário que você instale um driver.  
  
 Se está faltando um driver com suporte para qualquer adaptador de rede no computador, é necessário que você instale manualmente o driver de adaptador de rede correto e, então, reinicie o servidor. Use o procedimento a seguir.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Para instalar um driver de adaptador de rede  
  
1.  Obtenha o driver faltante com o fabricante do adaptador de rede.  
  
2.  Siga as instruções de instalação do fabricante para instalar o driver.  
  
3.  Reinicie o computador.  
  
    > [!IMPORTANT]
    >  Se você não reiniciar o servidor depois de instalar o driver de adaptador de rede ausente, a instalação do software do conector do Windows Server Essentials em seus computadores cliente poderá falhar.
