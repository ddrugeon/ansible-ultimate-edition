# Variables

```{admonition} Perle de sagesse
:class: tip

Les variables de rôle, de `defaults/main.yml` et `vars/main.yml` n'ont que leur propre rôle comme portée de variable et partagent
le même espace de variable.
```

## Defaults ou Vars ?

Une variable de pilotage appartenant à un rôle doit-elle trouver sa place :

* dans `defaults/main.yml` ?
* dans `vars/main.yml` ?

La réponse à cette question est assez simple, pour peu qu'on modélise correctement le problème. L'un des principes de la 
programmation objet est l'encapsulation. Si vous n'êtes pas familier avec le concept, il s'agit de masquer le fonctionnement interne 
d'un objet. Les échanges entre un objet et un autre se font au travers d'une interface clairement définie.

Si l'on considère notre objet "rôle" sous cet angle, alors toutes les variables définies dans `defaults/main.yml` sont notre interface 
avec l'extérieur. C'est au travers de ces variables qu'un utilisateur va pouvoir influer sur le comportement de notre rôle.

Le fichier `vars/main.yml` est à réserver à la construction de variables internes, potentiellement des compositions des variables 
issues de `defaults/main.yml`. Un bon exemple pourrait être :

```yaml
#
# defaults/main.yml
#
---
nginx_server_name: "alpha"
```

----

```yaml
#
# vars/main.yml 
#
---
_nginx_site_config_path: "/etc/nginx/sites-availables/{{ nginx_server_name }}.conf"
_nginx_site_link_path: "/etc/nginx/sites-enabled/{{ nginx_server_name }}.conf"
```

On offre à l'utilisateur la possibilité de redéfinir le nom du serveur que nous allons configurer, mais les chemins des 
fichiers de configuration sont soumis aux conventions de nginx et par conséquent on forge des variables dans `vars/main.yml`
pour respecter cela.

## Convention de nommage

* Toute variable de rôle de `defaults/main.yml` doit être préfixée par le nom du rôle.
* Toute variable de rôle de `vars/main.yml` doit être préfixée par '_' suivi par le nom du rôle.
