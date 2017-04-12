# 06 - React Router, Server-Side Rendering, and Helmet

Code for this chapter available [here](https://github.com/verekia/js-stack-walkthrough/tree/master/06-react-router-ssr-helmet).
この章で扱うコードは[こちら](https://github.com/verekia/js-stack-walkthrough/tree/master/06-react-router-ssr-helmet)です．

In this chapter we are going to create different pages for our app and make it possible to navigate between them.
この章ではページを複数作り，ページ間を移動する方法について紹介します．

## React Router

> 💡 **[React Router](https://reacttraining.com/react-router/)** is a library to navigate between pages in your React app. It can be used on both the client and the server.
> 💡 **[React Router](https://reacttraining.com/react-router/)** はReactでページ間を移動するためのライブラリーです．サーバー側とクライアント側の両方で動きます．

React Router has received a major update with its v4 release which is still in beta. Since I want this tutorial to be future-proof, we'll be using v4.
React Routerはv4（β版）でメジャーアップデートを行いました．このチュートリアルでは将来的にも使いやすいようにv4を使っていきます．

- Run `yarn add react-router@next react-router-dom@next`
- `yarn add react-router@next react-router-dom@next` を実行します

On the client side, we first need to wrap our app inside a `BrowserRouter` component.
まず，クライアント側ではBrowserRouterでappを囲います

- Update your `src/client/index.jsx` like so:
- `src/client/index.jsx` をこのように変更します:

```js
// [...]
import { BrowserRouter } from 'react-router-dom'
// [...]
const wrapApp = (AppComponent, reduxStore) =>
  <Provider store={reduxStore}>
    <BrowserRouter>
      <AppContainer>
        <AppComponent />
      </AppContainer>
    </BrowserRouter>
  </Provider>
```

## Pages

Our app will have 4 pages:
ページを4つ作ります:

- A Home page.
- ホームページ
- A Hello page showing a button and message for the synchronous action.
- 同期的にメッセージとボタンを表示するデモページ
- A Hello Async page showing a button and message for the asynchronous action.
- 非同期的にメッセージとボタンを表示するデモページ
- A 404 "Not Found" page.
- 404 "Not Found" ページ

- Create a `src/client/component/page/home.jsx` file containing:
- `src/client/component/page/home.jsx` を次のように作成します:

```js
// @flow

import React from 'react'

const HomePage = () => <p>Home</p>

export default HomePage
```

- Create a `src/client/component/page/hello.jsx` file containing:
- `src/client/component/page/hello.jsx` を次のように作成します:

```js
// @flow

import React from 'react'

import HelloButton from '../../container/hello-button'
import Message from '../../container/message'

const HelloPage = () =>
  <div>
    <Message />
    <HelloButton />
  </div>

export default HelloPage

```

- Create a `src/client/component/page/hello-async.jsx` file containing:
- `src/client/component/page/hello-async.jsx` を次のように作成します:

```js
// @flow

import React from 'react'

import HelloAsyncButton from '../../container/hello-async-button'
import MessageAsync from '../../container/message-async'

const HelloAsyncPage = () =>
  <div>
    <MessageAsync />
    <HelloAsyncButton />
  </div>

export default HelloAsyncPage
```

- Create a `src/client/component/page/not-found.jsx` file containing:
- `src/client/component/page/not-found.jsx` を次のように作成します:

```js
// @flow

import React from 'react'

const NotFoundPage = () => <p>Page not found</p>

export default NotFoundPage
```

## Navigation

Let's add some routes in the shared config file.
共有設定ファイルにルートを追加していきましょう．

- Edit your `src/shared/routes.js` like so:
- `src/shared/routes.js` を次のように編集します:

```js
// @flow

export const HOME_PAGE_ROUTE = '/'
export const HELLO_PAGE_ROUTE = '/hello'
export const HELLO_ASYNC_PAGE_ROUTE = '/hello-async'
export const NOT_FOUND_DEMO_PAGE_ROUTE = '/404'

export const helloEndpointRoute = (num: ?number) => `/ajax/hello/${num || ':num'}`
```

The `/404` route is just going to be used in a navigation link for the sake of demonstrating what happens when you click on a broken link.
ルート`/404`は存在しないリンクをクリックした時に何が起きるか見るためのデモ用のリンクになります．

- Create a `src/client/component/nav.jsx` file containing:
- `src/client/component/nav.jsx` を次のように作成します:

```js
// @flow

import React from 'react'
import { NavLink } from 'react-router-dom'
import {
  HOME_PAGE_ROUTE,
  HELLO_PAGE_ROUTE,
  HELLO_ASYNC_PAGE_ROUTE,
  NOT_FOUND_DEMO_PAGE_ROUTE,
} from '../../shared/routes'

const Nav = () =>
  <nav>
    <ul>
      {[
        { route: HOME_PAGE_ROUTE, label: 'Home' },
        { route: HELLO_PAGE_ROUTE, label: 'Say Hello' },
        { route: HELLO_ASYNC_PAGE_ROUTE, label: 'Say Hello Asynchronously' },
        { route: NOT_FOUND_DEMO_PAGE_ROUTE, label: '404 Demo' },
      ].map(link => (
        <li key={link.route}>
          <NavLink to={link.route} activeStyle={{ color: 'limegreen' }} exact>{link.label}</NavLink>
        </li>
      ))}
    </ul>
  </nav>

export default Nav
```

Here we simply create a bunch of `NavLink`s that use the previously declared routes.
ここではシンプルに上記で宣言したルートを使用するための`NavLink`を作成していきます．

- Finally, edit `src/client/app.jsx` like so:

```js
// @flow

import React from 'react'
import { Switch } from 'react-router'
import { Route } from 'react-router-dom'
import { APP_NAME } from '../shared/config'
import Nav from './component/nav'
import HomePage from './component/page/home'
import HelloPage from './component/page/hello'
import HelloAsyncPage from './component/page/hello-async'
import NotFoundPage from './component/page/not-found'
import {
  HOME_PAGE_ROUTE,
  HELLO_PAGE_ROUTE,
  HELLO_ASYNC_PAGE_ROUTE,
} from '../shared/routes'

const App = () =>
  <div>
    <h1>{APP_NAME}</h1>
    <Nav />
    <Switch>
      <Route exact path={HOME_PAGE_ROUTE} render={() => <HomePage />} />
      <Route path={HELLO_PAGE_ROUTE} render={() => <HelloPage />} />
      <Route path={HELLO_ASYNC_PAGE_ROUTE} render={() => <HelloAsyncPage />} />
      <Route component={NotFoundPage} />
    </Switch>
  </div>

export default App
```

🏁 Run `yarn start` and `yarn dev:wds`. Open `http://localhost:8000`, and click on the links to navigate between our different pages. You should see the URL changing dynamically. Switch between different pages and use the back button of your browser to see that the browsing history is working as expected.
🏁 `yarn start` と `yarn dev:wds` を実行して `http://localhost:8000` を開いて，リンクをクリックしてページ間を移動してみましょう．URLが動的に変更されているのが分かると思います．ページ間をいくつか移動したらブラウザの戻るボタンを押してみて，閲覧履歴が期待通りに動いていることを確認してみてください．

Now, let's say you navigated to `http://localhost:8000/hello` this way. Hit the refresh button. You now get a 404, because our Express server only responds to `/`. As you navigated between pages, you were actually only doing it on the client-side. Let's add server-side rendering to the mix to get the expected behavior.
例えば，今 `http://localhost:8000/hello` に移動したとしましょう．更新ボタンを押してみてください．そうすると，404ページが表示されます．なぜなら現状ではExpressサーバーは`/`にだけ反応するようになっているからです．ページ間の移動がクライアント側でしか行われていなかったということです．それではサーバーサイドレンダリングを導入して，期待通りに動作になるようにしましょう．

## Server-Side Rendering
## サーバーサイドレンダリング

> 💡 **Server-Side Rendering** means rendering your app at the initial load of the page instead of relying on JavaScript to render it in the client's browser.
> 💡 **サーバーサイドレンダリング** はクライアント側のブラウザのJavaScriptによるページ描画に頼ることなく，最初にページを読み込んだ時点でアプリケーションが読み込まれることを意味します．

SSR is essential for SEO and provides a better user experience by showing the app to your users right away.
SSR (Server Side Rendering) はSEO対策には必須であり，素早くアプリケーションを表示することが出来るので，ユーザーエクスペリエンスが向上します．

The first thing we're going to do here is to migrate most of our client code to the shared / isomorphic / universal part of our codebase, since the server is now going to render our React app too.
まず最初にすることはクライアント側のコードの大部分を shared (isomorphic, universal) に移動させて，サーバー側でもReactアプリケーションを描画できるようにすることです．

### The big migration to `shared`

- Move all the files located under `client` to `shared`, except `src/client/index.jsx`.
- `src/client/index.jsx` 以外の `client` 以下のすべてのファイルを `shared` に移動します

We have to adjust a whole bunch of imports:
各importを対応させます:

- In `src/client/index.jsx`, replace the 3 occurrences of `'./app'` by `'../shared/app'`, and `'./reducer/hello'` by `'../shared/reducer/hello'`
- `src/client/index.jsx` では `'./app'` を `'../shared/app' `に， `'./reducer/hello'` を `'../shared/reducer/hello'` に変更します

- In `src/shared/app.jsx`, replace `'../shared/routes'` by `'./routes'` and `'../shared/config'` by `'./config'`
- `src/shared/app.jsx` では `'./routes'` を `'../shared/routes'` に `'./config'` を `'../shared/config'` に変更します

- In `src/shared/component/nav.jsx`, replace `'../../shared/routes'` by `'../routes'`
- `src/shared/component/nav.jsx` では `'../routes'` を `'../../shared/routes'` に変更します

### Server changes

- Create a `src/server/routing.js` file containing:
- `src/server/routing.js` を次のように作成します:

```js
// @flow

import {
  homePage,
  helloPage,
  helloAsyncPage,
  helloEndpoint,
} from './controller'

import {
  HOME_PAGE_ROUTE,
  HELLO_PAGE_ROUTE,
  HELLO_ASYNC_PAGE_ROUTE,
  helloEndpointRoute,
} from '../shared/routes'

import renderApp from './render-app'

export default (app: Object) => {
  app.get(HOME_PAGE_ROUTE, (req, res) => {
    res.send(renderApp(req.url, homePage()))
  })

  app.get(HELLO_PAGE_ROUTE, (req, res) => {
    res.send(renderApp(req.url, helloPage()))
  })

  app.get(HELLO_ASYNC_PAGE_ROUTE, (req, res) => {
    res.send(renderApp(req.url, helloAsyncPage()))
  })

  app.get(helloEndpointRoute(), (req, res) => {
    res.json(helloEndpoint(req.params.num))
  })

  app.get('/500', () => {
    throw Error('Fake Internal Server Error')
  })

  app.get('*', (req, res) => {
    res.status(404).send(renderApp(req.url))
  })

  // eslint-disable-next-line no-unused-vars
  app.use((err, req, res, next) => {
    // eslint-disable-next-line no-console
    console.error(err.stack)
    res.status(500).send('Something went wrong!')
  })
}
```

This file is where we deal with requests and responses. The calls to business logic are externalized to a different `controller` module.
このファイルでリクエストとレスポンスの処理を扱います．ここでは処理の呼び出しだけして，実際の処理の中身は `controller` に書きます．

**Note**: You will find a lot of React Router examples using `*` as the route on the server, leaving the entire routing handling to React Router. Since all requests go through the same function, that makes it inconvenient to implement MVC-style pages. Instead of doing that, we're here explicitly declaring the routes and their dedicated responses, to be able to fetch data from the database and pass it to a given page easily.
**Note**: 多くのReact Routerの例ではサーバー側のルートの定義で`*`を使用して，ルーティングの処理をReact Routerに全て任せています．それだと全てのリクエストが同じ関数を通ることになるので，MVCスタイルのページの実装には向いていません．なので，ここではデータベースから取得したデータをページに簡単に組み込むために，ルートの定義とそれに対するレスポンスを明示的に定義します．

- Create a `src/server/controller.js` file containing:
- `src/server/controller.js` を次のように作成します:

```js
// @flow

export const homePage = () => null

export const helloPage = () => ({
  hello: { message: 'Server-side preloaded message' },
})

export const helloAsyncPage = () => ({
  hello: { messageAsync: 'Server-side preloaded message for async page' },
})

export const helloEndpoint = (num: number) => ({
  serverMessage: `Hello from the server! (received ${num})`,
})
```

Here is our controller. It would typically make business logic and database calls, but in our case we just hard-code some results. Those results are passed back to the `routing` module to be used to initialize our server-side Redux store.
これがコントローラーです．本来ならここでビジネスロジックやデーターベースにアクセスする処理を書きますが，一旦ここでは期待される結果をハードコーディングします．これらの処理の結果は `routing` モジュールに渡され，サーバーサイドのReduxストアを初期化するのに使われます．

- Create a `src/server/init-store.js` file containing:
- `src/server/init-store.js` を次のように作成します:

```js
// @flow

import Immutable from 'immutable'
import { createStore, combineReducers, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'

import helloReducer from '../shared/reducer/hello'

const initStore = (plainPartialState: ?Object) => {
  const preloadedState = plainPartialState ? {} : undefined

  if (plainPartialState && plainPartialState.hello) {
    // flow-disable-next-line
    preloadedState.hello = helloReducer(undefined, {})
      .merge(Immutable.fromJS(plainPartialState.hello))
  }

  return createStore(combineReducers({ hello: helloReducer }),
    preloadedState, applyMiddleware(thunkMiddleware))
}

export default initStore
```

The only thing we do here, besides calling `createStore` and applying middleware, is to merge the plain JS object we received from the `controller` into a default Redux state containing Immutable objects.
ここでは単に，`createStore` を呼び出してミドルウェアを追加して，`controller` から受け取った生のJSオブジェクトを，ReduxストアのImmutableオブジェクトにマージしています．

- Edit `src/server/index.js` like so:
- `src/server/index.js` を次のように編集します:

```js
// @flow

import compression from 'compression'
import express from 'express'

import routing from './routing'
import { WEB_PORT, STATIC_PATH } from '../shared/config'
import { isProd } from '../shared/util'

const app = express()

app.use(compression())
app.use(STATIC_PATH, express.static('dist'))
app.use(STATIC_PATH, express.static('public'))

routing(app)

app.listen(WEB_PORT, () => {
  // eslint-disable-next-line no-console
  console.log(`Server running on port ${WEB_PORT} ${isProd ? '(production)' :
    '(development).\nKeep "yarn dev:wds" running in an other terminal'}.`)
})
```

Nothing special here, we just call `routing(app)` instead of implementing routing in this file.
特に変わったことはしていません．このファイルにルートを定義するのではなく，`routing(app)` を呼び出している点に注意して下さい．

- Rename `src/server/render-app.js` to `src/server/render-app.jsx` and edit it like so:
- `src/server/render-app.js` を `src/server/render-app.jsx` に名前を変更して次のように編集します:

```js
// @flow

import React from 'react'
import ReactDOMServer from 'react-dom/server'
import { Provider } from 'react-redux'
import { StaticRouter } from 'react-router'

import initStore from './init-store'
import App from './../shared/app'
import { APP_CONTAINER_CLASS, STATIC_PATH, WDS_PORT } from '../shared/config'
import { isProd } from '../shared/util'

const renderApp = (location: string, plainPartialState: ?Object, routerContext: ?Object = {}) => {
  const store = initStore(plainPartialState)
  const appHtml = ReactDOMServer.renderToString(
    <Provider store={store}>
      <StaticRouter location={location} context={routerContext}>
        <App />
      </StaticRouter>
    </Provider>)

  return (
    `<!doctype html>
    <html>
      <head>
        <title>FIX ME</title>
        <link rel="stylesheet" href="${STATIC_PATH}/css/style.css">
      </head>
      <body>
        <div class="${APP_CONTAINER_CLASS}">${appHtml}</div>
        <script>
          window.__PRELOADED_STATE__ = ${JSON.stringify(store.getState())}
        </script>
        <script src="${isProd ? STATIC_PATH : `http://localhost:${WDS_PORT}/dist`}/js/bundle.js"></script>
      </body>
    </html>`
  )
}

export default renderApp
```

`ReactDOMServer.renderToString` is where the magic happens. React will evaluate our entire `shared` `App`, and return a plain string of HTML elements. `Provider` works the same as on the client, but on the server, we wrap our app inside `StaticRouter` instead of `BrowserRouter`. In order to pass the Redux store from the server to the client, we pass it to `window.__PRELOADED_STATE__` which is just some arbitrary variable name.
`ReactDOMServer.renderToString` では魔法が起こっています．Reactは `shared` `App` を読み込んで，素のHTML文字列を返します．`Provider` はクライアント側と同じように動作しますが，サーバー側ではアプリケーション全体を `BrowserRouter` ではなく， `StaticRouter` で囲います．また，`window.__PRELOADED_STATE__` （変数名は何でも良い）を経由して，Reduxストアをサーバー側からクライアント側に渡しています．

**Note**: Immutable objects implement the `toJSON()` method which means you can use `JSON.stringify` to turn them into plain JSON strings.
**Note**: Immutableオブジェクトは `toJSON()` メソッドを実装しています．つまり `JSON.stringify` を使って素のJSON文字列を得ることが出来ます．

- Edit `src/client/index.jsx` to use that preloaded state:
- `src/client/index.jsx` を次のように編集して，渡されたReduxストアを使用します:

```js
import Immutable from 'immutable'
// [...]

/* eslint-disable no-underscore-dangle */
const composeEnhancers = (isProd ? null : window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__) || compose
const preloadedState = window.__PRELOADED_STATE__
/* eslint-enable no-underscore-dangle */

const store = createStore(combineReducers(
  { hello: helloReducer }),
  { hello: Immutable.fromJS(preloadedState.hello) },
  composeEnhancers(applyMiddleware(thunkMiddleware)))
```

Here with feed our client-side store with the `preloadedState` that was received from the server.
ここではサーバー側から渡された `preloadedState` をクライアント側のストアに渡しています．

🏁 You can now run `yarn start` and `yarn dev:wds` and navigate between pages. Refreshing the page on `/hello`, `/hello-async`, and `/404` (or any other URI), should now work correctly. Notice how the `message` and `messageAsync` vary depending on if you navigated to that page from the client or if it comes from server-side rendering.
🏁 `yarn start` と `yarn dev:wds` を実行してページ間を移動してみましょう．`/hello` や， `/hello-async` や， `/404` （または適当なURL）でページを更新してみましょう．今度は正しく動作するはずです．クライアント側でページ間を移動してきたか，サーバーサイドレンダリングによって読み込まれたかによって `message` と `messageAsync` の動作が異なっていることに注目して下さい．

### React Helmet

> 💡 **[React Helmet](https://github.com/nfl/react-helmet)**: A library to inject content to the `head` of a React app, on both the client and the server.
> 💡 **[React Helmet](https://github.com/nfl/react-helmet)**: Reactアプリケーションの `head` の中に項目を挿入するライブラリです．クライアント側とサーバー側の両方で動作します．

I purposely made you write `FIX ME` in the title to highlight the fact that even though we are doing server-side rendering, we currently do not fill the `title` tag properly (or any of the tags in `head` that vary depending on the page you're on).
サーバーサイドレンダリングをしている一方で，`title`タグ（またはページ間で動的に変わる全ての`head`項目）を正しく書き換えていなかったので，そこに注目するためにあえて`FIX ME`と書いていました．

- Run `yarn add react-helmet`
- `yarn add react-helmet` を実行します

- `src/server/render-app.jsx` を次のように編集します:

```js
import Helmet from 'react-helmet'
// [...]
const renderApp = (/* [...] */) => {

  const appHtml = ReactDOMServer.renderToString(/* [...] */)
  const head = Helmet.rewind()

  return (
    `<!doctype html>
    <html>
      <head>
        ${head.title}
        ${head.meta}
        <link rel="stylesheet" href="${STATIC_PATH}/css/style.css">
      </head>
    [...]
    `
  )
}
```

React Helmet uses [react-side-effect](https://github.com/gaearon/react-side-effect)'s `rewind` to pull out some data from the rendering of our app, which will soon contain some `<Helmet />` components. Those `<Helmet />` components are where we set the `title` and other `head` details for each page.
React Helmet は [react-side-effect](https://github.com/gaearon/react-side-effect) の `rewind` を使って `<Helmet />` を含むレンダリング結果からデータを抜き出します．この `<Helmet />` コンポーネントの中に各ページ毎の `title` や他の `head` 項目が含まれています．

- `src/shared/app.jsx` を次のように編集します:

```js
import Helmet from 'react-helmet'
// [...]
const App = () =>
  <div>
    <Helmet titleTemplate={`%s | ${APP_NAME}`} defaultTitle={APP_NAME} />
    <Nav />
    // [...]
```

- `src/shared/component/page/home.jsx` を次のように編集します:

```js
// @flow

import React from 'react'
import Helmet from 'react-helmet'

import { APP_NAME } from '../../config'

const HomePage = () =>
  <div>
    <Helmet
      meta={[
        { name: 'description', content: 'Hello App is an app to say hello' },
        { property: 'og:title', content: APP_NAME },
      ]}
    />
    <h1>{APP_NAME}</h1>
  </div>

export default HomePage

```

- `src/shared/component/page/hello.jsx` を次のように編集します:

```js
// @flow

import React from 'react'
import Helmet from 'react-helmet'

import HelloButton from '../../container/hello-button'
import Message from '../../container/message'

const title = 'Hello Page'

const HelloPage = () =>
  <div>
    <Helmet
      title={title}
      meta={[
        { name: 'description', content: 'A page to say hello' },
        { property: 'og:title', content: title },
      ]}
    />
    <h1>{title}</h1>
    <Message />
    <HelloButton />
  </div>

export default HelloPage
```

- `src/shared/component/page/hello-async.jsx` を次のように編集します:

```js
// @flow

import React from 'react'
import Helmet from 'react-helmet'

import HelloAsyncButton from '../../container/hello-async-button'
import MessageAsync from '../../container/message-async'

const title = 'Async Hello Page'

const HelloAsyncPage = () =>
  <div>
    <Helmet
      title={title}
      meta={[
        { name: 'description', content: 'A page to say hello asynchronously' },
        { property: 'og:title', content: title },
      ]}
    />
    <h1>{title}</h1>
    <MessageAsync />
    <HelloAsyncButton />
  </div>

export default HelloAsyncPage

```

- `src/shared/component/page/not-found.jsx` を次のように編集します:

```js
// @flow

import React from 'react'
import Helmet from 'react-helmet'

const title = 'Page Not Found'

const NotFoundPage = () =>
  <div>
    <Helmet
      title={title}
      meta={[
        { name: 'description', content: 'A page to say hello' },
        { property: 'og:title', content: title },
      ]}
    />
    <h1>{title}</h1>
  </div>

export default NotFoundPage
```

The `<Helmet>` component doesn't actually render anything, it just injects content in the `head` of your document and exposes the same data to the server.
`<Helmet>` コンポーネントは単に `head` の項目をレンダリング結果に挿入します．こうすることでサーバー側でも `head` の項目を動的に変更することが出来ます．

🏁 Run `yarn start` and `yarn dev:wds` and navigate between pages. The title on your tab should change when you navigate, and it should also stay the same when you refresh the page. Show the source of the page to see how React Helmet sets the `title` and `meta` tags even for server-side rendering.
🏁 `yarn start` と `yarn dev:wds` を実行して，ページ間を移動してみましょう． ブラウザのタブを見てページを移動するたびにタイトルが変わっていることや，更新しても同じページが表示されることを確認しましょう．ページのソースを開いて，サーバーサイドレンダリングにおいて `title` や `meta` が正しく挿入されていることを確認しましょう．

Next section: [07 - Socket.IO](07-socket-io.md#readme)
次の章: [07 - Socket.IO](07-socket-io.md#readme)

Back to the [previous section](05-redux-immutable-fetch.md#readme) or the [table of contents](https://github.com/verekia/js-stack-from-scratch#table-of-contents).
[前の章](05-redux-immutable-fetch.md#readme)に戻る．[目次](https://github.com/verekia/js-stack-from-scratch#table-of-contents)に戻る．
