---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlets de autoridade de certificação de Backup e restauração do Windows PowerShell
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6055d9b694f72a6a874acdcb5135fde61bcf0d76
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442755"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets de autoridade de certificação de Backup e restauração do Windows PowerShell

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> **Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows  
> 
> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
## <a name="overview"></a>Visão geral  
O módulo do PowerShell do Windows ADCSAdministration foi introduzido no Windows Server 2012.  Dois novos cmdlets foram adicionados a esse módulo no Window Server 2012 R2 para dar suporte a Backup e restauração de uma autoridade de certificação.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Tabela tabela SEQ \\ \* árabe 17: Backup e restauração de Cmdlets do Windows PowerShell**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|Argumentos - **negrito** argumentos são necessários|Descrição|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadeia de caracteres - local para salvar o backup<br />-Este é o único parâmetro sem nome<br />-parâmetro posicional<br /><br />**Exemplo:**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-O certificado de autoridade de certificação sem o banco de dados de backup<br /><br />**Exemplo:**<br /><br />Backup-CARoleService c:\adcsbackup3 -KeyOnly|  
|-Password|-Especifica a senha para proteger os certificados de autoridade de certificação e chaves privadas<br />-Deve ser uma cadeia de caracteres segura<br />-Não é válido com o parâmetro – DatabaseOnly<br /><br />Exemplo:<br /><br />Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-senha (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)|  
|-DatabaseOnly|-Fazer backup de banco de dados sem o certificado de autoridade de certificação<br /><br />Backup-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|1.  Permite que você substituir o backup que preexists no local especificado no parâmetro - Path<br /><br />Backup-CARoleService c:\adcsbackup1 -Force|  
|-Incremental|-Executar um backup incremental<br /><br />Backup-CARoleService c:\adcsbackup7 -Incremental|  
|-KeepLog|1.  Instrui o comando para manter arquivos de log. Se a opção não for especificada, os arquivos de log são truncados por padrão, exceto no cenário Incremental<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Se a opção - senha parâmetro é usado, a senha fornecida deve ser uma cadeia de caracteres segura.  Use o **Read-Host** cmdlet para iniciar um prompt interativo para entrada de senha segura, ou usar o **ConvertTo-SecureString** cmdlet para especificar a senha na linha.  
  
Examine os exemplos a seguir  
  
**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|Argumentos - **negrito** argumentos são necessários|Descrição|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadeia de caracteres - local para restaurar o backup do<br />-Este é o único parâmetro sem nome<br />-parâmetro posicional<br /><br />**Exemplo:**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1 -Force<br /><br />Restore-CARoleService c:\adcsbackup2 -Force|  
|-KeyOnly|-O certificado de autoridade de certificação sem o banco de dados de restauração<br />-Deve ser especificado se o backup foi feito com a opção - KeyOnly<br /><br />**Exemplo:**<br /><br />Restore-CARoleService c:\adcsbackup3 - KeyOnly-Force|  
|-Password|-Especifica a senha dos certificados de autoridade de certificação e chaves privadas<br />-Deve ser uma cadeia de caracteres segura<br /><br />**Exemplo:**<br /><br />Restore-CARoleService c:\adcsbackup4 -Password (read-host -prompt "Password:" -AsSecureString) -Force<br /><br />Restore-CARoleService c:\adcsbackup5-senha (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force) -Force|  
|-DatabaseOnly|-Restaurar o banco de dados sem o certificado de autoridade de certificação<br /><br />Restore-CARoleService c:\adcsbackup6 – DatabaseOnly|  
|-Force|-Permite que você substitua as chaves preexistentes<br />-É um parâmetro opcional, mas ao restaurar no local, ele é provavelmente necessário<br /><br />Restore-CARoleService c:\adcsbackup1 -Force|  
  
### <a name="issues"></a>Problemas  
Um backup protegido de diferente de senha será executado se a função de ConvertTo-SecureString falhar enquanto estiver usando o Backup-CARoleService com o parâmetro-Password.  
  
![Autoridade de certificação backup e restauração](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabela tabela SEQ \\ \* 18 árabe: Erros comuns**  
  
|Ação|Erro|Comentário|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: O processo não pode acessar o arquivo porque ele está sendo usado por outro processo. (Exceção de HRESULT:<br /><br />0x80070020)|Parar o serviço de serviços de certificados do Active Directory antes de executar o cmdlet Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: O diretório não está vazio. (Exceção de HRESULT: 0x80070091)|Use o parâmetro - Force para substituir chaves preexistentes|  
|**Backup-CARoleService C:\ADCSBackup -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Backup-CARoleService: Conjunto de parâmetros não pode ser resolvido usando os parâmetros nomeados.|-Senha que o parâmetro é usado apenas para senha protegem chaves privadas e, portanto, é inválida quando não estiver fazendo-os para cima|  
|**Restore-CARoleService C:\ADCSBack15 -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Restore-CARoleService: Conjunto de parâmetros não pode ser resolvido usando os parâmetros nomeados.|-Senha que o parâmetro é usado apenas para senha protegem chaves privadas e, portanto, inválida quando eles não estão sendo restaurados|  
|**Restore-CARoleService C:\ADCSBack14 -Password (Read-Host -Prompt "Password:" -AsSecureString)**|Restore-CARoleService: O sistema não pode localizar o arquivo especificado. (Exceção de HRESULT: 0x80070002)|O caminho especificado não contém um backup de banco de dados válido.  Talvez o caminho é inválido ou o backup foi feito com a opção - KeysOnly?|  
  
## <a name="additional-resources"></a>Recursos adicionais  
[Guia de migração de serviços de certificados do Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Fazendo backup do banco de dados e da chave privada de uma AC](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restaurando o banco de dados e a configuração da autoridade de certificação no servidor de destino](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Tente o seguinte: Faça backup da AC em seu laboratório usando o Windows PowerShell  
  
1.  Use os comandos nesta lição para fazer backup de banco de dados de autoridade de certificação e chave privada protegida por uma senha.  
  
2.  Adiar a restauração da autoridade de certificação no momento.  
  


