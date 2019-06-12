---
title: Adicione entradas para os links SETUP, ADD-INS, QUICK STATUS e HELP
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f0a66d0d36a3012369a9bc26c513dad069235ad8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433789"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Adicione entradas para os links SETUP, ADD-INS, QUICK STATUS e HELP

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

É possível adicionar tarefas às listas de tarefas **SETUP**, **ADD-INS**, **QUICK STATUS** , e adicionar links à seção Community Links na home page do Dashboard. Tarefas e links são adicionados a essas listas e seção com a colocação de um arquivo XML nomeado OEMHomePageContent.home ou de um arquivo de recurso inserido nomeado OEMHomePageContent.dll em %ProgramFiles%\Windows Server\Bin\Addins\Home. O arquivo de recurso embutido pode ser usado para localizar o texto nas tarefas e links que você adicionar. O arquivo .home contém as definições de XML das tarefas e links.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Adicionando tarefas às listas de tarefas SETUP, ADD-INS, QUICK STATUS e adicionando links à tarefa HELP  
 É possível adicionar tarefas às listas de tarefas **SETUP**, **ADD-INS**, **QUICK STATUS** e links à tarefa **HELP** definindo as tarefas e links usando XML, opcionalmente criando o arquivo de recurso integrado e instalando o arquivo no servidor. Se o arquivo XML estiver sendo instalado no servidor sem um arquivo de recurso, ele deve ser chamado OEMHomePageContent.home. Se um conjunto estiver sendo usado para instalar o arquivo XML e um arquivo de recurso, ele deve primeiro receber o nome de OEMHomePageContent.dll e ser assinado por Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definir as tarefas e os links  
 É possível usar um editor de texto, como o Bloco de Notas, para criar o arquivo .home ou, se um arquivo de recurso inserido estiver sendo criado, é possível usar o Visual Studio 2010 ou superior para definir os arquivos. O procedimento a seguir mostra como usar o Visual Studio 2010 ou superior para criar os arquivos.  
  
##### <a name="to-define-the-tasks-and-links"></a>Para definir as tarefas e os links  
  
1. Abra o Visual Studio 2010 ou superior como administrador clicando com o botão direito do mouse no programa no menu Iniciar e selecione **Executar como administrador**.  
  
2. Clique em **Arquivo**, em **Novo**e em **Projeto**.  
  
3. No painel **Modelos** , clique em **Biblioteca de Classes**, digite **OEMHomePageContent** na caixa **Nome** e clique em **OK**.  
  
4. Exclua o arquivo Class1.cs.  
  
5. Clique com o botão direito do mouse no novo projeto, clique em **Adicionar**e em **Novo Item**.  
  
6. No painel **Modelos**, clique em **Arquivo XML**, digite **OEMHomePageContent.home** na caixa de texto **Nome** e, em seguida, clique em **Adicionar**.  
  
   > [!NOTE]
   >  Se o arquivo XML estiver sendo instalado sem um arquivo de recurso, ele deve ser chamado OEMHomePageContent.home. Se estiver incluído em um assembly, pode receber qualquer nome, desde que tenha uma extensão .home.  
  
