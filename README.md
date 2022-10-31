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

    - Definimos función para ver valores nulos por columnas y vemos que no todas las columnas con len(NaN) > 80% nos interesan ser borradas, porque tenemos un gran número de registros/filas en el que todas o casi todas las columnas tienen valores nulos. 
    - Creamos una columna 'num_nan' que cuente el número de nulos por fila. Nos damos cuenta de que vale más borrar esas filas con tantos nulos y a partir de ahí trabajar con las columnas que nos interesan. Sí que vamos a borrar las dos últimas columnas, pues no proporcionan conocimiento sobre qué se está midiendo con su nombre "unnamed" y para más inri, cerca del 100% de los registros son nulos para esas dos columnas.
   
   
![NaN values](https://user-images.githubusercontent.com/112175733/199070004-aa218512-0219-4ca7-8dde-d3001e166335.png)

Para eliminar las filas fijamos un umbral de más de 15 nulos. Tras hacer esto, borramos la columna "num_nan" porque ya no nos sirve.
        
![clean_nan](https://user-images.githubusercontent.com/112175733/199071030-8f5ce820-e5a4-48ea-86e2-1b7b5ce57041.png)

Hemos pasado de tener 25723 filas a tan solo 6302.
    
 b) Para el resto de NaN, exploramos valores y significado de columnas y valoramos por qué lo sustituimos (0, unknown...).
        
 - En mi caso, he optado por rellenar todas las columnas restantes con valores nulos con el valor 'unknown'
        
 ![nan_fill](https://user-images.githubusercontent.com/112175733/199072470-fa404a5e-543a-4efe-94dc-ce7aae2bd881.png)
        
 ![nan_cols0](https://user-images.githubusercontent.com/112175733/199072973-561226c2-4b68-4f43-9585-9bb1a2fc9c81.png)
 
 

4) ¿Qué columnas me interesa analizar más profundamente, ergo limpiar y preparar? 
 
    En mi caso, voy a elegir las columnas 'Year', 'Country', 'Sex', 'Age', 'Fatal' y 'Species' para posteriormente explorar insights en torno a ellas.
    
    a) En primer lugar, he limpiado la columna de Sex. Para ello he sustituido las 'M ' con M, las 'N' con 'M', y 'lli' y '.' con unknown.
    
    b) En segundo lugar he limpiado la columna de Fatal.
    
               - sharks1.Fatal.replace(['UNKNOWN', ' N', 'M', '2017', 'N ', 'y'], 
                                       ['unknown', 'N', 'N', 'unknown', 'N', 'Y'],
                                       inplace=True)
    
    c) Después he creado una nueva función y una nueva columna 'fatal_count'. Si ha habido fatalidades, añade 1 a la nueva casilla. Si no, añade un 0.
 
            def fatal_Count(x):
    
                if x == 'Y':
                    return 1
                else:
                    return 0
            
            sharks1['fatal_count']=sharks1.Fatal.apply(fatal_Count)

    d) Para la columna Year he rellenado los dos registros que estaban con valores nulos con el año que aparecía en las columnas Case_number y date.
    
            sharks1.loc[sharks1['Year'] == 'unknown']
            
            sharks1.at[187, 'Year'] = 2017
            sharks1.at[6079, 'Year'] = 1836
            
            sharks1['Year'] = sharks1['Year'].astype('int')
       
    e) Para la columna countries, he limpiado con str.contains y a destacar, aquellos ataques acontecidos entre dos países o en mares y océanos los he calificado como acontecidos en aguas internacionales.
    
    f) Para la columna Age he definido la función clean_Age y se la he aplicado a la columna Age para que se vea reflejada en una nueva columna mean_age. En resumen, va a coger aquellas celdas con más de una edad y va a devolver la media de las edades que aparecen en la celda.
    
                    def clean_Age(x):
                            x = x.lower()
                            patron = re.findall('\d+', x)[:5]
                            res = [eval(i) for i in patron]


                            if patron:
                                return np.mean(res)
                            elif "2to3months" in x: # around 0.208
                                return 0.2
                            elif "9months" in x:
                                return 0.75
                            elif "18months" in x:
                                return 1.5
                            elif "teen" in x: #entre 13 y 19
                                return 16
                            elif "young" in x: #entre 15 y 24
                                return 19.5
                            elif "adult" in x: # entre 18 y 65
                                return 41.5
                            elif "middle" in x : # entre 40 y 60
                                return 50
                            elif "elderly" in x: # +65
                                return 70
                            else:
                                return 'unknown'
                
                    sharks1['mean_age']=sharks1.Age.apply(clean_Age)

    g) Para la columna Species, he ido limpiando con str.contains.
    - Dentro me encontré que no podía asignar una especie a los valores de tiburones con tamaño o edad específicos, porque podrían ser distintas especies en distintas fases de crecimiento, por lo que decidí asignarle el valor 'unknown'.

