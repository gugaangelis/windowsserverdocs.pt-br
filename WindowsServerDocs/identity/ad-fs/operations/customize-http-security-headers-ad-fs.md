---
title: Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS
description: Este documento descirbes como personalizar cabeçalhos de segurança para proteger contra vulnerabilidades de segurança.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 231c8783032f51f607565922d90ea7f7eb877cfd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444694"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS de 2019 
 
Para proteger contra vulnerabilidades de segurança e fornecem aos administradores a capacidade de aproveitar os avanços mais recentes em mecanismos de proteção baseada em navegador, o AD FS 2019 adicionado a funcionalidade para personalizar os cabeçalhos de resposta de segurança HTTP enviado pelo AD FS. Isso é feito por meio da introdução de dois novos cmdlets: `Get-AdfsResponseHeaders` e `Set-AdfsResponseHeaders`.  
 
Neste documento, discutiremos comumente usado cabeçalhos de resposta de segurança para demonstrar como personalizar cabeçalhos enviados pelo AD FS de 2019.   
 
>[!NOTE]
>O documento pressupõe que o AD FS 2019 foi instalado.  

 
Antes de discutirmos os cabeçalhos, vamos dar uma olhada em alguns cenários, criando a necessidade de administradores personalizar os cabeçalhos de segurança 
 
