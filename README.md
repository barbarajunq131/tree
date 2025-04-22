# tree
base de dados de espécies vegetais
import streamlit as st
import pandas as pd

# Título do aplicativo
st.title("Consulta de Árvores Nativas e Exóticas")

# Upload do arquivo (opcional se já estiver no Streamlit Cloud)
# st.file_uploader("Faça upload da planilha", type=["xls", "xlsx"])

# Carregar a planilha
df = pd.read_excel("BD_espécies vegetais.xls", sheet_name="lista base")

# Mostrar dataframe completo se desejar
# st.dataframe(df)

# Filtros interativos
nome_popular = st.text_input("Filtrar por Nome Popular")
nome_cientifico = st.text_input("Filtrar por Nome Científico")
grupo = st.selectbox("Filtrar por Grupo", ["Todos"] + sorted(df["GRUPO"].dropna().unique().tolist()))
uso = st.multiselect("Filtrar por Uso", df["USO"].dropna().unique())

# Aplicar filtros
resultado = df.copy()

if nome_popular:
    resultado = resultado[resultado["NOME POPULAR"].str.contains(nome_popular, case=False, na=False)]

if nome_cientifico:
    resultado = resultado[resultado["NOME CIENTÍFICO"].str.contains(nome_cientifico, case=False, na=False)]

if grupo != "Todos":
    resultado = resultado[resultado["GRUPO"] == grupo]

if uso:
    resultado = resultado[resultado["USO"].isin(uso)]

# Exibir resultados
st.write(f"Total de espécies encontradas: {len(resultado)}")
st.dataframe(resultado)
