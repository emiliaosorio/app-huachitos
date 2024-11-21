import streamlit as st
import requests
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
import random
from PIL import Image
import numpy as np

@st.cache_data

def obtener_datos():
    url = 'https://huachitos.cl/api/animales'
    respuesta = requests.get(url)
    respuesta.raise_for_status() 
    datos = respuesta.json()
    if 'data' in datos:
        df = pd.DataFrame(datos['data'])
        return df

st.title('Aplicación Huachitos')
st.text('Huachitos es una plataforma colaborativa de adopción animal.')
st.text('La iniciativa ha nacido de la preocupación de los creadores por...')

datos = obtener_datos()

def convertir_a_años(edad):
    if "Meses" in edad or "Mes" in edad:
        return int(edad.split()[0]) / 12 
    elif "Año" in edad or "Años" in edad:
        return int(edad.split()[0])  
    else:
        return None  
    
datos["edad_años"] = datos["edad"].apply(convertir_a_años)

if st.checkbox('Mostrar datos de la API'):
  st.subheader('Datos: ')
  st.write(datos)

def vacunas():
  vacunados = datos[datos['vacunas'] == 1]

  conteo_vacunados = vacunados.groupby('edad_años').size()

  fig, ax = plt.subplots(figsize=(10, 5))
  ax.plot(conteo_vacunados.index, conteo_vacunados.values, color=random.choice(list(mcolors.CSS4_COLORS.keys())))
  ax.set_xlabel("Edad (Años)")
  ax.set_ylabel("Cantidad de Animales Vacunados")
  ax.set_title("Cantidad de Animales Vacunados por Edad")
  ax.grid(True)
  st.subheader("Gráfico de Líneas: Vacunación", divider=True)
  st.pyplot(fig)

def esterilizado():
  esterilizados = datos[datos['esterilizado'] == 1]

  conteo_esterilizados = esterilizados.groupby('edad_años').size()

  fig, ax = plt.subplots(figsize=(10, 5))
  ax.plot(conteo_esterilizados.index, conteo_esterilizados.values, color=random.choice(list(mcolors.CSS4_COLORS.keys())))
  ax.set_xlabel("Edad (Años)")
  ax.set_ylabel("Cantidad de Animales Esterilizados")
  ax.set_title("Cantidad de Animales Esterilizados por Edad")
  ax.grid(True)
  st.subheader("Gráfico de Líneas: Esterilización", divider=True)
  st.pyplot(fig)

def comuna():
  conteo_comunas = datos['comuna'].value_counts()

  fig, ax = plt.subplots(figsize = (12, 6))

  ax.bar(conteo_comunas.index, conteo_comunas.values, color=random.choice(list(mcolors.CSS4_COLORS.keys())))
  ax.set_xlabel("Comuna")
  ax.set_ylabel("Número de animales")
  ax.set_title("Cantidad de animales por comuna")
  
  ax.tick_params(axis='x', rotation=90, labelsize=10)
  plt.tight_layout()
  ax.grid(True)
  st.pyplot(fig)

def comuna_especifica(comuna):

  perros = datos[(datos['comuna'] == comuna) & (datos['tipo'] == 'Perro')] 
  gatos = datos[(datos['comuna'] == comuna) & (datos['tipo'] == 'Gato')] 
  conejos = datos[(datos['comuna'] == comuna) & (datos['tipo'] == 'Conejo')] 

  num_perros= len(perros)
  num_gatos = len(gatos)
  num_conejos = len(conejos)

  animales = np.array([num_perros, num_gatos, num_conejos])
  etiquetas_animales = ["Perros", "Gatos", "Conejos"]
  fig, ax = plt.subplots(figsize = (10, 5))

  ax.bar(etiquetas_animales, animales, color=random.choice(list(mcolors.CSS4_COLORS.keys())))
  ax.set_xlabel("Tipo de Animal")
  ax.set_ylabel("Número de animales")
  ax.set_title(f"Número de animales en {comuna}")
  
  plt.tight_layout()
  ax.grid(True)
  st.pyplot(fig)

