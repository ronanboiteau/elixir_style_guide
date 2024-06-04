# [Le guide de style Elixir][Elixir Style Guide]

## Sommaire

* __[Préambule](#préambule)__
* __[À propos](#à-propos)__
* __[Mise en forme](#mise-en-forme)__
  * [Espace blanc](#espace-blanc)
  * [Indentation](#indentation)
  * [Parenthèses](#parenthèses)
  * [Syntaxe](#syntaxe)
* __[Le guide](#le-guide)__
  * [Expressions](#expressions)
  * [Nommage](#nommage)
  * [Commentaires](#commentaires)
    * [Commentaires d'annotation](#commentaires-d-annotation)
  * [Modules](#modules)
  * [Documentation](#documentation)
  * [Typespecs](#typespecs)
  * [Structs](#structs)
  * [Exceptions](#exceptions)
  * _Collections_
  * [Strings](#strings)
  * _Expressions régulières_
  * [Méta-programmation](#méta-programmation)
  * [Les tests](#les-tests)
* __[Ressources](#ressources)__
  * [Alternatives à ce guide](#alternatives-à-ce-guide)
  * [Outils](#outils)
* __[Nous aider](#nous-aider)__
  * [Contribuer](#contribuer)
  * [Passez le mot](#passez-le-mot)
* __[Reproduction](#reproduction)__
  * [Licence](#licence)
  * [Attribution](#attribution)

## Préambule

> Liquid architecture. It's like jazz — you improvise, you work together, you
> play off each other, you make something, they make something.
>
> —Frank Gehry

Le style est important.
[Elixir] peut être plein de style, mais comme pour tous les langages, il faut
suivre un style de codage pour rendre son code agréable à lire.

## À propos

Ceci est un guide de style de codage pour le
[langage de prommation Elixir][Elixir]. N'hésitez pas à ouvrir des *pull
requests* et proposer des améliorations. Rejoignez la communauté Elixir !

Si vous cherchez d'autres projets auxquels contribuer, visitez le
[site du gestionnaire de paquets Hex][Hex] (en anglais).

<a name="traductions"></a>
Ce guide est disponible dans les langues suivantes:

* [Anglais]
* [Chinois simplifié]
* [Chinois traditionnel]
* [Français]
* [Japonais]
* [Coréen]
* [Portugais]
* [Espagnol]

## Mise en forme

Elixir v1.6 a introduit les tâches [Code Formatter] et [Mix format].
Le formateur doit être privilégié pour tous les nouveaux projets et codes sources.

Les règles de cette section sont appliquées automatiquement par le formateur de code mais
sont fournies ici à titre d'exemple du style conseillé.

### Espace blanc

* <a name="espace-blanc-en-fin-de-ligne"></a>
  Ne laissez pas d'espaces blancs inutiles en fin de ligne.
  <sup>[[lien](#espace-blanc-en-fin-de-ligne)]</sup>

* <a name="ligne-vide-en-fin-de-fichier"></a>
  Chaque fichier doit se terminer par une ligne vide.
  <sup>[[lien](#ligne-vide-en-fin-de-fichier)]</sup>

* <a name="fin-de-ligne"></a>
  Utilisez des fins de ligne Unix (\*pas de problème si vous utilisez BSD,
  Solaris, Linux ou macOS, mais faites très attention si vous utilisez
  Windows).
  <sup>[[lien](#fin-de-ligne)]</sup>

* <a name="autocrlf"></a>
  Si vous utilisez Git, vous pouvez ajouter le réglage suivant pour vous
  protéger des fins de ligne Windows qui pourraient se glisser dans votre
  dépôt :
  <sup>[[lien](#autocrlf)]</sup>

  ```sh
  git config --global core.autocrlf true
  ```

* <a name="espaces"></a>
  Utilisez des espaces autour des opérateurs et après les vigules, deux-points
  et point-virgules.
  Ne mettez pas d'espace autour des paires de parenthèses, accolades, etc.
  Ces espaces sont parfois inutiles à la bonne exécution de votre code, mais
  ils le rendent bien plus facile à lire.
  <sup>[[lien](#espaces)]</sup>

  ```elixir
  sum = 1 + 2
  {a, b} = {2, 3}
  [first | rest] = [1, 2, 3]
  Enum.map(["one", <<"two">>, "three"], fn num -> IO.puts(num) end)
  ```

* <a name="pas-d-espaces"></a>
  Ne mettez pas d'espace après les opérateurs qui ne sont pas un mot qui ne ne
  prennent qu'un seul argument, ou autour de l'opérateur de gamme (`..`).
  <sup>[[lien](#pas-d-espaces)]</sup>

  ```elixir
  0 - 1 == -1
  ^pinned = some_func()
  5 in 1..10
  ```

* <a name="def-espacement"></a>
  Placez des lignes vides à l'intérieur des `def` pour diviser vos fonctions en
  paragraphes logiques.
  <sup>[[lien](#def-espacement)]</sup>

  ```elixir
  def some_function(some_data) do
    some_data |> other_function() |> List.first()
  end

  def some_function do
    result
  end

  def some_other_function do
    another_result
  end

  def a_longer_function do
    one
    two

    three
    four
  end
  ```

* <a name="defmodule-ligne-vide"></a>
  Ne laissez pas de ligne vide après `defmodule`.
  <sup>[[lien](#defmodule-ligne-vide)]</sup>

* <a name="do-longs"></a>
  Si l'en-tête de votre fonction et la clause `do:` sont trop longs pour tenir
  sur une seule ligne, placez la clause `do:` sur une nouvelle ligne indentée
  d'un niveau de plus que la ligne précédente.
  <sup>[[lien](#do-longs)]</sup>

  ```elixir
  def some_function([:foo, :bar, :baz] = args),
    do: Enum.map(args, fn arg -> arg <> " is on a very long line!" end)
  ```

  Lorsque la clause `do:` est sur une nouvelle ligne, traitez votre fonction
  comme une fonction de plusieurs lignes et séparez-la donc des autres fonctions
  par une ligne vide.

  ```elixir
  # déconseillé
  def some_function([]), do: :empty
  def some_function(_),
    do: :very_long_line_here

  # conseillé
  def some_function([]), do: :empty

  def some_function(_),
    do: :very_long_line_here
  ```

* <a name="ligne-vide-apres-affectation-sur-plusieurs-lignes"></a>
  Ajoutez une ligne vide après une affectation sur plusieurs lignes. Cela permet
  de signaler visuellement la fin de l'affectation.
  <sup>[[lien](#ligne-vide-apres-affectation-sur-plusieurs-lignes)]</sup>

  ```elixir
  # déconseillé
  some_string =
    "Hello"
    |> String.downcase()
    |> String.strip()
  another_string <> some_string

  # conseillé
  some_string =
    "Hello"
    |> String.downcase()
    |> String.strip()

  another_string <> some_string
  ```

  ```elixir
  # déconseillé aussi
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end
  String.downcase(something)

  # conseillé
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end

  String.downcase(something)
  ```

* <a name="enums-sur-plusieurs-lignes"></a>
  Si une _list_, _map_ ou _struct_ prend plusieurs lignes, placez chaque élément
  ainsi que les accolades/crochets ouvrants et fermants sur des lignes séparées.
  Indentez chaque élément d'un niveau, mais pas les accolades/crochets.
  <sup>[[lien](#enums-sur-plusieurs-lignes)]</sup>

  ```elixir
  # déconseillé
  [:first_item, :second_item, :next_item,
  :final_item]

  # conseillé
  [
    :first_item,
    :second_item,
    :next_item,
    :final_item
  ]
  ```

* <a name="multiline-list-assign"></a>
  Lorsque vous déclarez une _list_, _map_ ou _struct_, laissez le crochet ou
  l'accolade ouvrante sur la même ligne que le début de l'affectation.
  <sup>[[lien](#multiline-list-assign)]</sup>

  ```elixir
  # déconseillé
  list =
  [
    :first_item,
    :second_item
  ]

  # conseillé
  list = [
    :first_item,
    :second_item
  ]
  ```

* <a name="clauses-case-sur-plusieurs-lignes"></a>
  Quand une clause `case` ou `cond` prend plusieurs lignes, séparez chaque cas
  par une ligne vide.
  <sup>[[lien](#clauses-case-sur-plusieurs-lignes)]</sup>

  ```elixir
  # déconseillé
  case arg do
    true ->
      :ok
    false ->
      :error
  end

  # conseillé
  case arg do
    true ->
      :ok

    false ->
      :error
  end
  ```

* <a name="commentaires-au-dessus-de-la-ligne"></a>
  Placez les commentaires au-dessus de la ligne concernée.
  <sup>[[lien](#commentaires-au-dessus-de-la-ligne)]</sup>

  ```elixir
  String.first(some_string) # déconseillé

  # conseillé
  String.first(some_string)
  ```

* <a name="espace-apres-dieze-commentaire"></a>
  Mettez un espace entre le `#` et le texte du commentaire.
  <sup>[[lien](#espace-apres-dieze-commentaire)]</sup>

  ```elixir
  #déconseillé
  String.first(some_string)

  # conseillé
  String.first(some_string)
  ```

### Indentation

* <a name="clauses-with"></a>
  Indentez et alignez les clauses `with` successives.
  Placez l'argument `do:` sur une nouvelle ligne, aligné avec les clauses
  précédentes.
  <sup>[[lien](#clauses-with)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar),
       do: {:ok, foo, bar}
  ```

* <a name="with-else"></a>
  Si un `with` a un bloc `do` possédant plus d'une ligne, ou a une option
  `else`, utilisez la syntaxe multilignes.
  <sup>[[lien](#with-else)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar) do
    {:ok, foo, bar}
  else
    :error ->
      {:error, :bad_arg}
  end
  ```

### Parenthèses

* <a name="operateur-pipe-et-parentheses"></a>
  Laissez des parenthèses, même vides, aux fonctions que vous enchaînez grâce à
  l'opérateur pipe (`|>`).
  <sup>[[lien](#operateur-pipe-et-parentheses)]</sup>

  ```elixir
  # déconseillé
  some_string |> String.downcase |> String.strip

  # conseillé
  some_string |> String.downcase() |> String.strip()
  ```

* <a name="nom-de-fonction-et-parentheses"></a>
  Ne mettez pas d'espace entre le nom d'une fonction et la parenthèse ouvrante.
  <sup>[[lien](#nom-de-fonction-et-parentheses)]</sup>

  ```elixir
  # déconseillé
  f (3 + 2)

  # conseillé
  f(3 + 2)
  ```

* <a name="appels-de-fonctions-et-parentheses"></a>
  Mettez des parenthèses lorsque vous appelez une fonction, surtout dans un
  pipeline.
  <sup>[[lien](#appels-de-fonctions-et-parentheses)]</sup>

  ```elixir
  # déconseillé
  f 3

  # conseillé
  f(3)

  # déconseillé et est interprété comme rem(2, (3 |> g)), ce qui
  # n'est pas ce que vous souhaitez !
  2 |> rem 3 |> g

  # conseillé
  2 |> rem(3) |> g
  ```

### Syntaxe

* <a name="syntaxe-liste-de-mots-cles"></a>
  Utilisez toujours la syntaxe spéciale pour les listes de mots-clés.
  <sup>[[lien](#syntaxe-liste-de-mots-cles)]</sup>

  ```elixir
  # déconseillé
  some_value = [{:a, "baz"}, {:b, "qux"}]

  # conseillé
  some_value = [a: "baz", b: "qux"]
  ```

* <a name="crochets-et-liste-de-mots-cles"></a>
  Ne mettez pas de crochets dans les listes de mots-clés lorsqu'ils sont
  optionnels.
  <sup>[[lien](#crochets-et-liste-de-mots-cles)]</sup>

  ```elixir
  # déconseillé
  some_function(foo, bar, [a: "baz", b: "qux"])

  # conseillé
  some_function(foo, bar, a: "baz", b: "qux")
  ```

## Le guide

Les règles dans cette section peuvent ne pas être appliquées par le formateur de code,
mais elles constituent généralement les bonnes pratiques.

### Expressions

* <a name="def-d-une-ligne"></a>
  Placez les `def` d'une seule ligne correspondant à une même fonction ensemble.
  Mais séparez d'une ligne vide les `def` de plusieurs lignes.
  <sup>[[lien](#def-d-une-ligne)]</sup>

  ```elixir
  def some_function(nil), do: {:error, "No Value"}
  def some_function([]), do: :ok

  def some_function([first | rest]) do
    some_function(rest)
  end
  ```

* <a name="plusieurs-fonctions-defs"></a>
  Si vous avez plus d'un `def` de plusieurs lignes, ne faites pas de `def` d'une
  seule ligne.
  <sup>[[lien](#plusieurs-fonctions-defs)]</sup>

  ```elixir
  def some_function(nil) do
    {:error, "No Value"}
  end

  def some_function([]) do
    :ok
  end

  def some_function([first | rest]) do
    some_function(rest)
  end

  def some_function([first | rest], opts) do
    some_function(rest, opts)
  end
  ```

* <a name="operateur-pipe"></a>
  Utilisez l'opérateur pipe (`|>`) pour enchaîner les fonctions.
  <sup>[[lien](#operateur-pipe)]</sup>

  ```elixir
  # déconseillé
  String.strip(String.downcase(some_string))

  # conseillé
  some_string |> String.downcase() |> String.strip()

  # L'indentation des pipelines de plusieurs lignes doit rester
  # au même niveau
  some_string
  |> String.downcase()
  |> String.strip()

  # Les pipelines de plusieurs lignes à la droite d'un pattern
  # match doivent être indentés sur une nouvelle ligne
  sanitized_string =
    some_string
    |> String.downcase()
    |> String.strip()
  ```

  Bien que ce soit la méthode conseillée, gardez en tête que copier-coller
  un pipeline de plusieurs lignes dans IEx peut donner une erreur de syntaxe,
  IEx évaluant la première ligne sans réaliser que la ligne suivante commence
  par un opérateur pipe. Pour éviter cela, vous pouvez mettre le code collé
  entre parenthèses.

* <a name="eviter-les-pipes-uniques"></a>
  Évitez d'utiliser l'opérateur pipe une seule fois.
  <sup>[[lien](#eviter-les-pipes-uniques)]</sup>

  ```elixir
  # déconseillé
  some_string |> String.downcase()

  # conseillé
  String.downcase(some_string)
  ```

* <a name="variable-seule"></a>
  Utilisez une variable seule pour débuter un enchaînement de fonctions.
  <sup>[[lien](#variable-seule)]</sup>

  ```elixir
  # déconseillé
  String.strip(some_string) |> String.downcase() |> String.codepoints()

  # conseillé
  some_string |> String.strip() |> String.downcase() |> String.codepoints()
  ```

* <a name="parentheses"></a>
  Mettez des parenthèses lorsqu'un `def` a des arguments, et n'en mettez pas
  lorsqu'il n'a pas d'arguments.
  <sup>[[lien](#parentheses)]</sup>

  ```elixir
  # déconseillé
  def some_function arg1, arg2 do
    # corps de la fonction
  end

  def some_function() do
    # corps de la fonction
  end

  # conseillé
  def some_function(arg1, arg2) do
    # corps de la fonction
  end

  def some_function do
    # corps de la fonction
  end
  ```

* <a name="do-avec-if-unless-d-une-seule-ligne"></a>
  Utilisez `do:` pour les conditions `if`/`unless` d'une seule ligne.
  <sup>[[lien](#do-avec-if-unless-d-une-seule-ligne)]</sup>

  ```elixir
  # conseillé
  if some_condition, do: # quelque_chose
  ```

* <a name="unless-avec-else"></a>
  N'utilisez jamais `unless` avec `else`.
  Remplacez ces conditions par des `if` en testant le cas positif en premier.
  <sup>[[lien](#unless-avec-else)]</sup>

  ```elixir
  # déconseillé
  unless success do
    IO.puts('failure')
  else
    IO.puts('success')
  end

  # conseillé
  if success do
    IO.puts('success')
  else
    IO.puts('failure')
  end
  ```

* <a name="true-en-derniere-condition"></a>
  Utilisez `true` en tant que dernière condition de la clause `cond` lorsque
  vous souhaitez intercepter toutes les valeurs dans votre `cond`.
  <sup>[[lien](#true-en-derniere-condition)]</sup>

  ```elixir
  # déconseillé
  cond do
    1 + 2 == 5 ->
      "Nope"

    1 + 3 == 5 ->
      "Uh, uh"

    :else ->
      "OK"
  end

  # conseillé
  cond do
    1 + 2 == 5 ->
      "Nope"

    1 + 3 == 5 ->
      "Uh, uh"

    true ->
      "OK"
  end
  ```

* <a name="parentheses-et-fonctions-sans-arguments"></a>
  Utilisez des parenthèses pour les appels de fonctions sans arguments, pour les
  distinguer des variables.
  À partir d'Elixir 1.4, le compilateur signale les endroits où il existe une
  ambiguïté entre nom de fonction et nom de variable.
  <sup>[[lien](#parentheses-et-fonctions-sans-arguments)]</sup>

  ```elixir
  defp do_stuff, do: ...

  # déconseillé
  def my_func do
    # est-ce une variable ou un appel de fonction ?
    do_stuff
  end

  # conseillé
  def my_func do
    # il s'agit clairement d'un appel de fonction
    do_stuff()
  end
  ```

### Nommage

* <a name="snake-case"></a>
  Utilisez la convention [`snake_case`][Snake case] pour nommer les atomes,
  fonctions et variables.
  <sup>[[lien](#snake-case)]</sup>

  ```elixir
  # déconseillé
  :"some atom"
  :SomeAtom
  :someAtom

  someVar = 5

  def someFunction do
    ...
  end

  # conseillé
  :some_atom

  some_var = 5

  def some_function do
    ...
  end
  ```

* <a name="camel-case"></a>
  Utilisez la convention [`CamelCase`][Camel case] pour nommer les modules.
  Laissez les accronymes comme HTTP, RFC, XML, etc. en majuscules.
  <sup>[[lien](#camel-case)]</sup>

  ```elixir
  # déconseillé
  defmodule Somemodule do
    ...
  end

  defmodule Some_Module do
    ...
  end

  defmodule SomeXml do
    ...
  end

  # conseillé
  defmodule SomeModule do
    ...
  end

  defmodule SomeXML do
    ...
  end
  ```

* <a name="noms-de-macro-predicat-avec-guards"></a>
  Les noms de macros prédicats (fonctions générées à la compilation et qui
  renvoient un booléen) _qui peuvent être utilisés entre des guards_ doivent
  posséder le préfixe `is_`.
  Pour une liste d'expressions autorisées, référez-vous à la [documention de
  Guard][Guard Expressions].
  <sup>[[lien](#noms-de-macro-predicat-avec-guards)]</sup>

  ```elixir
  defmacro is_cool(var) do
    quote do: unquote(var) == "cool"
  end
  ```

* <a name="noms-de-macro-predicat-sans-guards"></a>
  Les noms des fonctions prédicats _qui ne peuvent pas être utilisées entre
  des guards_ doivent se terminer par un point d'interrogation au lieu de
  commencer par le préfixe `is_` (ou similaire).
  <sup>[[lien](#noms-de-macro-predicat-sans-guards)]</sup>

  ```elixir
  def cool?(var) do
    # Complex check if var is cool not possible in a pure function.
  end
  ```

* <a name="fonctions-private-meme-nom-que-fonctions-public"></a>
  Les fonctions _private_ ayant le même nom que des fonctions _public_ doivent
  commencer par `do_`.
  <sup>[[lien](#fonctions-private-meme-nom-que-fonctions-public)]</sup>

  ```elixir
  def sum(list), do: do_sum(list, 0)

  # fonctions private
  defp do_sum([], total), do: total
  defp do_sum([head | tail], total), do: do_sum(tail, head + total)
  ```

### Commentaires

* <a name="code-expressif"></a>
  Écrivez du code clair dont on comprend l'objectif en lisant son flux de
  contrôle, sa structure, et les différents noms (fonctions, variables...).
  <sup>[[lien](#code-expressif)]</sup>

* <a name="grammaire-des-commentaires"></a>
  Les commentaires plus longs qu'un mot commencent par une majuscule. Utilisez
  de la ponctuation si vous faites des phrases complètes.
  Mettez [un espace][Sentence Spacing] après chaque point.
  <sup>[[lien](#grammaire-des-commentaires)]</sup>

  ```elixir
  # déconseillé
  # ce commentaire devrait commencer par une majuscule

  # conseillé
  # Example de commentaire
  # Utilisez de la ponctuation pour les phrases complètes.
  ```

#### Commentaires d'annotation

* <a name="annotations"></a>
  Les annotations devraient généralement être écrites sur la ligne juste
  au-dessus du code concerné.
  <sup>[[lien](#annotations)]</sup>

* <a name="mots-cles-d-annotation"></a>
  Les mots-clés d'annotation doivent être en majuscules et suivis de deux-points
  (`:`) et d'un espace, puis d'une note décrivant le problème.
  <sup>[[lien](#mots-cles-d-annotation)]</sup>

  ```elixir
  # TODO: Deprecate in v1.5.
  def some_function(arg), do: {:ok, arg}
  ```

* <a name="exceptions-aux-annotations"></a>
  Dans les cas où le problème est si évident qu'une documentation serait
  redondante, il est possible de laisser une annotation seule, sans note.
  Cette utilisation des annotations doit être une exception, pas une règle !
  <sup>[[lien](#exceptions-aux-annotations)]</sup>

  ```elixir
  start_task()

  # FIXME
  Process.sleep(5000)
  ```

* <a name="annotations-todo"></a>
  Utilisez `TODO` pour signaler du code auquel des fonctionnalités devront être
  ajoutées.
  <sup>[[lien](#annotations-todo)]</sup>

* <a name="annotations-fixme"></a>
  Utilisez `FIXME` pour signaler du code qui doit être corrigé.
  <sup>[[lien](#annotations-fixme)]</sup>

* <a name="annotations-optimize"></a>
  Utilisez `OPTIMIZE` pour signaler du code lent ou inefficace qui pourrait créer
  des problèmes de performance.
  <sup>[[lien](#annotations-optimize)]</sup>

* <a name="annotations-hack"></a>
  Utilisez `HACK` pour signaler du code issu de pratiques de programmation
  douteuses et qui devrait être refactorisé.
  <sup>[[lien](#annotations-hack)]</sup>

* <a name="annotations-review"></a>
  Utilisez `REVIEW` pour signaler des éléments qui doivent être vérifiés pour
  confirmer qu'ils fonctionnent comme souhaité.
  Par exemple : `REVIEW: Are we sure this is how the client does X currently?`
  <sup>[[lien](#annotations-review)]</sup>

* <a name="mots-cles-personnalises"></a>
  Utilisez d'autres mots-clés personnalisés si vous pensez que cela est utile.
  N'oubliez pas d'écrire une documentation sur ces mots-clés personnalisés dans
  le `README` (ou équivalent) de votre projet.
  <sup>[[lien](#mots-cles-personnalises)]</sup>

### Modules

* <a name="un-module-par-fichier"></a>
  Un seul module dans un seul fichier. Sauf si un module est utilisé uniquement
  à l'intérieur d'un autre module (un test, par exemple).
  <sup>[[lien](#un-module-par-fichier)]</sup>

* <a name="nom-de-fichier-snake-case"></a>
  Utilisez la convention [`snake_case`][Snake case] pour les noms de fichers
  et la convention [`CamelCase`][Camel case] pour les noms de modules.
  <sup>[[lien](#nom-de-fichier-snake-case)]</sup>

  ```elixir
  # dans un fichier appelé some_module.ex

  defmodule SomeModule do
  end
  ```

* <a name="nom-de-module-et-imbrication"></a>
  Representez chaque niveau d'imbrication dans un nom de module par des
  dossiers correspondants.
  <sup>[[lien](#nom-de-module-et-imbrication)]</sup>

  ```elixir
  # le code suivant se trouverait dans ce fichier: parser/core/xml_parser.ex

  defmodule Parser.Core.XMLParser do
  end
  ```

* <a name="ordre-des-attributs-des-modules"></a>
  Listez les attributs et directives des modules dans cet ordre :
  <sup>[[lien](#ordre-des-attributs-des-modules)]</sup>

  1. `@moduledoc`
  1. `@behaviour`
  1. `use`
  1. `import`
  1. `alias`
  1. `require`
  1. `defstruct`
  1. `@type`
  1. `@module_attribute`
  1. `@callback`
  1. `@macrocallback`
  1. `@optional_callbacks`

  Ajoutez une ligne vide entre chaque groupe, et trier les termes par ordre
  alphabétique (comme les noms de modules). Voici un exemple montrant
  globalement comment organiser vos modules :

  ```elixir
  defmodule MyModule do
    @moduledoc """
    An example module
    """

    @behaviour MyBehaviour

    use GenServer

    import Something
    import SomethingElse

    alias My.Long.Module.Name
    alias My.Other.Module.Example

    require Integer

    defstruct name: nil, params: []

    @type params :: [{binary, binary}]

    @module_attribute :foo
    @other_attribute 100

    @callback some_function(term) :: :ok | {:error, term}

    @macrocallback macro_name(term) :: Macro.t()

    @optional_callbacks macro_name: 1

    ...
  end
  ```

* <a name="module-pseudo-variable"></a>
  Utilisez la pseudo-variable `__MODULE__` lorsqu'un module doit référer à
  soi-même. Si ce module doit changer de nom, cela évite de devoir le mettre à
  jour à plusieurs endroits.
  <sup>[[lien](#module-pseudo-variable)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    defstruct [:name]

    def name(%__MODULE__{name: name}), do: name
  end
  ```

* <a name="alias-auto-reference-modules"></a>
  Si vous voulez un nom plus joli que `__MODULE__`, vous pouvez créer un alias.
  <sup>[[lien](#alias-auto-reference-modules)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    alias __MODULE__, as: SomeModule

    defstruct [:name]

    def name(%SomeModule{name: name}), do: name
  end
  ```

* <a name="noms-module-repetitifs"></a>
  Évitez les répétitions dans les noms de modules et _namespaces_.
  Cela améliore la lisibilité du code et permet d'éviter les
  [conflits d'alias][Conflicting Aliases] (anglais).
  <sup>[[lien](#noms-module-repetitifs)]</sup>

  ```elixir
  # déconseillé
  defmodule Todo.Todo do
    ...
  end

  # conseillé
  defmodule Todo.Item do
    ...
  end
  ```

### Documentation

La documentation en Elixir (lue dans `iex` avec `h`, ou générée avec [ExDoc])
utilise les [attributs de module] `@moduledoc` et `@doc`.

* <a name="moduledocs"></a>
  Dans vos modules, incluez toujours l'attribut `@moduledoc` sur la ligne juste
  après `defmodule`.
  <sup>[[lien](#moduledocs)]</sup>

  ```elixir
  # déconseillé

  defmodule AnotherModule do
    use SomeModule

    @moduledoc """
    About the module
    """
    ...
  end

  # conseillé

  defmodule AThirdModule do
    @moduledoc """
    About the module
    """

    use SomeModule
    ...
  end
  ```

* <a name="moduledoc-false"></a>
  Utilisez `@moduledoc false` si vous n'avez pas l'intention de documenter le
  module.
  <sup>[[lien](#moduledoc-false)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false
    ...
  end
  ```

* <a name="moduledoc-espacement"></a>
  Séparez `@moduledoc` du reste du code par une ligne vide.
  <sup>[[lien](#moduledoc-espacement)]</sup>

  ```elixir
  # déconseillé
  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    use AnotherModule
  end

  # conseillé
  defmodule SomeModule do
    @moduledoc """
    About the module
    """

    use AnotherModule
  end
  ```

* <a name="heredocs"></a>
  Utilisez heredocs avec markdown pour la documentation.
  <sup>[[lien](#heredocs)]</sup>

  ```elixir
  # déconseillé
  defmodule SomeModule do
    @moduledoc "About the module"
  end

  defmodule SomeModule do
    @moduledoc """
    About the module

    Examples:
    iex> SomeModule.some_function
    :result
    """
  end

  # conseillé
  defmodule SomeModule do
    @moduledoc """
    About the module

    ## Examples

        iex> SomeModule.some_function
        :result
    """
  end
  ```

### Typespecs

Les typespecs sont des notations pour déclarer des types et spécifications, ces
dernières sont utilisées pour documenter ou pour l'outil d'analyse statique
Dialyzer.

Les types personnalisés doivent être définis en haut des modules avec les autres
directives (voir le [chapitre sur les modules](#modules)).

* <a name="typedocs"></a>
  Placez les définitions de `@typedoc` and `@type` ensembles, en séparant chaque
  paire d'une ligne vide.
  <sup>[[lien](#typedocs)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false

    @typedoc "The name"
    @type name :: atom

    @typedoc "The result"
    @type result :: {:ok, term} | {:error, term}

    ...
  end
  ```

* <a name="union-types"></a>
  Si une union de type est trop longue pour tenir sur une seule ligne, placez
  chaque morceau du type sur une ligne séparée, en indentant d'un niveau après
  le nom du type.
  <sup>[[lien](#union-types)]</sup>

  ```elixir
  # déconseillé
  @type long_union_type ::
          some_type | another_type | some_other_type | one_more_type | a_final_type

  # conseillé
  @type long_union_type ::
          some_type
          | another_type
          | some_other_type
          | one_more_type
          | a_final_type
  ```

* <a name="nommage-des-types-principaux"></a>
  Nommer le type principal d'un module `t`. Voici par exemple la spécification
  de type pour une _struct_:
  <sup>[[lien](#nommage-des-types-principaux)]</sup>

  ```elixir
  defstruct name: nil, params: []

  @type t :: %__MODULE__{
          name: String.t() | nil,
          params: Keyword.t()
        }
  ```

* <a name="spec-spacing"></a>
  Placez les spécifications juste avant la définition de fonction, sans les
  séparer d'une ligne vide.
  <sup>[[lien](#spec-spacing)]</sup>

  ```elixir
  @spec some_function(term) :: result
  def some_function(some_data) do
    {:ok, some_data}
  end
  ```

### Structs

* <a name="struct-defaut-nil"></a>
  Utilisez une liste d'atomes pour les _struct_ qui ont des champs ayant `nil`
  comme valeur par défaut.
  <sup>[[lien](#struct-defaut-nil)]</sup>

  ```elixir
  # déconseillé
  defstruct name: nil, params: nil, active: true

  # conseillé
  defstruct [:name, :params, active: true]
  ```

* <a name="struct-def-crochets"></a>
  Ne mettez pas de crochets lorsque l'argument d'une `defstruct` est une liste
  de mots-clés.
  <sup>[[lien](#struct-def-crochets)]</sup>

  ```elixir
  # déconseillé
  defstruct [params: [], active: true]

  # conseillé
  defstruct params: [], active: true

  # obligatoire - les crochets ne sont pas optionnels quand
  # il y a au moins un atome dans la liste
  defstruct [:name, params: [], active: true]
  ```

* <a name="struct-sur-plusieurs-lignes"></a>
  Si la déclaration d'une _struct_ prends plusieurs lignes, placez chaque
  élément sur une ligne différente et indentez-les de sorte à ce qu'ils soient
  alignés.
  <sup>[[lien](#struct-sur-plusieurs-lignes)]</sup>

  ```elixir
  defstruct foo: "test",
            bar: true,
            baz: false,
            qux: false,
            quux: 1
  ```

  Si une _struct_ de plusieurs lignes nécessite des crochets, formattez-la comme
  une liste de plusieurs lignes.

  ```elixir
  defstruct [
    :name,
    params: [],
    active: true
  ]
  ```

### Exceptions

* <a name="nom-exception"></a>
  Terminez vos noms d'exceptions par `Error`.
  <sup>[[lien](#nom-exception)]</sup>

  ```elixir
  # déconseillé
  defmodule BadHTTPCode do
    defexception [:message]
  end

  defmodule BadHTTPCodeException do
    defexception [:message]
  end

  # conseillé
  defmodule BadHTTPCodeError do
    defexception [:message]
  end
  ```

* <a name="messages-d-erreur-en-minuscules"></a>
  Utilisez des messages d'erreurs composés uniquement de minuscules.
  <sup>[[lien](#messages-d-erreur-en-minuscules)]</sup>

  ```elixir
  # déconseillé
  raise ArgumentError, "This is not valid."

  # conseillé
  raise ArgumentError, "this is not valid"
  ```

### Collections

_Il n'y a pas encore de guide pour les collections._

### Chaînes de caractères

* <a name="strings-matching-with-concatenator"></a>
  Comparez des chaînes de caractères avec l'opérateur de concaténation plutôt
  que les modèles binaires.
  <sup>[[lien](#strings-matching-with-concatenator)]</sup>

  ```elixir
  # déconseillé
  <<"my"::utf8, _rest::bytes>> = "my string"

  # conseillé
  "my" <> _rest = "my string"
  ```

### Expressions régulières

_Il n'y a pas encore de guide pour les expressions régulières._

### Méta-programmation

* <a name="meta-programmation-inutile"></a>
  Évitez la méta-programmation inutile.
  <sup>[[lien](#meta-programmation-inutile)]</sup>

### Les tests

* <a name="tests-ordre-assert"></a>
  Lorsque vous écrivez des assertions [ExUnit], soyez consistant dans l'ordre
  entre la valeur attendue et la valeur testée.
  Préférez placer le résultat attendu sur la droite, sauf si l'assertion est un
  _pattern match_.
  <sup>[[lien](#tests-ordre-assert)]</sup>

  ```elixir
  # conseillé - résultat attendu sur la droite
  assert actual_function(1) == true
  assert actual_function(2) == false

  # déconseillé - ordre inconsistant
  assert actual_function(1) == true
  assert false == actual_function(2)

  # obligatoire - si l'assertion est un pattern match
  assert {:ok, expected} = actual_function(3)
  ```

## Ressources

### Alternatives à ce guide

* [Aleksei Magusev's Elixir Style Guide](https://github.com/lexmag/elixir-style-guide#readme)
  (en anglais) — Un guide de style Elixir bien ancré, issu du style de codage
  utilisé dans les bibliothèques de base d'Elixir.
  Développé par [Aleksei Magusev](https://github.com/lexmag) et
  [Andrea Leopardi](https://github.com/whatyouhide), membres de l'équipe Elixir.
  Bien que le projet Elixir ne respecte aucun guide de style spécifique,
  il s'agit du guide le plus proche de ses conventions.

* [Credo's Elixir Style Guide](https://github.com/rrrene/elixir-style-guide#readme)
  (en anglais) — Guide de style Elixir, réalisé par [Credo](http://credo-ci.org),
  outil d'analyse statique de code.

### Outils

Référez-vous au dépôt [Awesome Elixir][Code Analysis] pour des bibliothèques
et outils qui vous aideront à contrôler la qualité de votre code et de votre
style de codage.

## Nous aider

### Contribuer

Nous avons l'espoir que ce dépôt devienne un point de rencontre pour discuter
des meilleurs pratiques de programmation Elixir.
N'hésitez pas à ouvrir des tickets ou à ouvrir des *pull requests* pour améliorer
ce guide.
Merci d'avance pour votre aide !

Lisez les [indications sur comment contribuer][Contribute] (en anglais) pour
plus d'informations.

### Passez le mot

Un guide de style communautaire n'a pas d'intérêt sans l'aide de la communauté.
N'hésitez pas à laisser une [star][Stargazers] à ce dépôt pour le faire
connaître. Et partagez [ce guide][Elixir Style Guide] avec tous les
programmeurs Elixir que vous connaissez, pour qu'ils puissent y contribuer.

## Reproduction

### Licence

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
Ce guide est protégé par la licence
[Creative Commons Attribution 3.0 Unported License][Licence]

### Attribution

La structure de ce guide, des bribes de code d'exemple, ainsi que de nombreux
points initiaux de ce guide ont été empreintés au [Guide de style Ruby].
Beaucoup de choses étaient aussi applicables à Elixir et nous on permis de
sortir ce guide au plus vite.

Voici la [liste des personnes ayant généreusement contribué][Contributors] à ce
guide.

<!-- Links -->
[Anglais]: https://github.com/christopheradams/elixir_style_guide
[Camel case]: https://fr.wikipedia.org/wiki/Camel_case
[Chinois simplifié]: https://github.com/geekerzp/elixir_style_guide/blob/master/README-zhCN.md
[Chinois traditionnel]: https://github.com/elixirtw/elixir_style_guide/blob/master/README_zhTW.md
[Code Analysis]: https://github.com/h4cc/awesome-elixir#code-analysis
[Code Of Conduct]: https://github.com/elixir-lang/elixir/blob/master/CODE_OF_CONDUCT.md
[Code Formatter]: https://hexdocs.pm/elixir/Code.html#format_string!/2
[Conflicting Aliases]: https://elixirforum.com/t/using-aliases-for-fubar-fubar-named-module/1723
[Coréen]: https://github.com/marocchino/elixir_style_guide/blob/new-korean/README-koKR.md
[Contribute]: https://github.com/christopheradams/elixir_style_guide/blob/master/CONTRIBUTING.md
[Contributors]: https://github.com/christopheradams/elixir_style_guide/graphs/contributors
[Elixir Style Guide]: https://github.com/christopheradams/elixir_style_guide
[Elixir]: http://elixir-lang.org
[Espagnol]: https://github.com/albertoalmagro/elixir_style_guide/blob/spanish/README_esES.md
[ExDoc]: https://github.com/elixir-lang/ex_doc
[ExUnit]: https://hexdocs.pm/ex_unit/ExUnit.html
[Français]: https://github.com/ronanboiteau/elixir_style_guide/blob/master/README_frFR.md
[Guard Expressions]: http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses
[Hex]: https://hex.pm/packages
[Japonais]: https://github.com/kenichirow/elixir_style_guide/blob/master/README-jaJP.md
[Licence]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Mix format]: https://hexdocs.pm/mix/Mix.Tasks.Format.html
[attributs de module]: http://elixir-lang.org/getting-started/module-attributes.html#as-annotations
[Portugais]: https://github.com/gusaiani/elixir_style_guide/blob/master/README_ptBR.md
[Guide de style Ruby]: https://github.com/gauthier-delacroix/ruby-style-guide/blob/master/README-frFR.md
[Sentence Spacing]: http://en.wikipedia.org/wiki/Sentence_spacing
[Snake case]: https://fr.wikipedia.org/wiki/Snake_case
[Stargazers]: https://github.com/ronanboiteau/elixir_style_guide/stargazers
