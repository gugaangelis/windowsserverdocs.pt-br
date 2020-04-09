---
title: Preparando a Imagem para Implantação
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8ef4cb775f6d73f94a47d5d86ca235d459dd4b10
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819899"
---
# <a name="preparing-the-image-for-deployment"></a>Preparando a Imagem para Implantação

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Uma ferramenta típica para preparar uma imagem é sysprep.exe. Executar essa ferramenta generaliza a imagem e desliga o servidor, de modo que a Configuração Inicial executará quando o servidor que contém a imagem for reiniciado. Todas as modificações da imagem devem ser concluídas antes da execução do sysprep.exe.  
  
> [!NOTE]
>  Você pode redefinir a Ativação do Windows no máximo três vezes usando o sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Para preparar a imagem  
  
1.  Exclua o SkipIC.txt que você já adicionou.  
  
2.  Abra uma janela de prompt de comando elevado. Clique em **Iniciar**, clique com o botão direito do mouse em **Prompt de Comando** e selecione **Executar como Administrador**.  
  
3.  Execute o seguinte comando para redefinir a chave de registro, para que o usuário disponha do período de cortesia completo antes que o servidor torne-se não compatível.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Execute o seguinte comando para adicionar a chave de registro para exibir a chave, página de idioma, página de localidade e página de EULA. Por padrão, essas páginas não serão exibidas durante a configuração inicial. Assim, se você estiver liberando uma caixa pré-instalada, é preciso adicionar essa chave de registro. Porém, se você estiver liberando um DVD, não deve adicionar esta chave, pois essas páginas serão exibidas durante o WinPE e a configuração inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Desabilite a página de chave de configuração inicial se sua caixa estiver com a chave pré-inserida. A página de chave somente aparecerá quando ShowPreinstallPages = true e KeyPreInstalled != true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Execute o seguinte comando para adicionar a chave de registro se desejar desativar as verificações de requisitos de hardware. Isso é somente para sua caixa pré-instalada que não cumpre o requisito de hardware. Se estiver liberando um DVD, ou se sua caixa cumprir o requisito de hardware, recomenda-se não adicionar essa chave.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Opcional) Remova os logs sob **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Prepare o arquivo unattended.xml para o sysprep como mostra o modelo a seguir.  
  
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
  
9. Execute o seguinte comando para o sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  Você também pode adicionar unattend.xml sob %systemdrive%, em vez de como um parâmetro de sysprep. Se o arquivo estiver localizado em c:\ Ele será abordado pelas configurações do usuário, mas se for usado como parâmetro do Sysprep, ele não será coberto pelas configurações do usuário. O unattend.xml sob %systemdrive% será excluído sempre que o servidor reiniciar. Assim, garanta que depois de criar unattend.xml sob %systemdrive%, o servidor não seja reiniciado.  
  
10. Execute o seguinte comando para adicionar a chave de registro para ignorar a página de chave do OOBE do Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Execute o seguinte comando para adicionar a chave de registro para ignorar a página de seleção de idiomas do Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  É preciso realizar as duas últimas etapas, ou a página do Windows OOBE aparecerá, que é devida com a página de configuração inicial e interrompe um cenário de servidor administrado remotamente.  
  
12. Ao desligar a caixa após sysprep, é possível capturar uma imagem ou reiniciar o servidor para continuar a Configuração Inicial de um computador cliente.  
  
> [!IMPORTANT]
>  Os parceiros que estão planejando criar uma mídia de recuperação do servidor devem capturar a imagem e criar a mídia de recuperação antes de seguir para a próxima etapa.