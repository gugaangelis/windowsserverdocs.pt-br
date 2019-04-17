---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: "Apêndice B configurar o ambiente de teste"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Apêndice b: configurar o ambiente de teste

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as etapas para criar um laboratório prático para testar o controle de acesso dinâmico. As instruções devem ser seguidos sequencialmente porque há muitos componentes que têm dependências.  
  
## <a name="prerequisites"></a>Pré-requisitos  
**Requisitos de hardware e software**  
  
Requisitos para configurar o laboratório de teste:  
  
-   Um servidor host que executam o Windows Server 2008 R2 com SP1 e Hyper-V  
  
-   Uma cópia do ISO do Windows Server 2012  
  
-   Uma cópia do ISO do Windows 8  
  
-   Microsoft Office 2010  
  
-   Um servidor que executa o Microsoft Exchange Server 2003 ou posterior  
  
Você precisa para compilar as seguintes máquinas virtuais para testar os cenários de controle de acesso dinâmico:  
  
-   DC1 (controlador de domínio)  
  
-   DC2 (controlador de domínio)  
  
-   Arquivo1 (servidor de arquivos e Active Directory Rights Management Services)  
  
-   SRV1 (servidor POP3 e SMTP)  
  
-   CLIENT1 (computador cliente com o Microsoft Outlook)  
  
As senhas para as máquinas virtuais devem ser da seguinte maneira:  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\administrador:pass@word1  
  
-   Todas as outras contas:pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>Criar o teste de máquinas virtuais do laboratório  
  
### <a name="install-the-hyper-v-role"></a>Instale a função Hyper-V  
Você precisa instalar a função Hyper-V em um computador executando o Windows Server 2008 R2 com SP1.  
  
##### <a name="to-install-the-hyper-v-role"></a>Para instalar a função Hyper-V  
  
1.  Clique em **iniciar**e clique em Gerenciador do servidor.  
  
2.  Na área de resumo de funções da janela principal do Gerenciador do servidor, clique em **adicionar funções**.  
  
3.  Sobre o **selecionar funções de servidor** página, clique em **Hyper-V**.  
  
4.  Sobre o **criar redes virtuais** página, clique em um ou mais adaptadores de rede, se você quiser disponibilizar sua conexão de rede para máquinas virtuais.  
  
5.  Sobre o **confirmar seleções de instalação** página, clique em **instalar**.  
  
6.  O computador deve ser reiniciado para concluir a instalação. Clique em **fechar** para concluir o assistente e clique em **Sim** para reiniciar o computador.  
  
7.  Depois que você reiniciar o computador, entre com a mesma conta que você usou para instalar a função. Depois que o Assistente de configuração de retomada concluir a instalação, clique em **fechar** para concluir o assistente.  
  
### <a name="create-an-internal-virtual-network"></a>Criar uma rede virtual interna  
Agora, você criará uma rede virtual interna chamada ID_AD_Network.  
  
##### <a name="to-create-a-virtual-network"></a>Para criar uma rede virtual  
  
1.  Abra o Gerenciador do Hyper-V.  
  
2.  Do **ações** menu, clique em **Gerenciador de rede Virtual**.  
  
3.  Em **criar rede virtual**, selecione o **interna**.  
  
4.  Clique em **adicionar**. O **nova rede Virtual** página será exibida.  
  
5.  Tipo **ID_AD_Network** como o nome para a nova rede. Revise as outras propriedades e modificá-las, se necessário.  
  
6.  Clique em **Okey** para criar a rede virtual e fechar o Gerenciador de rede Virtual ou clique em **aplicar** para criar a rede virtual e continuar a usar o Gerenciador de rede Virtual.  
  
### <a name="BKMK_Build"></a>Criar o controlador de domínio  
Crie uma máquina virtual para ser usado como o controlador de domínio (DC1). Instale a máquina virtual usando o ISO do Windows Server 2012 e nomeie-o DC1.  
  
##### <a name="to-install-active-directory-domain-services"></a>Para instalar os serviços de domínio do Active Directory  
  
1.  Conectar-se a máquina virtual para o ID_AD_Network. Entre para o DC1 como administrador com a senha ** pass@word1 **.  
  
2.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**.  
  
3.  Sobre o **antes de começar** página, clique em **próxima**.  
  
4.  Sobre o **selecionar o tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado**e, em seguida, clique em **próxima**.  
  
5.  No **servidor de destino Select** página, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** página, clique em **Active Directory Domain Services**. No **assistente Adicionar funções e recursos** caixa de diálogo, clique em **adicionar recursos**e clique em **próxima**.  
  
7.  Sobre o **Selecione recursos** página, clique em **próxima**.  
  
8.  No **Active Directory Domain Services** página, examine as informações e, em seguida, clique em **próxima**.  
  
9. Sobre o **confirmar seleções de instalação** página, clique em **instalar**. A barra de progresso de instalação do recurso na página de resultados indica que a função está sendo instalada.  
  
10. Sobre o **resultados** página, verifique se a instalação foi bem-sucedida e clique em **fechar**. No Gerenciador do servidor, clique no ícone de aviso com um ponto de exclamação no canto superior direito da tela, em seguida **gerenciar**. Na lista de tarefas, clique no **promover esse servidor para um controlador de domínio** link.  
  
11. No **implantação configuração** página, clique em **adicionar uma nova floresta**, digite o nome do domínio raiz, **contoso.com**e clique em **próxima**.  
  
