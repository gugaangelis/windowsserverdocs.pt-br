---
title: Solução de problemas AD FS-conectividade do SQL
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.openlocfilehash: 2fb32d5b553b4d248c718fac766a83daa5dfedb2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964813"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Solução de problemas AD FS-conectividade do SQL
AD FS fornece a capacidade de usar SQL Server remotos para os dados do AD FS farm.  Você verá problemas se os servidores de AD FS em seu farm não puderem se comunicar com os SQL Servers de back-end.  O documento a seguir fornecerá algumas etapas básicas para testar a comunicação com os servidores de back-end.

## <a name="acquire-the-sql-database-connection-string"></a>Adquirir a cadeia de conexão do banco de dados SQL
A primeira coisa a ser testada ao verificar a conectividade do SQL é, se AD FS tiver as informações de conexão SQL corretas.  Isso pode ser feito usando o PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Para adquirir a cadeia de conexão SQL
1.  Abrir o Windows PowerShell
2. Insira o seguinte: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` e pressione Enter
3. Insira o seguinte: `$adfs.ConfigurationDatabaseConnectionString` e pressione Enter.
4. Você deve ver as informações da cadeia de conexão.

![Comando de execução da tela de comando do PowerShell](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Criar um arquivo de Universal Data Link (UDL) para testar a conectividade
Um arquivo de Universal Data Link ou um arquivo UDL é basicamente um arquivo de texto que contém uma cadeia de conexão de banco de dados.  Usando as informações que obtivemos acima, podemos testar se o SQL Server está respondendo a conexões ou não.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Para criar um arquivo UDL para testar a conectividade

1. Abra o bloco de notas e salve o arquivo como Test. udl.  Verifique se você tem **todos os arquivos** selecionados na lista suspensa para **salvar como tipo**.
2. Clique duas vezes em Test. udl
3. Preencha as seguintes informações: a. **Selecione ou insira um nome de servidor:**  Use a fonte de dados da cadeia de conexão acima de b. **Insira as informações para fazer logon no servidor:**  Use a conta de serviço do AD FS ou uma conta que tenha permissões para fazer logon remotamente.  Se a conta for uma conta do Windows, use a autenticação integrada, caso contrário, digite o nome de usuário e a senha.
    c. **Selecione o banco de dados no servidor:** Use o catálogo inicial da cadeia de caracteres acima.  Exemplo: AdfsConfigurationV3.
   ![Testar Conexão](media/ad-fs-tshoot-sql/sql4.png)
1. Clique em **Testar Conexão**.</br>
![Êxito](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Usar SQL Server Management Studio para testar a conectividade
Você também pode [baixar](https://go.microsoft.com/fwlink/?linkid=864329) e instalar o SSMS para testar a conectividade do banco de dados.

### <a name="to-test-connectivity-with-ssms"></a>Para testar a conectividade com o SSMS
1. Baixe e instale o SQL Server Management Studio.
![Instalar](media/ad-fs-tshoot-sql/sql5.png)
1. Abra o SSMS, insira o nome do servidor.  A fonte de dados acima.
2. Use a conta de serviço do AD FS ou uma conta que tenha permissões para fazer logon remotamente.  Se a conta for uma conta do Windows, use a autenticação integrada, caso contrário, digite o nome de usuário e a senha.
![Connect](media/ad-fs-tshoot-sql/sql6.png)
1. Você deve ver o lado esquerdo preenchido.  Expanda bancos de dados e verifique se você vê os bancos de dados do AD FS.
![Bancos de dados AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)