## <a name="scenarios"></a>Cenários 
1. Administrador ativou [ **segurança de transporte estrito HTTP (HSTS)** ](#http-strict-transport-security-hsts) (força todas as conexões de criptografia HTTPS) para proteger os usuários que podem acessar o aplicativo web usando HTTP de um acesso de Wi-Fi públicos ponto que pode ser violado. Ela gostaria de reforçar a segurança adicional, habilitando a HSTS para subdomínios.  
2. Administrador tiver configurado o [ **X-Frame-Options** ](#x-frame-options) cabeçalho de resposta (evita a renderização de qualquer página da web em um iFrame) para proteger as páginas da web sejam clickjacked. No entanto, ela precisa personalizar o valor do cabeçalho devido a um novo requisito de negócios para exibir dados (em iFrame) de um aplicativo com uma origem diferente (domínio).
3. Administrador ativou [ **X-XSS-Protection** ](#x-xss-protection) (impede ataques de scripts entre) para limpar e bloquear a página, se o navegador detecta ataques de scripts entre. No entanto, ela precisa personalizar o cabeçalho para permitir que a página seja carregada uma vez limpo.  
4. Administrador precisa habilitar [ **Cross Origin Resource CORS (compartilhamento)** ](#cross-origin-resource-sharing-cors-headers) e definir a origem (domínio) no AD FS para permitir que um aplicativo de página única acessar uma API web com outro domínio.  
5. Administrador ativou [ **política de segurança de conteúdo (CSP)** ](#content-security-policy-csp) ataques de cabeçalho para evitar cruzada injeção de script e dados do site, não permitindo solicitações entre domínios. No entanto, devido a um novo requisito de negócios ela precisa personalizar o cabeçalho para permitir que a página da web para carregar imagens de qualquer origem e restringir a mídia para provedores confiáveis.  

 
## <a name="http-security-response-headers"></a>Cabeçalhos de resposta de segurança HTTP 
Os cabeçalhos de resposta são incluídos na resposta de saída HTTP enviada pelo AD FS para um navegador da web. Os cabeçalhos podem ser listados usando o `Get-AdfsResponseHeaders` cmdlet, conforme mostrado abaixo.  

![Resposta do cabeçalho](media/customize-http-security-headers-ad-fs/header1.png)

O `ResponseHeaders` atributo na captura de tela acima identifica os cabeçalhos de segurança que serão incluídos pelo AD FS em cada resposta HTTP. Os cabeçalhos de resposta serão enviados somente se `ResponseHeadersEnabled` é definido como `True` (valor padrão). O valor pode ser definido como `False` para impedir que o AD FS, incluindo qualquer um dos cabeçalhos de segurança na resposta HTTP. No entanto isso não é recomendado.  Para isso, use o seguinte:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>Strict--segurança de transporte HTTP (HSTS) 
HSTS é um mecanismo de política de segurança da web que ajuda a atenuar ataques de downgrade de protocolo e sequestro de cookie para serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores web para declarar que navegadores da web (ou outros agentes de usuário complying) só devem interagir com ela usando HTTPS e nunca por meio do protocolo HTTP.  
 
Todos os pontos de extremidade do AD FS para o tráfego de autenticação da web são abertos exclusivamente via HTTPS. Como resultado, o AD FS efetivamente reduz as ameaças que fornece o mecanismo de diretiva de segurança de transporte estrito HTTP (por padrão não é nenhum downgrade para HTTP, pois não há nenhum ouvinte HTTP). O cabeçalho pode ser personalizado, definindo os parâmetros a seguir 
 
- **max-age =&lt;tempo expirar&gt;**  – o tempo de expiração (em segundos) que especifica quanto tempo o site deve ser acessado somente usando HTTPS. Valor padrão e recomendado é 31536000 segundos (1 ano).  
- **includeSubDomains** – esse é um parâmetro opcional. Se for especificado, a regra HSTS se aplica a todos os subdomínios também.  
 
#### <a name="hsts-customization"></a>Personalização de HSTS 
Por padrão, o cabeçalho é habilitado e `max-age` definido como 1 ano; no entanto, os administradores podem modificar os `max-age` (diminuir o valor de idade máxima não recomendado) ou habilitar HSTS para subdomínios por meio do `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Por padrão, o cabeçalho é incluído na `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio de `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
O AD FS por padrão não permite aplicativos externos usar iFrames, ao executar logons interativos. Isso é feito para impedir que determinado estilo dos ataques de phishing. Observe que os logons não interativo podem ser executados por meio do iFrame devido à segurança em nível de sessão anterior que foi estabelecida.  
 
No entanto, em alguns casos raros, você pode confiar um aplicativo específico que requer a página de logon iFrame compatível com AD interativo. O cabeçalho 'X-Frame-Options' é usado para essa finalidade.  
 
Esse cabeçalho de resposta de segurança HTTP é usado para comunicar-se para o navegador se ele pode renderizar uma página em um &lt;quadro&gt;/&lt;iframe&gt;. O cabeçalho pode ser definido como um dos seguintes valores: 
 
- **Negar** – a página em um quadro não será exibida. Isso é o padrão e a configuração recomendada.  
- **sameorigin** – a página só será exibida no quadro se a origem é o mesmo que a origem da página da web. A opção não é muito útil, a menos que todos os ancestrais também estão na mesma origem.  
- **permitir a partir <specified origin>**  -a página só será exibida no quadro se a origem (por exemplo, https://www. ". com) corresponde a origem específica no cabeçalho. 

#### <a name="x-frame-options-customization"></a>Personalização de X-Frame-Options  
Por padrão, cabeçalho será definido como Negar; No entanto, os administradores podem modificar o valor por meio de `Set-AdfsResponseHeaders` cmdlet.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Por padrão, o cabeçalho é incluído na `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio de `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Esse cabeçalho de resposta de segurança HTTP é usado para interromper a páginas da web seja carregado quando ataques de script de entre sites (XSS) são detectados pelos navegadores. Isso é conhecido como filtragem de XSS. O cabeçalho pode ser definido como um dos seguintes valores 
 
- **0** – XSS desabilita a filtragem. Não é recomendado.  
- **1** – XSS permite a filtragem. Se o ataque XSS for detectado, o navegador corrigirá a página.   
- **1; modo = block** – XSS permite a filtragem. Se o ataque XSS for detectado, o navegador impedirá a renderização da página. Isso é o padrão e a configuração recomendada.  

#### <a name="x-xss-protection-customization"></a>Personalização de X-XSS-Protection 
Por padrão, o cabeçalho será definido como 1; modo = bloco; No entanto, os administradores podem modificar o valor por meio de `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Por padrão, o cabeçalho é incluído na `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio de `Set-AdfsResponseHeaders` cmdlet. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Entre os cabeçalhos Origin Resource Sharing (CORS) 
Segurança do navegador da Web impede que uma página da web faça solicitações entre origens iniciadas dentro de scripts. No entanto, às vezes, você talvez queira acessar recursos em outras origens (domínios). O CORS é um padrão W3C que permite que um servidor relaxar a política de mesma origem. Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras.  
 
Para entender melhor a solicitação CORS, vamos passo a passo um cenário em que uma página única (SPA) do aplicativo precisa chamar uma API web com um domínio diferente. Além disso, vamos considerar que o SPA e API são configurados no AD FS 2019 e AD FS tem CORS habilitado ou seja, o AD FS pode identificar os cabeçalhos do CORS na solicitação HTTP, validar valores de cabeçalho e incluir cabeçalhos de CORS apropriados na resposta (detalhes sobre como habilitar e Configure CORS no AD FS 2019 na seção de personalização de CORS abaixo). Exemplo de fluxo: 

1. Usuário acessa o SPA por meio do navegador do cliente e é redirecionado para o ponto de extremidade de autenticação do AD FS para autenticação. Uma vez que o SPA está configurado para o fluxo de concessão implícita, solicitar o token de retorna um acesso + ID para o navegador após a autenticação bem-sucedida.  
2. Após a autenticação de usuário, o front-end JavaScript incluído no SPA faz uma solicitação para acessar a API da web. A solicitação é redirecionada para o AD FS com os seguintes cabeçalhos
    - As opções – descreve as opções de comunicação para o recurso de destino 
    - Origem – inclui a origem da API web
    - Access-Control-Request-Method – identifica o método HTTP (por exemplo, excluir) a ser usado quando é feita a solicitação real 
    - Access-Control-Request-Headers - identifica os cabeçalhos HTTP a ser usado quando é feita a solicitação real 
    
   >[!NOTE]
   >Solicitação de CORS é semelhante a uma solicitação HTTP padrão, no entanto, a presença de um cabeçalho origin sinaliza que a solicitação de entrada é CORS relacionados. 
3. O AD FS verifica se a origem API incluída no cabeçalho da web está listada nas origens confiáveis configuradas no AD FS (detalhes sobre como modificar fontes confiáveis na seção de personalização de CORS abaixo). O AD FS, em seguida, responde com cabeçalhos a seguir.  
    - Access-Control-Allow-Origin – mesmo valor como o cabeçalho de origem 
    - Access-Control-permitir-Method – mesmo valor como o cabeçalho Access-Control-Request-Method 
    - Access-Control-Allow-Headers - mesmo valor como o cabeçalho Access-Control-Request-Headers 
4. Navegador envia a solicitação real, incluindo os seguintes cabeçalhos 
    - Método HTTP (por exemplo, excluir) 
    - Origem – inclui a origem da API web 
    - Todos os cabeçalhos incluídos no cabeçalho de resposta Access-Control-Allow-Headers 
5. Depois de verificar, o AD FS aprova a solicitação, incluindo o domínio de API da web (origem) no cabeçalho de resposta Access-Control-Allow-Origin.  
6. A inclusão do cabeçalho Access-Control-Allow-Origin permitirá que o navegador para prosseguir com a chamada de API solicitada.

#### <a name="cors-customization"></a>Personalização de CORS 
Por padrão, a funcionalidade CORS não será habilitada; No entanto, os administradores podem habilitar a funcionalidade por meio do cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Uma delas habilitada, os administradores poderão enumerar uma lista de origens confiáveis usando o mesmo cmdlet. Por exemplo, o comando a seguir seria permitir solicitações CORS das origens **https&#58;//example1.com** e **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Os administradores podem permitir que solicitações CORS de qualquer origem, incluindo "*" na lista de fontes confiáveis, embora essa abordagem não é recomendada devido a vulnerabilidades de segurança e uma mensagem de aviso é fornecido se escolherem. 

### <a name="content-security-policy-csp"></a>Content Security Policy (CSP) 
Esse cabeçalho de resposta de segurança HTTP é usado para impedir que o sequestro de clique, scripts intersites e outros ataques de injeção de dados, impedindo que os navegadores de executar inadvertidamente o conteúdo mal-intencionado. Navegadores que não há suporte para o CSP simplesmente ignora os cabeçalhos de resposta do CSP.  
 
#### <a name="csp-customization"></a>Personalização de CSP 
Personalização do cabeçalho do CSP envolve modificar a política de segurança que define o navegador de recursos tem permissão para carregar para a página da web. A política de segurança padrão é  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
O **src padrão** diretiva é usada para modificar [diretivas - src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) listando explicitamente cada diretiva. Por exemplo, no exemplo abaixo, a política de 1 é o mesmo como a política de 2.  

Política de 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Política 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Se uma diretiva é listada explicitamente, o valor especificado substituirá o valor fornecido para o padrão-src. No exemplo a seguir, img-src obterá o valor como ' *' (permitindo que imagens a serem carregados de qualquer origem) enquanto outras diretivas - src receberá o valor como 'Auto' (restringindo a mesma origem que a página da web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
As origens a seguir pode ser definida para a política padrão src 
 
- 'self' – especificar isso restringe a origem do conteúdo para carregar para a origem da página da web 
- '-inline unsafe' – especificar isso na política permite o uso de JavaScript embutido e CSS 
- 'unsafe-eval' – especificar isso na política permite o uso de texto para JavaScript mecanismos, como eval 
- 'none' – especificar isso restringe o conteúdo de qualquer origem para carregar 
- dados:-especificar dados: URIs permite que os criadores de conteúdo incorporar arquivos pequenos embutida nos documentos. Não é recomendado o uso.  
 
>[!NOTE]
>O AD FS usa o JavaScript no processo de autenticação e, portanto, permite que JavaScript com a inclusão de '-inline unsafe' e 'unsafe eval' padrão fontes política.  

### <a name="custom-headers"></a>Cabeçalhos personalizados 
Além disso, listados cabeçalhos de resposta de segurança (HSTS, CSP, X-Frame-Options, X-XSS-Protection e CORS), AD FS 2019 fornece a capacidade de definir novos cabeçalhos.  
 
Exemplo: Para definir um novo cabeçalho "TestHeader" com o valor como "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Uma vez definido, o novo cabeçalho é enviado na resposta do AD FS (fiddler trecho de código abaixo).  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilidade de navegador da Web
Use a tabela e os links a seguir para determinar quais navegadores são compatíveis com cada um dos cabeçalhos de resposta de segurança.

|Cabeçalhos de resposta de segurança HTTP|Compatibilidade do navegador|
|-----|-----|
|Strict--segurança de transporte HTTP (HSTS)|[Compatibilidade com navegadores HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[Compatibilidade com navegador de X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilidade com navegador de X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Cross Origin Resource Sharing (CORS)|[Compatibilidade de navegadores CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Content Security Policy (CSP)|[Compatibilidade de navegadores CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Próximo

- [Use guias de troublehshooting Ajuda do AD FS](https://aka.ms/adfshelp/troubleshooting )
- [Solução de problemas do AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
