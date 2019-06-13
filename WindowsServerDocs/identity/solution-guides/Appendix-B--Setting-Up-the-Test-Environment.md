---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: Apêndice B Configurando o ambiente de teste
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3ebe125ce7850797d786e7b564c98889cfb19927
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445863"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Apêndice B: configuração do ambiente de teste

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as etapas para criar um laboratório prático para testar o Controle de Acesso Dinâmico. As instruções devem ser seguidas na ordem apresentada, pois muitos componentes dependem de outros.  

## <a name="prerequisites"></a>Pré-requisitos  
**Requisitos de hardware e software**  

Requisitos de configuração do laboratório de teste:  

-   Um servidor host executando o Windows Server 2008 R2 com SP1 e Hyper-V  

-   Uma cópia do ISO do Windows Server 2012  

-   Uma cópia do Windows 8 ISO  

-   Microsoft Office 2010  

-   Um servidor executando Microsoft Exchange Server 2003 ou posterior  

Você precisará criar as seguintes máquinas virtuais para testar os cenários de Controle de Acesso Dinâmico:  

-   DC1 (controlador de domínio)  

-   DC2 (controlador de domínio)  

-   FILE1 (servidor de arquivos e Active Directory Rights Management Services)  

-   SRV1 (servidor POP3 e SMTP)  

-   CLIENT1 (computador cliente com Microsoft Outlook)  

As senhas das máquinas virtuais deverão ser as seguintes:  

-   BUILTIN\Administradores: pass@word1  

-   Contoso\administrador: pass@word1  

-   Todas as outras contas: pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>Criar as máquinas virtuais do laboratório de teste  

### <a name="install-the-hyper-v-role"></a>Instalar a função Hyper-V  
É necessário instalar a função Hyper-V em um computador que executa o Windows Server 2008 R2 com SP1.  

##### <a name="to-install-the-hyper-v-role"></a>Para instalar a função Hyper-V  

1.  Clique em **Iniciar**e, em seguida, em Gerenciador de Servidores.  

2.  Na área Resumo das Funções da janela principal do Gerenciador do Servidor, clique em **Adicionar Funções**.  

3.  Na página **Selecionar Funções do Servidor**, clique em **Hyper-V**.  

4.  Na página **Criar Redes Virtuais** , clique em um ou mais adaptadores de rede se desejar disponibilizar sua conexão de rede para as máquinas virtuais.  

5.  Na página **Confirmar Seleções de Instalação**, clique em **Instalar**.  

6.  O computador deverá ser reinicializado para concluir a instalação. Clique em **Fechar** para encerrar o assistente e em **Sim** para reiniciar o computador.  

7.  Depois de reiniciar o computador, entre com a mesma conta usada para instalar a função. Depois que o Assistente para Continuar Configuração concluir a instalação, clique em **Fechar** para encerrar o assistente.  

### <a name="create-an-internal-virtual-network"></a>Criar uma rede virtual interna  
Agora você criará uma rede virtual interna chamada ID_AD_Network.  

##### <a name="to-create-a-virtual-network"></a>Para criar uma rede virtual  

1.  Abra o Gerenciador Hyper-V.  

2.  No menu **Ações**, clique em **Gerenciador de Rede Virtual**.  

3.  Em **Criar rede virtual**, selecione **Interna**.  

4.  Clique em **Adicionar**. A página **Nova Rede Virtual** é exibida.  

5.  Digite **ID_AD_Network** para o nome da nova rede. Analise as outras propriedades e modifique-as se necessário.  

6.  Clique em **OK** para criar a rede virtual e feche o Gerenciador de Rede Virtual, ou clique em **Aplicar** para criar a rede virtual e continuar a usar o Gerenciador de Rede Virtual.  

### <a name="BKMK_Build"></a>Criar o controlador de domínio  
Crie uma máquina virtual para ser usada como o controlador de domínio (DC1). Instale a máquina virtual usando o Windows Server 2012 ISO e nomeie-a como DC1.  

##### <a name="to-install-active-directory-domain-services"></a>Para instalar os Serviços de Domínio Active Directory  

1. Conecte a máquina virtual à ID_AD_Network. Entre no DC1 como administrador com a senha <strong>pass@word1</strong>.  

2. No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**.  

3. Na página **Antes de começar** , clique em **Avançar**.  

4. Na página **Selecionar tipo de instalação** , clique em **Instalação baseada em função ou recurso**e depois em **Avançar**.  

5. Na página **Selecionar servidor de destino**, clique em **Avançar**.  

6. Na página **Selecionar funções de servidor**, clique em **Serviços de Domínio Active Directory**. Na caixa de diálogo **Assistente de Adição de Funções e Recursos** , clique em **Adicionar Recursos**e em **Avançar**.  

7. Na página **Selecionar recursos**, clique em **Avançar**.  

8. Na página **Serviços de Domínio Active Directory** , revise as informações e clique em **Avançar**.  

9. Na página **Confirmar seleções de instalação** , clique em **Instalar**. A barra de progresso de instalação do recurso na página Resultados indica que a função está sendo instalada.  

10. Na página **Resultados** , confirme se a instalação foi concluída com êxito e clique em **Fechar**. No Gerenciador do Servidor, clique no ícone de aviso com o ponto de exclamação no canto superior direito da tela, ao lado de **Gerenciar**. Na lista de Tarefas, clique no link **Promover este servidor a um controlador de domínio**.  

