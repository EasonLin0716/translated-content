---
title: 'Tutoriel Django - 6e partie : Vues génériques pour les listes et les détails'
slug: Learn/Server-side/Django/Generic_views
tags:
  - Beginner
  - Learn
  - Tutorial
  - django
  - django templates
  - django views
translation_of: Learn/Server-side/Django/Generic_views
original_slug: Learn/Server-side/Django/Vues_generiques
---
<div></div>

<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Server-side/Django/Home_page", "Learn/Server-side/Django/Sessions", "Learn/Server-side/Django")}}</div>

<div>Ce tutoriel améliore notre site web <a href="/fr/docs/Learn/Server-side/Django/Tutorial_local_library_website">LocalLibrary</a>, en ajoutant des pages de listes et de détails pour les livres et les auteurs. Ici nous allons apprendre les vues génériques basées sur des classes, et montrer comment elles peuvent réduire le volume de code à écrire pour les cas ordinaires. Nous allons aussi entrer plus en détail dans la gestion des URLs, en montrant comment réaliser des recherches de patterns simples.</div>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Prérequis:</th>
   <td>
    <p>Avoir terminé tous les tutoriels précédents, y compris <a href="/fr/docs/Learn/Server-side/Django/Home_page">Django Tutorial Part 5: Creating our home page</a>.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Objectif:</th>
   <td>
    <p>Comprendre où et comment utiliser des vues génériques basées sur classes, et comment extraire des patterns dans des URLs pour transmettre les informations aux vues.</p>
   </td>
  </tr>
 </tbody>
</table>

<h2 id="Aperçu">Aperçu</h2>

<p>Dans ce tutoriel, nous allons terminer la première version du site web <a href="/fr/docs/Learn/Server-side/Django/Tutorial_local_library_website">LocalLibrary</a>, en ajoutant des pages de listes et de détails pour les livres et les auteurs (ou pour être plus précis, nous allons vous montrer comment implémenter les pages concernant les livres, et vous faire créer vous-mêmes les pages concernant les auteurs !).</p>

<p>Le processus est semblable à celui utilisé pour créer la page d'index, processus que nous avons montré dans le tutoriel précédent. Nous allons avoir de nouveau besoin de créer des mappages d'URLs, des vues et des templates. La principale différence est que, pour la page des détails, nous allons avoir le défi supplémentaire d'extraire de l'URL des informations que nous transmettrons à la vue. Pour ces pages, nous allons montrer comment utiliser un type de vue complètement différent : des vues "listes" et "détails" génériques et basées sur des classes. Cela peut réduire significativement la somme de code nécessaire, les rendant ainsi faciles à écrire et à maintenir.</p>

<p>La partie finale de ce tutoriel montrera comment paginer vos données quand vous utilisez des vues "listes" génériques basées sur des classes.</p>

<h2 id="Page_de_liste_de_livres">Page de liste de livres</h2>

<p>La page de liste des livres va afficher une liste de tous les enregistrements de livres disponibles, en utilisant l'URL: <code>catalog/books/</code>. La page va afficher le titre et l'auteur pour chaque enregistrement, et le titre sera un hyperlien vers la page de détails associée. La page aura la même structure et la même zone de navigation que les autres pages du site, et nous pouvons dès lors étendre le template de base (<strong>base_generic.html</strong>) que nous avons créé dans le tutoriel précédent.</p>

<h3 id="Mappage_dURL">Mappage d'URL</h3>

<p>Ouvrez le fichier <strong>/catalog/urls.py</strong>, et copiez-y la ligne en gras ci-dessous. Comme pour la page d'index, cette fonction <code>path()</code> définit un pattern destiné à identifier l'URL (<strong>'books/'</strong>), une fonction vue qui sera appelée si l'URL correspond (<code>views.BookListView.as_view()</code>), et un nom pour ce mappage particulier.</p>

<pre class="brush: python">urlpatterns = [
    path('', views.index, name='index'),
<strong>    </strong>path<strong>('books/', views.BookListView.as_view(), name='books'),</strong>
]</pre>

<p>Comme discuté dans le tutoriel précédent, l'URL doit auparavant avoir identifié la chaîne <code>/catalog</code>, aussi la vue ne sera réellement appelée que pour l'URL complète: <code>/catalog/books/</code>.</p>

<p>La fonction vue a un format différent de celui que nous avions jusqu'ici : c'est parce que cette vue sera en réalité implémentée sous forme de classe. Nous allons la faire hériter d'une fonction vue générique existante, qui fait la plus grande partie de ce que nous souhaitons réaliser avec cette vue, plutôt que d'écrire notre propre fonction à partir de zéro.</p>

<p>En Django, on accède à la fonction appropriée d'une vue basée sur classe en appelant sa méthode de classe <code>as_view()</code>. Cela a pour effet de créer une instance de la classe, et de s'assurer que les bonnes méthodes seront appelées lors de requêtes HTTP.</p>

<h3 id="Vue_basée_sur_classe">Vue (basée sur classe)</h3>

<p>Nous pourrions assez aisément écrire la vue "liste de livres" comme une fonction ordinaire (comme notre précédente vue "index"), qui interrogerait la base de données pour tous les livres, et qui ensuite appellerait <code>render()</code> pour passer la liste à un template spécifique. À la place, cependant, nous allons utiliser une vue "liste" générique, basée sur une classe (<code>ListView</code>), une classe qui hérite d'une vue existante. Parce que la vue générique implémente déjà la plupart des fonctionnalités dont nous avons besoin et suit les meilleures pratiques Django, nous pourrons créer une vue "liste" plus robuste avec moins de code, moins de répétition, et au final moins de maintenance.</p>

<p>Ouvrez le fichier <strong>catalog/views.py</strong>, et copiez-y le code suivant à la fin :</p>

<pre class="brush: python">from django.views import generic

class BookListView(generic.ListView):
    model = Book</pre>

<p>C'est tout ! La vue générique va adresser une requête à la base de données pour obtenir tous les enregistrements du modèle spécifié (<code>Book</code>), et ensuite rendre un template situé à l'adresse <strong>/locallibrary/catalog/templates/catalog/book_list.html</strong> (que nous allons créer ci-dessous). À l'intérieur du template vous pouvez accéder à la liste de livres grâce à la variable de template appelée <code>object_list</code> OU <code>book_list</code> (c'est-à-dire l'appellation générique "<code><em>the_model_name</em>_list</code>").</p>

