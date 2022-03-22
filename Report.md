# Escopo 

Investigar componentes que possibilitem os seguintes comportamentos:

- Possibilitar hyperlink

  - Validar preview de url (nice to have)

- Permitir emojis

- Segurança

  - Adicionar atributos no element a do link  (Ex: sanitizer → MUP)

    - target = '_blank';

    - rel = “noopener nofollow noreferer“

  - Sanitização do conteúdo

  - Libs para verificar segurança do link

    - Trocar uma ideia com outras pessoas (do time, Paulo Neves, alguém de SRE)

- obs: Iniciar pelo Quill

    - Utilizando a Quill, será que poderemos ocultar a toolbar exclusivamente para o PD Grupos.

# Investigação

## Iniciando pelo Quill

A categoria onde o Quill se encaixa é chamada **content editor**. Existem outros Froala, que serão analisados melhor depois desse subtópico de Quill.

### Escondendo a toolbar

É para esconder a toolbar, basta setar o valor do campo ```tooltip``` em modules, no [construtor do objeto de Quill](https://quilljs.com/guides/how-to-customize-quill/).

**TODO**: verificar se isso funciona com ```Quill.register('modules/toolbar',false)``` dado que o objeto Quill já vem instanciado quando puxado por ```vue-quill-editor```.

### Carregando informações

#### Overview

Nós precisamos fazer Web Scrapping para colher os dados de um recurso que serão usados para exibir nosso preview, como título, descrição e thumbnail. No entanto, isso envolve acessar o recurso e possivelmente nos depararmos com erro de CORS. Devem haver [cuidados](https://www.scrapehero.com/how-to-prevent-getting-blacklisted-while-scraping/) para que consigamos fazer isso sem sermos bloqueados por alguma ferramenta anti-scraping.

Uma forma que deve ser efetiva de colher informação de sites não tão restritivos é os fazendo acreditar que eles estão sendo acessado de um web browser com o uso de uma ferramenta de automação. Em diversos projetos, para testes automatizados, nós usamos o Puppeteer, que justamente cumpre com essa função.

#### Soluções prontas

Seguem algumas soluções prontas encontradas que fazem o scraping da página e retornam essas informações úteis para exibição de thumbnail.

- [**Link-Preview-js**](https://github.com/ospfranco/link-preview-js): API que retorna um json com as informações desejadas. Aparentemente, ele não tenta se passar por um browser e simplesmente requisita o recurso usando ```cross-fetch``` e parseia o que vem com ```cheerio```.
- [**Link-Prevue**](https://github.com/nivaldomartinez/link-prevue): ele já retorna o componente, visualmente. Para nós, acredito ser mais interessante buscar somente as informações, e isso é feito pela sua API que será descrita a seguir.
- [**Link-Prevue-API**](https://github.com/nivaldomartinez/link-preview-api): ela é em Go, sendo uma camada bem básica sobre a lib [GoScraper](https://github.com/badoux/goscraper), que também acessa os recursos sem disfarçar que é um script.

### Uso de emojis

Existe uma extensão do Quill chamada [Quill emoji](https://gshow.globo.com/) (**TODO**: testar isso). No entanto ela requer acesso através da toolbar, cuja ideia era de ser escondida. Se formos considerar ainda usar o quill, [é possível editar o estilo da toolbar](https://quilljs.com/guides/how-to-customize-quill/#themes) (**TODO**: buscar como fazer isso).

# Principais Referências

- [Make a Link Preview: Ultimate Step By Step Guide](https://javascript.plainenglish.io/make-a-link-preview-ultimate-step-by-step-guide-5f0ac827cc89)

- [How to Scrape Websites Without Getting Blocked](https://www.scrapehero.com/how-to-prevent-getting-blacklisted-while-scraping/)

- [Link-Prevue](https://github.com/nivaldomartinez/link-prevue)

- [Explicação amigável de CORS](https://dev.to/lydiahallie/cs-visualized-cors-5b8h#cs-cors)