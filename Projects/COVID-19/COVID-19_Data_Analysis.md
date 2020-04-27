# Coronavirus disease 2019 (COVID-19) Data Analysis using Python.

#### Import needed libraries
```python
import pandas as pd   
import chart_studio.plotly as py  
import plotly.graph_objects as go  
```
#### Data source  
```python
dfConfirmedWorldwide =  pd.read_csv(r'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv')  
dfDeathsWorldwide = pd.read_csv(r'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv')  
dfRecoveredWorldwide = pd.read_csv(r'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv')  
```

#### Drop columns  
```python
dfConfirmedWorldwide.drop(columns=['Lat','Long'], inplace = True)  
dfDeathsWorldwide.drop(columns=['Lat','Long'], inplace = True)  
dfRecoveredWorldwide.drop(columns=['Lat','Long'], inplace = True)  
```
#### Set the Dataframe index  
```python
dfConfirmedWorldwide = dfConfirmedWorldwide.set_index("Country/Region")  
dfDeathsWorldwide = dfDeathsWorldwide.set_index("Country/Region")  
dfRecoveredWorldwide = dfRecoveredWorldwide.set_index("Country/Region")  
```
#### Total Confirmed Cases Top 10 Countries  
```python
dfConfirmedTop10Countries= dfConfirmedWorldwide.nlargest(10,[dfConfirmedWorldwide.columns[-1]])  
dfConfirmedTop10Countries = dfConfirmedTop10Countries[dfConfirmedTop10Countries.columns[-30:]]  
```
#### Denmark's total confirmed cases and deaths
```python
dfConfirmedDenmark = dfConfirmedWorldwide.loc['Denmark']  
dfConfirmedDenmark = dfConfirmedDenmark.nlargest(1,[dfConfirmedDenmark.columns[-1]])  
```
```python
dfDeathsDenmark = dfDeathsWorldwide.loc['Denmark']  
dfDeathsDenmark = dfDeathsDenmark.nlargest(1,[dfDeathsDenmark.columns[-1]])  
```

#### Worldwide Total Confirmed Cases and Deaths
```python
fig1 = go.Indicator(
    mode = "number+delta",
    value = dfConfirmedWorldwide.sum()[-1],
    delta = {'position': "bottom", 'reference': dfConfirmedWorldwide.sum()[-2],
            'increasing':{'color':'red'},
            'decreasing':{'color':'green'}},
    title = {'text': "Total Confirmed Cases"},
    domain = {'x': [0, 0.5], 'y': [0.5,1]})
```

```python
fig2 = go.Indicator(
    mode = "number+delta",
    value = dfDeathsWorldwide.sum()[-1],
    delta = {'position': "bottom", 'reference': dfDeathsWorldwide.sum()[-2],
            'increasing':{'color':'red'},
            'decreasing':{'color':'green'}},
    title = {'text': "Total Deaths"},
    domain = {'x': [0.5, 1], 'y': [0.5,1]})
```

#### Denmark's Total Confirmed cases and Deaths
```python
fig3 = go.Indicator(
    mode = "number+delta",
    value = dfConfirmedDenmark.sum()[-1],
    delta = {'position': "bottom", 'reference': dfConfirmedDenmark.sum()[-2],
            'increasing':{'color':'red'},
            'decreasing':{'color':'green'}},
    title = {'text': "Denmark Confirmed Cases"},
    domain = {'x': [0, 0.5], 'y': [0,0.4]})
```

```python
fig4 = go.Indicator(
    mode = "number+delta",
    value = dfDeathsDenmark.sum()[-1],
    delta = {'position': "bottom", 'reference': dfDeathsDenmark.sum()[-2],
            'increasing':{'color':'red'},
            'decreasing':{'color':'green'}},
    title = {'text': "Denmark Deaths"},
    domain = {'x': [0.5, 1], 'y': [0,0.4]})

data = [fig1, fig2, fig3, fig4]
layout = dict(title='Worldwide and Denmark',paper_bgcolor='lightgray')
```

![Image](https://github.com/cloudstk/Data-Analysis/blob/master/Projects/COVID-19/media/Worldwide_Total_Confirmed_Cases.jpeg "icon") 

##### when working in a Jupyter Notebook to display the plot in the notebook.
```python
py.iplot(dict(data=data, layout=layout))
```
##### to return the unique url and optionally open the url.   
#py.plot(data, filename = 'Indicator', auto_open=True) 

##### Total Confirmed Cases Top 10 Countries 
```python
dfConfirmedTop10Countries 
```
![Image](https://github.com/cloudstk/Data-Analysis/blob/master/Projects/COVID-19/media/ConfirmedTop10countires.jpg "icon")   

##### Denmark's total confirmed cases
```python
dfDKconfirmed_deathsData= pd.concat([dfConfirmedDenmark, dfDeathsDenmark])
dfDKconfirmed_deathsData = dfDKconfirmed_deathsData[dfDKconfirmed_deathsData.columns[-30:]]
```
##### Denmark - Cumulative confirmed cases and deaths
```python
trace01 = go.Scatter(
    x = dfConfirmedDenmark.columns[-30:],
    y = dfConfirmedDenmark.iloc[0][-30:], name = 'Confirmed Cases')

trace02 = go.Scatter(
    x = dfDeathsDenmark.columns[-30:],
    y = dfDeathsDenmark.iloc[0][-30:], name = 'Deaths')

layout = dict(title='Denmark - Cumulative confirmed cases and deaths',paper_bgcolor='lightgray')
data = [trace01, trace02]

py.iplot(dict(data=data, layout=layout))
```
![Image](https://github.com/cloudstk/Data-Analysis/blob/master/Projects/COVID-19/media/DK_Cumulative_confirmed_cases_and_deaths.jpeg "icon")   
##### Denmark - Cumulative confirmed cases and deaths
```python
dfDKconfirmed_deathsData
```
![Image](https://github.com/cloudstk/Data-Analysis/blob/master/Projects/COVID-19/media/DKconfirmedanddeaths.jpg "icon") 