11. Na página **Configuração de implantação**, clique em **Adicionar nova floresta**, digite o nome do domínio raiz **contoso.com** e clique em **Avançar**.  

12. Sobre o **opções do controlador de domínio** , selecione os níveis funcionais de domínio e floresta como Windows Server 2012, especifique a senha do DSRM <strong>pass@word1</strong>e, em seguida, clique em **Avançar**.  

13. Na página **Opções de DNS**, clique em **Avançar**.  

14. Na página **Opções Adicionais** , clique em **Avançar**.  

15. Na página **Caminhos** , digite os locais do banco de dados Active Directory, dos arquivos de log e da pasta SYSVOL (ou aceite os locais padrão) e clique em **Avançar**.  

16. Na página **Opções de Revisão** , confirme suas seleções e clique em **Avançar**.  

17. Na página **Verificação de Pré-requisitos**, confirme se a validação foi concluída e clique em **Instalar**.  

18. Na página **Resultados**, verifique se o servidor foi configurado com êxito como controlador de domínio e clique em **Fechar**.  

19. Reinicie o servidor para concluir a instalação do AD DS. (Isso ocorre automaticamente por padrão.)  

Crie os seguintes usuários usando o Centro Administrativo do Active Directory.  

##### <a name="create-users-and-groups-on-dc1"></a>Criar os usuários e grupos no DC1  

1. Entre em contoso.com como Administrador. Inicie o Centro Administrativo do Active Directory.  

