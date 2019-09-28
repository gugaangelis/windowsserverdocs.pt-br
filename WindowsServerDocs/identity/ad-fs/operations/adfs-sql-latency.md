---
title: Ajuste fino do SQL e solução de problemas de latência com AD FS
description: Este documento discute como ajustar o SQL com AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 785ecd4de86c06dd12eb57e41efaa1103f2afdc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357806"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ajuste fino do SQL e solução de problemas de latência com AD FS
Em uma atualização para [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) , apresentamos os seguintes aprimoramentos para reduzir a latência entre bancos de dados. Uma atualização futura para o AD FS 2019 incluirá esses aprimoramentos.

## <a name="in-memory-cache-update-in-background-thread"></a>Atualização de cache na memória no thread em segundo plano 
Em implantações AoA (disponibilidade Always on) anteriores, a latência existia para qualquer operação de "leitura", pois o nó mestre poderia estar localizado em um datacenter separado. A chamada entre dois data centers diferentes resultou em latência.  

Na atualização mais recente para AD FS, uma redução na latência é direcionada pela adição de um thread em segundo plano para atualizar o cache de configuração AD FS e uma configuração para definir o período de atualização. O tempo gasto para uma pesquisa de banco de dados é significativamente reduzido no thread de solicitação, pois as atualizações do cache de banco de dados são movidas para o thread em segundo plano.  

Quando o `backgroundCacheRefreshEnabled` for definido como true, AD FS permitirá que o thread em segundo plano execute atualizações de cache. A frequência de busca de dados do cache pode ser personalizada para um valor de tempo por meio `cacheRefreshIntervalSecs`da configuração. O valor padrão é definido como 300 segundos quando `backgroundCacheRefreshEnabled` é definido como true. Após a duração do valor definido, AD FS começar a atualizar o cache e, enquanto a atualização estiver em andamento, os dados de cache antigos continuarão a ser usados.  

>[!NOTE]
> Os dados do cache serão atualizados fora do `cacheRefreshIntervalSecs` valor se o ADFS receber uma notificação do SQL, indicando que ocorreu uma alteração no banco de dados. Essa notificação irá disparar o cache a ser atualizado. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recomendações para definir a atualização do cache 
O valor padrão para a atualização do cache é de **cinco minutos**. É recomendável defini-lo para uma **hora** a fim de reduzir uma atualização de dados desnecessária por AD FS, pois os dados de cache serão atualizados se ocorrerem alterações de SQL.  

AD FS registra um retorno de chamada para alterações de SQL e, após uma alteração, o ADFS recebe uma notificação. Por meio desse método, o ADFS recebe cada nova alteração do SQL assim que ela ocorre. 

No caso de uma falha de rede que resulte em AD FS faltando a notificação SQL, AD FS será atualizada no intervalo especificado pelo valor de atualização do cache. Se houver suspeita de problemas de conectividade entre AD FS e SQL, é recomendável definir o valor de atualização do cache para menos de uma hora.  

### <a name="configuration-instructions"></a>Instruções de configuração 
O arquivo de configuração dá suporte a várias entradas de cache. Todos os itens listados abaixo podem ser configurados com base nas necessidades da sua organização. 

O exemplo a seguir habilita a atualização do cache em segundo plano e define o período de atualização do cache para 1800 segundos ou 30 minutos. Isso deve ser feito em cada nó do ADFS e o serviço ADFS deve ser reiniciado posteriormente. As alterações não afetam outros nós e testam o primeiro nó antes de fazer a alteração em todos os nós. 

  1. Navegue até o arquivo de configuração AD FS e, na seção "Microsoft. IdentityServer. Service", adicione a entrada abaixo:  
  
  - `backgroundCacheRefreshEnabled`-Especifica se o recurso de cache em segundo plano está habilitado. valores "true/false".
  - `cacheRefreshIntervalSecs`-Valor em segundos no qual o ADFS atualizará o cache. AD FS atualizará o cache se houver qualquer alteração no SQL. AD FS receberá uma notificação SQL e atualizará o cache.  
 
 >[!NOTE]
 > Todas as entradas no arquivo de configuração diferenciam maiúsculas de minúsculas.  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