def tipo():
  perros = datos.loc[datos['tipo'] == 'Perro']
  gatos = datos.loc[datos['tipo'] == 'Gato']
  conejos = datos.loc[datos['tipo'] == 'Conejo']

  num_perros= len(perros)
  num_gatos = len(gatos)
  num_conejos = len(conejos)

  animales = np.array([num_perros, num_gatos, num_conejos])
  etiquetas_animales = ["Perros", "Gatos", "Conejos"]
  fig, ax = plt.subplots(figsize = (10, 5))

  ax.bar(etiquetas_animales, animales, color=[random.choice(list(mcolors.CSS4_COLORS.keys())), random.choice(list(mcolors.CSS4_COLORS.keys())), random.choice(list(mcolors.CSS4_COLORS.keys()))])
  ax.set_xlabel("Tipo de Animal")
  ax.set_ylabel("Número de animales")
  ax.set_title(f"Número de animales por tipo registrados en Huachitos")
  
  plt.tight_layout()
  ax.grid(True)
  st.pyplot(fig)

def tipo_especifico(tipo):
  machos = datos[(datos['genero'] == 'macho') & (datos['tipo'] == tipo)] 
  hembras = datos[(datos['genero'] == 'hembra') & (datos['tipo'] == tipo)] 

  num_machos = len(machos)
  num_hembras = len(hembras)

  info_generos = np.array([num_machos, num_hembras])

  etiquetas_generos = ["Machos", "Hembras"]

  fig, ax = plt.subplots(figsize = (10, 5))

  explode = (0, 0.1)
  ax.pie(info_generos, explode=explode, labels=etiquetas_generos, autopct='%1.1f%%',colors=[random.choice(list(mcolors.CSS4_COLORS.keys())), random.choice(list(mcolors.CSS4_COLORS.keys()))], shadow=True, startangle=90)
  ax.set_title(f"Proporción de machos y hembras ({tipo})")
  st.pyplot(fig)

def imagen(nombre):
  imagen = datos.loc[datos['nombre'] == nombre, 'imagen'].iloc[0]
  edad = datos.loc[datos['nombre'] == nombre, 'edad'].iloc[0]
  genero = datos.loc[datos['nombre'] == nombre, 'genero'].iloc[0]
  comuna = datos.loc[datos['nombre'] == nombre, 'comuna'].iloc[0]

  if datos.loc[datos['nombre'] == nombre, 'vacunas'].iloc[0] == '1':  
    vacunado = 'Está vacunado/a'
  else:
    vacunado = 'No está vacunado/a'

  
  if datos.loc[datos['nombre'] == nombre, 'esterilizado'].iloc[0] == '1':  
    esterilizado =  'está esterilizado/a'
  else:
    esterilizado = 'no está esterilizado/a'
  

  descripcion_fisica = datos.loc[datos['nombre'] == nombre, 'desc_fisica'].iloc[0]
  descripcion_personalidad = datos.loc[datos['nombre'] == nombre, 'desc_personalidad'].iloc[0]
  descripcion_adicional = datos.loc[datos['nombre'] == nombre, 'desc_adicional'].iloc[0]
  
  st.subheader(f"Conoce a {nombre_seleccionado}: ", divider=True)
  st.write(f"Es {genero}, tiene {edad} y proviene de la comuna de {comuna}.")
  st.write(f"{vacunado} y {esterilizado}.")
  st.subheader("Desripción física: ", divider=True)
  st.write(descripcion_fisica.replace("</p>"," ").replace("<p>"," "))
  st.subheader("Desripción de personalidad: ", divider=True)
  st.write(descripcion_personalidad.replace("</p>"," ").replace("<p>"," "))
  st.subheader("Desripción adicional: ", divider=True)
  st.write(descripcion_adicional.replace("</p>"," ").replace("<p>"," "))
  st.image(imagen)
   