<div class="note">
<p><strong>Note :</strong> Ce chemin étrange vers le lieu du template n'est pas une faute de frappe : les vues génériques cherchent les templates dans <code>/<em>application_name</em>/<em>the_model_name</em>_list.html</code> (<code>catalog/book_list.html</code> dans ce cas) à l'intérieur du répertoire <code>/<em>application_name</em>/templates/</code> (<code>/catalog/templates/</code>).</p>
</div>

<p>Vous pouvez ajouter des attributs pour changer le comportement par défaut utilisé ci-dessus. Par exemple, vous pouvez spécifier un autre fichier de template si vous souhaitez avoir plusieurs vues qui utilisent ce même modèle, ou bien vous pourriez vouloir utiliser un autre nom de variable de template, si book_list n'est pas intuitif par rapport à l'usage que vous faites de vos templates. Probablement, le changement le plus utile est de changer/filtrer le sous-ensemble des résultats retournés : au lieu de lister tous les livres, vous pourriez lister les 5 premiers livres lus par d'autres utilisateurs.</p>

<pre class="brush: python">class BookListView(generic.ListView):
    model = Book
    context_object_name = 'my_book_list'   # your own name for the list as a template variable
    queryset = Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
    template_name = 'books/my_arbitrary_template_name_list.html'  # Specify your own template name/location</pre>

<h4 id="Ré-écrire_des_méthodes_dans_des_vues_basées_sur_classes">Ré-écrire des méthodes dans des vues basées sur classes</h4>

<p>Bien que nous n'ayons pas besoin de le faire ici, sachez qu'il vous est possible de ré-écrire des méthodes de classe.</p>

<p>Par exemple, nous pouvons ré-écrire la méthode <code>get_queryset()</code> pour changer la liste des enregistrements retournés. Cette façon de faire est plus flexible que simplement définir l'attribut <code>queryset</code>, comme nous l'avons fait dans le précédent fragment de code (bien qu'il n'y ait pas vraiment d'intérêt dans ce cas) :</p>

<pre class="brush: python">class BookListView(generic.ListView):
    model = Book

    def get_queryset(self):
        return Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
</pre>

<p>Nous pourrions aussi réécrire <code>get_context_data()</code>, afin d'envoyer au template des variables de contexte supplémentaires (par défaut c'est la liste de livres qui est envoyée). Le bout de code ci-dessous montre comment ajouter une variable appelée "<code>some_data</code>" au contexte (elle sera alors accessible comme variable de template).</p>

<pre class="brush: python">class BookListView(generic.ListView):
    model = Book

    def get_context_data(self, **kwargs):
        # Call the base implementation first to get the context
        context = super(BookListView, self).get_context_data(**kwargs)
        # Create any data and add it to the context
        context['some_data'] = 'This is just some data'
        return context</pre>

<p>Quand vous faites cela, il est important de suivre la procédure indiquée ci-dessus :</p>

<ul>
 <li>D'abord récupérer auprès de la superclasse le contexte existant.</li>
 <li>Ensuite ajouter la nouvelle information de contexte.</li>
 <li>Enfin retourner le nouveau contexte (mis à jour).</li>
</ul>

<div class="note">
<p><strong>Note :</strong> Voyez dans <a href="https://docs.djangoproject.com/en/2.1/topics/class-based-views/generic-display/">Built-in class-based generic views</a> (doc de Django) pour avoir beaucoup plus d'exemples de ce que vous pouvez faire.</p>
</div>

<h3 id="Créer_le_template_pour_la_Vue_Liste">Créer le template pour la Vue Liste</h3>

