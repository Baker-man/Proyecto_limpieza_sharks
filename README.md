# Proyecto_limpieza_sharks

🎯OBJETIVOS

1) Crear un repo nuevo con files src, img, data, Readme.md y gitignore.

2) Issue con el link pegado de nuestro Repo

3) Limpieza:

    a) Borrar columnas si y sólo si len(NaN) > 80%
    
    b) Al menos 6000 filas
    
    c) Mismo tipo de datos, duplicados, etc...

4) Presentación

BONUS: Objetivo (insight)

Ejemplo: ver ataques por países en un dataframe


--------------------------------------------------------------------


📋PASOS SEGUIDOS

1) Cargar el dataframe
2) Explorar el dataframe
3) Mirar valores nulos

    a) Si len(NaN) > 80% borramos columnas

      - Definimos función para ver valores nulos y vemos que no todas las columnas con len(NaN) > 80% nos interesan ser borradas, porque tenemos un gran número de registros/filas en el que todas o casi todas las columnas tienen valores nulos. Nos vale más borrar esas filas y a partir de ahí trabajar con las columnas que nos interesan. Sí que vamos a borrar las dos últimas columnas, pues no indican su significado con su nombre "unnamed" y cerca del 100% de los registros son nulos para esas dos columnas.
        
        
    b) El resto de NaN exploramos valores y significado de columnas y valoramos por qué lo sustituimos (0, unknown...)
        - 
4) Cambiar tipo de dato (optimizar memoria)
5) Checkear y eliminar posibles nuevos duplicados

-------------------------------------------------------------------

POSIBLES MEJORAS A FUTURO

1) Limpiar el dataframe al completo
2) Estudiar en profundidad el sujeto del análisis, los tiburones, para obtener más insights interesantes.
