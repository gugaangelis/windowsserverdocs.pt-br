---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Atualizar o AD RMS para Windows Server 2016
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: f5d621a0ba06f5b1beb97ccdbffb8376b5503168
ms.sourcegitcommit: 927adf32faa6052234ad08f21125906362e593dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67033347"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Atualizar o AD RMS para Windows Server 2016

## <a name="introduction"></a>Introdução

Active Directory Rights Management Services (AD RMS) é um serviço da Microsoft que protege os documentos e e-mails confidenciais. Ao contrário dos métodos tradicionais de proteção, como firewalls e ACLs, proteção e criptografia de AD RMS são persistentes, não importa onde vai a um arquivo ou como eles são transportados. 

Este documento fornece orientação para a migração do Windows Server 2012 R2 com o SQL Server 2012 para Windows Server 2016 e SQL Server 2016. O mesmo processo pode ser usado para migrar de versões mais antigas, mas com suporte do AD RMS.
Observe que o Active Directory Rights Management Services não está mais em desenvolvimento ativo, e para os recursos mais recentes os clientes devem considerar a migração para o [do Azure Information Protection](https://azure.microsoft.com/services/information-protection/), que oferece um muito mais conjunto abrangente de recursos com mais completa do dispositivo e o suporte do aplicativo. 

Para obter informações sobre como migrar para a proteção de informações do Azure do AD RMS, sem a necessidade de proteger novamente seu conteúdo, consulte [a documentação de migração do Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Sobre o ambiente usado neste guia

O AD FS é um componente opcional de uma instalação do AD RMS. Neste guia, o uso do ADFS será assumido. Se o AD FS não foi usado em seu ambiente para dar suporte a usuários do AD RMS, você poderá ignorar todas as etapas que se referem ao ADFS.

Neste guia, o SQL Server é atualizado para o SQL Server 2016 executando uma instalação paralela e movimentar os bancos de dados por meio de um backup. Como alternativa, se você pode atualizar seus servidores de banco de dados do AD RMS e AD FS para o SQL Server 2016 no local, você pode mover para a próxima seção neste documento após ter feito isso sem precisar seguir as etapas nesta seção.  

## <a name="installation"></a>Instalação

### <a name="configuring-sql-server-2016"></a>Configurando o SQL Server 2016

As seguintes tarefas de implementação de detalhes de seção relacionada diretamente para a configuração do SQL Server 2016. Este guia concentra-se sobre como usar o Gerenciador do servidor e o SQL Server Management Studio para concluir essas tarefas.

Essas etapas devem ser concluídas em uma instalação do SQL Server 2016. Instale o SQL Server 2016 em hardware adequado, de acordo com as políticas e práticas padrão de sua organização. 

#### <a name="preparing-the-sql-server"></a>Preparação do SQL Server

A seção a seguir fornece detalhes sobre como preparar o SQL Server, de modo que podem ser atualizado para o SQL Server 2016 antes de atualizar os outros serviços na plataforma do AD RMS para usar o Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Adicionando o CNAME para o SQL Server 2016 para o DNS

O CNAME é usado para ajudar a garantir que a instalação do Windows Server 2016 obterá os dados apropriados, pois ele será ser apontado para o novo SQL Server 2016. **Observação: Se estiver usando um CNAME para o serviço do ADFS e o AD RMS, você pode passar para as próximas etapas.**


**Para adicionar um CNAME para o SQL Server 2016 ao DNS**

1.  Faça logon para o controlador de domínio do Windows Server 2012 R2 com as credenciais de administrador de domínio.

2.  Abra o Gerenciador do Servidor.

3.  Clique em **ferramentas** e selecione **DNS** para abrir o Gerenciador de DNS.

4.  No painel de navegação esquerdo, expanda o controlador de domínio e abra **zonas de pesquisa direta**.

5.  Abrir os recursos de domínio apropriado e em seguida, clique com o botão direito no painel de exibição à direita e selecione **novo Alias (CNAME)** para começar a criar o CNAME.

6.  Para obter o nome do alias digitar em um nome lógico para diferenciá-lo de outros que pode estar presente (ex. SQLADRMS ou SQLADFS)

7.  Depois de inserir o nome, forneça o FQDN do host de destino que será o novo servidor do SQL Server 2016. (por exemplo SQL2016.contoso.com)

8.  Depois que todas as informações a serem inseridas, clique em **Okey**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Backup do AD RMS e bancos de dados do AD FS

Os bancos de dados do AD RMS e AD FS mantêm informações críticas necessárias para o AD RMS, como a chave pública do certificado de licenciante para servidor, modelos de política de direitos, dados de configuração do AD FS e informações de log. Sem esses bancos de dados, os clientes não podem emitir licenças para consumir conteúdo protegido, entre outros problemas.

Os bancos de dados, o banco de dados de configuração do AD RMS é considerado o mais importante, conforme ele armazena o SLC, direitos de modelos de política, as chaves dos usuários e informações de configuração. Portanto, embora você deve ter cuidado ao fazer backup de todos os bancos de dados do AD RMS e AD FS, você deve planejar fazer backup de banco de dados de configuração regularmente.

O registro em log de banco de dados armazena informações sobre solicitações do usuário para o cluster AD RMS para certificados e licenças de uso. Sua estratégia de backup desse banco de dados deve ser baseada na política da empresa para reter esse tipo de informação.

O banco de dados de serviços de diretório não é essencial para a funcionalidade do AD RMS e, se os dados mais recentes forem perdidos, o banco de dados será repopular com informações como o servidor do AD RMS recebe solicitações de certificados e licenças de uso. Você não precisa fazer backup desse banco de dados regularmente, mas você precisa ter pelo menos uma cópia do banco de dados, já que ela foi originalmente configurada depois de implantar o AD RMS.

**Para fazer backup de um banco de dados do AD RMS e/ou o ADFS com o Microsoft SQL Server**

1.  Faça logon no servidor de banco de dados do AD RMS do Windows Server 2012 R2 com o SQL 2012.

2.  Clique em **inicie**, clique em **todos os programas**, clique em **Microsoft SQL Server**e clique em **SQL Server Management Studio**.

3.  No **conectar ao servidor** janela, confirme se o servidor que hospeda os bancos de dados do AD RMS está no **nome do servidor** caixa e clique em **Connect**.

4.  Expandir **bancos de dados**. Clique com botão direito no banco de dados apropriado (**DRMS** e **Adfs**), aponte para **tarefas**e selecione **Backup**.

5.  Repita a etapa 4 para bancos de dados restantes.

6.  Certifique-se de que o backup dos bancos de dados pode ser acessado por outros computadores na rede ou usando um dispositivo de armazenamento, como eles serão necessários para as etapas posteriores durante a migração.

Agora você pode armazenar as cópias de banco de dados em um local seguro. Lembre-se de fazer backup de seus bancos de dados com frequência.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Adicionar administrador de domínio, SQL, o AD RMS e/ou o serviço ADFS da conta para o SQL Server 2016

As etapas a seguir mostrarão como adicionar várias contas de serviço para o SQL Server 2016 para ajudar a migrar os dados do ambiente do Windows Server 2012 R2. Isso lhe dará as permissões adequadas ao tentar acessar o conteúdo e tratar os dados.

**Para adicionar o administrador de domínio, SQL, AD RMS, e/ou conta de serviço do AD FS para o SQL Server**

1.  Faça logon no servidor com o SQL Server 2016 como a conta de Administrador Local.

2.  Clique em **inicie**, clique em **todos os programas**, clique em **Microsoft SQL Server**e clique em **SQL Server Management Studio**.

3.  No **conectar ao servidor** janela, confirme se o servidor que hospeda os bancos de dados do AD RMS está no **nome do servidor** caixa para autenticação, clique no menu suspenso e selecione **do SQL Server Autenticação**.

4.  No **Login** campo Insira o nome da conta de Administrador Local (ex.: localadmin) e, em seguida, forneça a senha apropriada e clique em **Connect**.

5.  Expandir **segurança** e, em seguida, clique com botão direito **logons** e selecione **novo logon** no menu de contexto que aparece.

6.  Depois que a janela for exibida, insira na conta de administrador de domínio na **nome de logon** campo (ex.: A Contoso\\ContosoAdmin)

7.  No painel de navegação esquerdo, escolha **funções de servidor**.

8.  Em seguida, marque a caixa de seleção **sysadmin** sob a funções de servidor e clique em **Okey**.

9.  Reinicie **SQL Server Management**.

10. No **conectar ao servidor** janela, confirme se o servidor que hospeda os bancos de dados do AD RMS está no **nome do servidor** caixa para autenticação, clique no menu suspenso e selecione **Windows Autenticação** e clique em **Connect**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restaurando o AD RMS e os bancos de dados do AD FS para SQL Server 2016

As etapas a seguir mostrarão como restaurar os dados de instância do SQL Server anterior para a nova instância de 2016. Isso permitirá que o novo SQL utilizem os dados de configuração relevante dos bancos de dados do AD RMS e AD FS anteriores.

**Para restaurar os dados do SQL Server anterior ao novo servidor SQL**

1.  Faça logon no servidor com o SQL Server 2016 com a conta apropriada.

2.  No painel de navegação esquerdo, clique com botão direito **bancos de dados** e selecione **restaurar banco de dados** para iniciar o processo de restauração.

3.  Sob **fonte** escolher **dispositivo** e, em seguida, navegue até o local onde os arquivos de banco de dados foram armazenados nas etapas anteriores.

4.  Depois que os arquivos tiverem sido selecionados, clique em **Okey**.

5.  Certifique-se de que todos os arquivos de banco de dados foram adicionados e concluir o processo clicando **Okey**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configurando serviços de Federação do Active Directory do Windows Server 2016 (AD FS)

O AD FS foi implantado para fornecer um único acesso sign-on (SSO) para o AD RMS como um aplicativo. Ele também foi configurado com o AD RMS Mobile dispositivo extensão (MDE), que permite que o Mac e suporte a dispositivos móveis para os usuários finais.

As seções a seguir fornecem diretrizes sobre tarefas operacionais, que talvez você precise executar em sua implantação do AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Adicionando um servidor 2016 AD FS no farm

Você pode implantar servidores adicionais do AD FS para dar suporte a implantação do AD RMS. Você pode optar por executar esta ação no caso de aumento do tráfego para os servidores de AD RMS, ou mais aplicativos, ou se você precisar desativar um dos servidores que está sendo usados atualmente para o AD FS.

**Para adicionar o servidor 2016 AD FS no farm**

1.  No servidor do Azure AD Connect, clique duas vezes o **do Azure AD Connect** ícone para iniciar o Assistente do Azure AD Connect.

2.  Na página de boas-vinda, clique em **configurar**.

3.  Na página tarefas adicionais, clique em **implantar um servidor de Federação adicional** e, em seguida, clique em **próxima**.

4.  Na página conectar a Azure AD, insira o nome de usuário e a senha de uma conta com permissões administrativas globais e, em seguida, clique em **próxima**.

5.  Na página de credenciais de administrador de domínio, insira o nome de usuário e a senha de uma conta com permissões de administrador de domínio e clique em **próxima**.

6.  Clique em **procurar** e selecione o arquivo de certificado usado quando configurar o farm do AD FS usando o Azure AD Connect.

7.  Clique em **digite a senha** para abrir a caixa de diálogo de senha do certificado.

8.  Insira a senha do certificado no campo de senha e, em seguida, clique em **Okey**.

9.  Clique em **Avançar**.

10. Na página servidores do AD FS, insira o nome ou o endereço IP do novo servidor do AD FS e clique em **adicionar**.

11. Na página pronto para configurar, clique em **instalar**.

12. Na página Instalação concluída, clique em **Exit**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Elevar o nível de comportamento de Farm do AD FS

Ao implantar um servidor ADFs que excede o nível atual do ambiente, como tendo um AD FS no Windows Server 2012 R2 e, em seguida, adicionando que um AD FS Windows Server 2016, o nível de comportamento do Farm precisará ser aumentado. Isso é necessário para garantir que o ambiente está usando as funções e as informações mais atualizadas.

**Para aumentar o nível de comportamento de Farm do AD FS**

1.  Navegue até o ADFS do Windows Server 2016.

2.  Abra uma sessão do PowerShell de administrador.

3.  Digite o seguinte comando:  **\$cred = Get-Credential**

4.  Será exibida uma janela solicitando as credenciais, digite as credenciais de administrador de domínio.

5.  Em seguida, digite este comando: **AdfsFarmBehaviorLevelRaise invocar-credencial \$cred**

6.  Será exibido um prompt perguntando **você deseja continuar com esta operação?** Em seguida, insira **um** aceite o aviso.

7.  Depois que o comando for concluído, o nível de comportamento de Farm seja configurado e pronto.

#### <a name="enabling-mobile-device-extension-logging"></a>Habilitando o log de extensão de dispositivo móvel

A extensão de dispositivo móvel pode registrar as solicitações recebidas de dispositivos do usuário final. Registro em log está desabilitado por padrão e é recomendável habilitar somente o registro em log em um cenário de solução de problemas. Todas as solicitações de dispositivos móveis e computadores desktop, a inicialização ou adquirir uma licença de uso final são registradas no banco de dados de log do AD RMS ou conta de armazenamento do Azure. Registro em log o MDE criará duas tabelas adicionais para o SQL Server usado pelo AD RMS: o cliente depurar a tabela de log e a tabela de log de desempenho do cliente.

**Para habilitar o log de extensão de dispositivo móvel**

1.  De um servidor AD RMS, abra o Windows PowerShell como administrador.

2.  Digite o seguinte comando e pressione **Enter**: **Import-Module AdRmsAdmin**

3.  Digite o seguinte comando e pressione **Enter**: **New-PSDrive-nome AdrmsCluster - PsProvider AdRmsAdmin-raiz https://localhost**

4.  Digite o seguinte comando e pressione **Enter**: **Set-ItemProperty-caminho AdrmsCluster:\\ -IsLoggingEnabled de nome-valor \$true**

Se você estiver usando o registro em log o MDE para solução de problemas, é recomendável desabilitá-lo depois de resolver o problema.

**Para desabilitar o log de extensão de dispositivo móvel**

1.  De um servidor AD RMS, abra o Windows PowerShell como administrador.

2.  Digite o seguinte comando e pressione **Enter**: **Import-Module AdRmsAdmin**

3.  Digite o seguinte comando e pressione **Enter**: **New-PSDrive-nome AdrmsCluster - PsProvider AdRmsAdmin-raiz https://localhost**

4.  Digite o seguinte comando e pressione **Enter**: **Set-ItemProperty-caminho AdrmsCluster:\\ -IsLoggingEnabled de nome-valor \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Atualizar o AD RMS para Windows Server 2016

As seções a seguir fornecem orientação sobre como adicionar um servidor do AD RMS com base no Windows Server 2016 para o cluster do Windows Server 2012 R2 atual. O servidor será adicionado ao cluster e as informações serão replicadas a ele para que o servidor AD RMS anterior pode ser preterido para liberar recursos.

Depois de adicionar um servidor baseado no Windows Server 2016 AD RMS foi adicionado ao seu cluster AD RMS, todos os nós com base em versões mais antigas do Windows irá se tornar inativos. Após ter feito isso, você pode desprovisionar nesses servidores (por exemplo, desligar, realocar ou reinstalar o Windows Server 2016 para ingressar no cluster AD RMS). 

Você pode implantar servidores adicionais do AD RMS para o cluster para suportar a carga em sua implantação do AD RMS. Você também pode optar por executar esta ação no caso de aumento do tráfego para os servidores AD RMS.

Este guia não abrange as etapas necessárias para alterar a mecanismos que talvez você esteja usando em seu ambiente para excluir os servidores que estão sendo preteridas à e incluem aqueles que você está adicionando ao cluster de balanceamento de carga. 

#### <a name="adding-a-2016-ad-rms-server"></a>Adicionando um servidor 2016 AD RMS

Se seu cluster AD RMS estiver usando um módulo de segurança de Hardware, em vez de uma chave centralmente gerenciada para o seu certificado de licenciante para servidor, você precisará instalar o software e outros artefatos HSM (por exemplo, arquivos de chave e configuragtion) no servidor antes de instalar o AD RMS. Você também precisará conectar o HSM para o servidor, fisicamente ou por meio de configurações de rede relevantes. Siga as diretrizes de HSM para essas etapas. 

**Para adicionar um servidor RMS de AD de 2016**

1.  Instale a função do AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **executar configuração adicional**.

3.  Selecione **ingressar em um cluster existente do AD RMS** e clique em **próxima**.

4.  Sobre o **Selecionar banco de dados de configuração** página, insira o CNAME especificado no DNS para o 2016 do SQL server (FQDN).

5.  Clique em **lista** na segunda linha e selecione o **DefaultInstance** na lista suspensa.

6.  Sob **nome do banco de dados de configuração**, selecione o menu suspenso e escolha a configuração de DRMS que aparece. Em seguida, clique em **Avançar**.

7.  Sobre o **informações de banco de dados** página, insira a senha da chave de cluster no campo fornecido. Em seguida, clique **próxima**.

8.  Na próxima página do assistente, especifique a conta de serviço do AD RMS e forneça a senha para ele e clique em **próxima** depois que ele foi verificado.

9.  Uma vez a **Site do Cluster** simplesmente página aparecer, certifique-se de que o site da web apropriado foi selecionado e clique em **próxima**.

10. Sobre o **escolher um certificado de autenticação de servidor** , selecione o certificado SSL importado e clique em **próxima**.

11. Clique em **Instalar** para iniciar a instalação.

12. Após a configuração for concluída, você precisará fazer logoff e voltar para administrar o AD RMS.

13. Depois que o logon back, abra **Gerenciador de servidores** selecionar **ferramentas** e, em seguida, **Active Directory Rights Management**. A janela de gerenciamento deve aparecer e indicar que o cluster tem o servidor adicional no cluster.

14. Se a extensão de dispositivos móveis AD RMS foi instalada no cluster original do AD RMS, você precisa instalar também o MDE em nós de cluster atualizado. Siga as instruções na documentação do MDE adicionar MDE ao seu cluster AD RMS. Neste ponto, você pode realocar todos os nós preexistentes ou atualizá-los para o Windows Server 2016 e novamente uni-las para o cluster AD RMS usando o mesmo processo descrito acima. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configurando o Proxy de aplicativo do Windows Server 2016 Web (WAP)

As seções a seguir fornecem diretrizes sobre tarefas operacionais, que talvez você precise executar em sua implantação do Proxy de aplicativo Web. Isso é uma etapa opcional, não é necessária se você estiver publicando AD RMS para a Internet por meio de outros mecanismos. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Adicionando um servidor WAP do Windows Server 2016

Você pode implantar servidores de Proxy de aplicativo Web adicionais para dar suporte à implantação do AD RMS. Você pode optar por executar esta ação no caso de aumento do tráfego para os servidores do AD RMS ou se você precisar desativar um dos servidores que está sendo usados atualmente para o Proxy de aplicativo Web.

**Para adicionar um servidor de Proxy de aplicativo Web de 2016**

1.  No servidor que deseja configurar como um Proxy de aplicativo Web, navegue até o console do Gerenciador do servidor e clique em **adicionar funções e recursos**.

2.  No **assistente Adicionar funções e recursos**, clique em **próxima** até chegar à tela de seleção de função de servidor.

3.  Na tela Selecionar funções do servidor, selecione **acesso remoto**e, em seguida, clique em **próxima** até que você esteja à tela Selecionar funções do servidor.

4.  Na tela Selecionar funções do servidor, selecione **Proxy de aplicativo Web**, clique em **adicionar recursos**e, em seguida, clique em **próxima**.

5.  Na tela Confirmar seleções de instalação, clique em **instalar**.

6.  Depois que a instalação for concluída, clique em **fechar**.

7.  Agora é hora de configurar o servidor. Para fazer isso, abra o console de gerenciamento de acesso remoto no servidor de Proxy de aplicativo Web. Abra o **inicie** menu, digite **RAMgmtUI.exe**e, em seguida, selecione o aplicativo.

8.  No painel de navegação, clique em **Proxy de aplicativo Web**.

9.  No console de gerenciamento de acesso remoto, clique em **executar o Assistente de configuração de Proxy de aplicativo Web**. Uma vez no assistente, clique em **próxima**.

10. Na tela de servidor de federação, digite o nome de domínio totalmente qualificado do servidor do AD FS (ex. ADFS.contoso.com) e, em seguida, insira as credenciais para um administrador no servidor do AD FS.

11. Na tela de certificado de Proxy do AD FS, na lista de certificados atualmente instalados no servidor de Proxy de aplicativo Web, selecione um certificado a ser usado pelo Proxy de aplicativo Web para o proxy do AD FS e, em seguida, clique em **próxima**.

12. Na tela de confirmação, examine as configurações e clique em **configurar**.

13. Depois que a configuração for concluída, clique em **fechar**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuração de DNS para o 2016 servidor WAP

Depois que o servidor de Proxy de aplicativo Web do Windows Server 2016 foi colocado em vigor, algumas alterações DNS precisam ser feitas. Isso exigirá usar um serviço como o GoDaddy para apontar o ADFS e serviços do AD RMS no servidor WAP 2016.

**Para apontar o DNS no servidor WAP**

1.  Navegue até o site do seu provedor (ex. GoDaddy).

2.  Vá para gerenciamento de domínio e, em seguida, o gerenciamento de DNS.

3.  Localize o serviço AD FS e o AD RMS e substitua os **aponta para** parte com o endereço IP público do servidor WAP 2016 e **salvar**.

4.  As alterações podem levar tempo para se propagar, mas depois que eles fazem essa configuração será concluída.

#### <a name="enabling-debugging-logs"></a>Habilitar Logs de depuração

Informações de registro em log detalhado estão disponíveis nos servidores de Proxy de aplicativo Web. Você pode configurar o log de depuração avançado usando o Visualizador de eventos. Configurações adicionais também podem ser selecionadas para o tamanho dos logs para ajudar a garantir que a análise é úteis para o visualizador.

**Habilitar Logs de depuração para o Proxy de aplicativo Web**

1.  Abra o **Visualizador de eventos** console no Proxy de aplicativo Web.

2.  Expanda o **Microsoft** nó.

3.  Expanda o **Windows** nó.

4.  Abra o **Proxy de aplicativo Web** logs.

5.  Você poderá, em seguida, abra o **Admin** logs.

6.  Abra o **ação** menu, localizado no canto superior esquerdo e selecione **propriedades**.

7.  Sob o **gerais** guia, escolha a opção de **habilitar registro em log**.

8.  Por fim, será possível personalizar o tamanho máximo do log e o que acontece quando o tamanho do log de eventos máximo for atingido.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configurar alta disponibilidade para serviços do Windows Server 2016

As seções a seguir fornecem diretrizes sobre tarefas operacionais, que talvez seja necessário configurar seu ambiente do Windows Server 2016 em alta disponibilidade.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Adicionando um servidor de 2016 AD RMS para alta disponibilidade

Você pode implantar servidores adicionais do AD RMS para configurar a alta disponibilidade. Você pode optar por executar esta ação no caso de aumento do tráfego para os servidores AD RMS.

**Para adicionar um servidor de RMS do AD de 2016 para alta disponibilidade**

1.  Instale a função do AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **executar configuração adicional**.

3.  Selecione **ingressar em um cluster existente do AD RMS** e clique em **próxima**.

4.  Sobre o **Selecionar banco de dados de configuração** página, insira o CNAME especificado no DNS para o 2016 do SQL server (FQDN).

5.  Clique em **lista** na segunda linha e selecione o **DefaultInstance** na lista suspensa.

6.  Sob **nome do banco de dados de configuração**, selecione o menu suspenso e escolha a configuração de DRMS que aparece. Em seguida, clique em **Avançar**.

7.  Sobre o **informações de banco de dados** página, insira a senha da chave de cluster no campo fornecido. Em seguida, clique **próxima**.

8.  Na próxima página do assistente, especifique a conta de serviço do AD RMS e forneça a senha para ele e clique em **próxima** depois que ele foi verificado.

9.  Uma vez a **Site do Cluster** simplesmente página aparecer, certifique-se de que o site da web apropriado foi selecionado e clique em **próxima**.

10. Sobre o **escolher um certificado de autenticação de servidor** , selecione o certificado SSL importado e clique em **próxima**.

11. Clique em **Instalar** para iniciar a instalação.

12. Após a configuração for concluída, você precisará fazer logoff e voltar para administrar o AD RMS.

13. Depois que o logon back, abra **Gerenciador de servidores** selecionar **ferramentas** e, em seguida, **Active Directory Rights Management**. A janela de gerenciamento deve aparecer e indicar que o cluster tem o servidor adicional no cluster.

14. Depois de confirmar a instalação do servidor, configure seu serviço de balanceamento de carga para balancear a carga entre os diferentes servidores de AD RMS no cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Adicionando um servidor do Windows Server 2016 AD FS para alta disponibilidade

Você pode implantar servidores adicionais do AD FS para configurar a alta disponibilidade. Você pode optar por executar esta ação no caso de aumento do tráfego para servidores do AD FS. **Observação: depois de aumentar o nível de comportamento de farm, uma nova entrada de banco de dados será inserida no SQL Server 2016 (Adfs Configv3) e o antigo banco de dados de configuração deve ser excluído antes de continuar com estas etapas.**

**Para adicionar o servidor do AD FS do Windows Server 2016 para alta disponibilidade**

1.  Instale a função do AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **configurar o serviço de Federação neste servidor**.

3.  Na seção de boas-vinda do assistente, escolha a opção para **adicionar um servidor de Federação a um farm de servidores de Federação** e, em seguida, clique em **próxima**.

4.  Especifique a conta do administrador apropriado e clique em **próxima**.

5.  No **especificar Farm** página, escolher o **especifique o local do banco de dados para um farm existente usando o SQL Server** , em seguida, insira o CNAME para o serviço do SQL para o nome de Host do banco de dados e clique em **Avançar**.

6.  Sob o **especificar conta de serviço** área do assistente, insira as credenciais para a conta de serviço do AD FS e, em seguida, clique em **próxima**.

7.  Na **opções de revisão**, clique em **próxima**.

8.  Clique em **configurar** quando o botão fica disponível.

9.  Após a configuração, reinicie o computador.

10. Depois de confirmar a configuração de servidor, o balanceamento de carga em servidores do AD FS conforme necessário.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Adicionando um servidor WAP do Windows Server 2016 para alta disponibilidade

Você pode implantar servidores WAP adicionais para configurar a alta disponibilidade. Você pode optar por executar esta ação no caso de aumento do tráfego para os servidores AD RMS.

**Para adicionar um servidor de Proxy de aplicativo Web do Windows Server 2016 para alta disponibilidade**

1.  No servidor que deseja configurar como um Proxy de aplicativo Web, navegue até o console do Gerenciador do servidor e clique em **adicionar funções e recursos**.

2.  No **assistente Adicionar funções e recursos**, clique em **próxima** até chegar à tela de seleção de função de servidor.

3.  Na tela Selecionar funções do servidor, selecione **acesso remoto**e, em seguida, clique em **próxima** até que você esteja à tela Selecionar funções do servidor.

4.  Na tela Selecionar funções do servidor, selecione **Proxy de aplicativo Web**, clique em **adicionar recursos**e, em seguida, clique em **próxima**.

5.  Na tela Confirmar seleções de instalação, clique em **instalar**.

6.  Depois que a instalação for concluída, clique em **fechar**.

7.  Agora é hora de configurar o servidor. Para fazer isso, abra o console de gerenciamento de acesso remoto no servidor de Proxy de aplicativo Web. Abra o **inicie** menu, digite **RAMgmtUI.exe**e, em seguida, selecione o aplicativo.

8.  No painel de navegação, clique em **Proxy de aplicativo Web**.

9.  No console de gerenciamento de acesso remoto, clique em **executar o Assistente de configuração de Proxy de aplicativo Web**. Uma vez no assistente, clique em **próxima**.

10. Na tela de servidor de federação, digite o nome de domínio totalmente qualificado do servidor do AD FS (ex. ADFS.contoso.com) e, em seguida, insira as credenciais para um administrador no servidor do AD FS.

11. Na tela de certificado de Proxy do AD FS, na lista de certificados atualmente instalados no servidor de Proxy de aplicativo Web, selecione um certificado a ser usado pelo Proxy de aplicativo Web para o proxy do AD FS e, em seguida, clique em **próxima**.

12. Na tela de confirmação, examine as configurações e clique em **configurar**.

13. Depois que a configuração for concluída, clique em **fechar**.

14. Depois de confirmar a instalação do servidor, balanceamento de carga de servidores WAP, no DMZ.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Adicionando um nó do SQL Server 2016 para alta disponibilidade Always On

Você pode implantar servidores adicionais do SQL para configurar a alta disponibilidade Always On. Você pode optar por executar esta ação no caso de aumento do tráfego para os servidores AD RMS. **Observação: Certifique-se de que ambos os SQL Servers tem a porta de entrada 5022 aberto.**

**Para adicionar um servidor SQL server 2016 de alta disponibilidade Always On**

1.  No servidor que deseja configurar como um servidor adicional do SQL Server 2016, navegue até o console do Gerenciador do servidor e clique em **adicionar funções e recursos**.

2.  Clique em **próxima** até que o **selecionar recursos** caixa de diálogo.

3.  Selecione o **Clustering de Failover** caixa de seleção. **Observação: execute essa etapa para SQL server 2016 servidor original, bem, para que ambos os SQL Servers têm o recurso de cluster de Failover.**

4.  Clique em **instalar** para instalar o recurso de Clustering de Failover.

5.  Agora, abra **Gerenciador de servidores** e selecione **ferramentas** , em seguida, **Gerenciador de Cluster de Failover**.

6.  No painel esquerdo, clique com botão direito **Gerenciador de Cluster de Failover** e selecione **criar Cluster**

7.  Isso abrirá o **Assistente para criação de Cluster**.

8.  Procurar servidores SQL server 2016 que serão usados para alta disponibilidade Always On e insira-os em seguida, clique **próxima**.

9.  Você receberá um aviso de validação. Selecione **Yes** para validar os nós do Cluster e, em seguida, clique em **próxima**.

10. Sob o **opções de teste** , selecione a opção **executar todos os testes** e clique em **Avançar.**

11. **Observação: O Assistente de validação de Cluster deve retornar várias mensagens de aviso, especialmente se você não usará o armazenamento compartilhado. Além disso, se você encontrar quaisquer mensagens de erro você precisará corrigi-los antes de criar o Cluster de Failover do Windows Server**.

12. No **ponto de acesso para administrar o Cluster** caixa de diálogo, insira o nome do cluster e o endereço IP virtual para o Cluster de Failover do Windows Server e clique em **próxima**.

13. Verifique se a configuração foi bem-sucedida no **resumo** e clique em **concluir**.

14. Volta a **Gerenciador de Cluster de Failover** com o botão direito no seu cluster e selecione **mais ações** , em seguida, escolha **configurar quorum do Cluster**

15. Clique em **próxima** e, em seguida, escolha a opção para **selecionar a testemunha de quorum** e clique em **próxima** novamente.

16. No **selecionar testemunha de Quorum** , selecione o **configurar uma testemunha de compartilhamento de arquivos** opção. Em seguida, clique em **Avançar**.

17. Selecione **procurar** e localize o caminho do compartilhamento de arquivo que você deseja usar na caixa de diálogo caminho de compartilhamento de arquivo. Clique em **Avançar**.

18. Na página de confirmação, clique em **próxima**.

19. Na página Resumo, clique em **concluir**.

20. Agora, abra o **inicie** menu e procure **SQL Server Configuration Manager**.

21. O nome do SQL Server com o botão direito e selecione **propriedades**.

22. Na caixa de diálogo Propriedades, selecione a **alta disponibilidade AlwaysOn** guia. Verifique as **habilitar grupos de disponibilidade do AlwaysOn** caixa de seleção. Clique em **OK**. **Observação: faça a isso em ambos os SQL server 2016 servidores.**

23. Em seguida, reinicie o serviço SQL Server.

24. Agora, abra o **inicie** menu e procure **SQL Server Management Studio** e no painel de navegação esquerdo, clique com botão direito **grupos de disponibilidade** e clique em **Assistente de novo grupo de disponibilidade** , em seguida, clique em **próxima**.

25. No **especificar o nome de grupo de disponibilidade** página Escolha um nome de grupo (Ex.SQLAvailabilityGroup2016). Em seguida, clique em **Avançar**.

26. Sob o **Selecionar bancos de dados** seção, especifique os bancos de dados. Clique em Avançar. **Observação: algum banco de dados talvez precise ser feito o backup novamente ou colocar em modo de recuperação completa**.

27. Uma vez na **especificar réplicas** , clique no **para adicionar réplica** botão e selecione seu outros 2016 do SQL Server.

28. Depois de adicionar outro servidor, clique nas caixas de seleção e definir o servidor secundário para ser um secundário legível.

29. Navegue até a **pontos de extremidade** guia e clique no **atualizar** opção. Enquanto também aqui, rolar e certifique-se de que a mesma conta de serviço está no nó primário e secundário.

30. Agora, escolha o **preferências de Backup** e selecione o **preferir secundário** opção.

31. Ir para o **ouvinte** guia.

32. Especifique um nome (ex.: SQLListener) e certifique-se de que a porta esteja **1433** e, em seguida, clique em **próxima**.

33. No **selecionar sincronização de dados inicial** página do assistente, escolha o **completo** opção e especifique o local de rede acessível por todos os servidores SQL e, em seguida, clique em **próximo**.

34. Por fim, clique em **concluir** e o processo será concluído.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Encerrar nós do Windows Server 2012 R2

As seções a seguir fornecem diretrizes sobre tarefas operacionais, que talvez seja necessário remover os servidores do Windows Server 2012 R2 depois de atualizar com êxito o cluster AD RMS para Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Removendo um servidor Windows Server 2012 R2 AD RMS

Você pode remover servidores do AD RMS desnecessário após uma atualização. Você pode optar por executar esta ação quando ele se torna necessário encerrar servidores AD RMS.

**Para remover uma** **servidor AD RMS do Windows Server 2012 R2**

1.  No servidor do AD RMS do Windows Server 2012 R2 no Gerenciador do servidor, selecione **gerenciar** na parte superior direita menus e, em seguida, escolha **remover funções e recursos**.

2.  O **remover funções e recursos do assistente** será aberta para cima e na **antes de começar** tela, clique em **próxima**.

3.  Sobre o **seleção de servidor** tela, clique em **próxima**.

4.  Sobre o **funções de servidor** tela, remova a seleção ao lado **Active Directory Rights Management Services** e clique em **próxima**.

5.  Sobre o **recursos** tela, clique em **próxima**.

6.  Sobre o **confirmação** tela, clique em **remover**.

7.  Quando isso for concluído, reinicie o servidor.

8.  Agora você pode desligar esse servidor e realocar recursos conforme necessário.
