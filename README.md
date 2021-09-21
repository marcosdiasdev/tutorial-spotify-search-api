# Utilizando a Search API do Spotify

A API utilizada no projeto será a Search API, que permite a busca por artistas, álbuns, músicas etc. na base de dados do Spotify: https://developer.spotify.com/documentation/web-api/reference/#category-search

O guia de autorização das APIs do Spotify pode ser acessado em https://developer.spotify.com/documentation/general/guides/authorization-guide/

Para obter autorização, primeiro é necessário registrar a aplicação em https://developer.spotify.com/documentation/general/guides/app-settings/#register-your-app, e em seguida seguir um dos fluxos de autorização. O adequado para sua aplicação parece ser o Client Credentials Flow: https://developer.spotify.com/documentation/general/guides/authorization-guide/#client-credentials-flow

Antes de seguir o fluxo de autorização, anote o `Client ID` e o `Client Secret Key` da sua aplicação na Dashboard: https://developer.spotify.com/dashboard/applications

## Obtendo um token de autorização

Para obter um token de autorização, envie uma requisição HTTP POST para a rota `https://accounts.spotify.com/api/token`.

Esta requisição possuirá o seguinte corpo (body): `{ "grant_type": "client_credentials" }`, e os seguintes cabeçalhos (headers):

```json
{
	"Content-Type": "application/x-www-form-urlencoded",
	"Authorization": "Basic <client_id:client_secret em base64>"
}
```

Para converter `client_id:client_secret` em formato Base64, entre em https://www.base64encode.org/ e clique na opção Encode. Cole sua string, que deverá ser semelhante a `2da9469d12df47ccb94e83589b01f140:dad0f16b64944014b6ee2914541fac5b`. O site te retornará uma string em formato Base64 semelhante a `MmRhOTQ2OWQxMmRkNDdjY2I4NGU4MzU4OWIwMWYxNDA6ZGRkMGYwNmI2NDk0NDAxNGI2ZWUyOTE0NTQxZmFjNWI=`. No fim, seu cabeçalho `Authorization` parecerá com o seguinte: 

```json
"Authorization": "Basic MmRhOTQ2OWQxMmRkNDdjY2I4NGU4MzU4OWIwMWYxNDA6ZGRkMGYwNmI2NDk0NDAxNGI2ZWUyOTE0NTQxZmFjNWI="
```

Se tudo der certo, a API te retornará um objeto JSON semelhante ao seguinte:

```json
{
  "access_token": "BQDXSlD3pdDSK9yjeB5MlnpbLwG9urqoA6ZYLqdLKW2NZIGaq8Ok_TvmdxMuk97XcPziVugvMddK5JjJjt4",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

Você precisa apenas armazenar o valor de `access_token`.

## Realizando requisições autorizadas na Search API

Agora você pode realizar requisições autorizadas para buscar dados na Search API. Para isto, realize uma requisição HTTP GET na rota `https://api.spotify.com/v1/search`.

A sua requisição deverá levar dois query parameters (parâmetros de consulta): o parâmetro `q`, que deverá conter as palavras chaves a serem buscadas, e o parâmetro `type`, que determinará o tipo de recurso que você está buscando, se artista ou álbum, por exemplo.

Um exemplo de URI requisitada pode ser `https://api.spotify.com/v1/search?q=slipknot&type=artist`.

Para que sua requisição seja aceita pela API, é necessário que ela acompanhe um cabeçalho de autorização contendo o `access_token` que você obteve no passo anterior. Assim, sua requisição deverá informar o seguinte cabeçalho:

```json
{
	"Authorization": "Bearer <access_token>"
}
```

Se tudo der certo, o resultado retornado pela API será semelhante a:


```json
{
  "artists": {
    "href": "https://api.spotify.com/v1/search?query=slipknot&type=artist&offset=0&limit=20",
    "items": [
      {
        "external_urls": {
          "spotify": "https://open.spotify.com/artist/05fG473iIaoy82BF1aGhL8"
        },
        "followers": {
          "href": null,
          "total": 7531388
        },
        "genres": [
          "alternative metal",
          "double drumming",
          "nu metal",
          "rap metal"
        ],
        "href": "https://api.spotify.com/v1/artists/05fG473iIaoy82BF1aGhL8",
        "id": "05fG473iIaoy82BF1aGhL8",
        "images": [
          {
            "height": 640,
            "url": "https://i.scdn.co/image/ab6761610000e5eb8e6164f9f60da15dd7d4129f",
            "width": 640
          },
          {
            "height": 320,
            "url": "https://i.scdn.co/image/ab676161000051748e6164f9f60da15dd7d4129f",
            "width": 320
          },
          {
            "height": 160,
            "url": "https://i.scdn.co/image/ab6761610000f1788e6164f9f60da15dd7d4129f",
            "width": 160
          }
        ],
        "name": "Slipknot",
        "popularity": 79,
        "type": "artist",
        "uri": "spotify:artist:05fG473iIaoy82BF1aGhL8"
      }
  	]
  }
}
```
