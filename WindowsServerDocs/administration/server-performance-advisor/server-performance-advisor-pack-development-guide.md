---
title: Guia de desenvolvimento do Pacote do Server Performance Advisor
description: Guia de desenvolvimento do Pacote do Server Performance Advisor
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c647e8a335aac924067d92dcb41ab4d17e0cceef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884857"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guia de desenvolvimento do Pacote do Server Performance Advisor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Este guia de desenvolvimento para o Microsoft Server Performance Advisor (SPA) fornece diretrizes para ajudar os desenvolvedores e administradores de sistema desenvolver pacotes advisor para analisar o desempenho do servidor.

Ele pressupõe que você esteja familiarizado com os Logs de desempenho e alertas (CCP), contadores de desempenho, as configurações do registro, Windows Management Instrumentation (WMI), Event Tracing for Windows (ETW) e Transact SQL (T-SQL).

Para obter mais informações sobre como usar o SPA, consulte [guia do usuário das Supervisor de desempenho do servidor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Visão geral do SPA Advisor Pack


Normalmente, um pacote de supervisor foi projetado para uma função de servidor específico, e define o seguinte:

* Dados a serem coletados por meio do CCP, incluindo o Windows Management Instrumentation (WMI), contadores de desempenho, as configurações do registro, arquivos e Event Tracing for Windows (ETW)

* Regras, que mostra os alertas e recomendações

* Dados a serem mostrados (dados brutos coletados, dados agregados ou listas dos 10 principais)

* Estatísticas para exibir um valor que é alterado ao longo do tempo

* Valores de estatísticas que podem ser direcionados

Um pacote de Supervisor inclui os seguintes elementos:

* **Metadados XML** (Provisionmetadata)

    * [Logs de desempenho e alertas (CCP)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx) conjunto de Coletores de dados

    * layout de relatório

* **Scripts SQL**

    * Um procedimento armazenado principal

    * Objetos SQL, como procedimentos armazenados e funções definidas pelo usuário

* **Arquivo de esquema ETW** (man), isso é opcional

### <a name="advisor-pack-workflow"></a>Fluxo de trabalho de pacote de Supervisor

![fluxo de trabalho de pacote de Supervisor](../media/server-performance-advisor/spa-dev-guide-workflow.png)

Este fluxograma, círculos verdes representam pacotes de Supervisor. Todos os outros círculos representam as fases que estão em execução no processo de estrutura do SPA. SPA usa um pacote de Supervisor para coletar dados, importar os dados para o banco de dados, inicializar o ambiente de execução e executar scripts SQL.

### <a name="collect-data"></a>Coletar dados

Quando um pacote de Supervisor é colocado na fila para um determinado servidor usando o SPA, o módulo de coleta de dados consulta o conjunto de Coletores de dados XML do pacote de Supervisor e coleta de dados do servidor de destino. Os dados brutos são armazenados em um compartilhamento de arquivo especificado pelo usuário. A coleta de dados não será interrompida até que o SPA designada pelo usuário a duração da execução for excedido.

### <a name="import-data-into-the-database"></a>Importar dados para o banco de dados

Após a coleta de dados, cada tipo de dados é importado para uma tabela correspondente no banco de dados do SQL Server. Por exemplo, as configurações do registro são importadas para uma tabela chamada \#chaves do registro.

importando ETW arquivo requer um arquivo de esquema do ETW para decodificar o arquivo. ETL. O arquivo de esquema ETW é um arquivo XML. Ele pode ser gerado usando tracerpt.exe, que está incluído no Windows. O arquivo de esquema ETW só é necessária quando o pacote de Supervisor precisa importar dados ETW.

### <a name="switch-to-low-user-rights"></a>Alterne para direitos de usuário baixa

A estrutura do SPA ajusta automaticamente os privilégios para minimizar o nível de acesso de segurança necessárias. Porque os pacotes do advisor podem ser desenvolvidos ou modificados por qualquer pessoa, é possível que um pacote de Supervisor conter os scripts SQL violados. Para atenuar o risco de segurança, qualquer script SQL para um pacote de Supervisor deve ser executado com direitos de usuário baixa. Ele só pode acessar objetos de banco de dados limitado, como tabelas temporárias e as APIs do SPA expostos como procedimentos armazenados. Os scripts SQL em um pacote advisor podem chamar os procedimentos para interagir com a estrutura SPA de armazenamento.

### <a name="initialize-execution-environment"></a>Inicializar o ambiente de execução

Pacotes de Supervisor podem gerar diferentes tipos de saída, como notificações, recomendações, tabelas de fatos, estatísticas e gráficos para estatísticas. Os scripts SQL executam determinados cálculos contra os dados coletados. Os resultados produzidos são armazenados em tabelas temporárias por meio de APIs públicas do SPA. no estágio de inicialização, essas tabelas temporárias e outros recursos do sistema precisam ser provisionados.

### <a name="run-sql-scripts"></a>Executar scripts SQL

Há um procedimento armazenado principal, que é chamado pelo desenvolvedor do pacote de Supervisor. A estrutura SPA chama esse procedimento armazenado para iniciar o cálculo. O procedimento armazenado consome os dados coletados e se comunica o resultado final para a estrutura do SPA.

### <a name="switch-to-administrative-rights"></a>Alternar a direitos administrativos

Direitos de administrador são necessários para gerar um relatório. Geração de relatórios é totalmente controlada pelo SPA. É menor probabilidade de ser violado.

### <a name="generate-a-report"></a>Gerar um relatório

Antes do procedimento armazenado principal é concluído para um pacote de Supervisor, todos os resultados calculados, como notificações e estatísticas, não são persistentes. Durante essa fase, a estrutura do SPA transfere os resultados finais de tabelas temporárias para tabelas em um formato específico. Após a conclusão desta fase, você pode exibir os relatórios usando o console do SPA.

## <a name="authoring-an-advisor-pack"></a>Criação de um pacote de Supervisor


### <a name="quick-guidelines"></a>Diretrizes rápidas

O fluxograma a seguir descreve as etapas para você desenvolver um pacote de Supervisor totalmente funcional. Esta seção também inclui exemplos passo a passo para explicar melhor a cada etapa.

![processo de desenvolvimento de pacote de Supervisor](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Geralmente, um pacote de Supervisor é estruturado da seguinte maneira:

Pacote de Supervisor

ProvisionMetadata.xml

Scripts

main.sql

func.sql

Schema.man

Cada pacote de Supervisor deve ter um arquivo chamado Provisionmetadata. Ele define as informações de pacote advisor básico, quais dados coletar, notificações e regras e como o relatório precisa ser armazenado e exibido. A estrutura do SPA usa essas informações para gerar uma tabela temporária e, em seguida, transfira os resultados na tabela temporária para uma tabela que os usuários podem acessar.

Todos os relatam de scripts SQL devem ser salvo em uma subpasta chamada **Scripts**. Para fins de manutenção, é recomendável que você salve os objetos de banco de dados diferentes em diferentes arquivos do SQL Server. Deve haver pelo menos um procedimento armazenado como um ponto de entrada principal.

> [!NOTE]
> O arquivo man não é necessário, a menos que seu pacote de Supervisor coleta rastreamentos ETW. Esse arquivo de esquema é usado para descrever o esquema de eventos ETW e para decodificar eventos ETW.

### <a name="defining-basic-information"></a>Definindo informações básicas

Esta seção descreve alguns dos elementos básicos que compõem um pacote de Supervisor, incluindo Provisionmetadata e atributos.

Este é um cabeçalho de exemplo para o arquivo Provisionmetadata:

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

### <a name="advisor-pack-version"></a>Versão do pacote de Supervisor

nome do atributo: **versão**

Os desenvolvedores do pacote Advisor podem definir as versões principais e secundárias para o pacote de Supervisor:

* Uma versão principal geralmente envolve melhorias significativas. Os resultados que são gerados por uma versão antiga não podem ser compatíveis com a nova. É altamente recomendável incluindo a versão principal no nome do pacote advisor.

* SPA permite atualizações de versão secundária quando há apenas pequenas alterações sem problemas de incompatibilidade de dados.

Para obter mais informações sobre controle de versão, consulte [tópicos avançados](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Ponto de entrada de script

nome do atributo: **reportScript**

A estrutura do SPA procura o nome do procedimento armazenado principal do ponto de entrada de script e executa-o em uma maneira segura.

### <a name="other-attributes"></a>Outros atributos

Aqui estão alguns outros atributos que podem ser usados para identificar um pacote de Supervisor:

* Nome de exibição: **displayName**

* Descrição: **descrição**

* Autor: **autor**

* Versão do Framework: **frameworkversion** (o padrão é 3.0)

* Versão mínima do sistema operacional: **minOSversion** (Isso é reservado para extensibilidade futura)

* Perdeu a notificação de eventos: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Definindo o conjunto de Coletores de dados

Um conjunto de Coletores de dados define os dados de desempenho que a estrutura do SPA deve coletar do servidor de destino. Ele dá suporte a contadores de desempenho de configurações, WMI, registro, arquivos do servidor de destino e ETW.

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

O **duração** atributo do **&lt;dataCollectorSet /&gt;** no exemplo anterior define a duração da coleta de dados (a unidade de tempo é segundos). **duração** é um atributo obrigatório. Essa configuração controla a duração da coleção que é usada pelo ETW e contadores de desempenho.

### <a name="collect-registry-data"></a>Coletar dados de registro

Você pode coletar dados de registro de seções do registro a seguir:

* HKEY\_CLASSES\_ROOT

* HKEY\_CURrenT\_CONFIG

* HKEY\_CURrenT\_USER

* HKEY\_LOCAL\_MACHINE

* HKEY\_USERS

Para coletar uma configuração do registro, especifique o caminho completo para o nome do valor: HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Para coletar todas as configurações em uma chave do registro, especifique o caminho completo para a chave do registro: HKEY\_LOCAL\_MACHINE\\MyKey\\

Para coletar todos os valores em uma chave do registro e suas subchaves (recursivamente CCP coleta os dados do registro), use duas barras invertidas para o último delimitador de caminho: HKEY\_LOCAL\_MACHINE\\MyKey\\\\

Para coletar informações de registro de um computador remoto, inclua o nome do computador no início do caminho do registro: HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

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

Exemplo 1: Somente o PowerSchemes Active Directory e seus valores de retorno:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Exemplo 2: Retorna todos os pares de valor da chave nesse caminho:

> [!NOTE]
> CCP é executado sob as credenciais do usuário. Algumas chaves do registro exigem credenciais administrativas. A enumeração para quando ele não pode acessar qualquer uma das chaves subpropriedades.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Todos os dados coletados serão importados para uma tabela temporária chamada  **\#registryKeys** antes de um relatório do SQL script é executado. A tabela a seguir mostra os resultados por exemplo 2:

KeyName | KeytypeId | Valor
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Fonte de energia otimizada
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

O esquema para o **#registryKeys** tabela é um como segue:

Nome da coluna | Tipo de dados SQL | Descrição
-------- | -------- | --------
KeyName | Nvarchar(300) NOT NULL | Nome de caminho completo da chave do registro
KeytypeId | smallint não NULL | Id de tipo interno
Valor | Nvarchar(4000) NOT NULL | Todos os valores

O **KeytypeID** coluna pode ter um dos seguintes tipos:
ID | Tipo
--- | ---
1 | String
2 | expandString
3 | Binário
4 | DWord
5 | DWordBigEndian
6 | Link
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | QWORD

### <a name="collect-wmi"></a>Coletar o WMI

Você pode adicionar qualquer consulta WMI. Para obter mais informações sobre como escrever consultas do WMI, consulte [WQL (SQL para WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). O exemplo a seguir consulta a um local de arquivo de página:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

A consulta no exemplo acima retorna um registro:

Legenda | Nome | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Como WMI retorna uma tabela com colunas diferentes, quando os dados coletados são importados para um banco de dados, o SPA executa a normalização de dados e é adicionado para as tabelas a seguir:

**\#Tabela WMIObjects**

SequenceID | Namespace | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\pagefile.sys | 1

**\#Tabela WmiObjectsProperties**

ID | consulta
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Tabela WmiQueries**

ID | consulta
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Esquema da tabela WmiObjects**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceId | Int NOT NULL | Correlacionar a linha e suas propriedades
Namespace | Nvarchar(200) NOT NULL | Namespace do WMI
ClassName | Nvarchar(200) NOT NULL | Nome da classe WMI
RelativePath | Nvarchar(500) NOT NULL | Caminho relativo do WMI
WmiqueryId | Int NOT NULL | Correlacionar a chave do #WmiQueries

**\#Esquema da tabela WmiObjectProperties**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceId | Int NOT NULL | Correlacionar a linha e suas propriedades
Nome | Nvarchar(1000) NOT NULL | Nome da propriedade
Valor | Nvarchar(4000) NULL | O valor da propriedade atual

**\#Esquema da tabela WmiQueries**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
Id | Int NOT NULL | > ID da consulta exclusivo
consulta | Nvarchar(4000) NOT NULL | Cadeia de caracteres de consulta original nos metadados de provisionamento

### <a name="collect-performance-counters"></a>Coletar contadores de desempenho

Veja um exemplo de como coletar um contador de desempenho:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

O **intervalo** atributo é uma configuração global necessária para todos os contadores de desempenho. Ele define o intervalo (a unidade de tempo é segundos) de coleta de dados de desempenho.

No exemplo anterior, contador \\PhysicalDisk (\*)\\média Disco s/transferência será consultada por segundo.

Pode haver duas instâncias: **\_Total** e **0 c: Unidade d:**, e a saída pode ser da seguinte maneira:

carimbo de hora | CategoryName | CounterName | Valor da instância do total | Valor da instância de c: 0 D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Méd. Disco s/transferência | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Méd. Disco s/transferência | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Méd. Disco s/transferência | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Méd. Disco s/transferência | 0.000933297607934224 | 0.000933297607934224

Para importar os dados para o banco de dados, os dados serão normalizados em uma tabela chamada  **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Valor
---- | ---- | ---- | ----
PhysicalDisk | _Total | Méd. Disco s/transferência | 0.00100008362473995
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00100008362473995
PhysicalDisk | _Total | Méd. Disco s/transferência | 0.00280023414927187
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00280023414927187
PhysicalDisk | _Total | Méd. Disco s/transferência | 0.00385999853230048
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.00385999853230048
PhysicalDisk | _Total | Méd. Disco s/transferência | 0.000933297607934224
PhysicalDisk | 0 C: D: | Méd. Disco s/transferência | 0.000933297607934224

**Observação** o localizado nomes, como **CategoryDisplayName** e **CounterdisplayName**, variam de acordo com o idioma de exibição usado no servidor de destino. Evite usar esses campos se desejar criar um pacote de Supervisor com neutralidade de idioma.

**\#PerformanceCounters** esquema da tabela

Nome da coluna | Tipo de dados SQL | Descrição
---- | ---- | ---- | ----
carimbo de hora | datetime2(3) NOT NULL | Coletados de data e hora em UNC
CategoryName | Nvarchar(200) NOT NULL | Nome da categoria
CategoryDisplayName | Nvarchar(200) NOT NULL | Nome da categoria localizada
InstanceName | Nvarchar(200) NULL | Nome da instância
CounterName | Nvarchar(200) NOT NULL | Nome do contador
CounterdisplayName | Nvarchar(200) NOT NULL | Nome do contador localizada
Valor | Float não nulo | O valor coletado

### <a name="collect-files"></a>Coletar arquivos

Os caminhos podem ser absolutos ou relativos. O nome do arquivo pode incluir o caractere curinga (\*) e o ponto de interrogação (?). Por exemplo, para coletar todos os arquivos na pasta temporária, você pode especificar c:\\temp\\\*. O caractere curinga se aplica a arquivos na pasta especificada.

Se você quiser coletar também os arquivos de subpastas da pasta especificada, use duas barras invertidas para o último delimitador de pasta, por exemplo, c:\\temp\\\\\*.

Veja um exemplo que consulta o **applicationHost. config** arquivo:

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Os resultados podem ser encontrados em uma tabela chamada  **\#arquivos**, por exemplo:

querypath | FullPath | Parentpath | nome_de_arquivo | Conteúdo
----- | ----- | ----- | ----- | -----
%windir%\...\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#Esquema da tabela de arquivos**

Nome da coluna | Tipo de dados SQL | Descrição
---- | ---- | ----
querypath | Nvarchar(300) NOT NULL | Instrução de consulta original
FullPath | Nvarchar(300) NOT NULL | Caminho de arquivo absoluto e o nome do arquivo
Parentpath | Nvarchar(300) NOT NULL | Caminho do arquivo
nome_de_arquivo | Nvarchar(300) NOT NULL | Nome do Arquivo
Conteúdo | Varbinary (max) NULL | Conteúdo do arquivo no binário

### <a name="defining-rules"></a>Definição de regras

Após a coleta de dados suficientes usando CCP de um servidor de destino, o pacote de Supervisor pode usar esses dados para validação e mostrar um rápido resumo para os administradores do sistema.

As regras oferecem uma visão geral sobre o servidor de desempenho de s. Eles destacam problemas e fornecem recomendações. Você pode listar todas as regras que você deseja validar um pacote de Supervisor. Por exemplo, se você quiser desenvolver um pacote de Supervisor do sistema operacional principal, as regras de possíveis poderia incluir:

* Se o modo de energia da CPU é economia de energia

* Se o servidor está em um ambiente virtualizado

* Se houver pressão de e/s de disco

Regras de contenham os seguintes elementos:

* Limite de dependente (configurável fizer parte de uma regra)

* Definição de regra (alertas e recomendações)

Veja um exemplo de uma regra simples:

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

Limite é um fator configurável que permite que os administradores de sistema decidir quando uma regra deve mostrar uma boa ou um status incorreto. O exemplo a seguir mostra uma regra para detectar o espaço livre em uma unidade de sistema e um aviso quando o espaço livre é menor que 10 GB.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

No entanto, nesse caso, o administrador do sistema tem um disco rígido menores. Ele acha que 5 GB de espaço livre pode ainda ser boas condições, e ele não quer ver um aviso. Ele pode atualizar o valor padrão de 10 a 5 por meio do console do SPA sem precisar entender como desenvolver um pacote de Supervisor.

Apresentando um limite ajuda os administradores de sistema alterar rapidamente o valor sem ter que modificar o pacote de Supervisor.

No exemplo, todos os atributos, exceto **descrição** são necessários. Você pode usar qualquer número de **valor**.

Um limite pode ser compartilhado entre as regras.

### <a name="alerts-and-recommendations"></a>Alertas e recomendações

A definição da regra não envolve nenhum cálculo de lógica. Ele define a aparência de interface do usuário e como relatar um script do SQL Server se comunica os resultados na interface do usuário.

Uma regra tem três partes:

* Alerta (legenda de regra)

* Recomendação (aviso)

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

Você pode definir tanta conselhos de como deseja e, normalmente, você deve definir as recomendações. O **nível** conselho pode ser **sucesso** ou **aviso**.

Você pode vincular a limites de quantos desejar. Você ainda pode vincular a um limite que é irrelevante para a regra atual. Vinculando ajuda o console do SPA gerenciar facilmente os limites.

O nome da regra e as recomendações são chaves, e eles sejam exclusivos em seu escopo. Não há duas regras podem ter o mesmo nome, e nenhuma dois recomendação dentro de uma regra pode ter o mesmo nome. Esses nomes serão muito importante quando você escreve um relatório de script SQL. Você pode chamar o \[dbo\].\[ SetNotification\] API para definir o status da regra.

### <a name="defining-ui-display-elements"></a>Definindo os elementos de exibição da interface do usuário

Depois que as regras são definidas, os administradores do sistema podem ver o relatório de resumo. No entanto, muitas vezes os administradores do sistema estiver interessados nos dados agregados, e desejam verificar as fontes de dados que foram usadas nas regras de desempenho.

Continuando com o exemplo anterior, o usuário saiba se há espaço em disco suficiente na unidade do sistema. Os usuários também podem estar interessados no tamanho real do espaço livre. Um grupo de valor único é usado para armazenar e exibir esses resultados. Vários valores únicos podem ser agrupados e mostrados em uma tabela no console do SPA. A tabela tem apenas duas colunas, nome e valor, conforme mostrado aqui.

Nome | Valor
---- | ----
Tamanho livre em disco na unidade do sistema (GB) | 100
Tamanho total do disco instalado (GB) | 500 

Se um usuário quiser ver uma lista de todos os discos rígidos que estão instalados no servidor e seu tamanho de disco, chamamos um valor de lista, que tem três colunas e várias linhas, conforme mostrado aqui.

Disco | Tamanho livre em disco (GB) | Tamanho total (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

Um pacote de Supervisor, pode haver muitas tabelas (grupos de valor único e tabelas de valor de lista). Podemos usar uma seção para organizar e categorizar essas tabelas.

Em resumo, há três tipos de elementos de interface do usuário:

* [Seções](#bkmk-ui-section)

* [Grupos de valor único](#bkmk-ui-svg)

* [Listar tabelas de valor](#bkmk-ui-lvt)

Aqui um exemplo que mostra os elementos de interface do usuário:

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

### <a href="" id="bkmk-ui-section"></a>Seções

Uma seção é apenas para o layout da interface do usuário. Ele não participa em nenhum cálculo de lógico. Cada relatório único contém um conjunto de seções de nível superior que não têm uma seção pai. As seções de nível superior são apresentadas como guias no relatório. Seções podem ter subseções, com no máximo 10 níveis. Subseções sob as seções de nível superior são apresentadas em áreas expansíveis. Uma seção pode conter várias subseções, grupos de valor único e tabelas de valor da lista. Grupos de valor único e listar tabelas de valor são apresentadas como tabelas.

Aqui está um exemplo da seção de nível superior.

``` syntax
<section name="CPU" caption="CPU"/>
```

Um nome de seção deve ser exclusivo. Ele é usado como uma chave que pode ser vinculada a outras seções, grupos de valor único e tabelas de valor da lista.

O exemplo a seguir tem um atributo **pai**, e ele está apontando para a seção da CPU. CPUFacts é um filho da seção denominada da CPU. **pai** deve se referir a um nome da seção anterior; caso contrário, ele pode resultar em um loop.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

O seguinte grupo de valor único tem um atributo **seção**, e ele pode apontar para qualquer seção, com base em seu design de interface do usuário.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Tipos de dados

Um grupo de valor único e uma lista de tabela de valor contêm diferentes tipos de dados, como string, int e float. Como esses valores são armazenados no banco de dados do SQL Server, você pode definir um tipo de dados SQL para cada propriedade de dados. No entanto, a definição de um tipo de dados SQL é bastante complicado. Você precisa especificar o comprimento ou precisão, que pode estar sujeito a alteração.

Para definir tipos de dados lógicos, você pode usar o primeiro filho do  **&lt;reportDefinition /&gt;**, que é onde você pode definir um mapeamento de tipo de dados SQL e o tipo de lógico.

O exemplo a seguir define dois tipos de dados. Uma é **cadeia de caracteres** e a outra é **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Um nome de tipo de dados pode ser qualquer cadeia de caracteres válida. Aqui está uma lista de tipos de dados SQL permitidos:

* bigint

* binary

* Bit

* char

* date

* datetime

* datetime2

* datetimeoffset

* decimal

* flutuante

* int

* dinheiro

* nchar

* numeric

* nvarchar

* real

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

No exemplo anterior, definimos um grupo de valor único. Ele é um nó filho da seção **SystemoverviewSection**. Esse grupo tem valores únicos, que são **OsName**, **Osversion**, e **OsLocation**.

Um único valor deve ter um atributo de nome global exclusivo. Neste exemplo, é o atributo de nome global exclusivo **Systemoverview**. O nome exclusivo será ser usado para gerar uma exibição correspondente para o relatório personalizado. Cada modo de exibição contém o prefixo **vw**, como vwSystemoverview.

Embora você possa definir vários grupos de valor único, nenhum dois nomes de valor único podem ser o mesmo, mesmo se eles estão em grupos diferentes. O nome de valor único é usado pelo relatório de script SQL para definir o valor correspondente.

Você pode definir um tipo de dados para cada valor único. A entrada permitida **tipo** é definida no  **&lt;datatype /&gt;**. O relatório final poderia ter esta aparência:

**fatos**

Nome | Valor
--- | ---
Sistema operacional | &lt;_um valor será definido pelo script de relatório_&gt;
Versão do SO | &lt;_um valor será definido pelo script de relatório_&gt;
Local do sistema operacional | &lt;_um valor será definido pelo script de relatório_&gt;

O **legenda** atributo do **&lt;valor /&gt;** é apresentada na primeira coluna. Valores na coluna de valor são definidos no futuro pelo relatório por meio de script \[dbo\].\[ SetSingleValue\]. O **descrição** atributo do **&lt;valor /&gt;** é mostrado em uma dica de ferramenta. Normalmente, a dica de ferramenta mostra aos usuários a fonte de dados. Para obter mais informações sobre dicas de ferramenta, consulte [dicas de ferramenta](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>Listar tabelas de valor

Definir um valor de lista é como o mesmo que definir uma tabela.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

O nome do valor de lista deve ser globalmente exclusivo. Esse nome se tornará o nome de uma tabela temporária. No exemplo anterior, a tabela chamada \#NetworkAdapterInformation será criado no estágio de inicialização do ambiente de execução, que contém todas as colunas que são descritas. Semelhante a um nome de valor único, um nome de valor de lista também é usado como parte do nome de exibição personalizado, por exemplo, vwNetworkAdapterInformation.

@type dos &lt;coluna /&gt; é definido pela &lt;datatype /&gt;

A interface do usuário fictício do relatório final poderia ser semelhante ao seguinte:

**Informações do adaptador de rede física**

ID | Nome | Tipo | Velocidade (Mbps) | Endereço MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


O **legenda** atributo do &lt;coluna /&gt; é mostrado como um nome de coluna e o **descrição** atributo do &lt;coluna /&gt; é mostrado como uma dica de ferramenta para o cabeçalho de coluna correspondente. Normalmente, a dica de ferramenta mostra o usuário com a fonte de dados. Para obter mais informações, consulte [dicas de ferramenta](#bkmk-tooltips).

Em alguns casos, uma tabela pode ter muitas colunas e apenas algumas linhas, portanto, trocar as colunas e linhas tornaria a tabela a melhores aparência. Para trocar as colunas e linhas, você pode adicionar o atributo de estilo a seguir:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Definição de elementos gráficos

Você pode escolher qualquer chave de estatísticas e exibir os valores em um gráfico de histórico ou um gráfico de tendência. Há dois tipos de estatísticas:

* **Estatísticas estáticas** um único valor, que é conhecido em tempo de design. Por exemplo, o espaço livre em disco em uma unidade de sistema seria uma estatística estática.

* **Estatísticas dinâmicas** pode ser desconhecido em tempo de design. Por exemplo, o uso de CPU médio de cada núcleo é uma estatística dinâmica porque você não souber o número de núcleos da CPU poderia no sistema em tempo de design.

A chave de estatísticas tem uma limitação que os dados devem ser compatíveis com o tipo de dados double. Ele pode ser um número inteiro, decimal ou uma cadeia de caracteres que pode ser convertida em duplo.

SPA usa um grupo de valor único para dar suporte a estatísticas estáticas e uma lista de tabela de valor para dar suporte a estatísticas dinâmicas. As seções a seguir descrevem como definir estatística estática e as chaves de estatística dinâmico.

### <a name="static-statistics"></a>Estatísticas estáticas

Conforme mencionado anteriormente, uma estatística estática é um único valor. Logicamente, qualquer valor único pode ser definido como uma estatística estática. No entanto, não faz sentido para exibir um valor único que não pode ser convertido em um tipo numérico. Para definir uma estatística estática, você pode simplesmente adicionar o atributo **trendable** para o único valor chave correspondente como mostrado abaixo:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Estatísticas dinâmicas

Chaves de estatística dinâmico não são conhecidas em tempo de design, portanto, o número de valores possíveis é desconhecido. No entanto, como valores de lista são armazenados em várias linhas, seria fácil usar uma tabela de valor de lista para armazenar as estatísticas dinâmicas.

Por exemplo, se for necessário mostrar gráficos de uso da CPU média de núcleos diferentes, podemos pode definir uma tabela com colunas para **CpuId** e **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Outro atributo, **columntype**, pode ser **chave**, **valor**, ou **informativo**. O tipo de dados a **chave** coluna deve ser double ou duplo que possa ser convertido. Em um **chave** coluna, você não pode inserir as mesmas chaves em uma tabela. **Valor** ou **informativo** colunas não têm essa limitação.

Os valores de estatísticas são armazenados em **valor** colunas.

**Informativo** colunas são como qualquer outra coluna nas tabelas de valor de lista normal. **Informativo** é o tipo de coluna padrão, se você não especificar uma. Essas colunas não afeta o número de chaves de estatísticas ou participe de cálculos de estatísticas.

Continuando com o exemplo anterior, se um servidor tiver dois núcleos de CPU, o resultado na tabela se parece com esta:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

Ao mesmo tempo, as chaves de duas estatísticas são geradas pela estrutura do SPA. Uma é para CPU 0 e o outro é para a CPU 1.

Como o exemplo a seguir indica os vários **valor** colunas com vários **chave** colunas é suportado.

CounterName | InstanceName | Média | Sum
--- | :---: | :---: | :---:
% Tempo do processador | _Total | 10 | 20
% Tempo do processador | CPU0 | 20 | 30 

Neste exemplo, você tem dois **chave** colunas e duas **valor** colunas. SPA gera duas chaves de estatísticas para a coluna média e outro duas chaves para a coluna de soma. As chaves de estatísticas são:

* CounterName (% tempo do processador) / InstanceName (\_Total) / média

* CounterName (% tempo do processador) / InstanceName (CPU0) / média

* CounterName (% tempo do processador) / InstanceName (\_Total) / soma

* CounterName (% tempo do processador) / InstanceName (CPU0) / soma

CounterName e InstanceName são combinados como uma chave. A chave combinada não pode ter nenhuma duplicação.

SPA gera muitas chaves de estatísticas. Alguns deles não podem ser interessante para você, e talvez você queira ocultá-los na interface de usuário. SPA permite aos desenvolvedores criar um filtro para mostrar somente as chaves de estatísticas úteis.

no exemplo anterior, os administradores de sistema somente podem ser interessante ler as chaves no qual o InstanceName é \_Total ou CPU1. O filtro pode ser definido da seguinte maneira:

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

**&lt;trendableKeyValues /&gt;**  podem ser definidos em qualquer coluna de chave. Se mais de uma coluna de chave tem um filtro configurado e lógica será aplicada.

### <a name="developing-report-scripts"></a>Desenvolver scripts de relatório

Depois que os metadados de provisionamento é definido, podemos começar a escrever o script de relatório, que é um procedimento armazenado T-SQL.

Há **nome** e **reportScript** atributos no cabeçalho de metadados de provisionamento, conforme mostrado aqui:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

O script do relatório principal é chamado pela combinação de **nome** e **reportScript** atributos. No exemplo a seguir, será \[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\].\[ ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

O **nome** atributo será usado como um nome de esquema de banco de dados, como um namespace. Essa regra se aplica a todos os outros objetos de banco de dados que pertencem ao pacote do Supervisor atual, como procedimentos armazenados e o valor da lista.

Benefícios de ter esse nome de esquema na frente de objetos de banco de dados:

* Evitando conflitos de nomenclatura para pacotes de supervisor diferente

* Fornecer maior segurança

No banco de dados do SQL Server, o nome do esquema padrão é **dbo**. Credenciais do proprietário do banco de dados geralmente são necessários para operar os objetos de banco de dados sob **dbo**. Se não podemos criar um esquema para cada pacote advisor, é provável que os dois pacotes de Supervisor definirá um valor de lista com o mesmo nome. Isso deve ser irrelevante porque você pode introduzir um nome de esquema para resolver esse problema. Além disso, o desprovisionamento de um pacote de Supervisor é muito mais fácil. Porque o objeto de pacote de supervisor pertence a um esquema diferente de **dbo**, isso permite que o SPA usar um menor privilégio de usuário para acessá-los.

Um script de relatório normal faz o seguinte:

* Acessa os dados coletados brutos

* Executa cálculos com base em dados brutos

* Recomendações e alertas de alterações

* Prepara os dados para o modo de exibição de relatório

### <a name="access-raw-collected-data"></a>Dados coletados brutos acesso

Todos os dados coletados são importados para as tabelas a seguir correspondentes. Para obter mais informações sobre o esquema da tabela, consulte [definindo o conjunto de Coletores de dados](#bkmk-definedatacollector).

* Registro

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Contador de desempenho

    * \#PerformanceCounters

* Arquivo

    * \#Arquivos

* ETW

    * \#Eventos

    * \#EventProperties

### <a name="set-rule-status"></a>Definir status da regra

O \[dbo\].\[ SetNotification\] API define o status da regra, para que você possa ver um **sucesso** ou **aviso** ícone na interface do usuário.

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

As mensagens de alerta e recomendação são armazenadas no arquivo XML de metadados de provisionar. Isso torna mais fácil de gerenciar o script de relatório.

Inicialmente, o status de cada regra é n/d. Você pode usar essa API para definir um status de regra, especificando um nome de conselho. O nível do nome do conselho será usado como o status da regra.

Lembre-se de que definimos a seguinte regra anteriormente:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

Supondo que o espaço livre é menor que 2 GB, é necessário definir a regra o **aviso** nível. O script SQL será da seguinte maneira:

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

### <a name="get-threshold-value"></a>Obter o valor de limite

O \[dbo\].\[ GetThreshold\] API obtém os limites:

* @key nvarchar(50)

* @value saída de float

> [!NOTE]
> Os limites são pares nome-valor, e eles podem ser referenciados em todas as regras. Os administradores de sistema podem usar o console do SPA para ajustar os limites.

 Continuando com o exemplo anterior, para um limite, a definição será da seguinte maneira:

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

O script de relatório pode ser modificado conforme mostrado aqui:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)
 
```

### <a name="set-or-remove-the-single-value"></a>Definir ou remover o único valor

O \[dbo\].\[ SetSingleValue\] API define o valor único:

* @key nvarchar(50)

* @value sql\_variant

Esse valor pode executar várias vezes para a mesma chave de valor único. O último valor é salvo.

O exemplo a seguir mostra que alguns definidos valores únicos:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Em seguida, você pode definir o valor único como mostrado aqui:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

Em casos raros, você talvez queira remover o resultado definido anteriormente usando o \[dbo\].\[ removeSingleValue\] API.

* @key nvarchar(50)

Você pode usar o script a seguir para remover o definido anteriormente valor.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Obter informações sobre coleta de dados

O \[dbo\].\[ GetDuration\] API obtém o usuário designado a duração em segundos para a coleta de dados:

* @duration saída de int

Aqui um exemplo de s relatório script:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

O \[dbo\].\[ GetInternal\] API obtém o intervalo de um contador de desempenho. Ele poderia retornar NULL se o relatório atual não tem informações do contador de desempenho.

* @interval saída de int

Aqui um exemplo de s relatório script:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Definir uma tabela de valor de lista

Há uma API para a atualização de tabelas de valor da lista. No entanto, você pode acessar diretamente as tabelas de valor da lista. no estágio de inicialização, uma tabela temporária correspondente será criada para cada valor de lista.

O exemplo a seguir mostra uma lista de tabela de valor:

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


### <a name="writing-logs"></a>Gravar logs

Se houver quaisquer informações adicionais que você deseja se comunicar com os administradores de sistema, você pode gravar logs. Se não houver nenhum log para um relatório específico, uma faixa amarela será mostrada no cabeçalho do relatório. O exemplo a seguir mostra como você pode escrever um log:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

O primeiro parâmetro é a mensagem que você deseja mostrar no log. O segundo parâmetro é o nível de log. A entrada válida para o segundo parâmetro pode ser **informativo**, **aviso**, ou **erro**.

### <a name="debug"></a>Depuração

O console do SPA pode ser executado em dois modos, depuração ou versão. Modo de versão é o padrão, e ele limpa todos os dados brutos coletados depois que o relatório é gerado. O modo de depuração mantém todos os dados brutos no compartilhamento de arquivos e do banco de dados, para que você possa depurar o script de relatório no futuro.

**Para depurar um script de relatório**

1.  Instale o Microsoft SQL Server Management Studio (SSMS).

2.  Depois que o SSMS é iniciado, conecte-se ao localhost\\SQLExpress. Lembre-se de que você deve usar o localhost, em vez de. . Caso contrário, você não poderá iniciar o depurador no SQL Server.

3.  Execute o script a seguir para habilitar o modo de depuração:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Inicie o console do SPA e execute o pacote de supervisor que você deseja depurar.

5.  Aguarde até que a tarefa seja concluída. Se o relatório é gerado com êxito, alterne para o SSMS e procure a tarefa mais recente.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Por exemplo, a saída pode ser:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Você pode executar o script a seguir quantas vezes você deseja executar o script de relatório pela Id de 12:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Observação** você também pode pressionar F11 para entrar na instrução anterior e a depuração.

     

Executando \[dbo\].\[ DebugReportScript\] retorna vários conjuntos de resultados, incluindo:

1.  Mensagens do Microsoft SQL Server e os logs de pacote de Supervisor

2.  Resultados de regras

3.  Valores e chaves de estatísticas

4.  Valores únicos

5.  Todas as tabelas de valor de lista

## <a name="best-practices"></a>Práticas recomendadas

### <a name="naming-convention-and-styles"></a>Estilos e a convenção de nomenclatura

Pascal casing | Concatenação com maiusculas e minúsculas | letras maiusculas
--- | ---- | ---
<ul><li>Nomes em Provisionmetadata</li><li>Procedimentos armazenados</li><li>Funções</li><li>Nomes de exibição</li><li>Nomes de tabelas temporárias</li></ul> | <ul><li>Nomes de parâmetro</li><li>Variáveis locais</li></ul> | Use para todas as palavras-chave SQL reservada

### <a name="other-recommendations"></a>Outras recomendações

* Mova as partes mais lógicas para outros procedimentos armazenados e funções definidas pelo usuário.

* Verifique o script principal o mais breve possível para fins de manutenção.

* Use o nome completo do objeto SQL.

* Trate seu código SQL de maiusculas e minúsculas.

* Adicione **SET NOCOUNT ON** para o início de cada procedimento armazenado.

* Considere o uso de tabelas temporárias para transferir grande quantidade de dados.

* Considere o uso de **definir XACT\_anular ON** para finalizar o processo se ocorrer um erro.

* Sempre inclua o número de versão principal no nome de exibição do pacote advisor.

## <a href="" id="bkmk-advancedtopics"></a>Tópicos avançados

### <a name="run-multiple-advisor-packs-simultaneously"></a>Executar vários pacotes de Supervisor simultaneamente

SPA oferece suporte à execução de vários pacotes de supervisor ao mesmo tempo. Isso é especialmente útil quando você deseja examinar os serviços de informações da Internet (IIS) e o desempenho do sistema operacional principal ao mesmo tempo. Vários coletores de dados que são usados pelo pacote de Supervisor do IIS também podem ser usados pelo pacote de Supervisor do SO principal. Quando dois ou mais pacotes do advisor estão em execução no mesmo computador de destino, SPA não coletar os mesmos dados duas vezes.

O exemplo a seguir mostra o fluxo de trabalho para executar dois pacotes de Supervisor.

![executando vários pacotes de Supervisor](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

O conjunto de Coletores de dados de mesclagem é somente para coletar o contador de desempenho e fontes de dados ETW. As seguintes regras de mesclagem se aplicam:

1.  SPA usa a maior duração como a nova duração.

2.  Onde há conflitos de mesclagem, as seguintes regras são seguidas:

    1.  Veja o intervalo mínimo de como o novo intervalo.

    2.  Usar o conjunto de super dos contadores de desempenho. Por exemplo, com **processo (\*)\\% tempo do processador** e **processo (\*)\\\*,\\processo (\*)\\ \***  retorna mais dados, portanto **processo (\*)\\% tempo do processador** e **processo (\*)\\ \***  é removido do conjunto de Coletores de dados mesclados.

### <a name="collect-dynamic-data"></a>Coletar dados dinâmicos

Necessidades SPA definido de um coletor de dados definidas no tempo de design. Nem sempre é possível saber quais dados são necessários para geração de relatórios porque os dados dinâmicos e o caminho da consulta não são conhecidos até que seus dados de dependentes estejam disponíveis.

Por exemplo, se você deseja listar todos os nomes amigáveis dos adaptadores de rede, você deve primeiro consultar WMI para enumerar todos os adaptadores de rede. Cada retornada objeto WMI tem um caminho de chave do registro, em que ele armazena o nome amigável. O caminho da chave do registro é desconhecido no tempo de design. Nesse caso, precisamos de dados dinâmicos do suporte.

Para enumerar todos os adaptadores de rede, você pode usar a seguinte consulta WMI usando o Windows PowerShell:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Retorna uma lista de objetos de adaptador de rede. Cada objeto tem uma propriedade chamada **PNPDeviceID**, que mantém um caminho de chave do registro relativo. Veja um exemplo de saída da consulta anterior:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000
 
```

Para localizar o **FriendlyName** valor, abra o editor do registro e navegue até configuração do registro combinando **HKEY\_LOCAL\_máquina\\sistema\\ CurrentControlSet\\Enum\\**  com cada linha no exemplo anterior. , por exemplo: **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Enum\\ ROOT\\\*IPHTTPS\\0000**.

Para converter as etapas anteriores em metadados de provisão do SPA, adicione o script no exemplo de código a seguir:

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

Neste exemplo, você primeiro adicionar uma consulta WMI em managementpaths e definir o nome da chave **adaptador de rede**. Em seguida, adicione uma chave do registro e consultem **adaptador de rede** usando a sintaxe **$(NetworkAdapter.PNPDeviceID)**.

A tabela a seguir define se um coletor de dados em SPA oferece suporte a dados dinâmicos e se ele pode ser referenciado por outros coletores de dados:

Tipo de dados | Suporte a dados dinâmicos | Pode ser referenciada
--- | :---: | :---:
Chave do registro | Sim | Sim
WMI | Sim | Sim
Arquivo | Sim | Não
Contador de desempenho | Não | Não
ETW | Não | Não

Para um coletor de dados do WMI, cada objeto WMI tem muitos atributos anexados. Qualquer tipo de objeto WMI sempre tem três atributos: \_\_NAMESPACE, \_ \_classe, e \_ \_RELpath.

Para definir um coletor de dados que é referenciado por outros coletores de dados, atribuir a **nome** atributo com uma chave exclusiva a Provisionmetadata. Essa chave é usada por Coletores de dados dependentes para gerar dados dinâmicos.

Veja um exemplo da chave do registro:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

E um exemplo para o WMI:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Para definir um coletor de dados dependentes, a sintaxe a seguir é usada: $(*{name}*.*{atributo}*).

*{name}*  e *{atributo}* são espaços reservados.

Ao SPA coleta dados de um servidor de destino, ele substitui dinamicamente o $ padrão (\*.\*) com os reais coletados dados de seu coletor de dados de referência (chave de registro / WMI), por exemplo:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Observação** SPA oferece suporte a uma profundidade ilimitada de referência, mas lembre-se da sobrecarga de desempenho se você tiver muitos níveis. Verifique se não há nenhuma referência circular ou referência que não é suportado.

### <a name="versioning-limitations"></a>limitações de controle de versão

SPA oferece suporte a atualizações de versão primária e secundária de ser redefinido. Esses processos usam o mesmo algoritmo. O processo é atualizar todos os objetos de banco de dados e configurações de limite, mas manter os dados existentes. Isso pode ser atualizar para uma versão mais recente ou fazer downgrade para versão inferior. Selecione o pacote de Supervisor e, em seguida, clique em **redefina** na **configurar pacotes Advisor** caixa de diálogo no SPA para redefinir ou aplicar ou as atualizações.

Esse recurso é principalmente para atualizações secundárias. Você não pode alterar consideravelmente os elementos de exibição da interface do usuário. Se você quiser fazer alterações significativas, você precisará criar um pacote de supervisor diferente. Você deve incluir a versão principal em nome do pacote advisor.

As limitações de modificações de versão secundária são que você **não é possível** fazer qualquer um dos seguintes:

* Alterar o nome do esquema

* Alterar o tipo de dados de qualquer grupo de valor único ou as colunas de uma tabela de valor de lista

* Adicionar ou remover os limites

* Adicionar ou remover regras

* Adicionar ou remover conselhos

* Adicionar ou remover valores únicos

* Adicionar ou remover valores da lista

* Adicionar ou remover uma coluna de valores de lista

### <a href="" id="bkmk-tooltips"></a>Dicas de ferramenta

Quase todos os **descrição** atributos serão exibidos como uma dica de ferramenta no console do SPA.

para uma tabela de valor de lista, uma dica de ferramenta de linha com base pode ser feita adicionando o seguinte atributo:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

O **descriptionColumn** atributo refere-se ao nome da coluna. Neste exemplo, a coluna de descrição não mostra como uma coluna de física. No entanto, ele mostra como uma dica de ferramenta quando você passa o mouse sobre cada linha da primeira coluna.

É recomendável que a dica de ferramenta mostram a fonte de dados para o usuário. Estes são os formatos para mostrar as fontes de dados:

Fonte de dados | Formatar | Exemplo
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;/&lt;campo&gt; | WMI: Win32_OperatingSystem/Caption
Contador de desempenho | PerfCounter: &lt;CategoryName&gt;/&lt;InstanceName&gt; | PerfCounter: Tempo do processador Process/%
Registro | Registro: &lt;registerKey&gt; | registry: HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
Arquivo de configuração | ConfigFile: &lt;Filepath&gt;\[; Xpath: &lt;Xpath&gt;\]<br>**Observação**<br>XPath é opcional e é válido somente quando o arquivo é um arquivo xml. | ConfigFile:  windir%\\System32\\inetsrv\config\\applicationHost.config<br>Xpath: configuration&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW: &lt;Provedor /&gt;(palavras-chave) | ETW: Rastreamento de Kernel do Windows (processo, net)

### <a name="table-collation"></a>Agrupamento de tabela

Quando um pacote de Supervisor se torna mais complicado, você pode criar suas próprias tabelas variáveis ou tabelas temporárias para armazenar resultados intermediários no script de relatório.

Colunas de cadeia de caracteres de agrupamento pode ser problemático porque o agrupamento de tabelas que você criar pode ser diferente daquele que é criado pela estrutura do SPA. Se você se correlaciona duas colunas de cadeia de caracteres em tabelas diferentes, você poderá ver um erro de agrupamento. Para evitar esse problema, você deve sempre definir a cadeia de caracteres para um agrupamento de coluna como **SQL\_Latin1\_gerais\_CP1\_CI\_AS** quando você definir uma tabela.

Veja como definir uma tabela de variável:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Coletar ETW

Veja como definir ETW em um arquivo Provisionmetadata:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Os seguintes atributos de provedor estão disponíveis para uso para coletar ETW:

Atributo | Tipo | Descrição
--- | --- | ---
guid | GUID | GUID do provedor
sessão | cadeia de caracteres | Nome de sessão ETW (opcional, somente o necessário para eventos de kernel)
keywordsany | Hex | Quaisquer palavras-chave (opcional, nenhum prefixo 0x)
keywordsAll | Hex | Todas as palavras-chave (opcional)
properties | Hex | Propriedades (opcionais)
level | Hex | Nível (opcional)
bufferSize | Int | Tamanho do buffer (opcional)
flushtime | Int | Tempo de liberação (opcional)
maxBuffer | Int | Máximo do buffer (opcional)
minBuffer | Int | Buffer mínimo (opcional)

Há duas tabelas de saída, conforme mostrado aqui.

**\#Esquema da tabela de eventos**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceID | Int NOT NULL | ID de correlação da sequência
EventtypeId | Int NOT NULL | ID do tipo de evento (consulte a [dbo]. [ EventTypes])
ProcessId | BigInt NOT NULL | ID do Processo
ThreadId | BigInt NOT NULL | ID do thread
carimbo de hora | datetime2 NOT NULL | carimbo de hora
Kerneltime | BigInt NOT NULL | Tempo de kernel
Usertime | BigInt NOT NULL | Tempo do usuário

**\#Esquema da tabela EventProperties**

Nome da coluna | Tipo de dados SQL | Descrição
--- | --- | ---
SequenceID | Int NOT NULL | ID de correlação da sequência
Nome | Nvarchar(100) | Nome da propriedade
Valor | Nvarchar(4000) | Valor

### <a name="etw-schema"></a>Esquema ETW

Um esquema ETW pode ser gerado pela execução tracerpt.exe no arquivo. ETL. Um arquivo man é gerado. Como o formato do arquivo. ETL é dependente do computador, o script a seguir só funciona nas seguintes situações:

1.  Execute o script no computador onde o arquivo. etl correspondente é coletado.

2.  Ou execute o script em um computador com o mesmo sistema operacional e os componentes instalados.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glossário


Os seguintes termos são usados neste documento:

**Pacote de Supervisor**

Um pacote de Supervisor é uma coleção de metadados e os scripts SQL que processam os logs de desempenho são coletados do servidor de destino. O pacote de Supervisor e gera relatórios dos dados de log de desempenho. Os metadados no pacote do Supervisor definem os dados a serem coletados do servidor de destino para as medidas de desempenho. Os metadados também definem o conjunto de regras, os limites e o formato de relatório. Geralmente, um pacote advisor foi criado especificamente para uma função de servidor único, por exemplo, Internet Information Services (IIS).

**Console do SPA**

O console do SPA se refere a SpaConsole.exe, que é a parte central do Server Performance Advisor. SPA não precisa executar no servidor de destino que você está testando. O console do SPA contém todas as interfaces de usuário para SPA, configuração de projeto para executar a análise e exibir relatórios. Por design, o SPA é um aplicativo de duas camadas. O console do SPA contém a camada de interface do Usuário e a parte da camada de lógica de negócios. O console do SPA agenda e processa as solicitações de análise de desempenho.

**Estrutura de SPA**

SPA contém duas partes principais, a estrutura e os pacotes de Supervisor. A estrutura do SPA fornece todas as interfaces do usuário, processamento de log de desempenho, configuração, tratamento de erros e procedimentos de gerenciamento e APIs de banco de dados.

**Projeto do SPA**

Um projeto do SPA é um banco de dados que contém todas as informações sobre os servidores de destino, pacotes de Supervisor e relatórios de análise de desempenho que são gerados nos servidores de destino para os pacotes de Supervisor. Você pode comparar e exibir gráficos de histórico e tendências dentro do mesmo projeto do SPA. O usuário pode criar mais de um projeto. Os projetos do SPA são independentes um do outro e nenhum dado compartilhada entre projetos.

**servidor de destino**

O servidor de destino é o computador físico ou máquina virtual que executa o Windows Server com certas funções do servidor, como o IIS.

**Sessão de análise de dados**

Uma sessão de análise de dados é uma análise de desempenho em um servidor de destino específico. Uma sessão de análise de dados pode incluir vários pacotes de Supervisor. Os conjuntos de Coletores de dados desses pacotes de Supervisor são mesclados em um conjunto de Coletores de dados único. Todos os logs de desempenho para uma sessão de análise de dados são coletados durante o mesmo período de tempo. Analisar relatórios que são gerados por pacotes de supervisor em execução na mesma sessão de análise de dados pode ajudar os usuários a entender a situação geral de desempenho e identificar as causas de problemas de desempenho.

**Rastreamento de eventos para Windows**

[Rastreamento de eventos](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) para Windows (ETW) é um sistema de rastreamento de alto desempenho, baixa sobrecarga e dimensionável que é fornecido nos sistemas operacionais Windows. Ele fornece perfis e depuração de recursos, que podem ser usados para solucionar uma variedade de cenários. SPA usa eventos ETW como uma fonte de dados para gerar os relatórios de desempenho. Para obter informações gerais sobre o ETW, consulte [melhorar a depuração e ajuste de desempenho com o ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Consulta WMI**

Windows Management Instrumentation (WMI) é a infraestrutura para gerenciamento de dados e operações nos sistemas operacionais do Windows. Você pode escrever scripts WMI ou aplicativos para automatizar tarefas administrativas em computadores remotos. WMI também fornece dados de gerenciamento para outras partes do sistema operacional e produtos. SPA usa informações de classe WMI e pontos de dados como fontes para geração de relatórios de desempenho.

**Contadores de desempenho**

Contadores de desempenho são usados para fornecer informações sobre como o sistema operacional ou um aplicativo, serviço ou driver está sendo executado. Os dados do contador de desempenho podem ajudar a determinar gargalos do sistema e ajustar o desempenho do sistema e do aplicativo. O sistema operacional, rede e dispositivos fornecem dados de contador que um aplicativo pode utilizar para fornecer aos usuários uma exibição gráfica de como o sistema está sendo executado. SPA usa informações do contador de desempenho e pontos de dados como fontes para gerar relatórios de desempenho.

**Alertas e Logs de desempenho**

Logs de desempenho e alertas (CCP) é um serviço interno no sistema operacional Windows. Ele é projetado para coletar rastreamentos e logs de desempenho e também gera alertas de desempenho quando determinados disparadores são atendidos. CCP pode ser usado para coletar contadores de desempenho, rastreamento de eventos para Windows (ETW), consultas WMI, chaves do registro e configuração de arquivos. CCP também oferece suporte a coleta de dados remotos por meio de chamadas de procedimento remoto (RPC). O usuário define um conjunto de Coletores de dados, que inclui informações sobre os dados a serem coletados, a frequência da coleta de dados, duração da coleta de dados, filtros e um local para salvar os arquivos de resultados. SPA usa PLA para coletar todos os dados de desempenho de servidores de destino.

**Relatório simples**

Relatório único é o relatório SPA que é gerado com base em uma sessão de análise de dados para um pacote de supervisor em um servidor de destino único. Ela pode conter várias seções de dados e notificações.

**Relatório de lado a lado**

Um relatório de lado a lado é um relatório do SPA que compara dois relatórios únicos para o mesmo pacote de Supervisor. Os dois relatórios podem ser gerados a partir de servidores de destino diferentes ou execuções de análise de desempenho separados no mesmo servidor de destino. O relatório side-by-side cria a capacidade de comparar dois relatórios para ajudar os usuários a identificar comportamentos anormais ou configurações em um dos relatórios. Um relatório de lado a lado contém várias seções de dados e notificações. Em cada seção, dados de ambos os relatórios são listada lado a lado.

**Gráfico de tendência**

Um gráfico de tendência é o relatório SPA que é usado para investigar padrões repetitivos dos problemas de desempenho. Muitos problemas de desempenho repetitivas são causados por alterações de carga do servidor agendado do servidor ou de computadores cliente, que podem ocorrer diariamente ou semanalmente. SPA fornece um gráfico de tendências de 24 horas e um gráfico de tendência de 7 dias para identificar esses problemas.

O usuário pode escolher uma ou mais séries de dados por vez, que é um valor numérico no relatório único, como **média de utilização de CPU total**. mais especificamente, um valor numérico é um valor escalar de um único servidor que é gerado por um único ponto de acesso em uma instância de determinado tempo. SPA agrupa esses valores em 24 grupos, um para cada hora do dia (sete para um relatório de 7 dias, um para cada dia da semana). SPA calcula a média, mínimo, máximo e desvios padrão para cada grupo.

**Gráfico de histórico**

Um gráfico de histórico é o relatório SPA que é usado para mostrar as alterações em determinadas numéricos valores dentro de relatórios únicos para um determinado servidor e o par de pacote de supervisor ao longo do tempo. O usuário pode escolher várias séries de dados e mostrá-los juntos no gráfico de histórico para entender a correlação entre a série de dados diferente.

**Série de dados**

Uma série de dados é dados numéricos que são coletados da mesma fonte de dados em um período de tempo. A mesma fonte significa que os dados tem que vêm do mesmo servidor de destino, como o comprimento da fila de média de solicitação do IIS em um servidor.

**regras**

As regras são combinações de lógica, limites e descrições. Eles representam um possível problema de desempenho. Cada pacote advisor contém várias regras. Cada regra é acionada por um processo de geração de relatórios. Uma regra se aplica a lógica e os limites para os dados em um único relatório. Se os critérios forem atendidos, uma notificação de aviso será gerada. Se não, a notificação é definida como o **OK** estado. Se a regra não se aplica, a notificação é definida como não aplicável (**NA**) estado.

**Notificações**

Uma notificação é as informações que exibe uma regra para os usuários. Ele inclui o status da regra (**Okey**, **NA**, ou **aviso**), o nome da regra e recomendações possíveis para resolver os problemas de desempenho.
