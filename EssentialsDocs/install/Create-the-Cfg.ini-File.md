---
title: Criar o Arquivo Cfg.ini
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820117"
---
# <a name="create-the-cfgini-file"></a>Criar o Arquivo Cfg.ini

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O arquivo cfg.ini é usado para automatizar uma instalação do sistema operacional, na seguinte situação:  
  
-   Ao testar a experiência do usuário final com uma imagem pré-instalada no computador de destino, a seção Configuração Inicial será usada para percorrer a instalação em modo assistido ou não assistido. Para fazer isso, consulte [Criar a seção Configuração Inicial](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="BKMK_CreateInit2"></a> Criar a seção configuração inicial  
 Use a seção Configuração Inicial no arquivo cfg.ini para percorrer a instalação em modo assistido ou não assistido.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Para definir a seção Configuração Inicial  
  
1.  Abra o arquivo cfg.ini no Bloco de Notas, se existir; caso contrário, crie um novo arquivo.  
  
2.  Adicione o seguinte texto para criar a seção InitialConfiguration.  
  
    ```  
  
    [InitialConfiguration]  
    ;Optional, display language can only be one of the installed language  
    Language=en-us  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
    ;Optional  
    Locale=en-us  
    ;Optional  
    Country=US  
    ;Optional  
    Keyboard=0409:00000409  
    AcceptEula=true  
    ;This is only required on a server where an OEM EULA has been specified   
    ;by using the OOBE.xml file  
    AcceptOEMEula=true  
    ;Optional. Example: My Company Name  
    CompanyName=EnterCompanyName  
    ServerName=EnterServerName  
    ; Example: CONTOSO  
    NetbiosName=EnterNetbiosDomainName  
    ; Example: contoso.local  
    DNSName=EnterDNSDomain   
    ; Used to set the user name for the domain admin  
    UserName=EnterDomainAdminUserName  
    ;The password has to be strong and at least 8 characters  
    PlainTextPassword=EnterAdminPassword  
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.  
    StdUserName=EnterDomainStandardUserName  
    ;. The password for the domain standard user account has to be strong and at least 8 characters  
    StdUserPlainTextPassword=EnterStandardUserPassword  
    ;Controls the Watson and automatic update settings  
    Settings=All or Updates or None  
    WebDomainName=www.abc.com  
    TrustedCertFileName=c:\cert\a.pfx  
    TrustedCertPassword=Enteryourpassword  
    EnableVPN=true  
    EnableRWA=true  
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>  
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>  
    VpnIPv4StartAddress=<IPV4Address>  
    VpnIPv4EndAddress=<IPV4Address>  
    VpnBaseIPv6Address=<IPV6Address>  
    VpnIPv6PrefixLength=<number>  
    ;All these section are optional.  
     [PostOSInstall]  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
  
    IsHosted=true  
    StaticIPv4Address=<IPV4Address>  
    StaticIPv4Gateway=<IPV4Address>  
    StaticIPv4SubnetMask=<IPV4SubnetMask>   
    StaticIPv6Address=<IPV6Address>  
    StaticIPv6SubnetPrefixLength=<number>  
    StaticIPv6Gateway=<IPV6Address>  
    ClientBackupOn=true  
    FileHistoryOn=true  
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>  
  
    ```  
  
    > [!NOTE]
    >  Não foi fornecida uma opção para selecionar outro idioma durante a Configuração Inicial. Se o sistema for redefinido, o idioma do sistema operacional será aquele instalado originalmente.  
  
    |Nome do parâmetro|Descrição do parâmetro|  
    |--------------------|---------------------------|  
    |*AcceptEula*|Indica que o usuário aceita os Termos de Licença para Software Microsoft. O valor pode ser igual a Verdadeiro ou Falso, mas a instalação continua apenas se estiver definido como Verdadeiro.|  
    |*AcceptOEMEula*|(Opcional) Indica que o usuário aceita o contrato de licença do parceiro. O valor pode ser igual a Verdadeiro ou Falso. Esse campo é obrigatório apenas se o servidor tiver sido comprado de um parceiro que fornece um contrato de licença separado.|  
    |*CompanyName*|(Opcional) Nome da empresa. O nome da empresa é usado para associá-lo ao seu servidor e para personalizar os relatórios da empresa. Pode ter até 254 caracteres.|  
    |*País*|(Opcional) String que representa o país/região desejado Exemplo: US para Estados Unidos.|  
    |*ServerName*|O nome do servidor identifica exclusivamente o servidor na rede. O nome do servidor deve atender aos seguintes critérios:<br /><br /> -Pode ter até 15 caracteres.<br /><br /> -Pode conter letras, números e hifens (-).<br /><br /> -Não deve começar com um hífen.<br /><br /> -Não deve conter espaços.<br /><br /> -Não deve conter apenas números.<br /><br /> Exemplo: ContosoServer.|  
    |*DNSName*|Um domínio interno agrupa o servidor e os computadores cliente para compartilhar um banco de dados comum com nomes de usuário, senhas e outras informações comuns. Os usuários visualizam esse nome ao fazer logon em seus computadores, mas ele é usado apenas internamente e não é igual a um nome de domínio da Internet. O nome de domínio interno deve satisfazer os mesmos critérios especificados para o *ServerName*.<br /><br /> Exemplo: contoso.local.|  
    |*NetbiosName*|Um nome NetBIOS é usado para identificar recursos que estão em execução no servidor. Pode ter até 15 caracteres. Exemplo: Contoso.|  
    |*Idioma*|(Opcional) Especifica o idioma de exibição. Pode ser apenas um dos idiomas instalados. Exemplo: en-us para inglês como usado no Estados Unidos.|  
    |*Localidade*|(Opcional) Especifica o formato de hora e moeda usando um formato *LocaleID* . Exemplo: en-us para moeda e hora exibidas em inglês e formatadas de acordo com os padrões usados nos Estados Unidos.|  
    |*Teclado*|O teclado pode estar nos dois formatos a seguir:<br /><br /> - **layout de teclado: idioma de entrada.** Por exemplo, 0409:00000409, em que 0409 antes de **:** é o idioma de entrada e **00000409** é o layout do teclado. É possível encontrar a lista de layout do teclado sob a chave de registro **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts**.<br /><br /> - **idioma de entrada: o identificador de IME.** Abaixo está uma lista completa de identificadores de IME.<br /><br /> -Método de entrada amárico {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8}<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} Microsoft Pinyin - rápido simples (chinês simplificado)<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} chinês (tradicional) - nova fonética<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chinês (tradicional) - ChangJie<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} chinês (tradicional) - rápido<br /><br /> -Matriz tradicional de chinês {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B}<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} DaYi tradicional chinês<br /><br /> - {03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76}            Microsoft IME (Japanese)<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft IME (coreano)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} antigo IME Hangul (coreano)<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}              Yi Input Method<br /><br /> -Método de entrada Tigrinya {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF}|  
    |*Configurações*|Define a seleção do usuário para atualizações. Use um dos seguintes valores:<br /><br /> **-Todos os itens** é igual ao usar configurações recomendadas.<br /><br /> **-Atualizações** é igual a instalar as atualizações importantes. Apenas<br /><br /> **-None** igual a não verificar atualizações.|  
    |*UserName*|-O nome da nova conta de administrador criada durante a instalação. Os nomes das contas de usuário administrador e padrão devem atender aos seguintes critérios:<br /><br /> -Pode ter até 19 caracteres.<br /><br /> - Cannot contain / \  [ ] &#124; < > + = ; , ? *<br /><br /> -Não deve começar ou terminar com um período.<br /><br /> -Não deve conter dois pontos consecutivos.<br /><br /> -Não deve ser o mesmo que o nome do servidor ou o nome de domínio interno.<br /><br /> -Não deve ser o mesmo que um nome de usuário predefinido, como administrador ou convidado.|  
    |*PlainTextPassword*|Essa é a senha para a nova conta de administrador criada durante a instalação.<br /><br /> -Deve ter pelo menos oito caracteres.<br /><br /> -Deve conter pelo menos três dos quatro categorias a seguir:<br /><br /> -Caracteres letras maiusculas.<br /><br /> -Letras minúsculas.<br /><br /> -Números.<br /><br /> -Símbolos.|  
    |*StdUserName*|O nome da nova conta de usuário padrão criada durante a instalação. Consulte o parâmetro *UserName* para obter os requisitos.|  
    |*StdUserPlainTextPassword*|A senha para a conta de usuário padrão criada durante a instalação.|  
    |WebDomainName|(Opcional) Configure o nome de domínio da Internet do servidor. Esse arquivo permite configurar o nome de domínio de maneira similar ao método usado para configuração manual no assistente de Configuração do Nome de Domínio.|  
    |TrustedCertFileName|(Opcional) Configure o Certificado Confiável para o nome de domínio. Isso permite colocar um cert .PFX, que contém a chave privada.|  
    |TrustedCertPassword|(Opcional) A senha para importar o .PFX.|  
    |EnableVPN|(Opcional) Ativar o VPN por padrão.|  
    |VpnIPv4StartAddress|(Opcional) Definir o endereço inicial da VPN.|  
    |VpnIPv4EndAddress|(Opcional) Definir o endereço final de VPN.|  
    |VpnBaseIPv6Address|(Opcional) Definir o endereço IPV6 de base para VPN.|  
    |VpnIPv6PrefixLength|(Opcional) Definir o comprimento do prefixo do endereço IPv6 de VPN.|  
    |IsHosted|(Opcional) O valor é padrão é falso se não for especificado. Defina esse valor se for instalá-lo no ambiente do hoster. Ele desativará a configuração do roteador.|  
    |StaticIPv4Address|(Opcional) Especifique o endereço IP estático se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |StaticIPv4Gateway|(Opcional) Especifique o endereço de gateway padrão se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |StaticIPv4SubnetMask|(Opcional) Especifique a máscara de sub-rede, se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |StaticIPv6Address|(Opcional) Especifique o endereço IP padrão se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |StaticIPv6SubnetPrefixLength|(Opcional) Especifique a extensão do prefixo da sub-rede IPV6 padrão se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |StaticIPv6Gateway|(Opcional) Especifique o endereço de gateway padrão se desejar configurar um endereço IP estático, em vez de um endereço dinâmico.|  
    |ClientBackupOn|(Opcional) Desligar o backup do Cliente por padrão quando novos clientes entrarem no servidor.|  
    |FileHistoryOn|(Opcional) Desativar o backup de Histórico de Arquivos por padrão quando novos clientes executando a Pré-apresentação do Consumidor do Windows 8 entrarem no servidor.|  
    |EnableRWA|Ele permite acesso via Web remoto ao instalar o Windows Server Essentials, mas ignorará a configuração do roteador. Isso tem suporte apenas na instalação limpa do produto. O valor padrão é falso.|  
    |IPv4DNSForwarder|Define o encaminhador de DNS do IPv4.|  
    |IPv6DNSForwarder|Define o encaminhador de DNS do IPv6.|  
    |LaunchPadHiddenTasks|-(Opcional) você pode ocultar a entrada de Backup ou / e entrada do Admin Dashboard na barra inicial.<br /><br /> -Para desabilitar o painel: LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -Para desabilitar o backup: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -Para desabilitar o backup e o painel: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  Salve o arquivo. Certifique-se de salvar o arquivo como cfg.ini, não como cfg.ini.txt.  
  
    > [!NOTE]
    >  É possível salvar o arquivo em uma unidade flash USB, que pode ser usada para etapas específicas da instalação, ou o arquivo cfg.ini pode ficar localizado na raiz de qualquer disco rígido no servidor de destino. Certifique-se de que a codificação para o arquivo esteja definida como ANSI ou Unicode. A codificação UTF-8 não é aceita.  
  
> [!IMPORTANT]
>  A seção Configuração Inicial do cfg.ini deve ser usada apenas pelo usuário final para personalizar o servidor ou para um parceiro testar a experiência do usuário do servidor usando um arquivo de resposta não assistido. Essa seção do arquivo não serve para ser usada para criação da imagem.  
  
## <a name="see-also"></a>Consulte também  

 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)

 [Introdução ao Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](../install/Testing-the-Customer-Experience.md)