def edad():
  datos["tipo_num"] = datos["tipo"].apply(lambda x: 1 if x == "Perro" else 2 if x == "Conejo" else 0)

  fig, ax = plt.subplots(figsize=(10, 5))
  ax.scatter(datos["edad_años"], datos["tipo_num"], alpha=0.6, c=datos["tipo_num"], cmap="inferno", edgecolor="k")

  ax.set_xlabel("Edad (Años)")
  ax.set_ylabel("Tipo de Animal")
  ax.set_title("Distribución de Edad por Tipo de Animal")
  ax.set_yticks([0, 1, 2], ["Gato", "Perro", "Conejo"])
  ax.grid(True)
  st.subheader("Gráfico de Dispersión", divider=True)
  st.pyplot(fig)

def minimos_maximos():
  edad_max = datos['edad_años'].max()
  edad_min = datos['edad_años'].min()

  nombres_max = datos.loc[datos['edad_años'] == edad_max, 'nombre'].tolist()
  nombres_min = datos.loc[datos['edad_años'] == edad_min, 'nombre'].tolist()

  tipo_max = datos.loc[datos['edad_años'] == edad_max, 'tipo'].tolist()
  tipo_min = datos.loc[datos['edad_años'] == edad_min, 'tipo'].tolist()

  st.subheader("Edad Máxima y Mínima: ", divider=True)
  st.write(f"La edad máxima registrada es: {edad_max} años. Su nombre es {', '.join(nombres_max)} y su tipo es: {', '.join(tipo_max)}")
  st.write(f"La edad mínima registrada es: {edad_min * 12:.2f} meses. Sus nombres son: {', '.join(nombres_min)}. Y sus tipos son: {', '.join(tipo_min)} respectivamente")

resultado = st.selectbox("Selecciona una palabra clave para mostrar información:", ["tipo de animal", "edad", "género", "esterilización", "vacunas", "rescatados", "comuna"])

if resultado == 'vacunas':
  vacunas()

  tasa_vacunacion = datos['vacunas'].mean() * 100 
  st.subheader("Tasa de Vacunación", divider=True)
  st.write(f"El {tasa_vacunacion:.2f}% de animales se encuentran vacunados.")

elif resultado == 'esterilización':
  esterilizado()

  tasa_esterilizacion = datos['esterilizado'].mean() * 100 
  st.subheader("Tasa de Esterilización", divider=True)
  st.write(f"El {tasa_esterilizacion:.2f}% de animales se encuentran esterilizados.")
elif resultado == 'comuna':
  comuna()
  comunas_disponibles = datos['comuna'].unique()
  comuna_seleccionada = st.selectbox("Si deseas, puedes ver gráficos específicos por comuna:", comunas_disponibles)
  if comuna_seleccionada:
    comuna_especifica(comuna_seleccionada)
elif resultado == 'tipo de animal':
  tipo()
  tipos_disponibles = datos['tipo'].unique()
  tipo_seleccionado = st.selectbox("Si deseas, puedes ver gráficos específicos por tipo de animal:", tipos_disponibles)
  if tipo_seleccionado:
    tipo_especifico(tipo_seleccionado)
elif resultado == 'género':
  st.subheader("Gráfico de Torta: Géneros", divider=True)
  tipo = st.radio("Selecciona un tipo: ", ["Perro", "Gato", "Conejo"])
  tipo_especifico(tipo)
elif resultado == 'rescatados':
  nombres_disponibles = datos['nombre'].unique()
  nombre_seleccionado = st.selectbox("Selecciona un nombre para ver su foto:", nombres_disponibles)
  if nombre_seleccionado:
    imagen(nombre_seleccionado)
elif resultado == 'edad':
  edad()
  prom_edades = datos["edad_años"].mean() 

  st.subheader("Promedio de edad", divider=True)
  st.write(f"\nEl promedio de edades entre perros, gatos y conejos es: {prom_edades:.2f} años")
  minimos_maximos()
else:
  st.write("No hay gráfico para mostrar.")    