<p>Créez le fichier HTML <strong>/locallibrary/catalog/templates/catalog/book_list.html</strong>, et copiez-y le texte ci-dessous. Comme nous l'avons dit ci-dessus, c'est ce fichier que va chercher par défaut la classe générique "liste" basée sur une vue (dans le cas d'un modèle appelé <code>Book</code>, dans une application appelée <code>catalog</code>).</p>

<p>Les templates pour vues génériques sont exactement comme les autres templates (cependant, bien sûr, le contexte et les informations envoyées au templates peuvent être différents). Comme pour notre template <em>index</em>, nous étendons notre template de base à la première ligne, et remplaçons ensuite le bloc appelé <code>content</code>.</p>

<pre class="brush: html">{% extends "base_generic.html" %}

{% block content %}
  &lt;h1&gt;Book List&lt;/h1&gt;
  <strong>{% if book_list %}</strong>
  &lt;ul&gt;
    {% for book in book_list %}
      &lt;li&gt;
        &lt;a href="\{{ book.get_absolute_url }}"&gt;\{{ book.title }}&lt;/a&gt; (\{{book.author}})
      &lt;/li&gt;
    {% endfor %}
  &lt;/ul&gt;
  <strong>{% else %}</strong>
    &lt;p&gt;There are no books in the library.&lt;/p&gt;
  <strong>{% endif %} </strong>
{% endblock %}</pre>

<p>La vue envoie le contexte (liste de livres), en utilisant par défaut les alias <code>object_list</code> et <code>book_list</code> ; l'un et l'autre fonctionnent.</p>

<h4 id="Exécution_conditionnelle">Exécution conditionnelle</h4>

<p>Nous utilisons les balises de templates <code><a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#if">if</a></code>, <code>else</code>, et <code>endif</code> pour vérifier que la <code>book_list</code> a été définie et n'est pas vide. Si <code>book_list</code> est vide, alors la condition <code>else</code> affiche un texte expliquant qu'il n'y a pas de livres à lister. Si <code>book_list</code> n'est pas vide, nous parcourons la liste de livres.</p>

<pre class="brush: html"><strong>{% if book_list %}</strong>
  &lt;!-- code here to list the books --&gt;
<strong>{% else %}</strong>
  &lt;p&gt;There are no books in the library.&lt;/p&gt;
<strong>{% endif %}</strong>
</pre>

<p>La condition ci-dessus ne vérifie qu'un seul cas, mais vous pouvez ajouter d'autres tests grâce à la balise de template <code>elif</code> (par exemple <code>{% elif var2 %}</code>). Pour plus d'information sur les opérateurs conditionnels, voyez ici :  <a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#if">if</a>, <a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#ifequal-and-ifnotequal">ifequal/ifnotequal</a>, et <a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#ifchanged">ifchanged</a> dans <a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins">Built-in template tags and filters</a> (Django Docs).</p>

<h4 id="Boucles_for">Boucles for</h4>

<p>Le template utilise les balises de template <a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins/#for">for</a> et <code>endfor</code> pour boucler à travers la liste de livres, comme montré ci-dessous. Chaque itération peuple la variable de template <code>book</code> avec l'information concernant l'élément courant de la liste.</p>

<pre class="brush: html">{% for <strong>book</strong> in book_list %}
  &lt;li&gt; &lt;!-- code here get information from each <strong>book</strong> item --&gt; &lt;/li&gt;
{% endfor %}
</pre>

<p>Bien que nous ne l'utilisions pas ici, Django, à l'intérieur de la boucle, va aussi créer d'autres variables que vous pouvez utiliser pour suivre l'itération. Par exemple, vous pouvez tester la variable <code>forloop.last</code> pour réaliser une action conditionnelle au dernier passage de la boucle.</p>

<h4 id="Accéder_aux_variables">Accéder aux variables</h4>

<p>Le code à l'intérieur de la boucle crée un élément de liste pour chaque livre, élément qui montre à la fois le titre (comme lien vers la vue détail, encore à créer), et l'auteur.</p>

<pre class="brush: html">&lt;a href="\{{ book.get_absolute_url }}"&gt;\{{ book.title }}&lt;/a&gt; (\{{book.author}})
</pre>

<p>Nous accédont aux <em>champs</em> de l'enregistrement "livre" associé, en utilisant la notation "à points" (par exemple <code>book.title</code> et <code>book.author</code>), où le texte suivant l'item <code>book</code> est le nom du champ (comme défini dans le modèle).</p>

<p>Nous pouvons aussi appeler des <em>fonctions</em> contenues dans le modèle depuis l'intérieur de notre template — dans ce cas nous appelons <code>Book.get_absolute_url()</code> pour obtenir une URL que vous pouvez utiliser pour afficher dans la vue détail l'enregistrement associé. Cela fonctionne, pourvu que la fonction ne comporte pas d'arguments (il n'y a aucun moyen de passer des arguments !).</p>

<div class="note">
<p><strong>Note :</strong> Il nous faut être quelque peu attentif aux "effets de bord" quand nous appelons des fonctions dans nos templates. Ici nous récupérons simplement une URL à afficher, mais une fonction peut faire à peu près n'importe quoi — nous ne voudrions pas effacer notre base de données (par exemple) juste parce que nous avons affiché notre template !</p>
</div>

<h4 id="Mettre_à_jour_le_template_de_base">Mettre à jour le template de base</h4>

<p>Ouvrez le template de base (<strong>/locallibrary/catalog/templates/<em>base_generic.html</em></strong>) et insérez <strong>{% url 'books' %}</strong> dans le lien URL pour <strong>All books</strong>, comme indiqué ci-dessous. Cela va afficher le lien dans toutes les pages (nous pouvons mettre en place ce lien avec succès, maintenant que nous avons créé le mappage d'URL "books").</p>

<pre class="brush: python">&lt;li&gt;&lt;a href="{% url 'index' %}"&gt;Home&lt;/a&gt;&lt;/li&gt;
<strong>&lt;li&gt;&lt;a href="{% url 'books' %}"&gt;All books&lt;/a&gt;&lt;/li&gt;</strong>
&lt;li&gt;&lt;a href=""&gt;All authors&lt;/a&gt;&lt;/li&gt;</pre>

<h3 id="À_quoi_cela_ressemble-t-il">À quoi cela ressemble-t-il ?</h3>

<p>Vous ne pouvez pas encore construire la liste des livres, car il nous manque toujours une dépendance, à savoir le mappage d'URL pour la page de détail de chaque livre, qui est requise pour créer des hyperliens vers chaque livre. Nous allons montrer les vues liste et détail après la prochaine section.</p>

<h2 id="Page_de_détail_dun_livre">Page de détail d'un livre</h2>

<p>La page de détail d'un livre va afficher les informations sur un livre précis, auquel on accède en utilisant l'URL <code>catalog/book/<em>&lt;id&gt;</em></code> (où <code><em>&lt;id&gt;</em></code> est la clé primaire pour le livre). En plus des champs définis dans le modèle <code>Book</code> (auteur, résumé, ISBN, langue et genre), nous allons aussi lister les détails des copies disponibles (<code>BookInstances</code>), incluant le statut, la date de retour prévue, la marque d'éditeur et l'id. Cela permettra à nos lecteurs, non seulement de s'informer sur le livre, mais aussi de confirmer si et quand il sera disponible.</p>

<h3 id="Mappage_dURL_2">Mappage d'URL</h3>

<p>Ouvrez <strong>/catalog/urls.py</strong> et ajoutez-y le mappeur d'URL '<strong>book-detail</strong>' indiqué en gras ci-dessous. Cette fonction <code>path()</code> définit un pattern, la vue générique basée sur classe qui lui est associée, ainsi qu'un nom.</p>

<pre class="brush: python">urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
<strong>    path('book/&lt;int:pk&gt;', views.BookDetailView.as_view(), name='book-detail'),</strong>
]</pre>

<p>Pour le chemin <em>book-detail</em>, le pattern d'URL utilise une syntaxe spéciale pour capturer l'id exact du livre que nous voulons voir. La syntaxe est très simple : les chevrons ('&lt;' et '&gt;') définissent la partie de l'URL qui doit être capturée et encadrent le nom de la variable que la vue pourra utiliser pour accéder aux données capturées. Par exemple, <strong>&lt;something&gt;</strong>  va capturer le pattern marqué et passer la valeur à la vue en tant que variable "something". De manière optionnelle, vous pouvez faire précéder le nom de variable d'une <a href="https://docs.djangoproject.com/en/2.1/topics/http/urls/#path-converters">spécification de convertisseur</a>, qui définit le type de la donnée (int, str, slug, uuid, path).</p>

