---
title: "Adicionar uma guia às configurações"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>Adicionar uma guia às configurações

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar uma guia às configurações no painel criando e instalando um assembly de código que é usado pelo Gerenciador de configurações no sistema operacional.  
  
## <a name="add-a-tab-to-settings"></a>Adicionar uma guia às configurações  
 Adicionar uma guia Configurações, executando as seguintes tarefas:  
  
-   [Adicione uma implementação da interface ISettingsData ao assembly](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [Assine o assembly com uma assinatura de Authenticode](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Instale o assembly no computador de referência](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="BKMK_ISettingsData"></a>Adicione uma implementação da interface ISettingsData ao assembly  
 Interface ISettingsData está incluída no namespace Microsoft.WindowsServerSolutions.Settings do assembly AdminCommon.dll que está localizado na \Program Files\Windows Server\Bin.  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Para adicionar o código ISettingsData ao assembly  
  
1.  Abra o Visual Studio 2010 como um administrador clicando com o programa no **iniciar** menu e selecionando **executar como administrador**.  
  
2.  Clique em **arquivo**, clique em **nova**e clique em **projeto**.  
  
3.  No **novo projeto** caixa de diálogo, clique em **Visual c#**, clique em **biblioteca de classes**, insira **DashboardSettingsPage** para o nome para a solução e clique **Okey**.  
  
    > [!IMPORTANT]
    >  O assembly que está instalado no servidor deve ser nomeado DashboardSettingsPage.dll e, em seguida, copie a dll para %ProgramFiles%\Windows server\bin\OEM..  
  
4.  Crie o controle que você deseja usar na guia. Neste exemplo o controle de configurações é chamado de Meucontroledeconfigurações.  
  
5.  Renomeie o arquivo Class1.cs. Por exemplo, MySettingTab.cs.  
  
6.  Adicione uma referência ao arquivo AdminCommon.dll.  
  
7.  Adicione a seguinte instrução using:  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Altere o namespace e o cabeçalho de classe para coincidir com o exemplo a seguir:  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. Criar uma instância do controle que você criou para a guia. Por exemplo:  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. Adicione o construtor da classe. O exemplo de código a seguir mostra o construtor:  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Adicione o método Commit, que envia as alterações de configuração. O exemplo de código a seguir mostra o método Commit:  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Adicione o método TabControl, que identifica o controle para a guia. O exemplo de código a seguir mostra o método TabControl:  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Adicione o método TabId, que fornece um identificador exclusivo para a guia. O exemplo de código a seguir mostra o método TabId:  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Adicione o método TabOrder, que retorna a ordem da guia. O exemplo de código a seguir mostra o método TabOrder:  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  A ordem de tabulação é definida usando números a partir de 0. As guias de configurações internas da Microsoft são exibidas primeiro e, em seguida, as guias são exibidas com base na ordem de tabulação que você definir. Por exemplo, se você tiver três guias de configurações, especifique a ordem de tabulação como 0, 1 e 2, com base na ordem em que você deseja que as guias sejam exibidos.  
  
15. Adicione o método TabTitle, que fornece o título da guia. O exemplo de código a seguir mostra o método TabTitle:  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  O texto do título também pode vir de um arquivo de recurso para acomodar as necessidades de localização.  
  
16. Salve e compile a solução.  
  
###  <a name="BKMK_SignAssembly"></a>Assine o assembly com uma assinatura de Authenticode  
 Você deve assinar o assembly com Authenticode para ser usado no sistema operacional. Para obter mais informações sobre como assinar o assembly, consulte [assinando e verificando códigos com Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Instale o assembly no computador de referência  
 Depois de compilar com êxito a solução, coloque uma cópia do arquivo DashboardSettingsPage.dll na seguinte pasta no computador de referência:  
  
 **%ProgramFiles%\Windows server\bin\OEM.**  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)