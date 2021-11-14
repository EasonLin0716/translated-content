---
title: Introduction à Express/Node
slug: Learn/Server-side/Express_Nodejs/Introduction
tags:
  - Beginner
  - CodingScripting
  - Express
  - Learn
  - Node
  - nodejs
  - server-side
translation_of: Learn/Server-side/Express_Nodejs/Introduction
---
<div>{{LearnSidebar}}</div>

<div>{{NextMenu("Learn/Server-side/Express_Nodejs/development_environment", "Learn/Server-side/Express_Nodejs")}}</div>

<p>Dans ce tout premier article consacré à Express, nous répondons aux questions «&nbsp;Qu'est-ce que Node&nbsp;?&nbsp;» et «&nbsp;Qu'est-ce que Express ?&nbsp;», et vous donnons un aperçu de ce qui fait d'Express un framework web si spécial. Nous décrivons les principales fonctionnalités et montrons quelques-uns des principaux composants d'une application Express (bien que vous ne disposiez pas encore d'un environnement de développement pour le tester).</p>

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prérequis&nbsp;:</th>
      <td>Une culture de base en informatique, une compréhension globale de la <a href="/fr/docs/Learn/Server-side/First_steps">programmation côté serveur</a> et, en particulier, les mécanismes d'<a href="/fr/docs/Learn/Server-side/First_steps/Client-Server_overview">interactions client-serveur dans un site web.</a></td>
    </tr>
    <tr>
      <th scope="row">Objectif&nbsp;:</th>
      <td>Devenir familier avec ce qu'est Express et comment il s'intègre dans Node, les fonctionnalités qu'il apporte, et les principales étapes pour construire une application Express.</td>
    </tr>
  </tbody>
</table>

<h2 id="introducing_node">Introduction à Node</h2>

<p><a href="https://nodejs.org/">Node</a> (ou plus formellement <em>Node.js</em>) est un environnement d'exécution open-source, multi-plateforme, qui permet aux développeuses et développeurs de créer toutes sortes d'applications et d'outils côté serveur en <a href="/fr/docs/Glossary/JavaScript">JavaScript</a>. Cet environnement est destiné à être utilisé en dehors du navigateur (il s'exécute directement sur son ordinateur ou dans le système d'exploitation du serveur). Aussi, Node ne permet pas d'utiliser les API JavaScript liées au navigateur mais des API plus traditionnellement utilisées sur un serveur dont notamment celles pour HTTP ou la manipulation de systèmes de fichier.</p>

<p>Dans une perspective de développement de serveur web, Node présente un certain nombre d'avantages&nbsp;:</p>

<ul>
  <li>D'excellentes performances ! Node a été créé pour optimiser le rendement et l'évolution des applications web. Il s'agit d'une bonne solution à de nombreux problèmes de développement web (par exemple, les applications en temps réel).</li>
  <li>Le code est intégralement écrit en JavaScript ce qui signifie que l'on dépense moins d'énergie à basculer d'un langage à l'autre quand on code côté client et côté serveur.</li>
  <li>Le JavaScript est un langage de programmation plutôt récent et bénéficie encore d'améliorations dans sa conception en comparaison à d'autres langages web côté serveur (Python, PHP, etc.). Beaucoup d'autres langages nouveaux et populaires compilent/convertissent en JavaScript pour pouvoir utiliser TypeScript, CoffeeScript, ClojureScript, Scala, LiveScript, etc.</li>
  <li>Le gestionnaire de paquets (NPM) offre l'accès à des milliers de bibliothèques réutilisables. Il dispose d'une excellente résolution de dépendances et peut être utilisé pour automatiser la plupart des chaines de compilation.</li>
  <li>Node.js est portable. Il est disponible sous Microsoft Windows, macOS, Linux, etc. De plus, il est bien supporté par beaucoup d'hébergeurs web qui fournissent souvent une infrastructure spécifique et une documentation pour héberger des sites Node.</li>
  <li>Node possède une communauté et un écosystème très dynamiques eavec beaucoup de gens désireux d'aider.</li>
</ul>

<p>Vous pouvez utiliser Node.js pour créer un simple serveur web en utilisant l'API Node HTTP.</p>

<h3 id="hello_node.js">Hello Node.js</h3>

<p>L'exemple qui suit crée un serveur web qui écoute toutes sortes de requêtes HTTP sur l'URL <code>https://127.0.0.1:8000/</code>. Quand une requête est reçue, le script répond avec la chaine « Salut tout le monde ». Si vous avez déjà installé Node, suivez les étapes de l'exemple suivant :</p>

<ol>
  <li>Ouvrez un terminal (sur Windows, ouvrez l'invite de commande (cmd)),</li>
  <li>Créez le dossier où vous voulez sauvegarder le programme, appelez-le par exemple <code>test-node</code> et placez-vous dedans en utilisant la commande suivante dans votre console :
    <pre>cd test-node</pre></li>
  <li>Dans votre éditeur de texte favori, créez un fichier nommé <code>"hello.js"</code> et collez ce qui suit dedans :
    <pre class="brush: js">// Charge le module HTTP
const http = require("http");

const hostname = "127.0.0.1";
const port = 8000;

// Crée un serveur HTTP
const server = http.createServer((req, res) => {

  // Configure l'en-tête de la réponse HTTP
  // avec le code du statut et le type de contenu
  res.writeHead(200, {'Content-Type': 'text/plain'});

  // Envoie le corps de la réponse « Salut tout le monde »
   res.end('Salut tout le monde\n');
})

// Démarre le serveur à l'adresse 127.0.0.1 sur le port 8000
// Affiche un message dès que le serveur commence à écouter les requêtes
server.listen(port, hostname, () =&gt; {
  console.log(`Le serveur tourne à l'adresse https://${hostname}:${port}/`);
})</pre></li>
  <li>Sauvegardez le fichier dans le dossier créé plus haut.</li>
  <li>Retournez au terminal et tapez :
    <pre class="brush: bash">node hello.js</pre>
  </li>
</ol>

<p>Puis saisissez l'URL <code>"https://localhost:8000"</code> dans votre navigateur. Vous devriez alors voir "<strong>Salut tout le monde</strong>" en haut à gauche d'une page web ne contenant rien d'autre que ce texte.</p>

<h2 id="web_frameworks">Les frameworks web</h2>

<p>D'autres tâches de développement web ne sont pas directement prises en charge par Node de façon native. Si vous voulez ajouter différentes manipulations pour divers requêtes HTTP (<code>GET</code>, <code>POST</code>, <code>DELETE</code>, etc.), gérer différemment des requêtes vers plusieurs chemins URL ("routes"), servir des pages statiques ou utiliser des modèles pour créer dynamiquement la réponse,  alors vous devrez écrire tout le code vous-même ou, pour éviter de réinventer la roue, vous servir des cadres applicatifs web (frameworks).</p>

<h2 id="introducing_express">Introduction à Express</h2>

<p><a href="https://expressjs.com/">Express</a> est le <i>framework</i> actuellement le plus populaire dans Node et est la bibliothèque sous-jacente pour un grand nombre d'autres <a href="https://expressjs.com/fr/resources/frameworks.html">cadres applicatifs web pour Node</a>. Il fournit des mécanismes pour :</p>

<ul>
  <li>Écrire des fonctions de traitement pour différentes requêtes HTTP répondant à différentes URI (par le biais des <em>routes</em>).</li>
  <li>Intégrer avec les moteurs de rendu de « vues » dans le but de générer des réponses en insérant des données dans des modèles (« <i>templates</i> »). Configurer certains paramètres d'applications comme le port à utiliser à la connexion et l'emplacement des modèles nécessaires pour la mise en forme de la réponse.</li>
  <li>Ajouter des requêtes de traitement « <i>middleware</i> » (intergiciel) où vous le voulez dans le tunnel gestionnaire de la requête.</li>
</ul>

<p>Bien qu'Express soit assez minimaliste, des <em>middlewares</em> (fonctions intermédiaires) compatibles ont été créés pour résoudre quasiment tous les problèmes de développement web. Il existe des bibliothèques pour se servir des cookies, gérer les sessions, la connexion de l'utilisateur, les paramètres de l'URL, les données <code>POST</code>, les entêtes de sécurité et d'autres encore. Vous trouverez une liste des paquets maintenus par l'équipe Express ici : <a href="https://expressjs.com/fr/resources/middleware.html">Express Middleware</a> (ainsi que la liste de paquets tiers populaires).</p>

<div class="note">
  <p><strong>Note :</strong> Cette flexibilité est à double tranchant. Il y a une multitude de paquets pour résoudre chaque problème mais trouver le bon paquet à utiliser peut vite devenir un défi. Il n'y a pas non plus de « bonne manière » pour structurer une application et beaucoup d'exemples que vous trouverez sur le net ne sont pas optimisés ou montrent seulement une infime partie de ce que vous devez faire pour développer une application web.</p>
</div>

<h2 id="where_did_node_and_express_come_from">D'où viennent Node et Express&nbsp;?</h2>

<p>À ses débuts en 2009, Node a été publié pour Linux uniquement. Le gestionnaire de paquets NPM est sorti en 2010, et le support natif de Windows fut ajouté en 2012. Ceci est un très court aperçu d'une aventure riche en rebondissements. Allez creuser ça sur <a href="https://fr.wikipedia.org/wiki/Node.js#Historique">Wikipédia</a> si vous voulez en savoir plus.</p>

<p>Express est sorti pour la première fois en novembre 2010. Vous pouvez consulter la <a href="https://expressjs.com/en/changelog/4x.html">liste des modifications</a> pour plus d'informations sur la version courante et <a href="https://github.com/expressjs/express/blob/master/History.md">GitHub</a> pour plus de détails sur l'historique des publications.</p>

<h2 id="how_popular_are_node_and_express">Quelle popularité pour Node et Express&nbsp;?</h2>

<p>La popularité d'un <i>framework</i> web est importante car elle conditionne la maintenance dans le temps et les ressources qu'il est raisonnable de mettre à disposition dans la documentation, les bibliothèques d'extensions et le support technique.</p>

<p>Il n'existe pas d'échelle de mesures définitive et fiable pour l'estimation de la popularité des <i>frameworks</i> côté serveur, bien que des sites comme <a href="https://hotframeworks.com/">Hot Frameworks</a> essaient d'estimer la popularité par le comptage du nombre de projets GitHub ou StackOverflow.  La question est : « Est-ce que Node et Express sont suffisamment populaires pour pouvoir s'affranchir des plateformes non-populaires ? Continuent-ils à évoluer ? Pouvez-vous avoir de l'aide si besoin ? Existe-t-il une opportunité pour vous de gagner de l'argent si vous apprenez Express ? ».</p>

<p>Si on se réfère à la <a href="https://expressjs.com/fr/resources/companies-using-express.html"> liste des entreprises utilisant Express</a>, la quantité de gens contribuant au code et le nombre de gens fournissant un support payant ou bien gratuit, alors oui, <em>Express</em> est un framework populaire !</p>

<h2 id="is_express_opinionated">Express est-il « dogmatique » ?</h2>

<p>Les cadres logiciels web se décrivent souvent comme ayant ou non des opinions données sur tel ou tel sujet au sens où ils sont orientés dans leur usage selon ces choix/opinions. En anglais, on dit d'un <i>framework</i> qu'il est <i>opinionated</i> ou non.</p>

<p>Les <i>frameworks</i> avec une opinion sont ceux qui ont un avis arrêté sur la « bonne manière » de gérer certaines tâches. Ils fournissent souvent un cadre permettant de développer rapidement <em>dans un domaine particulier</em> (résolvant des problèmes d'un type particulier) parce que la bonne manière de faire quoi que ce soit est généralement bien comprise et bien documentée. Toutefois, ils peuvent manquer de flexibilité pour la résolution de problèmes hors de leur portée et tendent à offrir peu de choix concernant les composants et approches qu'ils peuvent utiliser.</p>

<p>Les <i>frameworks</i> sans opinion, par contraste, ont beaucoup moins de restrictions sur la meilleure manière d'assembler des composants ensemble pour atteindre un objectif, ou encore sur les composants que vous devriez utiliser. Ils laissent aux développeurs la possibilité d'utiliser les outils les plus adaptés pour achever une tâche particulière, bien que cela nécessite que vous cherchiez et trouviez ces composants par vous-même.</p>

<p>Express n'est pas particulièrement dogmatique. Vous pouvez intégrer quasiment n'importe quelle fonction intermédiaire compatible voulue dans la pile de gestion des requêtes, dans quasiment n'importe quel ordre. Vous pouvez structurer l'application en un fichier comme en plusieurs, et utiliser n'importe quelle structure de dossiers. Vous pourrez même quelques fois vous sentir perdu⋅e par la liberté que vous avez de vous organiser comme vous le souhaitez !</p>

<h2 id="what_does_express_code_look_like">À quoi ressemble du code Express ?</h2>

<p>Dans un site web utilisé pour traiter des données, une application web attend des requêtes HTTP du navigateur web (ou d'un autre client). Quand une requête est reçue, l'application cherche quelle action est requise en fonction du modèle de l'URL et des possibles informations associés contenues dans les données <code>POST</code> ou <code>GET</code>. Selon ce qui est requis, il pourra alors lire ou écrire des informations dans une base de données ou effectuer d'autre tâches pour satisfaire la requête. L'application va alors retourner une réponse au navigateur web, souvent une page HTML créée dynamiquement pour le navigateur, en intégrant les données récupérées dans un modèle HTML.</p>

<p>Express fournit des méthodes pour spécifier quelle fonction est appelée pour une méthode HTTP particulière (<code>GET</code>, <code>POST</code>, <code>SET</code>, etc.) et un modèle d'URL ("Route"), ainsi que des méthodes pour spécifier quel moteur de rendu de vues ("view") est utilisé, où sont les modèles de vues et quel modèle utiliser pour générer une réponse. Vous pouvez utiliser les fonctions intermédiaires d'Express pour prendre en charge les cookies, les sessions, les utilisateurs, obtenir les paramètres <code>POST</code>/<code>GET</code>, etc. Vous pouvez utiliser n'importe que système de base données supporté par Node (Express ne définit aucun comportement relatif aux bases de données).</p>

<p>Les sections suivantes expliquent quelques choses communes que vous verrez en travaillant avec du code <em>Express</em> et <em>Node</em>.</p>

<h3 id="helloworld_express">Hello World Express</h3>

<p>Tout d'abord intéressons-nous à l'exemple <a href="https://expressjs.com/fr/starter/hello-world.html">Hello World</a> standard d'Express (nous expliquons chaque partie de cet exemple ci-dessous, et dans les sections suivantes).</p>

<div class="note">
  <p><strong>Note :</strong> Si vous avez déjà installé Node et Express (ou si vous les installez comme montré dans <a href="/fr/docs/Learn/Server-side/Express_Nodejs/development_environment">l'article suivant</a>), vous pouvez enregistrer ce code dans un fichier texte appelé <strong>app.js</strong> et l'exécuter via un terminal en tapant :</p>
  <p><strong><code>node app.js</code></strong></p>
</div>

<pre class="brush: js">const express = require('express');
const app = express();
const port = 3000;

<strong>app.get('/', (req, res) =&gt; {
  res.send('Hello World!')
});</strong>

app.listen(port, () =&gt; {
  console.log(`Application exemple à l'écoute sur le port ${port}!`)
});
</pre>

<p>Les deux premières lignes importent (avec <code>require()</code>) le module express et créent une <a href="https://expressjs.com/en/4x/api.html#app">application express</a>. Cet objet, qui est traditionnellement nommé <code>app</code>, possède des méthodes pour acheminer les requêtes HTTP, configurer les intergiciels, rendre les vues HTML, enregistrer un moteur de modèles et modifier les <a href="https://expressjs.com/en/4x/api.html#app.settings.table">paramètres de l'application</a> qui contrôlent le comportement de l'application (par exemple, le mode d'environnement, si les définitions de route sont sensibles à la casse, etc.).</p>

<p>La partie centrale du code (les trois lignes commençant par <code>app.get</code>) montre une <em>définition de route</em>. La méthode <code>app.get()</code> spécifie une fonction de rappel qui sera invoquée chaque fois qu'il y aura une requête HTTP <code>GET</code> avec un chemin (<code>'/'</code>) relatif à la racine du site. La fonction de rappel prend une requête et un objet réponse comme arguments, et appelle simplement <a href="https://expressjs.com/en/4x/api.html#res.send"><code>send()</code></a> sur la réponse pour renvoyer la chaîne de caractères <code>"Hello World!"</code>.</p>

<p>Le dernier bloc démarre le serveur sur le port 3000 et affiche un commentaire de journal dans la console. Avec le serveur en cours d'exécution, vous pouvez aller sur <code>localhost:3000</code> dans votre navigateur pour voir l'exemple de réponse renvoyé.</p>

<h3 id="importing_and_creating_modules">Créer et importer des modules</h3>

<p>Un module est une bibliothèque/fichier JavaScript que vous pouvez importer dans un autre code en utilisant la fonction <code>require()</code> de Node. <em>Express</em> lui-même est un module, tout comme les bibliothèques de <i>middleware</i> et de base de données que nous utilisons dans nos applications <em>Express</em>.</p>

<p>Le code ci-dessous montre comment nous importons un module par son nom, en utilisant le framework <em>Express</em> comme exemple. Tout d'abord, nous invoquons la fonction <code>require()</code>, en spécifiant le nom du module sous forme de chaîne (<code>'express'</code>), et en appelant l'objet retourné pour créer une <a href="https://expressjs.com/en/4x/api.html#app">applicationExpress</a>. Nous pouvons alors accéder aux propriétés et fonctions de l'objet application.</p>

<pre class="brush: js">const express = require('express');
const app = express();
</pre>

<p>Vous pouvez également créer vos propres modules qui peuvent être importés de la même manière.</p>

<div class="note">
  <p><strong>Note :</strong> Vous <em>voudrez</em> créer vos propres modules, car cela vous permet d'organiser votre code en parties maniables — une application monolithique à fichier unique est difficile à comprendre et à maintenir. L'utilisation de modules vous aide également à gérer votre espace de noms, car seules les variables que vous exportez explicitement sont importées lorsque vous utilisez un module.</p>
</div>

<p>Pour rendre les objets disponibles en dehors d'un module, il suffit de les affecter à l'objet <code>exports</code>. Par exemple, le module <strong>square.js</strong> ci-dessous est un fichier qui exporte les méthodes <code>area()</code> et <code>perimeter()</code> :</p>

<pre class="brush: js">exports.area = function(width) { return width * width; };
exports.perimeter = function(width) { return 4 * width; };</pre>

<p>Nous pouvons importer ce module en utilisant <code>require()</code>, puis appeler la ou les méthodes exportées comme indiqué :</p>

<pre class="brush: js">var square = require('./square'); // Ici, nous demandons le nom du fichier sans l'extension de fichier .js (facultative).
console.log("L'aire d'un carré dont la largeur est de 4 est la suivante " + square.area(4));</pre>

<div class="note">
  <p><strong>Note :</strong> Vous pouvez également spécifier un chemin absolu vers le module (ou un nom, comme nous l'avons fait initialement).</p>
</div>

<p>Si vous souhaitez exporter un objet complet en une seule affectation au lieu de le construire une propriété à la fois, affectez-le à <code>module.exports</code> comme indiqué ci-dessous (vous pouvez également procéder ainsi pour faire de la racine de l'objet exports un constructeur ou une autre fonction) :</p>

<pre class="brush: js">module.exports = {
  area: function(width) {
    return width * width;
  },

  perimeter: function(width) {
    return 4 * width;
  }
};</pre>

<div class="note">
  <p><strong>Note :</strong> L'objet <code>exports</code> peut être vu comme un <a href="https://nodejs.org/api/modules.html#modules_exports_shortcut">raccourci</a> vers <code>module.exports</code> au sein d'un module donné. En fait, <code>exports</code> est simplement une variable qui est initialisée avec la valeur de <code>module.exports</code> avant que le module soit évalué. Cette valeur est une référence vers un objet. Cela signifie que <code>exports</code> référence le même objet que celui référencé par <code>module.exports</code>. Cela signifie également qu'affecter une autre valeur à <code>exports</code> le détachera complètement de <code>module.exports</code>.</p>
  </div>
  

<p>Pour de plus amples informations sur les modules, voir <a href="https://nodejs.org/api/modules.html#modules_modules">Modules</a> (documentation Node).</p>

<h3 id="using_asynchronous_apis">Utilisation des API asynchrones</h3>

<p>Le code JavaScript utilise fréquemment des API asynchrones plutôt que synchrones pour les opérations qui peuvent prendre un certain temps à se terminer. Une API synchrone est une API dans laquelle chaque opération doit être terminée avant que l'opération suivante puisse commencer. Par exemple, les fonctions d'enregistrement suivantes sont synchrones et impriment le texte dans la console dans l'ordre (Premier, Second).</p>

<pre class="brush: js">console.log('Premier');
console.log('Second');</pre>

<p>En revanche, une API asynchrone est une API qui lance une opération et revient immédiatement (avant que l'opération ne soit terminée). Une fois l'opération terminée, l'API utilisera un mécanisme quelconque pour effectuer des opérations supplémentaires. Par exemple, le code ci-dessous imprimera « Second, Premier » car même si la méthode <code>setTimeout()</code> est appelée en premier, et revient immédiatement, l'opération ne se termine pas avant plusieurs secondes.</p>

<pre class="brush: js">setTimeout(function() {
  console.log('Premier');
}, 3000);
console.log('Second');</pre>

<p>L'utilisation d'API asynchrones non bloquantes est encore plus importante sur Node que dans le navigateur, car <em>Node</em> est un environnement d'exécution événementiel avec un seul <i>thread</i>. Cela signifie que toutes les requêtes adressées au serveur sont exécutées sur le même <i>thread</i> (plutôt que d'être fractionnées en <i>threads</i> distincts). Ce modèle est extrêmement efficace en termes de vitesse et de ressources du serveur, mais il signifie que si l'une de vos fonctions appelle des méthodes synchrones qui prennent beaucoup de temps pour se terminer, elle bloquera non seulement la demande actuelle, mais aussi toutes les autres demandes traitées par votre application Web.</p>

<p>Il existe plusieurs façons pour une API asynchrone d'informer votre application de la fin d'une opération. La méthode la plus courante consiste à enregistrer une fonction de rappel lorsque vous invoquez l'API asynchrone, qui sera rappelée lorsque l'opération sera terminée. C'est l'approche utilisée ci-dessus.</p>

<div class="note">
  <p><strong>Note :</strong> L'utilisation des rappels (« <em>callbacks</em> ») peut être assez « désordonnée » si vous avez une séquence d'opérations asynchrones dépendantes qui doivent être exécutées dans l'ordre, car cela entraîne de multiples niveaux de rappels imbriqués. Ce problème est communément appelé « l'enfer des callbacks ». Ce problème peut être réduit par de bonnes pratiques de codage dont l'utilisation des <a href="/fr/docs/Web/JavaScript/Reference/Global_Objects/Promise">promesses</a> ou de <a href="/fr/docs/Web/JavaScript/Reference/Statements/async_function"><code>async</code></a>/<a href="/fr/docs/Web/JavaScript/Reference/Operators/await"><code>await</code></a>.</p>
</div>

<div class="note">
  <p><strong>Note :</strong> Une convention courante pour Node et Express est d'utiliser des callbacks de type error-first. Dans cette convention, la première valeur de vos <em>fonctions de rappel</em> est une valeur d'erreur, tandis que les arguments suivants contiennent des données de succès. Il y a une bonne explication de l'utilité de cette approche dans ce blog : <a href="https://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js">The Node.js Way - Comprendre les callbacks de type « Error First ».</a> (fredkschott.com).</p>
</div>

<h3 id="creating_route_handlers">Créer des gestionnaires de route</h3>

<p>Dans notre exemple <em>Hello World</em> d'Express (voir ci-dessus), nous avons défini une fonction de gestion de route (un <i>callback</i>) pour les requêtes HTTP <code>GET</code> vers la racine du site (<code>'/'</code>).</p>

<pre class="brush: js">app.get('/', (req, res) =&gt; {
  res.send('Hello World!');
});</pre>

<p>La fonction de rappel prend une requête et un objet réponse comme arguments. Dans ce cas, la méthode appelle simplement <a href="https://expressjs.com/en/4x/api.html#res.send"><code>send()</code></a> sur la réponse pour renvoyer la chaîne de caractères « Hello World ! ». Il existe un <a href="https://expressjs.com/fr/guide/routing.html#response-methods">nombre d'autres méthodes de réponse</a> pour terminer le cycle requête/réponse, par exemple vous pourriez appeler <code><a href="https://expressjs.com/en/4x/api.html#res.json">res.json()</a></code> pour envoyer une réponse JSON ou <code><a href="https://expressjs.com/en/4x/api.html#res.sendFile">res.sendFile()</a></code> pour envoyer un fichier.</p>

<div class="note">
  <p><strong>Note :</strong> Vous pouvez utiliser les noms d'arguments de votre choix dans les fonctions de rappel ; lorsque le rappel est invoqué, le premier argument sera toujours la requête et le second sera toujours la réponse. Il est judicieux de les nommer de telle sorte que vous puissiez identifier l'objet avec lequel vous travaillez dans le corps du <i>callback</i>.</p>
</div>

<p>L'objet <em>Express</em> <code>application</code> fournit également des méthodes permettant de définir des gestionnaires de route pour tous les autres verbes HTTP, qui sont pour la plupart utilisés exactement de la même manière : </p>
<p><code>checkout()</code>, <code>copy()</code>, <strong><code>delete()</code></strong>, <strong><code>get()</code></strong>, <code>head()</code>, <code>lock()</code>, <code>merge()</code>, <code>mkactivity()</code>, <code>mkcol()</code>, <code>move()</code>, <code>m-search()</code>, <code>notify()</code>, <code>options()</code>, <code>patch()</code>, <strong><code>post()</code></strong>, <code>purge()</code>, <strong><code>put()</code></strong>, <code>report()</code>, <code>search()</code>, <code>subscribe()</code>, <code>trace()</code>, <code>unlock()</code>, <code>unsubscribe()</code>.</p>

<p>Il existe une méthode de routage spéciale, <code>app.all()</code>, qui sera appelée en réponse à toute méthode HTTP. Ceci est utilisé pour charger les fonctions <i>middleware</i> à un chemin particulier pour toutes les méthodes de requête. L'exemple suivant (tiré de la documentation d'Express) montre un gestionnaire qui sera exécuté pour les requêtes vers <code>/secret</code> indépendamment du verbe HTTP utilisé (à condition qu'il soit supporté par le <a href="https://nodejs.org/api/http.html#http_http_methods">module http</a>).</p>

<pre class="brush: js">app.all('/secret', (req, res, next) =&gt; {
  console.log('Accès à la section secrète ...');
  next(); // passe le contrôle au gestionnaire suivant
});</pre>

<p>Les routes vous permettent de faire correspondre des modèles particuliers de caractères dans une URL, d'extraire certaines valeurs de l'URL et de les transmettre comme paramètres au gestionnaire de la route (en tant qu'attributs de l'objet de la demande transmis comme paramètre).</p>

<p>Il est souvent utile de regrouper les gestionnaires de route pour une partie particulière d'un site et d'y accéder en utilisant un préfixe de route commun (par exemple, un site avec un Wiki pourrait avoir toutes les routes liées au wiki dans un seul fichier et les faire accéder avec un préfixe de route de <em>/wiki/</em>). Dans <em>Express</em>, ceci est réalisé en utilisant l'objet <code><a href="https://expressjs.com/fr/guide/routing.html#express-router">express.Router</a></code>. Par exemple, nous pouvons créer notre route wiki dans un module nommé <strong>wiki.js</strong>, puis exporter l'objet <code>Router</code>, comme indiqué ci-dessous :</p>

<pre class="brush: js">// wiki.js - Module route du wiki

const express = require('express');
const router = express.Router();

// Route vers la page d'accueil
router.get('/', (req, res) =&gt; {
  res.send("Page d'accueil du wiki");
});


// Route vers la page à propos
router.get('/about', (req, res) =&gt; {
  res.send('À propos de ce wiki');
});

module.exports = router;</pre>

<div class="note">
  <p><strong>Note :</strong> L'ajout de routes à l'objet <code>Router</code> est identique à l'ajout de routes à l'objet <code>app</code> (comme indiqué précédemment).</p>
</div>

<p>Pour utiliser le routeur dans notre fichier d'application principal, nous devrions alors <code>require()</code> le module route (<strong>wiki.js</strong>), puis appeler <code>use()</code> sur l'application <em>Express</em> pour ajouter le routeur au chemin de manipulation du middleware. Les deux routes seront alors accessibles depuis <code>/wiki/</code> et <code>/wiki/about/</code>.</p>

<pre class="brush: js">const wiki = require('./wiki.js');
// ...
app.use('/wiki', wiki);</pre>

<p>Nous vous montrerons beaucoup plus en détails comment travailler avec les routes, et en particulier comment utiliser le <code>Router</code>, plus tard dans la section liée <a href="/fr/docs/Learn/Server-side/Express_Nodejs/routes">Routes et contrôleurs</a>.</p>

<h3 id="using_middleware">Utilisation d'un middleware/intergiciel</h3>

<p>L'intergiciel (aussi appelé « <em>middleware</em> ») est largement utilisé dans les applications d'Express, pour des tâches allant du service de fichiers statiques à la gestion des erreurs, en passant par la compression des réponses HTTP. Alors que les fonctions de route terminent le cycle requête-réponse HTTP en renvoyant une réponse au client HTTP, les fonctions d'intergiciel effectuent <em>typiquement</em> une opération sur la demande ou la réponse, puis appellent la fonction suivante dans la « pile », qui peut être un autre intergiciel ou un gestionnaire de route. L'ordre dans lequel les intergiciels sont appelés dépend du code de l'application.</p>

<div class="note">
  <p><strong>Note :</strong> L'intergiciel peut effectuer n'importe quelle opération, exécuter n'importe quel code, apporter des modifications à l'objet requête et réponse, et il peut <em>aussi mettre fin au cycle requête-réponse</em>. S'il ne met pas fin au cycle, alors il doit appeler <code>next()</code> pour passer le contrôle à la fonction suivante de l'intergiciel (ou la requête sera laissée en suspens).</p>
</div>

<p>La plupart des apps utiliseront des <em>programmes intermédiaires tiers</em> afin de simplifier les tâches courantes de développement web comme le travail avec les cookies, les sessions, l'authentification des utilisateurs, l'accès aux données <code>POST</code> et JSON des requêtes, la journalisation, etc. Vous pouvez trouver une <a href="https://expressjs.com/fr/resources/middleware.html">liste des paquets <i>middleware</i> maintenus par l'équipe Express</a> (qui inclut également d'autres paquets populaires de tiers). D'autres paquets Express sont disponibles sur le gestionnaire de paquets NPM.</p>

<p>Pour utiliser un <i>middleware</i> tiers, vous devez d'abord l'installer dans votre application à l'aide de NPM. Par exemple, pour installer l'intergiciel de journalisation des requêtes HTTP <a href="https://expressjs.com/fr/resources/middleware/morgan.html">morgan</a>, vous devez procéder comme suit :</p>

<pre class="brush: bash">npm install morgan</pre>

<p>Vous pourriez alors appeler <code>use()</code> sur l'objet <em>Express application</em> pour ajouter l'intergiciel à la pile :</p>

<pre class="brush: js">const express = require('express');
const logger = require('morgan');
const app = express();
app.use(logger('dev'));
...</pre>

<div class="note">
  <p><strong>Note :</strong> Les fonctions d'intergiciel et de routage sont appelées dans l'ordre où elles sont déclarées. Pour certains intergiciels, l'ordre est important (par exemple, si l'intergiciel de session dépend de l'intergiciel de cookie, alors le gestionnaire de cookie doit être ajouté en premier). Il est presque toujours nécessaire d'appeler l'intergiciel avant de définir les routes, sinon vos gestionnaires de routes n'auront pas accès aux fonctionnalités ajoutées par votre intergiciel.</p>
</div>

<p>Vous pouvez écrire vos propres fonctions intergiciels, et vous serez probablement amené à le faire (ne serait-ce que pour créer un code de gestion des erreurs). La <strong>seule</strong> différence entre une fonction <i>middleware</i> et un <i>callback</i> de gestionnaire de route est que les fonctions <i>middleware</i> ont un troisième argument <code>next</code>, que les fonctions <i>middleware</i> sont censées appeler si elles ne sont pas celle qui termine le cycle de requête (lorsque la fonction <i>middleware</i> est appelée, cela contient la fonction <em>next</em> qui doit être appelée).</p>

<p>Vous pouvez ajouter une fonction d'intergiciel à la chaîne de traitement avec <code>app.use()</code> ou <code>app.add()</code>, selon que vous voulez appliquer l'intergiciel à toutes les réponses ou aux réponses avec un verbe HTTP particulier (<code>GET</code>, <code>POST</code>, etc). Vous spécifiez les routes de la même manière dans les deux cas, bien que la route soit facultative lors de l'appel à <code>app.use()</code>.</p>

<p>L'exemple ci-dessous montre comment vous pouvez ajouter la fonction <i>middleware</i> en utilisant les deux méthodes, et avec/sans route.</p>

<pre class="brush: js">const express = require('express');
const app = express();

// Un exemple de fonction middleware
let a_middleware_function = function(req, res, <em>next</em>) {
  // ... effectuer certaines opérations
  next(); // Appelez next() pour qu'Express appelle la fonction <i>middleware</i> suivante dans la chaîne.
}

// Fonction ajoutée avec use() pour toutes les routes et verbes
app.use(a_middleware_function);

// Fonction ajoutée avec use() pour une route spécifique
app.use('/uneroute', a_middleware_function);

// Une fonction <i>middleware</i> ajoutée pour un verbe et une route HTTP spécifiques
app.get('/', a_middleware_function);

app.listen(3000);</pre>

<div class="note">
  <p><strong>Note :</strong> Ci-dessus, nous déclarons la fonction <i>middleware</i> séparément, puis nous la définissons comme fonction de rappel. Dans notre précédente fonction de gestion de route, nous avons déclaré la fonction de rappel lorsqu'elle a été utilisée. En JavaScript, les deux approches sont valables.</p>
</div>

<p>La documentation d'Express contient beaucoup d'autres excellents documents sur <a href="https://expressjs.com/fr/guide/using-middleware.html">l'utilisation</a> et <a href="https://expressjs.com/fr/guide/writing-middleware.html">l'écriture</a> d'intergiciels Express.</p>

<h3 id="serving_static_files">Servir les fichiers statiques</h3>

<p>Vous pouvez utiliser l'intergiciel <a href="https://expressjs.com/en/4x/api.html#express.static">express.static</a> pour servir des fichiers statiques, notamment vos images, CSS et JavaScript (<code>static()</code> est la seule fonction de l'intergiciel qui fait réellement <strong>partie</strong> d'<em>Express</em>). Par exemple, vous utiliserez la ligne ci-dessous pour servir des images, des fichiers CSS et des fichiers JavaScript à partir d'un répertoire nommé <strong>'public'</strong> au même niveau que celui où vous appelez node :</p>

<pre class="brush: js">app.use(express.static('public'));</pre>

<p>Tous les fichiers du répertoire public sont servis en ajoutant leur nom de fichier (<em>relatif</em> au répertoire "public" de base) à l'URL de base. Ainsi, par exemple :</p>

<pre>https://localhost:3000/images/dog.jpg
https://localhost:3000/css/style.css
https://localhost:3000/js/app.js
https://localhost:3000/about.html</pre>

<p>Vous pouvez appeler <code>static()</code> plusieurs fois pour servir plusieurs répertoires. Si un fichier ne peut pas être trouvé par une fonction middleware, alors il sera simplement transmis au <i>middleware</i> suivant (l'ordre dans lequel le <i>middleware</i> est appelé est basé sur votre ordre de déclaration).</p>

<pre class="brush: js">app.use(express.static('public'));
app.use(express.static('media'));</pre>

<p>Vous pouvez également créer un préfixe virtuel pour vos URL statiques, plutôt que de voir les fichiers ajoutés à l'URL de base. Par exemple, ici nous <a href="https://expressjs.com/en/4x/api.html#app.use">spécifions un chemin de montage</a> pour que les fichiers soient chargés avec le préfixe « /media » :</p>

<pre class="brush: js">app.use('/media', express.static('public'));</pre>

<p>Maintenant, vous pouvez charger les fichiers qui se trouvent dans le répertoire <code>public</code> à partir du préfixe du chemin <code>/media</code>.</p>

<pre>https://localhost:3000/media/images/dog.jpg
https://localhost:3000/media/video/cat.mp4
https://localhost:3000/media/cry.mp3</pre>

<div class="note">
  <p><strong>Note :</strong> Voir également <a href="https://expressjs.com/fr/starter/static-files.html">Servir des fichiers statiques dans Express</a>.</p>
</div>

<h3 id="handling_errors">Traitement des erreurs</h3>

<p>Les erreurs sont traitées par une ou plusieurs fonctions spéciales du <i>middleware</i> qui ont quatre arguments, au lieu des trois habituels : <code>(err, req, res, next)</code>. Par exemple :</p>

<pre class="brush: js">app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send("Quelque chose s'est cassé !");
});</pre>

<p>Ceux-ci peuvent retourner tout contenu nécessaire, mais doivent être appelés après tous les autres <code>app.use()</code> et les appels de routes afin qu'ils soient le dernier <i>middleware</i> dans le processus de traitement des requêtes !</p>

<p>Express est livré avec un gestionnaire d'erreurs intégré, qui prend en charge toutes les erreurs restantes qui pourraient être rencontrées dans l'application. Cette fonction <i>middleware</i> de gestion des erreurs par défaut est ajoutée à la fin de la pile de fonctions middleware. Si vous passez une erreur à <code>next()</code> et que vous ne la gérez pas dans un gestionnaire d'erreurs, elle sera traitée par le gestionnaire d'erreurs intégré ; l'erreur sera écrite au client avec la trace de la pile.</p>

<div class="note">
  <p><strong>Note :</strong> La trace de la pile n'est pas incluse dans l'environnement de production. Pour exécuter une application serveur Express, la variable d'environnement <code>NODE_ENV</code> doit être définie avec la valeur <code>production</code>.</p>
</div>

<div class="note">
  <p><strong>Note :</strong> Les codes d'état HTTP 404 et autres « erreurs » ne sont pas traités comme des erreurs. Si vous voulez les gérer, vous pouvez ajouter une fonction <i>middleware</i> pour le faire. Pour plus d'informations, consultez la <a href="https://expressjs.com/fr/starter/faq.html#how-do-i-handle-404-responses">FAQ</a>.</p>
</div>

<p>Pour plus d'informations, voir <a href="https://expressjs.com/fr/guide/error-handling.html">Gestion des erreurs</a> (docs Express).</p>

<h3 id="using_databases">Utilisation des bases de données</h3>

<p>Les apps <em>Express</em> peuvent utiliser tout mécanisme de base de données pris en charge par <em>Node</em> (<em>Express</em> lui-même ne définit aucun comportements/exigences supplémentaire spécifique pour la gestion des bases de données). Il existe de nombreuses options, notamment PostgreSQL, MySQL, Redis, SQLite, MongoDB, etc.</p>

<p>Pour les utiliser, vous devez d'abord installer le pilote de base de données à l'aide de NPM. Par exemple, pour installer le pilote de la populaire base de données NoSQL MongoDB, vous devez utiliser la commande suivante :</p>

<pre class="brush: bash">$ npm install mongodb</pre>

<p>La base de données elle-même peut être installée localement ou sur un serveur en nuage. Dans votre code Express, vous avez besoin du pilote, vous vous connectez à la base de données, puis vous effectuez des opérations de création, lecture, mise à jour et suppression (en anglais, on utilise l'acronyme CRUD qui signifie <i>Create, Read, Update, Delete</i>). L'exemple ci-dessous (tiré de la documentation d'Express) montre comment vous pouvez trouver des enregistrements « mammifères » en utilisant MongoDB.</p>

<pre class="brush: js">// cela fonctionne avec les anciennes versions de mongodb version ~ 2.2.33
const MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/animals', function(err, db) {
  if (err) throw err;

  db.collection('mammals').find().toArray(function (err, result) {
    if (err) throw err;

    console.log(result);
  });
});

// pour mongodb version 3.0 et supérieure
const MongoClient = require('mongodb').MongoClient;
MongoClient.connect('mongodb://localhost:27017/animals', function(err, client){
   if(err) throw err;

   let db = client.db('animals');
   db.collection('mammals').find().toArray(function(err, result){
     if(err) throw err;
     console.log(result);
     client.close();
   });
});</pre>

<p>Une autre approche populaire consiste à accéder à votre base de données de manière indirecte, via un mappeur objet-relationnel (« ORM »). Dans cette approche, vous définissez vos données en tant qu'objets ou modèles et l'ORM les met en correspondance avec le format de base de données sous-jacent. L'avantage de cette approche est qu'en tant que développeur, vous pouvez continuer à penser en termes d'objets JavaScript plutôt qu'en termes de sémantique de base de données, et qu'il existe un endroit évident pour effectuer la validation et la vérification des données entrantes. Nous parlerons davantage des bases de données dans un article ultérieur.</p>

<p>Pour plus d'informations, voir <a href="https://expressjs.com/fr/guide/database-integration.html">Intégration de base de données</a> (docs Express).</p>

<h3 id="rendering_data_views">Rendu des données (vues)</h3>

<p>Les moteurs de modèles (appelés « moteurs de vue » par <em>Express</em>) vous permettent de spécifier la <em>structure</em> d'un document de sortie dans un modèle, en utilisant des espaces réservés pour les données qui seront remplies lorsqu'une page sera générée. Les modèles sont souvent utilisés pour créer du HTML, mais peuvent également créer d'autres types de documents. Express prend en charge <a href="https://github.com/expressjs/express/wiki#template-engines">un certain nombre de moteurs de modèles</a>, et il existe une comparaison utile des moteurs les plus populaires ici : <a href="https://strongloop.com/strongblog/compare-javascript-templates-jade-mustache-dust/">Comparaison des moteurs de création de modèles JavaScript : Jade, Mustache, Dust et plus</a>.</p>

<p>Dans le code des paramètres de votre application, vous définissez le moteur de modèles à utiliser et l'emplacement où Express doit rechercher les modèles à l'aide des paramètres « views » et « view engines », comme indiqué ci-dessous (vous devrez également installer le paquet contenant votre bibliothèque de modèles !)</p>

<pre class="brush: js">const express = require('express');
const path = require('path');
const app = express();

// Définir le répertoire contenant les modèles ('views')
app.set('views', path.join(__dirname, 'views'));

// Définir le moteur d'affichage à utiliser, dans ce cas 'some_template_engine_name'.
app.set('view engine', 'some_template_engine_name');</pre>

<p>L'apparence du modèle dépendra du moteur que vous utilisez. En supposant que vous ayez un fichier de modèle nommé « index.&lt;template_extension&gt; » qui contient des espaces réservés pour des variables de données nommées « title » et « message », vous appelleriez <code><a href="https://expressjs.com/en/4x/api.html#res.render">Response.render()</a></code> dans une fonction de gestionnaire de route pour créer et envoyer la réponse HTML :</p>

<pre class="brush: js">app.get('/', function(req, res) {
  res.render('index', { title: 'À propos des poules', message: 'Elles sont où ?' });
});</pre>

<p>Pour plus d'informations, voir <a href="https://expressjs.com/fr/guide/using-template-engines.html">Utilisation des moteurs de modèles avec Express</a> (docs Express).</p>

<h3 id="file_structure">Structure du fichier</h3>

<p>Express ne fait aucune supposition en termes de structure ou de composants que vous utilisez. Les routes, les vues, les fichiers statiques et toute autre logique spécifique à l'application peuvent vivre dans un nombre quelconque de fichiers avec n'importe quelle structure de répertoire. Bien qu'il soit parfaitement possible d'avoir l'ensemble de l'application <em>Express</em> dans un seul fichier, il est généralement judicieux de diviser votre application en fichiers basés sur la fonction (par exemple, gestion de compte, blogs, forums de discussion) et le domaine de problème architectural (par exemple, modèle, vue ou contrôleur si vous utilisez une <a href="/fr/docs/Glossary/MVC">architecture MVC</a>).</p>

<p>Dans une prochaine rubrique, nous utiliserons le <em>Générateur d'applications express</em>, qui crée un squelette d'application modulaire que nous pouvons facilement étendre pour créer des applications web.</p>

<h2 id="summary">Résumé</h2>

<p>Félicitations, vous avez terminé la première étape de votre voyage Express/Node ! Vous devriez maintenant comprendre les principaux avantages d'Express et de Node, et savoir à quoi ressemblent les principales parties d'une application Express (routes, intergiciels, gestion des erreurs et code modèle). Vous devez également comprendre qu'Express étant un framework non autonome, la manière dont vous assemblez ces éléments et les bibliothèques que vous utilisez dépendent largement de vous !</p>

<p>Bien sûr, Express est délibérément un cadre d'application web très léger, et une grande partie de ses avantages et de son potentiel provient de bibliothèques et de fonctionnalités tierces. Nous les examinerons plus en détail dans les articles suivants. Dans notre prochain article, nous nous pencherons sur la configuration d'un environnement de développement Node, afin que vous puissiez commencer à voir du code Express en action.</p>

<h2 id="see_also">Voir aussi</h2>

<ul>
  <li><a href="https://medium.com/@ramsunvtech/manage-multiple-node-versions-e3245d5ede44">Venkat.R - Gestion de plusieurs versions de Node</a></li>
  <li><a href="https://nodejs.org/api/modules.html#modules_modules">Modules</a> (docs Node)</li>
  <li><a href="https://expressjs.com/">Express</a> (page d'accueil)</li>
  <li><a href="https://expressjs.com/fr/starter/basic-routing.html">Routage de base</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/routing.html">Guide de routage</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/using-template-engines.html">Utilisation de moteurs de modèles avec Express</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/using-middleware.html">Utilisation d'intergiciel</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/writing-middleware.html">Écriture d'intergiciels à utiliser dans les applications Express</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/database-integration.html">Intégration des bases de données</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/starter/static-files.html">Servir les fichiers statiques dans Express</a> (docs Express)</li>
  <li><a href="https://expressjs.com/fr/guide/error-handling.html">Gestion des erreurs</a> (docs Express)</li>
</ul>

<div>{{NextMenu("Learn/Server-side/Express_Nodejs/development_environment", "Learn/Server-side/Express_Nodejs")}}</div>

<h2 id="in_this_module">Dans ce module</h2>

<ul>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/Introduction">Introduction à Express/Node</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/development_environment">Configuration d'un environnement de développement Node (Express)</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/Tutorial_local_library_website">Tutoriel Express : La bibliothèque locale du site Web</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/skeleton_website">Tutoriel Express, partie 2 : Création d'un squelette de site Web</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/mongoose">Tutoriel Express, partie 3 : Utilisation d'une base de données (avec Mongoose)</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/routes">Tutoriel Express, partie 4 : Routes et contrôleurs</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/Displaying_data">Tutoriel Express, partie 5 : Affichage des données de la bibliothèque</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/forms">Tutoriel Express, partie 6 : Travailler avec des formulaires</a></li>
  <li><a href="/fr/docs/Learn/Server-side/Express_Nodejs/deployment">Tutoriel Express, Partie 7 : Déploiement en production</a></li>
</ul>