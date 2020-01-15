---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: Implantando Pastas de Trabalho
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: Como implantar Pastas de Trabalho, incluindo a instalação da função de servidor, a criação de compartilhamentos de sincronização e a criação de registros DNS.
ms.openlocfilehash: b8ed2578f3f55b6540315b8744bcb72523869044
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950209"
---
# <a name="deploying-work-folders"></a>Implantando Pastas de Trabalho

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

Este tópico aborda as etapas necessárias à implantação de Pastas de Trabalho. Supõe-se que você já tenha lido [Planejando uma implementação de Pastas de Trabalho](plan-work-folders.md).  
  
 Para implantar Pastas de Trabalho, um processo que pode envolver vários servidores e tecnologias, use as etapas a seguir.  
  
> [!TIP]
>  A implantação de pastas de trabalho mais simples é um único servidor de arquivos (geralmente chamado de servidor de sincronização) sem suporte para sincronização pela Internet, que pode ser uma implantação útil para um laboratório de teste ou como uma solução de sincronização para computadores cliente ingressados no domínio. Para criar uma implantação simples, siga estas etapas mínimas: 
>  -   Etapa 1: Obter certificados SSL  
>  -   Etapa 2: Criar registros DNS 
>  -   Etapa 3: Instalar Pastas de Trabalho em servidores de arquivos  
>  -   Etapa 4: Associando o certificado SSL nos servidores de sincronização
>  -   Etapa 5: Criar grupos de segurança para Pastas de Trabalho  
>  -   Etapa 7: Criar compartilhamentos de sincronização para dados de usuário  
  
## <a name="step-1-obtain-ssl-certificates"></a>Etapa 1: Obter certificados SSL  
 Pastas de Trabalho usam HTTPS para sincronizar arquivos de forma segura entre os clientes de Pastas de Trabalho e o servidor de Pastas de Trabalho. Estes são os requisitos para certificados SSL usados por Pastas de Trabalho:  
  
- O certificado deve ser emitido por uma autoridade de certificação confiável. Na maioria das implementações de Pastas de Trabalho, é recomendável uma autoridade de certificação publicamente confiável, pois os certificados serão usados por dispositivos baseados na Internet e não ingressados no domínio.  
  
- O certificado deve ser válido.  
  
- A chave privada do certificado deve ser exportável (pois você precisará instalar o certificado em vários servidores).  
  
- O nome da entidade do certificado deve conter a URL pública de Pastas de Trabalho usada para descoberta do serviço de Pastas de Trabalho pela Internet; esse nome deve estar no formato `workfolders.` *<nome do domínio>* .  
  
- Os SANs (nomes alternativos de entidade) devem estar presentes no certificado que lista o nome de cada servidor de sincronização em uso.

  O gerenciamento de certificados de pastas de trabalho [blog](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/) fornece informações adicionais sobre como usar certificados com Pastas de Trabalho.
  
## <a name="step-2-create-dns-records"></a>Etapa 2: Criar registros DNS  
 Para permitir que os usuários realizem a sincronização pela Internet, você deve criar um registro Host (A) no DNS público para permitir que clientes da Internet retornem a URL de Pastas de Trabalho. Esse registro DNS deve retornar a interface externa do servidor proxy reverso.  
  
 Na rede interna, crie um registro CNAME no DNS denominado workfolders que retornará o FDQN de um servidor de Pastas de Trabalho. Quando os clientes de pastas de trabalho usam a descoberta automática, a URL usada para descobrir o servidor de pastas de trabalho é https:\//workfolders.domain.com. Se você pretende usar a descoberta automática, o registro CNAME workfolders deve constar no DNS.  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>Etapa 3: Instalar Pastas de Trabalho em servidores de arquivos  
 Você pode instalar Pastas de Trabalho em um servidor ingressado no domínio usando o Gerenciador do Servidor ou usando o Windows PowerShell, local ou remotamente em uma rede. Isso será útil se você estiver configurando vários servidores de sincronização na rede.  
  
Para implantar a função no Gerenciador do Servidor, execute o seguinte:  
  
1.  Inicie o **Assistente de Adição de Funções e Recursos**.  
  
2.  Na página **Selecionar tipo de instalação**, escolha **Implantação baseada em função ou recurso**.  
  
3.  Na página **Selecionar servidor de destino**, selecione o servidor em que você deseja instalar Pastas de Trabalho.  
  