Valores adicionais configuráveis com suporte: 

   - **maxRelyingPartyEntries** – número máximo de entradas de terceira parte confiável que AD FS manterá na memória. Esse valor também é usado pelo cache de permissão do aplicativo oAuth. Se houver mais permissões de aplicativo do que o RPs e se todas elas forem armazenadas na memória, esse valor deverá ser o número de permissões do aplicativo. O valor padrão é 1000.
   - **maxIdentityProviderEntries** -este é o número máximo de entradas do provedor de declarações AD FS manterá a memória. O valor padrão é 200. 
   - **maxClientEntries** -este é o número máximo de entradas do cliente OAuth AD FS permanecerá na memória. O valor padrão é 500. 
   - **maxClaimDescriptorEntries** – número máximo de entradas do descritor de declaração AD FS permanecerá na memória. O valor padrão é 500. 
   - **maxNullEntries** -isso é usado como cache negativo. Quando AD FS procura uma entrada no banco de dados e não é encontrada, AD FS adiciona em cache negativo. Esse é o tamanho máximo desse cache. Há um cache negativo para cada tipo de objeto; ele não é um único cache para todos os objetos. O valor padrão é 50, 0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Suporte a vários artefatos de BD em data centers 
Para configurações anteriores de vários data centers, AD FS tem suporte apenas para um único banco de dados de artefato, causando latência de datacenter entre os centros durante chamadas de recuperação.  

Para reduzir a latência entre datacenters, um administrador de AD FS agora pode implantar várias instâncias de BD de artefato e, em seguida, modificar o arquivo de configuração de um nó de AD FS para apontar para instâncias de BD de artefatos diferentes. A cadeia de conexão do banco de dados de artefato pode ser fornecida no arquivo de configuração, permitindo um BD de artefato por nó. Se a cadeia de conexão não estiver presente no arquivo de configuração, o nó voltará ao design anterior para usar o banco de dados de artefato que está presente no BD de configuração.  
Ambientes híbridos também têm suporte com essa configuração.  

### <a name="requirements"></a>Requisitos: 
Antes de configurar o suporte a vários bancos de dados de artefatos, execute uma atualização em todos os nós e atualize os binários, já que as chamadas de vários nós ocorrem por meio desse recurso. 
  1. Gerar script de implantação para criar o artefato DB: Para implantar várias instâncias de BD de artefato, um administrador precisará gerar o script de implantação do SQL para o artefato DB. Como parte dessa atualização, o cmdlet existente `Export-AdfsDeploymentSQLScript`foi atualizado para, opcionalmente, executar um parâmetro especificando para qual AD FS banco de dados gerar um script de implantação do SQL. 
 
 Por exemplo, para gerar o script de implantação apenas para o artefato DB, especifique `-DatabaseType` o parâmetro e passe o valor "artefato". O parâmetro `-DatabaseType` opcional especifica o tipo de banco de dados AD FS e pode ser definido como: Todos (padrão), artefato ou configuração. Se nenhum `-DatabaseType` parâmetro for especificado, o script configurará o artefato e os scripts de configuração.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
O script gerado deve ser executado no computador SQL para criar os bancos de dados necessários e fornecer a conta de serviço AD FS, as permissões SA do SQL para esses bancos de dados.

 2. Crie o artefato DB usando o script de implantação. Copie os scripts de implantação CreateDB. SQL e SetPermissions. SQL gerados recentemente para o computador do SQL Server e execute-os para criar o BD de artefato local. 
 
 3. Modifique o arquivo de configuração para adicionar a conexão de BD DB. 
 Navegue até o arquivo de configuração do nó AD FS e, na seção "Microsoft. IdentityServer. Service", adicione um ponto de entrada para o ArtifactDB recentemente configurado. 

 >[!NOTE] 
 > artifactStore e connectionString são valores diferenciados de maiúsculas e minúsculas. Verifique se eles estão configurados corretamente. &lt;artifactStore connectionString = "fonte de dados = .\SQLInstance; segurança integrada = true; catálogo inicial = AdfsArtifactStore"/&gt; 
