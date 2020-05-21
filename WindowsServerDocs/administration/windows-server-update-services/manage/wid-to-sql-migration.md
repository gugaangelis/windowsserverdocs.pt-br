---
title: Migrando o banco de dados do WSUS do WID (banco de dados interno do Windows) para o SQL
description: Tópico Windows Server Update Service (WSUS)-como migrar o banco de dados do WSUS (SUSDB) de uma instância de banco de dados interna do Windows para uma instância local ou remota do SQL Server.
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: facd846dd0c20ee2e5001b0592651ce310e19097
ms.sourcegitcommit: 29f7a4811b4d36d60b8b7c55ce57d4ee7d52e263
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83716881"
---
# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migrando o banco de dados do WSUS do WID para o SQL

> Aplica-se a: Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Use as etapas a seguir para migrar o banco de dados do WSUS (SUSDB) de uma instância de banco de dados interna do Windows para uma instância local ou remota do SQL Server.

## <a name="prerequisites"></a>Pré-requisitos

- Instância do SQL. Isso pode ser o padrão **MSSQLSERVER** ou uma instância personalizada.
- SQL Server Management Studio
- WSUS com função WID instalada
- IIS (normalmente, isso é incluído quando você instala o WSUS por meio de Gerenciador do Servidor). Ele ainda não está instalado, será necessário.

## <a name="migrating-the-wsus-database"></a>Migrando o banco de dados do WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Parar os serviços IIS e WSUS no servidor WSUS

No PowerShell (elevado), execute:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Desanexar SUSDB do banco de dados interno do Windows

#### <a name="using-sql-management-studio"></a>Usando o SQL Management Studio

1. Clique com o botão direito do mouse em tarefas **SUSDB** - &gt; **Tasks** - &gt; clique em **desanexar**: ![ image1](images/image1.png)
2. Marque a caixa de seleção **remover conexões existentes** e clique em **OK** (opcional, se houver conexões ativas).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Usando o Prompt de Comando