<p>Dans ce cas, nous utilisons <code>'&lt;int:pk&gt;'</code> pour capturer l'id du livre, qui doit être une chaîne formatée d'une certaine manière, et passer cet id à la vue en tant que paramètre nommé <code>pk</code> (abbréviation pour primary key - clé primaire). C'est l'id qui doit être utilisé pour stocker le livre de manière unique dans la base de données, comme défini dans le modèle Book.</p>

<div class="note">
<p><strong>Note :</strong> Comme nous l'avons dit précédemment, notre URL correcte est en réalité <code>catalog/book/&lt;digits&gt;</code> (comme nous sommes dans l'application <strong>catalog</strong>, <code>/catalog/</code> est supposé).</p>
</div>

<div class="warning">
<p><strong>Attention :</strong> La vue générique basée sur classe "détail" <em>s'attend</em> à recevoir un paramètre appelé <strong>pk</strong>. Si vous écrivez votre propre fonction, vous pouvez utiliser le nom que vous voulez pour votre paramètre, ou même passer l'information avec un argument non nommé.</p>
</div>

<h4 id="Introduction_aux_chemins_et_expressions_régulières_avancés">Introduction aux chemins et expressions régulières avancés</h4>

<div class="note">
<p><strong>Note :</strong> Vous n'aurez pas besoin de cette section pour achever le tutoriel ! Nous en parlons parce que nous savons que cette option vous sera probablement utile dans votre avenir centré sur Django.</p>
</div>

<p>La recherche de pattern fournie par <code>path()</code> est simple et utile pour les cas (très communs) où vous voulez seulement capturer <em>n'importe quelle</em> chaîne ou entier. Si vous avez besoin d'un filtre plus affiné (par exemple pour filtrer seulement les chaînes qui ont un certain nombre de caractères), alors vous pouvez utiliser la méthode <a href="https://docs.djangoproject.com/en/2.1/ref/urls/#django.urls.re_path">re_path()</a>.</p>

<p>Cette méthode est utilisée exactement comme <code>path()</code>, sauf qu'elle vous permet de spécifier un pattern utilisant une <a href="https://docs.python.org/3/library/re.html">Expression régulière</a>. Par exemple, le chemin précédent pourrait avoir été écrit ainsi :</p>

<pre class="brush: python"><strong>re_path(r'^book/(?P&lt;pk&gt;\d+)$', views.BookDetailView.as_view(), name='book-detail'),</strong>
</pre>

<p>Les <em>expressions régulières</em> sont un outil de recherche de pattern extrêmement puissant. Ils sont, il est vrai, assez peu intuitifs et peuvent se révéler intimidants pour les débutants. Ci-dessous vous trouverez une introduction très courte !</p>

<p>La première chose à savoir est que les expressions régulières devraient ordinairement être déclarées en utilisant la syntaxe "chaîne littérale brute" (c'est-à-dire encadrées ainsi : <strong>r'&lt;votre texte d'expression régulière va ici&gt;'</strong>).</p>

<p>L'essentiel de ce que vous aurez besoin de savoir pour déclarer une recherche de pattern est contenu dans le tableau qui suit :</p>

