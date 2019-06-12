---
title: Migrando o banco de dados WSUS (Windows Internal Database) WID to SQL
description: Tópico do Windows Server Update Service (WSUS) - como migrar o banco de dados do WSUS (SUSDB) de uma instância de banco de dados interno do Windows para uma instância Local ou remota do SQL Server.
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 9015bbc54a4c4bda0f691b79dbb7d3ba8ddbc4a1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439897"
---
>Aplica-se a: Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migrando o banco de dados do WSUS do WID para SQL

Use as seguintes etapas para migrar o banco de dados do WSUS (SUSDB) de uma instância de banco de dados interno do Windows para uma instância Local ou remota do SQL Server.

## <a name="prerequisites"></a>Pré-requisitos

- SQL Instance. Isso pode ser o padrão **MSSQLServer** ou uma instância personalizada.
- SQL Server Management Studio
- WSUS com função WID instalada
- IIS (Isso é normalmente incluído quando você instala o WSUS por meio do Gerenciador do servidor). Ele não estiver instalado, ele precisará ser.

## <a name="migrating-the-wsus-database"></a>Migrando o banco de dados do WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Parar os serviços IIS e o WSUS no servidor do WSUS

No PowerShell (elevado), execute:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Desanexar SUSDB do banco de dados interno do Windows

#### <a name="using-sql-management-studio"></a>Usando o SQL Management Studio

1. Clique com botão direito **SUSDB** - &gt; **tarefas** - &gt; clique em **desanexar**: ![image1](images/image1.png)
2. Verifique **descartar conexões existentes** e clique em **Okey** (opcional, se existirem conexões ativas).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Usando o Prompt de Comando

