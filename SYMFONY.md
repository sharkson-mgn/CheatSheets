# Symfony - CheatSheet

`symfony new {project_name}` or<br />
`composer create-project symfony/skeleton {project_name}` - create new project in directory `{project_name}`

`symfony check:req` - check requrements of system

`symfony serve -d` - run Symfony server in background<br />
`symfony server:start` - run Symfony server in current cmd

`symfony console` - print list of all parameters of console<br />
`symfony console debug:autowire [find]` - Show services<br />
`php bin/console debug:router` - print all routes with details<br />
`php bin/console cache:pool:list` - get all pool names of cache<br />
`php bin/console cache:pool:clear [cache.name]` - clear cache of name<br />
`symfony console debug:config [name]` - return current config of name<br />
`symfony console config:dump [name` - dump example config of name<br />
`symfony var:export --multiline` - get all env variables

## Example of Symfony Controller
```
<?php 

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class SomeController extends AbstractController {

  #[Route('/browse/{slug}', name: 'app_browse', methods: ['GET'])]
  public function homepage(string $slug = null): Response
  {
    return $this->render('dir_temp/template.html.twig', [
      'somevar' => $variableone,
      'othervar' => $variabletwo,
    ]);
  }

}
?>
```
Other examples of routes:<br />
`#[Route('/api/songs/{id<\d+>}', methods: ['GET'], name: 'api_songs_get_one')]`<br />
then `public function getSong(int $id, LoggerInterface $logger): Response`

## Methods of response
Remember: every controller must to be return Response!<br />
`return new Response('text');`<br />
`return $this->render('dir_temp/template.html.twig');`<br />
`return $this->json($song);`<br />
`return new JsonResponse($var)` is equal to return above

## Recipies
To check recipes: `composer recipes [find]`<br />
AE: `composer recipes` to list or `composer recipes symfony/twig-bundle` to get recipe details

`composer require templates` - require all things what you need to templates<br />
`composer require debug` - (...) to debug<br />
`composer require symfony/ux-turbo` - wooohooo dynamic load pages


Other recipies You can find on:<br />
https://github.com/symfony/recipes<br />
https://github.com/symfony/recipes-contrib

## Services


## Twig
`{{ some_var }}` or `{{ someFunction() }}`<br />
`{# comment #}`
```
{% for [index,]elem in elements %}
 <li>{{ elem }}</li>
{% endfor %}
```
`{{ somevar|modifier}}` ae `{{ somevar|upper}}`
### In eachother templatefile
```
{% extends 'base.html.twig' %}
{% block body %}
  stuff
{% endblock %}
```
### In main file
```
{% block blockname %}
  somestaff default
{% endblock %}
```
```
{% block stylesheets %}
   {{ encore_entry_link_tags('app') }}
{% endblock %}
```
```
{% block javascripts %}
   {{ encore_entry_script_tags('app') }}
{% endblock %}
```

`{{ asset('styles/app.css') }}` (need `composer require symfony asset`)<br />
`{{ path('app_routename') }}` - insert routename to template

## DEBUG
`dd($var)`<br />
`dump($var)`<br />
`LoggerInterface $logger`
```
$logger->info('Returning API response for song {song}', [
  'song' => $id,
]);
```

## Webpack
`composer require encore`<br />
`yarn install`<br />
`yarn watch`<br />
### elements...
`yarn add bootstrap --dev` then `@import '~bootstrap';`<br />
`yarn add @fontsource/font-name --dev` then `@import '~@fontsource/fontname';`<br />
`yarn add @fortawesome/fontawesome-free --dev` then `@import '~@fortawesome/fontawesome-free/css/all.css';`<br />
...in `assets/styles/app.css`


## Stimulus
`<div data-controller="hello"></div>`
```
<div class="song-list" {{ stimulus_controller('song-controls', {
	infoUrl: path('api_songs_get_one', { id: loop.index })
}) }}>
<a href="#" {{ stimulus_action('song-controls', 'play') }}>
	<i class="fas fa-play me-3"></i>
</a>
```
```
import { Controller } from '@hotwired/stimulus';

import axios from 'axios';

export default class extends Controller {
    static values = {
        infoUrl: String
    }

    play(event) {
        event.preventDefault();

        axios.get(this.infoUrlValue)
            .then((response) => {
                const audio = new Audio(response.data.url);
                audio.play();
            });
    }
}
```

## Doctrine
`composer require doctrine`<br />
`php bin/console make:docker:database`<br />
`symfony console doctrine:database:create` - to create database<br />
`symfony console d:d:c` - like above<br />
`symfony console make:entity` - to create entity<br />
`symfony console make:migration` - to make migration<br />
`symfony console doctrine:schema:update --dump-sql` - similar to command above<br />
`symfony console doctrine:migrations:migrate` - to do migration<br />
`symfony console doctrine:query:sql "SELECT * FROM vinyl_mix"` - to make sql query<br />
`symfony console doctrine:migrations:status` - to get migrations status<br />
`symfony console doctrine:migrations:list` - to get migrations list<br />
