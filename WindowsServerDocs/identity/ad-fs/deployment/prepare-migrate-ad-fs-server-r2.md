---
title: Preparar para migrar o servidor de Federação AD FS 2,0 para AD FS no Windows Server 2012 R2
description: Fornece informações sobre como preparar-se para migrar o servidor de AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 785a2a7c425e80b8f41e2c567826c34471cce9e9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959808"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparar para migrar o servidor de Federação AD FS 2,0 para AD FS no Windows Server 2012 R2

Este documento descreve como migrar um farm de servidores de Federação AD FS 2,0 ou Windows Server 2012 para um farm de AD FS do Windows Server 2012 R2.  As etapas podem ser usadas com AD FS farms que usam o WID ou SQL Server como o banco de dados subjacente.  
  
-   [Estrutura de tópicos do processo de migração](prepare-migrate-ad-fs-server-r2.md#migration-process-outline)  
  
-   [Novas funcionalidades do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisitos do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumentando os limites do Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Outras considerações e tarefas de migração](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Estrutura de tópicos do processo de migração

 Para concluir a migração de seu farm de servidores de federação do AD FS para o Windows Server 2012 R2, é preciso concluir as seguintes tarefas:  
  
1.  Exportação, registro e backup dos seguintes dados de configuração no farm do AD FS existente. Para obter instruções detalhadas sobre como concluir essas tarefas, consulte [Migração do Proxy do Servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
As configurações a seguir são migradas com os scripts localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2:  
  
-   Objetos de confiança do provedor de declarações, com exceção das regras de declaração personalizadas no objeto de confiança do provedor declarações do Active Directory. Para obter mais informações, consulte [Migração do Servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
-   Objetos de confiança da terceira parte confiável.  
  
-   Certificados de descriptografia de token e de assinatura de token autoassinados gerados internamente do AD FS.  
  
Todas as configurações personalizadas a seguir devem ser migradas manualmente:  
  
- Configurações de serviço:  
  
  - Certificados de descriptografia de token e de assinatura de token não padrão que foram emitidos por uma empresa ou autoridade de certificação pública.  
  
  - O certificado de autenticação do servidor SSL usado pelo AD FS.  
  
  - O certificado de comunicações de serviço usado pelo AD FS (por padrão, esse é o mesmo certificado do certificado SSL).  
  
    -   Valores não padrão para todas as propriedades do serviço de federação, como AutoCertificateRollover ou tempo de vida SSO.  
  
    -   Descrições de declarações e configurações de ponto de extremidade do AD FS não padrão.  
  
-   Regras de declaração personalizada sobre o objeto de confiança do provedor de declarações do Active Directory.  
  
    -   Personalizações de página de entrada do AD FS  
  
Para obter mais informações, consulte [Migração do Servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
2. Criar um farm de servidor de federação do Windows Server 2012 R2.  
  
3. Importar os dados de configuração original para o novo farm do AD FS do Windows Server 2012 R2  
  
4. Configurar e personalizar as páginas de entrada do AD FS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Novas funcionalidades do AD FS no Windows Server 2012 R2  
 As alterações de funcionalidade a seguir AD FS no Windows Server 2012 R2 afetam uma migração do AD FS 2,0 ou AD FS no Windows Server 2012:  
  
**Dependência do IIS**  
   - O AD FS no Windows Server 2012 R2 é auto-hospedado e não requer a instalação do IIS. Observe o seguinte como resultado dessa alteração:  
   -   O gerenciamento de certificados SSL para servidores de federação e computadores proxy no farm do AD FS deve agora ser realizado através do Windows PowerShell.  
  
**Alterações nas configurações e personalizações das páginas de entrada do AD FS**  
-   Em AD FS no Windows Server 2012 R2, há várias alterações pretendidas para melhorar a experiência de entrada para administradores e usuários. As páginas da Web hospedadas no IIS que existiam na versão anterior do AD FS agora foram removidas. A aparência das páginas da Web de entrada do AD FS são auto-hospedadas no AD FS e agora podem ser personalizadas de acordo com a experiência do usuário. As alterações incluem:  
    -   Personalização da experiência de entrada do AD FS, incluindo a personalização do logotipo, da ilustração, da descrição de entrada e do nome da empresa.  
    -   Personalização das mensagens de erro.  
    -   Personalização da experiência de descoberta de realm da página inicial do AD FS, que inclui o seguinte:  
        -   Configurar seu provedor de identidade para usar certos sufixos de email.  
        -   Configurar uma lista de provedores de identidade por terceira parte confiável.  
        -   Ignorar a descoberta de realm da página inicial para intranet.  
        -   Criar temas da Web personalizados.  
  
Para obter instruções detalhadas sobre como configurar a aparência das páginas de entrada do AD FS, consulte [Customizing the AD FS Sign-in Pages](../operations/ad-fs-customization-in-windows-server.md).  
  
Se você tiver personalização de página da Web no farm de AD FS existente que deseja migrar para o Windows Server 2012 R2, poderá recriá-las como parte do processo de migração usando os novos recursos de personalização do Windows Server 2012 R2.  
  
-   **Outras alterações**  
  
    -   AD FS no Windows Server 2012 R2 baseia-se no Windows Identity Foundation (WIF) 3,5, não no WIF 4,5. Portanto, alguns recursos específicos do WIF 4,5 (por exemplo, declarações Kerberos e controle de acesso dinâmico) não têm suporte em AD FS no Windows Server 2012 R2.  
  
    -   O DRS (serviço de registro de dispositivo) no Windows Server 2012 R2 opera na porta 443; ClientTLS para autenticação de certificado de usuário opera na porta 49443  
  
        -   Para clientes ativos e sem navegador usando autenticação de modo de transporte de certificado que são especificamente embutidos em código para apontar para a porta 443, é necessária uma alteração do código para a autenticação de certificado do usuário continuar a ser usada na porta 49443.  
  
        -   Para aplicações passivas, nenhuma mudança é necessária, porque o AD FS redireciona para a porta correta para a autenticação de certificado do usuário.  
  
        -   As portas de firewall entre o cliente e o proxy devem permitir tráfego na porta 49443 para passar pela autenticação de certificado do usuário.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisitos do AD FS no Windows Server 2012 R2  
 Para migrar com êxito o farm de AD FS para o Windows Server 2012 R2, você deve atender aos seguintes requisitos:  
  
 Para que AD FS funcionem, cada computador que você deseja que seja uma federação deve ingressar em um domínio.  
  
 Para AD FS em execução no Windows Server 2012 R2 para funcionar, seu domínio de Active Directory deve executar um dos seguintes:  
  
- Windows Server 2012 R2  
  
- Windows Server 2012  
  
- Windows Server 2008 R2  
  
- Windows Server 2008  
  
  Se você planeja usar uma conta de serviço gerenciado de grupo (gMSA) como a conta de serviço para AD FS, você deve ter pelo menos um controlador de domínio em seu ambiente em execução no sistema operacional Windows Server 2012 ou Windows Server 2012 R2.  
  
  Se você planeja implantar o serviço de registro de dispositivo (DRS) para AD Workplace Join como parte de sua implantação de AD FS, o esquema de AD DS precisa ser atualizado para o nível do Windows Server 2012 R2. Existem três maneiras de atualizar o esquema:  
  
1.  Em uma floresta Active Directory existente, execute adprep/forestprep na pasta \support\adprep do DVD do sistema operacional Windows Server 2012 R2 em qualquer servidor de 64 bits que execute o Windows Server 2008 ou posterior. Nesse caso, nenhum controlador de domínio adicional precisa ser instalado e nenhum controlador de domínio existente precisa ser atualizado.  
  
Para executar adprep /forestprep, é necessário ser membro do grupo Administradores de Esquema, do grupo Administradores de Empresa e do grupo Admins. do Domínio do domínio que hospeda o mestre de esquema.  
  
2. Em uma floresta Active Directory existente, instale um controlador de domínio que execute o Windows Server 2012 R2. Nesse caso, adprep /forestprep é executado automaticamente como parte da instalação do controlador de domínio.  
  
Durante a instalação do controlador de domínio, talvez seja necessário especificar credenciais adicionais a fim de executar adprep /forestprep.  
  
3. Crie uma nova floresta Active Directory instalando AD DS em um servidor que executa o Windows Server 2012 R2. Nesse caso, adprep /forestprep não precisa ser executado, porque o esquema será criado inicialmente com todos os objetos e contêineres necessários para suportar o DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Suporte do SQL Server para o AD FS no Windows Server 2012 R2  
 Se você deseja criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Aumentando os limites do Windows PowerShell  
 Se você tiver mais de 1000 objetos de confiança do provedor de declarações e objetos de confiança da terceira parte confiável em seu farm do AD FS, ou se vir o seguinte erro ao tentar executar a ferramenta de importação/exportação de migração do AD FS, será preciso aumentar os limites do Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Este erro foi lançado porque o limite de memória padrão de sessão do Windows PowerShell é muito baixo. No Windows PowerShell 2.0, a memória padrão de sessão é de 150 MB. No Windows PowerShell 3.0, a memória padrão de sessão é de 1024 MB. É possível verificar o limite de memória de sessão remota do Windows PowerShell usando o seguinte comando: `Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Você pode aumentar o limite executando o seguinte comando: `Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Outras considerações e tarefas de migração  
 Para migrar com sucesso seu farm do AD FS para o Windows Server 2012 R2, esteja ciente do seguinte:  
  
-   Os scripts de migração localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2 exigem que você retenha o mesmo nome do farm de servidores de Federação e o nome de identidade da conta de serviço que usou em seu farm de AD FS herdado ao migrá-lo para o Windows Server 2012 R2.  
  
-   Se você deseja migrar um farm do AD FS do SQL Server, observe que o processo de migração envolve a criação de uma nova instância de banco de dados do SQL para a qual é preciso importar os dados de configuração originais.  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de Federação do Active Directory (AD FS) serviços de função para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migrando o servidor de Federação de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrando o proxy do servidor de Federação AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificação da migração do AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)
