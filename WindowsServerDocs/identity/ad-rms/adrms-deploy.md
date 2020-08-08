---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Como atualizar o AD RMS para o Windows Server 2016
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: 8a2d0ec94619f74260f1fbc934e8e3328201ffa9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947206"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Como atualizar o AD RMS para o Windows Server 2016

## <a name="introduction"></a>Introdução

O Active Directory Rights Management Services (AD RMS) é um serviço da Microsoft que protege documentos e emails confidenciais. Ao contrário dos métodos de proteção tradicionais, como firewalls e ACLs, AD RMS a criptografia e a proteção são persistentes, independentemente de onde um arquivo vai ou como ele é transportado.

Este documento fornece diretrizes para migrar do Windows Server 2012 R2 com o SQL Server 2012 para o Windows Server 2016 e o SQL Server 2016. O mesmo processo pode ser usado para migrar de versões mais antigas, mas com suporte do AD RMS.
Observe que Active Directory Rights Management Services não está mais em desenvolvimento ativo e para os recursos mais recentes que os clientes devem considerar migrar para a [proteção de informações do Azure](https://azure.microsoft.com/services/information-protection/), que oferece um conjunto muito mais abrangente de recursos com suporte a dispositivos e aplicativos mais completos.

Para obter informações sobre como migrar para a proteção de informações do Azure de AD RMS sem precisar proteger novamente seu conteúdo, consulte [a documentação de migração da proteção de informações do Azure](/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Sobre o ambiente usado neste guia

AD FS é um componente opcional de uma instalação do AD RMS. Neste guia, o uso do ADFS é assumido. Se o ADFS não tiver sido usado em seu ambiente para dar suporte a AD RMS usuários, você poderá ignorar todas as etapas que se referem ao ADFS.

Neste guia, SQL Server é atualizado para SQL Server 2016 executando uma instalação paralela e movendo os bancos de dados por meio de um backup.
Como alternativa, se você puder atualizar seus servidores de banco de dados AD RMS e ADFS para SQL Server 2016 no local, poderá ir para a próxima seção deste documento depois de ter feito isso sem precisar seguir as etapas nesta seção.

## <a name="installation"></a>Instalação

### <a name="configuring-sql-server-2016"></a>Configurando o SQL Server 2016

A seção a seguir detalha as tarefas de implementação relacionadas diretamente à configuração do SQL Server 2016. Este guia se concentra no uso do Gerenciador do Servidor e do SQL Server Management Studio para concluir essas tarefas.

Essas etapas devem ser concluídas em uma instalação do SQL Server 2016. Instale o SQL Server 2016 no hardware adequado de acordo com as políticas e as práticas padrão da sua organização.

#### <a name="preparing-the-sql-server"></a>Preparando o SQL Server

A seção a seguir fornece detalhes sobre como preparar o SQL Server para que ele possa ser atualizado para SQL Server 2016 antes de atualizar outros serviços na plataforma AD RMS para usar o Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Adicionando CNAME para SQL Server 2016 ao DNS

O CNAME é usado para ajudar a garantir que a instalação do Windows Server 2016 obterá os dados apropriados, já que ele será apontado para o novo SQL Server 2016. **Observação: se já estiver usando um CNAME para o serviço ADFS e AD RMS, você poderá passar para as próximas etapas.**


**Para adicionar um CNAME para SQL Server 2016 ao DNS**

1.  Faça logon no controlador de domínio do Windows Server 2012 R2 com credenciais de administrador de domínio.

2.  Abra o Gerenciador de Servidor.

3.  Clique em **ferramentas** e selecione **DNS** para abrir o Gerenciador de DNS.

4.  No painel de navegação à esquerda, expanda o controlador de domínio e abra as **zonas de pesquisa direta**.

5.  Abra os recursos de domínio apropriados e clique com o botão direito do mouse no painel de exibição à direita e selecione **novo alias (CNAME)** para começar a criar o CNAME.

6.  Para o nome do alias, insira em um nome lógico para diferenciá-lo de outro que possa estar presente (por exemplo, SQLADRMS ou sqladfs)

7.  Depois de inserir o nome, forneça o FQDN para o host de destino que será o novo SQL Server servidor 2016. (ex. SQL2016.contoso.com)

8.  Depois que todas as informações forem inseridas, clique em **OK**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Fazer backup dos bancos de dados AD RMS e ADFS

Os bancos de dados AD RMS e ADFS mantêm informações críticas necessárias para AD RMS, como a chave pública do certificado de licenciante de servidor, modelos de política de direitos, dados de configuração do ADFS e informações de log. Sem esses bancos de dados, os clientes não podem emitir licenças para consumir conteúdo protegido, entre outros problemas.

Dos bancos de dados, o banco de dados de configuração do AD RMS é considerado o mais importante, pois armazena o SLC, os modelos de política de direitos, as chaves dos usuários e as informações de configuração. Portanto, embora você deva tomar cuidado para fazer backup de todos os bancos de dados AD RMS e ADFS, você deve planejar fazer backup do banco de dados de configuração regularmente.

O banco de dados de log armazena informações sobre solicitações de usuário para o cluster AD RMS para certificados e licenças de uso. Sua estratégia de backup desse banco de dados deve ser baseada na política da empresa para reter esse tipo de informação.

O banco de dados dos serviços de diretório não é essencial para AD RMS funcionalidade e, se os últimos data forem perdidos, ele repopulará com informações à medida que o servidor AD RMS receber solicitações de certificados e usar licenças. Você não precisa fazer backup desse banco de dados regularmente, mas precisa ter pelo menos uma cópia do banco de dados como foi originalmente configurado após a implantação de AD RMS.

**Para fazer backup de um banco de dados AD RMS e/ou ADFS com Microsoft SQL Server**

1.  Faça logon no servidor de banco de dados do Windows Server 2012 R2 AD RMS com o SQL 2012.

2.  Clique em **Iniciar**, em **todos os programas**, em **Microsoft SQL Server**e em **SQL Server Management Studio**.

3.  Na janela **conectar ao servidor** , confirme se o servidor que hospeda os bancos de dados do AD RMS está na caixa **nome do servidor** e clique em **conectar**.

4.  Expanda os **Bancos de dados**. Clique com o botão direito do mouse no banco de dados apropriado (**DRMs** e **ADFS**), aponte para **tarefas**e selecione **backup**.

5.  Repita a etapa 4 para os bancos de dados restantes.

6.  Verifique se o backup dos bancos de dados pode ser acessado por outros computadores na rede ou usando um dispositivo de armazenamento, pois eles serão necessários para etapas posteriores durante a migração.

Agora você pode armazenar as cópias de banco de dados em um local seguro. Lembre-se de fazer backup de seus bancos de dados com frequência.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Adicionando a conta de serviço do administrador de domínio, SQL, AD RMS e/ou ADFS ao SQL Server 2016

As etapas a seguir demonstram como adicionar as várias contas de serviço para SQL Server 2016 para auxiliar na migração dos dados do ambiente do Windows Server 2012 R2. Isso fornecerá as permissões adequadas ao tentar acessar o conteúdo e manipular os dados.

**Para adicionar o administrador de domínio, SQL, AD RMS e/ou conta de serviço do ADFS ao SQL Server**

1.  Faça logon no servidor com SQL Server 2016 como a conta de administrador local.

2.  Clique em **Iniciar**, em **todos os programas**, em **Microsoft SQL Server**e em **SQL Server Management Studio**.

3.  Na janela **conectar ao servidor** , confirme se o servidor que hospeda os bancos de dados do AD RMS está na caixa **nome do servidor** , em seguida, para autenticação, clique no menu suspenso e selecione **SQL Server autenticação**.

4.  No campo de **logon** , insira o nome da conta de administrador local (por exemplo, LocalAdmin) e, em seguida, forneça a senha apropriada e clique em **conectar**.

5.  Expanda **segurança** e clique com o botão direito do mouse em **logons** e selecione **novo logon** no menu de contexto que aparece.

6.  Depois que a janela for exibida, insira na conta do administrador de domínio no campo **nome de logon** (por exemplo, Contoso \\ ContosoAdmin)

7.  No painel de navegação à esquerda, escolha **funções de servidor**.

8.  Em seguida, marque a caixa de seleção para **sysadmin** sob as funções de servidor e clique em **OK**.

9.  Reinicie o **Gerenciamento de SQL Server**.

10. Na janela **conectar ao servidor** , confirme se o servidor que hospeda os bancos de dados do AD RMS está na caixa **nome do servidor** , em seguida, para autenticação, clique no menu suspenso e selecione **autenticação do Windows** e clique em **conectar**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restaurando os bancos de dados AD RMS e ADFS para SQL Server 2016

As etapas a seguir demonstrarão como restaurar os dados da instância de SQL Server anterior para a nova instância 2016. Isso permitirá que o novo SQL utilize os dados de configuração relevantes dos bancos de dado AD RMS e ADFS anteriores.

**Para restaurar os dados do SQL Server anterior para o novo SQL Server**

1.  Faça logon no servidor com SQL Server 2016 com a conta apropriada.

2.  No painel de navegação esquerdo, clique com o botão direito do mouse em **bancos** de dados e selecione **restaurar banco de dados** para iniciar o processo de restauração.

3.  Em **origem** , escolha **dispositivo** e procure o local onde os arquivos de banco de dados foram armazenados nas etapas anteriores.

4.  Depois que os arquivos tiverem sido selecionados, clique em **OK**.

5.  Verifique se todos os arquivos de banco de dados foram adicionados e conclua o processo clicando em **OK**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configurando o Windows Server 2016 Serviços de Federação do Active Directory (AD FS) (AD FS)

AD FS foi implantado para fornecer acesso de logon único (SSO) para AD RMS como um aplicativo. Ele também foi configurado com a AD RMS a extensão de dispositivo móvel (MDE), que habilita o suporte a dispositivos móveis e Mac para usuários finais.

As seções a seguir fornecem orientação sobre as tarefas operacionais que talvez você precise executar em sua implantação do AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Adicionando um servidor de AD FS de 2016 ao farm

Você pode implantar servidores AD FS adicionais para dar suporte à implantação de AD RMS. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS ou aplicativos adicionais ou se precisar desativar um dos servidores que estão sendo usados no momento para AD FS.

**Para adicionar o 2016 AD FS Server ao farm**

1.  No servidor de Azure AD Connect, clique duas vezes no ícone de **Azure ad Connect** para iniciar o assistente de Azure ad Connect.

2.  Na página de boas-vindas, clique em **Configurar**.

3.  Na página tarefas adicionais, clique em **implantar um servidor de Federação adicional** e, em seguida, clique em **Avançar**.

4.  Na página conectar ao Azure AD, insira o nome de usuário e a senha de uma conta com permissões administrativas globais e clique em **Avançar**.

5.  Na página credenciais de administrador de domínio, insira o nome de usuário e a senha de uma conta com permissões de administrador de domínio e clique em **Avançar**.

6.  Clique em **procurar** e selecione o arquivo de certificado usado ao configurar o farm de AD FS usando o Azure ad Connect.

7.  Clique em **digitar senha** para abrir a caixa de diálogo senha do certificado.

8.  Digite a senha do certificado no campo senha e clique em **OK**.

9.  Clique em **Próximo**.

10. Na página servidores AD FS, insira o nome ou o endereço IP do novo servidor AD FS e clique em **Adicionar**.

11. Na página pronto para configurar, clique em **instalar**.

12. Na página Instalação concluída, clique em **sair**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Gerando o nível de comportamento do farm do ADFS

Ao implantar um servidor ADFs que exceda o nível de ambiente atual, como, tendo um ADFS no Windows Server 2012 R2 e, em seguida, adicionando um Windows Server 2016 ao ADFS, o nível de comportamento do farm precisará ser aumentado. Isso é necessário para garantir que o ambiente esteja usando as informações e funções mais atualizadas.

**Para aumentar o nível de comportamento do farm do ADFS**

1.  Navegue até o ADFS do Windows Server 2016.

2.  Abra uma sessão do PowerShell de administrador.

3.  Insira o seguinte comando: ** \$ cred = Get-Credential**

4.  Uma janela será exibida solicitando credenciais, insira as credenciais de administrador de domínio.

5.  Em seguida, digite este comando: **Invoke-AdfsFarmBehaviorLevelRaise-Credential \$ creds**

6.  Um prompt será exibido perguntando **se deseja continuar com esta operação?** Em seguida, insira **um** para aceitar o prompt.

7.  Depois que o comando for concluído, o nível de comportamento do farm será configurado e estará pronto.

#### <a name="enabling-mobile-device-extension-logging"></a>Habilitando o log de extensão de dispositivo móvel

A extensão de dispositivo móvel pode registrar solicitações recebidas de dispositivos de usuário final. O registro em log está desabilitado por padrão e é recomendável habilitar o registro em log em um cenário de solução de problemas. Todas as solicitações, de dispositivos móveis e computadores desktop, para inicializar ou adquirir uma licença de uso final são registradas no banco de dados de log de AD RMS ou na conta de armazenamento do Azure. O log de MDE criará duas tabelas adicionais para o SQL Server usado pelo AD RMS: a tabela de log de depuração do cliente e a tabela de log de desempenho do cliente.

**Para habilitar o log de extensão de dispositivo móvel**

1.  Em um servidor de AD RMS, abra o Windows PowerShell como administrador.

2.  Digite o seguinte comando e pressione **Enter**: **Import-Module AdRmsAdmin**

3.  Digite o seguinte comando e pressione **Enter**: **novo-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost **

4.  Digite o seguinte comando e pressione **Enter**: **Set-ItemProperty-Path AdrmsCluster: \\ -Name IsLoggingEnabled-value \$ true**

Se você estiver usando log de MDE para solução de problemas, é recomendável desabilitá-lo depois de resolver o problema.

**Para desabilitar o log de extensão de dispositivo móvel**

1.  Em um servidor de AD RMS, abra o Windows PowerShell como administrador.

2.  Digite o seguinte comando e pressione **Enter**: **Import-Module AdRmsAdmin**

3.  Digite o seguinte comando e pressione **Enter**: **novo-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost **

4.  Digite o seguinte comando e pressione **Enter**: **Set-ItemProperty-Path AdrmsCluster: \\ -Name IsLoggingEnabled-value \$ false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Como atualizar o AD RMS para o Windows Server 2016

As seções a seguir fornecem orientação sobre como adicionar um servidor de AD RMS baseado no Windows Server 2016 ao cluster atual do Windows Server 2012 R2. O servidor será adicionado ao cluster e as informações serão replicadas para que o servidor de AD RMS anterior possa ser preterido para liberar recursos.

Depois que você adicionar um servidor AD RMS baseado no Windows Server 2016 tiver sido adicionado ao cluster AD RMS, todos os nós com base nas versões mais antigas do Windows ficarão inativos. Depois disso, você pode desprovisionar esses servidores (por exemplo, desligar, realocar ou reinstalar com o Windows Server 2016 para ingressar no cluster de AD RMS).

Você pode implantar servidores AD RMS adicionais no cluster para dar suporte à carga em sua implantação de AD RMS. Você também pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS.

Este guia não aborda as etapas necessárias para alterar os mecanismos de balanceamento de carga que você pode estar usando em seu ambiente para excluir os servidores que você está preterindo e para incluir aqueles que você está adicionando ao cluster.

#### <a name="adding-a-2016-ad-rms-server"></a>Adicionando um servidor de AD RMS de 2016

Se o cluster de AD RMS estiver usando um módulo de segurança de hardware em vez de uma chave gerenciada centralmente para seu certificado de licenciante de servidor, você precisará instalar o software e outros artefatos do HSM (por exemplo, arquivos de chave e configuragtion) no servidor antes de instalar o AD RMS. Você também precisará conectar o HSM ao servidor, seja fisicamente ou por meio das configurações de rede relevantes. Siga as orientações do HSM para essas etapas.

**Para adicionar um servidor de AD RMS de 2016**

1.  Instale a função AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **executar a configuração adicional**.

3.  Selecione **ingressar em um cluster existente do AD RMS** e clique em **Avançar**.

4.  Na página **Selecionar Banco de dados de configuração** , insira o CNAME especificado no DNS para o 2016 SQL Server (FQDN).

5.  Clique em **lista** na segunda linha e selecione o **DefaultInstance** na lista suspensa.

6.  Em **nome do banco de dados de configuração**, selecione o menu suspenso e escolha a configuração DRMs que aparece. Em seguida, clique em **Próximo**.

7.  Na página **informações do banco de dados** , insira a senha de chave do cluster no campo fornecido. Depois disso, clique em **Avançar**.

8.  Na próxima página do assistente, especifique a conta de serviço AD RMS e forneça a senha para ela e clique em **Avançar** depois que ela tiver sido verificada.

9.  Quando a página do **site do cluster** for exibida, basta garantir que o site apropriado tenha sido selecionado e clique em **Avançar**.

10. Na página **escolher um certificado de autenticação de servidor** , selecione o certificado SSL importado e clique em **Avançar**.

11. Clique em **Instalar** para iniciar a instalação.

12. Após a conclusão da configuração, será necessário fazer logoff e logon novamente para administrar AD RMS.

13. Depois de fazer logon novamente, abra **Gerenciador do servidor** selecione **ferramentas** e **Active Directory Rights Management**. A janela de gerenciamento deve aparecer e indicar que o cluster tem o servidor adicional no cluster.

14. Se a extensão de dispositivo móvel AD RMS tiver sido instalada no cluster de AD RMS original, você também precisará instalar o MDE nos nós de cluster atualizados. Siga as instruções na documentação do MDE para adicionar o MDE ao seu cluster AD RMS.
Neste ponto, você pode redefinir todos os nós preexistentes ou atualizá-los para o Windows Server 2016 e associá-los novamente ao cluster de AD RMS usando o mesmo processo descrito acima.

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configurando o proxy de aplicativo Web (WAP) do Windows Server 2016

As seções a seguir fornecem orientação sobre as tarefas operacionais que talvez você precise executar na implantação do proxy de aplicativo Web. Essa é uma etapa opcional, não necessária se você estiver publicando AD RMS na Internet por meio de outros mecanismos.

#### <a name="adding-a-windows-server-2016-wap-server"></a>Adicionando um servidor WAP do Windows Server 2016

Você pode implantar servidores de proxy de aplicativo Web adicionais para dar suporte à implantação de AD RMS. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS ou se precisar desativar um dos servidores que estão sendo usados no momento para o proxy de aplicativo Web.

**Para adicionar um servidor proxy de aplicativo Web 2016**

1.  No servidor que você deseja configurar como um proxy de aplicativo Web, navegue até o console do Gerenciador do Servidor e clique em **adicionar funções e recursos**.

2.  No **Assistente Adicionar funções e recursos**, clique em **Avançar** até chegar à tela de seleção de função de servidor.

3.  Na tela selecionar funções de servidor, selecione **acesso remoto**e clique em **Avançar** até voltar à tela selecionar funções de servidor.

4.  Na tela selecionar funções de servidor, selecione **proxy de aplicativo Web**, clique em **Adicionar recursos**e, em seguida, clique em **Avançar**.

5.  Na tela confirmar seleções de instalação, clique em **instalar**.

6.  Quando a instalação for concluída, clique em **fechar**.

7.  Agora é hora de configurar o servidor. Para fazer isso, abra o console de gerenciamento de acesso remoto no servidor proxy de aplicativo Web. Abra o menu **Iniciar** , digite **RAMgmtUI.exe**e, em seguida, selecione o aplicativo.

8.  No painel de navegação, clique em **Proxy do Aplicativo Web**.

9.  No console de gerenciamento de acesso remoto, clique em **executar o assistente de configuração de proxy de aplicativo Web**. Uma vez no assistente, clique em **Avançar**.

10. Na tela do servidor de Federação, insira o nome de domínio totalmente qualificado do servidor de AD FS (por exemplo, adfs.contoso.com) e insira as credenciais para um administrador no servidor de AD FS.

11. Na tela AD FS certificado de proxy, na lista de certificados atualmente instalados no servidor proxy de aplicativo Web, selecione um certificado a ser usado pelo proxy de aplicativo Web para AD FS proxy e clique em **Avançar**.

12. Na tela de confirmação, examine as configurações e clique em **Configurar**.

13. Quando a configuração estiver concluída, clique em **fechar**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuração de DNS para o servidor WAP 2016

Depois que o servidor proxy de aplicativo Web do Windows Server 2016 for colocado em vigor, algumas alterações de DNS precisarão ser feitas. Isso exigirá o uso de um serviço como o GoDaddy para apontar os serviços ADFS e AD RMS no servidor WAP 2016.

**Para apontar o DNS no servidor WAP**

1.  Navegue até o site do provedor (por exemplo, GoDaddy).

2.  Vá para gerenciamento de domínio e, em seguida, gerenciamento de DNS.

3.  Localize o serviço ADFS e AD RMS e substitua os **pontos em** parte pelo endereço IP público do servidor WAP 2016 e **salve**.

4.  As alterações podem levar tempo para serem propagadas, mas depois que essa configuração for concluída, a instalação será completa.

#### <a name="enabling-debugging-logs"></a>Habilitando logs de depuração

Informações detalhadas de log estão disponíveis nos servidores de proxy de aplicativo Web. Você pode configurar o log de depuração avançada usando o Visualizador de Eventos. Configurações adicionais também podem ser selecionadas para o tamanho dos logs para ajudar a garantir que a análise seja útil para o visualizador.

**Habilitando logs de depuração para o proxy de aplicativo Web**

1.  Abra o console do **Visualizador de eventos** no proxy de aplicativo Web.

2.  Expanda o nó **Microsoft** .

3.  Expanda o nó **Windows** .

4.  Abra os logs de **proxy de aplicativo Web** .

5.  Você poderá abrir os logs de **administrador** .

6.  Abra o menu **ação** , localizado na parte superior esquerda e selecione **Propriedades**.

7.  Na guia **geral** , escolha a opção para **habilitar o registro em log**.

8.  Por fim, você pode personalizar o tamanho máximo do log e o que acontece quando o tamanho máximo do log de eventos é atingido.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configurando a alta disponibilidade para os serviços do Windows Server 2016

As seções a seguir fornecem orientação sobre as tarefas operacionais que podem ser necessárias para configurar o ambiente do Windows Server 2016 em alta disponibilidade.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Adicionando um servidor de AD RMS de 2016 para alta disponibilidade

Você pode implantar servidores AD RMS adicionais para configurar a alta disponibilidade. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS.

**Para adicionar um servidor de AD RMS de 2016 para alta disponibilidade**

1.  Instale a função AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **executar a configuração adicional**.

3.  Selecione **ingressar em um cluster existente do AD RMS** e clique em **Avançar**.

4.  Na página **Selecionar Banco de dados de configuração** , insira o CNAME especificado no DNS para o 2016 SQL Server (FQDN).

5.  Clique em **lista** na segunda linha e selecione o **DefaultInstance** na lista suspensa.

6.  Em **nome do banco de dados de configuração**, selecione o menu suspenso e escolha a configuração DRMs que aparece. Em seguida, clique em **Próximo**.

7.  Na página **informações do banco de dados** , insira a senha de chave do cluster no campo fornecido. Depois disso, clique em **Avançar**.

8.  Na próxima página do assistente, especifique a conta de serviço AD RMS e forneça a senha para ela e clique em **Avançar** depois que ela tiver sido verificada.

9.  Quando a página do **site do cluster** for exibida, basta garantir que o site apropriado tenha sido selecionado e clique em **Avançar**.

10. Na página **escolher um certificado de autenticação de servidor** , selecione o certificado SSL importado e clique em **Avançar**.

11. Clique em **Instalar** para iniciar a instalação.

12. Após a conclusão da configuração, será necessário fazer logoff e logon novamente para administrar AD RMS.

13. Depois de fazer logon novamente, abra **Gerenciador do servidor** selecione **ferramentas** e **Active Directory Rights Management**. A janela de gerenciamento deve aparecer e indicar que o cluster tem o servidor adicional no cluster.

14. Depois de confirmar a configuração do servidor, configure o serviço de balanceamento de carga para balancear a carga entre os diferentes servidores de AD RMS no cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Adicionando um Windows Server 2016 AD FS Server para alta disponibilidade

Você pode implantar servidores AD FS adicionais para configurar a alta disponibilidade. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD FS. **Observação: depois de elevar o nível de comportamento do farm, uma nova entrada de banco de dados será inserida no SQL Server 2016 (ADFS Configv3) e o antigo banco de dados de configuração deverá ser excluído antes de continuar com estas etapas.**

**Para adicionar o Windows Server 2016 AD FS Server para alta disponibilidade**

1.  Instale a função AD RMS na implantação do Windows Server 2016 desejada.

2.  Após a conclusão da instalação, selecione o link para **Configurar o serviço de Federação neste servidor**.

3.  Na seção bem-vindo do assistente, escolha a opção para **Adicionar um servidor de Federação a um farm de servidores de Federação** e clique em **Avançar**.

4.  Especifique a conta de administrador apropriada e clique em **Avançar**.

5.  Na página **especificar farm** , escolha especificar o **local do banco de dados para um Farm existente usando SQL Server** em seguida, insira o CNAME para o serviço do SQL para o nome do host do banco de dados e clique em **Avançar**.

6.  Na área **especificar conta de serviço** do assistente, insira as credenciais para a conta de serviço AD FS e clique em **Avançar**.

7.  Em **Opções de revisão**, clique em **Avançar**.

8.  Clique em **Configurar** quando o botão ficar disponível.

9.  Após a configuração, reinicie o computador.

10. Depois de confirmar a configuração do servidor, equilibre a carga dos servidores AD FS conforme necessário.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Adicionando um servidor WAP do Windows Server 2016 para alta disponibilidade

Você pode implantar servidores WAP adicionais para configurar a alta disponibilidade. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS.

**Para adicionar um servidor proxy de aplicativo Web do Windows Server 2016 para alta disponibilidade**

1.  No servidor que você deseja configurar como um proxy de aplicativo Web, navegue até o console do Gerenciador do Servidor e clique em **adicionar funções e recursos**.

2.  No **Assistente Adicionar funções e recursos**, clique em **Avançar** até chegar à tela de seleção de função de servidor.

3.  Na tela selecionar funções de servidor, selecione **acesso remoto**e clique em **Avançar** até voltar à tela selecionar funções de servidor.

4.  Na tela selecionar funções de servidor, selecione **proxy de aplicativo Web**, clique em **Adicionar recursos**e, em seguida, clique em **Avançar**.

5.  Na tela confirmar seleções de instalação, clique em **instalar**.

6.  Quando a instalação for concluída, clique em **fechar**.

7.  Agora é hora de configurar o servidor. Para fazer isso, abra o console de gerenciamento de acesso remoto no servidor proxy de aplicativo Web. Abra o menu **Iniciar** , digite **RAMgmtUI.exe**e, em seguida, selecione o aplicativo.

8.  No painel de navegação, clique em **Proxy do Aplicativo Web**.

9.  No console de gerenciamento de acesso remoto, clique em **executar o assistente de configuração de proxy de aplicativo Web**. Uma vez no assistente, clique em **Avançar**.

10. Na tela do servidor de Federação, insira o nome de domínio totalmente qualificado do servidor de AD FS (por exemplo, adfs.contoso.com) e insira as credenciais para um administrador no servidor de AD FS.

11. Na tela AD FS certificado de proxy, na lista de certificados atualmente instalados no servidor proxy de aplicativo Web, selecione um certificado a ser usado pelo proxy de aplicativo Web para AD FS proxy e clique em **Avançar**.

12. Na tela de confirmação, examine as configurações e clique em **Configurar**.

13. Quando a configuração estiver concluída, clique em **fechar**.

14. Depois de confirmar a configuração do servidor, equilibre a carga dos servidores WAP na DMZ.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Adicionando um nó SQL Server 2016 para Always On alta disponibilidade

Você pode implantar servidores SQL adicionais para a instalação Always On alta disponibilidade. Você pode optar por executar essa ação no caso de aumento de tráfego para os servidores de AD RMS. **Observação: Verifique se os dois SQL Servers têm a porta de entrada 5022 aberta.**

**Para adicionar um servidor do SQL Server 2016 para alta disponibilidade do Always On**

1.  No servidor que você deseja configurar como um servidor SQL Server 2016 adicional, navegue até o console do Gerenciador do Servidor e clique em **adicionar funções e recursos**.

2.  Clique em **Avançar** até a caixa de diálogo **selecionar recursos** .

3.  Marque a caixa de seleção **clustering de failover** . **Observação: Siga esta etapa para o servidor do SQL Server 2016 original, para que ambos os servidores SQL tenham o recurso de clustering de failover.**

4.  Clique em **instalar** para instalar o recurso de cluster de failover.

5.  Agora, abra **Gerenciador do servidor** e selecione **ferramentas** e **Gerenciador de cluster de failover**.

6.  No painel de menu à esquerda, clique com o botão direito do mouse em **Gerenciador de cluster de failover** e selecione **criar cluster**

7.  Isso abrirá o **Assistente para criar cluster**.

8.  Procure os servidores do SQL Server 2016 que serão usados para Always On alta disponibilidade e insira-os em e clique em **Avançar**.

9.  Você receberá um aviso de validação. Selecione **Sim** para validar os nós de cluster e clique em **Avançar**.

10. Na página **Opções de teste** , selecione a opção **executar todos os testes** e clique em **Avançar.**

11. **Observação: espera-se que o assistente de validação de cluster retorne várias mensagens de aviso, especialmente se você não estiver usando o armazenamento compartilhado. Além disso, se você encontrar qualquer mensagem de erro, será necessário corrigi-las antes de criar o cluster de failover do Windows Server**.

12. Na caixa de diálogo **ponto de acesso para administrar o cluster** , insira o nome do cluster e o endereço IP virtual para o cluster de failover do Windows Server e clique em **Avançar**.

13. Verifique se a configuração foi bem-sucedida em **Resumo** e clique em **concluir**.

14. De volta ao **Gerenciador de cluster de failover,** clique com o botão direito do mouse no cluster e selecione **mais ações** e escolha **definir configurações de quorum do cluster**

15. Clique em **Avançar** e selecione a opção para **selecionar a testemunha de quorum** e clique **em Avançar** novamente.

16. Na página **selecionar testemunha de quorum** , selecione a opção **Configurar uma testemunha de compartilhamento de arquivos** . Em seguida, clique em **Próximo**.

17. Selecione **procurar** e localize o caminho do compartilhamento de arquivos que você deseja usar na caixa de diálogo caminho de compartilhamento de arquivos. Clique em **Próximo**.

18. Na página Confirmação, clique em **Avançar**.

19. Na página Resumo, clique em **Concluir**.

20. Agora, abra o menu **Iniciar** e procure **SQL Server Configuration Manager**.

21. Clique com o botão direito do mouse no nome do SQL Server e selecione **Propriedades**.

22. Na caixa de diálogo Propriedades, selecione a guia **alta disponibilidade AlwaysOn** . Marque a caixa de seleção **habilitar grupos de disponibilidade AlwaysOn** . Clique em **OK**. **Observação: faça isso nos servidores do SQL Server 2016.**

23. Em seguida, reinicie o serviço do SQL Server.

24. Agora, abra o menu **Iniciar** e procure **SQL Server Management Studio** e, no painel de navegação esquerdo, clique com o botão direito do mouse em **grupos de disponibilidade** e clique em assistente de **novo grupo de disponibilidade** e clique em **Avançar**.

25. Na página **especificar nome do grupo de disponibilidade** , escolha um nome de grupo (por exemplo, SQLAvailabilityGroup2016). Em seguida, clique em **Próximo**.

26. Na seção **selecionar bancos de dados** , especifique os bancos de dados. Clique em Avançar. **Observação: Talvez seja necessário fazer backup de algum banco de dados novamente ou colocá-lo em modo de recuperação completa**.

27. Uma vez na página **especificar réplicas** , clique no botão **Adicionar réplica** e escolha o outro SQL Server de 2016.

28. Depois de adicionar o outro servidor, clique nas caixas de seleção e defina o servidor secundário como um secundário legível.

29. Navegue até a guia **pontos de extremidade** e clique na opção **Atualizar** . Além disso, role para baixo e verifique se a mesma conta de serviço está no nó primário e secundário.

30. Agora, escolha a guia **preferências de backup** e selecione a opção **preferir secundário** .

31. Vá para a guia **ouvinte** .

32. Especifique um nome (ex. Sqllistener) e verifique se a porta é **1433** e clique em **Avançar**.

33. Na página **selecionar sincronização de dados inicial** do assistente, escolha a opção **completo** e especifique o local de rede acessível por todos os SQL Servers e clique em **Avançar**.

34. Por fim, clique em **concluir** e o processo será concluído.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Encerrar nós do Windows Server 2012 R2

As seções a seguir fornecem orientação sobre as tarefas operacionais que talvez você precise remover os servidores Windows Server 2012 R2 depois de atualizar com êxito o cluster de AD RMS para o Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Removendo um servidor de AD RMS do Windows Server 2012 R2

Você pode remover servidores AD RMS desnecessários após uma atualização. Você pode optar por executar essa ação quando for necessário descomissionar AD RMS servidores.

**Para remover um servidor de** **AD RMS do Windows Server 2012 R2**

1.  No servidor AD RMS do Windows Server 2012 R2 em Gerenciador do Servidor, selecione **gerenciar** nos menus superior direito e escolha **remover funções e recursos**.

2.  O **Assistente para remover funções e recursos** será aberto e, na tela **antes de começar** , clique em **Avançar**.

3.  Na tela de **seleção de servidor** , clique em **Avançar**.

4.  Na tela **funções de servidor** , remova a marca de seleção ao lado de **Active Directory Rights Management Services** e clique em **Avançar**.

5.  Na tela **recursos** , clique em **Avançar**.

6.  Na tela de **confirmação** , clique em **remover**.

7.  Quando isso for concluído, reinicie o servidor.

8.  Agora você pode desligar este servidor e realocar os recursos conforme necessário.
