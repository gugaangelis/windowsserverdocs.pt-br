---
title: Criar Plug-ins com o modelo de avaliação de risco 2019 do AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e43f505a02ec2241a84f74ff57e217c2fb95157b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445355"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Criar Plug-ins com o modelo de avaliação de risco 2019 do AD FS

Agora você pode criar seus próprio plug-ins para bloquear ou atribuir uma pontuação de risco para solicitações de autenticação durante vários estágios – solicitação recebida, pré-autenticação e pós-autenticação. Isso pode ser feito usando o novo modelo de avaliação de risco introduzido com o AD FS de 2019. 

## <a name="what-is-the-risk-assessment-model"></a>Qual é o modelo de avaliação de risco?

O modelo de avaliação de risco é um conjunto de interfaces e classes que permitem aos desenvolvedores ler os cabeçalhos de solicitação de autenticação e implementar sua própria lógica de avaliação de risco. Em seguida, executa o código implementado (plug-in) alinhada com o processo de autenticação do AD FS. Para, por exemplo, usando as interfaces e classes incluídas com o modelo, você pode implementar o código para qualquer bloco ou permitir que a solicitação de autenticação com base no endereço IP do cliente incluído no cabeçalho da solicitação. O AD FS executará o código para cada solicitação de autenticação e tomar as devidas providências, de acordo com a lógica implementada. 

O modelo permite ao código de plug-in em qualquer um dos três estágios de pipeline de autenticação do AD FS conforme mostrado abaixo

![modelo](media/ad-fs-risk-assessment-model/risk1.png)

1.  **Solicitação recebida estágio** – permite a criação de plug-ins para permitir ou bloquear a solicitação quando o AD FS recebe a solicitação de autenticação ou seja, antes que o usuário inserir credenciais. Você pode usar o contexto de solicitação (por exemplo, IP do cliente, o método de Http, o servidor de proxy DNS, etc.) disponíveis neste estágio para realizar a avaliação de risco. Para, por exemplo, você pode criar um plug-in para ler o IP do contexto de solicitação e bloquear a solicitação de autenticação se o IP na lista predefinida de IPs arriscados. 

2.  **Fase de pré-autenticação** – permite a criação de plug-ins para permitir ou bloquear a solicitação no ponto em que o usuário fornece as credenciais, mas antes do AD FS avalia. Nesse estágio, além do contexto de solicitação você também tem informações sobre o contexto de segurança (por exemplo, o token de usuário, o identificador de usuário, etc.) e o contexto de protocolo (por exemplo, protocolo de autenticação, clientid, resourceid, etc.) para usar em sua lógica de avaliação de risco. Para, por exemplo, você pode criar um plug-in para evitar ataques de senha de spray lendo a senha do usuário do token de usuário e bloqueando a solicitação de autenticação, se a senha está na lista predefinida de senhas arriscadas. 

3.  **Pós-autenticação** – permite criar um plug-in para avaliar os riscos depois que o usuário forneceu credenciais e o AD FS tenha realizado a autenticação. Nesse estágio, além do contexto de solicitação, o contexto de segurança e o contexto de protocolo, você também tem informações sobre o resultado da autenticação (êxito ou falha). O plug-in pode avaliar a pontuação de risco com base nas informações disponíveis e passar a pontuação de risco para reclamar e regras de política de avaliação adicional. 

Para entender melhor como criar uma plug-in de avaliação de risco e executá-lo de acordo com o processo do AD FS, vamos criar um plug-in de exemplo que bloqueia solicitações que chegam de determinadas **extranet** IPs identificado como arriscado, registre-se o plug-in com o AD FS e, finalmente, a funcionalidade de teste. 

>[!NOTE]
>Este passo a passo serve apenas para mostrar como você pode criar um exemplo de plug-in. Não é a solução que estamos criando uma solução pronta da empresa.  

## <a name="building-a-sample-plug-in"></a>Criando um exemplo de plug-in