5) A continuación, he cambiado el tipo de datos de los campos de tipo int y float que tenemos para optimizar algo de memoria. También he convertido los campos de tipo object en category, puesto que ya no queremos añadirles ningún valor único más.

6) Por último he comprobado que no hubiera posibles nuevos duplicados.

-------------------------------------------------------------------

💹INSIGHTS


Nº de ataques por año
----

- He restringido a los ataques en los últimos 50 años, porque es el periodo en el que más registros había.
    
        plt.figure(figsize=(10, 6))
        data = sharks2[sharks2.Year > 1970].Year

        data.hist(bins=500, width=0.8)
        plt.xlabel("Year", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.ylabel("Attacks", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.title("Attacks per year", fontweight='bold');
        

![Attacks_per_year](https://user-images.githubusercontent.com/112175733/199081214-a8c7ca2e-da8e-47d1-b36f-d39d9aba60b8.png)

Se aprecia un crecimiento en las últimas dos décadas, quizás las actividades acuáticas de los humanos con mayor peligrosidad frente a tiburones han crecido en este periodo.


Nº de ataques recibidos por sexo
----

        n_unk = sharks2['Sex'] != 'unknown'
        
        sharks4 = sharks2[n_unk]
        
        plt.figure(figsize=(9, 5))

        sharks4.Sex.value_counts().plot.barh();
        plt.xlabel("Attacks", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.ylabel("Sex", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.title("Attacks per sex", fontweight='bold');
        

![Attacks_per_sex](https://user-images.githubusercontent.com/112175733/199081740-e8279103-7027-4dca-b44c-cca3b65000ce.png)


Claramente hay más ataques a hombres que a mujeres. Podría estar relacionado con que las actividades con mayor riesgo frente a este tipo de ataques son realizadas principalmente por hombres.


Fatalidad por especie de tiburón y países en los que más ataca cada especie
----

Agrupamos las especies por número de fatalidades en base a la columna 'fatal_count' y por la moda de 'Country' para cada especie.

        not_unknown = sharks2['Species'] != 'unknown'
        
        sharks3 = sharks2[not_unknown]
        
        fatal = sharks3.groupby('Species').agg({'fatal_count': 'sum', 'Country': lambda x: x.mode().tolist()[:1]})



                    fatal_count	Country
        Species		
        Angel shark	0.0	[SPAIN]
        Basking shark	2.0	[SCOTLAND]
        Blue shark	11.0	[AUSTRALIA]
        Bronze whaler shark	6.0	[AUSTRALIA]
        Bull shark	40.0	[USA]
        Dogfish shark	0.0	[AUSTRALIA]
        Dusky shark	0.0	[SOUTH AFRICA]
        Galapagos shark	2.0	[ECUADOR]
        Goblin shark	0.0	[JAPAN]
        Grey reef shark	1.0	[AUSTRALIA]
        Hammerhead shark	2.0	[USA]
        Lemon shark	0.0	[USA]
        Leopard shark	0.0	[AUSTRALIA]
        Mako shark	2.0	[USA]
        Nurse shark	0.0	[USA]
        Porbeagle shark	0.0	[BRITISH ISLES]
        Raggedtooth shark	0.0	[SOUTH AFRICA]
        Salmon shark	0.0	[USA]
        Sand shark	0.0	[USA]
        Silvertip shark	0.0	[SUDAN]
        Soupfin shark	0.0	[SOUTH AFRICA]
        Spinner shark	0.0	[USA]
        Thresher shark	0.0	[USA]
        Tiger shark	68.0	[USA]
        Whale shark	0.0	[MOZAMBIQUE]
        White shark	150.0	[USA]
        Wobbegong shark	0.0	[AUSTRALIA]
        Zambezi shark	9.0	[SOUTH AFRICA]
        unknown	0.0	[]
        

        plt.figure(figsize=(15, 9))


        fatal[(fatal.fatal_count>0)].plot.barh();
        plt.xlabel("Fatalities", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.ylabel("Species", labelpad=10, fontweight='semibold', fontstyle='italic')
        plt.title("Fatalities per species", fontweight='bold');
        
![Fatalities_per_species](https://user-images.githubusercontent.com/112175733/199082573-6cf0de41-0b6d-4b5a-9fd3-8c79c95da6db.png)


El top 3 de tiburones con más fatalidades registradas son el Blanco, el Tigre y el Toro.

-------------------------------------------------------------------

POSIBLES MEJORAS A FUTURO

1) Limpiar el dataframe al completo
2) Estudiar en profundidad el sujeto del análisis, los tiburones, para obtener más insights interesantes.