> [!IMPORTANT]
> Estas etapas mostram como desanexar o banco de dados do WSUS (SUSDB) da instância do banco de dados interno do Windows usando o **sqlcmd** utilitário. Para obter mais informações sobre o **sqlcmd** utilitário, consulte [utilitário sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
> 1. Abra um prompt de comando elevado
> 2. Execute o seguinte comando SQL para desanexar o banco de dados do WSUS (SUSDB) da instância do banco de dados interno do Windows usando o **sqlcmd** utilitário:

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

1. Cópia **susdb. mdf** e **SUSDB\_log** da pasta de dados WID ( **% SystemDrive %** \** Windows\WID\Data * *) para os dados da instância SQL Pasta.

> [!TIP]
> Por exemplo, se sua pasta de instância do SQL for **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, e é a pasta de dados WID **C:\Windows\WID\Data,** copie os arquivos SUSDB da **C:\Windows\WID\Data** para **C:\Program Files\Microsoft SQL Server \MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Anexar SUSDB à instância do SQL

1. No **SQL Server Management Studio**, sob o **instância** nó, clique com botão direito **bancos de dados**e, em seguida, clique em **anexar**.
    ![image3](images/image3.png)
2. No **anexar bancos de dados** caixa, em **bancos de dados para anexar**, clique no **Add** e localize o **susdb. mdf** arquivo (copiado do Pasta de trabalho) e, em seguida, clique em **Okey**.
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> Isso também é capaz de ser feito usando o Transact-Sql.  Consulte a [documentação do SQL para anexar um banco de dados](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) para suas instruções.
>
> Exemplo (usando os caminhos do exemplo anterior):
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

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Verifique se o SQL Server e os logons de banco de dados e permissões

#### <a name="sql-server-login-permissions"></a>Permissões de logon do SQL Server

Depois de anexar o SUSDB, verifique **NT AUTHORITY\NETWORK SERVICE** tem permissões de logon para a instância do SQL Server, fazendo o seguinte:

1. Ir para o SQL Server Management Studio
2. Abrindo a instância
3. Clique em **segurança**
4. Clique em **logons**

O **NT AUTHORITY\NETWORK SERVICE** conta deve estar listada. Se não for, você precisará adicioná-lo adicionando o novo nome de logon.

> [!IMPORTANT]
> Se a instância do SQL está em um computador diferente do WSUS, conta de computador do servidor do WSUS deve ser listada no formato **[FQDN]\\[WSUSComputerName] $** .  Se não, as etapas a seguir podem ser usadas para adicioná-lo, substituindo **NT AUTHORITY\NETWORK SERVICE** com a conta de computador do servidor WSUS ( **[FQDN]\\[WSUSComputerName] $** ), isso deveria ser ***além*** conceder direitos a **NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Adicionando NT AUTHORITY\NETWORK SERVICE e conceder a ele direitos

1. Clique com botão direito **logons** e clique em **novo logon...**
    ![image6](images/image6.png)
2. No **gerais** página, preencha o **nome de logon** (**NT AUTHORITY\NETWORK SERVICE**) e defina o **banco de dados padrão** para SUSDB.
    ![image7](images/image7.png)
3. No **funções de servidor** página, verifique se **pública** e **sysadmin** estão selecionados.
    ![image8](images/image8.png)
4. Sobre o **mapeamento de usuário** página:
    - Sob **usuários mapeados para este logon**: selecione **SUSDB.**
    - Sob **associação de função para o banco de dados: SUSDB**, verifique se as seguintes são verificadas:
        - **public**
        - **webService** ![image9](images/image9.png)
5. Clique em **OK**.

Agora você deve ver **NT AUTHORITY\NETWORK SERVICE** em logons.
![image10](images/image10.png)

#### <a name="database-permissions"></a>Permissões de banco de dados

1. Clique com botão direito do SUSDB.
2. Selecione **propriedades**
3. Clique em **permissões**

O **NT AUTHORITY\NETWORK SERVICE** conta deve estar listada.

1. Se não for, adicione a conta.
2. Na caixa de texto de nome de logon, insira a máquina do WSUS no seguinte formato:
    > [**FQDN]\\[WSUSComputerName]$**
3. Verifique se que o **banco de dados padrão** é definido como **SUSDB**.

    > [!TIP]
    > No exemplo a seguir, é o FQDN **Contosto.com** e é o nome do computador WSUS **WsusMachine**:
    >
    > ![image11](images/image11.png)

4. Sobre o **mapeamento de usuário** página, selecione o **SUSDB** do banco de dados em **"Usuários mapeados para este logon"**
5. Verifique **webservice** sob o **"associação de função para o banco de dados: SUSDB"** : ![image12](images/image12.png)
6. Clique em **Okey** ao salvar as configurações.
    > [!NOTE]
    > Talvez você precise reiniciar o serviço do SQL para que as alterações entrem em vigor.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Editar o registro para o ponto de WSUS para a instância do SQL Server

> [!IMPORTANT]
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756) no caso de problemas.

1. Clique em **Iniciar**, **Executar**, digite **regedit** e clique em **OK**.
2. Localize a seguinte chave: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. No **valor** caixa de texto, digite **[ServerName]\\[NomedaInstância]** e, em seguida, clique em **Okey**. Se o nome da instância for a instância padrão, digite **[ServerName]** .
4. Localize a seguinte chave: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. Renomear a chave a ser **UpdateServices-banco de dados** ![image41](images/image14.png)

    > [!NOTE]
    > Se você não atualizar essa chave, em seguida, **WsusUtil** tentará o WID, em vez da instância de SQL para o qual você migrou do serviço.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Inicie os serviços IIS e o WSUS no servidor do WSUS

No PowerShell (elevado), execute:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Se você estiver usando o Console do WSUS, feche e reinicie-o.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Desinstalando a função WID (não recomendada)

> [!WARNING]
> Removendo a função do WID também remove uma pasta de banco de dados ( **%SystemDrive%\Program programas\update Services\Database**) que contém os scripts necessários para WSUSUtil.exe para tarefas de pós-instalação. Se você optar por desinstalar a função WID, verifique se você fazer backup de **%SystemDrive%\Program programas\update Services\Database** pasta com antecedência.

Usando o PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Depois que a função WID é removida, verifique se a seguinte chave do registro está presente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-Database**