12. Sobre o **opções de controlador de domínio** página, selecione os níveis funcionais de domínio e floresta como Windows Server 2012, especifique a senha DSRM ** pass@word1 **e, em seguida, clique em **próxima**.  
  
13. Sobre o **DNS opções** página, clique em **próxima**.  
  
14. Sobre o **opções adicionais** página, clique em **próxima**.  
  
15. Sobre o **caminhos** página, digite os locais para o banco de dados do Active Directory, arquivos de log e SYSVOL pasta (ou aceitar locais padrão) e, em seguida, clique em **próxima**.  
  
16. Sobre o **opções de revisão** página, confirme suas seleções e clique em **próxima**.  
  
17. Sobre o **pré-requisitos verificar** página, confirme se a validação de pré-requisitos for concluído e, em seguida, clique em **instalar**.  
  
18. Sobre o **resultados** de página, verifique se o servidor com êxito foi configurado como um controlador de domínio e, em seguida, clique em **fechar**.  
  
19. Reinicie o servidor para concluir a instalação do AD DS. (Por padrão, isso ocorre automaticamente.)  
  
Crie os usuários a seguir usando o Centro Administrativo do Active Directory.  
  
##### <a name="create-users-and-groups-on-dc1"></a>Criar os usuários e grupos no DC1  
  
1.  Entrar em contoso.com como administrador. Inicie o Centro Administrativo do Active Directory.  
  
2.  Crie os seguintes grupos de segurança:  
  
    |Nome do grupo|Endereço de email|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  Crie a seguinte unidade organizacional (UO):  
  
    |Nome de UO|Computadores|  
    |-----------|-------------|  
    |FileServerOU|ARQUIVO1|  
  
