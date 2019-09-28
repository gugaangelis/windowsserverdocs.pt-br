---
title: Gerenciar o Log de Inventário de Software
description: Descreve como gerenciar o log de inventário de software
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 812173d1-2904-42f4-a9e2-de19effec201
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd8a26d158f53121074881ac8ff204287f9a19ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382966"
---
# <a name="manage-software-inventory-logging"></a>Gerenciar o Log de Inventário de Software

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Este documento descreve como gerenciar o log de inventário de software, um recurso que ajuda os administradores do Datacenter a registrar facilmente os dados de gerenciamento de ativos de software da Microsoft para suas implantações ao longo do tempo. Este documento descreve como gerenciar o Log de Inventário de Software. Antes de usar o log de inventário de software com o Windows Server 2012 R2, verifique se Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) estão instalados em cada sistema que precisa ser inventariado. Nenhuma atualização de clique é necessária para o Windows Server 2016. Esse recurso é executado localmente em cada servidor a ser inventariado. Ele não coleta dados de servidores remotos.  

O recurso de log de inventário de software também pode ser adicionado a duas versões do Windows Server antes do Windows Server 2012 R2. Você pode instalar as seguintes atualizações para adicionar a funcionalidade de log de inventário de software ao Windows Server 2012 e ao Windows Server 2008 R2 SP1:

- **Windows Server 2012 (Standard ou Datacenter Edition)** 

> [!NOTE] 
> Verifique se você tem o [WMF 4,0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) instalado antes de aplicar o pacote de atualização abaixo.