7. Adicione o seguinte código XML ao arquivo OEMHomePageContent.home:  
  
   ```  
  
   <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
      <SetupMyServerTasks>  
         <Task name="MyTask"  
            description="MyTaskDescription"  
            id="GUID">  
                 <Action   
                 name=?MyAction1Name?   
                 image=?IconForAction1?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                 <Action   
                 name=?MyAction2Name?   
                 image=?IconForAction2?  
                 type=?TaskType?  
                 exelocation=?ActionExeLocation? />  
                  ¦  
          </Task>  
                  ¦  
       </SetupMyServerTasks>  
   <MailServiceTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
   </MailServiceTasks>  
   <LineOfBusinessTasks>  
        <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
   <GetQuickStatusTasks>  
         <Task name="MyQuickStatusTask1"  
            description="MyQuickStatusTask1Desc   "  
            id="GUID"  
            assembly="AssemblyName of quick status query implementation"  
            class="ClassName of quick status query implementation"           
            replaceid="GUID"/>  
              <!--  Same schema as Actions in œSetupMyServerTasks? -->   
            </Task>  
   </GetQuickStatusTasks>  
      <Links>  
         <Link  
            ID=?GUID?  
            Title="Displayed text of the link"  
            Description="A very short description"  
            ShellExecPath="Path to the application or URL"/>  
      </Links>  
   </Tasks>  
   ```  
  
    Onde:  
  
   |Atributo|Descrição|  
   |---------------|-----------------|  
   |Name (Task)|O nome que é exibido para a tarefa na lista. Se você criar um arquivo de recurso inserido, o valor deste atributo será o recurso da cadeia de caracteres.|  
   |description (Task)|A descrição da tarefa. Se você criar um arquivo de recurso inserido, o valor deste atributo será o recurso da cadeia de caracteres.|  
   |id (Task)|O identificador da tarefa. Esse identificador deve ser um GUID. Você cria um novo GUID para uma tarefa **exe**, mas para uma tarefa **global**, você usa o GUID criado quando você definiu a tarefa para o painel de tarefas da subguia. Para obter mais informações sobre como criar uma GUID, consulte [Criar Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
   |image|Esse campo será ignorado.|  
   |Name (Action)|Exibe o nome da tarefa.|  
   |Tipo (Ação)|Descreve o tipo de tarefa. A tarefa pode ser uma das seguintes:- tarefa **global**, **exe** ou url. Uma tarefa **global** é a mesma tarefa global que você criou ao definir as tarefas para o painel de tarefas na subguia. Para obter mais informações sobre como criar uma tarefa global que pode ser usada no painel de tarefas da subguia e as listas Getting Started Tasks ou Common Tasks da home page, consulte œCreating as classes de suporte? em como: Criar uma subguia? dos [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648). Uma tarefa **exe** pode ser usada para executar aplicativos a partir das listas Tarefas Iniciais ou Tarefas Comuns.|  
   |exelocation|O caminho para o aplicativo associado à tarefa. Esse atributo é usado apenas para tarefas **exe** .|  
   |replaceid|O identificador da tarefa que será substituído por esta tarefa.|  
   |assembly|O AssemblyName do assembly que fornece a classe para implementar a consulta de status rápido. O assembly precisa estar localizado no programa de programas \ windows server\bin\\.|  
   |class|O nome da classe implementa uma consulta de status rápido. A classe precisa implementar a interface **ITaskStatusQuery**.|  
   |Title (link)|O texto que é exibido para o link. Se você criar um arquivo de recurso inserido, o valor deste atributo será o recurso da cadeia de caracteres.|  
   |Description (link)|A descrição do destino do link. Se você criar um arquivo de recurso inserido, o valor deste atributo será o recurso da cadeia de caracteres.|  
   |ShellExecPath|O caminho para o aplicativo ou para a URL.<br /><br /> **Observação:** As variáveis de ambiente têm suporte no atributo ShellExecPath.|  
  
    O seguinte exemplo de código mostra como definir um link para um aplicativo:  
  
   ```  
   <Links>  
      <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
   </Links>  
   ```  
  
    O seguinte exemplo de código mostra como definir um link para uma página da Web:  
  
   ```  
   <Links>  
      <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
   </Links>  
   ```  
  
8. Altere os valores de atributo para representar seu link ou tarefa.  
  
9. Em **Gerenciador de Soluções**, clique com o botão direito do mouse em **OEMHomePageContent.home** e depois clique em **Propriedades**.  No painel **Propriedades**, sob **Ação de Compilação**, selecione **Recurso Incorporado**.  
  
10. Salve o arquivo OEMHomePageContent.home.  
  
    Para saber como implantar uma consulta de status rápido, consulte documentos e amostras no [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Alterar o status de uma tarefa SETUP/ADD-INS  
 As tarefas que são listadas em SETUP e ADD-INS podem ser alternadas em estados de concluídas (configuradas para Complementos) e não concluídas (não configuradas para Complementos).  
  
 Ao definir o aplicativo que está associado à sua nova tarefa, você pode usar o método SetTaskStatus do namespace Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (incluído, mas não documentado no Windows Server Solutions SDK) para alterar o status da tarefa. Por exemplo, você pode alterar a marca de seleção de cinza para verde chamando o método SetTaskStatus com o valor de enumeração TaskStatus.Complete (SetTaskStatus [ID, TaskStatus.Complete], em que **ID** é o identificador da tarefa). Os valores de enumeração que podem ser usados são TaskStatus.Complete, TaskStatus.Incomplete ou TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Substituir tarefas  
 Você pode substituir as tarefas predefinidas nas listas Tarefas Iniciais ou Tarefas Comuns adicionando o GUID para a tarefa ao atributo replaceid da definição da tarefa. A seguinte tabela lista as tarefas e os identificadores correspondentes que podem ser substituídos no Dashboard:  
  
|Nome da tarefa|Identificador|  
|---------------|----------------|  
|Obter atualizações para outros produtos da Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurar Backup do Servidor|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurar Acesso em Qualquer Local|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Configurar notificação de alerta por email|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Adicionar contas de usuário|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Adicionar pastas de servidor|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Acesso em Qualquer Local - Status Rápido|6093B462-1F04-4212-8804-9BC823070FAD|  
|Backup do Servidor - Status Rápido|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Pastas do Servidor - Status Rápido|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Contas de usuário ativas - Status Rápido|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Notificação de alerta por email - Status Rápido|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Computadores - Status Rápido|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Opcional) Criar o arquivo de recurso  
 Se você desejar localizar o texto nas tarefas que você adicionar às Tarefas Iniciais, às Tarefas Comuns e aos Links da Comunidade, será necessário criar um assembly contendo o arquivo .home e um arquivo .home.resx que defina as cadeias de texto.  
  
###### <a name="to-create-the-resource-file"></a>Para criar o arquivo de recurso  
  
1.  Clique com o botão direito do mouse no projeto criado para suas tarefas, clique em **Adicionar**e em **Novo Item**.  
  
2.  No painel **Modelos**, clique em **Arquivo de Recurso**, digite **OEMHomePageContent.home.resx** na caixa de texto **Nome** e clique em **Adicionar**.  
  
    > [!NOTE]
    >  Pode ser atribuído qualquer nome ao arquivo de recurso, desde que tenha uma extensão .home.resx.  
  
3.  Para cada tarefa ou link que você adicionar, será necessário acrescentar cadeias de caracteres e valores ao arquivo OEMHomePageContent.home.resx que corresponda aos elementos Tarefa e Link definidos no arquivo OEMHomePageContent.home. O seguinte exemplo de código mostra um exemplo de um arquivo Tasks.xml estruturado para o arquivo de recurso:  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  Os identificadores dos atributos que são usados para localização não podem conter espaços.  
  
4.  Adicione os nomes de recursos MyTask, MyTaskDescription, MyActionName e IconForAction ao arquivo .resx com os valores adequados.  
  
5.  Salve o arquivo OEMHomePageContent.home.resx e, em seguida, crie a solução.  
  
#####  <a name="BKMK_SignAssembly"></a> Assinar o assembly com uma assinatura Authenticode  
 Você deve assinar com a Authenticode o assembly para que seja usado no sistema operacional. Para obter mais informações sobre a assinatura do assembly, consulte [Assinando e verificando códigos com Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Instalar os arquivos de tarefa  
 Depois de criar os arquivos .home e .home.resx, é necessário instalá-los no servidor.  
  
###### <a name="to-install-the-task-files"></a>Para instalar os arquivos de tarefa  
  
1.  Certifique-se de que sua solução seja criada sem erros.  
  
2.  Se você não tiver criado um arquivo de recurso inserido, copie o arquivo OEMHomePageContent.home em **%ProgramFiles%\Windows Server\Bin\Addins\Home** no servidor. Se você tiver criado um arquivo de recurso inserido, copie o arquivo OEMHomePageContent.dll em **%ProgramFiles%\Windows Server\Bin\Addins\Home** no servidor.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)