<table class="standard-table">
 <thead>
  <tr>
   <th scope="col">Symbol</th>
   <th scope="col">Meaning</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>^</td>
   <td>Recherche le début du texte.</td>
  </tr>
  <tr>
   <td>$</td>
   <td>Recherche la fin du texte.</td>
  </tr>
  <tr>
   <td>\d</td>
   <td>Recherche un digit (0, 1, 2, ... 9).</td>
  </tr>
  <tr>
   <td>\w</td>
   <td>Recherche un caractère de mot, c'est-à-dire tout caractère dans l'alphabet (majuscule ou minuscule), un digit ou un underscore (_).</td>
  </tr>
  <tr>
   <td>+</td>
   <td>Recherche au moins une occurence du caractère précédent. Par exemple, pour rechercher au moins 1 digit, vous utiliseriez <code>\d+</code>. Pour rechercher au moins 1 caractère "a", vous utiliseriez <code>a+</code>.</td>
  </tr>
  <tr>
   <td>*</td>
   <td>Recherche zéro ou plus occurrence(s) du caractère précédent. Par exemple, pour rechercher "rien ou un mot", vous pourriez utiliser <code>\w*</code>.</td>
  </tr>
  <tr>
   <td>( )</td>
   <td>Capture la partie du pattern contenue dans les parenthèses. Toutes les valeurs capturées seront passées à la vue en tant que paramètres non nommés (si plusieurs patterns sont capturés, les paramètres associés seront fournis dans l'ordre de déclaration des captures).</td>
  </tr>
  <tr>
   <td>(?P&lt;<em>name</em>&gt;...)</td>
   <td>Capture le pattern (indiqué par …) en tant que variable nommée (dans ce cas "name"). Les valeurs capturées sont passées à la vue avec le nom spécifié. Votre vue doit par conséquent déclarer un argument avec le même nom !</td>
  </tr>
  <tr>
   <td>[  ]</td>
   <td>Recherche l'un des caractères contenus dans cet ensemble. Par exemple, [abc] va rechercher "a" ou "b" ou "c". [-\w] va rechercher le caractère "-" ou tout caractère de mot.</td>
  </tr>
 </tbody>
</table>

<p>La plupart des autres caractères peuvent être pris littéralement.</p>

<p>Considérons quelques exemples réels de patterns :</p>

<table class="standard-table">
 <thead>
  <tr>
   <th scope="col">Pattern</th>
   <th scope="col">Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><strong>r'^book/(?P&lt;pk&gt;\d+)$'</strong></td>
   <td>
    <p>C'est là l'expression régulière utilisée dans notre mappeur d'URL. Elle recherche une chaîne qui a <code>book/</code> au commencement de la ligne (<strong>^book/</strong>), ensuite a au moins 1 digit (<code>\d+</code>), et enfin se termine (avec aucun caractère non-digit avant la fin du marqueur de ligne).</p>

    <p>Elle capture aussi tous les digits <strong>(?P&lt;pk&gt;\d+)</strong> et les passe à la vue dans un paramètre appelé 'pk'. <strong>Les valeurs capturées sont toujours passées comme des chaînes !</strong></p>

    <p>Par exemple, cette expression régulière trouverait une correspondance dans l'URL <code>book/1234</code>, et enverrait alors une variable <code>pk='1234'</code> à la vue.</p>
   </td>
  </tr>
  <tr>
   <td><strong>r'^book/(\d+)$'</strong></td>
   <td>
    <p>Ceci rechercher la même URL que dans le cas précédent. L'information capturée serait envoyée à la vue en tant qu'argument non nommé.</p>
   </td>
  </tr>
  <tr>
   <td><strong>r'^book/(?P&lt;stub&gt;[-\w]+)$'</strong></td>
   <td>
    <p>Ceci recherche une chaîne qui a <code>book/</code> au commencement de la ligne (<strong>^book/</strong>), ensuite a au moins 1 caractère étant <em>soit</em> un '-', <em>soit</em> un caractère de mot (<strong>[-\w]+</strong>), puis la fin. Ce pattern capture aussi cet ensemble de caractères et le passe à la vue en tant que paramètre nommé 'stub'.</p>

    <p>Ceci est un pattern relativement typique pour un "stub". Les stubs sont des clés primaires basées sur des mots (plus agréables que des IDs) pour retrouver des données. Vous pouvez utiliser un stub si vous voulez que votre URL de livre contienne plus d'informations. Par exemple <code>/catalog/book/the-secret-garden</code>, plutôt que <code>/catalog/book/33</code>.</p>
   </td>
  </tr>
 </tbody>
</table>

<p>Vous pouvez capturer plusieurs patterns en une seule fois, et donc encoder beaucoup d'informations différentes dans l'URL.</p>

<div class="note">
<p><strong>Note :</strong> Comme défi, essayez d'envisager comment vous devriez encoder une URL pour lister tous les livres sortis en telle année, à tel mois et à tel jour, et l'expression régulière qu'il faudrait utiliser pour la rechercher.</p>
</div>

<h4 id="Passer_des_options_supplémentaires_dans_vos_mappages_dURL">Passer des options supplémentaires dans vos mappages d'URL</h4>

<p>Une fonctionnalité que nous n'avons pas utilisée ici, mais que vous pourriez trouver valable, c'est que vous pouvez passer à la vue des <a href="https://docs.djangoproject.com/en/2.1/topics/http/urls/#views-extra-options">options supplémentaires</a>. Les options sont déclarées comme un dictionnaire que vous passez comme troisième argument (non nommé) à la fonction <code>path()</code>. Cette approche peut être utile si vous voulez utiliser la même vue pour des ressources multiples, et passer des données pour configurer son comportement dans chaque cas (ci-dessous nous fournissons un template différent dans chaque cas).</p>

<pre class="brush: python">path('url/', views.my_reused_view, <strong>{'my_template_name': 'some_path'}</strong>, name='aurl'),
path('anotherurl/', views.my_reused_view, <strong>{'my_template_name': 'another_path'}</strong>, name='anotherurl'),
</pre>

<div class="note">
<p><strong>Note :</strong> Les options supplémentaires aussi bien que les patterns capturés sont passés à la vue comme arguments <em>nommés</em>. Si vous utilisez le<strong> </strong><strong>même nom</strong> pour un pattern capturé et une option supplémentaire, alors seul la value du pattern capturé sera envoyé à la vue (la valeur spécifiée dans l'option supplémentaire sera abandonnée).</p>
</div>

<h3 id="Vue_basée_sur_classe_2">Vue (basée sur classe)</h3>

<p>Ouvrez <strong>catalog/views.py</strong>, et copiez-y le code suivant à la fin du fichier :</p>

<pre class="brush: python">class BookDetailView(generic.DetailView):
    model = Book</pre>

<p>C'est tout ! La seule chose que vous avez à faire maintenant, c'est créer un template appelé <strong>/locallibrary/catalog/templates/catalog/book_detail.html</strong>, et la vue va lui passer les informations de la base de donnée concernant l'enregistrement <code>Book</code> spécifique, extrait par le mapper d'URL. À l'intérieur du template, vous pouvez accéder à la liste de livres via la variable de template appelée <code>object</code> OU <code>book</code> (c'est-à-dire, de manière générique, "le_nom_du_modèle").</p>

<p>Si vous en avez besoin, vous pouvez changer le template utilisé et le nom de l'objet-contexte utilisé pour désigner le livre dans le template. Vous pouvez aussi renommer les méthodes pour, par exemple, ajouter des informations supplémentaires au contexte.</p>

<h4 id="Que_se_passe-t-il_si_lenregistrement_nexiste_pas">Que se passe-t-il si l'enregistrement n'existe pas ?</h4>

<p>Si l'enregistrement demandé n'existe pas, alors la vue générique basée sur classe "détail" va lever automatiquement pour vous une exception <code>Http404</code> — en production, cela va automatiquement afficher une page appropriée "ressource non trouvée", que vous pouvez personnaliser si besoin.</p>

<p>Juste pour vous donner une idée de la manière dont tout cela fonctionne, le morceau de code ci-dessous montre comment vous implémenteriez cette vue comme une fonction si vous n'utilisiez <strong>pas</strong> la vue générique basée sur classe "détail".</p>

<pre class="brush: python">def book_detail_view(request, primary_key):
    try:
        book = Book.objects.get(pk=primary_key)
    except Book.DoesNotExist:
        raise Http404('Book does not exist')

    return render(request, 'catalog/book_detail.html', context={'book': book})
</pre>

<p>La vue essaie d'abord d'obtenir du modèle l'enregistrement correspondant au livre spécifié. Si cela échoue, la vue devrait lever une exception <code>Http404</code> pour indiquer que le livre est "non trouvé". L'étape finale est ensuite, comme d'habitude, d'appeler <code>render()</code> avec le nom du template et les données concernant le livre dans le paramètre <code>context</code> (comme un dictionnaire).</p>

<p>Une alternative consiste à utiliser la fonction <code>get_object_or_404()</code> comme un raccourci pour lever une exception <code>Http404</code> si l'enregistrement n'existe pas.</p>

<pre class="brush: python">from django.shortcuts import get_object_or_404

def book_detail_view(request, primary_key):
    book = get_object_or_404(Book, pk=primary_key)
    return render(request, 'catalog/book_detail.html', context={'book': book})</pre>

<h3 id="Créerle_template_de_la_Vue_Détail">Créerle template de la Vue Détail</h3>

<p>Créez le fichier HTML <strong>/locallibrary/catalog/templates/catalog/book_detail.html</strong>, et copiez-y le code ci-dessous. Comme on l'a dit plus haut, c'est là le nom de template attendu par défaut par la vue générique basée sur classe <em>detail</em> (pour un modèle appelé <code>Book</code> dans une application appelée <code>catalog</code>).</p>

<pre class="brush: html">{% extends "base_generic.html" %}

{% block content %}
  &lt;h1&gt;Title: \{{ book.title }}&lt;/h1&gt;

  &lt;p&gt;&lt;strong&gt;Author:&lt;/strong&gt; &lt;a href=""&gt;\{{ book.author }}&lt;/a&gt;&lt;/p&gt; &lt;!-- author detail link not yet defined --&gt;
  &lt;p&gt;&lt;strong&gt;Summary:&lt;/strong&gt; \{{ book.summary }}&lt;/p&gt;
  &lt;p&gt;&lt;strong&gt;ISBN:&lt;/strong&gt; \{{ book.isbn }}&lt;/p&gt;
  &lt;p&gt;&lt;strong&gt;Language:&lt;/strong&gt; \{{ book.language }}&lt;/p&gt;
  &lt;p&gt;&lt;strong&gt;Genre:&lt;/strong&gt; \{{ book.genre.all|join:", " }}&lt;/p&gt;

  &lt;div style="margin-left:20px;margin-top:20px"&gt;
    &lt;h4&gt;Copies&lt;/h4&gt;

    {% for copy in book.bookinstance_set.all %}
      &lt;hr&gt;
      &lt;p class="{% if copy.status == 'a' %}text-success{% elif copy.status == 'm' %}text-danger{% else %}text-warning{% endif %}"&gt;
        \{{ copy.get_status_display }}
      &lt;/p&gt;
      {% if copy.status != 'a' %}
        &lt;p&gt;&lt;strong&gt;Due to be returned:&lt;/strong&gt; \{{ copy.due_back }}&lt;/p&gt;
      {% endif %}
      &lt;p&gt;&lt;strong&gt;Imprint:&lt;/strong&gt; \{{ copy.imprint }}&lt;/p&gt;
      &lt;p class="text-muted"&gt;&lt;strong&gt;Id:&lt;/strong&gt; \{{ copy.id }}&lt;/p&gt;
    {% endfor %}
  &lt;/div&gt;
{% endblock %}</pre>

<ul>
</ul>

<div class="note">
<p><strong>Note :</strong> Le lien vers l'auteur dans le template ci-dessus est vide, parce que nous n'avons pas encore crée de page détail pour un auteur. Une fois que cette page sera créée, vous pourrez remplacer l'URL par ceci :</p>

<pre>&lt;a href="<strong>{% url 'author-detail' book.author.pk %}</strong>"&gt;\{{ book.author }}&lt;/a&gt;
</pre>
</div>

<p>Bien qu'en un peu plus grand, presque tout ce qu'il y a dans ce template a été décrit précédemment :</p>

<ul>
 <li>Nous étendons notre template de base et récrivons le block "content".</li>
 <li>Nous utilisons une procédure conditionnelle pour déterminer s'il faut ou non afficher tel contenu spécifique.</li>
 <li>Nous utilisons une boucle <code>for</code> pour boucler sur des listes d'objets.</li>
 <li>Nous accédons aux champs du contexte en utilisant la notation à point (parce que nous avons utilisé la vue générique <em>detail</em>, le contexte est nommé <code>book</code> ; nous pourrions aussi utiliser "<code>object</code>").</li>
</ul>

<p>Une chose intéressante que nous n'avons pas encore vue, c'est la fonction <code>book.bookinstance_set.all()</code>. Cette méthode est "automagiquement" construite par Django pour retourner l'ensemble des enregistrements <code>BookInstance</code> associés à un <code>Book</code> particulier.</p>

<pre class="brush: python">{% for copy in book.bookinstance_set.all %}
  &lt;!-- code to iterate across each copy/instance of a book --&gt;
{% endfor %}</pre>

<p>Cette méthode est requise parce que vous déclarez un champ <code>ForeignKey</code> (one-to-many) seulement du côté "one" de la relation. Comme vous ne faites rien pour déclarer la relation dans les modèles opposés ("many"), Django n'a pas de champ pour récupérer l'ensemble des enregistrements associés. Pour remédier à ce problème, Django construit une fonction justement nommée "recherche inversée", que vous pouvez utiliser. Le nom de la fonction est construit en mettant en minuscule le nom du modèle où a été déclarée la <code>ForeignKey</code>, suivi de <code>_set</code> (ainsi la fonction créée dans <code>Book</code> est <code>bookinstance_set()</code>).</p>

<div class="note">
<p><strong>Note :</strong> Ici nous utilisons <code>all()</code> pour récupérer tous les enregistrements (comportement par défaut). Bien que vous puissiez utiliser la méthode <code>filter()</code> pour obtenir un sous-ensemble d'enregistrements dans le code, vous ne pouvez faire cela directement dans le template, parce que vous ne pouvez pas spécifier d'arguments dans les fonctions.</p>

<p>Prenez garde également que, si vous ne définissez pas un ordre (dans votre vue basée sur classe ou votre modèle), vous allez voir des erreurs de ce genre en provenance du serveur de développement :</p>

<pre>[29/May/2017 18:37:53] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
/foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: &lt;QuerySet [&lt;Author: Ortiz, David&gt;, &lt;Author: H. McRaven, William&gt;, &lt;Author: Leigh, Melinda&gt;]&gt;
  allow_empty_first_page=allow_empty_first_page, **kwargs)
