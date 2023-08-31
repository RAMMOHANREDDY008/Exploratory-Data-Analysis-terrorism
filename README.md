#  CVIP Exploratory-Data-Analysis-terrorism
by using google colab  
coder.ipynb
import time
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
from matplotlib import animation
import numpy as np
import pandas as pd
pd.options.mode.chained_assignment = None
import seaborn as sns
import plotly.express as px
terror_df = pd.read_csv('/content/Base for Analysis.csv', sep=',', encoding="ISO-8859-1")

eventid	iyear	imonth	iday	extended	country_txt	region_txt	city	latitude	longitude	...	nperps	claimed	weaptype1_txt	nkill	nkillter	nwound	propextent_txt	ishostkid	ransom	nreleased
0	197000000002	1970	0	0	0	Mexico	North America	Mexico city	19.371887	-99.086624	...	7.0	NaN	Unknown	0.0	NaN	0.0	NaN	1.0	1.0	NaN
1	197001000001	1970	1	0	0	Philippines	Southeast Asia	Unknown	15.478598	120.599741	...	NaN	NaN	Unknown	1.0	NaN	0.0	NaN	0.0	0.0	NaN
2	197001000002	1970	1	0	0	Greece	Western Europe	Athens	37.997490	23.762728	...	NaN	NaN	Explosives	NaN	NaN	NaN	NaN	0.0	0.0	NaN
3	197001000003	1970	1	0	0	Japan	East Asia	Fukouka	33.580412	130.396361	...	NaN	NaN	Incendiary	NaN	NaN	NaN	NaN	0.0	0.0	NaN
4	197001010002	1970	1	1	0	United States	North America	Cairo	37.005105	-89.176269	...	-99.0	0.0	Firearms	0.0	0.0	0.0	Minor (likely < $1 million)	0.0	0.0	NaN
5	197001050001	1970	1	1	0	United States	North America	Baraboo	43.468500	-89.744299	...	NaN	NaN	Explosives	0.0	NaN	0.0	Minor (likely < $1 million)	0.0	0.0	NaN
6	197001020001	1970	1	2	0	Uruguay	South America	Montevideo	-34.891151	-56.187214	...	3.0	NaN	Firearms	0.0	NaN	0.0	NaN	0.0	0.0	NaN
7	197001020002	1970	1	2	0	United States	North America	Oakland	37.791927	-122.225906	...	-99.0	0.0	Explosives	0.0	0.0	0.0	Minor (likely < $1 million)	0.0	0.0	NaN
8	197001020003	1970	1	2	0	United States	North America	Madison	43.076592	-89.412488	...	1.0	1.0	Incendiary	0.0	0.0	0.0	Minor (likely < $1 million)	0.0	0.0	NaN
9	197001030001	1970	1	3	0	United States	North America	Madison	43.072950	-89.386694	...	1.0	0.0	Incendiary	0.0	0.0	0.0	Minor (likely < $1 million)	0.0	0.0	NaN
10 rows × 29 columns
terror_df.columns

Index(['eventid', 'iyear', 'imonth', 'iday', 'extended', 'country_txt',
       'region_txt', 'city', 'latitude', 'longitude', 'vicinity', 'crit1',
       'multiple', 'success', 'suicide', 'attacktype1_txt', 'targtype1_txt',
       'natlty1_txt', 'gname', 'nperps', 'claimed', 'weaptype1_txt', 'nkill',
       'nkillter', 'nwound', 'propextent_txt', 'ishostkid', 'ransom',
       'nreleased'],
      dtype='object')
      terror_df.info(verbose = True)
      <class 'pandas.core.frame.DataFrame'>