4.  Na página **Selecionar funções de servidor** , expanda **Serviços de Arquivo e Armazenamento**, expanda **Serviços de Arquivo e iSCSI**e selecione **Pastas de Trabalho**.  
  
5.  Quando perguntado se você deseja instalar o **Núcleo da Web Hospedável do IIS**, clique em **OK** para instalar a versão mínima do IIS (Serviços de Informações da Internet) exigida por Pastas de Trabalho.  
  
6.  Clique em **Avançar** até concluir o assistente.

Para implantar a função usando o Windows PowerShell, use o seguinte cmdlet:
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>Etapa 4: Associando o certificado SSL nos servidores de sincronização
 Pastas de Trabalho instala o Núcleo da Web Hospedável do IIS, que é um componente do IIS projetado para habilitar serviços Web sem exigir uma instalação completa do IIS. Após instalar o Núcleo da Web Hospedável do IIS, você deve associar o certificado SSL do servidor ao Site Padrão no servidor de arquivos. No entanto, o Núcleo da Web Hospedável do IIS não instala o console de Gerenciamento do IIS.

 Há duas opções para associar o certificado à Interface Web Padrão. Para usar qualquer uma das opções, você deve ter instalado a chave privada do certificado no repositório pessoal do computador.

- Utilize o console de gerenciamento do IIS em um servidor que o tenha instalado. No console, conecte-se ao servidor de arquivos que você deseja gerenciar e, em seguida, selecione o Site Padrão desse servidor. O Site Padrão aparecerá desabilitado, mas você ainda pode editar as associações do site e selecionar o certificado para associá-lo a esse site.

- Use o comando netsh para associar o certificado à interface https do Site Padrão. O comando é o seguinte:

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>Etapa 5: Criar grupos de segurança para Pastas de Trabalho
 Antes de criar compartilhamentos de sincronização, um membro dos grupos Admins. do Domínio ou Administradores de Empresa precisa criar alguns grupos de segurança no AD DS (Active Directory Domain Services) para Pastas de Trabalho (talvez eles também precisem delegar algum controle conforme descrito na Etapa 6). Aqui estão os grupos de que você precisa:

- Um grupo por compartilhamento de sincronização para especificar quais usuários podem ser sincronizados com o compartilhamento de sincronização

- Um grupo para todos os administradores de Pastas de Trabalho, de forma que possam editar um atributo em cada objeto de usuário que vincula o usuário ao servidor de sincronização correto (caso você pretenda usar vários servidores de sincronização)

  Os grupos devem seguir uma convenção de nomenclatura padrão e devem ser usados somente para Pastas de Trabalho, a fim de evitar possíveis conflitos com outros requisitos de segurança.

  Para criar os grupos de segurança apropriados, use os vários tempos de procedimento a seguir, uma vez para cada compartilhamento de sincronização e uma vez para criar opcionalmente um grupo para administradores do servidor de arquivos.

#### <a name="to-create-security-groups-for-work-folders"></a>Para criar grupos de segurança para Pastas de Trabalho

1. Abra o Gerenciador do Servidor em um computador Windows Server 2012 R2 ou Windows Server 2016 com o Centro de Administração do Active Directory instalado.

2.  No menu **Ferramentas**, clique em **Centro de Administração do Active Directory**. O Centro de Administração do Active Directory é exibido.

3.  Clique com o botão direito do mouse no contêiner em que você deseja criar o novo grupo (por exemplo, o contêiner Usuários do domínio ou unidade organizacional adequado), clique em **Novo**e em **Grupo**.

4.  Na janela **Criar Grupo** , na seção **Grupo** , especifique as seguintes configurações:

    -   Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Usuários de compartilhamento de sincronização de RH**, ou **Administradores de pastas de trabalho**.  
  
    -   No **Escopo do grupo**, clique em **Segurança**e em **Global**.  
  
5.  Na seção **Membros** , clique em **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.  
  
6.  Digite os nomes dos usuários ou grupos aos quais você deseja conceder acesso a um compartilhamento de sincronização específico (se você estiver criando um grupo para controlar o acesso a um compartilhamento de sincronização), ou digite os nomes dos administradores de Pastas de Trabalho (caso você pretenda configurar contas de usuário para descobrir automaticamente o servidor de sincronização apropriado), clique em **OK** e em **OK** novamente.

