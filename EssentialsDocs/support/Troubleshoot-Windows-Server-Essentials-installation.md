---
title: "Solucionar problemas de instalação do Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Solucionar problemas de instalação do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece a solução de problemas que ocorrem durante a instalação do Windows Server Essentials. Orientação é fornecida nas seguintes áreas:  
  

-   [Etapas de solução de problemas gerais](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Etapas de solução de problemas gerais](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Solucionar problemas de driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Para as informações de solução de problemas mais atuais da comunidade do Windows Server Essentials, sugerimos que você visitar o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). O fórum do Windows Server Essentials é um ótimo lugar para procurar ajuda ou fazer uma pergunta.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>Etapas de solução de problemas gerais  
 Se a instalação do Windows Server Essentials falhar, siga estas etapas para ajudar a identificar o problema que causou a falha.  
  
> [!IMPORTANT]
>  É importante não reiniciar manualmente o seu servidor durante a instalação do Windows Server Essentials. O servidor será reiniciado automaticamente várias vezes durante a instalação e configuração inicial. Se você reiniciou o servidor manualmente antes que você viu o **instalação bem-sucedida do servidor** message, que pode ter interrompeu a instalação e deixá-lo falhar.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Para identificar problemas em uma falha na instalação do Windows Server Essentials  
  
1.  Verifique se o hardware de servidor atenda aos requisitos mínimos. Para obter informações sobre requisitos de hardware, consulte [requisitos de sistema para o Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Se você recebeu o DVD de instalação do Windows Server Essentials do MSDN, verifique se o DVD é válido, verificando a soma SHA1. Para obter mais informações, consulte [disponibilidade e descrição do utilitário Verificador de integridade de soma de verificação de arquivo](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Verifique se o adaptador de rede no servidor é conectado a um roteador por um cabo de rede.  
  
4.  Se o servidor tem mais de um adaptador de rede, verifique se que apenas um adaptador de rede está habilitado.  
  
    > [!IMPORTANT]
    >  Não desconecte o cabo de rede ou reinicie o roteador durante a instalação do Windows Server Essentials.  
  
5.  Examine "implantação e instalação de servidor" em [documentação de lançamento para o Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Se você receber a mensagem de erro ocorreu um erro durante a configuração do servidor durante a instalação, use o DVD de recuperação de servidor e as instruções fornecidas pelo fabricante do seu hardware para restaurar o servidor para as configurações padrão de fábrica.  
  
##  <a name="BKMK_TroubleshootDrivers"></a>Solucionar problemas de driver  
 O problema mais comum ao instalar o Windows Server Essentials é controladores de armazenamento que precisam ter drivers instalados manualmente. O Windows inclui drivers para controladores de armazenamento muitos, mas ele não pode incluir drivers para seu hardware específico.  
  
 Você talvez precise instalar manualmente os drivers de placa de rede para o seu hardware específico.  
  
###  <a name="BKMK_StorageDrivers"></a>Adicionando drivers para controladores de armazenamento  
 Se seu hardware requer drivers de armazenamento que não estão incluídos no Windows Server Essentials, use as seguintes informações para concluir a instalação.  
  
 Se você vir a seguinte mensagem durante a instalação, você precisa adicionar manualmente os drivers para o controlador de armazenamento:  
  
 **Erro de instalação do Windows Server**  
  
 Unidade de disco rígido podem hospedar o Windows Server Essentials não foi encontrada. Você gostaria de carregar os drivers de armazenamento adicional?  
  
 Use o procedimento a seguir para instalar um driver de controlador de armazenamento.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Para instalar manualmente um driver de controlador de armazenamento  
  
1.  Encontre os drivers para o controlador de armazenamento. As informações são fornecidas pelo fabricante do hardware, e eles também podem estar disponíveis no site do fabricante.  
  
2.  Crie uma pasta chamada DRIVERS em um disquete ou unidade flash USB e, em seguida, copie os drivers para a pasta.  
  
3.  Conecte a unidade de disquete ou uma unidade flash USB com os drivers ao computador  
  
4.  Inicialize o computador a partir do DVD do Windows Server Essentials.  
  
     Se os drivers de controlador de armazenamento estão ausentes, será exibida a caixa de diálogo de erro de instalação do Windows Server Essentials.  
  
5.  Na caixa de diálogo de erro de instalação do Windows Server Essentials, clique em **Sim** para carregar os drivers de armazenamento adicional.  
  
6.  No **, selecione o arquivo inf do driver** solicitar, navegue até o arquivo. inf na pasta DRIVERS no disquete ou unidade flash USB, selecione o arquivo, clique com botão direito no nome do arquivo e clique em **abrir**. Isso carrega o driver.  
  
    > [!NOTE]
    >  Antes de tentar carregar o arquivo, verifique se a extensão de nome de arquivo (. inf) em letras minúsculas. Essa operação diferencia maiusculas de minúsculas, e um arquivo de driver não será carregado se a extensão de nome de arquivo tem letras maiusculas.  
  
7.  No prompt, clique em **Sim** para disponibilizar o driver de armazenamento durante a fase de modo de texto da instalação.  
  
 Instalação agora deve continuar normalmente.  
  
###  <a name="BKMK_AddingNICdrivers"></a>Adicionando drivers para adaptadores de rede  
 Se um adaptador de rede no computador não é compatível com o Windows Server Essentials, o servidor não terá conectividade de rede após a conclusão da instalação, e não será capaz de se conectar a computadores ao seu servidor.  
  
 No final da instalação do Windows Server Essentials, você será informado se um driver do adaptador de rede não foi instalado automaticamente. Você também pode usar **conexões de rede** no painel de controle para verificar se há um driver do adaptador de rede ausentes. Se você não vir uma conexão de rede associado com o adaptador de rede no servidor, você precisa instalar um driver.  
  
 Se o computador estiver faltando um driver com suporte para qualquer adaptador de rede, você precisa instalar manualmente o driver do adaptador de rede correto e, em seguida, reiniciar o servidor. Use o procedimento a seguir.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Para instalar um driver do adaptador de rede  
  
1.  Obter o driver ausente do fabricante do adaptador de rede.  
  
2.  Siga as instruções de instalação do fabricante para instalar o driver.  
  
3.  Reinicie o computador.  
  
    > [!IMPORTANT]
    >  Se você não reiniciar o servidor depois de instalar o driver do adaptador de rede ausentes, instalação do software do conector do Windows Server Essentials em computadores cliente poderá falhar.
