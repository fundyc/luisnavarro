---
layout: post
title: LuigiVision - videollamadas con webRTC
date: 2021-02-07 16:32:20 +0100
description: Como hacer una aplicacion de videollamada con webRTC # Add post description (optional)
img: 20210207/luigivision.jpg # Add image post (optional)
fig-caption: call me # Add figcaption (optional)
tags: [webrtc, javascript, frontend]
---
Tras haber superado el grandísimo reto de sobrevivir al 2020, empieza un nuevo año con el mismo objetivo: llegar al 2022. Por el camino intentaré seguir posteando "aventuras" nuevas. Vamos al lío:

A finales de 2020 pensé que con la que estaba cayendo y previendo que pasar tiempo con la familia iba a estar complicado, podría ser buena idea montar una alternativa a las videollamadas de Whatshapp (que es lo que usábamos en ese momento) por si se saturaba y petaba (cosas más raras hemos visto en el 2020 como que [google se caiga](https://www.xataka.com/servicios/caida-servicios-google-gmail-youtube-otros-no-cargan-movil-pc)).

Así que me puse a mirar cómo estaba el mercado, había muchas opciones comerciales que descarté porque lo que me interesaba era aprender cómo funcionan las videollamadas. Llegué a la conclusión que lo que lo está petando es el webRTC.

La idea que propone es bella en sí misma: transmisión de datos punto a punto sin intermediarios. Aunque la dura realidad me golpeó bien fuerte y os voy a relatar el viaje de un "fracaso".


## Fase del enamoramiento

![Doomed and sexy]({{site.image_url}}/20210207/first_date.jpg)

A poco que busques en internet salen tutoriales y blogs de fulanos contando lo fácil que es y un diagrama precioso que estoy cansado de ver de cómo funciona la "arquitectura" y que la parte del servidor turn es opcional y que con el stun es suficiente para la mayoria de los casos (Lo de local, same network e internet es de mi cosecha).
![webrtc architecture]({{site.image_url}}/20210207/webrtc_architecture.png)
Así que dices, algo que han hecho en google tiene que ser bueno... 

## Fase del inicio de la relación

![First kiss]({{site.image_url}}/20210207/first_kiss.jpg)

Pillo [este tutorial](https://codelabs.developers.google.com/codelabs/webrtc-web) y enseguida empiezo a ver progresos (la magia del front):
* Crear streams de video y audio
* Conectar los streams a fuentes de datos
* Crear una conexión RTCPeerConnection
* Usar RTCDataChannels
* Conectar 2 clientes mediante signaling con sus candidates, offers y answers.
* Montar distintas salas

Esto va viento en popa. Ya tengo el primer esbozo de lo que puede ser una aplicación de videoconferencia funcionando como un tiro en mi local.


##  Fase de la decepción


![Jar jar binks]({{site.image_url}}/20210207/jar_jar.jpg)

Como buen fan de los mvps y como en mi local funciona, parece que es una buena idea publicar la app en internet para hacer pruebas con móviles a ver cómo está quedando.

Monto una imagen docker y la chuto a heroku. Primeras pruebas en casa... y oh la la! funciona :D
Pequeñas cosas que retocar como ajustar los estilos para que se vea bien en un móvil, mejorar la claridad del video o hacer un mirror para que cuando te estás viendo no parecezcas un monguer.

Corrijo estos detallitos y me dispongo a hacer el primer friends & family...

**FRACASO ABSOLUTO**

La pantalla de la persona que quieres ver se ve totalmente en negro. 

![where are you?]({{site.image_url}}/20210207/travolta.gif)

## Fase de la superación de la crisis

![Crisis]({{site.image_url}}/20210207/crisis.jpg)

Saco la lupa de Sherlock Holmes y me pongo a ver los logs.
Todo tiene buena pinta, se recupera correctamente la ip pública, pero al final la conexión no se consigue establecer... a lo mejor hay que abrir el puerto UDP pero no es algo que le pueda decir a mis padres.

Parece que los casos "especiales" donde hace falta usar un servidor TURN es cuando quieres que funcione en internet (vamos que los que han llegado a esa conclusión no han pasado de la poc en su casa).

Me pongo a buscar servidores TURN públicos (en el caso de los STUN los hay a patadas) y no hay ni uno... y su razón tiene. Para empezar porque va a hacer de intermediario para enviar el video de un punto a otro y nadie te asegura lo que va a hacer con él y segundo lo caro que es recibir y enviar video (imaginaos que lo usas en una aplicación que tiene cierto éxito... la factura que te va a llegar por dejar un servidor TURN público va a ser de órdago).

Llegado a este punto doy por concluido el viaje.
Y como siempre os dejo [aquí el codigo de Luigivisión](https://github.com/fundyc/webrtc-server) para que podáis cacharrear y usarla en vuestras casas.

## Fase de la conexión y plan de futuro
![New generation]({{site.image_url}}/20210207/star_trek.jpeg)

En este caso la conexión ha sido con la dura realidad :D

Podría continuar con el viaje y montar [mi propio servidor turn](https://github.com/coturn/coturn) pero  
el tiempo que le puedo dedicar a mis sideprojects o sidequests es limitado. 2020 ha pasado y las aplicaciones de videollamada han aguantado el tirón y no hemos tenido problemas en la familia para estar conectados.

El viaje nunca es en balde porque ahora sé cómo funciona una aplicación de videollamadas y llegado el momento de hacer una en plan serio conozco los pains con los que me podría encontrar. 

Llegando a las siguientes conclusiones:

* Lo bueno de usar webRTC es que la curva de aprendizaje es rápida, gracias a que está implementado en todos los navegadores tienes tu tag video al que le puedes enchufar un stream y en seguida estás viendo resultados.
Hay librerias que ayudan bastante a manejar las sesiones y establecer conexiones y hay bastante documentación sobre el tema.

* Lo malo es que vas a necesitar de una infraestructura un poco compleja para echar a andar con su sobrecoste asociado y cuantas más piezas metes en la ecuación el soporte que vas a necesitar para mantenerlo también es mayor.

Tengo en la cabeza más movidas que me gustaría investigar así que estad atentos al siguiente post.

![See you]({{site.image_url}}/20210207/see_you.gif)