Para criar um grupo de segurança usando o Windows PowerShell, use os seguintes cmdlets:

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>Etapa 6: Delegar opcionalmente o controle de atributos de usuário para administradores de Pastas de Trabalho  
 Se você estiver implantando vários servidores de sincronização e desejar direcionar automaticamente usuários para o servidor de sincronização correto, será necessário atualizar um atributo em cada conta de usuário no AD DS. No entanto, isso geralmente requer a obtenção de um membro dos grupos Admins. do Domínio ou Administradores de Empresa para atualizar os atributos, o que pode se tornar rapidamente cansativo se você precisar adicionar usuários frequentemente ou movê-los entre servidores de sincronização.  
  
 Por esse motivo, um membro dos grupos Admins. do Domínio ou Administradores de Empresa talvez deseje delegar o recurso para modificar a propriedade msDS-SyncServerURL de objetos de usuário ao grupo Administradores de Pastas de Trabalho criado na Etapa 5, conforme descrito no procedimento a seguir.  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>Delegue o recurso para editar a propriedade msDS-SyncServerURL em objetos de usuário no AD DS  
  
1.  Abra o Gerenciador do Servidor em um computador Windows Server 2012 R2 ou Windows Server 2016 com Usuários e Computadores do Active Directory instalado.  
  
2.  No menu **Ferramentas** , clique em **Usuários e Computadores do Active Directory**. Usuários e Computadores do Active Directory é exibido.  
  
3.  Clique com o botão direito do mouse na UO em que constam todos os objetos de usuário de Pastas de Trabalho (se os usuários estiverem armazenados em várias UOs ou domínios, clique com o botão direito do mouse no contêiner comum a todos os usuários) e clique em **Delegar Controle…** . O Assistente para Delegação de Controle é exibido.  
  
4.  Na página **Usuários ou Grupos** , clique em **Adicionar…** e especifique o grupo criado para os administradores de Pastas de Trabalho (por exemplo, **Administradores de Pastas de Trabalho**).  
  
5.  Na página **Tarefas a Delegar**, clique em **Criar uma tarefa personalizada para delegar**.  
  
6.  Na página **Tipo de Objeto do Active Directory** , clique em **Somente os seguintes objetos na pasta**e marque a caixa de seleção **Objetos de usuário** .  
  
7.  Na página **Permissões** , desmarque a caixa de seleção **Geral** , marque a caixa de seleção **Propriedade Específica** e as caixas de seleção **Ler msDS-SyncServerUrl**e **Gravar msDS-SyncServerUrl** .

Para delegar o recurso para editar a propriedade msDS-SyncServerURL em objetos de usuário usando o Windows PowerShell, use o script de exemplo a seguir que faz uso do comando DsAcls.
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  A operação de delegação pode levar algum tempo para executar em domínios com um grande número de usuários.  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>Etapa 7: Criar compartilhamentos de sincronização para dados de usuário  
 Neste ponto, você está pronto para designar uma pasta no servidor de sincronização para armazenar arquivos do usuário. Essa pasta é chamada compartilhamento de sincronização e você pode usar o procedimento a seguir para criar um.  
  
1. Se você ainda não tiver um volume de NTFS com espaço livre para o compartilhamento de sincronização e os arquivos de usuário que ele contiver, crie um novo volume e formate-o com o sistema de arquivos NTFS.  
  
2. No Gerenciador do Servidor, clique em **Serviços de Arquivo e Armazenamento** e, em seguida, clique em **Pastas de Trabalho**.  
  
3. Uma lista de todos os compartilhamento de sincronização existentes aparece na parte superior do painel de detalhes. Para criar um novo compartilhamento de sincronização, no menu **Tarefas** , escolha **Novo Compartilhamento de Sincronização…** . O Assistente de Novo Compartilhamento de Sincronização é exibido.  
  
4. Na página **Selecionar o servidor e o caminho**, especifique onde armazenar o compartilhamento de sincronização. Se você já tiver um compartilhamento de arquivos criado para esses dados de usuário, será possível escolher esse compartilhamento. Se desejar, você pode criar uma nova pasta.  
  
   > [!NOTE]
   >  Por padrão, os compartilhamentos de sincronização não são diretamente acessíveis via compartilhamento de arquivos (a menos que você selecione um compartilhamento de arquivos existente). Para tornar um compartilhamento de sincronização acessível via compartilhamento de arquivos, use o bloco **Compartilhamentos** do Gerenciador do Servidor ou o cmdlet [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) para criar um compartilhamento de arquivos, preferencialmente com a enumeração baseada em acesso habilitada.  
  
