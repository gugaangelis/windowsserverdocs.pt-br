---
title: Solucionando problemas do AD FS - conectividade do SQL
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a4b7a568200bee7c2696c57f1dd964dd4e84ec21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820257"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Solucionando problemas do AD FS - conectividade do SQL
O AD FS fornece a capacidade de usar o remote SQL Server para os dados do farm do AD FS.  Você verá problemas se os servidores do AD FS no farm não podem se comunicar com os servidores SQL de back-end.  O seguinte documento fornecerá algumas etapas básicas para testar a comunicação com os servidores de back-end.

## <a name="acquire-the-sql-database-connection-string"></a>Adquirir a cadeia de caracteres de conexão de banco de dados SQL
A primeira coisa a testar durante a verificação de conectividade do SQL é, se o AD FS tem as informações de conexão SQL corretas.  Isso pode ser feito usando o PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Para obter a cadeia de caracteres de conexão do SQL
1.  Abrir do Windows PowerShell
2. Insira o seguinte: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` e pressionar Enter
3. Insira o seguinte: `$adfs.ConfigurationDatabaseConnectionString` e pressione enter.
4. Você deve ver as informações da cadeia de conexão.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Crie um arquivo de Universal Data Link (UDL) para testar a conectividade
Um arquivo de Link de dados Universal ou um arquivo UDL é basicamente um arquivo de texto que contém a uma cadeia de caracteres de conexão de banco de dados.  Usando as informações que obtivemos acima podemos testar se o SQL server está respondendo às conexões.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Para criar um arquivo udl para testar a conectividade

1. Abra o bloco de notas e salve o arquivo como udl.  Certifique-se de que você tenha **todos os arquivos** selecionado na lista suspensa para **Salvar como tipo**.
2. Clique duas vezes no UDL
3. Preencha as seguintes informações: um. **Selecione ou insira um nome de servidor:**  Use a fonte de dados da cadeia de conexão acima b. **Insira informações para fazer logon no servidor:**  Use a conta de serviço do AD FS ou uma conta que tenha permissões para fazer logon remotamente.  Se a conta é um uso de conta do windows integrado autenticação caso contrário, insira o nome de usuário e senha.
    c. **Selecione o banco de dados no servidor:** Use o catálogo inicial da cadeia de caracteres acima.  Exemplo:  AdfsConfigurationV3.
   ![Conexão de teste](media/ad-fs-tshoot-sql/sql4.png)
1. Clique em **Testar Conexão**.</br>
![Sucesso](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Use o SQL Server Management Studio para testar a conectividade
Você também pode [baixar](https://go.microsoft.com/fwlink/?linkid=864329) e instale o SSMS para testar a conectividade de banco de dados.

###<a name="to-test-connectivity-with-ssms"></a>Para testar a conectividade com o SSMS
1. Baixe e instale o SQL Server Management Studio.
![Instalar](media/ad-fs-tshoot-sql/sql5.png)
1. Abra o SSMS, insira o nome do servidor.  A fonte de dados acima.
2. Use a conta de serviço do AD FS ou uma conta que tenha permissões para fazer logon remotamente.  Se a conta é um uso de conta do windows integrado autenticação caso contrário, insira o nome de usuário e senha.
![Connect](media/ad-fs-tshoot-sql/sql6.png)
1. Você deve ver o lado esquerdo preenchido.  Expanda bancos de dados e verifique se os bancos de dados do AD FS.
![Bancos de dados do AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)