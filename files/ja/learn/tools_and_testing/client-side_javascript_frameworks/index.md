---
title: クライアントサイドの JavaScript フレームワークを理解する
slug: Learn/Tools_and_testing/Client-side_JavaScript_frameworks
tags:
  - Beginner
  - Frameworks
  - JavaScript
  - Learn
  - NeedsTranslation
  - TopicStub
  - client-side
translation_of: Learn/Tools_and_testing/Client-side_JavaScript_frameworks
---
<div>{{LearnSidebar}}</div>

<p class="summary">JavaScript フレームワークは、最新のフロントエンドウェブ開発に欠かせないものであり、開発者にスケーラブルでインタラクティブなウェブアプリケーションを構築するための試行錯誤されたツールを提供しています。現代の多くの企業では、フレームワークをツールの標準的な一部として使用しているため、多くのフロントエンド開発の仕事でフレームワークの経験が必要とされています。</p>

<p class="summary">フロントエンド開発者を目指していると、フレームワークを学ぶ際にどこから始めればいいのか悩むことがあります。フレームワークは常に新しいものが登場するため、非常に多くの種類の中から選ぶことができます。これらはほとんど同じように動作しますが、いくつかの点において異なっています。そしてフレームワークを利用する上では、具体的に注意しなければならないこともあります。</p>

<p class="summary">この記事では、あなたがフレームワークを学び始めるための快適な出発点を提供することを目的としています。私たちは、React/ReactDOM や Vue、その他の特定のフレームワークについて知っておく必要があるすべてのことを網羅的に教えることを目指しているわけではありません。その代わりに、以下のようなより基本的な質問に答えたいと思います。</p>

<ul>
 <li class="summary">なぜフレームワークを使うべきなのか？どんな問題を解決してくれるのか？</li>
 <li class="summary">フレームワークを選ぼうとするとき、どのような質問をすればいいか？フレームワークを使う必要があるのか？</li>
 <li class="summary">フレームワークにはどのような機能があるのか？フレームワークは一般的にどのように機能するのか、そしてその機能の実装はどのように異なるのか？</li>
 <li class="summary">これらは素の JavaScript や HTML とどのように関係しているのか？</li>
</ul>

<p class="summary">その後、異なるフレームワークの選択の要点をカバーするチュートリアルをいくつか提供し、あなたが自分自身でより深く学び始めるのに十分なコンテキストと親しみやすさを提供します。アクセシビリティなどのウェブプラットフォームの基本的なベストプラクティスを忘れずに、実用的な方法でフレームワークについて学んでいただきたいと思います。</p>

<p class="summary"><strong><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction">Get started now, with "Introduction to client-side frameworks"</a></strong></p>

<h2 id="Prerequisites">前提となる知識</h2>

<p>クライアントサイドのフレームワークを学習する前に、ウェブ開発で用いる主要な言語の基礎を学ぶ必要があります。それは <a href="/ja/docs/Learn/HTML">HTML</a> と <a href="/ja/docs/Learn/CSS">CSS</a>、そして特に <a href="/ja/docs/Learn/JavaScript">JavaScript</a> です。</p>

<p>ウェブプラットフォームの基本的な機能を理解すれば、その上で構築されているフレームワークのトラブルも自信を持って解決できるでしょう。そうすれば、あなたの書くコードはより豪華でプロフェッショナルなものになるはずです。</p>

<h2 id="Introductory_guides">入門ガイド</h2>

<dl>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction">1. Introduction to client-side frameworks</a></dt>
 <dd>We begin our look at frameworks with a general overview of the area, looking at a brief history of JavaScript and frameworks, why frameworks exist and what they give us, how to start thinking about choosing a framework to learn, and what alternatives there are to client-side frameworks.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Main_features">2. Framework main features</a></dt>
 <dd>Each major JavaScript framework has a different approach to updating the DOM, handling browser events, and providing an enjoyable developer experience. This article will explore the main features of “the big 4” frameworks, looking at how frameworks tend to work from a high level and the differences between them.</dd>
</dl>

<h2 id="React_tutorials">React チュートリアル</h2>

<div class="blockIndicator note">
<p><strong>注</strong>: この React チュートリアルは React/ReactDOM 16.13.1 と create-react-app 3.4.1 で、2020年5月に動作確認を行いました。</p>

<p>もし、あなたのコードとサンプルのバージョンとを確認する必要があれば、<a href="https://github.com/mdn/todo-react">todo-react repository</a> で最新版を見ることができます。実行中のライブバージョンについては、<a href="https://mdn.github.io/todo-react-build/">https://mdn.github.io/todo-react-build/</a> から確認ができます。</p>
</div>

