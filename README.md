
# Heroes of Pymoli

The Heoroes of Pymoli is a free-to-play game that encourages players to purchase items to enhance their playing experience.  Using the data gathered from the software, we came up with the following analysis

+ There are about 573 registered players of which 81% are males.
+ Players between the ages of 20-24 purchase more of the optional items.
+ Item prices average about $2.83.
+ Lower priced items are more popular than higher priced items.
+ The game's audience tends to lean to teenagers (15-19) and young adults (20-24).
+ The most profitale items are not considered the most popular items.


```python
import pandas as pd
import numpy as np

# open json file
data_file = "Resources/purchase_data.json"
# data_file_pd = pd.read_json(data_file)
data_file_pd = pd.read_json(data_file)
data_file_pd.fillna(0, inplace=True)
```


```python
# read columns
data_file_pd.columns
```




    Index(['Age', 'Gender', 'Item ID', 'Item Name', 'Price', 'SN'], dtype='object')



## Player Count


```python
# total number of uniques players
totalplayers= data_file_pd['SN'].nunique()
playerCount = pd.DataFrame([totalplayers], columns=['Total Players'])
playerCount.style.format({"Total Players": "{:,}"})
```




<style  type="text/css" >
</style>  
<table id="T_a1120df4_8f71_11e7_b5f0_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Players</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a1120df4_8f71_11e7_b5f0_c821588b3f07" class="row_heading level0 row0" >0</th> 
        <td id="T_a1120df4_8f71_11e7_b5f0_c821588b3f07row0_col0" class="data row0 col0" >573</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis (Total)


