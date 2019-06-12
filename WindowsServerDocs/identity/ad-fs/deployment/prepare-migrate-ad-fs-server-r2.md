---
title: Preparar para migrar o servidor do AD FS 2.0 Federation para o AD FS no Windows Server 2012 R2
description: Fornece informações sobre a preparação migrar o servidor do AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cb301d0d68f00625ccea8c11d315b9defffe40f3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444529"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparar para migrar o servidor do AD FS 2.0 Federation para o AD FS no Windows Server 2012 R2

Este documento descreve como migrar um AD FS 2.0 ou um farm de servidores de Federação do Windows Server 2012 para um farm do AD FS do Windows Server 2012 R2.  As etapas podem ser usadas com farms do AD FS que usam o WID ou SQL Server como banco de dados subjacente.  
  
-   [Contorno do processo de migração](prepare-migrate-ad-fs-server-r2.md#migration-process-outline)  
  
-   [Nova funcionalidade do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisitos do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumentar os limites do Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Outras considerações e tarefas de migração](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Estrutura de tópicos do processo de migração

 Para concluir a migração de seu farm de servidores de federação do AD FS para o Windows Server 2012 R2, é preciso concluir as seguintes tarefas:  
  
1.  Exportação, registro e backup dos seguintes dados de configuração no farm do AD FS existente. Para obter instruções detalhadas sobre como concluir essas tarefas, consulte [Migração do Proxy do Servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
As seguintes configurações são migradas com os scripts localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2:  
  
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
 A seguinte funcionalidade do AD FS é alterado no Windows Server 2012 R2 impacto em uma migração do AD FS 2.0 ou AD FS no Windows Server 2012:  
  
**Dependência do IIS**  
   - O AD FS no Windows Server 2012 R2 é auto-hospedado e não requer a instalação do IIS. Observe o seguinte como resultado dessa alteração:  
   -   O gerenciamento de certificados SSL para servidores de federação e computadores proxy no farm do AD FS deve agora ser realizado através do Windows PowerShell.  
  
**Alterações do AD FS sign-in das páginas configurações e personalizações**  
-   No AD FS no Windows Server 2012 R2, há várias alterações destinadas a melhorar a experiência de entrada para administradores e usuários. As páginas da Web hospedadas no IIS que existiam na versão anterior do AD FS agora foram removidas. A aparência das páginas da Web de entrada do AD FS são auto-hospedadas no AD FS e agora podem ser personalizadas de acordo com a experiência do usuário. As alterações incluem:  
    -   Personalização da experiência de entrada do AD FS, incluindo a personalização do logotipo, da ilustração, da descrição de entrada e do nome da empresa.  
    -   Personalização das mensagens de erro.  
    -   Personalização da experiência de descoberta de realm da página inicial do AD FS, que inclui o seguinte:  
        -   Configurar seu provedor de identidade para usar certos sufixos de email.  
        -   Configurar uma lista de provedores de identidade por terceira parte confiável.  
        -   Ignorar a descoberta de realm da página inicial para intranet.  
        -   Criar temas da Web personalizados.  
  
Para obter instruções detalhadas sobre como configurar a aparência das páginas de entrada do AD FS, consulte [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Se você tiver a personalização da página da web em seu farm do AD FS existente que você deseja migrar para o Windows Server 2012 R2, você poderá recriá-los como parte do processo de migração usando os novos recursos de personalização do Windows Server 2012 R2.  
  
-   **Outras alterações**  
  
    -   AD FS no Windows Server 2012 R2 se baseia no Windows Identity Foundation (WIF) 3.5, não no WIF 4.5. Portanto, alguns recursos específicos do WIF 4.5 (por exemplo, declarações Kerberos e controle de acesso dinâmico) não são suportados no AD FS no Windows Server 2012 R2.  
  
    -   Serviço de registro de dispositivo (DRS) no Windows Server 2012 R2 opera na porta 443; ClientTLS para a autenticação de certificado de usuário opera na porta 49443  
  
        -   Para clientes ativos e sem navegador usando autenticação de modo de transporte de certificado que são especificamente embutidos em código para apontar para a porta 443, é necessária uma alteração do código para a autenticação de certificado do usuário continuar a ser usada na porta 49443.  
  
        -   Para aplicações passivas, nenhuma mudança é necessária, porque o AD FS redireciona para a porta correta para a autenticação de certificado do usuário.  
  
        -   As portas de firewall entre o cliente e o proxy devem permitir tráfego na porta 49443 para passar pela autenticação de certificado do usuário.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisitos do AD FS no Windows Server 2012 R2  
 Para migrar com sucesso seu farm do AD FS para o Windows Server 2012 R2, você deve atender aos seguintes requisitos:  
  
 Para AD FS funcione, cada computador que você deseja ser uma federação deve estar associado a um domínio.  
  
 Para o AD FS em execução no Windows Server 2012 R2 para a função, seu domínio do Active Directory deve executar um destes procedimentos:  
  
- Windows Server 2012 R2  
  
- Windows Server 2012  
  
- Windows Server 2008 R2  
  
- Windows Server 2008  
  
  Se você planeja usar um grupo de conta de serviço gerenciado (gMSA) como a conta de serviço do AD FS, você deve ter pelo menos um controlador de domínio em seu ambiente que está em execução no sistema operacional Windows Server 2012 ou Windows Server 2012 R2.  
  
  Se você planeja implantar o serviço de registro de dispositivo (DRS) para ingresso no AD como parte da sua implantação do AD FS, o esquema do AD DS precisa ser atualizado para o nível do Windows Server 2012 R2. Existem três maneiras de atualizar o esquema:  
  
1.  Em uma floresta do Active Directory existente, execute adprep /forestprep da pasta \support\adprep do DVD do sistema operacional do Windows Server 2012 R2 em qualquer servidor de 64 bits que executa o Windows Server 2008 ou posterior. Nesse caso, nenhum controlador de domínio adicional precisa ser instalado e nenhum controlador de domínio existente precisa ser atualizado.  
  
Para executar adprep /forestprep, é necessário ser membro do grupo Administradores de Esquema, do grupo Administradores de Empresa e do grupo Admins. do Domínio do domínio que hospeda o mestre de esquema.  
  
2. Em uma floresta do Active Directory existente, instale um controlador de domínio que executa o Windows Server 2012 R2. Nesse caso, adprep /forestprep é executado automaticamente como parte da instalação do controlador de domínio.  
  
Durante a instalação do controlador de domínio, talvez seja necessário especificar credenciais adicionais a fim de executar adprep /forestprep.  
  
3. Crie uma nova floresta do Active Directory instalando o AD DS em um servidor que executa o Windows Server 2012 R2. Nesse caso, adprep /forestprep não precisa ser executado, porque o esquema será criado inicialmente com todos os objetos e contêineres necessários para suportar o DRS.  
  
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
  
-   Os scripts de migração localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2 requerem que você mantenha o mesmo nome de farm de servidor de Federação e o nome de identidade de conta de serviço que você usou no farm do AD FS herdado quando você migrá-lo para Windows Server 2012 R2.  
  
-   Se você deseja migrar um farm do AD FS do SQL Server, observe que o processo de migração envolve a criação de uma nova instância de banco de dados do SQL para a qual é preciso importar os dados de configuração originais.  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando a migração do AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)