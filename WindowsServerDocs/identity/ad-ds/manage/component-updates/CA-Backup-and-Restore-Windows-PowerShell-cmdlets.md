---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: "Cmdlets CA Backup e restauração do Windows PowerShell"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets CA Backup e restauração do Windows PowerShell

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
## <a name="overview"></a>Visão geral  
O módulo ADCSAdministration Windows PowerShell foi introduzido na janela Server 2012.  Dois novos cmdlets foram adicionados para esse módulo na janela Server 2012 R2 para dar suporte a Backup e restauração de uma autoridade de certificação.  
  
-   Backup CARoleService  
  
-   Restauração CARoleService  
  
## <a name="backup-caroleservice"></a>Backup CARoleService  
**Tabela SEQ tabela \ \ \ * árabe 17: Backup e restauração Cmdlets do Windows PowerShell**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|Argumentos - **Bold** argumentos são necessários|Descrição|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadeia de caracteres - local para salvar o backup<br />– Essa é a única sem o parâmetro<br />-parâmetro posicional<br /><br />**Exemplo:**<br /><br />Backup-CARoleService.-caminho c:\adcsbackup1<br /><br />Backup CARoleService c:\adcsbackup2|  
|-KeyOnly|-Backup o certificado da CA sem o banco de dados<br /><br />**Exemplo:**<br /><br />Backup CARoleService c:\adcsbackup3 - KeyOnly|  
|-Senha|-Especifica a senha para proteger as chaves privadas e certificados de autoridade de certificação<br />-Deve ser uma cadeia de caracteres segura<br />-Não é válido com o parâmetro - DatabaseOnly<br /><br />Exemplo:<br /><br />Backup CARoleService c:\adcsbackup4-senha (Host de leitura - prompt "senha:" - AsSecureString)<br /><br />Backup CARoleService c:\adcsbackup5-senha (ConvertTo-SecureString "Pa55w0rd!" Força - AsPlainText)|  
|-DatabaseOnly|-Backup do banco de dados sem o certificado da CA<br /><br />Backup CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Força|1. permite que você substitua o backup preexists no local especificado no parâmetro - Path<br /><br />Backup CARoleService c:\adcsbackup1-força|  
|-Incremental|-Executar um backup incremental<br /><br />Backup CARoleService c:\adcsbackup7-Incremental|  
|-KeepLog|1. instrui o comando para manter os arquivos de log. Se a opção não for especificada, os arquivos de log são truncados por padrão, exceto no cenário Incremental<br /><br />Backup CARoleService c:\adcsbackup7 - KeepLog|  
  
### <a name="-password-secure-string"></a>-Senha<Secure String>  
Se as opções - Password parâmetro é usado, a senha fornecida deve ser uma cadeia de caracteres segura.  Use o **Read-Host** cmdlet inicie um prompt interativo para entrada de senha de segurança ou usar o **ConvertTo-SecureString** cmdlet para especificar uma senha em linha.  
  
Examine os seguintes exemplos  
  
**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restauração CARoleService  
**ADCSAdministration Cmdlet: Restauração-CARoleService**  
  
|Argumentos - **Bold** argumentos são necessários|Descrição|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadeia de caracteres - local para restaurar o backup de<br />– Essa é a única sem o parâmetro<br />-parâmetro posicional<br /><br />**Exemplo:**<br /><br />Restaure-CARoleService.-caminho c:\adcsbackup1-força<br /><br />Restauração CARoleService c:\adcsbackup2-força|  
|-KeyOnly|-Restaurar o certificado da CA sem o banco de dados<br />-Deve ser especificado se o backup foi feito com a opção - KeyOnly<br /><br />**Exemplo:**<br /><br />Restauração CARoleService c:\adcsbackup3 - KeyOnly-força|  
|-Senha|-Especifica a senha da CA certificados e chaves privadas<br />-Deve ser uma cadeia de caracteres segura<br /><br />**Exemplo:**<br /><br />Restauração CARoleService c:\adcsbackup4-senha (host de leitura - prompt "senha:" - AsSecureString)-força<br /><br />Restauração CARoleService c:\adcsbackup5-senha (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText - força)-força|  
|-DatabaseOnly|-Restaurar o banco de dados sem o certificado da CA<br /><br />Restauração CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Força|-Permite que você substitua as chaves preexistentes<br />-É um parâmetro opcional, mas ao restaurar no local, é provavelmente necessária<br /><br />Restauração CARoleService c:\adcsbackup1-força|  
  
### <a name="issues"></a>Problemas  
Um backup de senha não protegido é levado se a função ConvertTo-SecureString falhar enquanto estiver usando o Backup-CARoleService com o - parâmetro de senha.  
  
![CA backup e restauração](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabela SEQ tabela \ \ \ * árabe 18: erros comuns**  
  
|Ação|Erro|Comentário|  
|----------|---------|-----------|  
|**Restauração CARoleService C:\ADCSBackup**|Restauração CARoleService: O processo não pode acessar o arquivo porque ele está sendo usado por outro processo. (Exceção de HRESULT:<br /><br />0x80070020)|Interrompa o serviço de serviços de certificados do Active Directory antes de executar o cmdlet de restauração CARoleService|  
|**Restauração CARoleService C:\ADCSBackup**|Restauração CARoleService: O diretório não está vazio. (Exceção de HRESULT: 0x80070091)|Use o parâmetro - Force para substituir chaves preexistentes|  
|**Backup CARoleService C:\ADCSBackup-senha (Host de leitura - Prompt "senha:" - AsSecureString) - DatabaseOnly**|Backup-CARoleService: Conjunto de parâmetro não puder ser resolvido usando especificado parâmetros nomeados.|As opções - Password parâmetro é utilizada somente para senha proteger chaves particulares e, portanto, é inválido quando você não estiver fazendo-los para cima|  
|**Restauração CARoleService C:\ADCSBack15-senha (Host de leitura - Prompt "senha:" - AsSecureString) - DatabaseOnly**|Restauração CARoleService: Conjunto de parâmetro não puder ser resolvido usando especificado parâmetros nomeados.|As opções - Password parâmetro é utilizada somente para senha proteger as chaves privadas e, portanto, é inválido quando você não é restaurá-los|  
|**Restauração CARoleService C:\ADCSBack14-senha (Host de leitura - Prompt "senha:" - AsSecureString)**|Restauração CARoleService: O sistema não pode localizar o arquivo especificado. (Exceção de HRESULT: 0x80070002)|O caminho especificado não contém um backup do banco de dados válido.  Talvez o caminho é inválido ou o backup foi feito com a opção - KeysOnly?|  
  
## <a name="additional-resources"></a>Recursos adicionais  
[Guia de migração de serviços de certificados do Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Fazer backup de um banco de dados de autoridade de certificação e a chave privada](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restaurando o banco de dados de autoridade de certificação e a configuração no servidor de destino](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Tente o seguinte: Backup CA no laboratório usando o Windows PowerShell  
  
1.  Use os comandos nesta lição para fazer backup do banco de dados de autoridade de certificação e a chave privada protegida com uma senha.  
  
2.  Adiar a restauração da autoridade de certificação neste momento.  
  


