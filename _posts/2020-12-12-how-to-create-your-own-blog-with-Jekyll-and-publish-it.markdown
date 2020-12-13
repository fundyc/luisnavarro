---
layout: post
title: Cómo crear tu propio blog con Jekyll y publicarlo
date: 2020-12-12 16:32:20 +0100
description: Crea tu marca perosnal online con un blog hecho en Jekyll publicado en tu propio hosting # Add post description (optional)
img: dr-jekyll.jpg # Add image post (optional)
fig-caption: It's alive!!! # Add figcaption (optional)
tags: [jekyll, ruby, hosting]
---
Aprovechando el Black Friday de 2020, en medio de una pandemia, me dio un flush y me compré un dominio (luisnavarro.eu) para animarme a montar mi propio blog con Jekyll usando Github Pages (un proyecto que tenía pendiente desde hace años). Así me obligo a "documentar" las motivaciones y problemas que me voy encontrando en mis proyectos personales.


## Stage 1: Comprar el domino

![Shut up and take my money]({{site.image_url}}/20201212/take_my_money.png)

Si ya tienes el nombre pensado es muy fácil, yo me decanté por el dominio .eu porque estaban baratitos y el .es y .com ya estaban pillados, cosas de tener un nombre común.
Hay infinidad de sitios donde cogerlo, al final tiré de [Nominalia](http://nominalia.com/) por ser una empresa que ya conocía y por la oferta que tenían. 
Al solo necesitar el dominio, cualquier empresa de hosting nos hace el apaño.

## Stage 2: Obtener un certificado SSL

![Protege internete]({{site.image_url}}/20201212/gorilla.jpg)

En los tiempos que corren un certificado SSL es indispensable para publicar una web en internete así que aquí se presentan varias opciones:

* Subscribirte a la seguridad que te ofrezca tu empresa de hosting.
* Pagar por un certificado de un año.
* Usar un certificado gratuito y renovable automáticamente cada 90 días como el de [Let's encrypt](https://letsencrypt.org/)

Obviamente me decanté por la tercera opción. Como sólo tengo el dominio sin máquinas usé [ZeroSSL](https://zerossl.com/) para generar el certificado y poder instalarlo sin problemas desde el panel de control del hosting (copy&paste del CRT y la key del certificado y a correr).


## Stage 3: Elegir un template para el blog


![I choose you]({{site.image_url}}/20201212/choose_you.png)

Entra en jekyllthemes.io y elige el tema que más se adapte a tu estilo y luego tunealo a tu gusto.
Si tienes conocimientos frontend de html, css y javascript o has trasteado alguna vez con velocity puedes hacer tu propia plantilla sin mucho esfuerzo :) 
Si te has decantado por elegir una plantilla suele ser tan fácil como forkearte el repo de la plantilla a tu github y listo.

## Stage 4: Configurar Github Pages

![configuring github pages]({{site.image_url}}/20201212/jekyllrb.png)

Ya tienes tu blog listo en local y es el momento de publicarlo en github pages. Para ello crea un nuevo repo en tu cuenta de github (si no tienes crea una nueva) y activa en la configuración que quieres usar github pages.

Al activarlo te creará una rama gh-pages que será la que va a usar github para publicar tu blog.
Sube tu proyecto a esta rama, si no quieres liarte márcala como default.
Y ya podrás ver tu blog publicado en internet en https://<tu_cuenta_de_github>.github.io/

## Final Boss: Redireccionar de tu dominio a github pages
![¡está vivo!]({{site.image_url}}/20201212/frankenstein.jpg)

Ya está casi todo listo... sólo un último paso.
Entra en la configuración de github y rellena el custom domain y marca el uso forzado de https 

![configuring github pages]({{site.image_url}}/20201212/github_pages_conf.png)

Por último el golpe de gracia, ve a la consola de gestión de tu hosting y cambiar las DNS para que apunte a tu página de github.
Haz un ping a github.io y pon esa ip en la entrada de tipo A de los DNS.

Esperamos a que se refresquen las DNS y...
![YOU WIN!]({{site.image_url}}/20201212/luisnavarroeu.png)