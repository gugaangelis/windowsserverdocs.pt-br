---
title: Ajuste de SQL e lidar com problemas de latência com o AD FS
description: Este documento discute como ajustar o SQL com o AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322862"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ajuste de SQL e lidar com problemas de latência com o AD FS
Em uma atualização para [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) , apresentamos as seguintes melhorias para reduzir entre a latência de banco de dados. Uma atualização futura para o AD FS 2019 incluirá essas melhorias.

## <a name="in-memory-cache-update-in-background-thread"></a>Atualização de cache na memória no thread em segundo plano 
Sempre anterior em implantações de disponibilidade (AoA), latência existia para qualquer "Operação de leitura" como o nó mestre pode estar localizado em um datacenter separado. A chamada entre dois datacenters diferentes resultou na latência.  

Na atualização mais recente para o AD FS, uma redução na latência é direcionada por meio da adição de um thread em segundo plano para atualizar o cache de configuração do AD FS e uma configuração para definir a período de tempo de atualização. O tempo gasto para uma pesquisa de banco de dados é reduzido significativamente no thread da solicitação, como as atualizações de cache de banco de dados são movidas para o thread em segundo plano.  

Quando o `backgroundCacheRefreshEnabled` é definido como true, o AD FS permitirá que o thread em segundo plano executar atualizações de cache. A frequência de coleta de dados do cache pode ser personalizada para um valor de tempo definindo `cacheRefreshIntervalSecs`. O valor padrão é definido como 300 segundos quando `backgroundCacheRefreshEnabled` é definido como true. Depois que o conjunto de valor de duração, o AD FS começa atualizando seu cache e enquanto a atualização está em andamento, os dados de cache antiga continuará a ser usado.  

>[!NOTE]
> Os dados do cache serão atualizados fora do `cacheRefreshIntervalSecs` valor se o ADFS recebe uma notificação do SQL, indicando que ocorreu uma alteração no banco de dados. Essa notificação disparará o cache a ser atualizado. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recomendações para a atualização do cache de configuração 
É o valor padrão para a atualização do cache **cinco minutos**. É recomendável defini-lo como **1 hora** para reduzir uma atualização de dados desnecessários pelo AD FS, porque os dados do cache serão atualizados se ocorrer alguma alteração SQL.  

O AD FS registra um retorno de chamada para que as alterações do SQL e, após uma alteração, o AD FS recebe uma notificação. Usando esse método, o ADFS recebe cada nova alteração do SQL, assim que ele ocorre. 

No caso de uma falha de rede que resulta no AD FS ausentes a notificação de SQL, o AD FS será atualizada no intervalo especificado pelo cache de atualização de valor. Se são suspeitas de quaisquer problemas de conectividade entre o AD FS e o SQL, é recomendável definir o valor de atualização do cache como inferior a 1 hora.  

### <a name="configuration-instructions"></a>Instruções de configuração 
O arquivo de configuração dá suporte a várias entradas de cache. O seguinte listado abaixo pode ser configurado com base nas necessidades da sua organização. 

O exemplo a seguir habilita a atualização do cache de plano de fundo e define o período de atualização do cache como 1800 segundos ou 30 minutos. Isso deve ser feito em cada nó do ADFS e o serviço ADFS deve ser reiniciado posteriormente. As alterações não afetam outros nós e teste o primeiro nó antes de fazer a alteração em todos os nós. 

  1. Navegue até o arquivo de configuração do AD FS e, na seção "Microsoft.IdentityServer.Service", adicione o abaixo de entrada:  
  
  - `backgroundCacheRefreshEnabled`  -Especifica se o recurso de cache do plano de fundo está habilitado. valores "true/false".
  - `cacheRefreshIntervalSecs` -Valor em segundos em que o ADFS atualizará o cache. O AD FS atualizará o cache, se houver qualquer alteração no SQL. AD FS receberá uma notificação de SQL e atualizar o cache.  
 
 >[!NOTE]
 > Todas as entradas no arquivo de configuração diferenciam maiusculas de minúsculas.  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
