---
title: "Preparando a imagem para implantação"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>Preparando a imagem para implantação

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Uma ferramenta típica para preparar uma imagem é sysprep.exe. Executar esta ferramenta generaliza a imagem e desliga o servidor para que a configuração inicial será executado quando o servidor que contém a imagem for reiniciado. Todas as modificações feitas na imagem devem ser concluídas antes que você execute sysprep.exe.  
  
> [!NOTE]
>  Você pode redefinir a ativação do produto Windows um máximo de três vezes usando sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Para preparar a imagem  
  
1.  Excluir SkipIC.txt que você tem que você adicionou?  
  
2.  Abra uma janela de prompt de comando com privilégios elevados. Clique em **iniciar**, clique com botão direito **Prompt de comando**e, em seguida, selecione **executar como administrador**.  
  
3.  Execute o seguinte comando para redefinir a chave do registro para que o usuário terá o período de carência completo antes do servidor se torne incompatível.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Execute o seguinte comando para adicionar a chave do registro para exibir a chave, página de idioma, página de localidade e página de EULA. Por padrão essas páginas não exibirá durante a configuração inicial. Portanto, se você estiver lançando uma caixa pré-instalado, você precisa adicionar essa chave do registro. No entanto, se você estiver lançando um DVD, você não deve adicionar essa chave, como essas páginas exibirão durante o WinPE e configuração inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Desabilite a página de chave de configuração inicial se sua caixa é previamente inserida. A página de chave só mostrará quando ShowPreinstallPages = true e KeyPreInstalled! = true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Execute o seguinte comando para adicionar a chave do registro, se você quiser desabilitar as verificações de requisito de hardware. Isso é apenas para sua caixa pré-instalado que não atendem ao requisito de hardware. Se você estiver lançando um DVD ou, sua caixa atende ao requisito de hardware, é recomendável não adicionar essa chave.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Opcional) Remover os registros em **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Prepare o arquivo xml autônomo para sysprep conforme mostrado no modelo a seguir.  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. Execute o comando a seguir para sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  Você também pode adicionar o unattend.xml em % systemdrive %, em vez de como um parâmetro do sysprep. Se o arquivo está localizado em c:\ que ele será coberto por configurações de s do usuário, mas se usado como um parâmetro do sysprep, ele não será coberto por configurações de s do usuário. O unattend.xml em % systemdrive % será excluído sempre que o servidor for reiniciado. Portanto, certifique-se de que, depois de criar o unattend.xml em % systemdrive %, o servidor não for reiniciado.  
  
10. Execute o seguinte comando para adicionar a chave do registro para ignorar a página de chave de tela de apresentação do Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Execute o seguinte comando para adicionar a chave do registro para ignorar a página de seleção de idioma do Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  Você deve executar as 2 últimos etapas, senão a página de tela de apresentação do Windows aparecerá que ocorre com o cenário de servidor de administração remota de página e quebra de configuração inicial.  
  
12. Desligar a caixa após sysprep, você pode capturar uma imagem ou reiniciar o servidor para continuar a configuração inicial de um computador cliente.  
  
> [!IMPORTANT]
>  Os parceiros que planejam criar mídia de recuperação do servidor devem capturar a imagem e criar a mídia de recuperação antes de prosseguir para a próxima etapa.