2. Crie os seguintes grupos de segurança:  


   |    Nome do Grupo    |        Endereço de email         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. Crie a seguinte unidade organizacional:  


   |   Nome da OU    | Computadores |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. Crie os usuários a seguir com os atributos indicados:  


   |       User       |  Nome de usuário  |     Endereço de email      | Departamento |      Grupo       | País/região |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  Finanças   |                  |       EUA       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  Finanças   |   FinanceAdmin   |       EUA       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | Operações | FinanceException |       EUA       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     RH     |                  |       EUA       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     RH     |                  |       EUA       |
   |    Servidor RMS    |    rms     |    rms@contoso.com     |            |                  |                |

   Para obter mais informações sobre como criar grupos de segurança, consulte [Criar um novo grupo](https://technet.microsoft.com/library/dd861305.aspx) no site do Windows Server.  

##### <a name="to-create-a-group-policy-object"></a>Para criar um Objeto de Política de Grupo  

1.  Mova o cursor para o canto superior direito da tela e clique no ícone de pesquisa. Na caixa Pesquisar, digite **gerenciamento de política de grupo** e clique em **Gerenciamento de Política de Grupo**.  

2.  Expanda **Floresta: contoso.com**e depois **Domínios**, navegue para **contoso.com**, expanda **(contoso.com)** e selecione **FileServerOU**. Clique com botão direito **criar um GPO neste domínio e vinculá-lo aqui**

3.  Digite um nome descritivo para o GPO, como **GPOdeAcessoFlexível**e depois clique em **OK**.  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Para habilitar o Controle de Acesso Dinâmico para contoso.com  

1.  No Console de Gerenciamento de Política de Grupo, clique em **contoso.com** e clique duas vezes em **Controladores de Domínio**.  

2.  Clique com o botão direito do mouse em **Política de Controladores de Domínio Padrão** e depois em **Editar**.  

3.  Na janela Editor de Gerenciamento de Política de Grupo, clique duas vezes em **Configuração do Computador**, duas vezes em **Políticas**, duas vezes em **Modelos Administrativos**, duas vezes em **Sistema** e, por fim, duas vezes em **KDC**.  

4.  Clique duas vezes em **Suporte KDC para declarações, autenticação composta e proteção Kerberos** e marque a opção ao lado de **Habilitado**. É necessário habilitar esta configuração para usar as Políticas de Acesso Central.  

5.  Abra um prompt de comandos com privilégios elevados e execute o seguinte comando:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_FS1"></a>Criar o servidor de arquivos e o servidor do AD RMS (FILE1)  

1. Crie uma máquina virtual com o nome FILE1 do ISO do Windows Server 2012.  

2. Conecte a máquina virtual à ID_AD_Network.  

3. Junte-se a máquina virtual ao domínio contoso.com e, em seguida, entre no FILE1 como contoso\administrator usando a senha <strong>pass@word1</strong>.  

#### <a name="install-file-services-resource-manager"></a>Instalar o Gerenciador de Recursos de Serviços de Arquivos  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Para instalar a função Serviços de Arquivos e o Gerenciador de Recursos de Servidor de Arquivos  

1.  No Gerenciador do Servidor, clique em **Adicionar Funções e Recursos**.  

2.  Na página **Antes de começar** , clique em **Avançar**.  

3.  Na página **Selecionar tipo de instalação** , clique em **Avançar**.  

4.  Na página **Selecionar servidor de destino**, clique em **Avançar**.  

5.  Na página **Selecionar Funções de Servidor**, expanda **Serviços de Arquivo e Armazenamento**, marque a caixa de seleção ao lado de **Serviços de Arquivo e iSCSI**, expanda e selecione **Gerenciador de Recursos do Servidor de Arquivos**.  

    No Assistente de Adição de Funções e Recursos, clique em **Adicionar Recursos**e em **Avançar**.  

6.  Na página **Selecionar recursos**, clique em **Avançar**.  

7.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  

8.  Na página **Progresso da instalação**, clique em **Fechar**.  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Instalar os Pacotes de Filtro do Microsoft Office no servidor de arquivos  
Você deve instalar os pacotes de filtro do Microsoft Office no Windows Server 2012 para habilitar os IFilters para uma gama de arquivos do Office que são fornecidos por padrão.  Windows Server 2012 não tem nenhum IFilter para arquivos do Microsoft Office instalados por padrão, e a infraestrutura de classificação de arquivos usa os IFilters para realizar a análise do conteúdo.  

Para baixar e instalar os IFilters, consulte [Pacotes de Filtros do Microsoft Office 2010](https://go.microsoft.com/fwlink/?LinkID=234122).  

#### <a name="configure-email-notifications-on-file1"></a>Configurar as notificações de email no FILE1  
Ao criar cotas e telas de arquivo, você tem a opção de enviar notificações por email para os usuários quando o seu limite de cota está se aproximando ou depois que eles tentaram salvar arquivos que foram bloqueados. Se você deseja notificar rotineiramente certos administradores sobre os eventos de cotas e triagem de arquivos, configure um ou mais destinatários padrão. Para enviar essas notificações, é necessário especificar o servidor SMTP usado para encaminhamento de mensagens de email.  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Para configurar as opções de email no Gerenciador de Recursos de Servidor de Arquivos  

1. Abra o Gerenciador de Recursos de Servidor de Arquivos. Para abrir o Gerenciador de Recursos de Servidor de Arquivos, clique em **Iniciar**, digite **gerenciador de recursos do servidor de arquivos**e clique em **Gerenciador de Recursos do Servidor de Arquivos**.  

2. Na interface do Gerenciador de Recursos de Servidor de Arquivos, clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos** e depois em **Configurar opções**. A caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos** é aberta.  

3. Na guia **Notificações por Email** , no nome do servidor SMTP ou endereço IP, digite o nome do host ou o endereço IP do servidor SMTP para encaminhar as notificações por email.  

4. Se você quiser notificar determinados administradores da cota rotineiramente ou eventos de triagem de arquivo sob **administradores destinatários padrão**, digite cada endereço de email como fileadmin@contoso.com. Use o formato account@domaine use ponto e vírgula para separar várias contas.  

#### <a name="create-groups-on-file1"></a>Criar grupos no FILE1  

###### <a name="to-create-security-groups-on-file1"></a>Para criar grupos de segurança no FILE1  

1. Entre no FILE1 como contoso\administrator com a senha: <strong>pass@word1</strong>.  

2. Adicione NT AUTHORITY\Authenticated Users ao grupo **WinRMRemoteWMIUsers__** .  

#### <a name="create-files-and-folders-on-file1"></a>Criar arquivos e pastas no FILE1  

1.  Crie um novo volume NTFS no FILE1 e depois crie a seguinte pasta: D:\Documentos Financeiros.  

2.  Crie os arquivos a seguir com os detalhes especificados:  

    -   **Memorando financeiro.docx**: Acrescente algum texto referente a finanças no documento. Por exemplo, ' as regras de negócios sobre quem pode acessar documentos financeiros foram alteradas. Documentos financeiros agora somente podem ser acessados por membros do grupo FinanceExpert. Outros departamentos ou grupos têm acesso.' Você precisará avaliar o impacto desta alteração antes de implementá-la no ambiente. Verifique se a indicação CONFIDENCIAL DA CONTOSO está no rodapé de todas as páginas do documento.  

    -   **Solicitação de aprovação para contratação.docx**: Crie um formulário neste documento para coletar as informações dos candidatos. É necessário possuir os seguintes campos neste documento: **Nome do Candidato, Cadastro de Pessoas Físicas, Cargo, Salário Proposto, Data de Início, Nome do supervisor, Departamento**. Acrescente uma seção adicional no documento com formulário para **Assinatura do Supervisor, Salário Aprovado, Confirmação da Oferta** e **Status da Oferta**.   
        Habilite o gerenciamento de direitos para documentos.  

    -   **Documento do Word1.docx**: Acrescente conteúdo de teste a este documento.  

    -   **Documento do Word2.docx**: Acrescente conteúdo de teste a este documento.  

    -   **Workbook1.xlsx**  

    -   **Workbook2.xlsx**  

    -   Crie uma pasta na área de trabalho chamada Expressões Regulares. Crie um documento de texto na pasta chamado **RegEx-SSN**. Digite o conteúdo abaixo no arquivo, depois salve e feche:   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  Compartilhe a pasta D:\Documentos Financeiros como Documentos Financeiros e permita que todos tenham acesso de Leitura e Gravação para compartilhar.  

> [!NOTE]  
> As políticas de acesso central não estão habilitadas por padrão no sistema ou volume de inicialização C:.  

#### <a name="BKMK_CS1"></a>Instalar o Active Directory Rights Management Services  
Adicione o AD RMS e todos os recursos necessários pelo Gerenciador do Servidor. Escolha todos os padrões.  

###### <a name="to-install-active-directory-rights-management-services"></a>Para instalar o Active Directory Rights Management Services  

1. Entre no FILE1 como CONTOSO\Administrator ou como membro do grupo Admins. do Domínio.  

   > [!IMPORTANT]  
   > Para instalar a função de servidor do AD RMS, a conta do instalador (neste caso, CONTOSO\Administrator) deverá receber associação no grupo Administradores local no computador do servidor onde o AD RMS está instalado e também no grupo Administradores de Empresa no Active Directory.  

2. No Gerenciador do Servidor, clique em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.  

3. Na tela **Antes de começar**, clique em **Avançar**.  

4. Na tela **Selecionar Tipo de Instalação** , clique em **Instalação Baseada em Função/Recurso**e em **Avançar**.  

5. Na tela **Selecionar Destinos do Servidor**, clique em **Avançar**.  

6. Na tela **Selecionar Funções de Servidor** , marque a caixa ao lado do **Active Directory Rights Management Services**e clique em **Avançar**.  

7. Na caixa de diálogo **Adicionar recursos requeridos para os Active Directory Rights Management Services?** , clique em **Adicionar Recursos**.  

8. Na tela **Selecionar Funções de Servidor**, clique em **Avançar**.  

9. Na tela **Selecionar Recursos a Serem Instalados** , clique em **Avançar**.  

10. Na tela **Active Directory Rights Management Services**, clique em Avançar.  

11. Na tela **Selecionar Serviços de Função** , clique em **Avançar**.  

12. Na tela **Função de Servidor Web (IIS)** , clique em **Avançar**.  

13. Na tela **Selecionar Serviços de Função** , clique em **Avançar**.  

14. Na tela **Confirmar Seleções de Instalação** , clique em **Instalar**.  

15. Depois da conclusão da instalação, na tela **Progresso da Instalação** , clique em **Executar configuração adicional**. O Assistente de Configuração de AD RMS é exibido.  

16. Na tela **AD RMS** , clique em **Avançar**.  

17. Na tela **Cluster do AD RMS** , selecione **Criar novo cluster AD RMS raiz** e clique em **Avançar**.  

18. Na tela **Banco de Dados de Configuração**, clique em **Usar o Banco de Dados Interno do Windows neste servidor** e depois em **Avançar**.  

    > [!NOTE]  
    > É recomendado usar o Banco de Dados Interno do Windows em ambientes de teste porque ele não dá suporte a mais de um servidor no cluster do AD RMS. Implantações de produção devem usar um servidor de banco de dados separado.  

19. Sobre o **conta de serviço** tela, **conta de usuário do domínio**, clique em **especifique** e, em seguida, especifique o nome de usuário (**contoso\rms**), e Senha (<strong>pass@word1</strong>) e clique em **Okey**e, em seguida, clique em **próximo**.  

20. Na tela **Modo Criptográfico**, clique em **Modo Criptográfico 2**.  

21. Na tela **Armazenamento de Chave do Cluster**, clique em **Avançar**.  

22. Sobre o **senha da chave de Cluster** tela, o **senha** e **Confirmar senha** caixas, digite <strong>pass@word1</strong>e, em seguida, clique em **Próxima**.  

23. Na tela **Site do Cluster** , verifique se **Site Padrão** está selecionado e depois clique em **Avançar**.  

24. Na tela **Endereço do Cluster**, selecione a opção **Usar uma conexão descriptografada** e na caixa **Nome de Domínio Totalmente Qualificado**, digite **FILE1.contoso.com** e clique em **Avançar**.  

25. Na tela **Nome do Certificado de Licenciante**, aceite o nome padrão (**FILE1**) na caixa de texto e clique em **Avançar**.  

26. Na tela **Registro de SCP** , selecione **Registrar SCP agora**e clique em **Avançar**.  

27. Na tela **Confirmação**, clique em **Instalar**.  

28. Na tela **Resultados**, clique em **Fechar** e depois em **Fechar** na tela **Progresso da Instalação**. Ao concluir, faça logoff e faça logon como contoso\rms usando a senha fornecida (<strong>pass@word1</strong>).  

29. Inicie o console do AD RMS e navegue para **Modelos de Política de Direitos**.  

    Para abrir o console do AD RMS, no Gerenciador do Servidor, clique em **Servidor Local** na árvore de console, clique em **Ferramentas** e selecione **Active Directory Rights Management Services**.  

30. Clique no modelo **Criar Política de Direitos Distribuídos** localizado no painel direito, clique em **Adicionar**e selecione as seguintes informações:  

    -   Idioma: Inglês dos EUA  

    -   Nome: Somente Administrador Financeiro da Contoso  

    -   Descrição: Somente Administrador Financeiro da Contoso  

    Clique em **Adicionar** e em **Avançar**.  

31. Na seção usuários e direitos, clique em **usuários e direitos**, clique em **Add**, digite <strong>financeadmin@contoso.com</strong>e clique em **Okey**.  

32. Selecione **Controle Total**e deixe **Conceder ao proprietário (autor) o direito ininterrupto de controle total** selecionado.  

33. Passe pelas guias seguintes sem fazer alterações e clique em **Concluir**. Faça login como CONTOSO\Administrator.  

34. Navegue até a pasta, C:\inetpub\wwwroot\\_wmcs\certification, selecione o arquivo ServerCertification. asmx e adicione usuários autenticados para ter permissões leitura e gravação para o arquivo.  

35. Abra o Windows PowerShell e execute `Get-FsrmRmsTemplate`. Verifique se que você pode ver o modelo de RMS criado nas etapas anteriores neste procedimento com este comando.  

> [!IMPORTANT]  
> Se desejar que os servidores de arquivo sejam alterados imediatamente para que você possa testá-los, faça o seguinte:  
>   
> 1.  No servidor de arquivos, FILE1, abra um prompt de comandos com privilégios elevados e execute os seguintes comandos:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  No controlador de domínio (DC1), replique o Active Directory.  
>   
>     Para obter mais informações sobre as etapas para forçar a replicação do Active Directory, consulte [Replicação do Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  

Opcionalmente, em vez de usar o Assistente de Adição de Funções e Recursos no Gerenciador do Servidor, é possível usar o Windows PowerShell para instalar e configurar a função de servidor do AD RMS como mostrado no procedimento a seguir.  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Para instalar e configurar um cluster de AD RMS no Windows Server 2012 usando o Windows PowerShell  

1. Entre como CONTOSO\Administrator com a senha: <strong>pass@word1</strong>.  

   > [!IMPORTANT]  
   > Para instalar a função de servidor do AD RMS, a conta do instalador (neste caso, CONTOSO\Administrator) deverá receber associação no grupo Administradores local no computador do servidor onde o AD RMS está instalado e também no grupo Administradores de Empresa no Active Directory.  

2. Na área de trabalho do Servidor, clique com o botão direito do mouse no ícone do Windows PowerShell na barra de tarefas e selecione **Executar como Administrador** para abrir um prompt do Windows PowerShell com privilégios administrativos.  

3. Para usar os cmdlets do Gerenciador do Servidor para instalar a função de servidor do AD RMS, digite:  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. Crie a unidade do Windows PowerShell para representar o servidor do AD RMS instalado.  

   Por exemplo, para criar uma unidade do Windows PowerShell chamada RC para instalar e configurar o primeiro servidor em um cluster de raiz do AD RMS, digite:  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. Defina as propriedades nos objetos no namespace da unidade que representa as definições de configuração requeridas.  

   Por exemplo, para definir a conta de serviço do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   $svcacct = Get-Credential  
   ```  

   Quando a caixa de diálogo de segurança do Windows for exibida, digite o nome de usuário de domínio da conta de serviço do AD RMS CONTOSO\RMS e a senha atribuída a ele.  

   Em seguida, para atribuir a conta de serviço do AD RMS às configurações de cluster do AD RMS, digite o seguinte:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   Em seguida, para definir o servidor do AD RMS par ausar o Banco de Dados Interno do Windows, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   Em seguida, para armazenar com segurança a senha da chave do cluster em uma variável, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   Digite a senha da chave do cluster e pressione a tecla ENTER.  

   Em seguida, para atribuir a senha à instalação do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   Em seguida, para definir o endereço do cluster do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   Em seguida, para atribuir o nome de SLC à instalação do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   Em seguida, para definir o SCP (ponto de conexão de serviço) para o cluster do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. Execute o cmdlet **Install-ADRMS**. Além de instalar a função do servidor do AD RMS e configurar o servidor, este cmdlet também instala outros recursos requeridos pelo AD RMS, se necessário.  

   Por exemplo, para alterar a unidade do Windows PowerShell chamada RC, instalar e configurar o AD RMS, digite:  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   Digite "Y" quando o cmdlet solicitar confirmação para o início da instalação.  

7. Faça logoff como Contoso\administrador e log como CONTOSO\RMS usando a senha fornecida ("pass@word1").  

   > [!IMPORTANT]  
   > Para gerenciar o servidor do AD RMS, a conta com a qual você entrou e que estará usando para gerenciar o servidor (neste caso, o CONTOSO\RMS) deverá possuir associação no grupo Administradores local e no computador do servidor do AD RMS, bem como associação no grupo Administradores de Empresa no Active Directory.  

8. Na área de trabalho do Servidor, clique com o botão direito do mouse no ícone do Windows PowerShell na barra de tarefas e selecione **Executar como Administrador** para abrir um prompt do Windows PowerShell com privilégios administrativos.  

9. Crie a unidade do Windows PowerShell para representar o servidor do AD RMS que você está configurando.  

    Por exemplo, para criar uma unidade do Windows PowerShell chamada RC para configurar o cluster raiz do AD RMS, digite:  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. Para criar um novo modelo de direitos para o administrador financeiro da Contoso e atribuí-lo direitos de usuário com controle total na instalação do AD RMS, digite o seguinte no prompt de comando do Windows PowerShell:  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. Para verificar se é possível ver o novo modelo de direitos para o administrador financeiro da Contoso, faça o seguinte no prompt de comando do Windows PowerShell:  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    Analise a saída deste cmdlet para confirmar se o modelo de RMS criado na etapa anterior está presente.  

### <a name="build-the-mail-server-srv1"></a>Criar o servidor de email (SRV1)  
SRV1 é o servidor de email SMTP/POP3. É necessário configurá-lo para enviar notificações por email como parte do cenário de assistência para acesso negado.  

Configure o Microsoft Exchange Server neste computador. Para obter mais informações, consulte [Como instalar o Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  

### <a name="build-the-client-virtual-machine-client1"></a>Criar a máquina virtual cliente (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>Para criar a máquina virtual cliente  

1. Conecte o CLIENT1 à ID_AD_Network.  

2. Instale o Microsoft Office 2010.  

3. Entre como Contoso\Administrator e use as informações a seguir para configurar o Microsoft Outlook.  

   - Seu nome: Administrador de Arquivos  

   - Endereço de email: fileadmin@contoso.com  

   - Tipo de conta: POP3  

   - Servidor de email de entrada: O endereço IP estático do SRV1  

   - Servidor de email de saída: O endereço IP estático do SRV1  

   - Nome de usuário: fileadmin@contoso.com  

   - Lembrar a senha: Selecionar  

4. Crie um atalho para o Outlook na área de trabalho do contoso\administrator.  

5. Abra o Outlook e resolva todas as mensagens 'aberto pela primeira vez'.  

6. Exclua as eventuais mensagens de teste geradas.  

7. Criar um novo atalho na área de trabalho para todos os usuários na máquina virtual cliente que aponta para \\\FILE1\Finance documentos.  

8. Reinicie se necessário.  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Habilitar a assistência para Acesso Negado na máquina virtual cliente  

1.  Abra o Editor de Registro e navegue para **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  

    -   Defina **EnableShellExecuteFileStreamCheck** para **1**.  

    -   Valor: DWORD  

## <a name="BKMK_CF"></a>Configuração do laboratório para implantar declarações em cenários entre florestas  

### <a name="BKMK_2.1"></a>Criar uma máquina virtual para o DC2  

-   Crie uma máquina virtual do ISO do Windows Server 2012.  

-   Crie a máquina virtual com o nome DC2.  

-   Conecte a máquina virtual à ID_AD_Network.  

> [!IMPORTANT]  
> Associar máquinas virtuais a um domínio e implantar tipos de declaração entre florestas requer que as máquinas virtuais possam resolver os FQDNs dos domínios em questão. Para tal, você pode definir as configurações de DNS manualmente nas máquinas virtuais. Para obter mais informações, consulte [Configurando uma rede virtual](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Todas as imagens de máquina virtual (servidores e clientes) devem ser reconfiguradas para usar um endereço IPv4 (IP versão 4) estático e configurações do cliente do DNS (Sistema de Nome de Domínio). Para obter mais informações, consulte [Configurar um cliente DNS para o endereço IP estático](https://go.microsoft.com/fwlink/?LinkId=150952).  

### <a name="BKMK_2.2"></a>Configurar uma nova floresta chamada adatum.com  

##### <a name="to-install-active-directory-domain-services"></a>Para instalar os Serviços de Domínio Active Directory  

1. Conecte a máquina virtual à ID_AD_Network. Entre no DC2 como administrador com a senha <strong>Pass@word1</strong>.  

2. No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**.  

3. Na página **Antes de começar** , clique em **Avançar**.  

4. Na página **Selecionar Tipo de Instalação** , clique em **Instalação baseada em função ou recurso**e depois em **Avançar**.  

5. Na página **Selecionar servidor de destino** , clique em **Selecionar um servidor no pool de servidores**, clique nos nomes do servidor em que você deseja instalar o AD DS (Serviços de Domínio Active Directory) e em **Avançar**.  

6. Na página **Selecionar Funções de Servidor** , clique em **Serviços de Domínio Active Directory**. Na caixa de diálogo **Assistente de Adição de Funções e Recursos** , clique em **Adicionar Recursos**e em **Avançar**.  

7. Na página **Selecionar Recursos**, clique em **Avançar**.  

8. Na página **AD DS**, revise a informação e clique em **Avançar**.  

9. Na página **Confirmação**, clique em **Instalar**. A barra de progresso de instalação do recurso na página Resultados indica que a função está sendo instalada.  

10. Na página **Resultados** , verifique se a instalação foi bem-sucedida e, em seguida, clique no ícone de aviso com um ponto de exclamação no canto superior direito da tela, ao lado de **Gerenciar**. Na lista de Tarefas, clique no link **Promover este servidor a um controlador de domínio**.  

    > [!IMPORTANT]  
    > Se você fechar o assistente de instalação neste momento, em vez de clicar em **Promover este servidor a controlador de domínio**, poderá continuar com a instalação do AD DS clicando em **Tarefas** no Gerenciador do Servidor.  

11. Na página **Configuração de Implantação**, clique em **Adicionar nova floresta**, digite o nome do domínio raiz **adatum.com** e clique em **Avançar**.  

12. Sobre o **opções do controlador de domínio** , selecione os níveis funcionais de domínio e floresta como Windows Server 2012, especifique a senha do DSRM <strong>pass@word1</strong>e, em seguida, clique em **Avançar**.  

13. Na página **Opções de DNS**, clique em **Avançar**.  

14. Na página **Opções Adicionais** , clique em **Avançar**.  

15. Na página **Caminhos** , digite os locais do banco de dados Active Directory, dos arquivos de log e da pasta SYSVOL (ou aceite os locais padrão) e clique em **Avançar**.  

16. Na página **Opções de Revisão** , confirme suas seleções e clique em **Avançar**.  

17. Na página **Verificação de Pré-requisitos**, confirme se a validação foi concluída e clique em **Instalar**.  

18. Na página **Resultados**, verifique se o servidor foi configurado com êxito como controlador de domínio e clique em **Fechar**.  

19. Reinicie o servidor para concluir a instalação do AD DS. (Isso ocorre automaticamente por padrão.)  

> [!IMPORTANT]  
> Para garantir que a rede foi configurada devidamente, depois de configurar ambas as florestas, faça o seguinte:  
>   
> -   Entre em adatum.com como adatum\administrator. Abra a janela do Prompt de Comando, digite **nslookup contoso.com** e pressione ENTER.  
> -   Entre no contoso.com como contoso\administrator. Abra a janela do Prompt de Comando, digite **nslookup adatum.com** e pressione ENTER.  
>   
> Se esses comandos forem executados sem erros, as florestas podem se comunicar. Para ver mais informações sobre os erros de nslookup, consulte a seção de solução de problemas no tópico [Usando o NSlookup.exe](https://support.microsoft.com/kb/200525)  

### <a name="BKMK_2.22"></a>Defina contoso.com como floresta confiável para adatum.com  
Nesta etapa, você criará a relação de confiança entre os sites da Adatum Corporation e da Contoso, Ltd.  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Para definir Contoso como floresta de confiança da Adatum  

1.  Entre no DC2 como administrador. Na tela **Iniciar** , digite domain.msc.  

2.  Na árvore de consoles, clique com o botão direito do mouse em adatum.com e clique em Propriedades.  

3.  Na guia **Relações de Confiança** , clique em **Nova Relação de Confiança**e em **Avançar**.  

4.  Na página **Nome da Relação de Confiança**, digite **contoso.com**, no campo de nome do DNS e depois clique em **Avançar**.  

5.  Na página **Tipo de Relação de Confiança**, clique em **Relação de Confiança da Floresta** e em **Avançar**.  

6.  Na página **Direção da Relação de Confiança** , clique em **Bidirecional**.  

7.  Na página **Lados da Relação de Confiança** , clique em **Neste domínio e no domínio especificado**e depois clique em **Avançar**.  

8.  Continue a seguir as instruções do assistente.  

### <a name="BKMK_2.4"></a>Criar usuários adicionais na floresta Adatum  
Crie o usuário Jeff Low com a senha <strong>pass@word1</strong>e atribua o atributo da empresa com o valor **Adatum**.  

##### <a name="to-create-a-user-with-the-company-attribute"></a>Para criar um usuário com o atributo Empresa  

1.  Abra um prompt de comandos com privilégios elevados no Windows PowerShell e cole o seguinte código:  

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

### <a name="BKMK_2.5"></a>Criar o tipo de declaração empresa no adataum.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para criar um tipo de declaração usando o Windows PowerShell  

1.  Entre em adatum.com como administrador.  

2.  Abra um prompt de comandos com privilégios elevados no Windows PowerShell e digite o seguinte código:  

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

### <a name="BKMK_2.55"></a>Habilitar a propriedade de recurso da empresa no contoso.com  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Para habilitar a propriedade de recurso Empresa no contoso.com  

1.  Entre em contoso.com como administrador.  

2.  No Gerenciador do Servidor, clique em **Ferramentas** e em **Centro Administrativo do Active Directory**.  

3.  No painel esquerdo do Centro Administrativo do Active Directory, clique em **Modo de Exibição de Árvore**. No painel esquerdo, clique em **Controle de Acesso Dinâmico** e clique duas vezes em **Propriedades do Recurso**.  

4.  Selecione **Empresa** na lista de **Propriedades do Recurso** , clique com o botão direito do mouse e selecione **Propriedades**. Na seção **Valores Sugeridos**, clique em **Adicionar** para adicionar os valores sugeridos: Contoso e Adatum, depois clique duas vezes em **OK**.  

5.  Selecione **Empresa** na lista **Propriedades do Recurso**, clique com o botão direito do mouse e selecione **Habilitar**.  

### <a name="BKMK_2.6"></a>Habilitar o controle de acesso dinâmico no adatum.com  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Para habilitar o Controle de Acesso Dinâmico para adatum.com  

1.  Entre em adatum.com como administrador.  

2.  Abra o Console de Gerenciamento de Política de Grupo, clique em **adatum.com** e clique duas vezes em **Controladores de Domínio**.  

3.  Clique com o botão direito do mouse em **Política de Controladores de Domínio Padrão** e depois em **Editar**.  

4.  Na janela Editor de Gerenciamento de Política de Grupo, clique duas vezes em **Configuração do Computador**, duas vezes em **Políticas**, duas vezes em **Modelos Administrativos**, duas vezes em **Sistema** e, por fim, duas vezes em **KDC**.  

5.  Clique duas vezes em **Suporte KDC para declarações, autenticação composta e proteção Kerberos** e marque a opção ao lado de **Habilitado**. É necessário habilitar esta configuração para usar as Políticas de Acesso Central.  

6.  Abra um prompt de comandos com privilégios elevados e execute o seguinte comando:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_2.8"></a>Criar o tipo de declaração empresa no contoso.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para criar um tipo de declaração usando o Windows PowerShell  

1.  Entre em contoso.com como administrador.  

2.  Abra um prompt de comandos com privilégios elevados no Windows PowerShell e digite o seguinte código:  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="BKMK_2.9"></a>Criar a regra de acesso central  

##### <a name="to-create-a-central-access-rule"></a>Para criar uma regra de acesso central  

1. No painel esquerdo do Centro Administrativo do Active Directory, clique em **Modo de Exibição de Árvore**. No painel esquerdo, clique em **Controle de Acesso Dinâmico** e clique em **Regras de Acesso Central**.  

2. Clique com o botão direito em **Regras de Acesso Central**, em **Nova**e depois em **Regra de Acesso Central**.  

3. No campo **Nome**, digite **RegradeAcessoparaFuncionáriosAdatum**.  

4. Na seção **Permissões**, selecione a opção **Usar as seguintes permissões como atuais**, clique em **Editar** e em **Adicionar**. Clique no link **Selecionar uma entidade** , digite **Usuários Autenticados**e clique em **OK**.  

5. Na caixa de diálogo **Permissão de Entrada para Permissões**, clique em **Adicionar uma condição** e digite as condições a seguir: [**Usuário**] [**Empresa**] [**Equivale**] [**Valor**] [**Adatum**]. As permissões devem ser **Modificar, Ler e Executar, Ler e Gravar**.  

6. Clique em **OK**.  

7. Clique em **OK** três vezes para concluir e voltar ao Centro Administrativo do Active Directory.  

   ![guias de soluções](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  

   O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  

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

1.  Entre em contoso.com como administrador.  

2.  Abra um prompt de comandos com privilégios elevados no Windows PowerShell e cole o seguinte código:  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="BKMK_2.11"></a>Publicar a nova política por meio da diretiva de grupo  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Para aplicar a política de acesso central aos servidores do arquivo por meio da Política de Grupo  

1.  Na tela **Iniciar** , digite **Ferramentas Administrativas**e na barra **Pesquisar** , clique em **Configurações**. Nos resultados de **Configurações** , clique em **Ferramentas Administrativas**. Abra o Console de Gerenciamento de Política de Grupo na pasta **Ferramentas Administrativas** .  

    > [!TIP]  
    > Se a configuração **Mostrar Ferramentas Administrativas** estiver desabilitada, a pasta Ferramentas Administrativas e seus conteúdos não aparecerão nos resultados de **Configurações**.  

2.  Clique com botão direito no domínio contoso.com, clique em **criar um GPO neste domínio e vinculá-lo aqui**  

3.  Digite um nome descritivo para o GPO, como **GPOdeAcessodaDatum**e depois clique em **OK**.  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Para aplicar a política de acesso central ao servidor de arquivos por meio da Política de Grupo  

1.  Na tela **Iniciar**, digite **Gerenciamento de Política de Grupo** na caixa **Pesquisar**. Abra **Gerenciamento de Política de Grupo** na pasta de Ferramentas Administrativas.  

    > [!TIP]  
    > Se a configuração **Mostrar Ferramentas Administrativas** estiver desabilitada, a pasta Ferramentas Administrativas e seus conteúdos não aparecerão nos resultados de Configurações.  

2.  Navegue e selecione Contoso da seguinte maneira: Group Policy Management\Forest: contoso.com\Domains\contoso.com.  

3.  Clique com o botão direito do mouse na política**GPOdeAcessodaDatum** e selecione **Editar**.  

4.  No Editor de Gerenciamento de Política de Grupo, clique em **Configuração do Computador**, expanda **Políticas**, expanda **Configurações do Windows**e clique em **Configurações de Segurança**.  

5.  Expanda **Sistema de Arquivos**, clique com o botão direito do mouse em **Política de Acesso Central** e clique em **Gerenciar políticas de acesso central**.  

6.  Na caixa de diálogo **Configuração de Políticas de Acesso Central** , clique em **Adicionar**, selecione **Política de Acesso Somente para Adatum**e clique em **OK**.  

7.  Feche o Editor de Gerenciamento de Política de Grupo. Você acaba de adicionar a política de acesso central à Política de Grupo.  

### <a name="BKMK_2.12"></a>Crie a pasta ganhos no servidor de arquivos  
Crie um novo volume NTFS no FILE1 e crie a seguinte pasta: D:\Ganhos.  

> [!NOTE]  
> As políticas de acesso central não estão habilitadas por padrão no sistema ou volume de inicialização C:.  

### <a name="BKMK_2.13"></a>Definir a classificação e aplicar a política de acesso central à pasta ganhos  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Para atribuir a política de acesso central no servidor de arquivos  

1. No Gerenciador Hyper-V, conecte-se ao servidor FILE1. Entrar para o servidor usando Contoso\Administrator com a senha <strong>pass@word1</strong>.  

2. Abra um prompt de comandos com privilégios elevados e digite: **gpupdate /force**. Isso garantirá que suas alterações na Política de Grupo entrarão em vigor no servidor.  

3. Será necessário atualizar as Propriedades de Recurso Global do Active Directory. Abra o Windows PowerShell, digite `Update-FSRMClassificationpropertyDefinition`e pressione ENTER. Feche o Windows PowerShell.  

4. Abra o Windows Explorer e navegue para D:\GANHOS. Clique com o botão direito do mouse na pasta **Ganhos** e clique em **Propriedades**.  

5. Clique na guia **Classificação**. Selecione Empresae **Adatum** no campo **Valor** .  

6. Clique em **Alterar**, selecione **Política de Acesso Somente para Adatum** no menu suspenso e clique em **Aplicar**.  

7. Clique na guia **Segurança**, em **Avançado** e por fim na guia **Política Central**. **AdatumEmployeeAccessRule** deverá estar listado. Você pode expandir o item para ver todas as permissões definidas ao criar a regra no Active Directory.  

8. Clique em **OK** para retornar ao Windows Explorer.  



