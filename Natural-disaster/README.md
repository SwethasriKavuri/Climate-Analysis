
1955 - 2015

All natural disasters and temperature increase - 0.820172793188
LinregressResult(slope=0.001392623009895261, intercept=-0.071620178403980245, rvalue=0.82017279318789815, pvalue=3.3056934472371589e-179, stderr=3.595429573972614e-05)

Wildfire and increase in temperature - 0.82045376391
LinregressResult(slope=0.0034228591427014599, intercept=-0.022837428477786664, rvalue=0.82045376391029856, pvalue=1.9756635684053791e-179, stderr=8.8277792566513608e-05)


Flood and increase in temperature - 0.5307043928968993
LinregressResult(slope=0.022083545783387538, intercept=0.094995412827989234, rvalue=0.5307043928968993, pvalue=9.2360714740984266e-46, stderr=0.0014279733905668941)


```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import numpy as np
import seaborn as sns
import datetime
import math
import scipy
```


```python
temp_df = pd.read_csv("cleanedGlobalTemp.csv")
disaster_df = pd.read_csv("number-of-natural-disaster-events.csv")
temp_df["dt"] = pd.to_datetime(temp_df["dt"])
temp_df["Year"] = temp_df.dt.dt.year
temp_df.drop(["dt"], axis=1)
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LandAverageTemperature</th>
      <th>LandAverageTemperatureUncertainty</th>
      <th>LandMaxTemperature</th>
      <th>LandMaxTemperatureUncertainty</th>
      <th>LandMinTemperature</th>
      <th>LandMinTemperatureUncertainty</th>
      <th>LandAndOceanAverageTemperature</th>
      <th>LandAndOceanAverageTemperatureUncertainty</th>
      <th>TemperatureChange</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.010</td>
      <td>0.082</td>
      <td>8.385</td>
      <td>0.122</td>
      <td>-2.220</td>
      <td>0.063</td>
      <td>13.678</td>
      <td>0.052</td>
      <td>0.111581</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.704</td>
      <td>0.085</td>
      <td>8.322</td>
      <td>0.117</td>
      <td>-2.853</td>
      <td>0.080</td>
      <td>13.731</td>
      <td>0.051</td>
      <td>-0.093452</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.451</td>
      <td>0.060</td>
      <td>11.194</td>
      <td>0.087</td>
      <td>-0.133</td>
      <td>0.086</td>
      <td>14.526</td>
      <td>0.050</td>
      <td>0.097484</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.633</td>
      <td>0.085</td>
      <td>14.421</td>
      <td>0.085</td>
      <td>2.905</td>
      <td>0.113</td>
      <td>15.356</td>
      <td>0.054</td>
      <td>0.056903</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11.426</td>
      <td>0.066</td>
      <td>17.203</td>
      <td>0.117</td>
      <td>5.756</td>
      <td>0.119</td>
      <td>16.136</td>
      <td>0.050</td>
      <td>0.051677</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>5</th>
      <td>13.569</td>
      <td>0.187</td>
      <td>19.396</td>
      <td>0.240</td>
      <td>7.908</td>
      <td>0.146</td>
      <td>16.713</td>
      <td>0.070</td>
      <td>0.025097</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>6</th>
      <td>13.973</td>
      <td>0.087</td>
      <td>19.711</td>
      <td>0.113</td>
      <td>8.362</td>
      <td>0.141</td>
      <td>16.906</td>
      <td>0.051</td>
      <td>-0.047839</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14.059</td>
      <td>0.087</td>
      <td>19.757</td>
      <td>0.113</td>
      <td>8.515</td>
      <td>0.092</td>
      <td>17.013</td>
      <td>0.050</td>
      <td>0.151032</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>8</th>
      <td>11.980</td>
      <td>0.080</td>
      <td>17.634</td>
      <td>0.138</td>
      <td>6.482</td>
      <td>0.135</td>
      <td>16.357</td>
      <td>0.054</td>
      <td>0.082484</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9.338</td>
      <td>0.089</td>
      <td>14.808</td>
      <td>0.048</td>
      <td>4.051</td>
      <td>0.099</td>
      <td>15.476</td>
      <td>0.054</td>
      <td>0.085742</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5.968</td>
      <td>0.104</td>
      <td>11.331</td>
      <td>0.102</td>
      <td>0.703</td>
      <td>0.107</td>
      <td>14.388</td>
      <td>0.055</td>
      <td>0.001742</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>11</th>
      <td>3.785</td>
      <td>0.107</td>
      <td>9.044</td>
      <td>0.154</td>
      <td>-1.368</td>
      <td>0.084</td>
      <td>13.808</td>
      <td>0.056</td>
      <td>0.032935</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>12</th>
      <td>3.315</td>
      <td>0.117</td>
      <td>8.818</td>
      <td>0.071</td>
      <td>-2.011</td>
      <td>0.076</td>
      <td>13.724</td>
      <td>0.057</td>
      <td>0.157581</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3.991</td>
      <td>0.113</td>
      <td>9.699</td>
      <td>0.202</td>
      <td>-1.637</td>
      <td>0.092</td>
      <td>14.084</td>
      <td>0.056</td>
      <td>0.259548</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>14</th>
      <td>5.755</td>
      <td>0.048</td>
      <td>11.629</td>
      <td>0.077</td>
      <td>0.015</td>
      <td>0.055</td>
      <td>14.608</td>
      <td>0.049</td>
      <td>0.179484</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>15</th>
      <td>8.871</td>
      <td>0.086</td>
      <td>14.772</td>
      <td>0.206</td>
      <td>3.105</td>
      <td>0.062</td>
      <td>15.452</td>
      <td>0.054</td>
      <td>0.152903</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11.581</td>
      <td>0.066</td>
      <td>17.393</td>
      <td>0.108</td>
      <td>5.889</td>
      <td>0.178</td>
      <td>16.163</td>
      <td>0.049</td>
      <td>0.078677</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>17</th>
      <td>13.439</td>
      <td>0.060</td>
      <td>19.211</td>
      <td>0.191</td>
      <td>7.720</td>
      <td>0.192</td>
      <td>16.696</td>
      <td>0.050</td>
      <td>0.008097</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>18</th>
      <td>14.293</td>
      <td>0.060</td>
      <td>19.987</td>
      <td>0.100</td>
      <td>8.739</td>
      <td>0.115</td>
      <td>16.991</td>
      <td>0.050</td>
      <td>0.037161</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>19</th>
      <td>13.972</td>
      <td>0.059</td>
      <td>19.874</td>
      <td>0.099</td>
      <td>8.433</td>
      <td>0.141</td>
      <td>16.938</td>
      <td>0.050</td>
      <td>0.076032</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>20</th>
      <td>11.817</td>
      <td>0.143</td>
      <td>17.713</td>
      <td>0.106</td>
      <td>6.395</td>
      <td>0.274</td>
      <td>16.285</td>
      <td>0.063</td>
      <td>0.010484</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>21</th>
      <td>9.299</td>
      <td>0.096</td>
      <td>14.803</td>
      <td>0.148</td>
      <td>3.881</td>
      <td>0.157</td>
      <td>15.455</td>
      <td>0.054</td>
      <td>0.064742</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>22</th>
      <td>5.919</td>
      <td>0.100</td>
      <td>11.232</td>
      <td>0.063</td>
      <td>0.702</td>
      <td>0.092</td>
      <td>14.407</td>
      <td>0.055</td>
      <td>0.020742</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>23</th>
      <td>3.751</td>
      <td>0.072</td>
      <td>9.071</td>
      <td>0.075</td>
      <td>-1.399</td>
      <td>0.105</td>
      <td>13.805</td>
      <td>0.052</td>
      <td>0.029935</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>24</th>
      <td>3.139</td>
      <td>0.115</td>
      <td>8.701</td>
      <td>0.082</td>
      <td>-2.344</td>
      <td>0.109</td>
      <td>13.758</td>
      <td>0.058</td>
      <td>0.191581</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>25</th>
      <td>4.134</td>
      <td>0.091</td>
      <td>9.792</td>
      <td>0.105</td>
      <td>-1.366</td>
      <td>0.092</td>
      <td>14.161</td>
      <td>0.053</td>
      <td>0.336548</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>26</th>
      <td>5.245</td>
      <td>0.076</td>
      <td>11.203</td>
      <td>0.074</td>
      <td>-0.467</td>
      <td>0.084</td>
      <td>14.538</td>
      <td>0.051</td>
      <td>0.109484</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>27</th>
      <td>8.595</td>
      <td>0.078</td>
      <td>14.690</td>
      <td>0.083</td>
      <td>2.835</td>
      <td>0.220</td>
      <td>15.447</td>
      <td>0.052</td>
      <td>0.147903</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>28</th>
      <td>11.525</td>
      <td>0.065</td>
      <td>17.315</td>
      <td>0.130</td>
      <td>5.798</td>
      <td>0.136</td>
      <td>16.252</td>
      <td>0.051</td>
      <td>0.167677</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>29</th>
      <td>13.927</td>
      <td>0.068</td>
      <td>19.661</td>
      <td>0.146</td>
      <td>8.105</td>
      <td>0.126</td>
      <td>16.900</td>
      <td>0.051</td>
      <td>0.212097</td>
      <td>1987</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>342</th>
      <td>15.003</td>
      <td>0.126</td>
      <td>20.737</td>
      <td>0.100</td>
      <td>9.330</td>
      <td>0.153</td>
      <td>17.503</td>
      <td>0.068</td>
      <td>0.549161</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>343</th>
      <td>14.742</td>
      <td>0.129</td>
      <td>20.596</td>
      <td>0.120</td>
      <td>9.014</td>
      <td>0.179</td>
      <td>17.462</td>
      <td>0.068</td>
      <td>0.600032</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>344</th>
      <td>13.154</td>
      <td>0.104</td>
      <td>18.947</td>
      <td>0.077</td>
      <td>7.482</td>
      <td>0.194</td>
      <td>16.894</td>
      <td>0.064</td>
      <td>0.619484</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>345</th>
      <td>10.256</td>
      <td>0.077</td>
      <td>15.994</td>
      <td>0.100</td>
      <td>4.663</td>
      <td>0.135</td>
      <td>15.905</td>
      <td>0.062</td>
      <td>0.514742</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>346</th>
      <td>7.424</td>
      <td>0.079</td>
      <td>12.965</td>
      <td>0.113</td>
      <td>2.016</td>
      <td>0.116</td>
      <td>15.107</td>
      <td>0.062</td>
      <td>0.720742</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>347</th>
      <td>4.724</td>
      <td>0.085</td>
      <td>10.083</td>
      <td>0.129</td>
      <td>-0.548</td>
      <td>0.105</td>
      <td>14.339</td>
      <td>0.065</td>
      <td>0.563935</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>348</th>
      <td>3.732</td>
      <td>0.075</td>
      <td>9.327</td>
      <td>0.138</td>
      <td>-1.755</td>
      <td>0.091</td>
      <td>14.136</td>
      <td>0.064</td>
      <td>0.569581</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>349</th>
      <td>3.500</td>
      <td>0.068</td>
      <td>9.146</td>
      <td>0.073</td>
      <td>-2.080</td>
      <td>0.095</td>
      <td>14.157</td>
      <td>0.065</td>
      <td>0.332548</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>350</th>
      <td>6.378</td>
      <td>0.098</td>
      <td>12.367</td>
      <td>0.108</td>
      <td>0.436</td>
      <td>0.139</td>
      <td>15.090</td>
      <td>0.068</td>
      <td>0.661484</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>351</th>
      <td>9.589</td>
      <td>0.111</td>
      <td>15.529</td>
      <td>0.090</td>
      <td>3.617</td>
      <td>0.193</td>
      <td>16.038</td>
      <td>0.069</td>
      <td>0.738903</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>352</th>
      <td>12.582</td>
      <td>0.068</td>
      <td>18.362</td>
      <td>0.158</td>
      <td>6.613</td>
      <td>0.166</td>
      <td>16.804</td>
      <td>0.059</td>
      <td>0.719677</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>353</th>
      <td>14.335</td>
      <td>0.096</td>
      <td>20.126</td>
      <td>0.095</td>
      <td>8.473</td>
      <td>0.157</td>
      <td>17.303</td>
      <td>0.064</td>
      <td>0.615097</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>354</th>
      <td>14.873</td>
      <td>0.078</td>
      <td>20.711</td>
      <td>0.091</td>
      <td>9.112</td>
      <td>0.135</td>
      <td>17.508</td>
      <td>0.061</td>
      <td>0.554161</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>355</th>
      <td>14.875</td>
      <td>0.115</td>
      <td>20.790</td>
      <td>0.098</td>
      <td>9.047</td>
      <td>0.173</td>
      <td>17.607</td>
      <td>0.064</td>
      <td>0.745032</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>356</th>
      <td>13.091</td>
      <td>0.086</td>
      <td>18.865</td>
      <td>0.106</td>
      <td>7.399</td>
      <td>0.155</td>
      <td>16.975</td>
      <td>0.059</td>
      <td>0.700484</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>357</th>
      <td>10.330</td>
      <td>0.076</td>
      <td>16.066</td>
      <td>0.111</td>
      <td>4.689</td>
      <td>0.126</td>
      <td>16.029</td>
      <td>0.059</td>
      <td>0.638742</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>358</th>
      <td>6.713</td>
      <td>0.121</td>
      <td>12.284</td>
      <td>0.124</td>
      <td>1.313</td>
      <td>0.115</td>
      <td>14.899</td>
      <td>0.064</td>
      <td>0.512742</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>359</th>
      <td>4.850</td>
      <td>0.090</td>
      <td>10.190</td>
      <td>0.148</td>
      <td>-0.331</td>
      <td>0.123</td>
      <td>14.410</td>
      <td>0.062</td>
      <td>0.634935</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>360</th>
      <td>3.881</td>
      <td>0.130</td>
      <td>9.432</td>
      <td>0.090</td>
      <td>-1.518</td>
      <td>0.097</td>
      <td>14.255</td>
      <td>0.066</td>
      <td>0.688581</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>361</th>
      <td>4.664</td>
      <td>0.121</td>
      <td>10.497</td>
      <td>0.092</td>
      <td>-1.138</td>
      <td>0.113</td>
      <td>14.564</td>
      <td>0.067</td>
      <td>0.739548</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>362</th>
      <td>6.740</td>
      <td>0.060</td>
      <td>12.659</td>
      <td>0.096</td>
      <td>0.894</td>
      <td>0.079</td>
      <td>15.193</td>
      <td>0.061</td>
      <td>0.764484</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>363</th>
      <td>9.313</td>
      <td>0.088</td>
      <td>15.224</td>
      <td>0.137</td>
      <td>3.402</td>
      <td>0.147</td>
      <td>15.962</td>
      <td>0.061</td>
      <td>0.662903</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>364</th>
      <td>12.312</td>
      <td>0.081</td>
      <td>18.181</td>
      <td>0.117</td>
      <td>6.313</td>
      <td>0.153</td>
      <td>16.774</td>
      <td>0.058</td>
      <td>0.689677</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>365</th>
      <td>14.505</td>
      <td>0.068</td>
      <td>20.364</td>
      <td>0.133</td>
      <td>8.627</td>
      <td>0.168</td>
      <td>17.390</td>
      <td>0.057</td>
      <td>0.702097</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>366</th>
      <td>15.051</td>
      <td>0.086</td>
      <td>20.904</td>
      <td>0.109</td>
      <td>9.326</td>
      <td>0.225</td>
      <td>17.611</td>
      <td>0.058</td>
      <td>0.657161</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>367</th>
      <td>14.755</td>
      <td>0.072</td>
      <td>20.699</td>
      <td>0.110</td>
      <td>9.005</td>
      <td>0.170</td>
      <td>17.589</td>
      <td>0.057</td>
      <td>0.727032</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>368</th>
      <td>12.999</td>
      <td>0.079</td>
      <td>18.845</td>
      <td>0.088</td>
      <td>7.199</td>
      <td>0.229</td>
      <td>17.049</td>
      <td>0.058</td>
      <td>0.774484</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>369</th>
      <td>10.801</td>
      <td>0.102</td>
      <td>16.450</td>
      <td>0.059</td>
      <td>5.232</td>
      <td>0.115</td>
      <td>16.290</td>
      <td>0.062</td>
      <td>0.899742</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>370</th>
      <td>7.433</td>
      <td>0.119</td>
      <td>12.892</td>
      <td>0.093</td>
      <td>2.157</td>
      <td>0.106</td>
      <td>15.252</td>
      <td>0.063</td>
      <td>0.865742</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>371</th>
      <td>5.518</td>
      <td>0.100</td>
      <td>10.725</td>
      <td>0.154</td>
      <td>0.287</td>
      <td>0.099</td>
      <td>14.774</td>
      <td>0.062</td>
      <td>0.998935</td>
      <td>2015</td>
    </tr>
  </tbody>
</table>
<p>372 rows × 10 columns</p>
</div>




```python
disaster_df = disaster_df[disaster_df.Year <=2015]
disaster_all_df = disaster_df[disaster_df['Entity']=="All natural disasters"]
disaster_flood_df = disaster_df[disaster_df['Entity']=="Flood"]
disaster_wildfire_df = disaster_df[disaster_df['Entity']=="Wildfire"]
print(disaster_df[disaster_df['Entity']=="Flood"])
```

        Entity  Code  Year  \
    549  Flood   NaN  1900   
    550  Flood   NaN  1903   
    551  Flood   NaN  1906   
    552  Flood   NaN  1909   
    553  Flood   NaN  1910   
    554  Flood   NaN  1911   
    555  Flood   NaN  1915   
    556  Flood   NaN  1917   
    557  Flood   NaN  1920   
    558  Flood   NaN  1925   
    559  Flood   NaN  1926   
    560  Flood   NaN  1927   
    561  Flood   NaN  1928   
    562  Flood   NaN  1930   
    563  Flood   NaN  1931   
    564  Flood   NaN  1933   
    565  Flood   NaN  1935   
    566  Flood   NaN  1936   
    567  Flood   NaN  1937   
    568  Flood   NaN  1938   
    569  Flood   NaN  1939   
    570  Flood   NaN  1940   
    571  Flood   NaN  1943   
    572  Flood   NaN  1947   
    573  Flood   NaN  1948   
    574  Flood   NaN  1949   
    575  Flood   NaN  1950   
    576  Flood   NaN  1951   
    577  Flood   NaN  1952   
    578  Flood   NaN  1953   
    ..     ...   ...   ...   
    611  Flood   NaN  1986   
    612  Flood   NaN  1987   
    613  Flood   NaN  1988   
    614  Flood   NaN  1989   
    615  Flood   NaN  1990   
    616  Flood   NaN  1991   
    617  Flood   NaN  1992   
    618  Flood   NaN  1993   
    619  Flood   NaN  1994   
    620  Flood   NaN  1995   
    621  Flood   NaN  1996   
    622  Flood   NaN  1997   
    623  Flood   NaN  1998   
    624  Flood   NaN  1999   
    625  Flood   NaN  2000   
    626  Flood   NaN  2001   
    627  Flood   NaN  2002   
    628  Flood   NaN  2003   
    629  Flood   NaN  2004   
    630  Flood   NaN  2005   
    631  Flood   NaN  2006   
    632  Flood   NaN  2007   
    633  Flood   NaN  2008   
    634  Flood   NaN  2009   
    635  Flood   NaN  2010   
    636  Flood   NaN  2011   
    637  Flood   NaN  2012   
    638  Flood   NaN  2013   
    639  Flood   NaN  2014   
    640  Flood   NaN  2015   
    
         Number of disasters (EMDAT (2017)) (reported disasters)  
    549                                                  1        
    550                                                  2        
    551                                                  2        
    552                                                  1        
    553                                                  1        
    554                                                  1        
    555                                                  1        
    556                                                  1        
    557                                                  1        
    558                                                  1        
    559                                                  4        
    560                                                  2        
    561                                                  2        
    562                                                  1        
    563                                                  1        
    564                                                  2        
    565                                                  1        
    566                                                  1        
    567                                                  2        
    568                                                  2        
    569                                                  2        
    570                                                  1        
    571                                                  2        
    572                                                  1        
    573                                                  5        
    574                                                  2        
    575                                                 10        
    576                                                  7        
    577                                                  4        
    578                                                 12        
    ..                                                 ...        
    611                                                 50        
    612                                                 68        
    613                                                 76        
    614                                                 46        
    615                                                 60        
    616                                                 77        
    617                                                 59        
    618                                                 84        
    619                                                 88        
    620                                                 94        
    621                                                 92        
    622                                                 95        
    623                                                 94        
    624                                                122        
    625                                                157        
    626                                                157        
    627                                                172        
    628                                                158        
    629                                                128        
    630                                                193        
    631                                                226        
    632                                                218        
    633                                                165        
    634                                                151        
    635                                                184        
    636                                                156        
    637                                                136        
    638                                                149        
    639                                                135        
    640                                                160        
    
    [92 rows x 4 columns]



```python
temp_df = temp_df[temp_df.dt.dt.year>=1950]
temp_new = pd.DataFrame()
for year in range(1950,2016):
    tmp = temp_df[temp_df.Year==year].TemperatureChange.mean()
    temp_new = temp_new.append({'year':year,'TemperatureChange':tmp},ignore_index=True)
    
