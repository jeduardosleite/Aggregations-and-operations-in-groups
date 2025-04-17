Exercise 1 for module 9, about functions agg, groupby and pivot_table.

Additionally, I created insights with map visualization, learning new analysis methods and Python.


import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import folium
import geopandas as gpd
import json

%matplotlib inline

sinasc_raw = pd.read_csv('SINASC_RO_2019.csv')
mapa_ro = gpd.read_file('RO_Municipios_2023.shp')

import unidecode

sinasc_raw['munResNome'] = sinasc_raw['munResNome'].str.upper().apply(unidecode.unidecode)
mapa_ro['NM_MUN'] = mapa_ro['NM_MUN'].str.upper().apply(unidecode.unidecode)

dados_nascimentos = sinasc_raw['munResNome'].value_counts().reset_index()
dados_nascimentos.columns = ['NM_MUN', 'nascimentos'] 
mapa_com_dados = mapa_ro.merge(dados_nascimentos, on='NM_MUN', how='left')

from folium import Choropleth

mapa = folium.Map(location=[-10.9, -63.5], zoom_start=12)

Choropleth(
    geo_data=mapa_com_dados,
    data=mapa_com_dados,
    columns=['NM_MUN', 'nascimentos'],  # <- aqui está a mudança
    key_on='feature.properties.NM_MUN',  # <- e aqui
    fill_color='YlGnBu',
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name='Nascimentos por Município - RO (2019)'
).add_to(mapa)

mapa
