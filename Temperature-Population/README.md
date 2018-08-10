

```python
import pandas as pd
tempData = pd.read_csv("./TempCountrywise.csv")
tempData.columns = ["indexbad", "avgTemp", "Country Name", "year"]
tempData = tempData[tempData.year>=1990]
tempData["year"] = tempData["year"].map(int)
tempData.head()
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>indexbad</th>
      <th>avgTemp</th>
      <th>Country Name</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>4324</td>
      <td>14.336636</td>
      <td>Afghanistan</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>41</th>
      <td>4325</td>
      <td>13.755273</td>
      <td>Afghanistan</td>
      <td>1991</td>
    </tr>
    <tr>
      <th>42</th>
      <td>4326</td>
      <td>13.536000</td>
      <td>Afghanistan</td>
      <td>1992</td>
    </tr>
    <tr>
      <th>43</th>
      <td>4327</td>
      <td>13.780818</td>
      <td>Afghanistan</td>
      <td>1993</td>
    </tr>
    <tr>
      <th>44</th>
      <td>4328</td>
      <td>14.431182</td>
      <td>Afghanistan</td>
      <td>1994</td>
    </tr>
  </tbody>
</table>
</div>




```python
popData = pd.read_csv("./PopulationDataCleaned.csv");
countries = list(set(popData["Country Name"]))
years = [year for year in range(1990, 2013)]

popTrans = pd.DataFrame()
for country in countries:
    for year in years:
        val = popData.loc[(popData["Country Name"]==country)][str(year)].values[0]
        popTrans = popTrans.append({'year': year, 'Country Name': country, 'Population': val}, ignore_index=True)
        
popTrans["year"] = popTrans["year"].map(int)
popTrans.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Name</th>
      <th>Population</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Belize</td>
      <td>187552.0</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Belize</td>
      <td>191126.0</td>
      <td>1991</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Belize</td>
      <td>194317.0</td>
      <td>1992</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Belize</td>
      <td>197616.0</td>
      <td>1993</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Belize</td>
      <td>201674.0</td>
      <td>1994</td>
    </tr>
  </tbody>
</table>
</div>




```python
mergedData = tempData.merge(popTrans, on=["Country Name", "year"], how='inner', sort=True)
mergedData.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>indexbad</th>
      <th>avgTemp</th>
      <th>Country Name</th>
      <th>year</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4324</td>
      <td>14.336636</td>
      <td>Afghanistan</td>
      <td>1990</td>
      <td>12249114.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4325</td>
      <td>13.755273</td>
      <td>Afghanistan</td>
      <td>1991</td>
      <td>12993657.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4326</td>
      <td>13.536000</td>
      <td>Afghanistan</td>
      <td>1992</td>
      <td>13981231.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4327</td>
      <td>13.780818</td>
      <td>Afghanistan</td>
      <td>1993</td>
      <td>15095099.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4328</td>
      <td>14.431182</td>
      <td>Afghanistan</td>
      <td>1994</td>
      <td>16172719.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
mergedData['avgTemp'].corr(mergedData['Population'])
```




    -0.06769782823563308