```python
# Total number of unique items
totalItems = data_file_pd['Item ID'].nunique()

# Total number of Purchases
totalPurchase = data_file_pd['Price'].count()

# Average purchase price
averagePrice = data_file_pd['Price'].mean()

# Total Revenue
totalRevenue = round(data_file_pd['Price'].sum(), 2)




totalAnalysis_df = pd.DataFrame([[totalItems,averagePrice,totalPurchase,totalRevenue]], columns=['Number of Unique Items','Average Price','Number of Purchases','Total Revenue'])
totalAnalysis_df.style.format({"Number of Purchase": "{:,}","Number of Unique Items": "{:,}","Average Price": "${:,.2f}", "Total Revenue": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a1150e74_8f71_11e7_92b6_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a1150e74_8f71_11e7_92b6_c821588b3f07" class="row_heading level0 row0" >0</th> 
        <td id="T_a1150e74_8f71_11e7_92b6_c821588b3f07row0_col0" class="data row0 col0" >183</td> 
        <td id="T_a1150e74_8f71_11e7_92b6_c821588b3f07row0_col1" class="data row0 col1" >$2.93</td> 
        <td id="T_a1150e74_8f71_11e7_92b6_c821588b3f07row0_col2" class="data row0 col2" >780</td> 
        <td id="T_a1150e74_8f71_11e7_92b6_c821588b3f07row0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 



## Gender Demographics


```python
# pd.options.display.float_format = '{:,.2f}'.format
uniqueSN =pd.DataFrame (data_file_pd['SN'].unique(), columns=['SN'])
gender = data_file_pd.groupby(['Gender'])['SN'].nunique()
gender_df = pd.DataFrame(index=["Female", "Male", "Other / Non-Disclosed"])
gender
```




    Gender
    Female                   100
    Male                     465
    Other / Non-Disclosed      8
    Name: SN, dtype: int64




```python
percent = data_file_pd.groupby(['Gender'])['SN'].nunique()/totalplayers * 100
# percent
```


```python
gender_df["Percentage of Players"] = percent
# gender_df
```


```python
gender_df['Total Count'] = gender
gender_df.style.format({"Total Count": "{:,}","Percentage of Players": "{:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a128d350_8f71_11e7_b0e9_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a128d350_8f71_11e7_b0e9_c821588b3f07" class="row_heading level0 row0" >Female</th> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row0_col0" class="data row0 col0" >17.45</td> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row0_col1" class="data row0 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_a128d350_8f71_11e7_b0e9_c821588b3f07" class="row_heading level0 row1" >Male</th> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row1_col0" class="data row1 col0" >81.15</td> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row1_col1" class="data row1 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_a128d350_8f71_11e7_b0e9_c821588b3f07" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row2_col0" class="data row2 col0" >1.40</td> 
        <td id="T_a128d350_8f71_11e7_b0e9_c821588b3f07row2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis (Gender)


```python
# find the Price count, mean, sum, normalized totals by Gender
genderGroup_df = data_file_pd.groupby(['Gender'])

purchaseCount = genderGroup_df["Price"].count()
averagePrice = round(genderGroup_df["Price"].mean(), 2)
totalPurchase = genderGroup_df["Price"].sum()
normalizedTotals = round(totalPurchase/genderGroup_df["SN"].nunique(), 2)

genderPurchase_df = pd.DataFrame({
    'Purchase Count': purchaseCount,
    'Average Purchase Price': averagePrice,
    'Total Purchase Value': totalPurchase,
    'Normalized Totals': normalizedTotals
})

# rearrange the columns
genderPurchase_final = genderPurchase_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
genderPurchase_final.style.format({"Total Revenue": "{:,}","Number of Unique Items": "{:,}","Average Purchase": "{:,.2f}%", "Total Revenue": "${:,.2f}"})
genderPurchase_final.style.format({"Purchase Count": "{:,}","Normalized Totals": "${:,.2f}","Average Purchase Price": "${:,.2f}","Total Purchase Value": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07" class="row_heading level0 row0" >Female</th> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row0_col0" class="data row0 col0" >136</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row0_col1" class="data row0 col1" >$2.82</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row0_col2" class="data row0 col2" >$382.91</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07" class="row_heading level0 row1" >Male</th> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row1_col0" class="data row1 col0" >633</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row1_col1" class="data row1 col1" >$2.95</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row1_col2" class="data row1 col2" >$1,867.68</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row2_col0" class="data row2 col0" >11</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row2_col1" class="data row2 col1" >$3.25</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row2_col2" class="data row2 col2" >$35.74</td> 
        <td id="T_a12e2ed8_8f71_11e7_ba47_c821588b3f07row2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



## Age Demographics


```python
# setup bins and labels
agebins =[0,10,15,20,25,30,35,40,data_file_pd['Age'].max()+1]
binlabels = [" <10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

# create an age bin
purchase_data = data_file_pd.copy()
purchase_data['AgeBins'] =pd.cut(data_file_pd["Age"], bins=agebins,labels=binlabels,include_lowest=True, right=False)
```


```python
playerCount = purchase_data.groupby(['AgeBins'])['SN'].nunique()
percentPlayer = round((playerCount/totalplayers)*100, 2)

ageDemographics_final = pd.DataFrame({
    "Total Count": playerCount,
    "Percentage of Players": percentPlayer
})

ageDemographics_final.style.format({"Percentage of Players": "{:,.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_a137d258_8f71_11e7_9205_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >AgeBins</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row0" > <10</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row0_col0" class="data row0 col0" >3.32</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row1" >10-14</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row1_col0" class="data row1 col0" >4.01</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row2" >15-19</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row2_col0" class="data row2 col0" >17.45</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row3" >20-24</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row3_col0" class="data row3 col0" >45.20</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row4" >25-29</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row4_col0" class="data row4 col0" >15.18</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row5" >30-34</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row5_col0" class="data row5 col0" >8.20</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row6" >35-39</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row6_col0" class="data row6 col0" >4.71</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_a137d258_8f71_11e7_9205_c821588b3f07" class="row_heading level0 row7" >40+</th> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row7_col0" class="data row7 col0" >1.92</td> 
        <td id="T_a137d258_8f71_11e7_9205_c821588b3f07row7_col1" class="data row7 col1" >11</td> 
    </tr></tbody> 
</table> 



## Purchase Analysis (Age)


```python
priceCount = purchase_data.groupby(['AgeBins'])['Price'].count()
priceMean = round(purchase_data.groupby(['AgeBins'])['Price'].mean(), 2)
priceSum = round(purchase_data.groupby(['AgeBins'])['Price'].sum(), 2)
normalizedTotals = round((priceSum / purchase_data.groupby(['AgeBins'])['SN'].nunique()), 2)

Purchase_Price_df = pd.DataFrame({
    "Purchase Count": priceCount,
    "Average Purchase Price" : priceMean,
    "Total Purchase Value" : priceSum,
    "Normalized Totals" : normalizedTotals
})

# Final report
Purchase_By__Age_final = (Purchase_Price_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value","Normalized Totals"]])
Purchase_By__Age_final.style.format({"Normalized Totals": "${:,.2f}","Average Purchase Price": "${:,.2f}","Total Purchase Value": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >AgeBins</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row0" > <10</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row0_col0" class="data row0 col0" >28</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row0_col1" class="data row0 col1" >$2.98</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row0_col2" class="data row0 col2" >$83.46</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row0_col3" class="data row0 col3" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row1" >10-14</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row1_col0" class="data row1 col0" >35</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row1_col1" class="data row1 col1" >$2.77</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row1_col2" class="data row1 col2" >$96.95</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row1_col3" class="data row1 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row2" >15-19</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row2_col0" class="data row2 col0" >133</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row2_col2" class="data row2 col2" >$386.42</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row2_col3" class="data row2 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row3" >20-24</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row3_col0" class="data row3 col0" >336</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row3_col1" class="data row3 col1" >$2.91</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row3_col2" class="data row3 col2" >$978.77</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row3_col3" class="data row3 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row4" >25-29</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row4_col0" class="data row4 col0" >125</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row4_col1" class="data row4 col1" >$2.96</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row4_col2" class="data row4 col2" >$370.33</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row4_col3" class="data row4 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row5" >30-34</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row5_col0" class="data row5 col0" >64</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row5_col1" class="data row5 col1" >$3.08</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row5_col2" class="data row5 col2" >$197.25</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row5_col3" class="data row5 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row6" >35-39</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row6_col0" class="data row6 col0" >42</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row6_col1" class="data row6 col1" >$2.84</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row6_col2" class="data row6 col2" >$119.40</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row6_col3" class="data row6 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_a13edd5c_8f71_11e7_8906_c821588b3f07" class="row_heading level0 row7" >40+</th> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row7_col0" class="data row7 col0" >17</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row7_col1" class="data row7 col1" >$3.16</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row7_col2" class="data row7 col2" >$53.75</td> 
        <td id="T_a13edd5c_8f71_11e7_8906_c821588b3f07row7_col3" class="data row7 col3" >$4.89</td> 
    </tr></tbody> 
</table> 



## Top Spenders


```python
# find the count, average, and total by player
priceCount = data_file_pd.groupby(['SN'])['Price'].count()
priceAverage = round(data_file_pd.groupby(['SN'])['Price'].mean(), 2)
priceTotal = round(data_file_pd.groupby(['SN'])['Price'].sum(), 2)
# priceTotal

topSpender_df = pd.DataFrame({
    "Purchase Count" : priceCount,
    "Average Purchase Price" : priceAverage,
    "Total Purchase Value" : priceTotal
})

# List top 5 spenders in game
topSpender_final = (topSpender_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]).sort_values("Total Purchase Value", ascending=False).head(5)
topSpender_final.style.format({"Average Purchase Price": "${:,.2f}","Total Purchase Value": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a1459838_8f71_11e7_af86_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a1459838_8f71_11e7_af86_c821588b3f07" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row0_col0" class="data row0 col0" >5</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row0_col1" class="data row0 col1" >$3.41</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_a1459838_8f71_11e7_af86_c821588b3f07" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row1_col0" class="data row1 col0" >4</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row1_col1" class="data row1 col1" >$3.39</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row1_col2" class="data row1 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_a1459838_8f71_11e7_af86_c821588b3f07" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row2_col0" class="data row2 col0" >4</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row2_col1" class="data row2 col1" >$3.18</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row2_col2" class="data row2 col2" >$12.74</td> 
    </tr>    <tr> 
        <th id="T_a1459838_8f71_11e7_af86_c821588b3f07" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row3_col0" class="data row3 col0" >3</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row3_col1" class="data row3 col1" >$4.24</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row3_col2" class="data row3 col2" >$12.73</td> 
    </tr>    <tr> 
        <th id="T_a1459838_8f71_11e7_af86_c821588b3f07" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row4_col0" class="data row4 col0" >3</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row4_col1" class="data row4 col1" >$3.86</td> 
        <td id="T_a1459838_8f71_11e7_af86_c821588b3f07row4_col2" class="data row4 col2" >$11.58</td> 
    </tr></tbody> 
</table> 



## Most Popular Items


```python
# Find count, average, and total by Item
itemCount = data_file_pd.groupby(['Item ID','Item Name'])['Price'].count()
itemPrice = round(data_file_pd.groupby(['Item ID','Item Name'])['Price'].max(), 2)
itemTotal = round(data_file_pd.groupby(['Item ID','Item Name'])['Price'].sum(), 2)

popularItems_df = pd.DataFrame({
    "Purchase Count" : itemCount,
    "Item Price" : itemPrice,
    "Total Purchase Value" : itemTotal
})

# 5 most popular item
popularItems_final = (popularItems_df[["Purchase Count", "Item Price", "Total Purchase Value"]]).sort_values("Purchase Count", ascending=False).head(5)
popularItems_final.style.format({"Item Price": "${:,.2f}","Total Purchase Value": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level0 row0" >39</th> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level1 row0" >Betrayal, Whisper of Grieving Widows</th> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row0_col0" class="data row0 col0" >11</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row0_col1" class="data row0 col1" >$2.35</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row0_col2" class="data row0 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level0 row1" >84</th> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level1 row1" >Arcane Gem</th> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row1_col0" class="data row1 col0" >11</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row1_col1" class="data row1 col1" >$2.23</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row1_col2" class="data row1 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level0 row2" >31</th> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level1 row2" >Trickster</th> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row2_col0" class="data row2 col0" >9</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row2_col1" class="data row2 col1" >$2.07</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row2_col2" class="data row2 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level0 row3" >175</th> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level1 row3" >Woeful Adamantite Claymore</th> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row3_col0" class="data row3 col0" >9</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row3_col1" class="data row3 col1" >$1.24</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row3_col2" class="data row3 col2" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level0 row4" >13</th> 
        <th id="T_a14b6926_8f71_11e7_9e16_c821588b3f07" class="row_heading level1 row4" >Serenity</th> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row4_col0" class="data row4 col0" >9</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row4_col1" class="data row4 col1" >$1.49</td> 
        <td id="T_a14b6926_8f71_11e7_9e16_c821588b3f07row4_col2" class="data row4 col2" >$13.41</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items


```python
# 5 most profitable item
popularItems_final = (popularItems_df[["Purchase Count", "Item Price", "Total Purchase Value"]]).sort_values("Total Purchase Value", ascending=False).head(5)
popularItems_final.style.format({"Item Price": "${:,.2f}","Total Purchase Value": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a150eb14_8f71_11e7_b881_c821588b3f07" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level0 row0" >34</th> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level1 row0" >Retribution Axe</th> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row0_col0" class="data row0 col0" >9</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level0 row1" >115</th> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level1 row1" >Spectral Diamond Doomblade</th> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row1_col0" class="data row1 col0" >7</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row1_col1" class="data row1 col1" >$4.25</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level0 row2" >32</th> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level1 row2" >Orenmir</th> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row2_col0" class="data row2 col0" >6</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row2_col1" class="data row2 col1" >$4.95</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level0 row3" >103</th> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level1 row3" >Singed Scalpel</th> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row3_col0" class="data row3 col0" >6</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row3_col1" class="data row3 col1" >$4.87</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level0 row4" >107</th> 
        <th id="T_a150eb14_8f71_11e7_b881_c821588b3f07" class="row_heading level1 row4" >Splitter, Foe Of Subtlety</th> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row4_col0" class="data row4 col0" >8</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row4_col1" class="data row4 col1" >$3.61</td> 
        <td id="T_a150eb14_8f71_11e7_b881_c821588b3f07row4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


