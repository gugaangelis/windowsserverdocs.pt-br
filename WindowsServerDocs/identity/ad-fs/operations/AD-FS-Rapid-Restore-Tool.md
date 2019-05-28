---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Ferramenta de restauração rápida do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 04/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a831154a8b1e84f5ed879375980882e208c33d73
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190355"
---
# <a name="ad-fs-rapid-restore-tool"></a>Ferramenta de restauração rápida do AD FS

## <a name="overview"></a>Visão geral
Hoje em dia do AD FS é feito altamente disponível ao configurar um farm do AD FS. Algumas organizações gostam de uma maneira de ter um único servidor de implantação do AD FS, eliminando a necessidade de vários servidores do AD FS e infraestrutura, enquanto ainda tem alguns de balanceamento de carga garantia de que o serviço pode ser restaurada rapidamente se há um problema.
A nova ferramenta de restauração rápida do AD FS fornece uma maneira de restaurar os dados do AD FS sem a necessidade de um backup completo e restauração do sistema operacional ou do estado do sistema. Você pode usar a nova ferramenta para exportar a configuração do AD FS no Azure ou em um local no local.  Em seguida, você pode aplicar os dados exportados para uma nova instalação do AD FS, recriando ou duplicar o ambiente do AD FS. 

## <a name="scenarios"></a>Cenários
A ferramenta de restauração rápida do AD FS pode ser usada nos seguintes cenários:

1. Restaurar rapidamente a funcionalidade do AD FS após um problema
    - Use a ferramenta para criar uma instalação em espera fria do AD FS que podem ser implantado rapidamente no lugar do servidor do AD FS online
2. Implantar ambientes de teste e produção idênticos
    - Usar a ferramenta para criar rapidamente uma cópia precisa da produção do AD FS em um ambiente de teste ou para implantar rapidamente uma configuração de teste validado para a produção

## <a name="what-is-backed-up"></a>O que é feito backup
A ferramenta faz o backup a seguinte configuração do AD FS
    
- AD FS configuration database (SQL ou um WID)
- Arquivo de configuração (localizado na pasta do AD FS)
- Gerado automaticamente o token de autenticação e descriptografia de certificados e chaves privadas (do contêiner DKM do Active Directory)
- Certificado SSL e qualquer externamente inscritos (assinatura de token, descriptografia de token e comunicação de serviço) de certificados e chaves privadas correspondentes (Observação: as chaves privadas devem ser exportáveis e o usuário que executa o script deve ter permissões para acessá-los)
- Uma lista dos provedores de autenticação personalizada, repositórios de atributos e provedor de declarações local confia que estão instaladas.