</pre>

<p>Ceci vient du fait que l'<a href="https://docs.djangoproject.com/en/2.1/topics/pagination/#paginator-objects">objet paginator</a> s'attend à ce qu'un ORDER BY soit exécuté sur votre base de données sous-jacente. Sans cela il ne peut pas être sûr que les enregistrements retournés sont vraiment dans le bon ordre !</p>

<p>Ce tutoriel n'a pas (encore !) traité de la <strong>pagination</strong>, mais comme vous ne pouvez pas utiliser <code>sort_by()</code> et passer un paramètre (pour la même raison que le <code>filter()</code> décrit précédemment), vous avez le choix entre trois options :</p>

<ol>
 <li>Ajouter un <code>ordering</code> lors de la déclaration de la <code>class Meta</code> dans votre modèle.</li>
 <li>Ajouter un attribut <code>queryset</code> dans votre vue personnalisée basée sur classe, en spécifiant un <code>order_by()</code>.</li>
 <li>Ajouter une méthode <code>get_queryset</code> à votre vue personnalisée basée sur classe, et préciser de même un <code>order_by()</code>.</li>
</ol>

<p>Si vous décidez d'ajouter une <code>class Meta</code> au modèle <code>Author</code> (solution peut-être pas aussi flexible que personnalier la vue basée sur classe, mais assez facile), vous allez vous retrouver avec quelque chose de ce genre :</p>

