---
layout: post
image: /images/articles/city-pixel/header.png
title: Ciudades en píxeles de tráfico
resume: Me encontré con un conjunto de datos que describe el flujo de tráfico dentro de 3 ciudades europeas, a partir de estos datos y con la ayuda de python generé imágenes comṕuestas de pixeles para representar un instante de tiempo en una ciudad.
---

El nombre de este post me sigue sonando extráño pero resúme bastante bien su contenido, desde hace un tiempo estoy trabajando con un conjunto de datos que describe el flujo de tráfico de 3 grandes ciudades europeas: 

* [Berlín - Alemánia](https://es.wikipedia.org/wiki/Berl%C3%ADn)
* [Estambul - Turquía](https://es.wikipedia.org/wiki/Estambul)
* [Moscú - Rusia](https://es.wikipedia.org/wiki/Mosc%C3%BA)

Los archivos fuente están en formato [HDF5](https://support.hdfgroup.org/HDF5/whatishdf5.html) el cual permite almacenar grandes volúmnes de datos en arreglos multidimencionales. Cada archivo contiene 24 horas de información sobre el flujo de tráfico en una ciudad, representada en un arreglo de 4 dimensiones con valores entre 0 y 255.
La verdad es que NO es muy cómodo validar la información de ésta manéra, así que dedíque algunas horas del fín de semana para generar mapas a partir de esos datos y el resultado es un poco de código python, este post y este lindo gif!

<center><img src="/images/articles/city-pixel/animacion_estambul_ch1.gif" alt="Estambul" width="300" align="center"/></center>

## ¿Que contienen los archivos HDF5 exactamente? 
como dije antes estos archivos contienen arreglos multidimensionales, los cuales tienen la forma \{288, 495, 436, 3\}, esto básicamente significa:

* **288 bloques:** Cada bloque representa 5 minútos de información generada, 288 bloques de 5 minutos son 24 horas de flujo de tráfico.
* **495 píxeles de alto:** Las ciudades fueron segmentadas en píxeles, cada píxel representa un espacio de apróximadamente 100x100 mts, cada píxel resúme la actividad del tráfico en ese lugar del mapa en un bloque de tiempo. Cada ciudad contiene 495 píxeles de alto.
* **436 píxeles de ancho:** Cada ciudad contiene 436 píxeles de ancho.

<center><img src="/images/articles/city-pixel/berlin_tessellation.png" alt="Estambul" width="600" align="center"/></center>

* **3 Canales:** La información del tráfico está dividida en trés canáles diferentes: *volúmen, velocidad y dirección*.

Entonces cuando nos referimos a un frame, estamos hablando de un bloque de 5 minutos que contiene 3 mapas (uno por cada canal), donde cada celda de la matriz de 495x436 contiene un valor escalado entre 0 y 255 que representa el estado del tráfico en un instante de tiempo en esa ubicación.

Puedes encontrar información mucho más técnica y detallada acerca del conjunto de datos en [*The Traffic4casting challenge 2019*](https://www.iarai.ac.at/traffic4cast/).

## y ¿cómo se hace una imagen con todo eso?

Pues es justo ahí donde aparece Python y algunas librerías muy útilies.

```python
import h5py
import numpy
import matplotlib.pyplot as plt
```
*h5py* nos va a permitir manipuar los archivos HDF5 de extensión .h5, con *numpy* vamos a transformat los arreglos mltidemncionales en matrices sencillas de nxn y  *matplotlib* nos va a permitor generar los gráficos. Ahora tenemos todo para iniciar, lo primero va a ser cargar el archivo.

```python
inpath = '/path/of/file/filename.h5'
f = h5py.File(inpath, 'r')

# Estructura propia de los archivos HDF5
keys = list(f.keys())
a_group_key = list(f.keys())[0]
data = list(f[a_group_key])
data = [data[0:]]

data = numpy.stack(data,axis=0)
```
Esta parte del código va a permitir acceder directamente a los datos que es lo que nos interesa. la función *numpy.stack* va a apilar las matrices de datos que va a extrayendo creando arreglos de numpy. Este tipo de archivos permiten almacenar grandes volúmenes de datos por lo que en ocaciones pueden tener varias 'keys', en este caso solo tenemos una por lo que acceder a los datos es bastante sencillo (solo copié y pegué este código de la documentación oficial).

```python
framearray = numpy.zeros([495, 436], dtype = int) 
nuchannel = 0 

for blockidx in range(0, 288):
    for rowidx in range(0, 494):
        for colidx in range(0, 435):
            framearray[rowidx, colidx] = data[0, blockidx, rowidx, colidx, nuchannel]
```
Con esto vamos a armar un arreglo de 3 dimensiones ya que elegimos un solo canal, la idea inicial es graficar cada canal independientente, además va a estar en formato numpy lo que lo hace más fácil de manejar. Dado que tenemos 3 canales y cada canal usa valores valores enre 0 y 255, me pareció buena idea usar un sistema de colores R,G,B. para esme armé un función para crear una paleta de colores personalizada.

```python
from matplotlib.colors import ListedColormap, LinearSegmentedColormap

def createcolormap(nuchannel):
 
     ltcolors = []
     for colhex in range(0,253):
 
         hexstr = hex(colhex).replace('0x','')
 
         if len(hexstr) == 1:
             hexstr = '0%s'%hexstr
 
         if nuchannel == 0:
             rgbstr = '#%s0000'%hexstr # red
         elif nuchannel == 1:
               rgbstr = '#00%s00'%hexstr # green
         else:
             rgbstr = '#0000%s'%hexstr # blue
 
         ltcolors.append(rgbstr)
 
     cmap = ListedColormap(ltcolors)
     return cmap

```

Esta función va a usar el número del canal para generar los valores RGB que van a componer el mapa de colores de cada imagen.

```python
def save_plotframe(frame2plot,custcmap,nublock,nuchannel):

    fig = plt.figure(figsize=(495,436))
    plt.imshow(frame2plot, cmap=custcmap)
    plt.axis('off')
    plt.title('block %s'%nublock)

    plt.show()
    fig.savefig('frames/ch%s/%s.png'%(nuchannel,nublock))
    plt.close(fig)
```

La función save\_plotframe va generar las 288 imágenes que componen un día usando maplotlib y las va a guardar en la direccion que más nos guste. finalmente vamos a usar dos funciones que nos van a permitir generar el gif animado.

<center>
<img src="/images/articles/city-pixel/ch0.png" alt="Canal 0" width="200" />
<img src="/images/articles/city-pixel/ch1.png" alt="Canal 1" width="200" />
<img src="/images/articles/city-pixel/ch2.png" alt="Canal 2" width="200" />
</center>


```python
from PIL import Image
import imageio

def cropimagesfromdir(inpath,outpath):
 
     f = []
 
     for (dirpath, dirnames, filenames) in walk(inpath):
         for dirname in dirnames:
             path.join(dirpath,dirname)
         for filename in filenames:
             file_path = "%s/%s"%(dirpath,filename)
 
             im = Image.open(file_path)
     
             # im.crop((left, top, right, bottom))
             region = im.crop((620, 80, 1290, 880))
             region.save("%s/%s"%(outpath,filename))
 
 
 def creategif(outpath):
     images = []
 
     for idx in range(0,288):
         images.append(imageio.imread('%s/%s.png'%(outpath,idx)))
 
     imageio.mimsave('%s/animation.gif'%outpath, images)
```

*cropimagesfromdir* va a recortar las imágenes que en mi caso tenian algunos bordes adicionales que no necesitaba. 
*creategif* va a generar el gif animado usando la librería *imageio*.


<center>
<img src="/images/articles/city-pixel/ch0_animation.gif" alt="Canal 0" width="200" />
<img src="/images/articles/city-pixel/ch1_animation.gif" alt="Canal 1" width="200" />
<img src="/images/articles/city-pixel/ch2_animation.gif" alt="Canal 2" width="200" />
</center>

El resultado final me deja bastante satisfecho, para mi es una herramienta muy útil ya que me permíte ver el comportamiento de los datos dentro dentro de un archivo y entender los momentos de mayor flujo de vehículos y enfocar mis esfuerzos en analizar esa parte del día.

Si te interesa conocer más sobre como funciona y la implementación completa de la solución podés ver mi repositorio de github [ploth5maps - Plotting pixel maps from HDF5 files](https://github.com/avelezd/plot_h5maps).

Espero que les resulte tán útil como a mi.

