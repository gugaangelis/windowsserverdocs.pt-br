---
title: "Adicione entradas para configurar, Suplementos, STATUS rápido e Links da Ajuda"
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
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Adicione entradas para configurar, Suplementos, STATUS rápido e Links da Ajuda

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar tarefas para o **instalação**, **ADD-INS**, **STATUS rápido** listas de tarefas e você pode adicionar links à seção Links da comunidade na home page do painel. Tarefas e os links são adicionados a essas listas e a seção colocando um arquivo XML chamado arquivo OEMHomePageContent.home ou um arquivo de recurso interno chamado OEMHomePageContent.dll %ProgramFiles%\Windows server\bin\addins\home.. O arquivo de recurso interno pode ser usado para localizar o texto nas tarefas e nos links que você adicionar. O arquivo. home contém as definições XML das tarefas e links.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Adicionando tarefas a SETUP, ADD-INS, listas de tarefas de STATUS rápido e adicionar links a tarefa de ajuda  
 Você pode adicionar tarefas para o **instalação**, **ADD-INS**, **STATUS rápido** tarefas listas e links para o **ajudar** tarefa, definindo as tarefas e links usando XML, opcionalmente criar o arquivo de recurso interno e instalar o arquivo no servidor. Se o arquivo XML estiver sendo instalado no servidor sem um arquivo de recurso, ele deverá ser chamado OEMHomePageContent.home. Se um assembly estiver sendo usado para instalar um arquivo XML e um arquivo de recurso, ele deverá ser chamado OEMHomePageContent.dll e deverá ser assinado com Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definir as tarefas e links  
 Você pode usar um editor de texto como o bloco de notas para criar o arquivo. Home ou, se você também estiver criando um arquivo de recurso interno, você pode usar o Visual Studio 2010 ou superior para definir os arquivos. O procedimento a seguir mostra como usar o Visual Studio 2010 ou superior para criar os arquivos.  
  
##### <a name="to-define-the-tasks-and-links"></a>Para definir as tarefas e links  
  
1.  Abra o Visual Studio 2010 ou superior como administrador clicando no programa no menu Iniciar e selecionando **executar como administrador**.  
  
2.  Clique em **arquivo**, clique em **nova**e clique em **projeto**.  
  
3.  No **modelos** painel, clique em **biblioteca de classes**, tipo **OEMHomePageContent** no **nome** caixa e clique em **Okey**.  
  
4.  Exclua o arquivo Class1.cs.  
  
5.  Clique com botão direito do novo projeto, clique em **adicionar**e clique em **Novo Item**.  
  
6.  No **modelos** painel, clique em **arquivo XML**, tipo **Oemhomepagecontent** no **nome** caixa e clique em **adicionar**.  
  
    > [!NOTE]
    >  Se o arquivo XML estiver sendo instalado sem um arquivo de recurso, ele deverá ser chamado OEMHomePageContent.home. Se estiver incluído em um assembly, ele pode ter qualquer nome, desde que ele tenha uma extensão. home.  
  