>
>Use um valor de fonte de dados que corresponda à sua conexão SQL.



 4. Reinicie o serviço AD FS para que as alterações entrem em vigor. 
 
 >[!NOTE] 
 > Não é recomendável usar a replicação do SQL ou a sincronização entre os bancos de dados de artefatos. A recomendação é configurar um banco de dados de artefato por datacenter. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Failover entre datacenters e recuperação de banco de dados  
É recomendável criar bancos de dados de artefato de failover no mesmo datacenter que o banco de dados de artefato mestre. Se ocorrer um failover, não haverá aumento na latência. Os bancos de dados de artefatos de failover em data centers não são recomendados. Os detalhes a seguir são como as chamadas para OAuth, SAML, ESL e a função de detecção de reprodução de token com vários bancos de dados de artefatos. 
 - **OAuth e SAML** 

   Para solicitações de artefato do OAuth e SAML, o nó criará o artefato no BD do artefato presente no arquivo de configuração. Se o arquivo de configuração não contiver uma conexão de banco de dados de artefato, ele usará o BD de artefatos comum. Quando a próxima solicitação para buscar o artefato vai para outro nó, o outro nó fará com que a API REST vá para o primeiro nó para buscar o artefato do BD do artefato. Isso é necessário, pois nós diferentes podem ter bancos de dados de artefato diferentes e os nós não sabem disso. Se o 1º nó estiver inativo, a resolução do artefato falhará. Devido a esse design, não é necessário replicar o artefato DB entre diferentes datacenters. Se um datacenter inteiro estiver inativo, provavelmente o nó que criou o artefato também estará inativo, o que significa que o artefato não pode mais ser resolvido.  

 - **Bloqueio de extranet** 

    O banco de dados de artefato referenciado no arquivo de configuração será usado para o bloqueio de extranet. No entanto, para o recurso ESL, AD FS escolhe um mestre que grava os dados no BD do artefato. Todos os nós fazem uma chamada à API REST para o nó mestre para obter e definir as informações mais recentes sobre cada usuário. Se vários BD de artefato estiverem em uso, o administrador deverá selecionar um nó mestre para cada artefato DB ou Datacenter. 

    Para selecionar um nó para ser o mestre do ESL, navegue até o arquivo de configuração do nó do ADFS e, na seção "Microsoft. IdentityServer. Service", adicione o seguinte:       
    
    No mestre, adicione a seguinte entrada. Observe que todas as três chaves diferenciam maiúsculas de minúsculas. 

    &lt;useractivityfarmrole masterFQDN = [FQDN do primário selecionado] IsMaster = "true"/&gt;
    
    Nos outros nós, adicione a seguinte entrada:

   &lt;useractivityfarmrole masterFQDN = [FQDN do primário selecionado] IsMaster = "false"/&gt;
 
    >[!NOTE] 
    >Uma vez que vários bancos de dados de artefato não sincronizam, os valores de ESL não serão sincronizados entre bancos de dados de artefato.
    Um usuário pode potencialmente atingir um datacenter diferente para uma solicitação, portanto, tornando o ExtranetLockoutThreshold dependente do número de bancos de usuários do artefato, ExtranetLockoutThreshold * número de bancos de usuários do artefato. 
 
  - **Detecção de reprodução de token** 
    
    Os dados de detecção de reprodução de token são sempre chamados do banco de dado de artefato central. AD FS salva o token da confiança do provedor de declarações, garantindo que o mesmo token não possa ser repetido. Se um invasor tentar reproduzir o mesmo token, AD FS verificará se o token existe no BD do artefato. Se o token estiver presente, a solicitação será rejeitada. O banco de dados de artefato central é usado para segurança, já que o artefato DB data não é replicado, um invasor pode enviar a solicitação para outro datacenter e repetir um token. A criação de cópias adicionais somente leitura do ArtifactDB não impedirá a latência entre datacenters nesse cenário, pois apenas o banco de dados de artefato central é usado.    
 
 