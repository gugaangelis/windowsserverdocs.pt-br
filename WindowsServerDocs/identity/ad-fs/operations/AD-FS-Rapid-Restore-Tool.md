---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Ferramenta de restauração rápida do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc924f5e5bdd7dabecac4fdd6805ad261a0fc634
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866170"
---
# <a name="ad-fs-rapid-restore-tool"></a>Ferramenta de restauração rápida do AD FS

## <a name="overview"></a>Visão geral
Hoje AD FS torna-se altamente disponível Configurando um farm de AD FS. Algumas organizações gostariam de uma maneira de ter um único servidor AD FS implantação, eliminando a necessidade de vários servidores AD FS e da infraestrutura de balanceamento de carga de rede, enquanto ainda tenho alguma garantia de que o serviço pode ser restaurado rapidamente se houver um problema.
A nova AD FS ferramenta de restauração rápida fornece uma maneira de restaurar AD FS dados sem a necessidade de um backup completo e a restauração do sistema operacional ou do estado do sistema. Você pode usar a nova ferramenta para exportar AD FS configuração para o Azure ou para um local.  Em seguida, você pode aplicar os dados exportados a uma nova instalação do AD FS, recriando ou duplicando o ambiente de AD FS. 

## <a name="scenarios"></a>Cenários
A ferramenta de restauração rápida AD FS pode ser usada nos seguintes cenários:

1. Restaure rapidamente AD FS funcionalidade após um problema
    - Use a ferramenta para criar uma instalação em espera passiva de AD FS que possa ser implantada rapidamente no lugar do servidor de AD FS online
2. Implantar ambientes de teste e produção idênticos
    - Use a ferramenta para criar rapidamente uma cópia precisa do AD FS de produção em um ambiente de teste ou para implantar rapidamente uma configuração de teste validada para a produção

>[!NOTE] 
>Se você estiver usando a replicação de mesclagem do SQL ou grupos de disponibilidade AlwaysOn, a ferramenta de restauração rápida não terá suporte. É recomendável usar backups baseados em SQL e um backup do certificado SSL como uma alternativa.

## <a name="what-is-backed-up"></a>O que é feito backup
A ferramenta faz backup da seguinte configuração de AD FS
    
- Banco de dados de configuração AD FS (SQL ou WID)
- Arquivo de configuração (localizado na pasta AD FS)
- Assinatura de token gerada automaticamente e descriptografia de certificados e chaves privadas (do contêiner Active Directory DKM)
- Certificado SSL e quaisquer certificados registrados externamente (assinatura de token, descriptografia de token e comunicação de serviço) e chaves privadas correspondentes (Observação: as chaves privadas devem ser exportáveis e o usuário que está executando o script deve ter permissões para acessá-las)
- Uma lista de provedores de autenticação personalizados, repositórios de atributos e relações de confiança do provedor de declarações locais que estão instalados.

