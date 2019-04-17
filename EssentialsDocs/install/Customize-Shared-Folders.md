---
title: Personalizar pastas compartilhadas
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>Personalizar pastas compartilhadas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por padrão, as pastas de servidor são criadas na maior partição de dados no disco 0. Os parceiros podem personalizar o local e especificar pastas de servidor adicionais usando as seguintes etapas:  
  
1.  Usando uma configuração de partição personalizado, crie a imagem de fábrica e, em seguida, crie uma nova chave de registro de armazenamento antes de você usar o sysprep. Durante a configuração inicial (IC), a tarefa de IC armazenamento verificará essa chave do registro. Se ele existir, as pastas de servidor padrão são criadas no diretório C:\ServerFolders.  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>Para criar uma nova chave de registro de armazenamento  
  
    1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
    2.  Na caixa de pesquisa, digite **regedit**e, em seguida, clique no **Regedit** aplicativo.  
  
    3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**e, em seguida, expanda **Microsoft **.  
  
    4.  Clique com botão direito **Windows Server**, clique em **nova**e clique em **chave **.  
  
    5.  Nomeie a chave **armazenamento **.  
  
    6.  No painel de navegação, clique com botão direito do novo armazenamento do registro chave, clique em **nova**e clique em **valor DWORD (32 bits)**.  
  
    7.  Nomeie a cadeia de caracteres **Icreatefoldersonsystem **.  
  
    8.  Clique com botão direito **Icreatefoldersonsystem**e clique em **modificar **. O **Editar cadeia de caracteres** caixa de diálogo aparece.  
  
    9. Defina o valor dessa nova chave para **1**e clique em **Okey **.  
  
2.  Use o script PostIC.cmd para mover as pastas para um local diferente ou criar pastas adicionais. Veja o exemplo a seguir: [exemplo 1: criar uma pasta personalizada e mover as pastas padrão para um novo local de PostIC.cmd usando o Windows PowerShell ](Customize-Shared-Folders.md#BKMK_Example1).  
  
3.  Use o SDK do Windows Server Solutions para mover as pastas para um local diferente ou para criar pastas adicionais. Veja o exemplo a seguir: [exemplo 2: criar uma pasta personalizada e mover uma pasta existente usando o SDK do Windows Server Solutions ](Customize-Shared-Folders.md#BKMK_Example2).  
  
 Opcionalmente, os parceiros podem deixar as pastas de dados na unidade C. Isso permite que o usuário final ou o revendedor Determine o layout das pastas de dados nas unidades de dados.  
  
###  <a name="BKMK_Example1"></a>Exemplo 1: Criar uma pasta personalizada e mover as pastas padrão para um novo local de PostIC.cmd usando o Windows PowerShell  
  
1.  Crie um arquivo PostIC.cmd para executar tarefas pós-configuração inicial conforme detalhado no [criar o arquivo PostIC.cmd para executar a postar das tarefas de configuração inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) seção.  
  
2.  Usando o bloco de notas, crie um arquivo chamado **customizefolders.ps1** na pasta C:\Windows\Setup\Scripts e cole o seguinte Windows PowerShell® comandos no arquivo (desmarque as linhas apropriadas dependendo do comportamento desejado).  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  Adicione a seguinte linha no arquivo PostIC.cmd para executar esse script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>Exemplo 2: Criar uma pasta personalizada e mover uma pasta existente usando o SDK do Windows Server Solutions  
 O código que você cria pode ser compilado como um executável e chamado do arquivo PostIC.cmd ou chamado diretamente de um suplemento instalado.  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)