RangeIndex: 62326 entries, 0 to 62325
Data columns (total 29 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   eventid          62326 non-null  int64  
 1   iyear            62326 non-null  int64  
 2   imonth           62326 non-null  int64  
 3   iday             62326 non-null  int64  
 4   extended         62326 non-null  int64  
 5   country_txt      62326 non-null  object 
 6   region_txt       62326 non-null  object 
 7   city             62326 non-null  object 
 8   latitude         58994 non-null  float64
 9   longitude        58993 non-null  float64
 10  vicinity         62325 non-null  float64
 11  crit1            62325 non-null  float64
 12  multiple         62325 non-null  float64
 13  success          62325 non-null  float64
 14  suicide          62325 non-null  float64
 15  attacktype1_txt  62325 non-null  object 
 16  targtype1_txt    62325 non-null  object 
 17  natlty1_txt      61962 non-null  object 
 18  gname            62325 non-null  object 
 19  nperps           8737 non-null   float64
 20  claimed          1356 non-null   float64
 21  weaptype1_txt    62325 non-null  object 
 22  nkill            56394 non-null  float64
 23  nkillter         3136 non-null   float64
 24  nwound           54504 non-null  float64
 25  propextent_txt   17140 non-null  object 
 26  ishostkid        62150 non-null  float64
 27  ransom           61959 non-null  float64
 28  nreleased        1147 non-null   float64
dtypes: float64(15), int64(5), object(9)
memory usage: 13.8+ MB
terror_df[['nkillter', 'nkill', 'nwound', 'propextent_txt', 
        'nreleased']].describe().transpose()
        
count	mean	std	min	25%	50%	75%	max
nkillter	3136.0	0.564413	2.856761	0.0	0.0	0.0	0.0	85.0
nkill	56394.0	2.065096	9.549799	0.0	0.0	0.0	1.0	1180.0
nwound	54504.0	1.959838	26.783140	0.0	0.0	0.0	0.0	5500.0
nreleased	1147.0	8.605929	34.565943	-99.0	1.0	1.0	3.0	390.0
f = plt.figure(figsize=(20, 7))

sns.set(font_scale = 1.1)
sns.set_theme(style = "darkgrid")
xaxis = sns.countplot(x = 'iyear', data = terror_df)
xaxis.set_xticklabels(xaxis.get_xticklabels(), rotation=60)
plt.ylabel('Count', fontsize=12)
plt.xlabel('Year', fontsize=12)
plt.title('Number of Terrorist Attack by Year', fontsize = 12)
Text(0.5, 1.0, 'Number of Terrorist Attack by Year')
f = plt.figure(figsize=(16, 8))

sns.set(font_scale=0.7)
sns.countplot(y='region_txt', data=terror_df)
plt.ylabel('Region_txt', fontsize=12)
plt.xlabel('success', fontsize=12)
plt.title('Number of Terrorist Attack by Region', fontsize=12)
Text(0.5, 1.0, 'Number of Terrorist Attack by Region')
![image](https://github.com/RAMMOHANREDDY008/Exploratory-Data-Analysis-terrorism/assets/96512613/f92439bf-3f68-436f-81d8-420ac43726fb)
f = plt.figure(figsize=(20, 8))

sns.set(font_scale=0.8)
sns.countplot(x='attacktype1_txt', data=terror_df,)
plt.xlabel('Methods of Attack', fontsize=12)
plt.ylabel('success', fontsize=12)
plt.title('Types of Terrorist Attack ', fontsize=12)
Text(0.5, 1.0, 'Types of Terrorist Attack ')
![image](https://github.com/RAMMOHANREDDY008/Exploratory-Data-Analysis-terrorism/assets/96512613/1d5f4d74-a939-47c6-b9af-a0a9da1e69e0)
f = plt.figure(figsize=(20, 8))

sns.set(font_scale=0.8)
xaxis = sns.countplot(x='targtype1_txt', data=terror_df,)

xaxis.set_xticklabels(xaxis.get_xticklabels(), rotation=60)
plt.xlabel('Target Types', fontsize=12)
plt.ylabel('success', fontsize=12)
plt.title('Types of Target', fontsize=12)
Text(0.5, 1.0, 'Types of Target')
![image](https://github.com/RAMMOHANREDDY008/Exploratory-Data-Analysis-terrorism/assets/96512613/40b13830-a519-4001-8f26-6d7761cb879f)
fig= plt.figure(figsize=(16, 10))
sns.set(font_scale=0.9)
terror_country = sns.barplot(x=terror_df['country_txt'].value_counts()[0:15].index, y=terror_df['country_txt'].value_counts()[0:15], palette='RdYlGn')
terror_country.set_xticklabels(terror_country.get_xticklabels(), rotation=70)
terror_country.set_xlabel('country_txt', fontsize=12)
terror_country.set_ylabel('success', fontsize=12)
plt.title('Top 15 Countries: Most Attacks by Terrorist Groups', fontsize=12)
Text(0.5, 1.0, 'Top 15 Countries: Most Attacks by Terrorist Groups')
![image](https://github.com/RAMMOHANREDDY008/Exploratory-Data-Analysis-terrorism/assets/96512613/0de10695-4ab7-469f-bf7b-39efa435ab2e)
region_year = pd.crosstab(terror_df.iyear, terror_df.region_txt)

region_year.head(20)
region_txt	Australasia & Oceania	Central America & Caribbean	Central Asia	East Asia	Eastern Europe	Middle East & North Africa	North America	South America	South Asia	Southeast Asia	Sub-Saharan Africa	Western Europe
iyear												
1970	1	7	0	2	12	28	472	65	1	10	3	50
1971	1	5	0	1	5	55	247	24	0	6	2	125
1972	8	3	0	0	1	53	73	33	1	16	4	376
1973	1	6	0	2	1	19	64	83	1	2	4	290
1974	1	11	0	4	2	42	111	81	2	3	7	317
1975	0	9	0	12	0	44	159	55	4	7	12	438
1976	0	45	0	2	0	55	125	91	4	12	11	578
1977	0	24	0	4	2	211	149	119	2	8	29	771
1978	2	199	0	35	2	128	117	222	2	44	46	729
1979	2	609	0	16	1	455	79	236	34	86	124	1020
1980	7	1070	0	1	1	437	75	319	12	87	58	594
1981	3	1148	0	4	4	312	77	383	23	50	98	484
1982	2	996	0	3	3	290	85	639	20	43	60	402
1983	0	858	0	13	2	334	47	950	63	22	106	475
1984	11	681	0	15	4	268	67	1492	244	46	126	541
1985	7	780	0	10	5	133	44	1040	161	128	141	465
1986	4	393	0	12	3	196	53	1184	273	102	185	455
1987	3	566	0	10	1	202	35	1265	348	163	174	416
1988	12	495	0	24	4	246	30	1041	801	242	337	488
1989	29	503	0	18	16	465	44	1385	926	203	291	444

fig = plt.figure(figsize=(16, 10))

color_list_reg_yr = ['palegreen', 'lime', 'green', 'Aqua', 'skyblue', 'darkred', 'darkgray', 'tan', 
                    'orangered', 'plum', 'salmon', 'mistyrose']
region_year.plot(figsize=(14, 10), fontsize=13, color=color_list_reg_yr)
#region_year.plot(figsize=(14, 10), fontsize=13)
plt.xlabel('iyear', fontsize=12)
plt.ylabel('Number of Attacks', fontsize=12)
plt.legend(fontsize=12)
plt.title('Number of Attacks per Region by Year', fontsize=12)
Text(0.5, 1.0, 'Number of Attacks per Region by Year')
<Figure size 1600x1000 with 0 Axes>

![image](https://github.com/RAMMOHANREDDY008/Exploratory-Data-Analysis-terrorism/assets/96512613/f7ad6e5a-cd2b-4e3a-9f0a-c95913f2de2a)
attacktype_year = pd.crosstab(terror_df.iyear, terror_df.attacktype1_txt)

attacktype_year.head(20)
attacktype1_txt	Armed Assault	Assassination	Bombing/Explosion	Facility/Infrastructure Attack	Hijacking	Hostage Taking (Barricade Incident)	Hostage Taking (Kidnapping)	Unarmed Assault	Unknown
iyear									
1970	61	22	333	174	11	3	38	3	6
1971	44	70	239	88	6	1	20	0	3
1972	63	265	188	19	12	4	16	0	1
1973	62	164	149	36	8	7	43	3	1
1974	46	158	285	42	3	5	37	4	1
1975	81	181	370	64	1	13	27	0	3
1976	124	204	419	113	4	6	45	3	5
1977	254	146	635	182	7	14	67	0	14
1978	241	263	644	181	0	43	97	5	52
1979	447	526	1058	197	9	76	146	7	196
1980	574	618	997	168	14	50	140	2	98
1981	697	405	1082	151	18	34	122	3	74
1982	665	360	1125	148	8	50	105	3	79
1983	852	360	1246	142	8	26	124	2	110
1984	823	443	1776	162	21	29	77	7	157
1985	659	311	1481	138	15	55	116	5	134
1986	592	371	1506	141	4	27	96	6	117
1987	798	495	1477	120	2	26	99	3	163
1988	922	821	1648	151	9	16	109	5	39
1989	1121	980	1797	240	13	18	130	8	17
fig = plt.figure(figsize=(16, 10))

color_list_reg_yr = ['palegreen', 'lime', 'green', 'Aqua', 'skyblue', 'darkred', 'darkgray', 'tan', 
                    'orangered', 'plum', 'salmon', 'mistyrose']
attacktype_year.plot(figsize=(14, 10), fontsize=13, color=color_list_reg_yr)
#region_year.plot(figsize=(14, 10), fontsize=13)
plt.xlabel('iyear', fontsize=12)
plt.ylabel('Number of Attacks', fontsize=12)
plt.legend(fontsize=12)
plt.title('Weapon trends by Year', fontsize=12)
Text(0.5, 1.0, 'Weapon trends by Year')
<Figure size 1600x1000 with 0 Axes>

![Uploading image.png…]()
organisation_year = pd.crosstab(terror_df.iyear, terror_df.gname)

organisation_year.head(20)
gname	1 May	14 K Triad	14th of December Command	15th of September Liberation Legion	16 January Organization for the Liberation of Tripoli	19th of July Christian Resistance Brigade	1st of May Group	2 April Group	20 December Movement (M-20)	22 May 1948	...	Zimbabwe African Nationalist Union (ZANU)	Zimbabwe African People's Union	Zimbabwe Guerrillas	Zimbabwe Patriotic Front	Zimbabwe People's Army (ZIPA)	Zionist Resistance Fighters	Zulu Militants	Zulu Miners	Zviadists	leftist guerrillas-Bolivarian militia
iyear																					
1970	0	0	0	0	0	0	3	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1971	0	0	0	0	0	0	1	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1972	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1973	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1974	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1975	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1976	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1977	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1978	0	0	0	0	0	0	0	0	0	0	...	1	5	4	1	1	0	0	0	0	0
1979	0	0	0	0	0	0	0	0	0	0	...	12	5	0	7	1	0	0	0	0	0
1980	0	0	0	1	0	0	0	0	0	0	...	2	0	0	1	0	0	0	0	0	0
1981	0	0	0	0	0	1	0	0	0	0	...	0	1	0	0	0	0	0	0	0	0
1982	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1983	0	0	0	0	0	0	0	6	0	0	...	0	2	0	0	0	0	0	0	0	0
1984	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1985	0	0	0	0	0	0	0	0	0	0	...	1	0	0	0	0	0	0	0	0	0
1986	0	0	0	0	0	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
1987	0	0	0	0	0	0	0	0	0	0	...	2	0	0	0	0	0	0	0	0	0
1988	0	0	0	0	0	0	0	0	0	1	...	0	0	0	0	0	0	0	0	0	0
1989	4	0	0	0	24	0	0	0	0	0	...	0	0	0	0	0	0	0	0	0	0
20 rows × 2450 columns