<pre>class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    def get_absolute_url(self):
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        return f'{self.last_name}, {self.first_name}'

<strong>    class Meta:
        ordering = ['last_name']</strong></pre>

<p>Bien sûr le champ n'est pas forcément <code>last_name</code> : ce pourrait être un autre champ.</p>

<p>Dernier point, mais non le moindre : vous devriez trier les données par un attribut/colonne qui a réellement un index (unique ou pas) dans votre base de données, afin d'éviter des problèmes de performance. Bien sûr ce n'est pas requis ici (ce serait un peu exagéré avec si peu de livres et d'utilisateurs), mais il vaut mieux avoir cela à l'esprit pour de futurs projets.</p>
</div>

<h2 id="À_quoi_cela_ressemble-t-il_2">À quoi cela ressemble-t-il ?</h2>

<p>À ce point, nous devrions avoir créé tout ce qu'il faut pour afficher à la fois la liste des livres et les pages de détail pour chaque livre. Lancez le serveur (<code>python3 manage.py runserver</code>) et ouvrez votre navigateur à l'adresse <a href="http://127.0.0.1:8000/">http://127.0.0.1:8000/</a>.</p>

<div class="warning">
<p><strong>Attention :</strong> Ne cliquez pas sur les liens vers le détail des auteurs : vous allez les créer lors du prochain défi !</p>
</div>

<p>Cliquez sur le lien <strong>Tous les livres</strong> pour afficher la liste des livres.</p>

<p><img alt="Book List Page" src="book_list_page_no_pagination.png"></p>

<p>Ensuite cliquez sur un lien dirigeant vers l'un de vos livres. Si tout est réglé correctement, vous allez voir quelque chose de semblable à la capture d'écran suivante :</p>

<p><img alt="Book Detail Page" src="book_detail_page_no_pagination.png"></p>

<h2 id="Pagination">Pagination</h2>

<p>Si vous avez seulement quelques enregistrements, notre page de liste de livres aura une bonne apparence. Mais si vous avez des dizaines ou des centaines d'enregistrements, la page va progressivement devenir plus longue à charger (et aura beaucoup trop de contenu pour naviguer de manière raisonnable). La solution à ce problème est d'ajouter une pagination à vos vues listes, en réduisant le nombre d'éléments affichés sur chaque page.</p>

<p>Django a d'excellents outils pour la pagination. Mieux encore, ces outils sont intégrés dans les vues listes génériques basées sur classes, aussi n'avez-vous pas grand chose à faire pour les activer !</p>

<h3 id="Vues">Vues</h3>

<p>Ouvrez <strong>catalog/views.py</strong>, et ajoutez la ligne <code>paginate_by</code>, en gras ci-dessous.</p>

<pre class="brush: python">class BookListView(generic.ListView):
    model = Book
    <strong>paginate_by = 10</strong></pre>

<p>Avec cet ajout, dès que vous aurez plus de 10 enregistrements, la vue démarrera la pagination des données qu'elle envoie au template. Les différentes pages sont obtenues en utilisant le paramètre GET : pour obtenir la page 2, vous utiliseriez l'URL <code>/catalog/books/<strong>?page=2</strong></code>.</p>

<h3 id="Templates">Templates</h3>

<p>Maintenant que les données sont paginées, nous avons besoin d'ajouter un outil au template pour parcourir l'ensemble des résultats. Et parce que nous voudrons sûrement faire cela pour toutes les listes vues, nous allons le faire d'une manière qui puisse être ajoutée au template de base.</p>

<p>Ouvrez <strong>/locallibrary/catalog/templates/<em>base_generic.html</em></strong>, et copiez-y, sous notre bloc de contenu, le bloc de pagination suivant (mis en gras ci-dessous). Le code commence par vérifier si une pagination est activée sur la page courante. Si oui, il ajoute les liens "précédent" et "suivant" appropriés (et le numéro de la page courante).</p>

<pre class="brush: python">{% block content %}{% endblock %}

<strong>  {% block pagination %}
    {% if is_paginated %}
        &lt;div class="pagination"&gt;
            &lt;span class="page-links"&gt;
                {% if page_obj.has_previous %}
                    &lt;a href="\{{ request.path }}?page=\{{ page_obj.previous_page_number }}"&gt;previous&lt;/a&gt;
                {% endif %}
                &lt;span class="page-current"&gt;
                    Page \{{ page_obj.number }} of \{{ page_obj.paginator.num_pages }}.
                &lt;/span&gt;
                {% if page_obj.has_next %}
                    &lt;a href="\{{ request.path }}?page=\{{ page_obj.next_page_number }}"&gt;next&lt;/a&gt;
                {% endif %}
            &lt;/span&gt;
        &lt;/div&gt;
    {% endif %}
  {% endblock %} </strong></pre>

<p>Le <code>page_obj</code> est un objet <a href="https://docs.djangoproject.com/en/2.1/topics/pagination/#paginator-objects">Paginator</a> qui n'existera que si une pagination est utilisée dans la page courante. Cet objet vous permet de récupérer toutes les informations sur la page courante, les pages précédentes, combien il y a de pages au total, etc.</p>

<p>Nous utilisons <code>\{{ request.path }}</code> pour récupérer l'URL de la page courante, afin de créer les liens de pagination. Cela est utile, car cette variable est indépendante de l'objet que nous sommes en train de paginer.</p>

<p>C'est tout !</p>

<h3 id="À_quoi_cela_ressemble-t-il_3">À quoi cela ressemble-t-il ?</h3>

<p>La capture d'écran ci-dessous montre à quoi ressemble la pagination. Si vous n'avez pas entré plus de 10 titres dans votre base de données, vous pouvez tester plus facilement cette pagination en diminuant le nombre spécifié à la ligne <code>paginate_by</code> dans votre  fichier <strong>catalog/views.py</strong>. Pour obtenir le résultat ci-dessous, nous avons changé la ligne en <code>paginate_by = 2</code>.</p>

<p>Les liens de pagination sont affichés en bas de la page, avec les liens suivant/précédent affichés selon la page sur laquelle nous nous trouvons.</p>

<p><img alt="Book List Page - paginated" src="book_list_paginated.png"></p>

<h2 id="Mettez-vous_vous-même_au_défi_!">Mettez-vous vous-même au défi !</h2>

<p>Le challenge dans cet article consiste à créer les vues détail et liste nécessaires à l'achèvement du projet. Ces pages devront être accessibles aux URLs suivantes :</p>

<ul>
 <li><code>catalog/authors/</code> — La liste de tous les auteurs.</li>
 <li><code>catalog/author/<em>&lt;id&gt;</em></code><em> </em>— La vue détail pour un auteur précis, avec un champ clé-primaire appelé <em><code>&lt;id&gt;</code></em>.</li>
</ul>

<p>Le code requis pour le mappeur d'URL et les vues sera virtuellement identique aux vues liste et détail du modèle <code>Book</code>, créées ci-dessus. Les templates seront différents, mais auront un comportement semblable.</p>

<div class="note">
<p><strong>Note :</strong></p>

<ul>
 <li>Une fois que vous aurez créé le mappeur d'URL pour la page "liste d'auteurs", vous aurez besoin de mettre aussi à jour le lien <strong>All authors</strong> dans le template de base. Suivez la <a href="#Update_the_base_template">même procédure</a> que celle adoptée quand nous avons mis à jour le lien <strong>All books</strong>.</li>
 <li>Une fois créé le mappeur d'URL pour la page de détails sur l'auteur, vous devrez aussi mettre à jour le <a href="#Creating_the_Detail_View_template">template de la vue détail d'un livre</a> (<strong>/locallibrary/catalog/templates/catalog/book_detail.html</strong>), de sorte que le lien vers l'auteur pointe vers votre nouvelle page de détails sur l'auteur (au lieu d'être une URL vide). La ligne va avoir comme changement la balise montrée en gras ci-dessous.
  <pre class="brush: html">&lt;p&gt;&lt;strong&gt;Author:&lt;/strong&gt; &lt;a href="<strong>{% url 'author-detail' book.author.pk %}</strong>"&gt;\{{ book.author }}&lt;/a&gt;&lt;/p&gt;