7.  Adicione o seguinte código XML ao arquivo OEMHomePageContent.home:  
  
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
    |Nome (tarefa)|O nome que é exibido para a tarefa na lista. Se você criar um arquivo de recurso interno, o valor desse atributo é o recurso de cadeia de caracteres.|  
    |Descrição (tarefa)|A descrição da tarefa. Se você criar um arquivo de recurso interno, o valor desse atributo é o recurso de cadeia de caracteres.|  
    |ID (tarefa)|O identificador da tarefa. Esse identificador deve ser um GUID. Criar um novo GUID para uma **exe** tarefa, mas para um **global** tarefa, você usa o GUID que você criou ao definir a tarefa para o painel de tarefas da subguia. Para obter mais informações sobre como criar um GUID, consulte [criar Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
    |imagem|Esse campo será ignorado.|  
    |Nome (ação)|Exibe o nome da tarefa.|  
    |Tipo (ação)|Descreve o tipo de tarefa. A tarefa pode uma das seguintes opções:- **global** tarefa, **exe**, ou uma tarefa de url. A **global** tarefa é a mesma tarefa global que você criou ao definir as tarefas para o painel de tarefas na subguia. Para obter mais informações sobre como criar uma tarefa global que pode ser usada em ambos os painel de tarefas da subguia e as listas de tarefas iniciais ou tarefas comuns da home page, consulte œCreating as classes de suporte? em œHow para: criar uma subguia? da [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648). Um **exe** tarefa pode ser usada para executar aplicativos de tarefas iniciais ou tarefas comuns de lista.|  
    |exelocation|O caminho para o aplicativo que está associado com a tarefa. Esse atributo é usado apenas para **exe** tarefas.|  
    |replaceid|O identificador da tarefa que é substituída por esta tarefa.|  
    |assembly|AssemblyName do assembly que fornece a classe para implementar a consulta de status rápido. O assembly precisa estar localizada no programa Files \ windows server\bin\\.|  
    |classe|O nome da classe que implementa a consulta de status rápido. A classe precisa implementar **ITaskStatusQuery** interface.|  
    |Título (link)|O texto que é exibido para o link. Se você criar um arquivo de recurso interno, o valor desse atributo é o recurso de cadeia de caracteres.|  
    |Descrição (link)|A descrição do destino do link. Se você criar um arquivo de recurso interno, o valor desse atributo é o recurso de cadeia de caracteres.|  
    |ShellExecPath|O caminho para o aplicativo ou a URL.<br /><br /> **Observação:** variáveis de ambiente têm suporte no atributo ShellExecPath.|  
  
     O exemplo de código a seguir mostra como definir um link para um aplicativo:  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     O exemplo de código a seguir mostra como definir um link para uma página da Web:  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  Altere os valores de atributo para representar a tarefa ou link.  
  
9. Em **Solution Explorer**, clique com botão direito **Oemhomepagecontent**e clique em **propriedades**.  No **propriedades** painel, em **Build Action**, selecione **recurso interno**.  
  
10. Salve o arquivo OEMHomePageContent.home.  
  
 Para saber como implementar uma consulta de status rápido, consulte documentos e exemplos no [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Alterar o status de uma tarefa SETUP/ADD-INS  
 As tarefas que são listadas em SETUP e ADD-INS podem ser alternadas nos Estados de concluída (configurada para Add-ins) e não concluída (não configurada para Add-ins).  
  
 Quando você define o aplicativo que está associado com sua nova tarefa, você pode usar o método SetTaskStatus do namespace taskstatushelper (incluído, mas não documentados no SDK do Windows Server Solutions) para alterar o status da tarefa. Por exemplo, você pode alterar a marca de seleção de cinza para verde chamando o método SetTaskStatus com o valor de enumeração TaskStatus (SetTaskStatus (id, TaskStatus), onde **id** é o identificador da tarefa). Os valores de enumeração que podem ser usados são TaskStatus,. Complete ou TaskStatus.  
  
##### <a name="replace-tasks"></a>Substituir tarefas  
 Você pode substituir as tarefas predefinidas nas tarefas iniciais ou as listas de tarefas comuns adicionando o GUID para a tarefa ao atributo replaceid da definição de tarefa. A tabela a seguir lista as tarefas e os identificadores correspondentes que podem ser substituídos no painel:  
  
|Nome da tarefa|Identificador|  
|---------------|----------------|  
|Obter atualizações para outros produtos da Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurar o Backup do servidor|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurar o acesso em qualquer lugar|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Notificação de alerta de email da instalação|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Adicionar contas de usuário|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Adicionar pastas de servidor|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Acesso em qualquer local - Status rápido|6093B462-1F04-4212-8804-9BC823070FAD|  
|Backup do servidor - Status rápido|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Pastas do servidor - Status rápido|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Contas de usuário ativas - Status rápido|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Email de notificação de alerta - Status rápido|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Computadores - Status rápido|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Opcional) Criar o arquivo de recurso  
 Se você quiser traduzir o texto nas tarefas que você adicionar tarefas iniciais, tarefas comuns e Links da comunidade, você deve criar um assembly que contém o arquivo. home e um. arquivo resx que define as cadeias de caracteres de texto.  
  
###### <a name="to-create-the-resource-file"></a>Para criar o arquivo de recurso  
  
1.  Clique com botão direito do projeto que você criou para suas tarefas, clique em **adicionar**e clique em **Novo Item**.  
  
2.  No **modelos** painel, clique em **arquivo de recursos**, tipo **Oemhomepagecontent** no **nome** caixa e clique em **adicionar**.  
  
    > [!NOTE]
    >  O arquivo de recursos pode ter qualquer nome, desde que ele tenha uma. resx extensão.  
  
3.  Para cada tarefa ou link adicionado, você deve adicionar cadeias de caracteres e valores ao arquivo OEMHomePageContent.home.resx que correspondem os elementos de tarefa e Link que são definidos no arquivo OEMHomePageContent.home. O exemplo de código a seguir mostra um exemplo de um arquivo Tasks.xml que está estruturado para o arquivo de recurso:  
  
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
    >  Os identificadores de atributos que são usados para localização não podem conter espaços.  
  
4.  Adicione os nomes dos recursos MinhaTarefa, Minhadescriçãodetarefa, Meunomedeação e Íconeparaação ao arquivo. resx com os valores apropriados.  
  
5.  Salve o arquivo OEMHomePageContent.home.resx e, em seguida, compile a solução.  
  
#####  <a name="BKMK_SignAssembly"></a>Assine o assembly com uma assinatura de Authenticode  
 Você deve assinar o assembly com Authenticode para ser usado no sistema operacional. Para obter mais informações sobre como assinar o assembly, consulte [assinando e verificando códigos com Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Instalar os arquivos de tarefas  
 Depois de criar o Home e. resx, você deve instalá-los no servidor.  
  
###### <a name="to-install-the-task-files"></a>Para instalar os arquivos de tarefas  
  
1.  Certifique-se de que sua solução é compilada sem erros.  
  
2.  Se você não tiver criado um arquivo de recurso interno, copie o arquivo OEMHomePageContent.home para **%ProgramFiles%\Windows Server\Bin\Addins\Home** no servidor. Se você criou um arquivo de recurso interno, copie o arquivo OEMHomePageContent.dll para **%ProgramFiles%\Windows Server\Bin\Addins\Home** no servidor.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)