---
title: Personalizar cabeçalhos de resposta de segurança HTTP com AD FS
description: Este documento descirbes como personalizar cabeçalhos de segurança para proteger contra vulnerabilidades de segurança.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3c497cbafb8f9313f988a1b892d2b8fef68115eb
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68523910"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS 2019 
 
Para proteger contra vulnerabilidades comuns de segurança e fornecer aos administradores a capacidade de aproveitar os avanços mais recentes em mecanismos de proteção baseados em navegador, AD FS 2019 adicionou a funcionalidade para personalizar os cabeçalhos de resposta de segurança HTTP enviado por AD FS. Isso é realizado por meio da introdução de dois novos cmdlets `Get-AdfsResponseHeaders` : `Set-AdfsResponseHeaders`e.  

>[!NOTE]
>A funcionalidade para personalizar os cabeçalhos de resposta de segurança http (exceto cabeçalhos CORS) usando cmdlets `Set-AdfsResponseHeaders` : `Get-AdfsResponseHeaders` e foi reportado para AD FS 2016. Você pode adicionar a funcionalidade ao seu AD FS 2016 instalando [KB4493473](https://support.microsoft.com/en-us/help/4493473/windows-10-update-kb4493473) e [KB4507459](https://support.microsoft.com/en-us/help/4507459/windows-10-update-kb4507459). 

Neste documento, abordaremos os cabeçalhos de resposta de segurança usados com frequência para demonstrar como personalizar os cabeçalhos enviados pelo AD FS 2019.   
 
>[!NOTE]
>O documento supõe que o AD FS 2019 foi instalado.  

 
Antes de discutirmos os cabeçalhos, vamos examinar alguns cenários que criam a necessidade de administradores personalizar cabeçalhos de segurança 
 
## <a name="scenarios"></a>Cenários 
1. O administrador habilitou o [**http Strict-Transport-Security (HSTS)** ](#http-strict-transport-security-hsts) (força todas as conexões via criptografia HTTPS) para proteger os usuários que podem acessar o aplicativo Web usando http de um ponto de acesso do WiFi público que pode ser invadido. Ela gostaria de reforçar ainda mais a segurança, habilitando HSTS para subdomínios.  
2. O administrador configurou o cabeçalho de resposta [**X-Frame-Options**](#x-frame-options) (impede a renderização de qualquer página da Web em um iframe) para proteger as páginas da Web de serem clickjacked. No entanto, ela precisa personalizar o valor do cabeçalho devido a um novo requisito de negócios para exibir dados (em iFrame) de um aplicativo com uma origem diferente (domínio).
3. O administrador habilitou a [**proteção X-XSS-Protection**](#x-xss-protection) (impede que ataques de script cruzado) limpem e bloqueiem a página se o navegador detectar ataques de script cruzado. No entanto, ela precisa personalizar o cabeçalho para permitir que a página seja carregada após a limpeza.  
4. O administrador precisa habilitar o [**CORS (compartilhamento de recursos entre origens)** ](#cross-origin-resource-sharing-cors-headers) e definir a origem (domínio) em AD FS para permitir que um aplicativo de página única acesse uma API da Web com outro domínio.  
5. O administrador habilitou o cabeçalho [**CSP (política de segurança de conteúdo)** ](#content-security-policy-csp) para evitar ataques de script entre sites e de injeção de dados, desautorizando as solicitações entre domínios. No entanto, devido a um novo requisito de negócios, ela precisa personalizar o cabeçalho para permitir que a página da Web carregue imagens de qualquer origem e restrinja a mídia a provedores confiáveis.  

 
## <a name="http-security-response-headers"></a>Cabeçalhos de resposta de segurança HTTP 
Os cabeçalhos de resposta são incluídos na resposta HTTP de saída enviada pelo AD FS para um navegador da Web. Os cabeçalhos podem ser listados usando o `Get-AdfsResponseHeaders` cmdlet, conforme mostrado abaixo.  

![Resposta de cabeçalho](media/customize-http-security-headers-ad-fs/header1.png)

O `ResponseHeaders` atributo na captura de tela acima identifica os cabeçalhos de segurança que serão incluídos por AD FS em cada resposta http. Os cabeçalhos de resposta serão enviados somente se `ResponseHeadersEnabled` for definido como `True` (valor padrão). O valor pode ser definido como `False` para evitar AD FS incluindo qualquer um dos cabeçalhos de segurança na resposta http. No entanto, isso não é recomendado.  Para fazer isso, use o seguinte:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-Transport-Security (HSTS) 
HSTS é um mecanismo de diretiva de segurança da Web que ajuda a mitigar ataques de downgrade de protocolo e seqüestro de cookie para serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores Web declarem que os navegadores da Web (ou outros agentes de usuário de conformidade) só devem interagir com ele usando HTTPS e nunca por meio do protocolo HTTP.  
 
Todos os pontos de extremidade AD FS para o tráfego de autenticação na Web são abertos exclusivamente por HTTPS. Como resultado, AD FS atenua efetivamente as ameaças que o mecanismo de política de segurança de transporte estrito de HTTP fornece (por padrão, não há nenhum downgrade para HTTP, pois não há ouvintes em HTTP). O cabeçalho pode ser personalizado definindo os seguintes parâmetros 
 
- **Max-age =&lt;tempo&gt;**  de expiração – o tempo de expiração (em segundos) especifica por quanto tempo o site só deve ser acessado usando HTTPS. O valor padrão e recomendado é 31536000 segundos (1 ano).  
- **includeSubDomains** – é um parâmetro opcional. Se especificado, a regra HSTS também se aplicará a todos os subdomínios.  
 
#### <a name="hsts-customization"></a>Personalização de HSTS 
Por padrão, o cabeçalho é habilitado e `max-age` definido como 1 ano; no entanto, os administradores `max-age` podem modificar o (o valor máximo de idade máxima não é recomendado) ou habilitar HSTS para subdomínios por meio do `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Por padrão, o cabeçalho é incluído no `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio do `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>Opções de X-frame 
AD FS por padrão não permite que aplicativos externos usem iFrames ao executar logons interativos. Isso é feito para evitar determinado estilo de ataques de phishing. Observe que os logons não interativos podem ser executados por meio de iFrame devido à segurança de nível de sessão anterior estabelecida.  
 
No entanto, em determinados casos raros, você pode confiar em um aplicativo específico que requer a página de logon de AD FS interativa compatível com iFrame. O cabeçalho ' X-Frame-Options ' é usado para essa finalidade.  
 
Esse cabeçalho de resposta de segurança http é usado para se comunicar com o navegador se ele pode renderizar &lt;uma&gt;página&gt;em um iframe de quadro/&lt;. O cabeçalho pode ser definido como um dos seguintes valores: 
 
- **negar** – a página em um quadro não será exibida. Essa é a configuração padrão e recomendada.  
- **SameOrigin** – a página só será exibida no quadro se a origem for igual à origem da página da Web. A opção não é muito útil, a menos que todos os ancestrais também estejam na mesma origem.  
- **permitir-from <specified origin>**  -a página só será exibida no quadro se a origem (por exemplo, https://www... com) corresponde à origem específica no cabeçalho. 

#### <a name="x-frame-options-customization"></a>Personalização de opções de X-frame  
Por padrão, o cabeçalho será definido como Deny; no entanto, os administradores podem modificar o `Set-AdfsResponseHeaders` valor por meio do cmdlet.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Por padrão, o cabeçalho é incluído no `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio do `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Esse cabeçalho de resposta de segurança HTTP é usado para interromper o carregamento de páginas da Web quando ataques XSS (script entre sites) são detectados por navegadores. Isso é chamado de filtragem XSS. O cabeçalho pode ser definido como um dos valores a seguir 
 
- **0** – desabilita a filtragem XSS. Não recomendado.  
- **1** – habilita a filtragem XSS. Se um ataque XSS for detectado, o navegador limpará a página.   
- **1; Mode = Block** – habilita a filtragem XSS. Se um ataque XSS for detectado, o navegador impedirá a renderização da página. Essa é a configuração padrão e recomendada.  

#### <a name="x-xss-protection-customization"></a>Personalização X-XSS-Protection 
Por padrão, o cabeçalho será definido como 1; Mode = bloquear; no entanto, os administradores podem modificar o `Set-AdfsResponseHeaders` valor por meio do cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Exemplo: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Por padrão, o cabeçalho é incluído no `ResponseHeaders` atributo; no entanto, os administradores podem remover o cabeçalho por meio do `Set-AdfsResponseHeaders` cmdlet. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Cabeçalhos CORS (compartilhamento de recursos entre origens) 
A segurança do navegador da Web impede que uma página da Web faça solicitações entre origens iniciadas de dentro dos scripts. No entanto, às vezes, talvez você queira acessar recursos em outras origens (domínios). O CORS é um padrão W3C que permite que um servidor Relaxe a política de mesma origem. Usando o CORS, um servidor pode permitir explicitamente algumas solicitações entre origens e, ao mesmo tempo, rejeitar outras.  
 
Para entender melhor a solicitação de CORS, vamos analisar um cenário em que um aplicativo de página única (SPA) precisa chamar uma API da Web com um domínio diferente. Além disso, vamos considerar que o SPA e a API estão configurados no ADFS 2019 e AD FS tem CORS habilitado, ou seja, AD FS pode identificar cabeçalhos CORS na solicitação HTTP, validar valores de cabeçalho e incluir cabeçalhos CORS apropriados na resposta (detalhes sobre como habilitar e Configure o CORS na seção AD FS 2019 na personalização CORS abaixo). Fluxo de exemplo: 

1. O usuário acessa o SPA por meio do navegador do cliente e é redirecionado para AD FS ponto de extremidade de autorização para autenticação. Como o SPA está configurado para o fluxo de concessão implícito, a solicitação retorna um token de ID de acesso + para o navegador após a autenticação bem-sucedida.  
2. Após a autenticação do usuário, o JavaScript de front-end incluído no SPA faz uma solicitação para acessar a API da Web. A solicitação é redirecionada para AD FS com os cabeçalhos a seguir
    - Opções – descreve as opções de comunicação para o recurso de destino 
    - Origem – inclui a origem da API Web
    - Access-Control-Request-Method – identifica o método HTTP (por exemplo, DELETE) a ser usado quando a solicitação real é feita 
    - Access-Control-request-headers-identifica os cabeçalhos HTTP a serem usados quando a solicitação real é feita 
    
   >[!NOTE]
   >A solicitação CORS é semelhante a uma solicitação HTTP padrão. no entanto, a presença de um cabeçalho de origem sinaliza que a solicitação de entrada está relacionada a CORS. 
3. AD FS verifica se a origem da API da Web incluída no cabeçalho está listada nas origens confiáveis configuradas no AD FS (detalhes sobre como modificar as origens confiáveis na seção de personalização do CORS abaixo). AD FS, em seguida, responde com os cabeçalhos a seguir.  
    - Acesso-controle-permitir-Origin – valor igual ao cabeçalho de origem 
    - Access-Control-Allow-Method – valor igual ao cabeçalho Access-Control-Request-Method 
    - Access-Control-Allow-Headers-Value igual ao cabeçalho Access-Control-request-headers 
4. O navegador envia a solicitação real, incluindo os seguintes cabeçalhos 
    - Método HTTP (por exemplo, DELETE) 
    - Origem – inclui a origem da API Web 
    - Todos os cabeçalhos incluídos no cabeçalho de resposta Access-Control-Allow-Headers 
5. Depois de verificado, AD FS aprova a solicitação incluindo o domínio da API da Web (origem) no cabeçalho de resposta Access-Control-Allow-Origin.  
6. A inclusão do cabeçalho acesso-controle-permitir-Origin permitirá que o navegador vá adiante, chamando a API solicitada.

#### <a name="cors-customization"></a>Personalização de CORS 
Por padrão, a funcionalidade CORS não será habilitada; no entanto, os administradores podem habilitar a funcionalidade por meio do cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Uma habilitada, os administradores poderão enumerar uma lista de origens confiáveis usando o mesmo cmdlet. Por exemplo, o comando a seguir permitiria solicitações CORS das origens **https&#58;//example1.com** e **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Os administradores podem permitir solicitações CORS de qualquer origem, incluindo "*" na lista de origens confiáveis, embora essa abordagem não seja recomendada devido a vulnerabilidades de segurança e uma mensagem de aviso seja fornecida se escolher. 

### <a name="content-security-policy-csp"></a>CSP (política de segurança de conteúdo) 
Esse cabeçalho de resposta de segurança HTTP é usado para impedir o script entre sites, sequestro e outros ataques de injeção de dados, impedindo que os navegadores executem inadvertidamente conteúdo mal-intencionado. Os navegadores que não dão suporte ao CSP simplesmente ignoram os cabeçalhos de resposta do CSP.  
 
#### <a name="csp-customization"></a>Personalização do CSP 
A personalização do cabeçalho CSP envolve a modificação da política de segurança que define que o navegador de recursos tem permissão para carregar para a página da Web. A política de segurança padrão é  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
A diretiva **Default-src** é usada para modificar as [diretivas-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) sem listar cada diretiva explicitamente. Por exemplo, no exemplo abaixo, a política 1 é a mesma que a política 2.  

Política 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Política 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Se uma diretiva estiver explicitamente listada, o valor especificado substituirá o valor fornecido para Default-src. No exemplo a seguir, o img-src usará o valor como "*" (permitindo que as imagens sejam carregadas de qualquer origem) enquanto outras diretivas src usarão o valor como "self" (restringindo à mesma origem da página da Web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
As fontes a seguir podem ser definidas para a política padrão-src 
 
- ' self ' – especificar isso restringe a origem do conteúdo a ser carregado na origem da página da Web 
- ' não seguro-embutido ' – especificar isso na política permite o uso de JavaScript embutido e CSS 
- ' unsafe-eval ' – especificar isso na política permite o uso de texto para mecanismos JavaScript como eval 
- ' none ' – especificar isso restringe o conteúdo de qualquer origem a ser carregada 
- dados:-especificando dados: Os URIs permitem que os criadores de conteúdo insiram arquivos pequenos embutidos em documentos. Uso não recomendado.  
 
>[!NOTE]
>O AD FS usa JavaScript no processo de autenticação e, portanto, habilita o JavaScript, incluindo fontes ' não seguras ' e ' não seguras ' na política padrão.  

### <a name="custom-headers"></a>Cabeçalhos personalizados 
Além dos cabeçalhos de resposta de segurança listados acima (HSTS, CSP, X-Frame-Options, X-XSS-Protection e CORS), AD FS 2019 fornece a capacidade de definir novos cabeçalhos.  
 
Exemplo: Para definir um novo cabeçalho "TestHeader" com o valor como "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Uma vez definido, o novo cabeçalho é enviado na resposta de AD FS (trecho de código Fiddler abaixo).  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilidade de plug Web
Use a tabela a seguir e os links para determinar quais navegadores da Web são compatíveis com cada um dos cabeçalhos de resposta de segurança.

|Cabeçalhos de resposta de segurança HTTP|Compatibilidade do navegador|
|-----|-----|
|HTTP Strict-Transport-Security (HSTS)|[Compatibilidade do navegador HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|Opções de X-frame|[Compatibilidade do navegador de opções X-frame](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilidade entre navegadores X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|CORS (compartilhamento de recursos entre origens)|[Compatibilidade de navegador CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|CSP (política de segurança de conteúdo)|[Compatibilidade do navegador CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Próximo

- [Usar AD FS guias de troublehshooting de ajuda](https://aka.ms/adfshelp/troubleshooting )
- [Solução de problemas do AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
