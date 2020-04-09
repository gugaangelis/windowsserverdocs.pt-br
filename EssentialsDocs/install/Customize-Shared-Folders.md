---
title: Personalizar pastas compartilhadas
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 861107035408fc39d0dc5e4d94a4d82d8dfba74e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818069"
---
# <a name="customize-shared-folders"></a>Personalizar pastas compartilhadas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por padrão, as pastas do servidor são criadas na maior partição de dados no Disco 0. Os parceiros podem personalizar o local e especificar pastas adicionais do servidor usando as seguintes etapas:  
  
1. Usando uma configuração de partição personalizada, crie a imagem de fábrica e, em seguida, uma nova chave do Registro de armazenamento antes de usar o sysprep. Durante a IC (Configuração Inicial), a tarefa da IC de armazenamento verifica essa chave do Registro. Se existir, as pastas do servidor padrão são criadas no diretório C:\ServerFolders.  
  
   #### <a name="to-create-a-new-storage-registry-key"></a>Para criar uma nova chave do Registro de armazenamento  
  
   1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.  
  
   2.  Na caixa de Pesquisa, digite **regedit**e, em seguida, clique no aplicativo **Regedit** .  
  
   3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE** e **Microsoft**.  
  
   4.  Clique com o botão direito do mouse em **Windows Server**, clique em **Novo** e em **Chave**.  
  
   5.  Dê à chave o nome **Armazenamento**.  
  
   6.  No painel de navegação, clique com o botão direito do mouse na chave do Registro de armazenamento, clique em **Novo** e no **Valor DWORD (32 bits)** .  
  
   7.  Atribua o nome **CreateFoldersOnSystem** à cadeia de caracteres.  
  
   8.  Clique com o botão direito do mouse em **CreateFoldersOnSystem** e clique em **Modificar**. A caixa de diálogo **Editar Cadeia de Caracteres** é exibida.  
  
   9. Defina o valor desta chave nova como **1** e clique em **OK**.  
  
2. Use o script PostIC.cmd para mover as pastas para um local diferente ou para criar pastas adicionais. Consulte o seguinte exemplo: [Exemplo 1: Criar uma pasta personalizada e mover as pastas padrão para um novo local de PostIC.cmd usando o Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3. Use o SDK do Windows Server Solutions para mover as pastas para um local diferente ou para criar pastas adicionais. Consulte o seguinte exemplo: [Exemplo 2: Criar uma pasta padrão e mover uma pasta existente usando o SDK do Windows Server Solutions](Customize-Shared-Folders.md#BKMK_Example2).  
  
   Opcionalmente, os parceiros podem deixar as pastas de dados na unidade C. Isso permite ao usuário final ou revendedor determinar o layout das pastas de dados nas unidades de dados.  
  
###  <a name="example-1-create-a-custom-folder-and-move-the-default-folders-to-a-new-location-from-posticcmd-by-using-windows-powershell"></a><a name="BKMK_Example1"></a>Exemplo 1: criar uma pasta personalizada e mover as pastas padrão para um novo local do postal. cmd usando o Windows PowerShell  
  
1.  crie um arquivo PostIC.cmd para executar tarefas após a configuração inicial, como detalhado na seção [Criar o Arquivo PostIC.cmd para Execução de Tarefas após a Configuração Inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md).  
  
2.  Usando o bloco de notas, crie um arquivo chamado **customizefolders. ps1** na pasta C:\Windows\Setup\Scripts e cole os seguintes comandos do Windows PowerShell&reg; no arquivo (desmarque as linhas apropriadas dependendo do comportamento desejado).  
  
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
  
3.  Adicione a seguinte linha ao arquivo PostIC.cmd para executar o script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to default  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="example-2-create-a-custom-folder-and-move-an-existing-folder-by-using-the-windows-server-solutions-sdk"></a><a name="BKMK_Example2"></a>Exemplo 2: criar uma pasta personalizada e mover uma pasta existente usando o SDK de soluções do Windows Server  
 O código criado pode ser compilado como um executável e chamado pelo arquivo PostIC.cmd ou diretamente de um suplemento instalado.  
  
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
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)
