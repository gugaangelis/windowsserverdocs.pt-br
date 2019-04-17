---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: "Ferramenta de restauração do AD FS rápida"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd1cc8dab07288ba73c507bc551f089bb79502bc
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2018
---
# <a name="ad-fs-rapid-restore-tool"></a>Ferramenta de restauração do AD FS rápida

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

## <a name="overview"></a>Visão geral
Hoje AD FS é disponibilizado altamente Configurando um farm AD FS. Algumas organizações gostaria de uma forma de ter um único servidor de implantação do AD FS, eliminando a necessidade de vários servidores do AD FS e infraestrutura de balanceamento de rede e ainda ter algumas garantem que o serviço possa ser restaurado rapidamente se houver um problema.
A nova ferramenta de restauração rápida do AD FS fornece uma maneira de restaurar os dados do AD FS sem a necessidade de um backup completo e a restauração do sistema operacional ou estado do sistema. Você pode usar a nova ferramenta para exportar a configuração do AD FS ao Azure ou em um local no local.  Em seguida, você pode aplicar os dados exportados para uma nova instalação do AD FS, recriar ou duplicar o ambiente AD FS. 

## <a name="scenarios"></a>Cenários
A ferramenta de restauração rápida do AD FS pode ser usada nos seguintes cenários:

1. Restaurar rapidamente a funcionalidade do AD FS após um problema
    - Use a ferramenta para criar uma instalação do modo de espera fria do AD FS que pode ser implantado rapidamente no lugar do servidor do AD FS online
2. Implantar idênticos ambientes de teste e produção
    - Use a ferramenta para criar rapidamente uma cópia da produção AD FS precisa em um ambiente de teste ou para implantar rapidamente uma configuração de teste validadas na produção

## <a name="what-is-backed-up"></a>O que será feito backup
A ferramenta de backup a seguinte configuração do AD FS
    
- AD FS configuração banco de dados (SQL ou trabalho)
- Arquivo de configuração (localizado na pasta do AD FS)
- Gerado automaticamente o token de assinatura e descriptografar certificados e chaves privadas (do contêiner DKM do Active Directory)
- Certificado SSL e qualquer externamente inscrito certificados (token de assinatura, token descriptografia e comunicação de serviço) e chaves privadas correspondentes (Observação: as chaves privadas devem ser exportable e o usuário que está executando o script deve ter permissões para acessá-los)
- Uma lista dos provedores de autenticação personalizados, repositórios de atributo e provedor local requerimentos judiciais ou Extrajudiciais confia em que estão instalados.