<dl>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_getting_started">1. React をはじめる</a></dt>
 <dd>この記事では React のはじめかたを説明します。React の背景と使い方について説明し、ローカル環境で基本的な React ツールチェーンを設定します。また、シンプルな基本アプリを作成して、React がどのようなプロセスで機能するのかを学んでいきます。</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_todo_list_beginning">2. React ToDo リストをはじめる</a></dt>
 <dd>例えば、React でアイディアを検証するためにアプリを試作してみることになったとします(いわゆる、Proof of Concept - 概念実証)。ユーザーが作業したいタスクを追加、編集、削除することができ、また、タスクを削除せずに完了とすることができるアプリです。この記事では、基本的な App コンポーネントの構造とスタイルを整え、後から追加する個々のコンポーネントの定義とインタラクティブな機能の準備を行っていきます。</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_components">3. React アプリのコンポーネント化</a></dt>
 <dd>この時点では、アプリは一枚岩です。アプリに何かをさせる前に、管理しやすく、記述しやすいコンポーネントに分解する必要があります。React には、何がコンポーネントで何がコンポーネントでないかという難しいルールはありません。それはあなた次第なのです！この記事では、アプリをコンポーネントに分解するための賢明な方法を紹介します。</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_interactivity_events_state">4. React interactivity: Events and state</a></dt>
 <dd>With our component plan worked out, it's now time to start updating our app from a completely static UI to one that actually allows us to interact and change things. In this article we'll do this, digging into events and state along the way.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_interactivity_filtering_conditional_rendering">5. React interactivity: Editing, filtering, conditional rendering</a></dt>
 <dd>As we near the end of our React journey (for now at least), we'll add the finishing touches to the main areas of functionality in our Todo list app. This includes allowing you to edit existing tasks and filtering the list of tasks between all, completed, and incomplete tasks. We'll look at conditional UI rendering along the way.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_accessibility">6. Accessibility in React</a></dt>
 <dd>In our final tutorial article, we'll focus on (pun intended) accessibility, including focus management in React, which can improve usability and reduce confusion for both keyboard-only and screen reader users.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_resources">7. React resources</a></dt>
 <dd>Our final article provides you with a list of React resources that you can use to go further in your learning.</dd>
</dl>

<h2 id="Ember_tutorials">Ember チュートリアル</h2>

<div class="blockIndicator note">
<p><strong>注</strong>: この Ember チュートリアルは Ember/Ember CLI version 3.18.0 で、2020年5月に動作確認を行いました。</p>

<p>もし、あなたのコードとサンプルのバージョンとを確認する必要があれば、<a href="https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc">ember-todomvc-tutorial repository</a> で最新版を見ることができます。実行中のライブバージョンについては <a href="https://nullvoxpopuli.github.io/ember-todomvc-tutorial/">https://nullvoxpopuli.github.io/ember-todomvc-tutorial/</a> から確認ができます(ただしチュートリアルで取り扱っていない機能も含まれています)。</p>
</div>

<dl>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_getting_started">1. Getting started with Ember</a></dt>
 <dd>In our first Ember article we will look at how Ember works and what it's useful for, install the Ember toolchain locally, create a sample app, and then do some initial setup to get it ready for development.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_structure_componentization">2. Ember app structure and componentization</a></dt>
 <dd>In this article we'll get right on with planning out the structure of our TodoMVC Ember app, adding in the HTML for it, and then breaking that HTML structure into components.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_interactivity_events_state">3. Ember interactivity: Events, classes and state</a></dt>
 <dd>At this point we'll start adding some interactivity to our app, providing the ability to add and display new todo items. Along the way, we'll look at using events in Ember, creating component classes to contain JavaScript code to control interactive features, and setting up a service to keep track of the data state of our app.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_conditional_footer">4. Ember Interactivity: Footer functionality, conditional rendering</a></dt>
 <dd>Now it's time to start tackling the footer functionality in our app. Here we'll get the todo counter to update to show the correct number of todos still to complete, and correctly apply styling to completed todos (i.e. where the checkbox has been checked). We'll also wire up our "Clear completed" button. Along the way, we'll learn about using conditional rendering in our templates.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_routing">5. Routing in Ember</a></dt>
 <dd>In this article we learn about routing or URL-based filtering as it is sometimes referred to. We'll use it to provide a unique URL for each of the three todo views — "All", "Active", and "Completed".</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Ember_resources">6. Ember resources and troubleshooting</a></dt>
 <dd>Our final Ember article provides you with a list of resources that you can use to go further in your learning, plus some useful troubleshooting and other information.</dd>
</dl>

<h2 id="Vue_tutorials">Vue チュートリアル</h2>

<div class="blockIndicator note">
<p><strong>注</strong>: この Vue チュートリアルは Vue 2.6.11 で、2020年5月に動作確認を行いました。</p>

<p>もし、あなたのコードとサンプルのバージョンとを確認する必要があれば、 <a href="https://github.com/mdn/todo-vue">todo-vue repository</a> で最新版を見ることができます。実行中のライブバージョンについては <a href="https://mdn.github.io/todo-vue/dist/">https://mdn.github.io/todo-vue/dist/</a> から確認ができます。</p>
</div>

