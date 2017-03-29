# JavaScript Stack from Scratch
# ゼロから作るJavaScriptスタック

[![Build Status](https://travis-ci.org/verekia/js-stack-from-scratch.svg?branch=master)](https://travis-ci.org/verekia/js-stack-from-scratch)
[![Release](https://img.shields.io/github/release/verekia/js-stack-from-scratch.svg?style=flat-square)](https://github.com/verekia/js-stack-from-scratch/releases)
[![Dependencies](https://img.shields.io/david/verekia/js-stack-boilerplate.svg?style=flat-square)](https://david-dm.org/verekia/js-stack-boilerplate)
[![Dev Dependencies](https://img.shields.io/david/dev/verekia/js-stack-boilerplate.svg?style=flat-square)](https://david-dm.org/verekia/js-stack-boilerplate?type=dev)
[![Gitter](https://img.shields.io/gitter/room/js-stack-from-scratch/Lobby.svg?style=flat-square)](https://gitter.im/js-stack-from-scratch/)

[![React](/img/react-padded-90.png)](https://facebook.github.io/react/)
[![Redux](/img/redux-padded-90.png)](http://redux.js.org/)
[![React Router](/img/react-router-padded-90.png)](https://github.com/ReactTraining/react-router)
[![Flow](/img/flow-padded-90.png)](https://flowtype.org/)
[![ESLint](/img/eslint-padded-90.png)](http://eslint.org/)
[![Jest](/img/jest-padded-90.png)](https://facebook.github.io/jest/)
[![Yarn](/img/yarn-padded-90.png)](https://yarnpkg.com/)
[![Webpack](/img/webpack-padded-90.png)](https://webpack.github.io/)
[![Bootstrap](/img/bootstrap-padded-90.png)](http://getbootstrap.com/)

Welcome to my modern JavaScript stack tutorial: **JavaScript Stack from Scratch**.
モダンJavaScriptスタックチュートリアル、**ゼロから作るJavaScriptスタック**へようこそ。

> 🎉 **This is the V2 of the tutorial, major changes happened since the 2016 release. Check the [Change Log](/CHANGELOG.md)!**
> 🎉 **これはチュートリアルのV2であり、2016年のリリースから大きく変更されています。 [Change Log](/CHANGELOG.md)を確認してください！**

This is a straight-to-the-point guide to assembling a JavaScript stack. It requires some general programming knowledge, and JavaScript basics. **It focuses on wiring tools together** and giving you the **simplest possible example** for each tool. You can see this tutorial as *a way to write your own boilerplate from scratch*. Since the goal of this tutorial is to assemble various tools, I do not go into details about how these tools work individually. Refer to their documentation or find other tutorials if you want to acquire deeper knowledge in them.
これは、JavaScriptスタック（訳注：JavaScriptスタックとはReact, Babel, WebpackなどReactを使ったWebアプリを作成するのに必要な一連のツールのことです）を組み立てるための指針です。一般的なプログラミング知識とJavaScriptの基礎が必要です。ツールを組み立てることに焦点を当て、各ツールで最も簡単な例を紹介します。このチュートリアルは、ボイラープレート（訳注：ボイラープレートとは、JavaScriptスタックと必要最低限の機能を有するひな形となるプロジェクトのことです）を一から自作する方法として参照できます。このチュートリアルの目的はさまざまなツールを組み立てることなので、これらのツールが個別にどのように機能するかについて詳しくは説明しません。より深い知識を習得したい場合は、そのドキュメントを参照するか、他のチュートリアルを参照してください。

You don't need to use this entire stack if you build a simple web page with a few JS interactions of course (a combination of Browserify/Webpack + Babel + jQuery is enough to be able to write ES6 code in different files), but if you want to build a web app that scales, and need help setting things up, this tutorial will work great for you.
小さくてシンプルで動的なWebページをJSで書きたいといった場合にはここで紹介する全てのスタックを使う必要はありません。（ES6コードを少し書きたい程度であれば、Browserify / Webpack + Babel + jQueryで十分です）。拡張性のあるWebアプリケーションを構築したくて、セットアップに困っている場合は、このチュートリアルを参考にしてください。

A big chunk of the stack described in this tutorial uses React. If you are beginning and just want to learn React, [create-react-app](https://github.com/facebookincubator/create-react-app) will get you up and running with a React environment very quickly with a pre-made configuration. I would for instance recommend this approach to someone who arrives in a team that's using React and needs to catch up with a learning playground. In this tutorial you won't use a pre-made configuration, because I want you to understand everything that's happening under the hood.
ここで紹介しているほとんどのスタックはReactを使用しています。もしReactに関してあまり知らなくて、単にReactを学びたいという場合は [create-react-app](https://github.com/facebookincubator/create-react-app) をおすすめします。これを使うと構築済み設定ファイルと共にReactの環境が一瞬で構築できます。例えばReactを使うチームにアサインされてキャッチアップするためにReactをいじれる環境が欲しいといった場合にはそちらをおすすめします。このチュートリアルでは皆さんに何が起こっているかをゼロから理解して欲しいので、構築済みの設定は使用しません。

Code examples are available for each chapter, and you can run them all with `yarn && yarn start`. I recommend writing everything from scratch yourself by following the **step-by-step instructions** though.
各章に対応したコードがあり、全て `yarn && yarn start` で実行することができます。とはいえ、**ステップバイステップの指示**に従ってご自身でゼロから全て書いてみることをおすすめします。

Final code available in the [JS-Stack-Boilerplate repository](https://github.com/verekia/js-stack-boilerplate), and in the [releases](https://github.com/verekia/js-stack-from-scratch/releases). There is a [live demo](https://js-stack.herokuapp.com/) too.
最終的なコードは [JS-Stack-Boilerplate repository](https://github.com/verekia/js-stack-boilerplate) と、 [releases](https://github.com/verekia/js-stack-from-scratch/releases) にあります。[ライブデモ](https://js-stack.herokuapp.com/) もあるので、もしよければ見てみて下さい。

Works on Linux, macOS, and Windows.
Linux, MacOS, Windowsで動作します。

## Table of contents

[01 - Node, Yarn, `package.json`](/tutorial/01-node-yarn-package-json.md#readme)

[02 - Babel, ES6, ESLint, Flow, Jest, Husky](/tutorial/02-babel-es6-eslint-flow-jest-husky.md#readme)

[03 - Express, Nodemon, PM2](/tutorial/03-express-nodemon-pm2.md#readme)

[04 - Webpack, React, HMR](/tutorial/04-webpack-react-hmr.md#readme)

[05 - Redux, Immutable, Fetch](/tutorial/05-redux-immutable-fetch.md#readme)

[06 - React Router, Server-Side Rendering, Helmet](/tutorial/06-react-router-ssr-helmet.md#readme)

[07 - Socket.IO](/tutorial/07-socket-io.md#readme)

[08 - Bootstrap, JSS](/tutorial/08-bootstrap-jss.md#readme)

[09 - Travis, Coveralls, Heroku](/tutorial/09-travis-coveralls-heroku.md#readme)

## Coming up next
## 次にやること

Setting up your editor (Atom first), MongoDB, Progressive Web App.
テキストエディタ（まずはAtom）、MongoDB、Progressig Web Appの設定

## Translations

If you want to add your translation, please read the [translation recommendations](/how-to-translate.md) to get started!

### V2

Your link here soon ;)

Check out the [ongoing translations](https://github.com/verekia/js-stack-from-scratch/issues/147).

### V1

- [中文](https://github.com/pd4d10/js-stack-from-scratch) by [@pd4d10](http://github.com/pd4d10)
- [Italiano](https://github.com/fbertone/js-stack-from-scratch) by [Fabrizio Bertone](https://github.com/fbertone)
- [日本語](https://github.com/takahashim/js-stack-from-scratch) by [@takahashim](https://github.com/takahashim)
- [Русский](https://github.com/UsulPro/js-stack-from-scratch) by [React Theming](https://github.com/sm-react/react-theming)
- [ไทย](https://github.com/MicroBenz/js-stack-from-scratch) by [MicroBenz](https://github.com/MicroBenz)

## Credits

Created by [@verekia](https://twitter.com/verekia) – [verekia.com](http://verekia.com/).

License: MIT