### <a name="pre-requisites"></a>Pré-requisitos
Abaixo está a lista de pré-requisitos necessários para compilar este exemplo de plug-in

- AD FS instalado e configurado de 2019
- .NET framework 4.7 e superiores
- Visual Studio

### <a name="build-plug-in-dll"></a>Compilar a dll de plug-in
O procedimento a seguir orientará você por meio da criação de uma dll de plug-in de exemplo.

1. Baixe o exemplo de plug-in, use Git Bash e digite o seguinte: 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Criar uma **. csv** arquivo em qualquer local no servidor do AD FS (no meu caso, eu criei o **authconfigdb.csv** arquivo cada **C:\extensions**) e adicione os IPs que você deseja bloquear a esse arquivo. 

   O exemplo de plug-in bloqueará qualquer solicitação de autenticação proveniente do **IPs Extranet** listados nesse arquivo. 

   >{! Observação] se você tiver um Farm do AD FS, você pode criar o arquivo em qualquer ou todos os servidores do AD FS. Qualquer um dos arquivos pode ser usado para importar os IPs arriscados para AD FS. Vamos discutir o processo de importação em detalhes na [registrar a dll de plug-in com o AD FS](#register-the-plug-in-dll-with-ad-fs) seção abaixo. 

3. Abra o projeto `ThreatDetectionModule.sln` usando o Visual Studio

4. Remover o `Microsoft.IdentityServer.dll` do Gerenciador de soluções, conforme mostrado abaixo:</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk2.png)

5. Adicionar referência para o `Microsoft.IdentityServer.dll` do seu AD FS conforme mostrado abaixo

   a.   Clique com botão direito **referências** na **Solutions Explorer** e selecione **adicionar referência...**</br> 
   ![Modelo](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.   Sobre o **Gerenciador de referências** janela select **procurar**. No **selecionar os arquivos a referenciar...** caixa de diálogo, selecione `Microsoft.IdentityServer.dll` da sua pasta de instalação do AD FS (no meu caso **C:\Windows\ADFS**) e clique em **Add**.
   
   >[!NOTE]
   >No meu caso, estou criando o plug-in no próprio servidor do AD FS. Se seu ambiente de desenvolvimento estiver em um servidor diferente, copie o `Microsoft.IdentityServer.dll` da sua pasta de instalação do AD FS no servidor do AD FS logon em sua caixa de desenvolvimento.</br> 
   
   ![modelo](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.   Clique em **Okey** sobre o **Gerenciador de referências** janela depois de verificar se `Microsoft.IdentityServer.dll` caixa de seleção está selecionada</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk5.png)
 
6. Todas as classes e referências estão agora em vigor para fazer um build.   No entanto, como a saída desse projeto é uma dll, ele terá que ser instalado na **Cache de Assembly Global**, ou GAC, do servidor do AD FS e a dll precisa ser assinado pela primeira vez. Isso pode ser feito da seguinte maneira:

   a.   **Clique com botão direito** no nome do projeto, ThreatDetectionModule. No menu, clique em **propriedades**.</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk6.png)
   
   b.   Dos **propriedades** , clique em **Signing**, à esquerda e marque a caixa de seleção marcada **assinar o assembly**. Dos **escolher um arquivo de chave de nome forte**: Puxe para baixo do menu, selecione **< novo... >**</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk7.png)

   c.   No **caixa de diálogo Criar chave de nome forte**, digite um nome (você pode escolher qualquer nome) para a chave, desmarque a caixa de seleção **proteger o arquivo de chaves com senha**. Clique em **OK**.
   ![Modelo](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.   Salve o projeto, conforme mostrado abaixo</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk9.png)

7. Compile o projeto clicando **construir** e, em seguida **recompilar solução** conforme mostrado abaixo</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk10.png)
 
   Verifique as **janela de saída**, na parte inferior da tela, para ver se ocorreu algum erro</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk11.png)