## <a name="how-to-use-the-tool"></a>Como usar a ferramenta
Primeiro, [baixar](https://go.microsoft.com/fwlink/?LinkId=825646) e instalar o MSI para o servidor do AD FS.  

>[!NOTE]
>A ferramenta Restauração do AD FS rápido não é compatível com FIPS.

Execute o seguinte comando em um prompt do PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Se você estiver usando Windows integrado banco de dados (trabalho), essa ferramenta precisa ser executado no servidor do AD FS principal.  Você pode usar o `Get-SyncProperties` cmdlet do PowerShell para determinar se o servidor que você estiver em é o servidor primário.

### <a name="system-requirements"></a>Requisitos do sistema

- Essa ferramenta funciona do AD FS no Windows Server 2012 R2 e versões posteriores. 
- O .NET framework necessário é pelo menos 4.0. 
- A restauração deve ser feita em um servidor do AD FS da mesma versão do backup e que usa a mesma conta do Active Directory como a conta de serviço do AD FS.

## <a name="create-a-backup"></a>Criar um backup
Para criar um backup, use o cmdlet ADFS de Backup. Este cmdlet faz backup a configuração do AD FS banco de dados, certificados SSL, etc. 

O usuário deve ser pelo menos um administrador local para executar este cmdlet. Para fazer backup do contêiner do Active Directory DKM (necessário no passo de configuração padrão AD FS), o usuário tem que ser bem admin do domínio, ou precisa passar as credenciais de conta de serviço do AD FS.

O backup será chamado de acordo com o padrão "adfsBackup_ID_Date tempo". Ele conterá o número de versão, data e hora em que o backup foi feito.
O cmdlet aceita os seguintes parâmetros:
    
Conjuntos de parâmetros

![Ferramenta de restauração do AD FS rápida](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descrição detalhada

- **BackupDKM** -faz backup o contêiner do Active Directory DKM que contém as chaves do AD FS no passo de configuração padrão (gerado automaticamente pelo token de assinatura e certificados de descriptografia). Isso usa uma ferramenta do AD 'ldifde' para exportar o contêiner de anúncios e todas as suas subárvores.

- -**StorageType &lt;cadeia de caracteres&gt; ** -o tipo de armazenamento que o usuário deseja usar. "FileSystem" indica que o usuário deseja armazená-lo em uma pasta localmente ou na rede "Azure" indica o usuário deseja armazená-lo no contêiner de armazenamento do Azure, quando o usuário executa o backup, ele deve selecionar o local de backup, o sistema de arquivos ou na nuvem. Do Azure ser usado, as credenciais de armazenamento do Azure deve ser passadas para o cmdlet. As credenciais de armazenamento contém o nome da conta e a chave. Além disso, um nome de contêiner deve ser passado em. Se o contêiner não existir, ele é criado durante o backup. Para o sistema de arquivo a ser usado, você deve ter um caminho de armazenamento. Nesse diretório, um novo diretório será criado para cada backup. Cada diretório criado conterá os arquivos de backups. 

- **SenhaDeCriptografia &lt;cadeia de caracteres&gt; ** -a senha que vai ser usada para criptografar todos os arquivos de backups antes de armazená-lo

- **AzureConnectionCredentials &lt;pscredential&gt; ** -o nome da conta e a chave para a conta de armazenamento do Azure

- **AzureStorageContainer &lt;cadeia de caracteres&gt; ** -o contêiner de armazenamento onde o backup será armazenado no Azure

- **StoragePath &lt;cadeia de caracteres&gt; ** -o local em que os backups serão armazenados

- **ServiceAccountCredential &lt;pscredential&gt; ** -Especifica a conta de serviço que está sendo usada para o serviço do AD FS em execução no momento. Esse parâmetro é necessário somente se o usuário gostaria de fazer o backup do DKM e não é um administrador do domínio.

- **BackupComment &lt;cadeia de caracteres []&gt; ** -uma cadeia de caracteres informativa sobre o backup que será exibido durante a restauração, semelhante ao conceito de nomenclatura do Hyper-V checkpoint. O padrão é uma cadeia de caracteres vazia

 
## <a name="backup-examples"></a>Exemplos de backup
A seguir estão exemplos de backup para usar a ferramenta Restauração do AD FS rápida.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-while-running-as-the-domain-admin"></a>Fazer backup da configuração do AD FS, com DKM, ao sistema de arquivos, durante a execução como o administrador de domínio

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuração FS o anúncio de backup, com o DKM, ao sistema de arquivos com as credenciais da conta de serviço, executar como administrador local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Fazer backup da configuração do AD FS sem DKM ao contêiner de armazenamento do Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Fazer backup da configuração do AD FS sem DKM ao sistema de arquivos

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar a partir do backup
Para aplicar uma configuração criada usando o Backup ADFS para uma nova instalação do AD FS, use o cmdlet ADFS de restauração.

Este cmdlet cria um novo farm AD FS usando o cmdlet `Install-AdfsFarm` e restaura a configuração do AD FS banco de dados, certificados, etc. Se a função AD FS não tiver sido instalada no servidor, ele será instalado o cmdlet.  O cmdlet verifica se o local de restauração de backups existentes e solicita que o usuário escolha um backup apropriado com base na data/hora que foi tirado e os comentários de backup que o usuário pode ter anexado ao backup. Se houver várias configurações do AD FS com nomes de serviço de Federação diferente, o usuário é solicitado primeiro escolher a configuração do AD FS apropriada.
O usuário tem de ser um administrador local e de domínio para executar este cmdlet.


>[!NOTE] 
>Antes de usar a ferramenta de recuperação rápida do AD FS, certifique-se de que o servidor ingressou no domínio antes de restaurar o backup. 

O cmdlet aceita os seguintes parâmetros: 

![Ferramenta de restauração do AD FS rápida](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descrição detalhada

- **StorageType &lt;cadeia de caracteres&gt; ** -o tipo de armazenamento que o usuário deseja usar.
 "FileSystem" indica que o usuário deseja armazená-la em uma pasta localmente ou na rede "Azure" indica que o usuário deseja armazená-lo no contêiner de armazenamento do Azure

- **DecryptionPassword &lt;cadeia de caracteres&gt; ** -a senha que foi usada para criptografar todos os arquivos de backups 

- **AzureConnectionCredentials &lt;pscredential&gt; ** -o nome da conta e a chave para a conta de armazenamento do Azure

- **AzureStorageContainer &lt;cadeia de caracteres&gt; ** -o contêiner de armazenamento onde o backup será armazenado no Azure

- **StoragePath &lt;cadeia de caracteres&gt; ** -o local em que os backups serão armazenados

- **ADFSName &lt; cadeia de caracteres &gt; ** -o nome da federação que foi feito o backup e vai ser restaurado. Se isso não é fornecido e houver nome do serviço de Federação apenas uma e depois que será usado. Se houver mais de um serviço de Federação copiados para o local, em seguida, o usuário é solicitado a escolher entre os serviços de federação de backup.

- **ServiceAccountCredential &lt; pscredential &gt; ** -Especifica a conta de serviço que será usada para o novo serviço AD FS sendo restaurado 

- **GroupServiceAccountIdentifier &lt;cadeia de caracteres&gt; ** -GMSA o que o usuário deseja usar para o novo serviço AD FS sendo restaurado. Por padrão, se nenhum for fornecido, em seguida, foi feito o nome da conta é usado se fosse GMSA, mais o usuário é solicitado a colocar em uma conta de serviço

- **DBConnectionString &lt;cadeia de caracteres&gt; ** - se o usuário gostaria de usar um banco de dados diferente para a restauração, em seguida, eles devem passar a cadeia de caracteres de Conexão SQL ou tipo de trabalho para WID.

- **Força &lt;bool&gt; ** -ignorar os avisos que a ferramenta pode ter depois que o backup é escolhido

- **RestoreDKM &lt;bool&gt; ** - restaurar o contêiner DKM ao anúncio, deve ser definido caso queira um novo anúncio e o DKM foi feito o backup inicialmente.

## <a name="restore-examples"></a>Restaurar exemplos

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurar a configuração do AD FS sem o DKM do contêiner de armazenamento do Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurar a configuração do AD FS sem o DKM do sistema de arquivos
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurar a configuração do AD FS com o DKM ao sistema de arquivos 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar a configuração do AD FS para trabalho

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurar a configuração do AD FS para SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaura a configuração do AD FS com o GMSA especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurar a configuração do AD FS com as credenciais de conta de serviço especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informações de criptografia
Todos os dados de backup são criptografados antes empurrando-lo para a nuvem ou armazená-lo no sistema de arquivos.  

Cada documento que é criado como parte do backup é criptografado usando AES-256. A senha passada para a ferramenta é usada como uma frase secreta para gerar uma nova senha usando a classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider é usado para gerar o salt usado pelo AES e a classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Arquivos de log
Sempre que um backup ou restauração é realizada é criado um arquivo de log. Eles podem ser encontrados no seguinte local:

- **%LocalAppData%\ADFSRapidRecreationTool**

>[!NOTE]
> Quando executar uma restauração de que um arquivo PostRestore_Instructions pode ser criado que contém uma visão geral dos provedores de autenticação adicional, o atributo armazena e relações de confiança de provedor de declarações local a ser instalado manualmente antes de iniciar o serviço do AD FS.