-  Pacote de atualização do WMF 4,0 para Windows Server 2012: [KB 3119938](https://support.microsoft.com/en-us/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> Verifique se você tem o [WMF 4,0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) instalado antes de aplicar o pacote de atualização abaixo.


- Requer o [.NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)


- Pacote de atualização do WMF 4,0 para Windows Server 2008 R2: [KB 3109118](https://support.microsoft.com/en-us/kb/3109118)


Há dois métodos principais realizar o inventário usando esse recurso:  
  
1.  Iniciando a funcionalidade de registro em log SIL para coletar fontes de dados SIL e encaminhar a carga pela rede para um destino especificado (URI) a cada hora.  
  
2.  Consultando manualmente os dados de SIL usando o PowerShell ou WMI em qualquer intervalo.  
  
Iniciar o registro em log de SIL envolve algum planejamento e previsão, mas tem vantagens significativas em comparação com consultar os dados manualmente. Usar o registro em log de SIL tem as três principais vantagens a seguir para administradores de data center:  
  
-   Um histórico em andamento (log) pode ser coletado ao longo do tempo, permitindo relatórios flexíveis e abrangentes de uma única fonte.  
  
-   Os desafios de descoberta de computador típicos das diversas ferramentas de inventário podem ser superados.  
  
-   Os desafios de limite de relação de confiança e a necessidade de privilégios de usuário elevados típicos de muitas ferramentas de inventário podem ser superados, mantendo um alto nível de segurança uma vez que os dados são criptografados por HTTPS com SSL.  
  
O Log de Inventário de Software é instalado por padrão, mas não é iniciado por padrão. Todas as configurações do Log de Inventário de Software são feitas com cmdlets do PowerShell. Existem apenas algumas opções de configuração para o Log de Inventário de Software. Este documento descreve essas opções e sua finalidade pretendida, bem como os cmdlets usados para coleta de dados (se o segundo método acima estiver sendo utilizado).  
  
**Neste documento**  
  
As opções de configuração abordadas neste documento incluem:  
  
-   [Iniciando e parando o log de inventário de software](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [Log de inventário de software ao longo do tempo](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [Exibindo dados de log de inventário de software](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [Excluindo dados registrados no log de inventário de software](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [Fazendo backup e restaurando dados registrados pelo log de inventário de software] Manage-software-inventário-Logging. MD # BKMK_Step5)  
  
-   [Lendo dados registrados e publicados pelo log de inventário de software](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [Segurança de log de inventário de software](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [Trabalhando com configurações de data e hora no log de inventário de software do Windows Server](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [Habilitando e Configurando o log de inventário de software em um disco rígido virtual montado](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [Visão geral do uso do log de inventário de software no Windows Server 2012 R2 sem KB 3000850](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [Usando o log de inventário de software em um ambiente Windows Server 2012 R2 Hyper-V sem KB 3000850](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte usando cmdlets.

  
## <a name="BKMK_Step1"></a>Iniciando e parando o log de inventário de software  
A coleta diária de log de inventário de software e o encaminhamento pela rede devem ser habilitados em um computador que esteja executando o Windows Server 2012 R2 para registrar o inventário de software.  
  
> [!NOTE]  
> Você pode usar o cmdlet **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** do PowerShell para recuperar informações sobre Serviço de Log de Inventário de Software, incluindo se ele está em execução ou parado.  
  
#### <a name="to-start-software-inventory-logging"></a>Para iniciar o Log de Inventário de Software  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  Abra o PowerShell como Administrador.  
  
3.  No prompt do PowerShell, digite **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> É possível definir o destino sem definir uma impressão digital do certificado, mas se você fizer isso, os encaminhamentos falharão e os dados serão armazenados localmente por até um valor padrão de 30 dias (após o qual eles serão excluídos). Depois que um hash de certificado válido é definido para o destino (e um certificado válido correspondente é instalado no repositório de LocalMachine/Pessoal), dados armazenados localmente serão encaminhados para o destino assim que ele é configurado para aceitar esses dados com esse certificado (consulte [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) para obter mais informações).  
  
#### <a name="to-stop-software-inventory-logging"></a>Para parar o Log de Inventário de Software  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  Abra o PowerShell como Administrador.  
  
3.  No prompt do PowerShell, digite **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>Configurando o Log de Inventário de Software  
Há três etapas para configurar o Log de Inventário de Software para encaminhar dados para um servidor de agregação ao longo do tempo:  
  
1.  Use **set-SilLogging – targetUri** para especificar o endereço Web do servidor de agregação (deve começar com "https://").  
  
2.  Use **Set-SilLogging –CertificateThumbprint** para especificar o hash de impressão digital do seu certificado SSL válido a ser usado para autenticar as transmissões de dados para o servidor de agregação (o servidor de agregação precisará ser configurado para aceitar o hash).  
  
3.  Instale um certificado SSL válido (para sua rede) em **Computador Local/Repositório Pessoal** (ou **/ComputadorLocal/MEU**) do servidor local, de onde encaminhar os dados.  
  
É melhor concluir estas etapas antes de usar **Start-SilLogging**.  Se você quiser usá-los depois de usar **Start-SilLogging**, basta parar e iniciar o SIL novamente.  Ou, você pode usar o cmdlet Publish-SilData para garantir que o servidor de agregação tenha um conjunto completo de dados para esse servidor.  
  
Para obter um guia abrangente para configurar a estrutura do SIL como um todo, consulte [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).  Em particular, se **Publish-SilData** produzir um erro ou o Log de Inventário de Software falhar, consulte a seção solução de problemas.  
  
## <a name="BKMK_Step2"></a>Log de inventário de software ao longo do tempo  
Se o Log de Inventário de Software foi iniciado por um administrador, a coleta de hora em hora e o encaminhamento dos dados par o servidor de agregação (URI de destino) começa. O primeiro encaminhamento será um conjunto de dados completo dos mesmos dados que [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) recupera e exibe no console em um ponto no tempo. Depois, a cada intervalo, o SIL fará uma verificação dos dados e encaminhará somente uma pequena confirmação de identificação para o servidor de agregação de destino se não houver nenhuma alteração nos dados desde a última coleta. Se algum valor tiver sido alterado, o SIL enviará novamente um conjunto de dados completo.  
  
> [!IMPORTANT]  
> Se em qualquer intervalo o URI de destino estiver inacessível ou a transferência de dados pela rede for malsucedida por algum motivo, os dados coletados serão armazenados localmente por até um valor padrão de 30 dias (após o qual eles serão excluídos). No próximo encaminhamento bem-sucedido dos dados para o servidor de agregação de destino, todos os dados armazenados localmente serão encaminhados e os dados armazenados em cache locais serão excluídos.  
  
## <a name="BKMK_Step3"></a>Exibindo dados de log de inventário de software  
Além dos cmdlets do PowerShell descritos na seção anterior, seis cmdlets adicionais podem ser usados para coletar dados de Log de Inventário de Software:  
  
-   **[Get-SilComputer](https://technet.microsoft.com/library/dn283392.aspx)** : Exibe os valores de um ponto no tempo para dados relacionados ao sistema operacional e servidor específicos, bem como o FQDN ou nome do host físico, se disponível.  
  
-   **[Get-SilComputerIdentity (KB 3000850)](https://technet.microsoft.com/library/dn858074.aspx)** : Exibe identificadores utilizados pelo SIL para servidores individuais.  
  
-   **[Get-SilData](https://technet.microsoft.com/library/dn283388.aspx)** : Exibe uma coleta de um ponto no tempo de todos os dados de Log de Inventário de Software.  
  
-   **[Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)** : Exibe a identidade do ponto no tempo de todo o software instalado no computador.  
  
-   **[Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx)** : Exibe o número total de solicitações únicas do dispositivo cliente e solicitações do usuário do cliente do servidor de dois dias antes.  
  
-   **[Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx)** : Exibe a lista do ponto no tempo de todas as atualizações do Windows instaladas no computador.  
  
Um cenário típico de caso de uso dos cmdlets do Log de Inventário de Software seria o de um administrador consultar o Log de Inventário de Software para uma coleta de um ponto no tempo de todos os dados do Log de Inventário de Software usando o [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx).  
  
**Exemplo de saída**  
  
```  
PS C:\> Get-SilData   
  
ID                 : 961FF8A1-8549-4BEC-8DF6-3B3E32C26FFA  
UUID               : B49ACB4C-7D9C-4806-9917-AE750BB3DA84  
VMGUID             : E84CCCBD-0D0F-486B-A424-9780C7CF92E4  
Name               : Server01Guest.Test.Contoso.com  
HypervisorHostName : Server01.Test.Contoso.com  
  
ID          : {F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}  
Name        : Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219  
InstallDate : 12/5/2013  
Publisher   : Microsoft Corporation  
Version     : 10.0.40219  
  
ID          : {89F4137D-6C26-4A84-BDB8-2E5A4BB71E00}  
Name        : Microsoft Silverlight  
InstallDate : 3/20/2014  
Publisher   : Microsoft Corporation  
Version     : 5.1.30214.0  
  
ChassisSerialNumber       : 4452-0564-0284-2290-0113-6804-05  
CollectedDateTime         : 10/27/2014 4:01:33 PM  
Model                     : Virtual Machine  
Name                      : Server01Guest.Test.Contoso.com  
NumberOfCores             : 1  
NumberOfLogicalProcessors : 1  
NumberOfProcessors        : 1  
OSName                    : Microsoft Windows Server 2012 R2 Datacenter  
OSSku                     : 8  
OSSuite                   : 400  
OSSuiteMask               : 400  
OSVersion                 : 6.3.9600  
ProcessorFamily           : 179  
ProcessorManufacturer     : GenuineIntel  
ProcessorName             : Intel(R) Xeon(R) CPU           E5440  @ 2.83GHz  
SystemManufacturer        : Microsoft Corporation  
  
```  
  
> [!NOTE]  
> A saída desse cmdlet é igual à de todos os outros cmdlets **Get-Sil** para esse recurso combinados, mas é fornecida para o console de maneira assíncrona, portanto a ordem dos objetos pode nem sempre ser a mesma.  
>   
> Não é necessário ter o Log de Inventário de Software iniciado para usar os cmdlets **Get-Sil**.  
  
## <a name="BKMK_Step4"></a>Excluindo dados registrados no log de inventário de software  
O Log de Inventário de Software não tem a finalidade de ser um componente crítico. Seu design destina-se a afetar as operações do sistema local o mínimo possível, mantendo um alto nível de confiabilidade. Isso também permite que o administrador exclua manualmente o banco de dados de log de inventário de software e os arquivos de suporte (todos os arquivos no diretório \Windows\System32\LogFiles\SIL) para atender às necessidades operacionais.  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>Para excluir dados registrados pelo Log de Inventário de Software  
  
1. No PowerShell, pare o Log de Inventário de Software com o comando **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** .  
  
2. Abra o Windows Explorer.  
  
3. Vá para **\Windows\System32\Logfiles\SIL\\**  
  
4. Exclua todos os arquivos da pasta.  
  
## <a name="BKMK_Step5"></a>Fazendo backup e restaurando dados registrados no log de inventário de software  
O Log de Inventário de Software armazenará temporariamente coletas de dados de hora em hora se os encaminhamentos pela rede estiverem falhando. Os arquivos de log são armazenados no diretório \Windows\System32\LogFiles\SIL\. Os backups desses dados de Log de Inventário de Software podem ser feitos com seus backups de servidora programados regularmente.  
  
> [!IMPORTANT]  
> Se for necessária uma instalação de reparo ou atualização do sistema operacional por algum motivo, os arquivos de log armazenados localmente serão perdidos.  Se esses dados forem essenciais para operações, é recomendável fazer backup antes da instalação do novo sistema operacional. Após o reparo ou atualização, simplesmente restaure no mesmo local.  
  
> [!NOTE]  
> Se por algum motivo o gerenciamento da duração da retenção dos dados registrados localmente pelo SIL se tornar importante, isso poderá ser configurado alterando o valor do registro aqui: \HKEY_LOCAL_MACHINE @ no__t-0SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. O padrão é ' 30 ' por 30 dias.  
  
## <a name="BKMK_Step6"></a>Lendo dados registrados e publicados pelo log de inventário de software  
Dados registrados por SIL, mas armazenados localmente (se o encaminhamento para o URI de destino falhar) ou dados que são encaminhados com êxito para o servidor de agregação de destino, são armazenados em um arquivo binário (para os dados de cada dia). Para exibir esses dados no PowerShell, use o cmdlet [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) .  
  
## <a name="BKMK_Step7"></a>Segurança de log de inventário de software  
São necessários privilégios administrativos no servidor local para recuperar dados do WMI de Log de Inventário de Software e das APIs do PowerShell.  
  
Para utilizar com êxito a capacidade total do recurso de Log de Inventário de Software para encaminhar dados para um ponto de agregação continuamente ao longo do tempo (em intervalos de hora em hora), um administrador precisa utilizar certificados de cliente para garantir sessões SSL seguras para a transferência de dados por HTTPS. Uma visão geral da autenticação HTTPS pode ser encontrada aqui: [Autenticação https](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx).  
  
Todos os dados armazenados localmente em um Windows Server (ocorre apenas se o recurso é iniciado, mas o destino está inacessível por qualquer motivo) são acessíveis apenas com privilégios administrativos no servidor local.  
  
## <a name="BKMK_Step8"></a>Trabalhando com configurações de data e hora no log de inventário de software do Windows Server 2012 R2  
  
-   Ao usar [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay para definir a hora em que o registro em log do SIL é executado, é necessário especificar uma data e hora. A data do calendário será definida e registro em log não ocorrerá até que a data seja atingida, na hora do sistema local.  
  
-   Ao usar [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)ou [Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx), "InstallDate" sempre mostrará 12:00:10:00, um valor sem sentido.  
  
-   Ao usar [Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx), "SampleDate" sempre mostrará 11:59:13h, um valor sem sentido.  A data é o dado pertinente para essas consultas de cmdlet.  
  
## <a name="BKMK_Step10"></a>Habilitando e Configurando o log de inventário de software em um disco rígido virtual montado  
O Log de Inventário de Software também dá suporte à configuração e habilitação em máquinas virtuais offline. Os usos práticos para isso se destinam a cobrir a configuração de "imagem ouro" para ampla implantação em data centers, bem como a configuração de imagens do usuário final de um local para uma implantação em nuvem.  
  
Para dar suporte a esses usos, o Log de Inventário de Software tem entradas de Registro associadas a cada opção configurável.  Esses valores de registro podem ser encontrados em \HKEY_LOCAL_MACHINE @ no__t-0SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging.  
  
|||||  
|-|-|-|-|  
|**Funcionamento**|**Nome do valor**|**Dado**|**Cmdlet correspondente (disponível somente no sistema operacional em execução)**|  
|Iniciar/parar o recurso|CollectionState|1 ou 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx), [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|Especifica o ponto de agregação de destino na rede|TargetUri|cadeia de caracteres|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TargetURI|  
|Especifica a impressão digital do certificado ou Hash do certificado usado para a autenticação SSL para o servidor Web de destino|CertificateThumbprint|cadeia de caracteres|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|Especifica a data e hora em que o recurso deve iniciar (se o valor definido for no futuro de acordo com a hora do sistema local)|CollectionTime|Default:  2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
Para modificar esses valores em um VHD offline (sistema operacional da VM não em execução), um VHD primeiro deve ser montado e, em seguida, os comandos a seguir podem ser usados para fazer alterações:  
  
-   [Reg load](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [Reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [Reg add](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [Reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
O Log de Inventário de Software verificará esses valores quando o sistema operacional for iniciado e executará de acordo.  
  
## <a name="BKMK_Step11"></a>Visão geral do uso do log de inventário de software no Windows Server 2012 R2 sem KB 3000850  
Foram feitas as seguintes alterações nas configurações padrão e de funcionalidade do Log de Inventário de Software com [KB 3000850](https://support.microsoft.com/kb/3000850):  
  
-   O intervalo padrão para coleta e encaminhamento pela rede quando o registro em log do SIL é iniciado alterado de diário para cada hora (aleatório dentro de cada hora).  
  
-   A carga de dados padrão foi reduzida para incluir somente os objetos Get-SilComputer, Get-SilComputerIdentity e Get-SilSoftware.  
  
-   O convidado para comunicação de canal do host em ambientes Hyper-V foi removido.  
  
## <a name="BKMK_Step12"></a>Usando o log de inventário de software em um ambiente Windows Server 2012 R2 Hyper-V sem KB 3000850  
  
> [!NOTE]  
> Esta funcionalidade é removida com a instalação da atualização [KB 3000850](https://support.microsoft.com/kb/3000850) .  
  
Ao usar o log de inventário de software em um host Hyper-V do Windows Server 2012 R2, é possível recuperar dados do SIL de convidados do Windows Server 2012 R2 que estão em execução localmente, se o log do SIL tiver sido iniciado nos convidados. No entanto, isso só é possível ao usar os cmdlets do PowerShell Get-SilData e Publish-SilData e só é possível com o WIndows Server 2012 R2 no host e no convidado.  O objetivo dessa funcionalidade é permitir que os administradores do data center que fornecem VMs convidadas para locatários ou outras entidades de uma grande corporação capturem dados de inventário de software no host do hipervisor e subsequentemente encaminhem todos esses dados para um agregador (ou URI de destino).  
  
Veja a seguir dois exemplos de como a saída no console do PowerShell ficaria (muito abreviada) em um host do Windows Server 2012 R2 Hyper-V executando uma VM de convidado do Windows Server 2012 R2 com o log do SIL iniciado.  Você observará que o primeiro exemplo, que usa apenas o Get-SilData, retornará todos os dados do host conforme o esperado.  Também estão incluídos todos os dados de SIL do convidado, mas em um formato recolhido.  Para expandir e exibir esses dados do convidado, basta recortar e colar o trecho usado no segundo exemplo abaixo.  Os objetos de dados SIL do convidado sempre terão o GUID da VM associado no objeto.  
  
> [!NOTE]  
> Como os dados de SIL são emitidos no console, ao usar o cmdlet Get-SilData, em fluxos de dados, os objetos nem sempre serão emitidos em uma ordem previsível.  Nos dois exemplos abaixo, o texto foi codificado por cores (azul para dados de host físico e verde para dados de convidado virtual) apenas como uma ferramenta ilustrativa para este documento.  
  
**Exemplo de saída 1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**Exemplo de saída 2** (função w/Expand-SilData)  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>Consulte também  
[Introdução ao log de inventário de software](get-started-with-software-inventory-logging.md)  
[Agregador de registro em log de inventário de software](software-inventory-logging-aggregator.md)  
[Cmdlets de log de inventário de software no Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)  
[Importar-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx)  
[Export-BinaryMiLog](https://technet.microsoft.com/library/dn262591.aspx)  
  