5. Na página **Especificar a estrutura de pastas do usuário** , escolha uma convenção de nomenclatura para pastas de usuário no compartilhamento de sincronização. Há duas opções disponíveis:  
  
   - **Alias de usuário** cria pastas de usuário que não incluem um nome de domínio. Se você estiver usando um compartilhamento de arquivos que já está em uso com Redirecionamento de Pasta ou outra solução de dados do usuário, selecione essa convenção de nomenclatura. Opcionalmente, você pode marcar a caixa de seleção **Sincronizar apenas esta subpasta** para sincronizar apenas uma subpasta específica, como a pasta Documentos.  
  
   - <strong>alias@domain de usuário</strong> cria pastas de usuário que incluem um nome de domínio. Se você não estiver usando um compartilhamento de arquivos em uso com Redirecionamento de Pasta ou outra solução de dados do usuário, selecione essa convenção de nomenclatura para eliminar os conflitos de nomenclatura de pastas quando vários usuários do compartilhamento tiverem alias idênticos (o que pode ocorrer se os usuários pertencerem a domínios diferentes).  
  
6. Na página **Inserir o nome de compartilhamento da sincronização** , especifique um nome e uma descrição para o compartilhamento de sincronização. Isso não é anunciado na rede, mas está visível no Gerenciador do Servidor e no Windows Powershell para ajudar a distinguir um compartilhamento de sincronização do outro.  
  
7. Na página **Conceder acesso à sincronização a grupos**, especifique o grupo criado que lista os usuários permitidos a usar esse compartilhamento de sincronização.  
  
   > [!IMPORTANT]
   >  Para melhorar o desempenho e a segurança, conceda acesso a grupos, e não a usuários individuais, e seja o mais específico possível, evitando grupos genéricos, como Usuários Autenticados e Usuários do Domínio. A concessão de acesso a grupos com uma grande quantidade de usuários aumenta o tempo que as Pastas de Trabalho levam para consultar o AD DS. Se você tiver um grande número de usuários, crie vários compartilhamentos de sincronização para ajudar a dispersar a carga.  
  
8. Na página **Especificar políticas de dispositivo**, especifique se deve solicitar qualquer restrição de segurança em computadores e dispositivos clientes. Existem duas políticas de dispositivo que podem ser selecionadas individualmente:  
  
   -   **Criptografar Pastas de Trabalho** Solicita que as Pastas de Trabalho sejam criptografadas em computadores e dispositivos clientes  
  
   -   **Bloquear tela automaticamente e solicitar uma senha** Solicita que os computadores e dispositivos clientes bloqueiem automaticamente suas telas após 15 minutos, exijam uma senha de seis ou mais caracteres para desbloquear a tela e ativem um modo de bloqueio de dispositivo após 10 tentativas com falha  
  
       > [!IMPORTANT]
       >  Para impor as políticas de senha para computadores Windows 7 e para não administradores em computadores ingressados no domínio, use as políticas de senha da Política de Grupo para os domínios de computador e exclua esses domínios das políticas de senha de Pastas de Trabalho. Você pode excluir domínios usando o cmdlet [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) após criar o compartilhamento de sincronização. Para obter informações sobre como definir políticas de senha da Política de grupo, consulte [Password Policy](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx).  
  
9. Revise suas seleções e conclua o assistente para criar o compartilhamento de sincronização.

Você pode criar compartilhamentos de sincronização usando o Windows PowerShell por meio do cmdlet [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx) . Abaixo está um exemplo deste método:  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
O exemplo acima cria um novo compartilhamento de sincronização chamado *Share01* com o caminho *K:\Share-1*e o acesso concedido ao grupo denominado *Usuários de Compartilhamento de Sincronização de RH*  
  