</pre>
 </li>
</ul>
</div>

<p>Quand vous aurez fini, vos pages vont ressembler aux captures d'écran suivantes.</p>

<p><img alt="Author List Page" src="author_list_page_no_pagination.png"></p>

<ul>
</ul>

<p><img alt="Author Detail Page" src="author_detail_page_no_pagination.png"></p>

<ul>
</ul>

<h2 id="Résumé">Résumé</h2>

<p>Félicitations ! Notre application basique pour bibliothèque est maintenant terminée.</p>

<p>Dans cet article, nous avons appris comment utiliser les vues génériques basées sur classe "liste" et "détail", et nous les avons utilisées pour créer des pages permettant de voir nos livres et nos auteurs. Au passage nous avons appris la recherche d'un pattern d'URL grâce aux expressions régulières, et la manière de passer des données depuis les URLs vers les vues. Nous avons aussi appris quelques trucs supplémentaires pour mieux utiliser les templates. Et en dernier nous vous avons montré comment paginer les vues liste, de façon à pouvoir gérer des listes même avec beaucoup d'enregistrements.</p>

<p>Dans les articles que nous vous présenterons ensuite, nous améliorerons cette application pour intégrer des comptes utilisateurs, et nous allons donc vous montrer comment gérer l'authentification des utilisateurs, les permissions, les sessions et les formulaires.</p>

<h2 id="Voyez_aussi">Voyez aussi</h2>

<ul>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/class-based-views/generic-display/">Built-in class-based generic views</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/ref/class-based-views/generic-display/">Generic display views</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/class-based-views/intro/">Introduction to class-based views</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/ref/templates/builtins">Built-in template tags and filters</a> (Django docs).</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/pagination/">Pagination</a> (Django docs)</li>
</ul>

<p>{{PreviousMenuNext("Learn/Server-side/Django/Home_page", "Learn/Server-side/Django/Sessions", "Learn/Server-side/Django")}}</p>

<h2 id="In_this_module">In this module</h2>

<ul>
 <li><a href="/fr/docs/Learn/Server-side/Django/Introduction">Django introduction</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/development_environment">Setting up a Django development environment</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Tutorial_local_library_website">Django Tutorial: The Local Library website</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/skeleton_website">Django Tutorial Part 2: Creating a skeleton website</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Models">Django Tutorial Part 3: Using models</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Admin_site">Django Tutorial Part 4: Django admin site</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Home_page">Django Tutorial Part 5: Creating our home page</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Generic_views">Django Tutorial Part 6: Generic list and detail views</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Sessions">Django Tutorial Part 7: Sessions framework</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Authentication">Django Tutorial Part 8: User authentication and permissions</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Forms">Django Tutorial Part 9: Working with forms</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Testing">Django Tutorial Part 10: Testing a Django web application</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/Deployment">Django Tutorial Part 11: Deploying Django to production</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/web_application_security">Django web application security</a></li>
 <li><a href="/fr/docs/Learn/Server-side/Django/django_assessment_blog">DIY Django mini blog</a></li>
</ul>