O plug-in (dll) agora está pronto para uso e está no **\bin\Debug** pasta da pasta do projeto (no meu caso, isso **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

A próxima etapa é registrar essa dll com o AD FS, para que ele seja executado em linha com o processo de autenticação do AD FS. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Registrar a dll de plug-in com o AD FS

Precisamos registrar a dll no AD FS usando o `Register-AdfsThreatDetectionModule` comando do PowerShell no servidor do AD FS, no entanto, antes de se registrar, é preciso obter o Token de chave pública. Esse token de chave pública foi criado quando criamos a chave e assinado o dll usando essa chave. Para saber o que o Token de chave pública para a dll é, você pode usar o **SN.exe** da seguinte maneira

1. Copie o arquivo dll do **\bin\Debug** pasta para outro local (no meu caso, copiá-la para **C:\extensions**)

2. Iniciar o **Prompt de comando do desenvolvedor** para Visual Studio e vá até o diretório que contém o **sn.exe** (no meu caso, o diretório é **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A ferramentas de \bin\NETFX 4.7.2**) ![modelo](media/ad-fs-risk-assessment-model/risk12.png)

3. Execute o **SN** com o **-T** parâmetro e o local do arquivo (no meu caso `SN -T “C:\extensions\ThreatDetectionModule.dll”`) ![modelo](media/ad-fs-risk-assessment-model/risk13.png)</br>
   O comando fornecerá a você o token de chave pública (para mim, o **Token de chave pública é 714697626ef96b35**)

4. Adicionar a dll para o **Cache de Assembly Global** do servidor do AD FS, nossa prática recomendada seria que você cria um instalador apropriado para seu projeto e use o instalador para adicionar o arquivo ao GAC. Outra solução é usar **Gacutil.exe** (para obter mais informações sobre **Gacutil.exe** disponível [aqui](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) no computador de desenvolvimento.  Como tenho meu visual studio no mesmo servidor do AD FS, usarei **Gacutil.exe** da seguinte maneira

   a.   No Prompt de comando do desenvolvedor para Visual Studio e vá até o diretório que contém o **Gacutil.exe** (no meu caso, o diretório é **C:\Program arquivos (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 ferramentas**)

   b.   Execute o **Gacutil** comando (no meu caso `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modelo](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >Se você tiver um farm do AD FS, os itens acima precisa ser executado em cada servidor do AD FS no farm. 

5. Abra **Windows PowerShell** e execute o seguinte comando para registrar a dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
   ```
   No meu caso, o comando é: 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
   ```
 
   >[!NOTE]
   >Você precisa registrar a dll apenas uma vez, mesmo se você tiver um farm do AD FS. 

6. Reinicie o serviço AD FS depois de registrar a dll

Isso é tudo, a dll agora está registrada com o AD FS e pronto para uso!

 >[!NOTE]
 > Se as alterações são feitas para o plug-in e o projeto for reconstruído, a dll atualizada precisa ser registrado novamente. Antes de registrar, você precisará cancelar o registro da dll atual usando o seguinte comando:</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> No meu caso, o comando é:
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>O plug-in de teste

1. Abra o **authconfig.csv** arquivo criado anteriormente (no meu caso, no local **C:\extensions**) e adicione o **IPs Extranet** você deseja bloquear. Cada IP deve estar em uma linha separada e não deve haver nenhum espaço no final</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk18.png)
 
2. Salve e feche o arquivo

3. Importe o arquivo atualizado no AD FS, executando o seguinte comando do PowerShell 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   No meu caso, o comando é: 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. Solicitação de inicie a autenticação do servidor com o mesmo IP que você adicionou na **authconfig.csv**.

   Para esta demonstração, usarei [ferramenta de raios x de declarações Ajuda do AD FS](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) para iniciar uma solicitação. Se você quiser usar a ferramenta de raios-x, siga as instruções 

   Digite a instância do servidor de Federação e clique em **autenticação de teste** botão.</br> 
   ![Modelo](media/ad-fs-risk-assessment-model/risk15.png) 

5. Autenticação é bloqueada, conforme mostrado abaixo.</br>
   ![Modelo](media/ad-fs-risk-assessment-model/risk16.png)
 
Agora que sabemos como criar e registrar o plug-in, vamos passo a passo do código para entender a implementação usando as novas interfaces e classes de plug-in introduzido com o modelo. 

## <a name="plug-in-code-walkthrough"></a>Instruções passo a passo de plug-in de código

Abra o projeto `ThreatDetectionModule.sln` usando o Visual Studio e, em seguida, abra o arquivo principal **UserRiskAnalyzer.cs** da **Gerenciador de soluções** à direita da tela</br>
![Modelo](media/ad-fs-risk-assessment-model/risk17.png)
 
O arquivo contém a classe principal UserRiskAnalyzer que implementa a classe abstrata [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) e a interface [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) para ler o IP da solicitação contexto, compare o IP obtido com os IPs carregados do banco de dados do AD FS e bloquear a solicitação se houver uma correspondência IP. Vamos falar sobre esses tipos em mais detalhes

### <a name="threatdetectionmodule-abstract-class"></a>Classe abstrata de ThreatDetectionModule

Essa classe abstrata carrega o plug-in no pipeline de AD FS, tornando possível executar o código de plug-in alinhada com o processo do AD FS. 

```
public abstract class ThreatDetectionModule 
{ 
    protected ThreatDetectionModule(); 
 
    public abstract string VendorName { get; } 
    public abstract string ModuleIdentifier { get; } 
 
 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger); 
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 }   
```
A classe inclui métodos e propriedades a seguir.

|Método |Tipo|Definição|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |void|Chamado pelo AD FS quando o plug-in é carregado em seu pipeline| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |void|Chamado pelo AD FS quando o plug-in é descarregado do seu pipeline| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| void|Chamado pelo AD FS na atualização de configuração |
|**Propriedade** |**Tipo** |**Definição**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|String |Obtém o nome do fornecedor que possui o plug-in|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|String |Obtém o identificador do plug-in|

Nossos plug-in de exemplo, estamos usando [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) e [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) métodos para ler os IPs predefinidos do banco de dados do AD FS. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) é chamado quando o plug-in está registrado com o AD FS, enquanto [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) é chamado quando o. csv é importada usando o `Import-AdfsThreatDetectionModuleConfiguration` cmdlet. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>Interface IRequestReceivedThreatDetectionModule

Isso [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) permite que você implemente a avaliação de risco no ponto em que o AD FS recebe a solicitação de autenticação, mas antes que o usuário insere credenciais ou seja, no estágio de solicitação recebida do processo de autenticação. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

A interface inclui [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) método que permite que você use o contexto da solicitação de autenticação é passado no parâmetro de entrada requestContext para escrever sua lógica de avaliação de risco. O parâmetro requestContext é do tipo [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

O outro parâmetro de entrada passado é o agente que é do tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). O parâmetro pode ser usado para gravar o erro, auditoria e/ou mensagens para o AD FS logs de depuração. 

O método retornará [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, 1 para o bloco e 2 para permitir) para o AD FS que bloqueia ou permite que a solicitação.

Em nosso exemplo de plug-in, [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) implementação do método analisa a [clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) do [requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) parâmetro e o compara com todos os IPs carregados a partir do banco de dados do AD FS. Se uma correspondência for encontrada, o método retorna 2 para **bloco**, caso contrário, retorna 1 para **permitir**. Com base no valor retornado, o AD FS ou bloqueia ou permite que a solicitação. 

>[!NOTE]
>O plug-in de exemplo discutido acima implementa apenas IRequestReceivedThreatDetectionModule interface. No entanto, o modelo de avaliação de risco fornece duas interfaces – IPreAuthenticationThreatDetectionModule (a fase de pré-autenticação durante do lógica de avaliação de risco de implementar) e IPostAuthenticationThreatDetectionModule (para implementar o risco lógica de avaliação durante a fase de pós-autenticação). Os detalhes em duas interfaces são fornecidos abaixo. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interface IPreAuthenticationThreatDetectionModule 

Isso [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) permite que você implemente a lógica de avaliação de risco no ponto em que o usuário fornece as credenciais, mas antes do AD FS avalia de estágio ou seja, pré-autenticação. 

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
IList<Claim> additionalClams
);
}
```
A interface inclui [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) método que permite que você use as informações passado a [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), e [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parâmetros de entrada para escrever sua lógica de avaliação de risco de pré-autenticação. 

>[!NOTE]
>Para obter a lista de propriedades passado com cada tipo de contexto, visite [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) definições de classe. 

O outro parâmetro de entrada passado é o agente que é do tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). O parâmetro pode ser usado para gravar o erro, auditoria e/ou mensagens para o AD FS logs de depuração.

O método retornará [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, 1 para o bloco e 2 para permitir) para o AD FS que bloqueia ou permite que a solicitação. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interface IPostAuthenticationThreatDetectionModule

Isso [interface](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) permite que você implemente a lógica de avaliação de risco depois que o usuário forneceu credenciais e o AD FS realizou a autenticação, ou seja, fase de pós-autenticação. 

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
AuthenticationResult authenticationResult, 
IList<Claim> additionalClams
);
}
```

A interface inclui [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) método que permite que você use as informações passado a [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), e [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parâmetros de entrada para escrever sua lógica de avaliação de risco de pós-autenticação. 

>[!NOTE]
> Para obter uma lista completa das propriedades passadas com cada tipo de contexto, consulte [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) definições de classe. 

O outro parâmetro de entrada passado é o agente que é do tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). O parâmetro pode ser usado para gravar o erro, auditoria e/ou mensagens para o AD FS logs de depuração. 

O método retorna o [pontuação de risco](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) que pode ser usado na política do AD FS e regras de declaração. 

>[!NOTE]
>Para o plug-in funcionar, a classe principal (no caso UserRiskAnalyzer) precisa derivar [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) classe abstrata e deve implementar pelo menos uma das três interfaces descritas acima. Quando a dll estiver registrada, o AD FS verifica quais das interfaces são implementadas e os chama apropriado estágio no pipeline.

### <a name="faqs"></a>Perguntas frequentes

**Por que eu deve criar esses plug-ins?**</br>
**R:** Esses plug-ins não apenas oferecem funcionalidade adicional para proteger seu ambiente contra ataques, como ataques de senha de irrigação, mas também oferecem a flexibilidade para criar sua própria lógica de avaliação de risco com base em seus requisitos. 

**Onde os logs são capturados?**</br>
**R:** Você pode gravar logs de erro "AD FS/administrador" log de eventos usando o método WriteAdminLogErrorMessage, logs de auditoria de segurança "Do AD FS auditoria" fazer logon usando o método WriteAuditMessage e depurar os logs "Rastreamento do AD FS" log de depuração usando o método WriteDebugMessage. 

**Adicionando esses plug-ins pode aumentar a latência de processo de autenticação do AD FS?**</br>
**R:** Impacto da latência será determinado pelo tempo gasto para executar a lógica de avaliação de risco que você implementar. É recomendável avaliar o impacto da latência antes de implantar o plug-in no ambiente de produção. 

**Por que o AD FS não pode sugerir a lista de IPs arriscados, usuários, etc?**</br>
**R:** Embora não disponível no momento, estamos trabalhando na criação de inteligência para sugerir IPs arriscados, usuários, etc. no modelo de avaliação de risco conectável. Nós Compartilharemos a inicialização datas em breve. 