4.  Crie os usuários a seguir com os atributos indicados:  
  
    |Usuário|Nome de usuário|Endereço de email|Departamento|Grupo|País/região|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|Finanças||NÓS|  
    |Milhas Reid|MReid|MReid@contoso.com|Finanças|FinanceAdmin|NÓS|  
    |Esther Valle|EValle|EValle@contoso.com|Operações|FinanceException|NÓS|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|HR||NÓS|  
    |Jeff baixa|JLow|JLow@contoso.com|HR||NÓS|  
    |Servidor RMS|RMS|rms@contoso.com||||  
  
    Para obter mais informações sobre a criação de grupos de segurança, consulte [criar um novo grupo](https://technet.microsoft.com/library/dd861305.aspx) no site do Windows Server.  
  
##### <a name="to-create-a-group-policy-object"></a>Para criar um objeto de política de grupo  
  
1.  Passe o cursor no canto superior direito da tela e clique no ícone de pesquisa. Na caixa de pesquisa, digite **gerenciamento de política de grupo**e clique em **Group Policy Management**.  
  
2.  Expanda **floresta: contoso.com**e, em seguida, expanda **domínios**, navegue até **contoso.com**, expanda **(contoso.com)**e, em seguida, selecione **FileServerOU**. Clique com botão direito **criar um GPO neste domínio e vinculá-lo aqui**
  
3.  Digite um nome descritivo para o GPO, como **FlexibleAccessGPO**e clique em **Okey**.  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Para habilitar o controle de acesso dinâmico para contoso.com  
  
1.  Abra o Console de gerenciamento de política de grupo, clique em **contoso.com**e clique duas vezes em **controladores de domínio**.  
  
2.  Clique com botão direito **política de controladores de domínio padrão**e selecione **editar**.  
  
3.  Na janela do Editor de gerenciamento de política de grupo, clique duas vezes em **configuração do computador**, clique duas vezes em **políticas**, clique duas vezes em **modelos administrativos**, clique duas vezes em **sistema**e clique duas vezes em **KDC**.  
  
4.  Clique duas vezes em **suporte KDC para declarações, autenticação composta e proteção Kerberos** e selecione a opção próximo ao **Enabled**. Você precisa habilitar essa configuração usar políticas de acesso Central.  
  
5.  Abra um prompt de comando com privilégios elevados e execute o seguinte comando:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>Criar o servidor de arquivos e o servidor de AD RMS (arquivo1)  
  
1.  Crie uma máquina virtual com o nome arquivo1 do Windows Server 2012 ISO.  
  
2.  Conectar-se a máquina virtual para o ID_AD_Network.  
  
3.  Participe da máquina virtual ao domínio contoso.com e, em seguida, entrar arquivo1 como contoso\administrator usando a senha ** pass@word1 **.  
  
#### <a name="install-file-services-resource-manager"></a>Instalar o Gerenciador de recursos de serviços de arquivos  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Para instalar a função Serviços de arquivo e o Gerenciador de recursos do servidor de arquivos  
  
1.  No Gerenciador do servidor, clique em **adicionar funções e recursos**.  
  
2.  Sobre o **antes de começar** página, clique em **próxima**.  
  
3.  Sobre o **selecionar o tipo de instalação** página, clique em **próxima**.  
  
4.  No **servidor de destino Select** página, clique em **próxima**.  
  
5.  Sobre o **selecionar funções de servidor** página, expanda **File and Storage Services**, marque a caixa de seleção ao lado de **arquivo e iSCSI serviços**, expanda e selecione **Gerenciador de recursos do servidor de arquivos**.  
  
    Na adição de funções e recursos do assistente, clique em **adicionar recursos**e clique em **próxima**.  
  
6.  Sobre o **Selecione recursos** página, clique em **próxima**.  
  
7.  Sobre o **confirmar seleções de instalação** página, clique em **instalar**.  
  
8.  Sobre o **progresso da instalação** página, clique em **fechar**.  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Instalar os pacotes de filtro do Microsoft Office no servidor de arquivos  
Você deve instalar os pacotes de filtro do Microsoft Office no Windows Server 2012 para habilitar IFilters para uma gama mais ampla de arquivos do Office que são fornecidos por padrão.  Windows Server 2012 não tem qualquer IFilters para arquivos do Microsoft Office instalado por padrão, e a infraestrutura de classificação de arquivo usa IFilters para executar a análise de conteúdo.  
  
Para baixar e instalar os IFilters, consulte [pacotes de filtro do Microsoft Office 2010](https://go.microsoft.com/fwlink/?LinkID=234122).  
  
#### <a name="configure-email-notifications-on-file1"></a>Configurar notificações de email em arquivo1  
Quando você cria cotas e telas de arquivo, você tem a opção de enviar notificações por email aos usuários quando seus limites de cota está se aproximando ou depois que eles tentaram salvar arquivos que foram bloqueados. Se você quiser sistematicamente notificar determinados administradores de cota e eventos de triagem de arquivo, você pode configurar um ou mais destinatários padrão. Para enviar essas notificações, você deve especificar o servidor a ser usado para encaminhar as mensagens de email SMTP.  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Para configurar as opções de email no Gerenciador de recursos de servidor de arquivos  
  
1.  Abra o Gerenciador de recursos do servidor de arquivos. Para abrir o Gerenciador de recursos do servidor de arquivos, clique em **iniciar**, tipo **Gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
2.  Na interface do Gerenciador de recursos do servidor de arquivos, clique com botão direito **Gerenciador de recursos do servidor de arquivos**e clique em **configurar opções**. O **opções do Gerenciador de recursos do servidor de arquivos** caixa de diálogo é aberta.  
  
3.  Sobre o **notificações por email** guia, em nome do servidor SMTP ou endereço IP, digite o nome de host ou o endereço IP do servidor SMTP que irá encaminhar as notificações de email.  
  
4.  Se você quiser notificar determinados administradores de cota sistematicamente ou eventos de triagem de arquivos em **administradores destinatários padrão**, digite cada endereço de email como fileadmin@contoso.com. Use o formato account@domaine use ponto e vírgula para separar várias contas.  
  
#### <a name="create-groups-on-file1"></a>Criar grupos em arquivo1  
  
###### <a name="to-create-security-groups-on-file1"></a>Para criar grupos de segurança em arquivo1  
  
1.  Entrar arquivo1 como Contoso\administrador, com a senha: ** pass@word1 **.  
  
2.  Adicionar NT AUTHORITY\Authenticated os usuários a **WinRMRemoteWMIUsers__** grupo.  
  
#### <a name="create-files-and-folders-on-file1"></a>Criar arquivos e pastas em arquivo1  
  
1.  Criar um novo volume NTFS em arquivo1 e, em seguida, crie a seguinte pasta: D:\Finance documentos.  
  
2.  Crie os seguintes arquivos com os detalhes especificados:  
  
    -   **Finanças Memo.docx**: adicionar algumas Finanças texto relacionados no documento. Por exemplo, ' as regras de negócios sobre quem pode acessar documentos de finanças mudaram. Documentos de finanças agora são acessados somente por membros do grupo FinanceExpert. Outros departamentos ou grupos têm acesso.' Você precisa avaliar o impacto dessa alteração antes de implementá-la no ambiente. Certifique-se de que este documento tem CONTOSO confidencial como rodapé em cada página.  
  
    -   **Solicitar para aprovação para Hire.docx**: criar um formulário neste documento que coleta informações do candidato. Você deve ter os campos a seguir no documento: **candidato nome, número de CPF, cargo, salário proposta, a partir do data, nome de Supervisor, departamento**. Adicione uma seção adicional no documento que possui um formulário para **Supervisor de assinatura, salário aprovados, confirmação de oferecer**, e **Status de oferecer**.   
        Verifique o documento-gerenciamento de direitos ativado.  
  
    -   **Word Document1.docx**: adicionar algum conteúdo de teste para esse documento.  
  
    -   **Word Document2.docx**: adicionar conteúdo a este documento de teste.  
  
    -   **Workbook1.xlsx**  
  
    -   **Workbook2.xlsx**  
  
    -   Crie uma pasta na área de trabalho chamada expressões regulares. Crie um documento de texto abaixo da pasta chamado **RegEx SSN**. Digite o seguinte conteúdo no arquivo e, em seguida, salve e feche o arquivo:   
        ^(?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! \d {4}$ 0000)  
  
3.  Compartilhe a pasta Documentos D:\Finance como documentos de finanças e permitir que todos têm ler e gravar o acesso para o compartilhamento.  
  
> [!NOTE]  
> Políticas de acesso central não estão habilitadas por padrão no sistema ou inicializar o volume c.  
  
#### <a name="BKMK_CS1"></a>Instalar o Active Directory Rights Management Services  
Adicione o Active Directory Rights Management Services (AD RMS) e todos os recursos necessários por meio do Gerenciador do servidor. Escolha todos os padrões.  
  
###### <a name="to-install-active-directory-rights-management-services"></a>Para instalar o Active Directory Rights Management Services  
  
1.  Entrar para o arquivo1 como Contoso\administrador ou como um membro do grupo Admins. do domínio.  
  
    > [!IMPORTANT]  
    > Para instalar a função de servidor de AD RMS o instalador conta (nesse caso, Contoso\administrador) precisará ter a associação ao grupo tanto o grupo local Administradores no computador servidor onde do AD RMS é a serem instalados, bem como a associação ao grupo Administradores de empresa no Active Directory.  
  
2.  No Gerenciador do servidor, clique em **adicionar funções e recursos**. Adição de funções e recursos de assistente aparece.  
  
3.  Sobre o **antes de começar** de tela, clique em **próxima**.  
  
4.  Sobre o **selecione tipo de instalação** de tela, clique em **função/recurso com base em instalar**e, em seguida, clique em **próxima**.  
  
5.  Sobre o **selecione destinos de servidor** de tela, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** de tela, marque a caixa ao lado de **Active Directory Rights Management Services**e, em seguida, clique em **próxima**.  
  
7.  No **adicionar recursos que são necessários para o Active Directory Rights Management Services? ** caixa de diálogo, clique em **adicionar recursos**.  
  
8.  Sobre o **selecionar funções de servidor** de tela, clique em **próxima**.  
  
9. Sobre o **Selecione recursos a serem instalados** de tela, clique em **próxima**.  
  
10. Sobre o **Active Directory Rights Management Services** tela, clique em Avançar.  
  
11. Sobre o **selecionar serviços de função** de tela, clique em **próxima**.  
  
12. Sobre o **função Web Server (IIS)** de tela, clique em **próxima**.  
  
13. Sobre o **selecionar serviços de função** de tela, clique em **próxima**.  
  
14. Sobre o **confirmar seleções de instalação** de tela, clique em **instalar**.  
  
15. Depois que a instalação for concluída, pela **progresso da instalação** de tela, clique em **configuração adicional executar**. O Assistente de configuração do AD RMS aparece.  
  
16. Sobre o **do AD RMS** de tela, clique em **próxima**.  
  
17. Sobre o **Cluster AD RMS** e selecione **criar um novo cluster raiz do AD RMS** e, em seguida, clique em **próxima**.  
  
18. No **banco de dados de configuração** de tela, clique em **usar Windows Internal Database nesse servidor**e clique em **próxima**.  
  
    > [!NOTE]  
    > Usar o banco de dados interno do Windows é recomendado para ambientes de teste apenas porque ele não dá suporte a mais de um servidor no cluster AD RMS. Implantações de produção devem usar um servidor de banco de dados separado.  
  
19. No **conta de serviço** de tela, em **conta de usuário de domínio**, clique em **especificar** e, em seguida, especifique o nome do usuário (**contoso\rms**) e a senha (**pass@word1**) e clique em **Okey**e clique em **próxima**.  
  
20. No **modo criptográfico** de tela, clique em **criptográfico 2 do modo**.  
  
21. Sobre o **armazenamento de chave de Cluster** de tela, clique em **próxima**.  
  
22. No **senha de chave de Cluster** tela, o **senha** e **Confirmar senha** caixas, digite ** pass@word1 **e, em seguida, clique em **próxima**.  
  
23. No **Cluster Web Site** de tela, certifique-se de que **Default Web Site** está selecionado e clique em **próxima**.  
  
24. No **endereço de Cluster** e selecione o **usar uma conexão não criptografada** opção, o **nome de domínio totalmente qualificado** , digite **FILE1.contoso.com**e clique em **próxima**.  
  
25. Sobre o **licenciante certificado nome** tela, aceite o nome padrão (**arquivo1**) na caixa de texto e clique **próxima**.  
  
26. No **registro SCP** e selecione **registrar SCP agora**e clique em **próxima**.  
  
27. Sobre o **confirmação** de tela, clique em **instalar**.  
  
28. Sobre o **resultados** de tela, clique em **fechar**e, em seguida, clique em **fechar** em **progresso da instalação** tela. Quando terminar, faça logoff e logon como contoso\rms usando a senha fornecida (**pass@word1**).  
  
29. Iniciar o console de AD RMS e navegar até **modelos de política de direitos**.  
  
    Para abrir o console do AD RMS, no Gerenciador do servidor, clique em **servidor Local** na árvore de console, clique em **ferramentas**e clique em **Active Directory Rights Management Services**.  
  
30. Clique no **Criar política de direitos distribuídos** modelo localizado no painel direito, clique em **adicionar**e selecione as seguintes informações:  
  
    -   Idioma: Em inglês dos EUA  
  
    -   Nome: Contoso Finanças Admin somente  
  
    -   Description: Contoso Finanças Admin somente  
  
    Clique em **adicionar**e clique em **próxima**.  
  
31. Na seção direitos e os usuários, clique em **usuários e direitos**, clique em **adicionar**, tipo ** financeadmin@contoso.com **e clique em **Okey**.  
  
32. Selecione **controle total**e deixar **conceder controle total do proprietário (autor) diretamente com nenhuma data de validade** selecionado.  
  
33. Porém clique nas guias restantes sem alterações e, em seguida, clique em **concluir**. Entre como Contoso\administrador.  
  
34. Vá para selecionar pasta, C:\inetpub\wwwroot\\_wmcs\certification, o arquivo ServerCertification e adicionar usuários autenticados para ter ler e gravar as permissões para o arquivo.  
  
35. Abra o Windows PowerShell e execute `Get-FsrmRmsTemplate`. Verifique se você pode ver o modelo de RMS que você criou nas etapas anteriores neste procedimento com esse comando.  
  
> [!IMPORTANT]  
> Se você quiser que seus servidores de arquivos para alterar imediatamente para que você possa testá-los, você precisa fazer o seguinte:  
>   
> 1.  No servidor de arquivos, arquivo1, abra um prompt de comando com privilégios elevados e execute os seguintes comandos:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  No controlador de domínio (DC1), replica o Active Directory.  
>   
>     Para obter mais informações sobre as etapas para forçar a replicação do Active Directory, consulte [replicação do Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
Opcionalmente, em vez de usar a adição de funções e recursos de assistente no Gerenciador do servidor, você pode usar o Windows PowerShell para instalar e configurar a função de servidor do AD RMS, como mostrado no procedimento a seguir.  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Instalar e configurar um cluster AD RMS no Windows Server 2012 usando o Windows PowerShell  
  
1.  Logon como CONTOSO\Administrator. com a senha: ** pass@word1 **.  
  
    > [!IMPORTANT]  
    > Para instalar a função de servidor de AD RMS o instalador conta (nesse caso, Contoso\administrador) precisará ter a associação ao grupo tanto o grupo local Administradores no computador servidor onde do AD RMS é a serem instalados, bem como a associação ao grupo Administradores de empresa no Active Directory.  
  
2.  Na área de trabalho do servidor, clique com botão direito no ícone do Windows PowerShell na barra de tarefas e selecione **executar como administrador** para abrir um prompt do Windows PowerShell com privilégios administrativos.  
  
3.  Para usar os cmdlets do Gerenciador do servidor para instalar a função de servidor de AD RMS, digite:  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  Crie a unidade do Windows PowerShell para representar o servidor de AD RMS que você está instalando.  
  
    Por exemplo, para criar uma unidade do Windows PowerShell chamada RC para instalar e configurar o primeiro servidor em um cluster raiz de AD RMS, digite:  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  Definir propriedades em objetos no namespace de unidade que representam as configurações necessárias.  
  
    Por exemplo, para definir a conta de serviço do AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    Quando aparece a caixa de diálogo de segurança do Windows, digite o nome de usuário de domínio da conta de serviço do AD RMS CONTOSO\RMS e a senha atribuída.  
  
    Em seguida, para atribuir a conta de serviço do AD RMS para as configurações de cluster de AD RMS, digite o seguinte:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    Em seguida, para definir o servidor de AD RMS para usar o banco de dados interno do Windows, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    Em seguida, para armazenar com segurança a senha da chave de cluster em uma variável, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    Digite a senha de chave de cluster e, em seguida, pressione a tecla ENTER.  
  
    Em seguida, para atribuir a senha para sua instalação do AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    Em seguida, para definir o endereço de cluster de AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    Em seguida, para atribuir o nome SLC para sua instalação do AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    Em seguida, para definir o ponto de conexão de serviço (SCP) para o cluster AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  Execute o **instalar ADRMS** cmdlet. Além de instalar a função de servidor de AD RMS e configurar o servidor, este cmdlet instala também outros recursos exigidos pelo AD RMS, se necessário.  
  
    Por exemplo, para mudar para a unidade do Windows PowerShell chamada RC e instalar e configurar o AD RMS, digite:  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    Digite "Y" quando o cmdlet solicita que você confirmar que você deseja iniciar a instalação.  
  
7.  Faça logoff como Contoso\administrador e log como CONTOSO\RMS usando a senha fornecida ("pass@word1").  
  
    > [!IMPORTANT]  
    > Para gerenciar o servidor de AD RMS a conta que você está conectado logon e usar para gerenciar o servidor (nesse caso, CONTOSO\RMS) precisa ser concedida participação em ambas do grupo Administradores local no computador do servidor de AD RMS, bem como membro do grupo Administradores de empresa no Active Directory.  
  
8.  Na área de trabalho do servidor, clique com botão direito no ícone do Windows PowerShell na barra de tarefas e selecione **executar como administrador** para abrir um prompt do Windows PowerShell com privilégios administrativos.  
  
9. Crie a unidade do Windows PowerShell para representar o servidor de AD RMS que você está configurando.  
  
    Por exemplo, para criar uma unidade do Windows PowerShell chamada RC para configurar o cluster raiz de AD RMS, digite:  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. Para criar o novo modelo de direitos para o administrador de finanças da Contoso e atribua a ele direitos de usuário com controle total sobre a instalação do AD RMS, no prompt de comando do Windows PowerShell, digite:  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. Para verificar que você pode ver o novo modelo de direitos para o administrador de finanças Contoso, no prompt de comando do Windows PowerShell:  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    Reveja a saída desse cmdlet para confirmar o modelo de RMS que você criou na etapa anterior está presente.  
  
### <a name="build-the-mail-server-srv1"></a>Criar o servidor de email (SRV1)  
SRV1 é o servidor de email POP3/SMTP. Você precisa configurá-lo para que você possa enviar notificações por email como parte do cenário de acesso negado assistência.  
  
Configure o Microsoft Exchange Server neste computador. Para obter mais informações, consulte [como instalar o servidor do Exchange](https://go.microsoft.com/fwlink/?prd=12364).  
  
### <a name="build-the-client-virtual-machine-client1"></a>Criar a máquina virtual de cliente (CLIENT1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>Para criar a máquina virtual de cliente  
  
1.  Conecte o CLIENT1 com o ID_AD_Network.  
  
2.  Instale o Microsoft Office 2010.  
  
3.  Entre como Contoso\administrador e use as seguintes informações para configurar o Microsoft Outlook.  
  
    -   Seu nome: administrador de arquivo  
  
    -   Endereço de email:fileadmin@contoso.com  
  
    -   Tipo de conta: POP3  
  
    -   Servidor de email de entrada: endereço IP estático do SRV1  
  
    -   Servidor de email de saída: endereço IP estático do SRV1  
  
    -   Nome de usuário:fileadmin@contoso.com  
  
    -   Lembre-se de senha: selecionar  
  
4.  Crie um atalho para o Outlook na área de trabalho Contoso\administrador.  
  
5.  Abra o Outlook e lidar com todas as mensagens de 'primeira vez iniciado'.  
  
6.  Exclua as mensagens de teste que foram geradas.  
  
7.  Crie um novo atalho na área de trabalho para todos os usuários a máquina virtual de cliente que aponta para \\\FILE1\Finance documentos.  
  
8.  Reinicialize conforme necessário.  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Habilitar o acesso negado assistência na máquina virtual cliente  
  
1.  Abra o Editor do registro e navegue até **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  
  
    -   Defina **EnableShellExecuteFileStreamCheck** para **1**.  
  
    -   Valor: DWORD  
  
## <a name="BKMK_CF"></a>Configuração de laboratório para a implantação declarações em um cenário de florestas  
  
### <a name="BKMK_2.1"></a>Criar uma máquina virtual para DC2  
  
-   Crie uma máquina virtual do Windows Server 2012 ISO.  
  
-   Crie o nome da máquina virtual como DC2.  
  
-   Conectar-se a máquina virtual para o ID_AD_Network.  
  
> [!IMPORTANT]  
> Ingressar em máquinas virtuais em um domínio e implantar os tipos de declaração entre florestas exigem que as máquinas virtuais seja capaz de resolver FQDNs dos domínios relevantes. Você terá que configurar manualmente as configurações de DNS nas máquinas virtuais para realizar essa tarefa. Para obter mais informações, consulte [Configurando uma rede virtual](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Todas as imagens de máquina virtual (clientes e servidores) devem ser reconfiguradas para usar um IP versão 4 (IPv4) estático endereço e as configurações de cliente do sistema de nome de domínio (DNS). Para obter mais informações, consulte [configurar um cliente DNS para o endereço IP estático](https://go.microsoft.com/fwlink/?LinkId=150952).  
  
### <a name="BKMK_2.2"></a>Configurar uma nova floresta chamado adatum.com  
  
##### <a name="to-install-active-directory-domain-services"></a>Para instalar os serviços de domínio do Active Directory  
  
1.  Conectar-se a máquina virtual para o ID_AD_Network. Entre para o DC2 como administrador com a senha ** Pass@word1 **.  
  
2.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**.  
  
3.  Sobre o **antes de começar** página, clique em **próxima**.  
  
4.  Sobre o **selecione tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado**e, em seguida, clique em **próxima**.  
  
5.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, clique nos nomes de servidor onde deseja instalar os serviços de domínio do Active Directory (AD DS) e, em seguida, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** página, clique em **Active Directory Domain Services**. No **assistente Adicionar funções e recursos** caixa de diálogo, clique em **adicionar recursos**e clique em **próxima**.  
  
7.  Sobre o **selecionar recursos** página, clique em **próxima**.  
  
8.  Sobre o **AD DS** página, examine as informações e, em seguida, clique em **próxima**.  
  
9. Sobre o **confirmação** página, clique em **instalar**. A barra de progresso de instalação do recurso na página de resultados indica que a função está sendo instalada.  
  
10. Sobre o **resultados** página, verifique se a instalação foi bem-sucedida e, em seguida, clique no ícone de aviso com um ponto de exclamação no canto superior direito da tela, lado **gerenciar**. Na lista de tarefas, clique no **promover esse servidor para um controlador de domínio** link.  
  
    > [!IMPORTANT]  
    > Se você fechar o Assistente de instalação neste ponto, em vez de clicar **promover esse servidor para um controlador de domínio**, você pode continuar a instalação do AD DS, clicando em **tarefas** no Gerenciador do servidor.  
  
11. No **implantação configuração** página, clique em **adicionar uma nova floresta**, digite o nome do domínio raiz, **adatum.com**e clique em **próxima**.  
  
12. Sobre o **opções de controlador de domínio** página, selecione os níveis funcionais de domínio e floresta como Windows Server 2012, especifique a senha DSRM ** pass@word1 **e, em seguida, clique em **próxima**.  
  
13. Sobre o **DNS opções** página, clique em **próxima**.  
  
14. Sobre o **opções adicionais** página, clique em **próxima**.  
  
15. Sobre o **caminhos** página, digite os locais para o banco de dados do Active Directory, arquivos de log e SYSVOL pasta (ou aceitar locais padrão) e, em seguida, clique em **próxima**.  
  
16. Sobre o **opções de revisão** página, confirme suas seleções e clique em **próxima**.  
  
17. Sobre o **pré-requisitos verificar** página, confirme se a validação de pré-requisitos for concluído e, em seguida, clique em **instalar**.  
  
18. Sobre o **resultados** de página, verifique se o servidor com êxito foi configurado como um controlador de domínio e, em seguida, clique em **fechar**.  
  
19. Reinicie o servidor para concluir a instalação do AD DS. (Por padrão, isso ocorre automaticamente.)  
  
> [!IMPORTANT]  
> Para garantir que a rede está configurada corretamente, depois que você configurou as duas as florestas, você deve fazer o seguinte:  
>   
> -   Entre para adatum.com como adatum\administrator. Abra uma janela de Prompt de comando, tipo **nslookup contoso.com**, e pressione ENTER.  
> -   Entrar contoso.com como Contoso\administrador. Abra uma janela de Prompt de comando, tipo **nslookup adatum.com**, e pressione ENTER.  
>   
> Se esses comandos executado sem erros, as florestas podem se comunicar uns com os outros. Para obter mais informações sobre erros de nslookup, consulte a seção solução de problemas no tópico [usando NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>Definir contoso.com como uma floresta confiante adatum.com  
Nesta etapa, você cria uma relação de confiança entre o site Adatum Corporation e o site de Contoso, Ltd..  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Para configurar Contoso como uma floresta confiante Adatum  
  
1.  Entrar no DC2 como administrador. Sobre o **iniciar** de tela, digite domain.msc.  
  
2.  Na árvore de console, clique com botão direito adatum.com e, em seguida, clique em Propriedades.  
  
3.  Sobre o **confie** , clique em **nova relação de confiança**e, em seguida, clique em **próxima**.  
  
4.  No **relação de confiança** página, digite **contoso.com**campo o nome no sistema de nome de domínio (DNS) e, em seguida, clique em **próxima**.  
  
5.  Sobre o **confiar em tipo** página, clique em **confiança de floresta**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **direção da relação de confiança** página, clique em **bidirecional**.  
  
7.  Sobre o **lados da relação de confiança** página, clique em **este domínio e o domínio especificado**e clique em **próxima**.  
  
8.  Continue a seguir as instruções no assistente.  
  
### <a name="BKMK_2.4"></a>Criar usuários adicionais na floresta Adatum  
Criar o usuário Jeff Low com a senha ** pass@word1 **e atribua o atributo da empresa com o valor **Adatum**.  
  
##### <a name="to-create-a-user-with-the-company-attribute"></a>Para criar um usuário com o atributo de empresa  
  
1.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e cole o seguinte código:  
  
    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  
  
    ```  
  
### <a name="BKMK_2.5"></a>Criar o tipo de declaração da empresa em adataum.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para criar um tipo de declaração usando o Windows PowerShell  
  
1.  Entrar no adatum.com como administrador.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e digite o seguinte código:  
  
    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  
  
    ```  
  
### <a name="BKMK_2.55"></a>Habilitar a propriedade de recursos da empresa em contoso.com  
  
##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Para habilitar a propriedade de recursos da empresa em contoso.com  
  
1.  Entrar contoso.com como administrador.  
  
2.  No Gerenciador do servidor, clique em **ferramentas**e clique em **Centro Administrativo do Active Directory**.  
  
3.  No painel à esquerda do Centro Administrativo do Active Directory, clique em **modo de exibição de árvore**. No painel esquerdo, clique em **controle de acesso dinâmico**e clique duas vezes em **propriedades do recurso**.  
  
4.  Selecione **empresa** do **propriedades do recurso** lista, clique com botão direito e selecione **propriedades**. No **valores sugeridos** seção, clique em **adicionar** para adicionar os valores sugeridos: Contoso e Adatum e, em seguida, clique em **Okey** duas vezes.  
  
5.  Selecione **empresa** do **propriedades do recurso** lista, clique com botão direito e selecione **habilitar**.  
  
### <a name="BKMK_2.6"></a>Habilitar o controle de acesso dinâmico em adatum.com  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Para habilitar o controle de acesso dinâmico para adatum.com  
  
1.  Entrar no adatum.com como administrador.  
  
2.  Abra o Console de gerenciamento de política de grupo, clique em **adatum.com**e clique duas vezes em **controladores de domínio**.  
  
3.  Clique com botão direito **política de controladores de domínio padrão**e selecione **editar**.  
  
4.  Na janela do Editor de gerenciamento de política de grupo, clique duas vezes em **configuração do computador**, clique duas vezes em **políticas**, clique duas vezes em **modelos administrativos**, clique duas vezes em **sistema**e clique duas vezes em **KDC**.  
  
5.  Clique duas vezes em **suporte KDC para declarações, autenticação composta e proteção Kerberos** e selecione a opção próximo ao **Enabled**. Você precisa habilitar essa configuração usar políticas de acesso Central.  
  
6.  Abra um prompt de comando com privilégios elevados e execute o seguinte comando:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>Criar o tipo de declaração da empresa em contoso.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para criar um tipo de declaração usando o Windows PowerShell  
  
1.  Entrar contoso.com como administrador.  
  
2.  Abrir um prompt de comando com privilégios elevados no Windows PowerShell, digite o seguinte código:  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>Criar a regra de acesso central  
  
##### <a name="to-create-a-central-access-rule"></a>Para criar uma regra de acesso central  
  
1.  No painel à esquerda do Centro Administrativo do Active Directory, clique em **modo de exibição de árvore**. No painel esquerdo, clique em **controle de acesso dinâmico**e clique em **regras de acesso Central**.  
  
2.  Clique com botão direito **regras de acesso Central**, clique em **nova**e depois **regras de acesso Central**.  
  
3.  No **nome** , digite **AdatumEmployeeAccessRule**.  
  
4.  No **permissões** seção, selecione o **execute os seguintes permissões como permissões atuais** opção, clique em **editar**e clique em **adicionar**. Clique no **selecionar uma entidade de segurança** vincular, digite **usuários autenticados**e clique em **Okey**.  
  
5.  No **entrada de permissão para permissões** caixa de diálogo, clique em **adicionar uma condição**e insira as seguintes condições: [**usuário**] [**empresa**] [**é igual a**] [**valor**] [**Adatum**]. Permissões devem ser **modificar, ler e executar, ler, gravar**.  
  
6.  Clique em **Okey**.  
  
7.  Clique em **Okey** três vezes para concluir e retornar ao centro administrativo do Active Directory.  
  
    ![guias de soluções](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
    O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>Criar a política de acesso central  
  
##### <a name="to-create-a-central-access-policy"></a>Para criar uma política de acesso central  
  
1.  Entrar contoso.com como administrador.  
  
2.  Abra um prompt de comando com privilégios elevados no Windows PowerShell e, em seguida, cole o seguinte código:  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>Publicar a nova política por meio da política de grupo  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Para aplicar a política de acesso central em servidores de arquivos por meio da política de grupo  
  
1.  No **iniciar** de tela, digite **ferramentas administrativas**e no **pesquisa** barra, clique em **configurações**. No **configurações** resultados, clique em **ferramentas administrativas**. Abra o Console de gerenciamento de política de grupo do **ferramentas administrativas** pasta.  
  
    > [!TIP]  
    > Se o **ferramentas administrativas Mostrar** configuração está desabilitada, a pasta de ferramentas administrativas e seu conteúdo não será exibida no **configurações** resultados.  
  
2.  Clique com botão direito do domínio contoso.com, clique em **criar um GPO neste domínio e vinculá-lo aqui**  
  
3.  Digite um nome descritivo para o GPO, como **AdatumAccessGPO**e clique em **Okey**.  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Para aplicar a política de acesso central para o servidor de arquivos por meio da política de grupo  
  
1.  No **iniciar** de tela, digite **Group Policy Management**, no **pesquisa** caixa. Abrir **Group Policy Management** da pasta de ferramentas administrativas.  
  
    > [!TIP]  
    > Se o **ferramentas administrativas Mostrar** configuração está desabilitada, a pasta de ferramentas administrativas e seu conteúdo não aparecerá nos resultados de configurações.  
  
2.  Navegue e selecione Contoso da seguinte forma: Management\Forest de política de grupo: contoso.com\Domains\contoso.com.  
  
3.  Clique com botão direito do **AdatumAccessGPO** política e selecione **editar**.  
  
4.  No Editor de gerenciamento de política de grupo, clique em **configuração do computador**, expanda **políticas**, expanda **configurações do Windows**e clique em **configurações de segurança**.  
  
5.  Expanda **sistema de arquivos**, clique com botão direito **política de acesso Central**e clique em **políticas de acesso Central gerenciar**.  
  
6.  No **configuração de políticas de acesso Central** caixa de diálogo, clique em **adicionar**, selecione **política de acesso apenas Adatum**e clique em **Okey**.  
  
7.  Feche o Editor de gerenciamento de política de grupo. Agora, você adicionou a política de acesso central a política de grupo.  
  
### <a name="BKMK_2.12"></a>Crie a pasta de lucros no servidor de arquivos  
Crie um novo volume NTFS em arquivo1 e crie a seguinte pasta: D:\Earnings.  
  
> [!NOTE]  
> Políticas de acesso central não estão habilitadas por padrão no sistema ou inicializar o volume c.  
  
### <a name="BKMK_2.13"></a>Definir a classificação e aplicar a política de acesso central na pasta lucros  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Para atribuir a política de acesso central no servidor de arquivos  
  
1.  No Gerenciador do Hyper-V, se conecte ao servidor Arquivo1. Entrar no servidor usando Contoso\administrador, com a senha ** pass@word1 **.  
  
2.  Abra um prompt de comando com privilégios elevados e digite: **gpupdate /force**. Isso garante que as alterações de política de grupo entrarão em vigor em seu servidor.  
  
3.  Você também precisa atualizar as propriedades do recurso Global do Active Directory. Abra o Windows PowerShell, tipo `Update-FSRMClassificationpropertyDefinition`, e pressione ENTER. Feche o Windows PowerShell.  
  
4.  Abra o Windows Explorer e navegue até D:\EARNINGS. Clique com botão direito do **lucros** pasta e clique em **propriedades**.  
  
5.  Clique no **Classification** guia **empresa**e, em seguida, selecione **Adatum** no **valor** campo.  
  
6.  Clique em **alteração**, selecione **política de acesso apenas Adatum** do menu suspenso e depois clique em **aplicar**.  
  
7.  Clique no **segurança**, clique em **avançado**e, em seguida, clique no **política Central** guia. Você deve ver o **AdatumEmployeeAccessRule** listados. Você pode expandir o item para exibir todas as permissões que você definiu quando você criou a regra no Active Directory.  
  
8.  Clique em **Okey** para retornar ao Windows Explorer.  
  