## <a name="how-to-use-the-tool"></a>Como usar a ferramenta
Primeiro, [Baixe](https://go.microsoft.com/fwlink/?LinkId=825646) e instale o MSI em seu servidor de AD FS.  

Execute o seguinte comando em um prompt do PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Se você estiver usando o banco de dados integrado do Windows (WID), essa ferramenta precisará ser executada no servidor de AD FS primário.  Você pode usar o `Get-AdfsSyncProperties` cmdlet do PowerShell para determinar se o servidor no qual você está ou não é o servidor primário.

### <a name="system-requirements"></a>Requisitos de sistema

- Essa ferramenta funciona para AD FS no Windows Server 2012 R2 e posterior. 
- O .NET Framework necessário é pelo menos 4,0. 
- A restauração deve ser feita em um servidor de AD FS da mesma versão que o backup e que usa a mesma conta de Active Directory que a conta de serviço do AD FS.

## <a name="create-a-backup"></a>Criar um backup
Para criar um backup, use o cmdlet backup-ADFS. Esse cmdlet faz backup da configuração do AD FS, do banco de dados, dos certificados SSL, etc. 

O usuário deve ser pelo menos um administrador local para executar este cmdlet. Para fazer backup do contêiner Active Directory DKM (necessário na configuração de AD FS padrão), o usuário deve ser administrador de domínio, precisa passar as credenciais da conta de serviço do AD FS ou ter acesso ao contêiner DKM.  Se você estiver usando uma conta do gMSA, o usuário deverá ser administrador de domínio ou ter permissões para o contêiner; Você não pode fornecer as credenciais do gMSA. 

O backup será nomeado de acordo com o padrão "adfsBackup_ID_Date-time". Ele conterá o número de versão, a data e a hora em que o backup foi feito.
O cmdlet usa os seguintes parâmetros:
    
Conjuntos de parâmetros

![Ferramenta de restauração rápida do AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descrição detalhada

- **BackupDKM** -faz backup do contêiner Active Directory DKM que contém as chaves de AD FS na configuração padrão (certificados de assinatura e descriptografia de token gerados automaticamente). Isso usa uma ferramenta de anúncio ' LDIFDE ' para exportar o contêiner do AD e todas as suas subárvores.

- -**Cadeia decaracteres&gt; storagetype-o tipo de armazenamento que o usuário deseja usar. &lt;** "FileSystem" indica que o usuário deseja armazená-lo em uma pasta localmente ou na rede "Azure" indica que o usuário deseja armazená-lo no contêiner de armazenamento do Azure quando o usuário executa o backup, seleciona o local de backup, o sistema de arquivos ou o nuvem. Para que o Azure seja usado, as credenciais de armazenamento do Azure devem ser passadas para o cmdlet. As credenciais de armazenamento contêm o nome da conta e a chave. Além disso, um nome de contêiner também deve ser passado. Se o contêiner não existir, ele será criado durante o backup. Para o sistema de arquivos a ser usado, um caminho de armazenamento deve ser fornecido. Nesse diretório, um novo diretório será criado para cada backup. Cada diretório criado conterá os arquivos de backup. 

- **Cadeia &lt;decaracteres&gt; EncryptionPassword** -a senha que será usada para criptografar todos os arquivos de backup antes de armazená-los

- **AzureConnectionCredentials&lt;PSCredential&gt;** -o nome da conta e a chave da conta de armazenamento do Azure

- **Cadeia &lt;decaracteres&gt; AzureStorageContainer** -o contêiner de armazenamento em que o backup será armazenado no Azure

- **Cadeia &lt;decaracteres&gt; StoragePath** -o local em que os backups serão armazenados

- **ServiceAccountCredential&lt;PSCredential&gt;** -especifica a conta de serviço que está sendo usada para o serviço AD FS em execução no momento. Esse parâmetro só será necessário se o usuário quiser fazer backup do DKM e não for um administrador de domínio ou não tiver acesso ao conteúdo do contêiner. 

- **BackupComment&lt;String []&gt;** – uma cadeia de caracteres informativa sobre o backup que será exibida durante a restauração, semelhante ao conceito de nomenclatura do ponto de verificação do Hyper-V. O padrão é uma cadeia de caracteres vazia

 
## <a name="backup-examples"></a>Exemplos de backup
Veja a seguir exemplos de backup para usar a ferramenta de restauração rápida AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>Fazer backup da configuração de AD FS, com DKM, para o sistema de arquivos e ter acesso ao conteúdo do contêiner DKM (administrador de domínio ou delegado)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Faça backup da configuração de AD FS, com o DKM, para o sistema de arquivos com a credencial de conta de serviço, executando como administrador local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Faça backup da configuração de AD FS sem o DKM para o contêiner de armazenamento do Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Fazer backup da configuração de AD FS sem o DKM para o sistema de arquivos

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar do backup
Para aplicar uma configuração criada usando backup-ADFS em uma nova instalação do AD FS, use o cmdlet Restore-ADFS.

Esse cmdlet cria um novo farm de AD FS usando o `Install-AdfsFarm` cmdlet e restaura a configuração de AD FS, o banco de dados, os certificados, etc.  Se a função de AD FS não tiver sido instalada no servidor, o cmdlet irá instalá-lo.  O cmdlet verifica o local de restauração de backups existentes e solicita que o usuário escolha um backup apropriado com base na data/hora em que foi tirado e qualquer comentário de backup que o usuário pode ter anexado ao backup. Se houver várias configurações de AD FS com diferentes nomes de serviço de Federação, o usuário será solicitado a escolher primeiro a configuração apropriada de AD FS.
O usuário deve ser administrador local e de domínio para executar este cmdlet.


>[!NOTE] 
>Antes de usar a ferramenta de recuperação rápida AD FS, verifique se o servidor ingressou no domínio antes de restaurar o backup. 

O cmdlet usa os seguintes parâmetros: 

![Ferramenta de restauração rápida do AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descrição detalhada

- **Cadeia decaracteres&gt; storagetype-o tipo de armazenamento que o usuário deseja usar. &lt;**
 "FileSystem" indica que o usuário deseja armazená-lo em uma pasta localmente ou na rede "Azure" indica que o usuário deseja armazená-lo no contêiner de armazenamento do Azure

- **Cadeia &lt;decaracteres&gt; DecryptionPassword** -a senha que foi usada para criptografar todos os arquivos de backup 

- **AzureConnectionCredentials&lt;PSCredential&gt;** -o nome da conta e a chave da conta de armazenamento do Azure

- **Cadeia &lt;decaracteres&gt; AzureStorageContainer** -o contêiner de armazenamento em que o backup será armazenado no Azure

- **Cadeia &lt;decaracteres&gt; StoragePath** -o local em que os backups serão armazenados

- **Cadeia de caracteres&gt; adfsname-o nome da Federação cujo backup foi feito e será restaurado. &lt;** Se isso não for fornecido e houver apenas um nome de serviço de Federação, ele será usado. Se houver mais de um serviço de Federação do qual foi feito backup no local, o usuário será solicitado a escolher um dos serviços de Federação do backup.

- **ServiceAccountCredential&lt; PSCredential&gt;** -especifica a conta de serviço que será usada para o novo serviço de AD FS que está sendo restaurado 

- **Cadeia &lt;decaracteres&gt; GroupServiceAccountIdentifier** -o GMSA que o usuário deseja usar para o novo serviço de AD FS que está sendo restaurado. Por padrão, se nenhum for fornecido, o nome da conta de backup será usado se for GMSA, caso contrário, o usuário será solicitado a colocar uma conta de serviço

- Cadeia de dbconnectionstring-se o usuário quiser usar um banco de forma diferente para a restauração, ele deverá passar a cadeia de conexão SQL ou digitar o wid para wid. **&lt;&gt;**

- **Forçar&lt;bool&gt;** – ignore os prompts que a ferramenta pode ter depois que o backup é escolhido

- **RestoreDKM&lt;bool&gt;** -restaurar o contêiner DKM para o anúncio, deverá ser definido se for para um novo anúncio e o DKM tiver sido copiado inicialmente.

## <a name="restore-examples"></a>Exemplos de restauração

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurar a configuração de AD FS sem o DKM do contêiner de armazenamento do Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurar a configuração de AD FS sem o DKM do sistema de arquivos
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurar a configuração de AD FS com o DKM para o sistema de arquivos 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar a configuração de AD FS para o WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurar a configuração de AD FS para o SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaura a configuração de AD FS com o GMSA especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurar a configuração de AD FS com as credenciais de conta de serviço especificadas

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informações de criptografia
Todos os dados de backup são criptografados antes de enviá-los para a nuvem ou armazená-los no sistema de arquivos.  

Cada documento criado como parte do backup é criptografado usando o AES-256. A senha passada para a ferramenta é usada como uma frase secreta para gerar uma nova senha usando a classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider é usado para gerar o Salt usado pelo AES e a classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Arquivos de log
Toda vez que um backup ou restauração é executado, um arquivo de log é criado. Eles podem ser encontrados no seguinte local:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Ao executar uma restauração, um arquivo PostRestore_Instructions pode ser criado contendo uma visão geral dos provedores de autenticação adicionais, repositórios de atributos e relações de confiança do provedor de declarações locais a serem instalados manualmente antes de iniciar o serviço de AD FS.

## <a name="version-release-history"></a>Histórico de lançamento de versão

### <a name="version-10820"></a>1\.0.82.0 da versão
Liberar Julho de 2019

**Problemas corrigidos:**
- Correção de bug para nomes de conta de serviço AD FS que contêm caracteres de escape LDAP


### <a name="version-10810"></a>Versão: 1.0.81.0
Liberar Abril de 2019

**Problemas corrigidos:**


- Correções de bugs para backup e restauração de certificado
- Informações de rastreamento adicionais para o arquivo de log


### <a name="version-10750"></a>Versão: 1.0.75.0
Liberar Agosto de 2018

**Problemas corrigidos:**
* Atualize backup-ADFS ao usar a opção-BackupDKM.  A ferramenta determinará se o contexto atual tem acesso ao contêiner DKM.  Nesse caso, ele não exigirá privilégios de administrador de domínio ou credenciais de conta de serviço.  Isso permite que os backups automatizados ocorram sem fornecer credenciais explicitamente ou em execução como uma conta de administrador de domínio.

### <a name="version-10730"></a>Versão: 1.0.73.0
Liberar Agosto de 2018

**Problemas corrigidos:**
* Atualizar os algoritmos de criptografia para que o aplicativo seja compatível com FIPS
    
    >[!NOTE]
    > Os backups antigos não funcionarão com a nova versão devido a alterações nos algoritmos de criptografia de acordo com a conformidade com o FIPS
    
* Adicionar suporte para clusters SQL que usam replicação de mesclagem

### <a name="version-10720"></a>Versão: 1.0.72.0
Liberar Julho de 2018

**Problemas corrigidos:**

   - Correção de bug: Corrigido o. Instalador do MSI para dar suporte a atualizações in-loco 

### <a name="10180"></a>1.0.18.0
Liberar Julho de 2018

**Problemas corrigidos:**

   - Correção de bug: manipular senhas de conta de serviço que têm caracteres especiais nelas (por ex., ' & ')
   - Correção de bug: falha na restauração porque Microsoft. IdentityServer. ServiceHost. exe. config está sendo usado por outro processo


### <a name="1000"></a>1.0.0.0
Liberado Outubro de 2016

Versão inicial do AD FS ferramenta de restauração rápida
