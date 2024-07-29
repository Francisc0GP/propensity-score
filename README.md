# propensity-score
### Welcome to the Propensity Score Matching and Data Mining Project

Hello,

Welcome to my repository! Here, we will explore and implement techniques related to propensity score matching and data mining. This project aims to combine statistical methods with data analysis to uncover meaningful patterns and insights.

#### *Project Objectives:*

1. *Propensity Score Matching (PSM):*
   - Propensity score matching is a statistical technique used to estimate the effect of a treatment by accounting for the covariates that predict receiving the treatment. 
   - We will cover the basics of propensity score calculation, matching algorithms, and evaluating the balance between treated and control groups.
   
   #### *libraries*
   ```
   import pandas as pd
   import warnings
   from psmpy import PsmPy
   from psmpy.functions import cohenD
   from psmpy.functions import *
   sns.set(rc={'figure.figsize':(10,8), 'font.size': 1.3})
   ```
   ### Librarie PsmPy 0.3.13 *link https://pypi.org/project/psmpy/*
   #### *Read csv*
   ```
   warnings.filterwarnings("ignore")
   df_dataset = pd.read_csv('datasets/SetRegexFin.csv')
   df_comorb = pd.read_csv('datasets/regex_comorb.csv')
   df_antfam = pd.read_csv('datasets/regex_antfam.csv')
   df_diag = pd.read_csv('datasets/regex_diag.csv')
   df_catescolar = pd.read_csv('datasets/CatEscolaridad.csv')
   df_catedociv = pd.read_csv('datasets/CatEstadoCivil.csv')
   df_catlgbt = pd.read_csv('datasets/CatLgbt.csv')
   df_catEstrato = pd.read_csv('datasets/CatEstratoSocial.csv')
   df_catocup = pd.read_csv('datasets/CatOcupacion.csv')
   df_pais = pd.read_csv('datasets/Pais.csv')
   df_lugnac = pd.read_csv('datasets/LugNac.csv')
   df_EUA = pd.read_csv('datasets/EstadosEUA.csv')
   df_cat008 = pd.read_csv('datasets/CatComunMet008.csv')
   df_nivsoc = pd.read_csv('datasets/CatEstratoSocial.csv')
   ```
   #### *Def function*
   ```
   def gen_list(df):
    """
    Generate a list of columns in a DataFrame
    param: df_DataFrame
    return: list of columns
    """
    listexc = []
    list = ['FolioId','Migracion', 'Edad', 'Sexo', 'EstadoCivil', 'Escolaridad' , 'Ocupacion', 'NivSocioEco', 'ComunidadIndigena', 'DiscapacidadPerceptual' ] # Columns to include
    for col in df.columns:
        if col not in list:
            listexc.append(col) # Columns to exclude
    return listexc 

   def get_psm(df_dataset, listinclude):
    """
    Initialize PsmPy Class
    param: df_DataFrame, list of columns to exclude
    return: psm object
    """
    psm = PsmPy(df_dataset, treatment= 'Migracion', indx= 'FolioId', exclude = listinclude)
    psm.logistic_ps()
    return psm
   ```
   #### Note: 

PsmPy - The class. It will use all covariates in the dataset unless formally excluded in the exclude argument.
df - the dataframe being passed to the class
exclude - (optional) parameter and will ignore any covariates (columns) passed to the it during the model fitting process. This will be a list of strings. Note, it is not necessary to pass the unique index column here. That process will be taken care of within the code after specifying your index column.
indx - required parameter that references a unique ID number for each case in the dataset.

   ```
   def moddatagen(df_dataset):
    """
    Modify the DataFrame to be used in the PSM
    param: df_DataFrame
    return: df_DataFrame modified
    """
    df_dataset['Migracion'] = df_dataset['Migracion'].fillna(0)
    df_dataset['Migracion'] = df_dataset['Migracion'].replace('Si', 1)
    df_dataset['ComunidadIndigena'] = df_dataset['ComunidadIndigena'].fillna(0)
    df_dataset['ComunidadIndigena'] = df_dataset['ComunidadIndigena'].replace('Si', 1)
    df_dataset['DiscapacidadPerceptual'] = df_dataset['DiscapacidadPerceptual'].fillna(0)
    df_dataset['DiscapacidadPerceptual'] = df_dataset['DiscapacidadPerceptual'].replace('Si', 1)
    df_dataset['Migracion']
    return df_dataset

    def mod_data (df ,dict_escolar, dict_edociv, dict_lgbt, dict_ocup, dict_pais, dict_nivsoc):
    """
    Modify the DataFrame to be used in the PSM
    param: df_DataFrame, dictionaries
    return: df_DataFrame modified
    """
    df['Escolaridad'] = df['Escolaridad'].map(dict_escolar)
    df['EstadoCivil'] = df['EstadoCivil'].map(dict_edociv)
    df['LGBTTTI'] = df['LGBTTTI'].map(dict_lgbt)
    df['Ocupacion'] = df['Ocupacion'].map(dict_ocup)
    df['PaisNac'] = df['LugarNacimiento'].map(dict_pais)
    df['NivSocioEco'] = df['NivSocioEco'].map(dict_nivsoc)
    return df

   def graf_psm(psm):
    """
    Generate a Propensity Score Matching 
    param: psm object
    return: plots
    """
    #psmdata = psm.knn_matched(matcher='propensity_logit', replacement=False, caliper=None, drop_unmatched=True) match 1:1
    psmdata = psm.knn_matched_12n(matcher='propensity_logit', how_many= 2) # match 1:2
    psm.plot_match(Title='Side by side matched controls', Ylabel='Number of patients', Xlabel='Propensity logit', names = ['Migracion', 'No Migracion'], colors = ['blue', 'red'], save=True)
   ```
   #### Result
   dict_escolar, dict_edociv, dict_lgbt, dict_ocup, dict_pais, dict_nivsoc  = dict_data(df_catescolar, df_catedociv, df_catlgbt, df_catocup, df_pais, df_nivsoc)
   df = mod_df(df_dataset)
   listexclude = gen_list(df_dataset)
   df_dataset = moddatagen(df_dataset)
   df = mod_data(df_dataset, dict_escolar, dict_edociv, dict_lgbt, dict_ocup, dict_pais, dict_nivsoc)
   psm = get_psm(df, listexclude)
graf_psm(psm)
   ```


2. *Data Mining:*
   - Data mining involves extracting useful information from large datasets using various methods such as clustering, classification, and association rule mining.
   - We will apply data mining techniques to our matched datasets to discover patterns, trends, and insights that could drive decision-making

