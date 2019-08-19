---
title: Guia de desenvolvimento do Pacote do Server Performance Advisor
description: Guia de desenvolvimento do Pacote do Server Performance Advisor
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c27dd0602c5993fd84e6956c2f50f6e2bfec8691
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "66435477"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guia de desenvolvimento do Pacote do Server Performance Advisor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Este guia de desenvolvimento do Microsoft Server performance Advisor (SPA) fornece diretrizes para ajudar os desenvolvedores e administradores de sistema a desenvolverem pacotes do Advisor para analisar o desempenho do servidor.

Ele pressupõe que você esteja familiarizado com Logs e Alertas de Desempenho (PLA), contadores de desempenho, configurações do registro, Instrumentação de Gerenciamento do Windows (WMI), rastreamento de eventos para Windows (ETW) e Transact SQL (T-SQL).

Para obter mais informações sobre como usar o SPA, consulte [Guia do usuário do supervisor de desempenho do servidor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Visão geral do pacote do supervisor de SPA


Um pacote do Advisor normalmente é projetado para uma função de servidor específica e define o seguinte:

* Dados a serem coletados por meio de PLA, incluindo Instrumentação de Gerenciamento do Windows (WMI), contadores de desempenho, configurações do registro, arquivos e rastreamento de eventos para Windows (ETW)

* Regras, que mostram alertas e recomendações

* Dados a serem mostrados (coletados dados brutos, dados agregados ou 10 listas principais)

* Estatísticas para exibir um valor que muda ao longo do tempo

* Valores de estatísticas que podem ser tendências

Um pacote do Advisor inclui os seguintes elementos:

* **Metadados XML** (ProvisionMetadata. xml)

    * Conjunto de coletores [de dados logs e alertas de desempenho (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)

    * Layout do relatório

* **Scripts SQL**

    * Um procedimento armazenado principal

    * Objetos SQL, como procedimentos armazenados e funções definidas pelo usuário

* **Arquivo de esquema ETW** (Schema. Man) isso é opcional

### <a name="advisor-pack-workflow"></a>Fluxo de trabalho do pacote do Advisor

![fluxo de trabalho do pacote do Advisor](../media/server-performance-advisor/spa-dev-guide-workflow.png)

Neste fluxograma, os círculos verdes representam os pacotes do Advisor. Todos os outros círculos representam as fases que estão sendo executadas no processo da estrutura SPA. O SPA usa um pacote do Advisor para coletar dados, importar os dados para o banco de dado, inicializar o ambiente de execução e executar scripts SQL.

### <a name="collect-data"></a>Coletar dados

Quando um pacote do Advisor é enfileirado para um servidor específico usando SPA, o módulo de coleta de dados consulta o XML do conjunto de coletores de dados do pacote do Advisor e coleta dados do servidor de destino. Os dados brutos são armazenados em um compartilhamento de arquivos especificado pelo usuário. A coleta de dados não será interrompida até que a duração da execução de SPA designada pelo usuário seja excedida.

### <a name="import-data-into-the-database"></a>importar dados para o banco de dado

Depois que a coleta de dados for concluída, cada tipo de dados será importado para uma tabela correspondente no banco SQL Server dados. Por exemplo, as configurações do registro são importadas \#para uma tabela chamada RegistryKeys.

a importação do arquivo ETW requer um arquivo de esquema ETW para decodificar o arquivo. etl. O arquivo de esquema ETW é um arquivo XML. Ele pode ser gerado usando o tracerpt. exe, que está incluído no Windows. O arquivo de esquema ETW só é necessário quando o pacote do Advisor precisa importar dados ETW.

### <a name="switch-to-low-user-rights"></a>Alternar para direitos de usuário baixo

A estrutura SPA ajusta automaticamente os privilégios para minimizar o nível de acesso de segurança necessário. Como os pacotes do Advisor podem ser desenvolvidos ou modificados por qualquer pessoa, é possível que um pacote do Advisor contenha scripts SQL adulterados. Para atenuar o risco de segurança, qualquer script SQL para um supervisor Pack deve ser executado com direitos de usuário baixo. Ele só pode acessar objetos de banco de dados limitados, como tabelas temporárias e APIs SPA expostas como procedimentos armazenados. Os scripts SQL em um pacote do Advisor podem chamar esses procedimentos de loja para interagir com a estrutura SPA.

### <a name="initialize-execution-environment"></a>Inicializar ambiente de execução

Os pacotes do Advisor podem gerar diferentes tipos de saída, como notificações, recomendações, tabelas de fatos, estatísticas e gráficos para estatísticas. Os scripts SQL executam determinados cálculos em relação aos dados coletados. Os resultados resultantes são armazenados em tabelas temporárias por meio de APIs públicas do SPA. no estágio de inicialização, essas tabelas temporárias e outros recursos do sistema precisam ser provisionados.

### <a name="run-sql-scripts"></a>Executar scripts SQL

Há um procedimento armazenado principal, chamado pelo desenvolvedor do Advisor Pack. A estrutura SPA chama esse procedimento armazenado para iniciar o cálculo. O procedimento armazenado consome os dados coletados e comunica o resultado final à estrutura SPA.

### <a name="switch-to-administrative-rights"></a>Alternar para direitos administrativos

São necessários direitos de administrador para gerar um relatório. A geração de relatórios é totalmente controlada pelo SPA. É menos provável que seja adulterado.

### <a name="generate-a-report"></a>Gerar um relatório

Antes que o procedimento armazenado principal seja concluído para um pacote do Advisor, todos os resultados calculados, como notificações e estatísticas, não são persistidos. Durante essa fase, a estrutura SPA transfere os resultados finais de tabelas temporárias para tabelas em um formato específico. Depois que essa fase for concluída, você poderá exibir os relatórios usando o console do SPA.

## <a name="authoring-an-advisor-pack"></a>Criando um pacote do Advisor


### <a name="quick-guidelines"></a>Diretrizes rápidas

O fluxograma a seguir descreve as etapas para você desenvolver um pacote Advisor totalmente funcional. Esta seção também inclui exemplos passo a passo para explicar melhor cada etapa.

![processo de desenvolvimento do Advisor Pack](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Um pacote do Advisor geralmente é estruturado da seguinte maneira:

Pacote do Advisor

ProvisionMetadata. xml

Scripts

Main. SQL

Func. SQL

Schema. Man

Cada pacote do Advisor deve ter um arquivo chamado ProvisionMetadata. xml. Ele define informações básicas do pacote do Advisor, quais dados coletar, notificações e regras e como o relatório precisa ser armazenado e exibido. A estrutura SPA usa essas informações para gerar uma tabela temporária e, em seguida, transferir os resultados na tabela temporária para uma tabela que os usuários podem acessar.

Todos os scripts SQL de relatório devem ser salvos em uma subpasta chamada **scripts**. Para fins de manutenção, recomendamos que você salve objetos de banco de dados diferentes em diferentes SQL Server arquivos. Deve haver pelo menos um procedimento armazenado como um ponto de entrada principal.

> [!NOTE]
> O arquivo Schema. Man não é necessário, a menos que seu pacote do Advisor colete rastreamentos ETW. Esse arquivo de esquema é usado para descrever o esquema dos eventos ETW e para decodificar eventos ETW.

### <a name="defining-basic-information"></a>Definindo informações básicas

Esta seção descreve alguns dos elementos básicos que compõem um pacote do Advisor, incluindo ProvisionMetadata. xml e atributos.

Este é um cabeçalho de exemplo para o arquivo ProvisionMetadata. xml:

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Versão do pacote do Advisor

nome do atributo: **versão**

Os desenvolvedores do pacote do Advisor podem definir versões principais e secundárias para o pacote do Advisor:

* Uma versão principal geralmente envolve melhorias significativas. Os resultados gerados por uma versão antiga podem não ser compatíveis com o novo. É altamente recomendável incluir a versão principal no nome do pacote do Advisor.

* O SPA permite atualizações secundárias de versão quando há apenas pequenas alterações sem problemas de incompatibilidade de dados.

para obter mais informações sobre o controle de versão, consulte [Tópicos avançados](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Ponto de entrada de script

nome do atributo: **reportScript**

A estrutura SPA procura o nome do procedimento armazenado principal do ponto de entrada de script e o executa de maneira segura.

### <a name="other-attributes"></a>Outros atributos

Aqui estão alguns outros atributos que podem ser usados para identificar um pacote do Advisor:

* Nome de exibição: **DisplayName**

* Descrição: **Descrição**

* Autor: **autor**

* Versão do Framework: **frameworkVersion** (o padrão é 3,0)

* Versão mínima do sistema operacional: **minOSversion** (isso é reservado para extensibilidade futura)

* Notificação de eventos perdidos: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Definindo o conjunto de coletores de dados

Um conjunto de coletores de dados define os dados de desempenho que a estrutura SPA deve coletar do servidor de destino. Ele dá suporte a configurações do registro, WMI, contadores de desempenho, arquivos do servidor de destino e ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

O atributo **Duration** de **&lt;datacoletorset/&gt;** no exemplo anterior define a duração da coleta de dados (a unidade de tempo é segundos). **Duration** é um atributo obrigatório. Essa configuração controla a duração da coleta que é usada por contadores de desempenho e ETW.

### <a name="collect-registry-data"></a>Coletar dados do registro

Você pode coletar dados do registro dos seguintes hives do registro:

* RAIZ\_DE\_CLASSES HKEY

* Configuração\_atual\_de hKey

* HKey\_CURrenT\_User

* MÁQUINA\_LOCAL\_HKEY

* USUÁRIOS\_DE HKEY

Para coletar uma configuração do registro, especifique o caminho completo para o nome do valor: HKey\_local\_MachineMyKey\\myValue\\

Para coletar todas as configurações em uma chave do registro, especifique o caminho completo para a chave do registro: HKey\_local\_MachineMyKey\\\\

Para coletar todos os valores em uma chave do registro e suas subchaves (a PLA coleta recursivamente os dados do registro), use duas barras invertidas para o último delimitador de caminho: HKey\_local\_MachineMyKey\\\\\\

Para coletar informações de registro de um computador remoto, inclua o nome do computador no início do caminho do registro: HKey\_local\_MachineMyKey\\myValue\\

Por exemplo, você pode ter uma chave do registro que aparece da seguinte maneira:

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

Exemplo 1: Retornar somente o PowerSchemes ativo e seus valores:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Exemplo 2: Retorna todos os pares de chave-valor neste caminho:

> [!NOTE]
> A PLA é executada em credenciais do usuário. Algumas chaves do registro exigem credenciais administrativas. A enumeração é interrompida quando falha ao acessar qualquer uma das subchaves.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Todos os dados coletados serão importados para uma  **\#** tabela temporária chamada RegistryKeys antes que um script de relatório SQL seja executado. A tabela a seguir mostra os resultados, por exemplo 2:

KeyName | Keytypeid | Valor
------ | ----- | -------
HKEY_LOCAL_MACHINE. ..\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Fonte de energia otimizada
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

O esquema para a tabela de **#registryKeys** é o seguinte:

Nome da coluna | Tipo de dados SQL | Descrição
-------- | -------- | --------
KeyName | Nvarchar (300) NOT NULL | Nome do caminho completo da chave do registro
Keytypeid | Smallint não nulo | ID de tipo interno
Valor | Nvarchar (4000) não nulo | Todos os valores

A coluna keytypeid pode ter um dos seguintes tipos:

id | type
--- | ---
1 | String
2 | ExpandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | Link
7 | MultipleString
8 | ResourceList
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | QWORD

### <a name="collect-wmi"></a>Coletar WMI

Você pode adicionar qualquer consulta WMI. Para obter mais informações sobre como escrever consultas WMI, consulte [WQL (SQL para WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). O exemplo a seguir consulta um local do arquivo de paginação:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

A consulta no exemplo acima retorna um registro:

Legenda | Nome | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Como o WMI retorna uma tabela com colunas diferentes, quando os dados coletados são importados para um banco de dado, o SPA executa a normalização de dados e é adicionado às seguintes tabelas:

**\#A tabela WMIObjects**

SequenceID | Namespace | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage. Name =<br>C:\\pagefile. sys | 1

**\#Tabela WmiObjectsProperties**

id | consulta
--- | ---
1 | Root\Cimv2: selecione * em Win32_PageFileUsage

**\#Tabela WmiQueries**

id | consulta
--- | ---
1 | Root\Cimv2: selecione * em Win32_PageFileUsage

**\#Esquema da tabela WmiObjects**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceId | Int não nulo | Correlacione a linha e suas propriedades
Namespace | Nvarchar (200) não nulo | Namespace WMI
ClassName | Nvarchar (200) não nulo | Nome da classe WMI
RelativePath | Nvarchar (500) não nulo | Caminho relativo do WMI
WmiqueryId | Int não nulo | Correlacione a chave de #WmiQueries

**\#O esquema da tabela WmiObjectproperties**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceId | Int não nulo | Correlacione a linha e suas propriedades
Nome | Nvarchar (1000) não nulo | Nome da propriedade
Valor | Nvarchar (4000) NULL | O valor da propriedade atual

**\#Esquema de tabela WmiQueries**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
Id | Int não nulo | > ID de consulta exclusiva
consulta | Nvarchar (4000) não nulo | Cadeia de caracteres de consulta original nos metadados de provisionamento

### <a name="collect-performance-counters"></a>Coletar contadores de desempenho

Aqui está um exemplo de como coletar um contador de desempenho:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

O atributo **Interval** é uma configuração global necessária para todos os contadores de desempenho. Ele define o intervalo (a unidade de tempo é de segundos) da coleta de dados de desempenho.

No exemplo anterior, Counter \\PhysicalDisk (\*)\\AVG. Disco s/transferência serão consultados a cada segundo.

Pode haver duas instâncias: Total e 0**C:  **\_** D:** e a saída pode ser a seguinte:

estampa | CategoryName | CounterName | Valor da instância de _ total | Valor de instância de 0 C: D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Méd. Disco s/transferência | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Méd. Disco s/transferência | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Méd. Disco s/transferência | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Méd. Disco s/transferência | 0.000933297607934224 | 0.000933297607934224

Para importar os dados para o banco de dado, os dados serão normalizados em uma tabela  **\#** chamada PerformanceCounters.

CategoryDisplayName | Instância | MyDisplayName | Valor
---- | ---- | ---- | ----
PhysicalDisk | _ Total | Méd. Disco s/transferência | 0.00100008362473995
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00100008362473995
PhysicalDisk | _ Total | Méd. Disco s/transferência | 0.00280023414927187
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00280023414927187
PhysicalDisk | _ Total | Méd. Disco s/transferência | 0.00385999853230048
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00385999853230048
PhysicalDisk | _ Total | Méd. Disco s/transferência | 0.000933297607934224
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.000933297607934224

**Observação** Os nomes localizados, como **CategoryDisplayName** e MyDisplayName, variam de acordo com o idioma de exibição usado no servidor de destino. Evite usar esses campos se desejar criar um pacote Advisor neutro por idioma.

Esquema da tabela PerformanceCounters  **\#**

Nome da coluna | Tipo de dados SQL | Descrição
---- | ---- | ---- | ----
estampa | datetime2 (3) não nulo | A data e a hora coletadas no UNC
CategoryName | Nvarchar (200) não nulo | Nome da categoria
CategoryDisplayName | Nvarchar (200) não nulo | Nome da categoria localizada
Instância | Nvarchar (200) NULL | Nome da instância
CounterName | Nvarchar (200) não nulo | Nome do contador
MyDisplayName | Nvarchar (200) não nulo | Nome do contador localizado
Valor | Float NOT NULL | O valor coletado

### <a name="collect-files"></a>Coletar arquivos

Os caminhos podem ser absolutos ou relativos. O nome do arquivo pode incluir o caractere curinga\*() e o ponto de interrogação (?). Por exemplo, para coletar todos os arquivos na pasta temporária, você pode especificar c:\\Temp.\\\* O caractere curinga aplica-se aos arquivos na pasta especificada.

Se você também deseja coletar arquivos das subpastas da pasta especificada, use duas barras invertidas para o último delimitador de pasta, por exemplo, c\\:\\temp\\.\*

Aqui está um exemplo que consulta o arquivo **ApplicationHost. config** :

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Os resultados podem ser encontrados em uma tabela chamada  **\#arquivos**, por exemplo:

querypath | FullPath | Parentpath | nome_de_arquivo | Conteúdo
----- | ----- | ----- | ----- | -----
% WINDIR%\... \applicationHost.config |C:\Windows<br>\... \applicationHost.config | C:\Windows<br>\... \CONFIG | applicationHost. confi | 0x3C3F78

**\#Esquema de tabela de arquivos**

Nome da coluna | Tipo de dados SQL | Descrição
---- | ---- | ----
querypath | Nvarchar (300) NOT NULL | Instrução de consulta original
FullPath | Nvarchar (300) NOT NULL | Caminho de arquivo absoluto e nome de arquivo
Parentpath | Nvarchar (300) NOT NULL | Caminho do arquivo
nome_de_arquivo | Nvarchar (300) NOT NULL | Nome do Arquivo
Conteúdo | Varbinary (MAX) NULL | Conteúdo do arquivo em binário

### <a name="defining-rules"></a>Definindo regras

Depois que dados suficientes são coletados usando a PLA de um servidor de destino, o pacote do Advisor pode usar esses dados para validação e mostrar um resumo rápido para os administradores do sistema.

As regras fornecem uma visão geral rápida sobre o desempenho do servidor. Eles destacam problemas e fornecem recomendações. Você pode listar todas as regras que deseja validar para um pacote do Advisor. Por exemplo, se você quiser desenvolver um pacote do supervisor do sistema operacional principal, as regras possíveis podem incluir:

* Se o modo de energia da CPU é economia de energia

* Se o servidor está em um ambiente virtualizado

* Se há pressão de e/s de disco

As regras contêm os seguintes elementos:

* Limite dependente (uma parte configurável de uma regra)

* Definição de regra (alertas e recomendações)

Aqui está um exemplo de uma regra simples:

``` syntax
<advisorPack>

  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>Limite

Limite é um fator configurável que permite que os administradores do sistema decidam quando uma regra deve mostrar um status bom ou ruim. O exemplo a seguir mostra uma regra para detectar espaço livre em uma unidade do sistema e um aviso quando o espaço livre é menor que 10 GB.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

No entanto, nesse caso, o administrador do sistema tem um disco rígido menor. Ele pensa que 5 GB de espaço livre ainda pode ser uma boa condição e não quer ver um aviso. Ele pode atualizar o valor padrão de 10 para 5 por meio do console do SPA sem ter que entender como desenvolver um pacote do Advisor.

A introdução de um limite ajuda os administradores do sistema a alterar rapidamente o valor sem a necessidade de modificar o pacote do Advisor.

No exemplo, todos os atributos, exceto **Descrição** , são necessários. Você pode usar qualquer número para **valor**.

Um limite pode ser compartilhado entre as regras.

### <a name="alerts-and-recommendations"></a>Alertas e recomendações

A definição de regra não envolve nenhum cálculo de lógica. Ele define como a interface do usuário pode parecer e como o SQL Server script de relatório comunica os resultados para a interface do usuário.

Uma regra tem três partes:

* Alerta (legenda da regra)

* Recomendação (Conselho)

* Limite associado (informações opcionais sobre dependências)

Aqui está um exemplo de uma regra:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

Você pode definir tantos conselhos quanto desejar, e normalmente definiria recomendações. O **nível** de aviso pode ser **êxito** ou **aviso**.

Você pode vincular a quantos limites desejar. Você pode até mesmo vincular a um limite que é irrelevante para a regra atual. A vinculação ajuda o console SPA a gerenciar facilmente os limites.

O nome da regra e as recomendações são chaves e são exclusivas em seu escopo. Duas regras não podem ter o mesmo nome e duas recomendações dentro de uma regra podem ter o mesmo nome. Esses nomes serão muito importantes quando você escrever um relatório de script SQL. Você pode chamar o \[dbo\].\[ API setnotification\] para definir o status da regra.

### <a name="defining-ui-display-elements"></a>Definindo elementos de exibição da interface do usuário

Depois que as regras são definidas, os administradores do sistema podem ver o resumo do relatório. No entanto, geralmente os administradores do sistema estão interessados nos dados agregados e querem verificar as fontes de dados que foram usadas nas regras de desempenho.

Continuando com o exemplo anterior, o usuário sabe se há espaço livre em disco suficiente na unidade do sistema. Os usuários também podem estar interessados no tamanho real do espaço livre. Um único grupo de valor é usado para armazenar e exibir esses resultados. Vários valores únicos podem ser agrupados e mostrados em uma tabela no console do SPA. A tabela tem apenas duas colunas, nome e valor, conforme mostrado aqui.

Nome | Valor
---- | ----
Tamanho do disco livre na unidade do sistema (GB) | 100
Tamanho total do disco instalado (GB) | 500 

se um usuário quiser ver uma lista de todos os discos rígidos que estão instalados no servidor e seu tamanho de disco, poderíamos chamar um valor de lista, que tem três colunas e várias linhas, como mostrado aqui.

Disco | Tamanho do disco livre (GB) | Tamanho total (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

Em um pacote do Advisor, pode haver muitas tabelas (grupos de valor único e tabelas de valor de lista). Podemos usar uma seção para organizar e categorizar essas tabelas.

Em resumo, há três tipos de elementos de interface do usuário:

* [As](#bkmk-ui-section)

* [Grupos de valor único](#bkmk-ui-svg)

* [listar tabelas de valores](#bkmk-ui-lvt)

Aqui está um exemplo que mostra os elementos da interface do usuário:

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>As

Uma seção é puramente para o layout da interface do usuário. Ele não participa de nenhum cálculo lógico. Cada relatório único contém um conjunto de seções de nível superior que não tem uma seção pai. As seções de nível superior são apresentadas como guias no relatório. As seções podem ter subseções, com um máximo de 10 níveis. Subseções sob as seções de nível superior são apresentadas em áreas expansíveis. Uma seção pode conter várias subseções, grupos de valor único e tabelas de valor de lista. Grupos de valor único e tabelas de valor de lista são apresentados como tabelas.

Aqui está um exemplo de seção de nível superior.

``` syntax
<section name="CPU" caption="CPU"/>
```

Um nome de seção deve ser exclusivo. Ele é usado como uma chave que pode ser vinculada por outras seções, grupos de valor único e tabelas de valor de lista.

O exemplo a seguir tem um atributo, **pai**, e ele está apontando para a seção CPU. CPUFacts é um filho da seção chamada CPU. o **pai** deve se referir a um nome de seção anterior; caso contrário, isso pode resultar em um loop.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

O grupo de valor único a seguir tem um atributo, uma **seção**e pode apontar para qualquer seção, com base em seu design de interface do usuário.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Tipos de dados

Um grupo de valor único e uma tabela de valor de lista contêm tipos diferentes de dados, como String, int e float. Como esses valores são armazenados no banco de dados SQL Server, você pode definir um tipo de dado SQL para cada propriedade de dados. No entanto, a definição de um tipo de dados SQL é bastante complicada. Você precisa especificar o comprimento ou a precisão, o que pode ser propenso a alterações.

Para definir tipos de dados lógicos, você pode usar o primeiro filho de  **&lt;reportDefinition/&gt;** , que é onde você pode definir um mapeamento do tipo de dados SQL e seu tipo lógico.

O exemplo a seguir define dois tipos de dados. Uma é a **cadeia de caracteres** e a outra é **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Um nome de tipo de dados pode ser qualquer cadeia de caracteres válida. Aqui está uma lista de tipos de dados SQL permitidos:

* bigint

* binary

* parte

* º

* date

* datetime

* datetime2

* datetimeoffset

* decimal

* float

* int

* dinheiro

* nchar

* numeric

* nvarchar

* foto

* smalldatetime

* smallint

* smallmoney

* time

* tinyint

* uniqueidentifier

* varbinary

* varchar

Para obter mais informações sobre esses tipos de dados SQL, consulte [tipos de dados (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Grupos de valor único

Um grupo de valor único agrupa vários valores únicos em conjunto para apresentar em uma tabela, conforme mostrado aqui.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

No exemplo anterior, definimos um único grupo de valor. É um nó filho da seção **SystemoverviewSection**. Esse grupo tem valores únicos, que são **OsName**, **OSVersion**e **OsLocation**.

Um único valor deve ter um atributo de nome exclusivo global. Neste exemplo, o atributo global Unique Name é **Systemoverview**. O nome exclusivo será usado para gerar uma exibição correspondente para o relatório personalizado. Cada exibição contém o prefixo **VW**, como vwSystemoverview.

Embora você possa definir vários grupos de valor único, nenhum nome de dois valores único pode ser o mesmo, mesmo se eles estiverem em grupos diferentes. O nome de valor único é usado pelo relatório de script SQL para definir o valor de acordo.

Você pode definir um tipo de dados para cada valor único. A entrada permitida para o **tipo** é definida em  **&lt;DataType&gt;/** . O relatório final poderia ser assim:

**Ocorrência**

Nome | Valor
--- | ---
Sistema operacional | &lt;_um valor será definido pelo script de relatório_&gt;
Versão do SO | &lt;_um valor será definido pelo script de relatório_&gt;
Local do sistema operacional | &lt;_um valor será definido pelo script de relatório_&gt;

O atributo **Caption** do **&lt;valor/&gt;** é apresentado na primeira coluna. Os valores na coluna valor são definidos no futuro pelo relatório de script por meio \[de\]dbo\[ . Setúnicovalue\]. O atributo de **Descrição** do  **&lt;valor&gt; /** é mostrado em uma dica de ferramenta. Normalmente, a dica de ferramenta mostra aos usuários a origem dos dados. Para obter mais informações sobre dicas de ferramenta, consulte [tooltips](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>listar tabelas de valores

Definir um valor de lista é o mesmo que definir uma tabela.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

O nome do valor da lista deve ser globalmente exclusivo. Esse nome se tornará o nome de uma tabela temporária. No exemplo anterior, a tabela chamada \#NetworkAdapterInformation será criada no estágio de inicialização do ambiente de execução, que contém todas as colunas descritas. Semelhante a um único nome de valor, um nome de valor de lista também é usado como parte do nome de exibição personalizado, por exemplo, vwNetworkAdapterInformation.

@typede &lt;Column/&gt; é definido por &lt;DataType/&gt;

A interface do usuário fictícia do relatório final poderia ter a seguinte aparência:

**Informações do adaptador de rede física**

id | Nome | type | Velocidade (Mbps) | Endereço MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


O atributo **Caption** da &lt;coluna/&gt; é mostrado como um nome de coluna e o atributo de descrição &lt;de Column&gt; /é mostrado como uma dica de ferramenta para o cabeçalho de coluna correspondente. Geralmente, a dica de ferramenta mostra o usuário a fonte dos dados. Para obter mais informações, consulte [tooltips](#bkmk-tooltips).

Em alguns casos, uma tabela pode ter muitas colunas e apenas algumas linhas, portanto, alternar as colunas e linhas tornaria a aparência da tabela muito melhor. Para trocar as colunas e as linhas, você pode adicionar o seguinte atributo de estilo:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Definindo elementos de gráfico

Você pode escolher qualquer chave de estatísticas e exibir os valores em um gráfico histórico ou em um gráfico de tendências. Há dois tipos de estatísticas:

* **Estatísticas estáticas** Um único valor, que é conhecido em tempo de design. Por exemplo, o espaço livre em disco em uma unidade do sistema seria uma estatística estática.

* **Estatísticas dinâmicas** Pode ser desconhecido em tempo de design. Por exemplo, o uso médio de CPU de cada núcleo é uma estatística dinâmica, pois você não sabe quantos núcleos de CPU podem estar no sistema em tempo de design.

A chave de estatísticas tem uma limitação de que os dados devem ser compatíveis com o tipo de dados Double. Pode ser um inteiro, um decimal ou uma cadeia de caracteres que pode ser convertida em Double.

O SPA usa um único grupo de valor para dar suporte a estatísticas estáticas e uma tabela de valor de lista para dar suporte a estatísticas dinâmicas. As seções a seguir descrevem como definir a estatística estática e as chaves de estatística dinâmicas.

### <a name="static-statistics"></a>Estatísticas estáticas

Como mencionado anteriormente, uma estatística estática é um único valor. Logicamente, qualquer valor único pode ser definido como uma estatística estática. No entanto, não há sentido para exibir um único valor que não pode ser convertido em um tipo de número. Para definir uma estatística estática, você pode simplesmente adicionar o atributo **trendable** à chave de valor único correspondente, conforme mostrado abaixo:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Estatísticas dinâmicas

As chaves de estatística dinâmicas não são conhecidas no momento do design, portanto, o número de valores possíveis é desconhecido. No entanto, como os valores de lista são armazenados em várias linhas, seria fácil usar uma tabela de valor de lista para armazenar estatísticas dinâmicas.

por exemplo, se for necessário mostrar gráficos para o uso médio de CPU de núcleos diferentes, poderíamos definir uma tabela com colunas para **CpuId** e **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Outro atributo, **ColumnType**, pode ser **chave**, **valor**ou informativo. O tipo de dados da coluna de **chave** deve ser duplo ou conversível duplo. Em uma coluna de **chave** , você não pode inserir as mesmas chaves em uma tabela. **As** colunas **valor** ou informativo não têm essa limitação.

Os valores de estatísticas são armazenados em colunas de **valor** .

**As** colunas informativas são como colunas comuns em tabelas de valor de lista normal. Informativo é o tipo de coluna padrão se você não especificar um. Essas colunas não afetarão o número de chaves de estatísticas ou participarão de cálculos relacionados a estatísticas.

Continuando com o exemplo anterior, se um servidor tiver dois núcleos de CPU, o resultado na tabela poderá ser assim:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

Ao mesmo tempo, duas chaves de estatísticas são geradas pela estrutura SPA. Um é para a CPU 0 e o outro é para a CPU 1.

Como o exemplo a seguir indica que há suporte para várias colunas de **valor** com várias colunas de **chave** .

CounterName | Instância | Average | Sum
--- | :---: | :---: | :---:
% Tempo do processador | _ Total | 10 | 20
% Tempo do processador | CPU0 | 20 | 30 

Neste exemplo, você tem duas colunas de **chave** e duas colunas de **valor** . SPA gera duas chaves de estatísticas para a coluna média e outras duas chaves para a coluna Sum. As chaves de estatísticas são:

* CounterName (% Processor Time)/InstanceName (\_total)/média

* CounterName (% Processor Time)/InstanceName (CPU0)/média

* CounterName (% tempo do processador)/InstanceName\_(total)/Sum

* CounterName (% Processor Time)/InstanceName (CPU0)/Sum

CounterName e InstanceName são combinados como uma chave. A chave combinada não pode ter nenhuma duplicação.

O SPA gera muitas chaves de estatísticas. Alguns deles podem não ser interessantes para você e talvez você queira ocultá-los da interface do usuário. O SPA permite que os desenvolvedores criem um filtro para mostrar apenas as chaves de estatísticas úteis.

no exemplo anterior, os administradores do sistema só podem estar interessados em chaves em que InstanceName é \_total ou CPU1. O filtro pode ser definido da seguinte maneira:

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**trendableKeyValues/pode&gt; ser definido em qualquer coluna de chave. &lt;** Se mais de uma coluna de chave tiver esse filtro configurado, e a lógica será aplicada.

### <a name="developing-report-scripts"></a>Desenvolvendo scripts de relatório

Depois que os metadados de provisionamento são definidos, podemos começar a gravar o script de relatório, que é um procedimento armazenado T-SQL.

Há atributos **Name** e **reportScript** no cabeçalho provisionar metadados, como mostrado aqui:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

O script de relatório principal é nomeado combinando os atributos **Name** e **reportScript** . No exemplo a seguir, será \[Microsoft. ServerPerformanceAdvisor. CoreOS. v2.\[ \] ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

O atributo de **nome** será usado como um nome de esquema de banco de dados, como um namespace. Essa regra se aplica a todos os outros objetos de banco de dados que pertencem ao pacote do Advisor atual, como valor de lista e procedimentos armazenados.

Os benefícios para ter esse nome de esquema na frente dos objetos de banco de dados incluem:

* Evitando conflitos de nomenclatura para diferentes pacotes do Advisor

* Fornecendo maior segurança

No banco de dados SQL Server, o nome do esquema padrão é **dbo**. As credenciais do proprietário do banco de dados geralmente são necessárias para operar objetos de banco de dados em **dbo**. Se não criarmos um esquema para cada pacote do Advisor, é provável que dois supervisor packs definam um valor de lista com o mesmo nome. Isso deve ser irrelevante, pois você pode introduzir um nome de esquema para resolver esse problema. Além disso, o desprovisionamento de um pacote do Advisor é muito mais fácil. Como o objeto do pacote do Advisor pertence a um esquema diferente de **dbo**, isso permite que o Spa use um privilégio de usuário menor para acessá-los.

Um script de relatório normal faz o seguinte:

* Acessa dados brutos coletados

* Executa cálculos com base nos dados brutos

* Alertas e recomendações de alterações

* Prepara os dados para o modo de exibição de relatório

### <a name="access-raw-collected-data"></a>Acessar dados brutos coletados

Todos os dados coletados são importados para as seguintes tabelas correspondentes. Para obter mais informações sobre o esquema de tabela, consulte [definindo o conjunto de coletores de dados](#bkmk-definedatacollector).

* Registry

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectproperties

    * \#WmiQueries

* Contador de desempenho

    * \#PerformanceCounters

* Arquivo

    * \#Arquivos

* ETW

    * \#LostFocus

    * \#Eventoproperties

### <a name="set-rule-status"></a>Definir status da regra

O \[dbo.\]\[ A API\] setnotification define o status da regra para que você possa ver um ícone de **êxito** ou de **aviso** na interface do usuário.

* @ruleNamenvarchar (50)

* @adviceNamenvarchar (50)

As mensagens de alerta e recomendação são armazenadas no arquivo XML de provisionamento de metadados. Isso torna o script de relatório mais fácil de gerenciar.

Inicialmente, cada status de regra é N/A. Você pode usar essa API para definir um status de regra especificando um nome de aviso. O nível do nome do aviso será usado como o status da regra.

Lembre-se de que definimos a seguinte regra anteriormente:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

Supondo que o espaço livre seja menor que 2 GB, precisamos definir a regra para o nível de **aviso** . O script SQL será o seguinte:

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>Obter valor do limite

O \[dbo.\]\[ A API\] getlimite Obtém os limites:

* @keynvarchar (50)

* @valuesaída float

> [!NOTE]
> Os limites são pares de nome-valor e podem ser referenciados em qualquer regra. Os administradores do sistema podem usar o console do SPA para ajustar os limites.

 Continuando com o exemplo anterior, para um limite, a definição será a seguinte:

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

O script de relatório pode ser modificado como mostrado aqui:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)

```

### <a name="set-or-remove-the-single-value"></a>Definir ou remover o valor único

O \[dbo.\]\[ \] A API define o valor único:

* @keynvarchar (50)

* @valuevariante\_do SQL

Esse valor pode ser executado várias vezes para a mesma chave de valor único. O último valor é salvo.

O exemplo a seguir mostra alguns valores únicos definidos:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Em seguida, você pode definir o valor único, conforme mostrado aqui:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

Em casos raros, talvez você queira remover o resultado definido anteriormente usando o \[dbo.\[ \] API\] removeSingleValue.

* @keynvarchar (50)

Você pode usar o script a seguir para remover o valor definido anteriormente.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Obter informações de coleta de dados

O \[dbo.\]\[ A API\] GetDuration Obtém a duração designada pelo usuário em segundos para a coleta de dados:

* @durationsaída int

Aqui está um exemplo de script de relatório:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

O \[dbo.\]\[ A API\] getinterna Obtém o intervalo de um contador de desempenho. Isso poderá retornar NULL se o relatório atual não tiver informações do contador de desempenho.

* @intervalsaída int

Aqui está um exemplo de script de relatório:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Definir uma tabela de valor de lista

Não há API para atualizar as tabelas de valor da lista. No entanto, você pode acessar diretamente as tabelas de valor de lista. no estágio de inicialização, uma tabela temporária correspondente será criada para cada valor de lista.

O exemplo a seguir mostra uma tabela de valor de lista:

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Em seguida, você pode escrever um script SQL para inserir, atualizar ou excluir os resultados:

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (

)
```

## <a name="development-and-debugging"></a>Desenvolvimento e depuração


### <a name="writing-logs"></a>Gravando logs

Se houver informações adicionais que você deseja que se comuniquem com os administradores do sistema, você poderá gravar logs. Se houver algum log para um relatório específico, uma faixa amarela será mostrada no cabeçalho do relatório. O exemplo a seguir mostra como você pode escrever um log:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

O primeiro parâmetro é a mensagem que você deseja mostrar no log. O segundo parâmetro é o nível de log. A entrada válida para o segundo parâmetro pode serinformativa, **aviso**ou **erro**.

### <a name="debug"></a>Depurar

O console do SPA pode ser executado em dois modos, debug ou Release. O modo de liberação é o padrão e limpa todos os dados brutos coletados depois que o relatório é gerado. O modo de depuração mantém todos os dados brutos no compartilhamento de arquivos e no banco de dados, para que você possa depurar o script de relatório no futuro.

**Para depurar um script de relatório**

1.  Instale o Microsoft SQL Server Management Studio (SSMS).

2.  Depois que o SSMS for iniciado, conecte\\-se ao localhost SQLExpress. Lembre-se de que você deve usar localhost, em vez de. . Caso contrário, talvez você não consiga iniciar o depurador no SQL Server.

3.  Execute o script a seguir para habilitar o modo de depuração:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Inicie o console do SPA e execute o pacote do Advisor que você deseja depurar.

5.  Aguarde a conclusão da tarefa. Se o relatório for gerado com êxito, volte para o SSMS e procure a tarefa mais recente.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Por exemplo, a saída pode ser:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Você pode executar o script a seguir quantas vezes desejar para executar o script de relatório para a ID 12:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Observação** Você também pode pressionar F11 para entrar na instrução anterior e depurar.



Executando \[dbo.\]\[ DebugReportScript\] retorna vários conjuntos de resultados, incluindo:

1.  Microsoft SQL Server mensagens e logs do pacote do Advisor

2.  Resultados de regras

3.  Chaves e valores de estatísticas

4.  Valores únicos

5.  Todas as tabelas de valor de lista

## <a name="best-practices"></a>Práticas recomendadas

### <a name="naming-convention-and-styles"></a>Convenção de nomenclatura e estilos

|                                                                 Compartimento de Pascal                                                                 |                       Letras maiúsculas e minúsculas                        |             Maiúsculas             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>Nomes em ProvisionMetadata. xml</li><li>Procedimentos armazenados</li><li>Funções</li><li>Exibir nomes</li><li>Nomes de tabela temporária</li></ul> | <ul><li>Nomes de parâmetro</li><li>Variáveis locais</li></ul> | Usar para todas as palavras-chave reservadas do SQL |

### <a name="other-recommendations"></a>Outras recomendações

* Mova as partes mais lógicas para outros procedimentos armazenados e funções definidas pelo usuário.

* Torne seu script principal o mais breve possível para fins de manutenção.

* Use o nome completo do objeto SQL.

* Trate seu código SQL como diferenciar maiúsculas de minúsculas.

* Adicione **SET NOCOUNT no** início de cada procedimento armazenado.

* Considere o uso de tabelas temporárias para transferir enormes quantidades de dados.

* Considere usar **set transação\_Abort on** para encerrar o processo se ocorrer um erro.

* Sempre inclua o número de versão principal no nome de exibição do Advisor Pack.

## <a href="" id="bkmk-advancedtopics"></a>Tópicos avançados

### <a name="run-multiple-advisor-packs-simultaneously"></a>Executar vários pacotes do Advisor simultaneamente

O SPA dá suporte à execução de vários pacotes do Advisor ao mesmo tempo. Isso é especialmente útil quando você deseja examinar Serviços de Informações da Internet (IIS) e o desempenho do sistema operacional principal ao mesmo tempo. Muitos coletores de dados que são usados pelo pacote Advisor do IIS também podem ser usados pelo pacote do supervisor do sistema operacional principal. Quando dois ou mais pacotes do Advisor estiverem em execução no mesmo computador de destino, o SPA não coletará os mesmos dados duas vezes.

O exemplo a seguir mostra o fluxo de trabalho para executar dois pacotes do Advisor.

![executando vários pacotes do Advisor](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

O conjunto de coletores de dados de fusão é apenas para coletar as fontes de dados do contador de desempenho e do ETW. As seguintes regras de mesclagem se aplicam:

1. O SPA leva a maior duração como a nova duração.

2. Quando houver conflitos de mesclagem, as seguintes regras serão seguidas:

   1. Tome o menor intervalo como o novo intervalo.

   2. Pegue o super conjunto dos contadores de desempenho. Por exemplo, com **processo (\*)\\% tempo de processador** e **processo\*(\\)\\\*, Process\*(\\)\\** * retorna mais dados, de modo que **process\\(\*)% Processor Time** e **process\\(\*)\\** * é removido do conjunto de coletores de dados mesclados.

### <a name="collect-dynamic-data"></a>Coletar dados dinâmicos

SPA precisa de um conjunto de coletores de dados definido em tempo de design. Nem sempre é possível saber quais dados são necessários para a geração de relatórios porque os dados dinâmicos e o caminho de consulta não são conhecidos até que seus dados dependentes estejam disponíveis.

por exemplo, se você quiser listar todos os nomes amigáveis dos adaptadores de rede, primeiro você deve consultar o WMI para enumerar todos os adaptadores de rede. Cada objeto WMI retornado tem um caminho de chave do registro, no qual ele armazena o nome amigável. O caminho da chave do registro é desconhecido no tempo de design. Nesse caso, precisamos de suporte a dados dinâmicos.

Para enumerar todos os adaptadores de rede, você pode usar a consulta WMI a seguir usando o Windows PowerShell:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Ele retorna uma lista de objetos de adaptador de rede. Cada objeto tem uma propriedade chamada **PNPDeviceID**, que mantém um caminho de chave do registro relativo. Aqui está um exemplo de saída da consulta anterior:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

Para localizar o **valor amigável** , abra o editor do registro e navegue até configuração do registro combinando **\_hKey\\local\\\_Machine\\System CurrentControlSet enum\\** com cada linha no exemplo anterior. , por exemplo: **HKEY\_localMachine\_System\\CurrentControlSetenum\\ raizIPHTTPS0000\\.\\\\\\\***

Para converter as etapas anteriores em metadados de provisionamento de SPA, adicione o script no exemplo de código a seguir:

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

Neste exemplo, você primeiro adiciona uma consulta WMI em managementpaths e define o nome da chave **adaptador**. Em seguida, adicione uma chave do registro e consulte **adaptador** usando a sintaxe, **$ (adaptador. PNPDeviceID)** .

A tabela a seguir define se um coletor de dados no SPA dá suporte a dados dinâmicos e se ele pode ser referenciado por outros coletores de dados:

Tipo de dados | Suporte a dados dinâmicos | Pode ser referenciado
--- | :---: | :---:
chave do registro | Sim | Sim
WMI | Sim | Sim
Arquivo | Sim | Não
Contador de desempenho | Não | Não
ETW | Não | Não

Para um coletor de dados WMI, cada objeto WMI tem muitos atributos anexados. Qualquer tipo de objeto WMI sempre tem três atributos: \_\_NAMESPACE, \_ \_classe e \_ RELpath\_.

Para definir um coletor de dados que é referenciado por outros coletores de dados, atribua o atributo **Name** com uma chave exclusiva no ProvisionMetadata. xml. Essa chave é usada pelos coletores de dados dependentes para gerar dados dinâmicos.

Aqui está um exemplo de chave do registro:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

E um exemplo para o WMI:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Para definir um coletor de dados dependente, a seguinte sintaxe é usada: $ ( *{Name}* . *{Attribute}* ).

*{Name}* e *{Attribute}* são espaços reservados.

Quando o Spa coleta dados de um servidor de destino, ele substitui dinamicamente o padrão $\*(\*.) pelos dados coletados reais de seu coletor de dados de referência (chave do registro/WMI), por exemplo:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Observação** O SPA dá suporte a uma profundidade ilimitada de referência, mas esteja ciente da sobrecarga de desempenho se você tiver muitos níveis. Verifique se não há nenhuma referência circular ou autoreferência que não tenha suporte.

### <a name="versioning-limitations"></a>limitações de controle de versão

O SPA oferece suporte a redefinição e atualizações de versão secundárias. Esses processos usam o mesmo algoritmo. O processo é atualizar todos os objetos de banco de dados e as configurações de limite, mas manter os existentes. Isso pode ser atualizado para uma versão superior ou fazer downgrade para uma versão inferior. Selecione o pacote do Advisor e clique em **Redefinir** na caixa de diálogo **configurar pacotes do Advisor** no Spa para redefinir ou aplicar as atualizações.

Esse recurso é principalmente para atualizações secundárias. Você não pode alterar drasticamente os elementos de exibição da interface do usuário. Se você quiser fazer alterações significativas, precisará criar um pacote do Advisor diferente. Você deve incluir a versão principal no nome do pacote do Advisor.

As limitações das modificações secundárias da versão são que você **não pode** fazer o seguinte:

* Alterar o nome do esquema

* Alterar o tipo de dados de qualquer grupo de valor único ou as colunas de uma tabela de valor de lista

* Adicionar ou remover limites

* Adicionar ou remover regras

* Adicionar ou remover conselhos

* Adicionar ou remover valores únicos

* Adicionar ou remover valores da lista

* Adicionar ou remover uma coluna de valores de lista

### <a href="" id="bkmk-tooltips"></a>Dicas

Quase todos os atributos de **Descrição** serão mostrados como uma dica de ferramenta no console do Spa.

para uma tabela de valor de lista, uma dica de ferramenta baseada em linha pode ser obtida adicionando o seguinte atributo:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

O atributo **descriptionColumn** refere-se ao nome da coluna. Neste exemplo, a coluna Descrição não é mostrada como uma coluna física. No entanto, ele aparece como uma dica de ferramenta quando você passa o mouse sobre cada linha da primeira coluna.

Recomendamos que a dica de ferramenta mostre a fonte de dados para o usuário. Estes são os formatos para mostrar as fontes de dados:

Fonte de dados | Formatar | Exemplo
--- | --- | ---
WMI | WMI: &lt;campo&gt;WMIClass/&lt;&gt; | ESSES Win32_OperatingSystem/legenda
Contador de desempenho | PerfCounter &lt;InstanceName&gt;de CategoryName/&lt;&gt; | PerfCounter Processo/% tempo do processador
Registry | Registro: &lt;registerKey&gt; | Registry HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
Arquivo de configuração | ConfigFile &lt;FilePath&gt;;\[ XPath &lt;XPath&gt;\]<br>**Observação**<br>O XPath é opcional e é válido somente quando o arquivo é um arquivo XML. | ConfigFile: WINDIR%\\system32\\inetsrv\config\\ApplicationHost. config<br>XPath: Configuration&frasl;System. WebServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW &lt;Provider/&gt;(palavras-chave) | ETW Rastreamento de kernel do Windows (processo, rede)

### <a name="table-collation"></a>Agrupamento de tabelas

Quando um pacote do Advisor se torna mais complicado, você pode criar suas próprias tabelas variáveis ou tabelas temporárias para armazenar resultados intermediários no script de relatório.

A agrupamento de colunas de cadeia de caracteres pode ser problemática porque o agrupamento de tabelas que você cria pode ser diferente daquele criado pela estrutura SPA. Se você correlacionar duas colunas de cadeia de caracteres em tabelas diferentes, poderá ver um erro de agrupamento. Para evitar esse problema, você sempre deve definir a cadeia de caracteres para um agrupamento de colunas como **\_SQL\_latino1\_\_geral\_CP1 CI como** quando você define uma tabela.

Aqui, como definir uma tabela de variáveis:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Coletar ETW

Aqui, como definir o ETW em um arquivo ProvisionMetadata. xml:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Os seguintes atributos de provedor estão disponíveis para uso no ETW de coleta:

Atributo | type | Descrição
--- | --- | ---
guid | GUID | GUID do provedor
sessão | cadeia de caracteres | Nome da sessão ETW (opcional, necessário apenas para eventos de kernel)
keywordsany | Hex | Qualquer palavra-chave (opcional, sem prefixo 0x)
keywordsAll | Hex | Todas as palavras-chave (opcional)
properties | Hex | Propriedades (opcional)
level | Hex | Nível (opcional)
bufferSize | int | Tamanho do buffer (opcional)
flushtime | int | Tempo de liberação (opcional)
maxBuffer | int | Buffer máximo (opcional)
minBuffer | int | Buffer mínimo (opcional)

Há duas tabelas de saída, conforme mostrado aqui.

**\#Esquema de tabela de eventos**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceID | Int não nulo | ID da sequência de correlação
EventTypeId | Int não nulo | ID do tipo de evento (consulte [dbo]. [ EventTypes])
ProcessId | BigInt não nulo | ID do Processo
ThreadId | BigInt não nulo | ID do thread
estampa | datetime2 não nulo | estampa
Kerneltime | BigInt não nulo | Tempo do kernel
Usertime | BigInt não nulo | Hora do usuário

**\#Esquema de tabela eventproperties**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceID | Int não nulo | ID da sequência de correlação
Nome | Nvarchar(100) | Nome da propriedade
Valor | Nvarchar (4000) | Valor

### <a name="etw-schema"></a>Esquema ETW

Um esquema ETW pode ser gerado executando o tracerpt. exe em relação ao arquivo. etl. Um arquivo Schema. Man é gerado. Como o formato do arquivo. etl é dependente do computador, o script a seguir funciona apenas nas seguintes situações:

1.  Execute o script no computador em que o arquivo. etl correspondente é coletado.

2.  Ou execute o script em um computador com o mesmo sistema operacional e componentes instalados.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glossário


Os seguintes termos são usados neste documento:

**Pacote do Advisor**

Um pacote do Advisor é uma coleção de metadados e scripts SQL que processam os logs de desempenho que são coletados do servidor de destino. O pacote de Supervisor e gera relatórios dos dados de log de desempenho. Os metadados no pacote do Supervisor definem os dados a serem coletados do servidor de destino para as medidas de desempenho. Os metadados também definem o conjunto de regras, os limites e o formato de relatório. Geralmente, um pacote advisor foi criado especificamente para uma função de servidor único, por exemplo, Internet Information Services (IIS).

**Console do SPA**

O console do SPA refere-se a SpaConsole. exe, que é a parte central do supervisor de desempenho do servidor. SPA não precisa executar no servidor de destino que você está testando. O console do SPA contém todas as interfaces de usuário para SPA, configuração de projeto para executar a análise e exibir relatórios. Por design, o SPA é um aplicativo de duas camadas. O console do SPA contém a camada de interface do Usuário e a parte da camada de lógica de negócios. O console do SPA agenda e processa as solicitações de análise de desempenho.

**Estrutura SPA**

O SPA contém duas partes principais, a estrutura e os pacotes do Advisor. A estrutura SPA fornece todas as interfaces do usuário, processamento de log de desempenho, configuração, tratamento de erros e APIs de banco de dados e procedimentos de gerenciamento.

**Projeto SPA**

Um projeto SPA é um banco de dados que contém todas as informações sobre os servidores de destino, os pacotes do Advisor e os relatórios de análise de desempenho gerados nos servidores de destino para os pacotes do Advisor. Você pode comparar e exibir gráficos de histórico e tendências dentro do mesmo projeto do SPA. O usuário pode criar mais de um projeto. Os projetos do SPA são independentes um do outro e nenhum dado compartilhada entre projetos.

**Servidor de destino**

O servidor de destino é o computador físico ou a máquina virtual que executa o Windows Server com determinadas funções de servidor, como o IIS.

**Sessão de análise de dados**

Uma sessão de análise de dados é uma análise de desempenho em um servidor de destino específico. Uma sessão de análise de dados pode incluir vários pacotes de Supervisor. Os conjuntos de Coletores de dados desses pacotes de Supervisor são mesclados em um conjunto de Coletores de dados único. Todos os logs de desempenho para uma sessão de análise de dados são coletados durante o mesmo período de tempo. Analisar relatórios que são gerados por pacotes de supervisor em execução na mesma sessão de análise de dados pode ajudar os usuários a entender a situação geral de desempenho e identificar as causas de problemas de desempenho.

**Rastreamento de eventos para Windows**

O ETW ( [rastreamento de eventos](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) para Windows) é um sistema de rastreamento escalonável de alto desempenho e baixa sobrecarga que é fornecido nos sistemas operacionais Windows. Ele fornece perfis e depuração de recursos, que podem ser usados para solucionar uma variedade de cenários. SPA usa eventos ETW como uma fonte de dados para gerar os relatórios de desempenho. Para obter informações gerais sobre o ETW, consulte [melhorar a depuração e ajuste de desempenho com o ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Consulta WMI**

Instrumentação de Gerenciamento do Windows (WMI) é a infraestrutura para dados de gerenciamento e operações em sistemas operacionais Windows. Você pode escrever scripts WMI ou aplicativos para automatizar tarefas administrativas em computadores remotos. WMI também fornece dados de gerenciamento para outras partes do sistema operacional e produtos. SPA usa informações de classe WMI e pontos de dados como fontes para geração de relatórios de desempenho.

**Contadores de desempenho**

Os contadores de desempenho são usados para fornecer informações sobre o desempenho do sistema operacional ou de um aplicativo, serviço ou driver. Os dados do contador de desempenho podem ajudar a determinar gargalos do sistema e ajustar o desempenho do sistema e do aplicativo. O sistema operacional, rede e dispositivos fornecem dados de contador que um aplicativo pode utilizar para fornecer aos usuários uma exibição gráfica de como o sistema está sendo executado. SPA usa informações do contador de desempenho e pontos de dados como fontes para gerar relatórios de desempenho.

**Logs e Alertas de Desempenho**

Logs e Alertas de Desempenho (PLA) é um serviço interno no sistema operacional Windows. Ele foi projetado para coletar logs e rastreamentos de desempenho e também gera alertas de desempenho quando determinados gatilhos são atendidos. CCP pode ser usado para coletar contadores de desempenho, rastreamento de eventos para Windows (ETW), consultas WMI, chaves do registro e configuração de arquivos. CCP também oferece suporte a coleta de dados remotos por meio de chamadas de procedimento remoto (RPC). O usuário define um conjunto de Coletores de dados, que inclui informações sobre os dados a serem coletados, a frequência da coleta de dados, duração da coleta de dados, filtros e um local para salvar os arquivos de resultados. SPA usa PLA para coletar todos os dados de desempenho de servidores de destino.

**Relatório único**

O relatório único é o relatório SPA gerado com base em uma sessão de análise de dados para um pacote do Advisor em um único servidor de destino. Ela pode conter várias seções de dados e notificações.

**Relatório lado a lado**

Um relatório lado a lado é um relatório SPA que compara dois relatórios únicos para o mesmo pacote do Advisor. Os dois relatórios podem ser gerados a partir de servidores de destino diferentes ou execuções de análise de desempenho separados no mesmo servidor de destino. O relatório side-by-side cria a capacidade de comparar dois relatórios para ajudar os usuários a identificar comportamentos anormais ou configurações em um dos relatórios. Um relatório de lado a lado contém várias seções de dados e notificações. Em cada seção, dados de ambos os relatórios são listada lado a lado.

**Gráfico de tendência**

Um gráfico de tendência é o relatório SPA usado para investigar padrões repetitivos de problemas de desempenho. Muitos problemas de desempenho repetitivas são causados por alterações de carga do servidor agendado do servidor ou de computadores cliente, que podem ocorrer diariamente ou semanalmente. SPA fornece um gráfico de tendências de 24 horas e um gráfico de tendência de 7 dias para identificar esses problemas.

O usuário pode escolher uma ou mais séries de dados por vez, que é um valor numérico no relatório único, como **média de utilização de CPU total**. mais especificamente, um valor numérico é um valor escalar de um único servidor gerado por um único AP em uma determinada instância de tempo. SPA agrupa esses valores em 24 grupos, um para cada hora do dia (sete para um relatório de 7 dias, um para cada dia da semana). SPA calcula a média, mínimo, máximo e desvios padrão para cada grupo.

**Gráfico histórico**

Um gráfico histórico é o relatório SPA usado para mostrar alterações em determinados valores numéricos dentro de relatórios únicos para um determinado servidor e par do Advisor Pack ao longo do tempo. O usuário pode escolher várias séries de dados e mostrá-los juntos no gráfico de histórico para entender a correlação entre a série de dados diferente.

**Série de dados**

Uma série de dados são dados numéricos que são coletados da mesma fonte de dados ao longo de um período de tempo. A mesma fonte significa que os dados tem que vêm do mesmo servidor de destino, como o comprimento da fila de média de solicitação do IIS em um servidor.

**Regras**

As regras são combinações de lógica, limites e descrições. Eles representam um possível problema de desempenho. Cada pacote advisor contém várias regras. Cada regra é acionada por um processo de geração de relatórios. Uma regra se aplica a lógica e os limites para os dados em um único relatório. Se os critérios forem atendidos, uma notificação de aviso será gerada. Se não, a notificação é definida como o **OK** estado. Se a regra não se aplica, a notificação é definida como não aplicável (**NA**) estado.

**Notificações**

Uma notificação é a informação que uma regra exibe para os usuários. Ele inclui o status da regra (**OK**, **na**ou **aviso**), o nome da regra e possíveis recomendações para resolver os problemas de desempenho.
