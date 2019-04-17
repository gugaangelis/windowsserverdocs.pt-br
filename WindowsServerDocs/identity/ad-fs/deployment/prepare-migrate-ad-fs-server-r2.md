---
title: "Preparar para migrar o servidor de Federação 2.0 do AD FS para o AD FS no Windows Server 2012 R2"
description: "Fornece informações sobre Preparando-se para migrar o servidor do AD FS para o Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparar para migrar o servidor de Federação 2.0 do AD FS para o AD FS no Windows Server 2012 R2

Este documento descreve como migrar um AD FS 2.0 ou farm de servidores de Federação do Windows Server 2012 para um farm AD FS do Windows Server 2012 R2.  As etapas podem ser usadas com farms AD FS que usam o trabalho ou SQL Server como o banco de dados subjacente.  
  
-   [Descrição do processo de migração](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Nova funcionalidade do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisitos do AD FS no Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumentar os limites do Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Outras tarefas de migração e as considerações de](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Descrição do processo de migração  
 Para concluir a migração de seu farm de servidores de Federação do AD FS ao Windows Server 2012 R2, você deve concluir as seguintes tarefas:  
  
1.  Exportar, registro e os seguintes dados de configuração no seu farm AD FS existente de backup. Para obter instruções detalhadas sobre como concluir essas tarefas, consulte [migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
As seguintes configurações são migradas com os scripts localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2:  
  
-   Declarações de relações de confiança do provedor, com exceção de regras de declaração personalizada na relação de confiança do Active Directory requerimentos judiciais ou Extrajudiciais provedor. Para obter mais informações, consulte [migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
-   Dependência relações de confiança de terceiros.  
  
-   AD FS gerado internamente, autoassinados token de assinatura e certificados de descriptografia de token.  
  
Qualquer uma das seguintes configurações personalizadas devem ser migrados manualmente:  
  
 -   Configurações de serviço:  
  
     -   Não padrão token de assinatura e certificados de descriptografia token que foram emitidos por uma empresa ou da autoridade de certificação pública.  
  
     -   O certificado de autenticação de servidor SSL usado pelo AD FS.  
  
     -   O certificado de comunicações de serviço usado pelo AD FS (por padrão, isso é o mesmo certificado como o certificado SSL.  
  
      -   Valores não padrão para quaisquer propriedades de serviço de federação, como o tempo de vida AutoCertificateRollover ou SSO.  
  
      -   Configurações de ponto de extremidade do AD FS não padrão e descrições de declaração.  
  
-   Personalizado reivindicar regras na relação de confiança do provedor de declarações do Active Directory.  
  
    -   Personalizações de página de entrada do AD FS  
  
Para obter mais informações, consulte [migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md).  
  
2.  Crie um farm de servidores de Federação do Windows Server 2012 R2.  
  
3.  Importe os dados de configuração original para esse novo farm AD FS do Windows Server 2012 R2.  
  
4.  Configurar e personalizar as páginas de entrada do AD FS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nova funcionalidade do AD FS no Windows Server 2012 R2  
 A seguinte funcionalidade AD FS muda no Windows Server 2012 R2 impacto uma migração do AD FS 2.0 ou AD FS no Windows Server 2012:  
  
**Dependência do IIS**  
   - AD FS no Windows Server 2012 R2 estiver hospedado automaticamente e não requer a instalação do IIS. Certifique-se de que você observe o seguinte como resultado dessa alteração:  
   -   Gerenciamento de certificados SSL para servidores de Federação e computadores de proxy no seu farm AD FS agora deve ser realizado por meio do Windows PowerShell.  
  
**Alterações em configurações e personalizações do AD FS entrar páginas**  
-   No AD FS no Windows Server 2012 R2, existem várias alterações que se destina a melhorar a experiência de entrada para administradores e usuários. As páginas da web hospedado por IIS que existia na versão anterior do AD FS agora são removidos. A aparência do AD FS entrar páginas da web que são hospedados no AD FS internamente e agora pode ser personalizado para adaptar a experiência do usuário. As alterações incluem:  
    -   Personalizando a AD FS experiência de entrada, incluindo a personalização do nome da empresa, logotipo, ilustração e descrição de entrada.  
    -   Personalizando as mensagens de erro.  
    -   Personalizando a experiência do AD FS Home território descoberta, que inclui o seguinte:  
        -   Configurando o provedor de identidade para usar determinados sufixos de email.  
        -   Configurando uma lista de provedores de identidade por confiar terceiros.  
        -   Ignorando a casa território descoberta de intranet.  
        -   Criando temas web personalizado.  
  
Para obter instruções detalhadas sobre como configurar a aparência das páginas do AD FS entrar, consulte [Personalizando as AD FS Sign-in páginas](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Se você tiver a personalização da página da web em seu farm AD FS existente que você deseja migrar para o Windows Server 2012 R2, você poderá recriá-los como parte do processo de migração usando os novos recursos de personalização no Windows Server 2012 R2.  
  
-   **Outras mudanças**  
  
    -   AD FS no Windows Server 2012 R2 é baseado no Windows Identity Foundation WIF () 3.5, não WIF 4.5. Portanto, alguns recursos específicos do 4.5 WIF (por exemplo, requerimentos judiciais ou Extrajudiciais Kerberos e controle de acesso dinâmico) não são suportados no AD FS no Windows Server 2012 R2.  
  
    -   Serviço de registro do dispositivo (DRS) no Windows Server 2012 R2 opera na porta 443; ClientTLS para autenticação de certificado de usuário opera em porta 49443  
  
        -   Para clientes ativos, o navegador não usando a autenticação de modo de transporte do certificado que são especificamente embutidas para apontar para a porta 443, uma alteração de código é necessária para continuar a usar a autenticação de certificado de usuário na porta 49443.  
  
        -   Para aplicativos passivos nenhuma alteração é necessária porque o AD FS redireciona à porta correta para autenticação de certificado de usuário.  
  
        -   Portas de firewall entre o cliente e o proxy devem habilitar o tráfego de porta 49443 para passar por meio de autenticação de certificado de usuário.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisitos do AD FS no Windows Server 2012 R2  
 Para migrar com êxito seu farm AD FS ao Windows Server 2012 R2, você deve atender aos seguintes requisitos:  
  
 Para AD FS funcione, cada computador que você deseja ser uma reunião deve ter ingressado em um domínio.  
  
 Do AD FS em execução no Windows Server 2012 R2 à função, seu domínio do Active Directory deve executar um destes procedimentos:  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 Se você pretende usar um grupo de conta de serviço gerenciado (gMSA) como a conta de serviço do AD FS, você deve ter pelo menos um controlador de domínio em seu ambiente que está em execução no sistema operacional Windows Server 2012 ou Windows Server 2012 R2.  
  
 Se você planeja implantar o serviço de registro de dispositivo (DRS) para o ingresso de empresa de AD como parte da implantação do AD FS, o esquema do AD DS precisa ser atualizado para o nível do Windows Server 2012 R2. Há três maneiras de atualizar o esquema:  
  
1.  Em uma floresta do Active Directory existente, execute adprep /forestprep da pasta de \support\adprep o sistema operacional Windows Server 2012 R2 DVD em qualquer servidor de 64 bits que executa o Windows Server 2008 ou posterior. Nesse caso, nenhum controlador de domínio adicional deve ser instalado, e nenhum controlador de domínio existentes precisa ser atualizados.  
  
Para executar adprep/forestprep, você deve ser um membro do grupo Administradores de esquemas, do grupo Administradores corporativos e do grupo Admins. do domínio do domínio que hospeda o mestre de esquema.  
  
2.  Em uma floresta do Active Directory existente, instale um controlador de domínio que executa o Windows Server 2012 R2. Nesse caso, adprep /forestprep é executado automaticamente como parte da instalação do controlador de domínio.  
  
Durante a instalação do controlador de domínio, talvez seja necessário especificar credenciais adicionais para executar adprep /forestprep.  
  
3.  Crie uma nova floresta do Active Directory instalando o AD DS em um servidor que executa o Windows Server 2012 R2. Nesse caso, adprep /forestprep não precisa ser executado porque o esquema será criado inicialmente com todos os contêineres necessários e objetos para oferecer suporte ao DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Suporte do SQL Server do AD FS no Windows Server 2012 R2  
 Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Aumentar os limites do Windows PowerShell  
 Se você tiver mais de 1000 declarações relações de confiança do provedor e terceiro confie no farm AD FS, ou se você vir o seguinte erro ao tentar executar a ferramenta de exportação e importação de migração do AD FS, você deve aumentar os limites do Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Esse erro é lançado porque o limite de memória do padrão de sessão do Windows PowerShell é muito baixo. No Windows PowerShell 2.0, a memória de padrão de sessão é 150MB. No Windows PowerShell 3.0, a memória de padrão de sessão é 1024MB. Você pode verificar o limite de memória de sessão remota do Windows PowerShell usando o seguinte comando:`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Você pode aumentar o limite, executando o seguinte comando:`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Outras tarefas de migração e as considerações de  
 Para migrar com êxito seu farm AD FS ao Windows Server 2012 R2, certifique-se de que você esteja ciente das seguintes opções:  
  
-   Os scripts de migração localizados na pasta \support\adfs no CD de instalação do Windows Server 2012 R2 exigem que você mantenha o mesmo nome de farm de servidor de Federação e o nome de identidade de conta de serviço que você usou em seu farm herdado do AD FS ao migrar para o Windows Server 2012 R2.  
  
-   Se você quiser migrar um farm de SQL Server AD FS, observe que o processo de migração envolve a criação de uma nova instância de banco de dados SQL para os quais você deve importar os dados de configuração original.  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrando o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando o AD FS migração ao Windows Server 2012 R2](verify-ad-fs-migration.md)