## <a name="how-to-use-the-tool"></a>Como usar a ferramenta
Primeiro, [baixar](https://go.microsoft.com/fwlink/?LinkId=825646) e instale o MSI para o servidor do AD FS.  

Execute o seguinte comando em um prompt do PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Se você estiver usando o banco de dados integrado de Windows (WID), esta ferramenta precisa ser executado no servidor do AD FS primário.  Você pode usar o `Get-AdfsSyncProperties` cmdlet do PowerShell para determinar se o servidor que você estiver usando é o servidor primário.

### <a name="system-requirements"></a>Requisitos de sistema

- Essa ferramenta funciona para o AD FS no Windows Server 2012 R2 e versões posteriores. 
- O .NET framework necessário é pelo menos 4.0. 
- A restauração deve ser feita em um servidor do AD FS da mesma versão do backup e que usa a mesma conta do Active Directory como a conta de serviço do AD FS.

## <a name="create-a-backup"></a>Criar um backup
Para criar um backup, use o cmdlet Backup-ADFS. Esse cmdlet faz o backup de configuração do AD FS, banco de dados, certificados SSL etc. 

O usuário deve ser pelo menos um administrador local para executar este cmdlet. Para fazer o backup do contêiner DKM do Active Directory (exigido na configuração padrão do AD FS), o usuário deve ser administrador de domínio, precisa passar as credenciais de conta de serviço do AD FS ou tem acesso ao contêiner DKM.  Se você estiver usando uma conta gMSA, o usuário deve ser administrador de domínio ou ter permissões para o contêiner; Você não pode fornecer as credenciais da gMSA. 

O backup será nomeado de acordo com o padrão "adfsBackup_ID_Date-Time". Ele conterá o número de versão, a data e hora em que o backup foi feito.
O cmdlet usa os seguintes parâmetros:
    
Conjuntos de parâmetros

![Ferramenta de restauração rápida do AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descrição detalhada

- **BackupDKM** -faz backup de contêiner DKM do Active Directory que contém as chaves do AD FS na configuração padrão (gerado automaticamente pelo token de autenticação e certificados de descriptografia). Isso usa uma ferramenta AD 'ldifde' para exportar o contêiner AD e todas as suas subárvores.

- -**StorageType &lt;cadeia de caracteres&gt;**  -o tipo de armazenamento que o usuário deseja usar. "FileSystem" indica que o usuário deseja armazená-lo em uma pasta, localmente ou na rede "Azure" indica o usuário deseja armazená-los no contêiner de armazenamento do Azure, quando o usuário executa o backup, eles selecionam o local de backup, o sistema de arquivos ou na nuvem. Para o Azure a ser usada, as credenciais do armazenamento do Azure devem ser passadas para o cmdlet. As credenciais de armazenamento contém o nome da conta e a chave. Além disso, um nome de contêiner deve ser passado no. Se o contêiner não existir, ele será criado durante o backup. O sistema de arquivos a ser usado, um caminho de armazenamento deve ser fornecido. Nesse diretório, um novo diretório será criado para cada backup. Cada diretório criado conterá os arquivos de backups. 

- **SenhaDeCriptografia &lt;cadeia de caracteres&gt;**  -a senha que será usada para criptografar todos os arquivos de backup antes de armazená-lo

- **AzureConnectionCredentials &lt;pscredential&gt;**  -o nome da conta e a chave da conta de armazenamento do Azure

- **AzureStorageContainer &lt;cadeia de caracteres&gt;**  -o contêiner de armazenamento onde o backup será armazenado no Azure

- **StoragePath &lt;cadeia de caracteres&gt;**  -o local em que os backups serão armazenados

- **ServiceAccountCredential &lt;pscredential&gt;**  -Especifica a conta de serviço que está sendo usada para o serviço do AD FS em execução no momento. Esse parâmetro só é necessário se o usuário gostaria de fazer backup de DKM e não é administrador de domínio ou não tem acesso ao conteúdo do contêiner. 

- **BackupComment &lt;string []&gt;**  -uma cadeia de caracteres informativa sobre o backup que será exibido durante a restauração, semelhante ao conceito de nomenclatura de ponto de verificação do Hyper-V. O padrão é uma cadeia de caracteres vazia

 
## <a name="backup-examples"></a>Exemplos de backup
A seguir estão exemplos de backup para usar a ferramenta de restauração do AD FS rápida.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>Fazer backup da configuração do AD FS, com o DKM, o sistema de arquivos, e tem acesso para o conteúdo do contêiner DKM (seja administrador de domínio ou delegado)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuração de FS de backup do AD, com o DKM, o sistema de arquivos com a credencial de conta de serviço, executando como administrador local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Fazer backup de configuração do AD FS sem o DKM para o contêiner de armazenamento do Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Configuração do AD FS sem o DKM ao sistema de arquivos de backup

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar do backup
Para aplicar uma configuração criada usando o ADFS de Backup para uma nova instalação do AD FS, use o cmdlet Restore-ADFS.

Esse cmdlet cria um novo farm de AD FS usando o cmdlet `Install-AdfsFarm` e restaura a configuração do AD FS, banco de dados, certificados etc.  Se a função AD FS não foi instalada no servidor, o cmdlet irá instalá-lo.  O cmdlet verifica o local de restauração de backups existentes e solicita que o usuário escolher um backup apropriado com base em como a data/hora que foi obtido e qualquer comentário de backup que o usuário pode ter anexado para o backup. Se houver várias configurações do AD FS com nomes de serviço de Federação diferente, o usuário é solicitado pela primeira vez, escolha a configuração apropriada do AD FS.
O usuário deve ser um administrador local e de domínio para executar este cmdlet.


>[!NOTE] 
>Antes de usar a ferramenta de recuperação rápida do AD FS, certifique-se de que o servidor ingressou no domínio antes da restauração do backup. 

O cmdlet usa os seguintes parâmetros: 

![Ferramenta de restauração rápida do AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descrição detalhada

- **StorageType &lt;cadeia de caracteres&gt;**  -o tipo de armazenamento que o usuário deseja usar.
 "FileSystem" indica que o usuário deseja armazená-lo em uma pasta localmente ou na rede "Azure" indica que o usuário deseja armazená-los no contêiner de armazenamento do Azure

- **DecryptionPassword &lt;cadeia de caracteres&gt;**  -a senha que foi usada para criptografar todos os arquivos de backup 

- **AzureConnectionCredentials &lt;pscredential&gt;**  -o nome da conta e a chave da conta de armazenamento do Azure

- **AzureStorageContainer &lt;cadeia de caracteres&gt;**  -o contêiner de armazenamento onde o backup será armazenado no Azure

- **StoragePath &lt;cadeia de caracteres&gt;**  -o local em que os backups serão armazenados

- **ADFSName &lt; cadeia de caracteres &gt;**  -o nome da federação que foi feito o backup e vai ser restaurado. Se isso não for fornecido e não há nome de serviço de Federação apenas um, em seguida, que será usado. Se não houver mais de um serviço de Federação é feito backup no local e, em seguida, o usuário é solicitado a escolher uma opção de backup dos serviços de Federação.

- **ServiceAccountCredential &lt; pscredential &gt;**  -Especifica a conta de serviço que será usada para o novo serviço do AD FS que está sendo restaurado 

- **GroupServiceAccountIdentifier &lt;cadeia de caracteres&gt;**  -GMSA o que o usuário deseja usar para o novo serviço do AD FS que está sendo restaurado. Por padrão, se nenhum for fornecido, em seguida, o backup dos backup nome da conta é usado se ele tiver sido GMSA, mais o usuário é solicitado a colocar em uma conta de serviço

- **DBConnectionString &lt;cadeia de caracteres&gt;**  - se o usuário gostaria de usar um banco de dados diferente para a restauração, em seguida, eles devem passar a cadeia de caracteres de Conexão SQL ou o tipo no WID para WID.

- **Força &lt;bool&gt;**  -ignorar os avisos que a ferramenta pode ter depois que o backup for escolhido

- **RestoreDKM &lt;bool&gt;**  - restaurar o contêiner DKM AD, deve ser definido se um novo anúncio e o DKM foi feito backup inicialmente.

## <a name="restore-examples"></a>Exemplos Restore

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

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar a configuração do AD FS WID

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
Todos os dados de backup são criptografados antes de enviá-la para a nuvem ou armazená-los no sistema de arquivos.  

Cada documento que é criado como parte do backup é criptografado usando AES-256. A senha passada para a ferramenta é usada como uma frase secreta para gerar uma nova senha usando a classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider é usado para gerar o SAL usado por AES e a classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Arquivos de log
Sempre que um backup ou restauração é executada, um arquivo de log é criado. Eles podem ser encontrados no seguinte local:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Quando executar uma restauração de que um arquivo de PostRestore_Instructions pode ser criado que contém uma visão geral dos provedores de autenticação adicional, repositórios de atributos e relações de confiança de provedor de declarações local para ser instalado manualmente antes de iniciar o serviço AD FS.

## <a name="version-release-history"></a>Histórico de versão

### <a name="version-10810"></a>Versão: 1.0.81.0
Versão: Abril de 2019

**Problemas corrigidos:**


- Correções de bug para o certificado backup e restauração
- Informações de rastreamento adicionais para o arquivo de log


### <a name="version-10750"></a>Versão: 1.0.75.0
Versão: Agosto de 2018

**Problemas corrigidos:**
* Ao usar a opção - BackupDKM, atualize ADFS do Backup.  A ferramenta determinará se o contexto atual tem acesso ao contêiner DKM.  Nesse caso, ele não exigirá privilégios de administrador de domínio ou credenciais de conta de serviço.  Isso permite que os backups automatizados para acontecer sem fornecer credenciais explicitamente ou em execução como uma conta de administrador de domínio.

### <a name="version-10730"></a>Versão: 1.0.73.0
Versão: Agosto de 2018

**Problemas corrigidos:**
* Atualizar os algoritmos de criptografia para que o aplicativo está em conformidade com FIPS
    
    >[!NOTE]
    > Backups antigos não funcionarão com a nova versão devido a alterações nos algoritmos de criptografia de acordo com a conformidade com FIPS
    
* Adicionar suporte para clusters SQL que usam a replicação de mesclagem

### <a name="version-10720"></a>Versão: 1.0.72.0
Versão: Julho de 2018

**Problemas corrigidos:**

   - Correção de bug: Corrigido o. Instalador MSI para dar suporte a atualizações in-loco 

### <a name="10180"></a>1.0.18.0
Versão: Julho de 2018

**Problemas corrigidos:**

   - Correção de bug: lidar com senhas de contas de serviço que têm caracteres especiais (isto é, '&')
   - Correção de bug: restauração falhará porque Microsoft.IdentityServer.Servicehost.exe.config está sendo usado por outro processo


### <a name="1000"></a>1.0.0.0
Lançamento: Outubro de 2016

Versão inicial do AD FS rápida ferramenta de restauração
