---
title: Criar o arquivo Cfg.ini
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>Criar o arquivo Cfg.ini

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O arquivo cfg.ini é usado para automatizar uma instalação do sistema operacional nas seguintes situações:  
  
-   Ao testar a experiência do usuário final com uma imagem pré-instalado no computador de destino, a seção de configuração inicial é usada para percorrer a instalação em um modo assistida ou autônoma. Para fazer isso, consulte [criar a seção de configuração inicial](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="BKMK_CreateInit2"></a>Criar a seção de configuração inicial  
 Use a seção de configuração inicial no arquivo cfg.ini para percorrer a instalação em um modo assistida ou autônoma.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Para definir a seção de configuração inicial  
  
1.  Abra o arquivo de cfg.ini no bloco de notas, se ele existir. Caso contrário, crie um novo arquivo.  
  
2.  Adicione o seguinte texto para criar uma seção InitialConfiguration.  
  
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
    >  Uma opção para selecionar um idioma diferente durante a configuração inicial não é fornecida. Se o sistema é reinicializado, o idioma do sistema operacional será aquela que foi instalada originalmente.  
  
    |Nome do parâmetro|Descrição do parâmetro|  
    |--------------------|---------------------------|  
    |*AcceptEula*|Indica que o usuário aceitar os termos de licença de Software da Microsoft. O valor pode ser igual a True ou False, mas a instalação continua somente se for definido como True.|  
    |*AcceptOEMEula*|(Opcional) Indica que o usuário aceitar o contrato de licença do parceiro. O valor pode ser igual a True ou False. Esse campo é obrigatório somente se o servidor foi comprado de um parceiro que forneceu um contrato de licença separada.|  
    |*CompanyName*|(Opcional) Nome da empresa. Nome da sua empresa é usado para associar seu servidor de sua empresa e personalizar seus relatórios de empresa. Pode ter até 254 caracteres.|  
    |*País*|(Opcional) Cadeia de caracteres que representa o país/região desejado. Exemplo: Nos Estados Unidos.|  
    |*ServerName*|O nome do servidor identifica exclusivamente o servidor na rede. Nome do seu servidor deve atender aos seguintes critérios:<br /><br /> -Pode ter até 15 caracteres.<br /><br /> -Pode conter letras, números e hífens (-).<br /><br /> -Não deve começar com um hífen.<br /><br /> -Não deve conter espaços.<br /><br /> -Não deve conter apenas números.<br /><br /> Exemplo: ContosoServer.|  
    |*DNSName*|Um domínio interno agrupa computadores servidor e cliente juntos para compartilhar um banco de dados comuns de nomes de usuário, senhas e outras informações comuns. Os usuários veem esse nome quando eles fizerem logon em seus computadores, mas ele é usado interno somente e não é o mesmo que um nome de domínio da Internet. O nome de domínio interno deve atender os mesmos critérios que são especificados para o *nomedoservidor*.<br /><br /> Exemplo: contoso. local.|  
    |*Nome_netbios*|Um nome NetBIOS é usado para identificar os recursos que estão em execução no servidor. Ele pode ter até 15 caracteres. Exemplo: Contoso.|  
    |*Idioma*|(Opcional) Especifica o idioma de exibição. Ele só pode ser um dos idiomas instalados. Exemplo: en-us para inglês como usado nos Estados Unidos.|  
    |*Localidade*|(Opcional) Especifica o formato de hora e moeda usando um *identificador de localidade* formato. Exemplo: en-us para moeda e a hora exibidos em inglês e formatados de acordo com padrões usados nos Estados Unidos.|  
    |*Teclado*|O teclado pode estar em dois formatos a seguir:<br /><br /> - **layout de entrada de idioma: teclado.** Por exemplo 0409: 00000409, onde 0409 antes **:** é o idioma de entrada, e **00000409** é o layout do teclado. Você pode encontrar a lista de layout do teclado sob a chave do registro **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts**.<br /><br /> - **idioma de entrada: o identificador do IME.** A seguir está uma lista completa de identificadores IME.<br /><br /> -Método de entrada amárico {e429b25a-e5d3-4d1f-9be3-0c608477e3a1} 8f96574e-c86c-4bd6-9666-3f7327d4cbe8}<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e} {fa550b04-5ad7-411f-a5ac-ca038ec515d7} Microsoft Pinyin - Simple Fast (chinês simplificado)<br /><br /> -{531fdebf-9b4c-4a43-a2aa-960e8fcdc732} {b2f9c502-1742-11D4-9790-0080c882687e} chinês (tradicional) - nova fonética<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chinês (tradicional) - ChangJie<br /><br /> -{531fdebf-9b4c-4a43-a2aa-960e8fcdc732} 6024b45f-5c54-11D4-b921-0080c882687e} chinês (tradicional) - rápido<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} matriz tradicional chinês<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} DaYi tradicional chinês<br /><br /> -{03b5835f-f03c-411b-9ce2-aa23e1171e36} {a76c93d9-5523-4e90-aafa-4db112f9ac76} Microsoft IME (japonês)<br /><br /> -{A028ae76-01b1-46c2-99c4-acd9858ae02f} {b5fe1f02-d5f2-4445-9c03-c568f23c99a1} Microsoft IME (coreano)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} o IME Hangul antigo (coreano)<br /><br /> -Método de entrada de Yi {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}<br /><br /> -Método de entrada de Tigrinya {e429b25a-e5d3-4d1f-9be3-0c608477e3a1} 3cab88b7-cc3e-46a6-9765-b772ad7761ff}|  
    |*Configurações*|Define a seleção do usuário para atualizações. Use um dos seguintes valores:<br /><br /> **-Todos os itens** é igual a usar configurações recomendadas.<br /><br /> **-Atualiza** é igual a instala as atualizações importantes. somente<br /><br /> **-Nenhum** igual não verificar se há atualizações.|  
    |*Nome de usuário*|-O nome da nova conta de administrador que é criado durante a instalação. O administrador e nomes de contas de usuário padrão devem atender aos seguintes critérios:<br /><br /> -Pode ter até 19 caracteres.<br /><br /> -Não podem conter / \ [] & #124; < > + =;, ? *<br /><br /> -Não comece ou termine com um ponto.<br /><br /> -Não deve conter dois pontos consecutivos.<br /><br /> -Não deve ser o mesmo que o nome do servidor ou o nome de domínio interno.<br /><br /> -Não deve ser o mesmo que um nome de usuário predefinidas como administrador ou convidado.|  
    |*PlainTextPassword*|Esta é a senha para a nova conta de administrador é criada durante a instalação.<br /><br /> -Deve ter pelo menos oito caracteres.<br /><br /> -Deve conter pelo menos três das quatro categorias a seguir:<br /><br /> -Caracteres em letras maiusculas.<br /><br /> -Minúsculas caracteres.<br /><br /> -Números.<br /><br /> -Símbolos.|  
    |*StdUserName*|O nome da nova conta de usuário padrão que é criado durante a instalação. Consulte o *UserName* parâmetro para requisitos.|  
    |*StdUserPlainTextPassword*|A senha da conta de usuário padrão que é criado durante a instalação.|  
    |WebDomainName|(Opcional) Configure o nome de domínio da Internet do servidor. Esse arquivo permite que você configure o nome de domínio semelhante ao método usado para configuração manual no Assistente de configuração de nome de domínio.|  
    |TrustedCertFileName|(Opcional) Configure o certificado confiável para o nome de domínio. Isso permite que você colocar um. Certificado PFX, que contém a chave privada.|  
    |TrustedCertPassword|(Opcional) A senha para importar o. PFX.|  
    |EnableVPN|(Opcional) Ative VPN por padrão.|  
    |VpnIPv4StartAddress|(Opcional) Defina o endereço de inicial VPN.|  
    |VpnIPv4EndAddress|(Opcional) Defina o endereço de fim VPN.|  
    |VpnBaseIPv6Address|(Opcional) Defina o endereço IPV6 base para VPN.|  
    |VpnIPv6PrefixLength|(Opcional) Defina o comprimento do prefixo do endereço IPv6 de VPN.|  
    |IsHosted|(Opcional) Valor padrão é false se não for especificado. Defina esse valor se você configurar isso no ambiente hoster. Isso desativará a configuração do roteador.|  
    |StaticIPv4Address|(Opcional) Especifique o endereço IP estático se você quiser configurar um endereço IP estático em vez de um endereço dinâmico.|  
    |StaticIPv4Gateway|(Opcional) Especifique o endereço do gateway padrão se você quiser configurar um endereço IP estático em vez de um endereço dinâmico.|  
    |StaticIPv4SubnetMask|(Opcional) Especifique a máscara de sub-rede, se você quiser configurar um endereço IP estático em vez de um endereço dinâmico.|  
    |StaticIPv6Address|(Opcional) Especifique o endereço IP padrão se você quiser configurar um endereço IP estático em vez de um endereço dinâmico.|  
    |StaticIPv6SubnetPrefixLength|(Opcional) Especifica o comprimento do prefixo da sub-rede IPV6 padrão se você quiser configurar um endereço IP estático em vez de um endereço dinâmico.|  
    |StaticIPv6Gateway|(Opcional) Especifique o endereço do gateway padrão se você quiser configurar um endereço IP estático em vez de um dinâmico.|  
    |ClientBackupOn|(Opcional) Desative cliente backup por padrão quando novos clientes ingressa no servidor.|  
    |FileHistoryOn|(Opcional) Desative o histórico de arquivos backup por padrão quando novos clientes que executam o Windows 8 Consumer Preview ingressa no servidor.|  
    |EnableRWA|Ele permitirá o acesso via Web remoto ao instalar o Windows Server Essentials, mas irá ignorar a configuração do roteador. Isso só é compatível com uma instalação limpa do produto. O valor padrão é false.|  
    |IPv4DNSForwarder|Defina o encaminhador DNS IPv4.|  
    |IPv6DNSForwarder|Defina o encaminhador DNS IPv6.|  
    |LaunchPadHiddenTasks|-(Opcional) você pode ocultar a entrada de Backup ou / e a entrada do painel de administração na barra inicial.<br /><br /> -Para desabilitar o painel: LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -Para desativar o backup: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -Para desativar o backup e painel: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  Salve o arquivo. Certifique-se de que você salve o arquivo como cfg.ini, cfg.ini.txt.  
  
    > [!NOTE]
    >  Você pode salvar o arquivo em uma unidade flash USB, que pode ser usada em fases específicas da instalação, ou o arquivo cfg.ini pode ser localizado na raiz de qualquer unidade de disco rígida no servidor de destino. Você deve garantir que a codificação do arquivo é definida como ANSI ou Unicode, não há suporte para a codificação UTF-8.  
  
> [!IMPORTANT]
>  A seção de configuração inicial do cfg.ini só deve ser usada pelo usuário final para personalizar o servidor ou um parceiro testar a experiência do usuário do servidor usando um arquivo de resposta autônomo. Esta seção do arquivo não se destina a ser usado para criar a imagem.  
  
## <a name="see-also"></a>Consulte também  

 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Introdução ao Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

