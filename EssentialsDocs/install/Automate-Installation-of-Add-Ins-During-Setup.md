---
title: "Automatizar a instalação de suplementos durante a instalação"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automatizar a instalação de suplementos durante a instalação

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_AddIns"></a>Automatizar instalar suplementos durante a instalação  
 Para instalar suplementos durante a instalação, use o método PostIC.cmd descrito no [criar o arquivo PostIC.cmd para executar Post tarefas de configuração inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) seção deste documento.  
  
 Adicione a seguinte entrada ao PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 O suplemento agora dá suporte a etapas de pré-instalação e de desinstalação personalizada.  
  
 A etapa de pré-instalação é executada antes da instalação de todos os **. msi** arquivos especificados no suplemento. Quando executado no modo interativo, a caixa de diálogo de progresso será mostrada, mas sem alterar o progresso. O botão de cancelamento é desabilitado durante a fase de pré-instalação. Para implementar uma etapa de pré-instalação, adicione o seguinte conteúdo addin.xml (logo após Package):  
  
> [!NOTE]
>  O esquema xml deve seguir exatamente a mostrada abaixo:  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>  
<Executable>exefile</Executable>  
<NormalArgs>args-for-interactive-mode</NormalArgs>  
<SilentArgs>args-for-silent-mode</SilentArgs>  
<IgnoreExitCode>true</IgnoreExitCode>  
  </Preinstall>  
  <UninstallConfirm>...</UninstallConfirm>      
</Package>  
<¦>  
<¦>  
```  
  
 Wherein **exefile** é o arquivo executável no pacote de suplemento para realizar a etapa de pré-instalação e deve ser especificado. **NormalArgs** Especifica argumentos a serem passados para exefile no modo de linha de comando quando interativo é usado. Nesse modo, o exefile pode exibir algumas caixas de diálogo para interação do usuário. **SilentArgs** Especifica argumentos a serem passados para exefile no modo de linha de comando quando silencioso é usado (-q é especificado ao invocar installaddin.exe). O exefile não deverá exibir janelas nesse modo. Se **IgnoreExitCode** for especificado com verdadeiro, a etapa de pré-instalação sempre será considerada como bem-sucedida, caso contrário, o código de saída 0 indica êxito, 1 indicará cancelamento e outros valores indicarão falha. Marcas **NormalArgs**, **SilentArgs**, e **IgnoreExitCode** são todas opcionais.  
  
 Uma etapa de desinstalação personalizada pode ser usada para qualquer um destes procedimentos:  
  
-   Substitua a caixa de diálogo interna de confirmação.  
  
-   Popule caixas de diálogo personalizadas antes da desinstalação.  
  
-   Execute determinadas tarefas antes da desinstalação.  
  
 Para implementar uma etapa de desinstalação, adicione o seguinte conteúdo addin.xml (logo após Package):  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>¦</Preinstall>  
<UninstallConfirm>  
<Executable>full-path-to-exefile</Executable>  
<Arguments>command-line-arguments</Arguments>  
</UninstallConfirm>  
</Package>  
```  
  
 Wherein **caminho completo para exefile** Especifica o exefile já instalado no sistema. **Argumentos** é opcional e especifica os argumentos de linha de comando para o exefile. O exefile é invocado antes da interna de confirmação de desinstalação caixa de diálogo será exibida.  
  
 O exefile pode realizar as seguintes tarefas nessa fase:  
  
-   Algumas caixas de diálogo para interação do usuário será exibida.  
  
-   Realize algumas tarefas em segundo plano.  
  
 O código de saída desse arquivo exe determina como o processo de desinstalação avança:  
  
-   0: o processo de desinstalação continua sem popular a caixa de diálogo interna de confirmação, assim como o usuário já confirmou. (essa abordagem pode ser usada para substituir a caixa de diálogo interna de confirmação);  
  
-   1: o processo de desinstalação é cancelado e, finalmente uma mensagem de cancelamento será mostrada ao usuário. Tudo permanece inalterado;  
  
-   Outros: o processo de desinstalação continua com caixa de diálogo interna de confirmação, assim como a etapa de desinstalação personalizada não está presente.  
  
 Qualquer falha ao invocar o exefile causará o mesmo comportamento, pois o exefile retorna um código diferente de 0 ou 1.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)