Valores configuráveis com suporte adicionais: 

   - **maxRelyingPartyEntries** - máximo número de entradas de terceiros de terceira parte confiável que manterá o AD FS na memória. Esse valor também é usado pelo cache de permissão de aplicativo oAuth. Se houver mais permissões do aplicativo que RPs e se todos os serão armazenados na memória, esse valor deve ser o número de permissões de aplicativo. O valor padrão é 1000.
   - **maxIdentityProviderEntries** - este é o número máximo de declarações do AD FS irá manter na memória de entradas de provedor. O valor padrão é 200. 
   - **maxClientEntries** -este é o número máximo de entradas de cliente OAuth do AD FS irá manter na memória. O valor padrão é 500. 
   - **maxClaimDescriptorEntries** - máximo número de entradas de descritor de declaração do AD FS irá manter na memória. O valor padrão é 500. 
   - **maxNullEntries** -isso é usado como cache negativo. Quando o AD FS procura por uma entrada no banco de dados e não for encontrado, o AD FS adiciona no cache negativo. Esse é o tamanho máximo desse cache. Não há cache negativo para cada tipo de objeto, não é um cache único para todos os objetos. O valor padrão é 50,0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Suporte a vários artefatos de banco de dados em data centers 
Para configurações anteriores de vários datacenters, o AD FS tem suporte apenas um único banco de dados de artefato, fazendo com que a latência de datacenter central cruzada durante as chamadas de recuperação.  

Para reduzir a latência entre datacenters, um administrador do AD FS pode agora implantar várias instâncias de banco de dados de artefato e, em seguida, modifique o arquivo de configuração de um nó AD FS para apontar para instâncias diferentes de artefato DB. A cadeia de caracteres de conexão de banco de dados de artefato pode ser fornecida no arquivo de configuração, permitindo que um banco de dados de artefato por nó. Se a cadeia de caracteres de conexão não está presente no arquivo de configuração, o nó fará o fallback para o projeto anterior para usar o banco de dados de artefato que está presente na configuração do banco de dados.  
Também há suporte para ambientes híbridos com essa configuração.  

### <a name="requirements"></a>Requisitos: 
Antes de configurar o suporte a vários artefato banco de dados, executar uma atualização em todos os nós e os binários de atualização, pois as chamadas de vários nós ocorrem através deste recurso. 
  1. Gere script de implantação para criar o banco de dados de artefato: Para implantar várias instâncias de banco de dados de artefato, um administrador será necessário gerar o script de implantação do SQL para o banco de dados de artefato. Como parte dessa atualização, existente `Export-AdfsDeploymentSQLScript`cmdlet foi atualizado para tirar opcionalmente em um parâmetro que especifica qual banco de dados do AD FS para gerar um script de implantação do SQL. 
 
 Por exemplo, para gerar o script de implantação para apenas o BD de artefato, especifique o `-DatabaseType` parâmetro e passe o valor "Artefato". Opcional `-DatabaseType` parâmetro especifica o tipo de banco de dados do AD FS e pode ser definido como: Todos (padrão), configuração ou artefato. Se nenhum `-DatabaseType` parâmetro for especificado, o script configurará o artefato e a configuração de scripts.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
O script gerado deve ser executado na máquina SQL para criar os bancos de dados necessários e dê a conta de serviço do AD FS, permissões de SA do SQL para esses bancos de dados.

 2. Crie o banco de dados de artefato usando o script de implantação. Copiar os scripts de implantação CREATEDB. SQL e SetPermissions.sql recém gerados para a máquina do servidor SQL e executá-los para criar o banco de dados local do artefato. 
 
 3. Modifique o arquivo de configuração para adicionar a conexão de banco de dados de artefato. 
 Navegue até o arquivo de configuração do nó do AD FS e, na seção "Microsoft.IdentityServer.Service", adicione um ponto de entrada para o ArtifactDB recém-configurado. 

 >[!NOTE] 
 > artifactStore e connectionString são valores diferencia maiusculas de minúsculas. Certifique-se de que eles estão configurados corretamente. &lt;artifactStore connectionString="Data Source=.\SQLInstance;Integrated Security=True;Initial Catalog=AdfsArtifactStore" /&gt; 
