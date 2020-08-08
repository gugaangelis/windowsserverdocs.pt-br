---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlets do Windows PowerShell de backup e restauração de AC
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 817a4c117bfd39799a5147d657262eb208c9a79b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943441"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets do Windows PowerShell de backup e restauração de AC

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
> **Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows
>
> [!NOTE]
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.

## <a name="overview"></a>Visão geral
O módulo ADCSAdministration do Windows PowerShell foi introduzido no servidor de janelas 2012.  Dois novos cmdlets foram adicionados a esse módulo no Windows Server 2012 R2 para dar suporte ao backup e à restauração de uma CA.

-   Backup-CARoleService

-   Restore-CARoleService

## <a name="backup-caroleservice"></a>Backup-CARoleService
**Tabela SEQ tabela \\ \* árabe 17: backup e restauração de cmdlets do Windows PowerShell**

**Cmdlet ADCSAdministration: backup-CARoleService**

|Argumentos-argumentos em **negrito** são necessários|Descrição|
|------------------------------------------------|---------------|
|**-Caminho**|-Cadeia de caracteres-local para salvar o backup<br />-Este é o único parâmetro sem nome<br />-parâmetro posicional<p>**Exemplo:**<p>Backup-CARoleService.-path c:\adcsbackup1<p>Backup-CARoleService c:\adcsbackup2|
|-Somente|-Fazer backup do certificado de autoridade de certificação sem o banco de dados<p>**Exemplo:**<p>Backup-CARoleService c:\adcsbackup3-keyonly|
|-Password|-Especifica a senha para proteger os certificados de autoridade de certificação e as chaves privadas<br />-Deve ser uma cadeia de caracteres segura<br />-Não é válido com o parâmetro-DatabaseOnly<p>Exemplo:<p>Backup-CARoleService c:\adcsbackup4-senha (leitura-host-prompt "senha:"-assegurastring)<p>Backup-CARoleService c:\adcsbackup5-senha (ConvertTo-SecureString "Pa55w0rd!" -Astexto não criptografado-forçar)|
|-DatabaseOnly|-Fazer backup do banco de dados sem o certificado de autoridade de certificação<p>Backup-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|1. permite que você substitua o backup que existe no local especificado no parâmetro-Path<p>Backup-CARoleService c:\adcsbackup1-Force|
|-Incremental|-Executar um backup incremental<p>Backup-CARoleService c:\adcsbackup7-incremental|
|-KeepLog|1. instrui o comando para manter os arquivos de log. Se a opção não for especificada, os arquivos de log serão truncados por padrão, exceto no cenário incremental<p>Backup-CARoleService c:\adcsbackup7-KeepLog|

### <a name="-password-secure-string"></a>-Password <Secure String>
Se o parâmetro-password for usado, a senha fornecida deverá ser uma cadeia de caracteres segura.  Use o cmdlet **Read-Host** para iniciar um prompt interativo para a entrada de senha segura ou use o cmdlet **ConvertTo-SecureString** para especificar a senha em linha.

Examine os exemplos a seguir

**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando o Read-Host**

```powershell
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)
```

**Especificando uma cadeia de caracteres segura para o parâmetro de senha usando ConvertTo-SecureString**

```powershell
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)
```

## <a name="restore-caroleservice"></a>Restore-CARoleService
**Cmdlet ADCSAdministration: Restore-CARoleService**

|Argumentos-argumentos em **negrito** são necessários|Descrição|
|------------------------------------------------|---------------|
|**-Caminho**|-Cadeia de caracteres-local para o qual restaurar o backup<br />-Este é o único parâmetro sem nome<br />-parâmetro posicional<p>**Exemplo:**<p>Restore-CARoleService.-path c:\adcsbackup1-Force<p>Restore-CARoleService c:\adcsbackup2-Force|
|-Somente|-Restaurar o certificado de autoridade de certificação sem o banco de dados<br />-Deve ser especificado se o backup foi feito com a opção-keyonly<p>**Exemplo:**<p>Restore-CARoleService c:\adcsbackup3-addonly-Force|
|-Password|-Especifica a senha dos certificados de autoridade de certificação e chaves privadas<br />-Deve ser uma cadeia de caracteres segura<p>**Exemplo:**<p>Restore-CARoleService c:\adcsbackup4-Password (leitura-host-prompt "senha:"-assegurastring)-Force<p>Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AutoTexto-Force)-Force|
|-DatabaseOnly|-Restaurar o banco de dados sem o certificado de autoridade de certificação<p>Restore-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|-Permite que você substitua as chaves preexistentes<br />-É um parâmetro opcional, mas ao restaurar in-loco, é provável que seja necessário<p>Restore-CARoleService c:\adcsbackup1-Force|

### <a name="issues"></a>Problemas
Um backup não protegido por senha será obtido se a função ConvertTo-SecureString falhar ao usar o backup-CARoleService com o parâmetro-password.

![Backup e restauração de CA](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)

**Tabela SEQ tabela \\ \* árabe 18: erros comuns**

|Ação|Erro|Comentário|
|----------|---------|-----------|
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: o processo não pode acessar o arquivo porque ele está sendo usado por outro processo. (Exceção de HRESULT:<p>0x80070020|Pare o serviço de serviços de certificados Active Directory antes de executar o cmdlet Restore-CARoleService|
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: o diretório não está vazio. (Exceção de HRESULT: 0x80070091)|Usar o parâmetro-Force para substituir chaves preexistentes|
|**Backup-CARoleService C:\ADCSBackup-senha (leitura-host-prompt "senha:"-assegurastring)-DatabaseOnly**|Backup-CARoleService: o conjunto de parâmetros não pode ser resolvido usando os parâmetros nomeados especificados.|O parâmetro-password é usado somente para proteger chaves privadas com senha e, portanto, é inválido quando você não está fazendo backup delas|
|**Restore-CARoleService C:\ADCSBack15-Password (Read-Host-prompt "senha:"-assegurastring)-DatabaseOnly**|Restore-CARoleService: não é possível resolver o conjunto de parâmetros usando os parâmetros nomeados especificados.|O parâmetro-password é usado somente para proteger chaves privadas com senha e, portanto, é inválido quando você não as restaura|
|**Restore-CARoleService C:\ADCSBack14-Password (leitura-host-prompt "senha:"-assegurastring)**|Restore-CARoleService: o sistema não pode localizar o arquivo especificado. (Exceção de HRESULT: 0x80070002)|O caminho especificado não contém um backup de banco de dados válido.  Talvez o caminho seja inválido ou o backup tenha sido feito com a opção-KeysOnly?|

## <a name="additional-resources"></a>Recursos adicionais
[Guia de Migração de Serviços de Certificados do Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))

[Fazendo backup do banco de dados e da chave privada de uma AC](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_BackUpDB)

[Restaurando o banco de dados e a configuração da autoridade de certificação no servidor de destino](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_RestoreCA)

## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Experimente: faça backup da autoridade de certificação em seu laboratório usando o Windows PowerShell

1.  Use os comandos nesta lição para fazer backup do banco de dados de CA e da chave privada protegidas por uma senha.

2.  Aguarde a restauração da autoridade de certificação no momento.