```


```python
temp = disaster_all_df.merge(temp_df,on = ["Year"], how='inner', sort=True)
print(temp["Number of disasters (EMDAT (2017)) (reported disasters)"].corr(temp["TemperatureChange"]))
scipy.stats.linregress(temp["Number of disasters (EMDAT (2017)) (reported disasters)"],temp["TemperatureChange"])
```

    0.6613048888017973





    LinregressResult(slope=0.0013500600131126814, intercept=-0.07157075872926699, rvalue=0.6613048888017972, pvalue=3.921342488303607e-48, stderr=7.961219800180803e-05)




```python
temp = disaster_flood_df.merge(temp_df,on = ["Year"], how='inner', sort=True)
print(temp["Number of disasters (EMDAT (2017)) (reported disasters)"].corr(temp["TemperatureChange"]))
scipy.stats.linregress(temp["Number of disasters (EMDAT (2017)) (reported disasters)"],temp["TemperatureChange"])
```

    0.690411811585937





    LinregressResult(slope=0.0027472157294053654, intercept=0.05795491943304426, rvalue=0.690411811585937, pvalue=5.634973063973637e-54, stderr=0.0001496484215713633)




```python
temp = disaster_wildfire_df.merge(temp_df,on = ["Year"], how='inner', sort=True)
temp["Number of disasters (EMDAT (2017)) (reported disasters)"].corr(temp["TemperatureChange"])
scipy.stats.linregress(temp["Number of disasters (EMDAT (2017)) (reported disasters)"],temp["TemperatureChange"])
```




    LinregressResult(slope=0.004863523698953266, intercept=0.3476859925846402, rvalue=0.15429140437704789, pvalue=0.003336699844144625, stderr=0.0016460229806104453)