>
>Use um valor de fonte de dados que corresponde à sua conexão de sql.



 4. Reinicie o serviço do AD FS para que as alterações entrem em vigor. 
 
 >[!NOTE] 
 > Não é recomendável usar a replicação do SQL ou a sincronização entre os bancos de dados de artefato. A recomendação é definir um banco de dados de artefato por data center. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Entre a recuperação de failover e o banco de dados do data center  
É recomendável criar bancos de dados de artefato de failover no mesmo datacenter que o banco de dados mestre do artefato. Se ocorrer um failover, não haverá nenhum aumento na latência. Bancos de dados de artefato de failover em datacenters não é recomendado. Os seguintes detalhes como chamadas para o OAuth, SAML, ESL e token de repetir a função de detecção com vários bancos de dados de artefato. 
 - **OAuth e SAML** 

   Para solicitações de artefato SAML e OAuth, o nó criará o artefato no artefato DB presente no arquivo de configuração. Se o arquivo de configuração não contém uma conexão de banco de dados de artefato, ele usará o banco de dados de artefato comuns. Quando a próxima solicitação para buscar o artefato fica para outro nó, o outro nó fará API rest para o nó 1º para buscar o artefato de artefato de banco de dados. Isso é necessário como nós diferentes podem ter bancos de dados diferente de artefato e os nós não souber sobre isso. Se o 1º nó estiver inativo, a resolução de artefato falhará. Devido a esse design, não é necessário replicar o banco de dados de artefato em diferentes datacenters. Se um datacenter inteiro estiver inativo, provavelmente o nó que criou o artefato é também para baixo, que significa que esse artefato mais não pode ser resolvido.  

 - **Bloqueio de extranet** 

    O banco de dados de artefato referenciado no arquivo de configuração será usado para dados de bloqueio de Extranet. No entanto, para o recurso ESL, o AD FS escolhe um mestre que grava os dados no banco de dados do artefato. Todos os nós de fazem uma API REST chamada para o nó mestre para obter e definir as informações mais recentes sobre todos os usuários. Se vários artefato DB estiverem em uso, o administrador deve selecionar um nó mestre para cada artefato de banco de dados ou data center. 

    Para selecionar um nó para ser o mestre ESL, navegue até o arquivo de configuração do nó do ADFS e, na seção "Microsoft.IdentityServer.Service", adicione o seguinte:       
    
    No mestre de adicionar entrada a seguir. Observe que todas as três chaves diferenciam maiusculas de minúsculas. 

    &lt;useractivityfarmrole masterFQDN = isMaster [FQDN do primário selecionado] = "true" /&gt;
    
    Nos outros nós adicione o seguinte entrada:

   &lt;useractivityfarmrole masterFQDN=[FQDN of the selected primary] isMaster="false"/&gt;
 
    >[!NOTE] 
    >Uma vez que vários bancos de dados de artefato não sincronizam dados, valores ESL não serão sincronizadas entre bancos de dados de artefato.
    Um usuário pode potencialmente obter um datacenter diferente para uma solicitação, portanto, fazendo o ExtranetLockoutThreshold depende do número de bancos de dados de artefato, ExtranetLockoutThreshold * número de artefato de bancos de dados. 
 
  - **Detecção de reprodução de token** 
    
    Dados de detecção de reprodução de token sempre são chamados de banco de dados central do artefato. O AD FS evita o token a confiança do provedor de declarações, garantindo que o mesmo token não pode ser reproduzido. Se um invasor tentar reproduzir o mesmo token, o AD FS verifica se o token existe no BD de artefato. Se o token estiver presente, a solicitação será rejeitada. O banco de dados central do artefato é usado para segurança, pois os dados do banco de dados de artefato não são replicados, um invasor pode enviar a solicitação para outro datacenter e reproduzir um token. Criação de cópias adicionais de somente leitura do ArtifactDB não impedirá o datacenter cruzada latência nesse cenário, como apenas o artefato banco de dados central é usado.    
 
 