<dl>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_getting_started">1. Getting started with Vue</a></dt>
 <dd>Now let's introduce Vue, the third of our frameworks. In this article, we'll look at a little bit of Vue background, learn how to install it and create a new project, study the high-level structure of the whole project and an individual component, see how to run the project locally, and get it prepared to start building our example.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_first_component">2. Creating our first Vue component</a></dt>
 <dd>Now it's time to dive deeper into Vue, and create our own custom component — we'll start by creating a component to represent each item in the todo list. Along the way, we'll learn about a few important concepts such as calling components inside other components, passing data to them via props and saving data state.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_rendering_lists">3. Rendering a list of Vue components</a></dt>
 <dd><span class="author-d-1gg9uz65z1iz85zgdz68zmqkz84zo2qoxwoxz78zz83zz84zz69z2z80zgwxsgnz83zfkt5e5tz70zz68zmsnjz122zz71z">At this point we've got a fully working component; we're now ready to add multiple <code>ToDoItem</code> components to our App. In this article we'll look at adding a set of todo item data to our <code>App.vue</code> component, which we'll then loop through and display inside <code>ToDoItem</code> components using the <code>v-for</code> directive. </span></dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_methods_events_models">4. Adding a new todo form: Vue events, methods, and models</a></dt>
 <dd>We now have sample data in place and a loop that takes each bit of data and renders it inside a <code>ToDoItem</code> in our app. What we really need next is the ability to allow our users to enter their own todo items into the app, and for that, we'll need a text <code>&lt;input&gt;</code>, an event to fire when the data is submitted, a method to fire upon submission to add the data and rerender the list, and a model to control the data. This is what we'll cover in this article.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_styling">5. Styling Vue components with CSS</a></dt>
 <dd>The time has finally come to make our app look a bit nicer. In this article, we'll explore the different ways of styling Vue components with CSS.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_computed_properties">6. Using Vue computed properties</a></dt>
 <dd>In this article we'll add a counter that displays the number of completed todo items, using a feature of Vue called computed properties. These work similarly to methods but only re-run when one of their dependencies changes.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_conditional_rendering">7. Vue conditional rendering: editing existing todos</a></dt>
 <dd>Now it is time to add one of the major parts of functionality that we're still missing — the ability to edit existing todo items. To do this, we will take advantage of Vue's conditional rendering capabilities — namely <code>v-if</code> and <code>v-else</code> — to allow us to toggle between the existing todo item view and an edit view where you can update todo item labels. We'll also look at adding functionality to delete todo items.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_refs_focus_management">8. Focus management with Vue refs</a></dt>
 <dd>We are nearly done with Vue. The last bit of functionality to look at is focus management, or put another way, how we can improve our app's keyboard accessibility. We'll look at using Vue refs to handle this — an advanced feature that allows you to have direct access to the underlying DOM nodes below the virtual DOM, or direct access from one component to the internal DOM structure of a child component.</dd>
 <dt><a href="/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_resources">9. Vue resources</a></dt>
 <dd>Now we'll round off our study of Vue by giving you a list of resources that you can use to go further in your learning, plus some other useful tips.</dd>
</dl>

<h2 id="Which_frameworks_did_we_choose">どのフレームワークを選ぶべきか？</h2>

<p>ここでは主要なフレームワークである３つに焦点を当てて、そのガイトを取り扱いました。React/ReactDOM、Ember、Vue です。これらを取り上げた理由は以下の通りです。</p>

<ul>
 <li>どのようなツールでもそうですが、積極的に開発がなされ、廃止される可能性がないもの、また就職活動でも助けになるスキルを選ぶことが望ましい。</li>
 <li>初心者が複雑なものを学ぶ際に手助けを得られることは非常に重要であるため、活発なコミュニティと優れたドキュメントがあること。</li>
 <li>モダンなフレームワークの<strong>すべて</strong>を網羅するリソースはありません。常に新たなフレームワークが登場するため、このリストをその度に更新することは非常に困難です。</li>
 <li>初心者は数ある選択肢の中から、何を選べば良いのかを悩みます。そのため、リストを短くすると役に立ちます。</li>
</ul>

<p>前もって言っておきますが、取り上げたフレームワークは私たちが<strong>ベストだと思うから選んだわけではありません</strong>。また推奨しているわけでもありません。ただ、上記の基準を満たしていると考えただけです。 </p>

<p>初期の段階では、もっと多くのフレームワークを取り上げたいと考えていましたが、このコンテンツの公開を遅らせることよりも、後からフレームワークを追加する方が良いと考えました。もし、あなたのお気に入りのフレームワークがこのコンテンツに含まれておらず、それを変える手助けをしたいのであれば、私たちと気軽に意見を交わしましょう！<a href="https://wiki.mozilla.org/Matrix">Matrix</a> や <a href="https://discourse.mozilla.org/c/mdn">Discourse</a> を通じて私たちと連絡を取るか、<a href="mailto:mdn-admins@mozilla.org">mdn-admins list</a> にメールを送ってください。</p>