> [!TIP]
>  Após criar compartilhamentos de sincronização, você pode utilizar a funcionalidade Gerenciador de Recursos de Servidor de Arquivos para gerenciar os dados nos compartilhamentos. Por exemplo, você pode usar o bloco **Cota** na página Pastas de Trabalho no Gerenciador do Servidor para definir cotas nas pastas de usuário. Você também pode usar o [Gerenciamento de Triagem de Arquivo](https://technet.microsoft.com/library/cc732074.aspx) para controlar os tipos de arquivos que as Pastas de Trabalho sincronizarão ou pode usar os cenários descritos em [Controle de Acesso Dinâmico](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview) para obter tarefas de classificação de arquivos mais sofisticadas.  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>Etapa 8: Opcionalmente, especifique um endereço de email de suporte técnico   
 Depois de instalar as Pastas de Trabalho em um servidor de arquivos, você provavelmente deseja especificar um endereço de email de contato administrativo para o servidor. Para adicionar um endereço de email, use o procedimento a seguir:  
  
#### <a name="specifying-an-administrative-contact-email"></a>Especificando um email de contato administrativo 
  
1.  No Gerenciador de Servidores, clique em **Serviços de Arquivo e Armazenamento**e, em seguida, clique em **Servidores**.  
  
2.  Clique com o botão direito do mouse no servidor de sincronização e clique em **Configurações de Pastas de Trabalho**. A janela Configurações de Pastas de Trabalho é exibida.  
  
3.  No painel de navegação, clique em **Email de Suporte** e digite o endereço de email ou os endereços que os usuários devem usar ao enviar um email solicitando ajuda com Pastas de Trabalho. Clique em **OK** quando tiver terminado.  
  
     Os usuários de Pastas de Trabalho podem clicar em um link no item de Painel de Controle de Pastas de Trabalho que envia um email contendo informações de diagnóstico sobre o computador cliente para o(s) endereço(s) especificado(s) aqui.  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>Etapa 9: Configurar opcionalmente a descoberta automática do servidor  
 Se você estiver hospedando vários servidores de sincronização em seu ambiente, será necessário configurar a descoberta automática de servidor preenchendo a propriedade **msDS-SyncServerURL** em contas de usuário no AD DS.  
  
>[!NOTE]
>A propriedade msDS-SyncServerURL no Active Directory não deve ser definida para usuários remotos que estão acessando Pastas de Trabalho por meio de uma solução de proxy reversa como Proxy de aplicativo Web ou Proxy de aplicativo do Azure AD. Se a propriedade msDS-SyncServerURL for definida, o cliente de pastas de trabalho tentará acessar uma URL interna que não está acessível por meio da solução de proxy reverso. Ao usar o Proxy de aplicativo Web ou Proxy de aplicativo do Azure AD, você precisa criar aplicativos de proxy exclusivos para cada servidor de Pastas de Trabalho. Para obter mais detalhes, consulte [Implantando Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: visão geral](deploy-work-folders-adfs-overview.md) ou [Implantando Pastas de Trabalho com proxy de aplicativo do Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/).


 Para que você possa fazer isso, é necessário instalar um controlador de domínio do Windows Server 2012 R2 ou atualizar esquemas de floresta e domínio usando os comandos `Adprep /forestprep` e `Adprep /domainprep`. Para obter informações sobre como executar com segurança esses comandos, consulte [Executando Adprep](https://technet.microsoft.com/library/dd464018.aspx).  
  
 Você provavelmente também precisará criar um grupo de segurança para administradores do servidor de arquivos e dar a eles permissões delegadas para modificar esse atributo de usuário específico, conforme descrito nas Etapas 5 e 6. Sem essas etapas, você precisará obter um membro do grupo Admins. do Domínio ou Administradores de Empresa para configurar a descoberta automática para cada usuário.  
  
#### <a name="to-specify-the-sync-server-for-users"></a>Para especificar o servidor de sincronização para usuários  
  
1.  Abra o Gerenciador do Servidor em um computador com Ferramentas de Administração do Active Directory instaladas.  
  
2.  No menu **Ferramentas**, clique em **Centro de Administração do Active Directory**. O Centro de Administração do Active Directory é exibido.  
  
3.  Navegue até o contêiner **Usuários** no domínio apropriado, clique com o botão direito do mouse no usuário ao qual você deseja atribuir um compartilhamento de sincronização e clique em **Propriedades**.  
  
4.  No painel de navegação, clique em **Extensões**.  
  
5.  Clique na guia **Editor de Atributos**, selecione **msDS-SyncServerUrl** e clique em **Editar**. A caixa de diálogo Editor de Cadeia de Caracteres com Valores Múltiplos é exibida.  
  
6.  Na caixa **Valor a ser adicionado** , digite a URL do servidor de sincronização com o qual você deseja sincronizar esse usuário, clique em **Adicionar**, em **OK**e em **OK** novamente.  
  
    > [!NOTE]
    >  A URL do servidor de sincronização é apenas `https://` ou `http://` (dependendo se você deseja exigir uma conexão segura) seguida pelo nome de domínio totalmente qualificado do servidor de sincronização. Por exemplo, **https:\//sync1.contoso.com**.

Para preencher o atributo de vários usuários, use o Active Directory PowerShell. Abaixo está um exemplo que preenche o atributo de todos os membros do grupo *Usuários de Compartilhamento de Sincronização de RH*, abordado na Etapa 5.
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>Etapa 10: Opcionalmente, configure Proxy de aplicativo Web, Proxy de aplicativo Azure AD ou outro proxy reverso  

Para habilitar usuários remotos para acessar seus arquivos, você precisa publicar o servidor de Pastas de Trabalho por meio de um proxy reverso, tornando as Pastas de Trabalho disponíveis externamente na Internet. Você pode usar o Proxy de aplicativo Web, Proxy de aplicativo do Azure Active Directory ou outra solução de proxy reverso.  
  
Para configurar o acesso das Pastas de Trabalho usando AD FS e o Proxy de aplicativo Web, consulte [Implantando Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web (WAP)](deploy-work-folders-adfs-overview.md). Para obter informações básicas sobre o Proxy de aplicativo Web, consulte [Proxy de aplicativo Web no Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server). Para obter detalhes sobre a publicação de aplicativos, como Pastas de Trabalho, na Internet usando o Proxy de aplicativo Web, consulte [Publicando aplicativos por meio da pré-autenticação do AD FS](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication).
 
Para configurar o acesso das Pastas de Trabalho usando o Proxy de aplicativo do Azure Active Directory, consulte [Habilitar o acesso remoto a Pastas de Trabalho usando o Proxy de aplicativo do Azure Active Directory](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>Etapa 11: Opcionalmente, use a Política de Grupo para configurar computadores ingressados no domínio  

Se você tiver um grande número de computadores ingressados no domínio nos quais deseja implantar Pastas de Trabalho, use a Política de Grupo para executar as seguintes tarefas de configuração do computador cliente:  
  
- Especificar com quais usuários do servidor de sincronização a sincronização deve ser feita  
  
- Forçar as Pastas de Trabalho a serem configuradas automaticamente, usando as configurações padrão (revise a discussão da Política de Grupo em [Designing a Work Folders Implementation](plan-work-folders.md) antes de fazer isso)  
  
  Para controlar essas configurações, crie um novo GPO (objeto de Política de Grupo) para Pastas de Trabalho e defina as seguintes configurações de Política de Grupo conforme apropriado:  
  
- Configuração de política "Especificar configurações de Pastas de Trabalho" em Configuração do Usuário\Políticas\Modelos Administrativos\Componentes do Windows\WorkFolders  
  
- Configuração de política "Forçar configuração automática para todos os usuários" em Configuração do Computador\Políticas\Modelos Administrativos\Componentes do Windows\WorkFolders  
  
> [!NOTE]
>  Essas configurações de política estão disponíveis somente ao editar a Política de Grupo de um computador que executa o Gerenciamento de Política de Grupo no Windows 8.1, no Windows Server 2012 R2 ou posterior. As versões de Gerenciamento de Política de Grupo de sistemas operacionais anteriores não têm essa configuração disponível. Essas configurações de política são aplicadas aos computadores Windows 7 no qual o aplicativo [Pastas de trabalho para o Windows 7](https://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) foi instalado.  
  
##  <a name="BKMK_LINKS"></a> Confira também  
 Para obter informações adicionais relacionadas, consulte os recursos a seguir.  
  
|Tipo de conteúdo|Referências|  
|------------------|----------------|  
|**Básicas**|-   [pastas de trabalho](work-folders-overview.md)|  
|**Planejamento**|-   [criar uma implementação de pastas de trabalho](plan-work-folders.md)|
|**Implantação**|-   [Implantando Pastas de trabalho com o AD FS e o proxy de aplicativo Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [implantação de laboratório de teste de pastas de trabalho](https://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (postagem de blog)<br />-   [um novo atributo de usuário para pastas de trabalho URL do servidor](https://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx) (postagem de blog)|  
|**Referência Técnica**|-   [logon interativo: limite de bloqueio de conta do computador](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   os [cmdlets de compartilhamento de sincronização](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|