> [!IMPORTANT]
> Estas etapas mostram como desanexar o banco de dados do WSUS (SUSDB) da instância do banco de dados interno do Windows usando o utilitário **sqlcmd** . Para obter mais informações sobre o utilitário **sqlcmd** , consulte [sqlcmd Utility](https://go.microsoft.com/fwlink/?LinkId=81183).
> 1. Abrir um prompt de comandos com privilégios elevados
> 2. Execute o seguinte comando SQL para desanexar o banco de dados do WSUS (SUSDB) da instância do banco de dados interno do Windows usando o utilitário **sqlcmd** :

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copie os arquivos SUSDB para o SQL Server

1. Copie **SUSDB. MDF** e **SUSDB \_ log. ldf** da pasta de dados wid (**% systemdrive%** \\ **Windows \\ wid \\ Data**) para a pasta de dados da instância do SQL.

> [!TIP]
> Por exemplo, se a pasta da instância do SQL for **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, e a pasta de dados wid é **C:\Windows\WID\Data,** Copie os arquivos SUSDB de **C:\WINDOWS\WID\DATA** para **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Anexar SUSDB à instância do SQL

1. No **SQL Server Management Studio**, no nó **instância** , clique com o botão direito do mouse em **bancos de dados**e clique em **anexar**.
    ![image3](images/image3.png)
2. Na caixa **anexar bancos de dados** , em **bancos de dados a serem anexados**, clique no botão **Adicionar** e localize o arquivo **SUSDB. MDF** (copiado da pasta wid) e clique em **OK**.
    ![image4 ](images/image4.png) ![ image5](images/image5.png)

> [!TIP]
> Isso também pode ser feito usando o Transact-SQL.  Consulte a [documentação do SQL para anexar um banco de dados](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) para obter suas instruções.
>
> Exemplo (usando caminhos do exemplo anterior):
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Verificar os logons e as permissões do SQL Server e do banco de dados

#### <a name="sql-server-login-permissions"></a>SQL Server permissões de logon

Depois de anexar o SUSDB, verifique se o **NT Authority\Network Service** tem permissões de logon para a instância do SQL Server fazendo o seguinte:

1. Entrar em SQL Server Management Studio
2. Abrindo a instância
3. Clique em **segurança**
4. Clique em **logons**

A conta **NT Authority\Network Service** deve ser listada. Se não for, você precisará adicioná-lo adicionando um novo nome de logon.

> [!IMPORTANT]
> Se a instância do SQL estiver em um computador diferente do WSUS, a conta de computador do servidor do WSUS deverá ser listada no formato **[FQDN] \\ [WSUSComputerName] $**.  Caso contrário, as etapas abaixo podem ser usadas para adicioná-lo, substituindo o **serviço NT AUTHORITY\NETWORK** pela conta de computador do servidor do WSUS (**[FQDN] \\ [WSUSComputerName] $**) isso seria ***adicional ao*** conceder direitos ao **NT Authority\Network Service**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Adicionando NT AUTHORITY\NETWORK SERVICE e concedendo direitos de ti

1. Clique com o botão direito do mouse em **logons** e clique em **novo logon...**
    ![image6](images/image6.png)
2. Na página **geral** , preencha o nome de **logon** (**NT Authority\Network Service**) e defina o banco de **dados padrão** como SUSDB.
    ![image7](images/image7.png)
3. Na página **funções de servidor** , verifique se **público** e **sysadmin** estão selecionados.
    ![image8](images/image8.png)
4. Na página **mapeamento de usuário** :
    - Em **Usuários mapeados para este logon**: selecione **SUSDB**
    - Em **Associação de função de banco de dados para: SUSDB**, verifique se o seguinte está marcado:
        - **publicada**
        - **WebService** ![ image9](images/image9.png)
5. Clique em **OK**

Agora você deve ver **NT Authority\Network Service** em logons.
![image10](images/image10.png)

#### <a name="database-permissions"></a>Permissões de banco de dados

1. Clique com o botão direito do mouse no SUSDB
2. Selecionar **Propriedades**
3. **Permissões** de clique

A conta **NT Authority\Network Service** deve ser listada.

1. Se não estiver, adicione a conta.
2. Na caixa de texto nome de logon, insira o computador do WSUS no seguinte formato:
    > [**FQDN] \\ [WSUSComputerName] $**
3. Verifique se o **banco de dados padrão** está definido como **SUSDB**.

    > [!TIP]
    > No exemplo a seguir, o FQDN é **Contosto.com** e o nome da máquina do WSUS é **WsusMachine**:
    >
    > ![image11](images/image11.png)

4. Na página **mapeamento de usuário** , selecione o banco de dados **SUSDB** em **Usuários mapeados para este logon**
5. Marque **WebService** na **associação da função de banco de dados para: SUSDB**: ![ IMAGE12](images/image12.png)
6. Clique em **OK** para salvar as configurações.
    > [!NOTE]
    > Talvez seja necessário reiniciar o serviço SQL para que as alterações entrem em vigor.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Editar o registro para apontar o WSUS para a instância de SQL Server

> [!IMPORTANT]
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.

1. Clique em **Iniciar**, clique em **Executar**, digite **regedit&** e clique em **OK**.
2. Localize a seguinte chave: **HKEY_LOCAL_MACHINE \software\microsoft\updateservices\server\setup\sqlservername**
3. Na caixa de texto **valor** , digite **[servername] \\ [InstanceName]** e clique em **OK**. Se o nome da instância for a instância padrão, digite **[servername]**.
4. Localize a seguinte chave: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed role Services\UpdateServices-WidDatabase** ![ image13](images/image13.png)
5. Renomeie a chave para **updateservices-Database** ![ image41](images/image14.png)

    > [!NOTE]
    > Se você não atualizar essa chave, o **WsusUtil** tentará atender ao wid em vez da instância do SQL para a qual você migrou.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Iniciar os serviços IIS e WSUS no servidor do WSUS

No PowerShell (elevado), execute:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Se você estiver usando o console do WSUS, feche-o e reinicie-o.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Desinstalando a função WID (não recomendado)

> [!WARNING]
> A remoção da função WID também remove uma pasta de banco de dados (**%systemdrive%\Arquivos de Files\Update Services\Database**) que contém os scripts exigidos pelo WSUSutil. exe para tarefas pós-instalação. Se você optar por desinstalar a função WID, certifique-se de fazer backup da pasta **%systemdrive%\Arquivos de Files\Update Services\Database** com antecedência.

Usando o PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Depois que a função WID for removida, verifique se a seguinte chave do registro está presente: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed role Services\UpdateServices-Database**
