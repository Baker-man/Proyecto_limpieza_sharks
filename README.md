# Proyecto_limpieza_sharks

#Objetivos

#1) Crear un repo nuevo con files src, img, data, Readme.md y gitignore.

#2) Issue con el link pegado de nuestro Repo

#3) Limpieza:
    #a) Borrar columnas si y sólo si len(NaN) > 80%
    #b) Al menos 6000 filas
    #c) Mismo tipo de datos, duplicados, etc...

#4) Presentación

BONUS: Objetivo (insight)

Ejemplo: ver ataques por países en un dataframe





#PASOS SEGUIDOS

1) Cargar el dataframe
2) Explorar el dataframe
3) Mirar valores nulos
    a) Si len(NaN) > 80% borramos columnas
    b) El resto de NaN exploramos valores y significado de columnas y valoramos por qué lo sustituimos (0, unknown...)
4) Mirar datos inconsistentes
    cambiar tipo de dato (optimizar memoria)
5) Eliminar duplicados
6) Columnas constantes o de baja varianza
7) Outliers
8) Colinealidad
9) Checkear que no hay duplicados de nuevo